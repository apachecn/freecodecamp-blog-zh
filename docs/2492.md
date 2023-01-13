# 舵图教程:Kubernetes 软件包管理器解释

> 原文：<https://www.freecodecamp.org/news/helm-charts-tutorial-the-kubernetes-package-manager-explained/>

大规模运行生产服务有不同的方式。在生产中运行容器的一个流行解决方案是 Kubernetes。但是与 Kubernetes 直接互动有一些注意事项。

Helm 试图通过一些有用的功能来解决一些挑战，这些功能可以提高工作效率并减少复杂部署的维护工作。

在这篇文章中，你将学到:

*   什么是头盔
*   Helm 最常见的使用案例
*   如何配置和部署公开可用的 Helm 包
*   如何使用 Helm 部署自定义应用程序

本文中的每个代码示例都需要一个 Kubernetes 集群。获得集群的最简单方法是安装 Docker 并激活它的 Kubernetes 集群特性。此外，您需要[安装 kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) 和 [Helm](https://helm.sh/docs/intro/install/) 来与您的集群交互。

请注意:当你尝试这些例子时，要有耐心。如果你太快，那么容器还没有准备好。容器可能需要几分钟才能接收请求。

## 什么是头盔？

Helm 自称为“Kubernetes 包管理器”。它是一个命令行工具，使您能够创建和使用所谓的舵图。

Helm Chart 是描述一组 Kubernetes 资源的模板和设置的集合。它的功能从管理单个节点定义到高度可伸缩的多节点集群。

Helm 的架构在过去几年中发生了变化。Helm 的当前版本通过 Rest 直接与您的 Kubernetes 集群通信。如果你在 Helm 的上下文中读到一些关于 Tiller 的东西，那么你是在读一篇旧文章。舵柄在头盔 3 中被移除。

Helm 本身是有状态的。当安装了 Helm Chart 后，定义的资源将被部署，元信息将存储在 Kubernetes secrets 中。

## 如何部署简单的 Helm 应用程序

让我们把手弄脏，确保头盔随时可用。

首先，我们需要连接到 Kubernetes 集群。在本例中，我将重点介绍 Docker 安装中附带的 Kubernetes 集群。因此，如果您使用其他 Kubernetes 集群，配置和输出可能会有所不同。

```
$ kubectl config use-context docker-desktop

  Switched to context "docker-desktop".

$ kubectl get node

   NAME             STATUS   ROLES    AGE   VERSION
   docker-desktop   Ready    master   20d   v1.19.3
```

让我们使用 Helm 部署一个 Apache 服务器。作为第一步，我们需要通过添加一个 Helm 存储库来告诉 Helm 要搜索的位置:

```
$ helm repo add bitnami https://charts.bitnami.com/bitnami
```

让我们安装实际的容器:

```
$ helm install my-apache bitnami/apache --version 8.0.2
```

几分钟后，您的部署就准备好了。我们可以使用 kubectl 检查容器的状态:

```
$ kubectl get pods

  NAME                               READY   STATUS    RESTARTS   AGE
  my-apahe-apache-589b8df6bd-q6m2n   1/1     Running   0          2m27s
```

现在，打开 [http://localhost](http://localhost) 可以在本地看到默认的 Apache 暴露的网站。此外，Helm 可以向我们显示有关当前部署的信息:

```
$ helm list

 NAME     	REVISION	STATUS  	CHART       	VERSION
 my-apache	1		deployed  	apache-8.0.2	2.4.46
```

### 如何升级 Helm 应用程序

我们可以将部署的应用程序升级到新版本，如下所示:

```
$ helm upgrade my-apache bitnami/apache --version 8.0.3

$ helm list

NAME     	REVISION	STATUS  	CHART      	VERSION
my-apache	2       	deployed	apache-8.0.3	2.4.46
```

列修订表明这是我们部署的第二个版本。

### 如何回滚 Helm 应用程序

因此，让我们尝试回滚到第一个部署的版本:

```
$ helm rollback my-apache 1                    

Rollback was a success! Happy Helming!

$ helm list

NAME     	REVISION	STATUS  	CHART		VERSION
my-apache	3       	deployed	apache-8.0.2	2.4.46 
```

这是一个非常强大的功能，允许您快速回滚生产中的更改。

我提到过 Helm 将部署信息存储在秘密中，以下是这些信息:

```
$ kubectl get secret

NAME		  		  TYPE        	   	  	DATA   AGE
default-token-nc4hn               kubernetes.io/sat		3      20d
sh.helm.release.v1.my-apache.v1   helm.sh/release.v1		1      1m
sh.helm.release.v1.my-apache.v2   helm.sh/release.v1		1      1m
sh.helm.release.v1.my-apache.v3   helm.sh/release.v1		1      1m
```

### 如何移除已部署的 Helm 应用程序

让我们通过删除 my-apache 版本来清理我们的 Kubernetes:

```
$ helm delete my-apache

  release "my-apache" uninstalled
```

Helm 为您提供了一种非常方便的方式来管理一组应用程序，使您能够部署、升级、回滚和删除。

现在，我们准备使用更先进的头盔功能，将提高您的生产力！

## 如何访问生产就绪的舵图

您可以在公共中心搜索图表，这些图表使您能够使用可定制的配置快速部署所需的应用程序。

掌舵图不仅仅包含一组静态的定义。Helm 具有挂钩到 Kubernetes 部署的任何生命周期状态的能力。这意味着在应用程序的安装或升级过程中，可以执行各种操作，如在更新实际数据库之前创建数据库更新。

Helm Charts 的这一强大定义允许您共享和改进部署设置的可执行描述，从初始安装和版本升级到回滚功能。

对于像单节点 web 服务器这样的简单容器来说，Helm 可能很重，但是对于更复杂的应用程序来说，它非常有用。例如，它非常适合 Kafka 或 Cassandra 这样的分布式系统，这些系统通常运行在不同数据中心的多个分布式节点上。

我们已经利用 Helm 部署了一个 Apache 容器。现在，我们将部署一个生产就绪的 WordPress 应用程序，它包含:

*   为 WordPress 服务的容器，
*   用于持久性和
*   Prometheus sidecar 容器为每个 WordPress 容器提供健康指标。

在我们部署之前，建议将您的 Docker 限制增加到至少 4GB 内存。

设置一切听起来像是一项需要几周时间的工作。让它具有弹性和规模，这可能需要几个月的时间。在这些领域，掌舵图真的可以大放异彩。由于社区的增长，可能已经有一个我们可以使用的掌舵图。

### 如何部署 WordPress 和 MariaDB

舵图有不同的公共枢纽。其中一个就是 [artifacthub.io](https://artifacthub.io) 。我们可以搜索“WordPress”，找到一个有趣的 [WordPress 图表](https://artifacthub.io/packages/helm/bitnami/wordpress)。

在右侧，有一个安装按钮。如果你点击它，你会得到清晰的指示:

```
$ helm repo add bitnami https://charts.bitnami.com/bitnami

$ helm install my-wordpress bitnami/wordpress --version 10.1.4
```

您还会看到一些说明，告诉您如何在安装后访问管理界面和管理密码。

下面是如何在 Mac OS 上获取并解码管理员用户的密码:

```
$ echo Username: user
$ echo Password: $(kubectl get secret --namespace default my-wordpress-3 -o jsonpath="{.data.wordpress-password}" | base64 --decode)

Username: user
Password: sZCa14VNXe
```

在 windows 上，您可以在 powershell 中获取**用户**的密码:

```
$pw=kubectl get secret --namespace default my-wordpress -o jsonpath="{.data.wordpress-password}"
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($pw))
```

我们的本地开发将在: [http://localhost](http://localhost) 提供。
我们的管理界面将在: [https://localhost/admin](https://localhost/admin ) 可用。

所以我们有一切在本地运行它。但在制作中，我们希望扩大它的某些部分，以服务越来越多的游客。我们可以扩大 WordPress 服务的数量。我们还想公开一些健康指标，比如 CPU 和内存的使用情况。

我们可以[从 WordPress 图表的维护者那里下载产品](https://raw.githubusercontent.com/bitnami/charts/master/bitnami/wordpress/values-production.yaml)的示例配置。最重要的变化是:

```
### Start 3 WordPress instances that will all receive 
### requests from our visitors. A load-balancer will distribute calls 
### to all containers evenly.
replicaCount: 3

### start a sidecar container that will expose metrics for your wordpress container
metrics:
  enabled: true
  image:
    registry: docker.io
    repository: bitnami/apache-exporter
    tag: 0.8.0-debian-10-r243
```

让我们停止默认应用程序:

```
$ helm delete my-wordpress

  release "my-wordpress" uninstalled
```

### 如何启动多实例 WordPress 和 MariaDB 部署

使用生产值部署新版本:

```
$ helm install my-wordpress-prod bitnami/wordpress --version 10.1.4 -f values-production.yaml
```

这一次，我们运行了更多的容器:

```
$ kubectl get pods
NAME                                 READY   STATUS    RESTARTS   AGE
my-wordpress-prod-5c9776c976-4bs6f   2/2     Running   0          103s
my-wordpress-prod-5c9776c976-9ssmr   2/2     Running   0          103s
my-wordpress-prod-5c9776c976-sfq84   2/2     Running   0          103s
my-wordpress-prod-mariadb-0          1/1     Running   0          103s 
```

我们看到 4 行:1 行用于 MariaDB，3 行用于我们实际的 WordPress pods。

Kubernetes 中的 pod 是一组容器。每个组包含 2 个容器，一个用于 WordPress，另一个用于 Prometheus 的导出器，以特殊的格式公开有价值的度量。

在默认设置中，我们可以[打开 localhost](http://localhost) 并使用我们的 WordPress 应用程序。

### 如何访问公开的健康指标

我们可以通过代理其中一个正在运行的 pod 来检查公开的运行状况指标:

```
kubectl port-forward my-wordpress-prod-5c9776c976-sfq84 9117:9117
```

执行 port-forward 命令时，确保用您自己的 pod ID 替换 pod-id。

现在，我们连接到 WordPress Prometheus exporter 的端口 9117，并将端口映射到我们的本地端口 9117。打开 [http://localhost:9117](http://localhost:9117/metrics) 查看输出。

如果你不习惯普罗米修斯格式，一开始可能会有点混乱。但它实际上很容易读懂。没有 *'#'* 的每一行都包含一个度量键及其后面的值:

```
apache_cpuload 1.2766
process_resident_memory_bytes 1.6441344e+07
```

如果你不习惯这样的标准，不要担心，你会很快习惯的。你可以谷歌一下每一个键，找出它的含义。一段时间后，您将确定哪些指标对您最有价值，以及当您的容器接收到越来越多的生产流量时，它们会如何表现。

让我们通过以下方式来整理我们的设置:

```
$ helm delete my-wordpress-prod

  release "my-wordpress-prod" uninstalled
```

我们谈到了许多部署领域和特性。我们部署了多个 WordPress 实例，并将其扩展到更多的生产容器。你甚至可以更进一步，激活自动缩放。查看舵图的文档，并使用它！

### MariaDB 舵轮图

WordPress 的掌舵图的持久性依赖于 MariaDB。它建立在 MariaDB 的另一个[掌舵图的基础上，您可以根据需要进行配置和扩展，例如，通过启动多个副本。](https://artifacthub.io/packages/helm/bitnami/mariadb)

使用 Kubernetes 在生产中运行容器的可能性是巨大的。WordPress 图表的定义是公开的。

在下一节中，我们将使用一个基本的应用程序来创建我们自己的舵图，以理解创建舵图的基础，并使静态容器部署更加动态。

## 如何为自定义应用程序创建模板

Helm 为您的 Kubernetes 部署文件增加了更多的灵活性。Kubernetes 部署文件本质上是静态的。这意味着，调整像

*   期望的集装箱数量，
*   环境变量或
*   CPU 和内存限制

不能通过使用普通的 Kubernetes 部署文件进行调整。要么通过复制配置文件来解决这个问题，要么在 Kubernetes 部署文件中放置占位符，在部署时替换这些占位符。

这两种解决方案都需要做一些额外的工作，并且如果您部署大量具有不同变化的应用程序，将无法很好地扩展。

但可以肯定的是，有一个基于 Helm 的更智能的解决方案，它包含了 Helm 社区的许多方便的功能。让我们为一个博客引擎创建一个自定义图表，这次是为一个基于 NodeJS 的博客创建的，名为 [ghost blog](https://ghost.org/) 。

### 如何使用 Docker 创建一个幽灵博客

可以使用 pure Docker 启动一个简单的实例:

```
docker run --rm -p 2368:2368 --name my-ghost ghost
```

我们的博客地址: [http://localhost:2368](http://localhost:2368/) 。

让我们停止该实例，以便能够使用 Kubernetes 启动另一个实例:

```
$ docker rm -f my-ghost

  my-ghost
```

现在，我们想在 Kubernetes 集群中部署带有 2 个实例的 ghost 博客。让我们首先建立一个简单的部署:

```
# file 'application/deployment.yaml'

apiVersion: apps/v1
kind: Deployment
metadata:
  name: ghost-app
spec:
  selector:
    matchLabels:
      app: ghost-app
  replicas: 2
  template:
    metadata:
      labels:
        app: ghost-app
    spec:
      containers:
        - name: ghost-app
          image: ghost

          ports:
            - containerPort: 2368
```

并在它之前放置一个负载平衡器，以便能够访问我们的容器并将流量分配给两个容器:

```
# file 'application/service.yaml'

apiVersion: v1
kind: Service
metadata:
  name: my-service-for-ghost-app
spec:
  type: LoadBalancer
  selector:
    app: ghost-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 2368
```

我们现在可以使用 kubectl 部署这两种资源:

```
$ kubectl apply -f ./appplication/deployment.yaml -f ./appplication/service.yaml

  deployment.apps/ghost-app created
  service/my-service-for-ghost-app created
```

ghost 应用程序现在可以通过 [http://localhost](http://localhost) 获得。让我们再次停止应用程序:

```
$ kubectl delete -f ./appplication/deployment.yaml -f ./appplication/service.yaml

  deployment.apps/ghost-app delete
  service/my-service-for-ghost-app delete
```

到目前为止还不错，它与普通 Kubernetes 一起工作。但是如果不同的环境需要不同的设置呢？

假设我们想在不同阶段(非生产、生产)将它部署到多个数据中心。你最终会一遍又一遍地复制你的 Kubernetes 文件。这将是维修的地狱。我们可以利用 Helm，而不是编写大量的脚本。

让我们从头开始创建一个新的舵图:

```
$ helm create my-ghost-app

  Creating my-ghost-app
```

Helm 为您创建了一系列文件，这些文件通常对 Kubernetes 中的生产就绪服务非常重要。为了专注于最重要的部分，我们可以删除大量创建的文件。让我们看一下这个例子中唯一需要的文件。

我们需要一个名为 Chart.yaml 的项目文件:

```
# Chart.yaml

apiVersion: v2
name: my-ghost-app
description: A Helm chart for Kubernetes
type: application
version: 0.1.0
appVersion: 1.16.0
```

部署模板文件:

```
# templates/deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: ghost-app
spec:
  selector:
    matchLabels:
      app: ghost-app
  replicas: {{ .Values.replicaCount }}
  template:
    metadata:
      labels:
        app: ghost-app
    spec:
      containers:
        - name: ghost-app
          image: ghost
          ports:
            - containerPort: 2368
          env:
            - name: url
              {{- if .Values.prodUrlSchema }}
              value: http://{{ .Values.baseUrl }}
              {{- else }}
              value: http://{{ .Values.datacenter }}.non-prod.{{ .Values.baseUrl }}
              {{- end }}
```

它看起来非常类似于我们普通的 Kubernetes 文件。在这里，您可以看到副本数量的不同占位符，以及名为 url 的环境变量的 if-else 条件。在下面的文件中，我们将看到所有定义的值。

服务模板文件:

```
# templates/service.yaml

apiVersion: v1
kind: Service
metadata:
  name: my-service-for-my-webapp
spec:
  type: LoadBalancer
  selector:
    app: ghost-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 2368
```

我们的服务配置是完全静态的。

模板的值是我们的舵图最后缺失的部分。最重要的是，需要一个名为 values.yaml 的缺省值文件:

```
# values.yaml

replicaCount: 1
prodUrlSchema: false
datacenter: us-east
baseUrl: myapp.org
```

舵图应该能够使用默认值运行。在继续之前，请确保您已经删除了:

*   my-ghost-app/templates/tests/test-connection . YAML
*   my-ghost-app/templates/service account . YAML
*   my-ghost-app/templates/ingress . YAML
*   my-ghost-app/templates/HPA . YAML
*   my-ghost-app/templates/notes . txt

我们可以通过执行“试运行”来获得发送给 Kubernetes 的最终输出:

```
$ helm template --debug my-ghost-app

install.go:159: [debug] Original chart version: ""
install.go:176: [debug] CHART PATH: /helm/my-ghost-app

---
# Source: my-ghost-app/templates/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service-for-my-webapp
spec:
  type: LoadBalancer
  selector:
    app: my-example-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 2368
---
# Source: my-ghost-app/templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ghost-app
spec:
  selector:
    matchLabels:
      app: ghost-app
  replicas: 1
  template:
    metadata:
      labels:
        app: ghost-app
    spec:
      containers:
        - name: ghost-app
          image: ghost
          ports:
            - containerPort: 2368
          env:
            - name: url
              value: us-east.non-prod.myapp.org 
```

Helm 插入了所有值，还将 url 设置为 *`us-east.non-prod.myapp.org`* ，因为在*`values.yaml`*`prodUrlSchema`中设置为 false，数据中心设置为 us-east。

为了获得更大的灵活性，我们可以定义一些覆盖值文件。让我们为每个数据中心定义一个:

```
# values.us-east.yaml
datacenter: us-east
```

```
# values.us-west.yaml
datacenter: us-west
```

每个阶段一个:

```
# values.nonprod.yaml
replicaCount: 1
prodUrlSchema: false
```

```
# values.prod.yaml
replicaCount: 3
prodUrlSchema: true
```

我们现在可以使用 Helm 将它们组合起来，并再次检查结果:

```
$ helm template --debug my-ghost-app -f my-ghost-app/values.nonprod.yaml  -f my-ghost-app/values.us-east.yaml 

install.go:159: [debug] Original chart version: ""
install.go:176: [debug] CHART PATH: /helm/my-ghost-app

---
# Source: my-ghost-app/templates/service.yaml
# templates/service.yaml

apiVersion: v1
kind: Service
metadata:
  name: my-service-for-my-webapp
spec:
  type: LoadBalancer
  selector:
    app: my-example-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 2368
---
# Source: my-ghost-app/templates/deployment.yaml
# templates/deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: ghost-app
spec:
  selector:
    matchLabels:
      app: ghost-app
  replicas: 1
  template:
    metadata:
      labels:
        app: ghost-app
    spec:
      containers:
        - name: ghost-app
          image: ghost
          ports:
            - containerPort: 2368
          env:
            - name: url
              value: http://us-east.non-prod.myapp.org 
```

当然，我们可以做最后的部署:

```
$ helm install -f my-ghost-app/values.prod.yaml my-ghost-prod ./my-ghost-app/

NAME: my-ghost-prod
LAST DEPLOYED: Mon Dec 21 00:09:17 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

和以前一样，我们的 ghost 博客可以通过 [http://localhost](http://localhost) 访问。

我们可以删除此部署，并使用 us-east 和非生产设置部署应用程序，如下所示:

```
$ helm delete my-ghost-prod                                                 
  release "my-ghost-prod" uninstalled

$ helm install -f my-ghost-app/values.nonprod.yaml -f my-ghost-app/values.us-east.yaml my-ghost-nonprod ./my-ghost-app
```

我们最终通过 Helm 清理了我们的 Kubernetes 部署:

```
$ helm delete my-ghost-nonprod
```

因此，我们可以根据需要组合多个覆盖值文件。我们可以灵活地自动化部署，这是部署管道的许多用例所需要的。

特别是对于公司来说，这意味着只需定义一次图表框架，以确保满足所需的标准。以后，您可以复制它们并根据应用程序的需要进行调整。

## 结论

强大的模板引擎以及执行发布、升级和回滚的可能性让 Helm 变得非常棒。除此之外，还有公开发布的 Helm Chart Hub，其中包含数千个可用于生产的模板。如果你在更大的范围内使用 Kubernetes，这使得 Helm 成为你工具箱中的必备工具！

我希望你喜欢这个实践教程。激励自己四处搜索，查看其他示例，部署容器，连接它们，并使用它们。

您将在未来学到许多很酷的特性，这些特性使您能够以一种轻松、可重用和可伸缩的方式将您的应用程序发布到产品中。

一如既往，我感谢任何反馈和意见。我希望你喜欢这篇文章。如果你喜欢它，觉得需要一轮掌声，[在 Twitter 上关注我](https://twitter.com/sesigl)。我在易贝·克莱南泽根公司工作，这是全球最大的机密公司之一。顺便说一下，[我们正在招聘](https://jobs.ebayclassifiedsgroup.com/ebay-kleinanzeigen)！

参考资料:

*   [https://helm.sh/docs/chart_template_guide/getting_started/](https://helm.sh/docs/chart_template_guide/getting_started/)