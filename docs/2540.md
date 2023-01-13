# Kubernetes VS Docker:有什么区别？举例说明

> 原文：<https://www.freecodecamp.org/news/kubernetes-vs-docker-whats-the-difference-explained-with-examples/>

如今，开发人员工具箱中的两个基本工具是 Docker 和 Kubernetes。两者都允许开发人员将应用程序打包到容器中，以便在不同的环境中运行。

虽然你可以用这两种方法实现相似的东西，但实际上它们的用法是不同的。

在本文中，您将获得对 Docker 和 Kubernetes 的解释，并且您将构建一个示例 NodeJS web 应用程序并使用这两种技术部署它。

## Docker 是什么？

以下是人们在维基百科上对 Docker 的定义:

> “Docker 可以将一个应用程序及其依赖项打包到一个虚拟容器中，该容器可以在任何 Linux 服务器上运行。这使得应用程序可以在各种位置运行，例如内部、公共云和/或私有云中。Docker 使用 Linux 内核的资源隔离特性(如 cgroups 和内核命名空间)和支持 union 的文件系统(如 OverlayFS)来允许容器在单个 Linux 实例中运行，从而避免了启动和维护虚拟机的开销。”—维基百科

简而言之，Docker 是一个在期望的机器上运行不可变容器的平台，这些容器以接近本机的性能封装。

Docker 有类似属性的替代品，如 LC、rkt 或 containerd。Docker 只是最受欢迎的一个。

## What is Kubernetes?

以下是人们在维基百科上对 Kubernetes 的定义:

> Kubernetes 定义了一组构建块(“原语”)，它们共同提供了基于 CPU、内存或自定义指标部署、维护和扩展应用程序的机制。Kubernetes 是松散耦合的，可以扩展以满足不同的工作负载。这种可扩展性很大程度上是由 Kubernetes API 提供的，内部组件以及运行在 Kubernetes 上的扩展和容器都使用这个 API。该平台通过将资源定义为对象来控制计算和存储资源，然后可以像这样管理这些资源。—维基百科

简而言之，Kubernetes 管理多个主机并向它们部署容器。在这些主机上运行容器最常用的容器技术是 Docker。

说够了，让我们亲自动手体验一下不同之处。

## 如何使用 Docker 和 Kubernetes 构建和部署 NodeJS web 应用程序？

如果你还没有安装 Docker，你应该这样做。检查并安装 https://docs.docker.com/get-docker/的 Docker。

```
$ docker --version

Docker version 19.03.13, build 4484c46d9d
```

让我们创建一个 NodeJS 包文件，并添加一个名为 [Express](https://expressjs.com) 的 webserver 依赖项。

```
// file: package.json

{
  "name": "docker-vs-k8s",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

此外，我们需要启动 web 服务器并定义一个端点。

```
// file: server.js

const express = require('express');

// Constants
const PORT = 8080;
const HOST = '0.0.0.0';

// App
const app = express();
app.get('/', (req, res) => {
  res.send('Hello World');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);
```

参考消息:如果你没有安装 NodeJS，可以跳过下一步。下一步将利用一个 Docker 映像，它附带了一个现成的节点环境。

如果您在本地 pc 上安装了 NodeJS，那么您可以尝试使用普通的 NodeJS 运行应用程序。

```
$ npm install
$ node server.js 

Running on http://0.0.0.0:8080
```

打开 [http://localhost:8080](http://localhost:8080/) ，你会看到你的 hello world 响应。

### 让我们找到一个基本的 Docker 映像来运行我们的应用程序

公众 [Docker Hub](https://hub.docker.com/) 是一个很好的来源。如果你搜索“节点”，你会很快找到一个已经被使用超过 10 亿次的 i [法师。](https://hub.docker.com/_/node)

集装箱需要从基础开始组装。我们从一个包含随时可用的 NodeJS 环境的基础映像开始。它通常构建在一个简单的 Linux 映像上。我们复制容器中所有需要的文件。

然后，我们执行命令，例如，获取所有需要的依赖项。最后一步是告诉容器在容器启动时运行什么命令。

```
# file: 'Dockerfile'

# lts-alpine means long term support and alpine is a very small Linux 
# distribution that is a lot smaller than the default one (node:lts).
# smaller images mean faster builds and startup time that is very handy 
# when it comes to scaling containers for production up and down
FROM node:lts-alpine

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
COPY package*.json ./

# Install all dependencies
RUN npm install

# Copy sources
COPY server.js server.js

CMD [ "node", "server.js" ]
```

现在，让我们构建图像:

```
docker build -t node-web-app .
```

我们可以通过以下方式运行 Docker 容器:

```
$ docker run --name my_container -p 8080:8080 node-web-app

Running on http://0.0.0.0:8080
```

在你的浏览器中打开 [http://localhost:8080](http://localhost:8080) ，你会看到 hello world 页面。这一次，它在一个容器中独立运行。

您甚至不需要 NodeJS 或任何其他东西来构建和运行这个容器。一切都被封装，由于 Docker 的性质，它以本机性能运行。

让我们停止这个可能仍在后台运行的容器:

```
$ docker rm -f my_container
```

*仅供参考:接近 100%的本机性能仅适用于 Linux 主机。对于 Mac OS 和 windows，需要一些转换和虚拟化，这会带来一些性能下降。对于发展来说，应该是可以的。最重要的是，生产服务器通常运行与 Docker 配合良好的原生 Linux。*

接下来，让我们在 Kubernetes 集群中使用之前构建的容器。在本教程中，我们将关注本地集群。如果你去遥远的地方，这是非常相似的。

在远程设置中，您还需要将您的映像推送到一个公共可用的注册中心，这允许您的远程集群访问该映像。

如果人们要求的话，我可能会再写一篇博文。

### 在 Kubernetes 中运行您的 web 应用程序

您的 Docker 已经集成了 Kubernetes。打开 Docker app，进入*设置- > Kubernetes* ，启用 Kubernetes。

应用更改可能需要一段时间。只要 Docker 应用程序底部栏中的 Kubernetes 状态为绿色，您就准备好了。

如果您有任何问题，请转到“故障诊断”(右上角的小错误图标)，然后按“重置为出厂默认值”。之后，Docker 应该会重启，你需要再次激活 Kubernetes。

让我们安装 kubectl，它是与您的 Kubernetes 集群交互的最重要的工具之一。按照这个指南安装它:[https://kubernetes.io/docs/tasks/tools/install-kubectl/](https://kubernetes.io/docs/tasks/tools/install-kubectl/)。

现在，我们可以检查一切是否设置正确:

```
$ kubectl get services

NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE
kubernetes ClusterIP 10.96.0.1 <none> 443/TCP 3m14s
```

让我们在集群中部署 Docker 容器:

```
# file 'application/deployment.yaml'

apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-web-app
spec:
  selector:
    matchLabels:
      app: node-web-app
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: node-web-app
    spec:
      containers:
        - name: node-web-app
          image: node-web-app

          # only use this to for local development
          # we never pushed our image to a remote registry
          # and by default Kubernetes pulls images
          # this property forces kubernetes to always use 
          # the local image that is not a good practice in production
          imagePullPolicy: Never
          ports:
            - containerPort: 8080 
```

deployment.yaml 是一个描述要做什么部署的文件。我们可以通过以下方式执行它:

```
$ kubectl apply -f application/deployment.yaml

deployment.apps/node-web-app created
```

并检查容器是否正在运行:

```
$ kubectl get pods

NAME READY STATUS RESTARTS AGE
node-web-app-6788cfd6cc-bcbb2 1/1 Running 0 3s
node-web-app-6788cfd6cc-t5t6w 1/1 Running 0 3s 
```

我们的 Kubernetes 管理一个包含单一主机的集群，这是我们的本地机器。在远程集群上，可能有数百个节点托管不同的部署。

它在我们的环境中部署了两个容器。这些容器在一个孤立的网络中运行。否则，不可能两次暴露同一个端口。

那么我们如何访问实际的容器呢？您可以通过定义所谓的服务来访问已部署的容器。每个公共应用程序都需要一个服务来定义公开的公共端口。

```
# file 'application/service.yaml'

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
      targetPort: 8080 
```

我们将容器端口 8080 映射到公共可用端口 80。该服务充当负载平衡器。它在容器之间分发请求。

让我们部署我们的服务:

```
$ kubectl apply -f ./application/service.yaml 

service/my-service-for-my-webapp created
```

我们可以检查我们的服务是否正在运行:

```
$ kubectl describe svc my-service-for-my-webapp

Name: my-service-for-my-webapp
Namespace: default
Labels: <none>
Annotations: Selector: app=my-example-app
Type: LoadBalancer
IP: 10.104.18.24
LoadBalancer Ingress: localhost
Port: <unset> 80/TCP
TargetPort: 8080/TCP
NodePort: <unset> 32114/TCP
Endpoints: 10.1.0.17:8080,10.1.0.18:8080
Session Affinity: None
External Traffic Policy: Cluster
Events: <none> 
```

输出非常具有描述性，并确认了我们想要实现的目标。它使用来自两个已部署容器(Kubernetes 中所谓的 pod)的端点。

现在，你可以打开 [http://localhost:80](http://localhost:80)

就是这样！您创建了一个 Docker 容器，并在 Kubernetes 集群中使用了它。这种设置非常强大，是当今许多可扩展产品和业务的基础。

## 收尾工作

让我们整理一下我们的实验空间:

```
$ kubectl delete -f ./application/service.yaml 

service "my-service-for-my-webapp" deleted

$ kubectl delete -f application/deployment.yaml

deployment.apps "node-web-app" deleted
```

为了保持我们设备的资源自由，我们还应该停止 Docker 的 Kubernetes 功能。

我希望你喜欢这个动手的例子。激励自己四处搜索，查看其他示例，部署容器，连接它们，并使用它们。

您将在未来学到许多很酷的特性，这些特性使您能够以一种轻松、可重用和可伸缩的方式将您的应用程序交付到产品中。

一如既往，我感谢任何反馈和意见。

我希望你喜欢这篇文章。如果你喜欢它，觉得需要一轮掌声，[在 Twitter 上关注我](https://twitter.com/sesigl)。我在易贝·克莱南泽根公司工作，这是全球最大的机密公司之一。顺便说一下，[我们正在招聘](https://jobs.ebayclassifiedsgroup.com/ebay-kleinanzeigen)！

码头快乐！

## 参考资料:

*   [https://en.wikipedia.org/wiki/Kubernetes](https://en.wikipedia.org/wiki/Kubernetes)
*   [https://en . Wikipedia . org/wiki/dock _(软件)](https://en.wikipedia.org/wiki/Docker_(software))
*   [https://labs.play-with-docker.com/](https://labs.play-with-docker.com/)
*   [https://labs.play-with-k8s.com/](https://labs.play-with-k8s.com/)
*   [https://nodejs . org/en/docs/guide/nodejs dock-web app/](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)