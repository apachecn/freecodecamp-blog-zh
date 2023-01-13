# 如何使用 Travis CI(几乎)零恐惧地部署您的 Cloud Foundry 应用程序

> 原文：<https://www.freecodecamp.org/news/how-to-deploy-your-cloud-foundry-app-with-almost-zero-fear-using-travis-ci-926697fff8f6/>

罗宾·博比特

# **如何使用 Travis CI(几乎)零恐惧地部署您的 Cloud Foundry 应用程序**

![1*PXXH-HGbEP2x3LWooWGLlA](img/025e8a97775159629e968064e459ded1.png)

Photo by William Canady from Pexels

将应用程序部署到云应该是轻松的。我们应该能够无所畏惧地不断部署新代码，只要我们愿意。[蓝绿色部署](https://docs.cloudfoundry.org/devguide/deploy-apps/blue-green.html)模式使我们能够做到这一点。

我最近加入了一个新团队，该团队使用 Travis 和 [Bluemix CloudFoundry 部署提供商](https://docs.travis-ci.com/user/deployment/bluemixcloudfoundry/)将 node.js Cloud Foundry 应用程序部署到 IBM Cloud。这对于只需几个参数就能快速轻松地设置您的部署非常有用。

不幸的是，每次部署都意味着应用程序的中断，因为现有版本停止了，而新版本启动了。此外，在旧代码被吹走之前，没有验证新代码是好的。

使用蓝绿色部署技术，您当前的应用程序(蓝色)继续运行并接受网络流量。而您的新应用程序代码(绿色)部署在临时路由上。可以在临时路线上验证新的绿色应用。如果有任何问题，部署就会停止。蓝色应用程序继续不间断地提供流量服务。一旦绿色应用程序通过审查，路由器就会更新为指向绿色应用程序。蓝色可以停止。

通过这种方式，总有一个版本的应用程序可用于接收流量。新代码的部署或运行时中的任何问题都不会影响应用程序的可用性。

我立即开始寻找蓝绿色部署我们的应用程序的方法。为了编写尽可能少的定制代码，我决定将 [cf-blue-green-deploy 插件](https://github.com/bluemixgaragelondon/cf-blue-green-deploy)用于 Cloud Foundry 命令行界面(CLI)。我将在这里分享我是如何做到这一点的。

我想，如果你来到这里，你可能已经过了与特拉维斯简单“开始”的阶段。我不会在这里讨论这些细节。

如果您没有兴趣跟随，只想直接进入主题，您可以从 [GitHub](https://github.com/robinbobbitt/blue-green-cf-travis) 克隆我的工作示例。

### **安装 CF CLI 和蓝绿插件**

由于我们没有使用 Cloud Foundry [部署提供商](https://github.com/travis-ci/dpl)，我们必须自己安装 Cloud Foundry CLI，以及蓝绿色部署插件。我们可以在 [Travis 构建生命周期](https://docs.travis-ci.com/user/customizing-the-build/)的`before_deploy`阶段这样做。

注意，`before_deploy`阶段在每个部署提供者之前运行。如果您在部署阶段做了额外的事情，您可能希望将这些步骤移到`after_success`阶段(在成功构建后只运行一次)以避免不必要的额外安装。您也可以将这些步骤移动到部署脚本本身中，这是我们接下来要编写的。

不管你把它放在哪里，这里是脚本:

```
- echo "Installing cf cli"- test x$TRAVIS_OS_NAME = "xlinux" && rel="linux64-binary" || rel="macosx64"; wget "https://cli.run.pivotal.io/stable?release=${rel}&source=github" -qO cf.tgz && tar -zxvf cf.tgz && rm cf.tgz- export PATH="$PATH:."- cf --version
```

```
- echo "Installing cf blue-green deploy plugin"- cf add-plugin-repo CF-Community https://plugins.cloudfoundry.org- cf install-plugin blue-green-deploy -r CF-Community -f
```

安装 CLI 的命令直接来自 CloudFoundry DPL [源](https://github.com/travis-ci/dpl/blob/master/lib/dpl/provider/cloud_foundry.rb)。安装蓝绿色部署插件的命令来自插件的[自述文件](https://github.com/bluemixgaragelondon/cf-blue-green-deploy)。

### **调用蓝绿部署**

为了调用蓝绿色部署，我们将使用[脚本部署提供者](https://github.com/travis-ci/dpl#script)，它执行提供的命令并检查零状态作为成功的指示。

```
deploy:# on update to master branch we deploy to Cloud Foundry- provider: script  skip_cleanup: true  script:  ./scripts/cf-blue-green-deploy.sh blue-green-cf-travis manifest.yml prod  on:    branch: master
```

注意`skip_cleanup`被设置为`true`。如果不这样做，您刚刚安装的 cf CLI 和蓝绿色部署插件将在部署运行之前被删除。

[cf-blue-green-deploy.sh 脚本](https://github.com/robinbobbitt/blue-green-cf-travis/blob/master/scripts/cf-blue-green-deploy.sh)登录 Cloud Foundry API 并调用 blue-green deploy 插件。除了指定应用程序名称和清单文件之外，您还可以将冒烟测试脚本传递给 blue-green deploy 插件。在新的应用程序代码已经被部署之后，但是在应用程序路由被切换到新的应用程序之前，插件将调用冒烟测试脚本。这允许您在任何实际流量访问新代码之前对其运行任意数量的测试。

冒烟测试脚本被传递了一个参数。参数是新部署的应用程序的临时 URL。如果冒烟测试脚本成功退出，蓝绿色部署将通过切换到新应用程序的路径来完成。如果冒烟测试脚本因失败而退出，流量将继续流向旧版本的应用程序。新版本仍可用于故障排除。

在我的示例项目中，smoke 测试脚本调用/version API，并验证它是否返回 200 状态代码。

在我们实际工作的项目中，我们针对新部署的应用程序运行了一个 Postman 集合。您希望您的冒烟测试套件足够大，使您对新代码充满信心，但又不要太大，以至于需要很长时间才能完成部署，或者古怪的测试会阻碍您成功完成部署。

在您的新代码上线后，您可以选择运行一套更全面的回归测试作为`after_deploy`步骤。

### **IBM 云中蓝绿部署的副作用**

如果您要部署到 IBM Cloud，需要注意这种方法的一些细微差别。因为每次蓝绿色部署时您都在创建一个新的 CF 应用程序实例，所以您的应用程序 guid 将会改变。如果使用可用性监视服务，当 guid 更改时，您配置的测试将会丢失。

要解决这个问题，建立一个永久的虚拟应用程序。在这个虚拟应用程序的配置中为蓝绿色部署的应用程序编写测试。在编写可用性监控测试时，您可以指定任何 URL。

类似地，如果您使用日志分析服务，您将会看到，当您单击应用程序仪表板的 Logs 选项卡上的“View in Kibana”链接时，将会启动对应用程序 guid 字符串的 Kibana 搜索。最近一次部署之前的任何应用程序日志都不会显示。要解决这个问题，您可以简单地根据应用程序名称而不是应用程序 guid 进行筛选。

另一个具有相同问题的服务是自动伸缩。每当一个新的应用程序作为蓝绿色部署的一部分出现时，它都需要重新配置其自动扩展策略。有一个可用的命令行界面，您大概可以用它来编写脚本，但是我还没有尝试过。

如果这些问题中的任何一个对您来说都不是新手，您总是可以选择编写一个定制的蓝绿色部署脚本，该脚本利用两个永久的 CF 应用程序，一个蓝色的，一个绿色的。这两个应用程序将轮流运行和空闲。例如，您可以用自动伸缩策略配置这两个应用程序。

当然，这种方法意味着您不能利用本文中描述的蓝绿色部署插件，并且您需要维护自己的自定义脚本。

### **结束**

在这篇文章中，我们研究了如何使用 Travis 和 cf blue-green deploy 插件实现低风险、零停机的部署。

在一个真实的项目中，我们将有更大的保证，因为我们将有一套单元测试，并且在部署有机会运行之前，那里的错误将使 Travis 构建失败。我们还可能将开发和暂存分支配置为部署到我们的 Cloud Foundry 组织中各自的空间，从而允许我们在将更改升级到生产之前，根据需要验证和稳定应用程序。

感谢阅读！这是我的第一个媒体故事，希望你觉得有用。