# 如何选择正确的容器编排以及如何部署它

> 原文：<https://www.freecodecamp.org/news/how-to-choose-the-right-container-orchestration-and-how-to-deploy-it-41844021c241/>

迈克尔·道格拉斯

# 如何选择正确的容器编排以及如何部署它

![vbWtSLRX0gqNpbfNrjFI5rZXZWm71-4D4nXz](img/03313a14c40ed8a840dbca937a700914.png)

在容器中运行服务器进程已经成为一种趋势。如果您的环境很小，只有几台服务器运行几十个容器，那么您很可能不用手工做任何事情。除此之外，您还需要很好的工具来处理繁重的工作，并提供一个通用的基线功能。另一种选择是大量繁琐、容易出错、重复的手动工作。

如果您不利用 CI/CD 管道和编排系统，开发和运营将不得不执行极端、持续的协作和协调。

![321SoCA0X4RdnVeQZsY4ttSZPRd4cBi2pdZf](img/ff40c599af2b88c63dbc8c38ac44eba6.png)

Image Courtesy Julius Silver - [Pexels.com](https://www.pexels.com/photo/white-water-boat-753331/) — I sure hope they have an orchestration for how containers are loaded on these vessels… I cannot imagine the variables involved: Weight distribution. Destination and order of removal. Volatility. This image makes me glad I work in software where I get to help manage the complexity!

当我今年早些时候开始调查微服务世界时，我并不知道我将发现的广泛的支持基础设施。Kubernetes 绝对是一个发现的宝库，Istio 似乎只是微服务的惊人之举——尽管我知道我只是触及了这两项技术的皮毛。

从不到三年前的卑微起步，Kubernetes 已经迅速成长为一个令人惊叹的编排引擎，被无数公司采用，并嵌入到许多其他项目中。作为一名拥有几十年经验的软件设计师，我对 Kubernetes 架构印象深刻。它是非常模块化的，并且是在许多部件可以被替换的预期下建造的。在某些情况下，给定的组件已经有许多选择。

所有这些新奇和多样的选择会让开始变得相当困难。就在我坐在全面进入 Kubernetes 的边缘时，我被一个更基本的决定震惊了…

### 做出正确的容器编排选择

随着我开始更深入地研究容器编排，很明显有很多选择。我的直觉告诉我应该用 Kubernetes，但是我也开始怀疑我怎么知道我是对的。没有什么比不确定性更能让人挖掘得更深了。

我的第一个问题是，容器编排的替代方案是什么？

在花了相当多的时间搜索和阅读之后，下面是我能找到的编排系统列表:

*   Kubernetes -他们中明显的大人物。这个项目本身非常活跃，并且这个架构让我感到安慰，继续开发将会是快速和安全的。这是我本能的选择。
*   Docker Swarm -这是默认内置在 Docker 中的，拥有很多你想要的系统核心功能。它与 Kubernetes 有很多相似之处，但它缺少一个关键的东西，即免费的开源版本是基于角色的访问控制(RBAC)。你可以在付费的企业版中获得。
*   [马拉松](https://github.com/mesosphere/marathon)在 [Mesos](http://mesos.apache.org/) 上——Mesos 本身是一个高度可扩展的集群系统，用于运行各种任务。它依赖框架来支持不同类型的任务，而 Marathon 是一个插件，它为 Mesos 生态系统中的容器编排提供支持。T4 的框架列表令人印象深刻。
*   [Titus](https://github.com/Netflix/titus) -当我写这篇文章的时候，网飞[开源了他们的内部编排系统](https://medium.com/netflix-techblog/titus-the-netflix-container-management-platform-is-now-open-source-f868c9fb5436)。谢谢网飞！Titus 旨在提供与亚马逊 AWS 基础设施(网飞维护其运营)的最紧密集成。他们的意图之一是其他项目将使用他们的技术，以便网飞在未来可以使用它们。
*   [黄牛](https://github.com/rancher/cattle)——这是为牧场主系统设计的编排引擎，嵌入在牧场主系统中。我没有给牛一个非常深刻的印象，因为它的父项目显然已经购买了 Kubernetes 作为其首选和主要的编排引擎。牧场主网站上的主要标题是，“企业 Kubernetes 使之变得容易。”该页面充满了它如何帮助您运行 Kubernetes 集群。网页上没有提到牛。很明显，牧场主项目已经做出了选择。
*   好的，这是哈希公司。作为哈希公司的超级粉丝，如果我没有给他们的产品至少看一眼，我会觉得不公平。该产品表面上看起来很有趣，但有一些相当大的付费墙问题。名称空间仅在企业版中可用。对于服务发现，您需要添加 Consul，对于秘密管理，您需要添加 Vault。通过查看文档，它似乎还缺少基本的 CNI 配置—网络配置的主要讨论是关于映射端口和静态 IP 映射。
*   kontena——这是一款视觉震撼的产品。您可以在他们的云产品中运行，也可以在自己选择的基础设施上安装自己的平台主机。如果您选择自带基础设施，您可以选择以 15 美元/月的价格将其连接到 Kontena Cloud，也可以不连接。在这种情况下，漂亮的网络界面是你所放弃的。除了在他们的网站周围挖掘了几个小时之外，我没有深入研究过，所以我不确定这会造成什么影响。

如果你足够努力的话，还可以找到其他的一些提示:Deis、Mantl、Cloud Foundry 和 Amazon ECS 等等。这些人应该得到的不仅仅是简单的荣誉奖。

#### 需求第一

在这里做出选择是困难的。当然，这取决于您的需求，所以让我列出几个重要的需求:

1.  **积极发展:**容器编排世界相对年轻。不活跃的项目将会很快落后，这意味着错误没有被解决。我感觉牛快要灭绝了。所以我在这里刮掉它。
2.  **没有云供应商锁定:**我目前没有兴趣与任何一家云供应商捆绑在一起。Titus 由于与 AWS 的紧密集成而落在了后面，这无疑是一个缺点。
3.  简单:系统越复杂，操作起来就越困难。这个需求导致我放弃了 Mesos，因为它首先不是一个容器编排系统。它试图成为很多人的很多东西，这感觉像是一个错误的适合。
4.  **CNI 网络:**在我的服务之间保持简单的网络连接是非常重要的。我不希望开发人员花费时间在特殊用途的代码上来寻找依赖的服务。Docker Swarm 和 Kubernetes，你们两个都还有机会。
5.  **RBAC 命名空间-** 我在一个公司环境中工作，我的目标之一是提供不冲突的开发、QA、试运行和生产设置。我可以为每一个设置单独的集群，或者我可以使用 RBAC 并共享我的计算能力。码头工人 Swarm，我很遗憾你要走了，但这是我们共同旅程的终点。我喜欢 Hashicorp，但 Nomad 也将这一功能置于付费墙之后。

现在你知道了，一些相当高层次的要求很快就削减了比赛场地。将 Mesos 排除在“简单性”类别之外似乎不太公平。但是如果你花我一半的时间来研究所有这些选项，你会明白，在某些时候，你必须简化你的决策，以便真正开始前进。

我感到奇怪的是，Kubernetes 和 Kontena 仍然在名单上。Kontena 实际上是第 11 个小时的调查。我差点把它归入其他人的列表。如果我这样做了，我写作的最后一个小时就不会那么痛苦了。但这就是了。必须做出决定，虽然我最终会回到 Kontena，但 Kubernetes 是我目前的投票。

把这么多令人惊叹的项目留在剪辑室的地板上，我感到很内疚。这就是在当今这个有着惊人选择的世界里发生的事情，再加上做决定的古老需求。

### Kubernetes 入门

所以我选择 Kubernetes 作为我的容器编排系统。我如何让集群运行以供测试和生产使用？这个问题的答案也各不相同。

#### Kubernetes 部署方法

*   [Minikube](https://github.com/kubernetes/minikube) :为了测试和开发目的，让单节点 Kubernetes 快速运行的推荐方法。我更喜欢看到完整的操作，所以我没有满足于我的测试中的单个节点部署。
*   Kubeadm :这是由 kubernetes.io 提供的一种部署单主多节点集群的方法。还提供了设置多主配置的附加说明。我以前通过一些 Terraform 脚本使用 Kubeadm 来设置我的数字海洋试验台集群。
*   Docker Enterprise 2.0 :当我在写这篇文章的时候，Docker 宣布升级到 EE 2.0。这个新版本现在将一个完整的 Kubernetes 部署整合到了产品中。通过快速阅读，他们利用 Swarm 引导集群并部署 Kubernetes。
*   牧场主:“企业 Kubernetes 变得容易”是他们的主张。事实上，通过遵循他们的指南，我能够在不到一个小时的时间内在数字海洋上运行完整的 Kubernetes 集群。我最初的反应是:“圣牛！牧场主真了不起。”它支持管理许多环境中的 Kubernetes 部署，并简化了高可用性部署。它声称允许管理多个集群，同时管理其他编排方案，包括它们自己的 own 和 Apache Mesos。
*   Mesosphere DC/OS :作为一个独立的容器编排系统，它可能会成为重量级冠军，但现在也能够管理 Kubernetes 集群。这个产品看起来很吸引人……除了真正好的东西在[企业薪酬墙](https://mesosphere.com/pricing/)下面。从他们的网站上我也不清楚 DC/OS 版本是免费的，DC/OS 企业版是付费的(或者他们都是付费的)。每当我看到“联系我们了解价格”时，我就会继续前进。这将使我不会看得太仔细——向我冒犯的任何人道歉。
*   [Kontena 的 Pharos](https://pharos.sh/)——似乎即使是那些拥有 Kubernetes 的完整替代方案的公司也无法置身于 Kubernetes 部署软件计划之外。他们的“[使用 Terraform](https://pharos.sh/docs/usage/terraform.html) ”文档看起来很有力量，可以让你的 Kubernetes 安装成为一个独特的、可组合的步骤。您可以使用任何工具一步完成基础设施的设置，然后在此基础上设置 Kubernetes。`setup-infrastructure | install-kubernetes > pro`合身

名单还在继续:Pivitol 的 Kubo，Apprenda Kismatic，CoreOS structural，RedHat Openshift v3，Openshift Origin，当然还有更多。

#### 托管选项

*   [亚马逊 EKS](https://aws.amazon.com/eks/)-Kubernetes 的弹性容器服务——亚马逊托管的 Kubernetes 集群。这是目前亚马逊的一项“预览”技术。这说明了 Kubernetes 的生存能力和未来…
*   [谷歌 Kubernetes 引擎(GKE)](https://cloud.google.com/kubernetes-engine/) —这是谷歌的托管服务。我想说更多，但由于某种原因，我的帐户是打破了获得它。
*   [OpenShift](https://www.openshift.com/) -红帽的在线容器服务。

#### 我的 Kubernetes 部署选择？

对于 Kubernetes 的部署，我计划继续与 Kubeadm(可能用 Pharos 代替)以及 Rancher 合作。

我第一次用它的时候，Rancher 表现出了巨大的潜力。唯一的缺点是，我必须首先有一个安装 Rancher 的控制机器，但这是一个很小的代价。我不确定我是否想使用 Rancher 接口与我的 Kubernetes 集群进行交互，只要它不妨碍我使用`kubectl`来控制集群，我们就可以相处得很好。

### 下一步是什么？

现在，我已经通过练习了解了选项的世界，我准备开始使用 Kubernetes 进行实验。对于我选择的部署方法，我需要做很多探索。

我之前也谈到过 Istio，它位于 Kubernetes 之上，为支持微服务通信和监控提供了更多基础。期待在接下来的文章中看到更多。哦，现在我被 Kontena 绊倒了，我觉得有必要试一试。？