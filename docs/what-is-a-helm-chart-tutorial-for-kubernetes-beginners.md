# 什么是舵轮图？Kubernetes 初学者教程

> 原文：<https://www.freecodecamp.org/news/what-is-a-helm-chart-tutorial-for-kubernetes-beginners/>

Kubernetes 对于云原生开发者来说是一个非常有用的工具。但是它本身并没有涵盖所有的基础——有些事情 Kubernetes 无法解决或者超出了它的范围。

这是开源项目如此伟大的原因之一。当我们将它们与其他优秀的开源工具结合在一起时，它们帮助令人惊叹的工具变得更加令人惊叹。通常这些工具的开发仅仅是为了填补空白。其中一个工具是头盔。

## 什么是头盔？

Helm 被广泛称为“T2 Kubernetes 的包装经理”。虽然它是这样呈现的，但是它的范围远远超出了简单的包管理器的范围。不过，还是从头说起吧。

Helm 是一个开源项目，最初由[de iblas](https://deislabs.io/)创建，并捐赠给 [CNCF](https://azure.microsoft.com/blog/announcing-cncf/?WT.mc_id=containers-19838-ludossan) ，现在由后者维护。Helm 的最初目标是为用户提供一种更好的方式来管理我们在 [Kubernetes](https://azure.microsoft.com/services/kubernetes-service/?WT.mc_id=containers-19838-ludossan) 项目上创建的所有 [Kubernetes](https://azure.microsoft.com/services/kubernetes-service/?WT.mc_id=containers-19838-ludossan) YAML 文件。

为了解决这个问题,[掌舵](https://docs.microsoft.com/azure/aks/kubernetes-helm?WT.mc_id=containers-19838-ludossan)采取的路径是创建掌舵**T3 图表**。每个图表都是一个包含一个或多个清单的包——一个图表可以有子图表和依赖图表。

这意味着如果您为顶层图表运行 install 命令，Helm 将安装项目的整个依赖树。你只需要一个命令来安装你的整个应用程序，而不是通过`kubectl`列出要安装的文件。

图表也允许你对你的清单文件进行版本控制，就像我们对 Node.js 或任何其他包所做的一样。这允许您安装特定的图表版本，这意味着以代码的形式保存基础设施的特定配置。

Helm 还保留了所有已部署图表的发布历史，因此如果出现问题，您可以返回到以前的版本。

[Helm](https://docs.microsoft.com/azure/aks/kubernetes-helm?WT.mc_id=containers-19838-ludossan) 原生支持 [Kubernetes](https://azure.microsoft.com/services/kubernetes-service/?WT.mc_id=containers-19838-ludossan) ，这意味着你不必编写任何复杂的语法文件或任何东西来开始使用 Helm。只要把你的模板文件放到一个新的图表中，你就可以开始了。

但是为什么要用呢？管理应用程序清单可以通过几个命令组合轻松完成。

## 你为什么要使用头盔？

Helm 确实在 Kubernetes 没有去的地方大放异彩。例如，模板化。Kubernetes 项目的范围是为您处理您的容器，而不是您的模板文件。

这使得创建在大型团队或大型组织中使用的真正通用的文件变得非常困难，因为需要为每个文件设置许多不同的参数。

此外，当模板文件是纯文本时，如何使用 Git 对敏感信息进行版本控制？

答案是:Go 模板。Helm 允许您在模板文件中添加变量和使用函数。这使得它非常适合那些最终需要更改参数的可伸缩应用程序。让我们看一个例子。

我有一个开源项目叫做 [Zaqar](https://github.com/khaosdoctor/zaqar/) ，这是一个用于 Node.js 的简单电子邮件微服务，它与 SendGrid 通信。该项目基本上由一个服务、一个部署和一个自动缩放器组成。

让我们以部署文件为例。我会有这样的东西:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: zaqar
  namespace: default
  labels:
    app: zaqar
    version: v1.0.0
    env: production
spec:
  replicas: 1
  selector:
    matchLabels:
      app: zaqar
      env: production
  template:
    metadata:
      labels:
        app: zaqar
        version: v1.0.0
        env: production
    spec:
      containers:
        - name: zaqar
          image: "khaosdoctor/zaqar:v1.0.0"
          imagePullPolicy: IfNotPresent
          env:
            - name: SENDGRID_APIKEY
              value: "MY_SECRET_KEY"
            - name: DEFAULT_FROM_ADDRESS
              value: "my@email.com"
            - name: DEFAULT_FROM_NAME
              value: "Lucas Santos"
          ports:
            - name: http
              containerPort: 3000
              protocol: TCP
          resources:
            requests:
              cpu: 100m
              memory: 128Mi
            limits:
              cpu: 250m
              memory: 256Mi
```

如果我想在 [CI 管道](https://docs.microsoft.com/learn/modules/aks-deployment-pipeline-github-actions/?WT.mc_id=containers-19838-ludossan)上使用这个模板，或者在我的 [GitHub](https://github.com/khaosdoctor) 上发布它，我需要用占位符替换可变部分。所以我们可以用需要的信息替换这些文本。

在这种情况下，版本标签、`env`标签和环境变量都将被占位符替换，如下所示:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: zaqar
  namespace: default
  labels:
    app: zaqar
    version: #!VERSION!#
    env: #!ENV!#
spec:
  replicas: 1
  selector:
    matchLabels:
      app: zaqar
      env: #!ENV!#
  template:
    metadata:
      labels:
        app: zaqar
        version: #!VERSION!#
        env: #!ENV!#
    spec:
      containers:
        - name: zaqar
          image: "khaosdoctor/zaqar:#!VERSION!#"
          imagePullPolicy: IfNotPresent
          env:
            - name: SENDGRID_APIKEY
              value: "#!SENDGRID_KEY!#"
            - name: DEFAULT_FROM_ADDRESS
              value: "#!FROM_ADDR!#"
            - name: DEFAULT_FROM_NAME
              value: "#!FROM_NAME!#"
          ports:
            - name: http
              containerPort: 3000
              protocol: TCP
          resources:
            requests:
              cpu: 100m
              memory: 128Mi
            limits:
              cpu: 250m
              memory: 256Mi
```

我们现在可以运行我们的 [CI 管道](https://docs.microsoft.com/learn/modules/aks-deployment-pipeline-github-actions/?WT.mc_id=containers-19838-ludossan)。但在此之前，我们需要用实际值替换占位符。

为此，我们可以使用`sed`和它的“超级简单”语法`sed 's/#!PLACEHOLDER!#/replacement/g'`，直到我们完成所有的占位符。最后的命令应该是这样的:

```
cat deploy.yaml | \
    sed 's/#!ENV!#/production/g' | \
    sed 's/#!VERSION!#/v1.0.0/g' | \
    sed 's/#!SENDGRID_KEY!#/MyKey/g' | \
    sed 's/#!FROM_ADDR!#/my@email.com/g' | \
    sed 's/#!FROM_NAME!#/Lucas Santos/g'
```

默认情况下，sed 将所有内容输出到`stdout`，因此我们可以向`kubectl -f`添加另一个管道，就像`<all the command from before> | kubectl -f -`。那我们就可以部署到位了。唯一的问题是，我们需要对所有的**和其他的**文件做同样的处理。

现在想象一个更大的项目，有许多其他变量和占位符。你可能会写一个脚本来帮你做这件事，不是吗？那个剧本是赫尔姆。

当您创建一个图表时(稍后将详细介绍)，我们有一个必须遵循的特定目录树，以便 Helm 了解我们想要做什么。在`templates`目录中，我们可以添加我们的清单文件，**用本地的 go 模板，**如下:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Values.name }}
  namespace: {{ default .Release.Namespace .Values.namespace }}
  labels:
    app: {{ .Values.name }}
    version: {{ .Values.image.tag }}
    env: {{ .Values.env }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Values.name }}
      env: {{ .Values.env }}
  template:
    metadata:
      labels:
        app: {{ .Values.name }}
        version: {{ .Values.image.tag }}
        env: {{ .Values.env }}
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "khaosdoctor/zaqar:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          env:
            - name: SENDGRID_APIKEY
              value: {{ required "You must set a valid Sendgrid API key" .Values.environment.SENDGRID_APIKEY | quote }}
            - name: DEFAULT_FROM_ADDRESS
              value: {{ required "You must set a default from address" .Values.environment.DEFAULT_FROM_ADDRESS | quote }}
            - name: DEFAULT_FROM_NAME
              value: {{ required "You must set a default from name" .Values.environment.DEFAULT_FROM_NAME | quote }}
          ports:
            - name: http
              containerPort: 3000
              protocol: TCP
          resources:
            {{- toYaml .Values.resources | nindent 12 }}
```

所有这些值都可以从一个`Values.yaml`文件中获得(默认值)，或者您可以在 CLI 中使用`--set <path> value`标志来设置它们。

如果我们想安装我们的图表，我们可以发出以下命令:

```
helm upgrade --install --create-namespace myChart ./path/to/my/chart \
  --set image.tag=v1.0.0 \
  --set env=production \
  --set environment.SENDGRID_APIKEY=myKey \
  --set environment.DEFAULT_FROM_ADDRESS="my@email.com" \
  --set environment.DEFAULT_FROM_NAME="Lucas Santos"
```

Helm 还允许我们在部署中使用函数。所以我们可以让`default`函数在没有被填充的情况下返回默认值，就像名称空间一样。或者我们可以使用`required`,它显示一条消息，如果没有提供值，就无法安装图表，这就是我们的环境变量的情况。

在他们的[文档](https://helm.sh/docs/chart_template_guide/)中还有很多其他有用的功能——看看吧。

现在，我们不仅能够更有效地管理我们的应用资源，而且能够在开源版本系统中发布这些资源，没有任何麻烦或安全问题。

## 如何创建舵图

在 Helm 中创建图表非常容易。首先，你需要安装[头盔](https://helm.sh/docs/intro/quickstart/)。然后，只需输入`helm create <chart name>`，它将创建一个目录，其中包含文件和其他目录。Helm 创建图表需要这些文件。

让我们仔细看看这个文件树是什么样子，以及其中有哪些文件:

*   **chart.yaml:** 这是你放置图表相关信息的地方。其中包括图表版本、名称和描述，因此如果您将它发布到开放的存储库中，就可以找到它。同样在这个文件中，你可以使用`dependencies`键设置外部[依赖关系](https://helm.sh/docs/topics/charts/#chart-dependencies)。
*   **values.yaml** :就像我们之前看到的，这是包含变量默认值的文件。
*   模板(目录):这是放置所有清单文件的地方。这里的一切都将在 Kubernetes 得到传承和创造。
*   **图表:**如果您的图表依赖于您拥有的另一个图表，或者如果您不想依赖于 Helm 的默认库(Helm 从中提取图表的默认注册表)，您可以在这个目录中引入相同的结构。图表依赖项是从下到上安装的，这意味着如果图表 A 依赖于图表 B，而 B 依赖于 C，安装顺序将是 C - > B - > A

还有其他字段，但这些是最常见的，也是必需的。你可以快速浏览一下 [Zaqar 的知识库](https://github.com/khaosdoctor/zaqar/tree/master/helm)，看看我们如何发布开源图表。

快速提示:安装 Helm 时，确保你安装的是版本 3。版本 2 仍然有效，但是它需要一个名为 Tiller 的服务器端组件，将您的 helm 安装绑定到一个集群。Helm 3 增加了几个 CRD，消除了这个需求，但是并不是所有的 Kubernetes 版本都支持它。

## 如何主持舵图

好了，你创建了你的图表，现在做什么？我们必须下载整个存储库来安装这些图表吗？不要！Helm 有一个用于最常用图表的公共库，类似于 Docker Hub。

你也可以创建自己的图表库，并在线托管它。Helm 和家酿或者 Linux 喝的是同一个喷泉。您可以点击这些存储库来下载其中包含的图表。

因为图表存储库基本上是一个静态 web 服务器提供的`index.yaml`文件，所以您几乎可以在任何地方创建图表存储库。

以 [Zaqar](https://github.com/khaosdoctor/zaqar/tree/master/helm) 为例——它托管在 GitHub 页面上，可以通过[我的域名](https://lsantos.me/zaqar/helm/index.yaml)访问。当 Helm 查找一个`index.yaml`文件时，它实际上是在查找该图表的可用版本列表、它们的 SHA256 摘要以及打包的`.tgz`文件的位置，以便下载图表本身。这几乎就是 NPM 在幕后做的事情(过于简化)。

这意味着您不需要永远克隆您的存储库，并且您的图表也可以是私有的。您只需要创建一个图表存储库。

你甚至可以使用像 Azure CR 这样的[托管服务来完成这项工作，或者你可以有一个名为](https://docs.microsoft.com/azure/container-registry/container-registry-helm-repos?WT.mc_id=containers-19838-ludossan)[图表博物馆](https://github.com/helm/chartmuseum)的完整解决方案，它允许你存储你的图表，并为你提供一个整洁的用户界面。

## 结论

赫尔姆会留下来。长期以来，它已经并将继续帮助许多 Kubernetes 开发者。

如果你想知道如何使用 Helm，你可以参考他们的[文档](https://helm.sh)，或者你可以参加这个[的免费学习模块](https://docs.microsoft.com/learn/modules/aks-app-package-management-using-helm/?WT.mc_id=containers-19838-ludossan)，学习如何使用 Helm 轻松地在 Kubernetes 上部署你的应用。