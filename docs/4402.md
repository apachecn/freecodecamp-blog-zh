# Docker 101 -如何从创建到部署

> 原文：<https://www.freecodecamp.org/news/docker-101-creation-to-deployment/>

Docker 是游戏规则的改变者，它极大地改变了应用程序开发的世界。了解今天使用这种容器技术所需的重要技能。

## Docker 是什么？

简单来说， **Docker** 是一个让开发者在容器中创建、部署和运行应用程序的工具。 ***容器化*** 就是使用 Linux 容器来部署应用程序。

那么，为什么 Docker 如此伟大，为什么我们作为开发人员还要费心学习它呢？

| 理由 | 说明 |
| --- | --- |
| 灵活的 | 即使是最复杂的应用程序也可以容器化。 |
| 轻量级选手 | 容器利用并共享主机内核。 |
| 可互换的 | 您可以即时部署更新和升级。 |
| 轻便的 | 您可以在本地构建，部署到云中，并在任何地方运行。 |
| 可攀登的 | 您可以增加并自动分发容器副本。 |
| 可叠起堆放的 | 您可以垂直和动态地堆叠服务。 |

现在我们知道了为什么 Docker 如此重要，让我们在本地机器上安装它。

在 [Docker Hub](https://hub.docker.com/signup) 上注册一个帐户，并下载免费的 Docker 桌面应用程序。

## Docker 与传统虚拟机有何不同？

容器在 Linux 上本地运行，并与其他容器共享主机的内核。它作为一个独立的进程运行，占用的内存不比任何其他可执行文件多，这意味着它非常轻量级。

相比之下，虚拟机(VM)运行成熟的“来宾”操作系统，通过虚拟机管理程序对主机资源进行虚拟访问。一般来说，虚拟机提供的环境资源比大多数应用程序需要的多。

当使用 Docker 时，一个“Dockerfile”定义了你的容器内部的环境中发生了什么。对网络接口和磁盘驱动器等资源的访问是在此环境中虚拟化的，与系统的其余部分隔离开来。这意味着您需要将端口映射到外部世界，并明确您想要将哪些文件“复制”到该环境中。然而，在这样做之后，您可以预期在这个“Dockerfile”中定义的应用程序的构建无论在哪里运行都完全相同。

## Docker 命令

要测试您是否拥有 Docker 的运行版本，请运行以下命令:

`docker --version`

要测试您的安装是否工作正常，请尝试运行简单的 Docker **hello-world** 映像:

`docker run hello-world`

如果所有设置都正确，您应该会看到类似如下的输出:

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
ca4f61b1923c: Pull complete
Digest: sha256:ca0eeb6fb05351dfc8759c20733c91def84cb8007aa89a5bf606bc8b315b9fc7
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
... 
```

要查看下载到您计算机上的 **hello-world** Docker 映像，请使用 Docker 映像列表命令:

`docker image ls`

厉害！您已经开始使用 Docker 开发容器化的应用程序。以下是一些有用的基本 Docker 命令:

```
## List Docker CLI commands
docker
docker container --help

## Display Docker version and info
docker --version
docker version
docker info

## Execute Docker image
docker run hello-world

## List Docker images
docker image ls

## List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls --all
docker container ls -aq 
```

> 集装箱化使 CI/CD 无缝连接。比如:
> 
> *   The application has no system dependency.
> *   Updates can be pushed to any part of the distributed application.
> *   Resource density can be optimized. With Docker, expanding your application means rotating new executable files instead of running a heavy virtual machine host.

## 让我们使用 Docker 构建一个 Node.js web 应用程序

我们做的第一件事是创建一个`package.json`文件。我们可以通过简单地运行以下命令来快速完成这项工作:

```
npm init -y 
```

这将创建上面的文件，其中某些基本字段已经填充或留空。

您的文件应该如下所示:

```
{
  "name": "app-name",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
} 
```

接下来，我们安装`express.js`，根据[官网](http://expressjs.com/)的说法，这是一个“**快速、非个性化、极简的 node . js**web 框架”。

我们通过在终端中运行以下命令来实现这一点:

```
npm install express --save 
```

上面的命令将`express.js`框架添加到我们的应用程序中，用 **- save** 标志作为应用程序使用`express.js`作为依赖项的指令。

现在，进入您的`package.json`，将`"main": "index.js"`键值对更改如下:

```
"main": "app.js" 
```

接下来，使用以下命令创建一个`.gitignore`文件:

```
touch .gitignore 
```

然后添加下面一行:

```
node_modules/ 
```

> 这可以防止对`node.js`开发至关重要的 **node_modules** 文件夹被`git`跟踪。

现在将以下代码添加到`app.js`文件中:

```
const express = require('express');

const app = express();

const PORT = 8080;
const HOST = '0.0.0.0';

app.get('/', (req, res) => {
  res.send(
    `
    <h1>Home</h1>
    <p>Docker is awesome!</p>
    <a href="/more" alt="Next Page">Next Page</a>
    `
  )
});

app.get('/more', (req, res) => {
  res.send(
    `
    <h1>Page Two</h1>
    <p>Node.js is pretty great too!!</p>
    <a href="/" alt="Back Home">Back Home</a>
    `
  )
});

app.listen(PORT, HOST);
console.log(`Running on https://${HOST}:${PORT}`); 
```

要在本地计算机上运行此程序，请在应用程序文件夹中运行以下命令:

```
npm start 
```

> 您会发现应用程序在`http://0.0.0.0:8080/`运行

### 厉害！

![Congratulations](img/3918a6d8a31419baaa9b86118630847e.png)
*祝贺你走到这一步*

* * *

## 进码头

现在用下面的命令创建一个`Dockerfile`:

```
touch Dockerfile 
```

然后添加以下代码:

```
# An official Docker image for Node.js
FROM node:10-alpine

# Working directory for the containerised application
WORKDIR /src/app

# This copies significant package.json files to the current directory
COPY package*.json ./
# Install essential Node.js dependencies
RUN npm install

COPY . .

# Opens up this port on the Docker container
EXPOSE 8080

# This starts the Docker application
CMD [ "npm", "start" ] 
```

上面的注释试图解释每个`Dockerfile`命令的作用。

另外，添加一个`dockerignore`文件来防止应用程序的某些组件的容器化。

将它放在`dockerignore`文件中:

```
node_modules
npm-debug.log
Dockerfile*
docker-compose*
.dockerignore
.git
.gitignore
README.md
LICENSE 
```

## 如何部署

`<image-name>`是您分配给 Docker 应用程序的名称，`<tag>`本质上只是 Docker 映像的版本指示器。

*   `docker build -t image-name:tag .`

运行此程序，从您的终端访问您的 Docker 帐户。

*   `docker login`

在 Docker Hub 上创建一个存储库。

标签`<image>`上传到注册表。

*   `docker tag <image-name> username/repository:tag`

将标记的图像上传到注册表。

*   `docker push username/repository:tag`

通过连接端口，在本地机器上运行部署的 Docker 容器。将暴露的 8080 端口作为目标，并将其分配给机器上的端口 10203。

*   `docker run -p 10203:8080 username/repository:tag`

* * *

#### 就是这样！您已经构建并部署了一个容器化的 Node.js web 应用程序。

以上所有代码都可以在[这个 Github 库](https://github.com/Usheninte/docker-101)中找到。

> 原贴[此处](https://blog.ninte.dev/docker-101-creation-to-deployment-cjzylgqnc0019eus1s10lg39r)在 [**blog.ninte.dev**](https://blog.ninte.dev)