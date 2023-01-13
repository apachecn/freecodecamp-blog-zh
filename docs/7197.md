# 如何简化容器化应用的部署

> 原文：<https://www.freecodecamp.org/news/how-can-we-simplify-deployment-of-containarized-apps-a2effd01a662/>

奥马尔·利维·赫弗罗尼

# 如何简化容器化应用的部署

![ulCZyWum4B47VZ37bmrkdeMnsd0EQHuWbMzz](img/f9d5f66eaff58ebd01b3e207ba8fcca5.png)

Photo by [Ilze Lucero](https://unsplash.com/photos/l_VVd8nV3PE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/container?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当我开始学习 [Kubernetes](https://kubernetes.io/) 时，我问自己这个问题:我们如何简化容器化应用的部署？乍一看，部署容器化的应用程序似乎很简单。您所需要的只是一堆 YAML 文件，通过使用`[kubectl](https://kubernetes.io/docs/reference/generated/kubectl/kubectl/)`(Kubernetes 命令行实用程序)，您将在您的 Kubernetes 集群中启动并运行您的服务。

但是，尽管部署一个应用程序是一项简单的任务，但如何部署数百个应用程序呢？在 [Soluto](https://www.solutotlv.com) ，我们有超过 100 个活跃的微服务，而且这个数字还在继续增长。因此，当我们开始考虑将工作负载转移到我们的 Kubernetes 集群时，我们面临着一些挑战:

首先，Kubernetes 的部署实际上更复杂。您需要正确设置许多活动部件:pod 自动缩放器、pod 资源、入口等。这些部分需要一些关于 Kubernetes 如何工作的经验，如果设置不正确，可能会导致生产中的问题。理想情况下，我们会有一种方法来简化这一点，这样开发人员就可以专注于编写代码，减少对部署的担心。

其次，安全性也是一个挑战。生产中的所有服务都应该有某些东西，比如传输层安全性(TLS)。这些不一定是复杂的事情，但仍然需要处理。我们希望对它们进行预配置，以便在默认情况下保护任何新部署。

### 寻找解决方案

为了解决这些挑战并加快和简化采用过程，我们寻找一种方法来为 Kubernetes 创建一个模板。任何开发人员都可以使用，并且只需要几个参数(例如，服务的 Docker 映像)就可以在生产中启动和运行服务。

另一方面，我们需要小心不要隐藏太多——开发人员必须能够理解正在发生的事情，以便他们能够处理生产问题。我们必须找到合适的抽象层次，使其更容易部署到 Kubernetes，而不隐藏太多的细节。

考虑到这一点，我们开始寻找解决方案。在尝试了一些事情之后，我们找到了[掌舵人](https://helm.sh/)。Helm 是 Kubernetes 的包装经理。您可以使用它在您的集群上安装任何应用程序，Helm 将负责获取所有需要的配置文件并将其安装在您的集群上。Helm 还支持更新部署、回滚和许多其他很酷的功能。每个头盔包被称为一个“图表”，图表存储在一个存储库中。有了头盔，安装[蒙戈](https://github.com/kubernetes/charts/tree/master/stable/mongodb)就像`helm install stable/mongodb`一样简单。

听起来是个很好的解决方案！我们可以为每种类型的服务定义一个图表——就像为我们所有的 web APIs 定义一个图表，它将处理负载平衡器和 TLS 之类的事情——开发人员只需使用 Helm 的配置文件指定所需的参数。

![4P0D8IGfVHiObVNHgAdPTrrhcNFOZh2irhoj](img/787ca8505189a50d3abbe6bb18355c67.png)

Me and my teammate, when we found Helm

### 赫尔姆:让我们看看它是怎么做的

为了使用 Helm，我们首先需要[安装](https://docs.helm.sh/using_helm/#installing-helm)它。Helm 有两个组件:运行在您的计算机上的 Helm 客户端和运行在您的集群上的服务器端组件 Tiller。然后，我们需要使用这个简单的 Helm CLI 命令创建图表:`helm create web-api`

运行这个命令后，您会注意到创建了一个名为“web-api”的新文件夹。在这个文件夹中，您会发现所有熟悉的 Kubernetes 配置文件:部署、服务、入口等等。现在是时候定制一下了:我们可以添加一个[水平 pod 自动缩放器](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)，定义 pod 需要的默认资源，当然，默认情况下启用 TLS。

基于 [Go 模板机制](https://docs.helm.sh/chart_template_guide/)，一切都是高度可定制的。因此，我们添加的任何内容都可以被开发人员覆盖，以防默认配置不符合需要。

所以现在我们有了一张图表——但是我们如何消费它呢？图表必须存在于一个 [Helm 存储库](https://github.com/kubernetes/helm/blob/master/docs/chart_repository.md)中，这个存储库基本上是一个服务器，包含几个 Zip 存档(存储库中的所有图表)和一个供 CLI 使用的索引文件。你可以使用 Azure Blob 或 AWS S3 等任何存储服务手动设置回购，但最简单的选择是[海图博物馆](https://github.com/kubernetes-helm/chartmuseum)。

Chart museum 是一个带有 CRUD API 的 Helm 存储库，用于管理您的图表。它支持基本的认证，所以你可以限制谁可以推新的图表到你的头盔库。Helm 不提供任何“博物馆即服务”解决方案，因此您必须自行开发。但是它非常简单——简单地使用它的 docker 图像。

现在，我们可以为 web-api 图表构建 CI/CD 管道，以简化修改流程:

*   运行某种测试，以确保新版本没有被破坏。我将在下一段讨论如何实现。
*   使用 Helm CLI 打包新图表。
*   使用 Chart Museum 的 API 将新包推送到我们的 Chart Museum 实例。

### 测试它

现在我们的图表已经准备好供开发人员使用了！但是等等…我们怎么知道图表真的有效呢？我们如何确保它能继续工作？这就是为什么我们需要为我们的图表编写测试(就像我们为其他任何东西编写测试一样)。

![WGGYmgftYuRQOqzlognwNV8yF4IFx6etjAF2](img/c05250180e274e2e918aab2486252cd6.png)

我们主要想测试两件事。

1.  我们想要测试我们的模板——例如，如果一个入口应该存在一个 TLS 和特定的规则(由开发人员定义),我们应该测试生成的模板，并确保入口创建正确。
2.  我们想测试这些文件是否是有效的 Kubernetes 配置，以及它们是否能按预期工作。

测试第一件事相对简单——看看这个[样本回购](https://github.com/omerlh/helm-chart-tests-demo),看看它有多简单。

这允许我们使用`[kubetest](https://github.com/garethr/kubetest)`来测试生成的 Kubernetes 文件。这很好，但是会很复杂和混乱，特别是当你的模板文件中有很多分支的时候。

需要一个更好的解决方案——允许我们对模板进行单元测试，而不需要生成 Kubernetes 文件。直到最近，当我们开始在模板中有很多分支时，这还不是一个问题，我们现在正在寻找选项。

第二件事——测试 Kubernetes 文件是否有效——有点复杂。目前，在 Soluto 我们使用 Helm 的版本机制:每个图表都有一个版本，我们所有的服务都将使用最新的稳定版本。当推出新的图表版本时，我们可以在特定的服务上测试这个版本。如果它正常工作，更新其余的服务。另一个选择是使用`minikube`进行测试，但是它对于我们的需求来说太复杂了。

### 最后:部署！

所以现在我们有了一个用于舵图的 CI/CD 管道，并且我们准备了一个开发者可以使用的舵图。现在，当一个新的开发人员想要将新的服务部署到生产环境中时，他们所要做的就是:

1.使用`helm repo add chartmuseum http://<chart-museum-u` rl >
2 将我们的库添加到他们的本地舵中。创建一个新的 Helm 配置文件并指定所需的参数(例如服务的 docker 镜像)
3 `. Run helm upgrade —install <service-name> chartmusuem/web-api -f<path_` to_config_file >就这样。该服务处于活动状态。

为了更容易理解，我创建了一个示例库。这个存储库包含了我在这篇博文中讨论的所有内容:一个 web 应用程序的通用图表，它可以与这个图表和图表博物馆一起部署。查看它以更好地了解正在发生的事情，甚至还有一个演练来帮助您开始。

感谢您的阅读。如果您有任何问题，或者您需要 Helm 入门帮助，请随时通过这里的评论或通过 [Twitter](https://twitter.com/intent/tweet?text=.%20%40omerlh%2C%20I%20have%20a%20question%20about%20%40Helm&via=SolutoEng) 联系我们。

快乐驾驶！

最初发布在 [Soluto 的博客](https://blog.solutotlv.com/?utm_source=medium)