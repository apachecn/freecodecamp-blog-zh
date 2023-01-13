# Docker 分离模式解释

> 原文：<https://www.freecodecamp.org/news/docker-detached-mode-explained/>

## **对接器分离模式**

由选项`--detach`或`-d`显示的分离模式意味着 Docker 容器在你的终端后台运行。它不接收输入或显示输出。

```
docker run -d IMAGE
```

如果您在后台运行容器，您可以使用`docker ps`找出它们的细节，然后将您的终端重新连接到它的输入和输出。

#### **更多信息:**

*   [连接到运行中的容器和从运行中的容器分离| Docker Docs](https://docs.docker.com/engine/reference/commandline/attach/#examples)
*   [分离 vs 前台| Docker 文档](https://docs.docker.com/engine/reference/run/#detached-vs-foreground)

## 关于 Docker 的更多信息

Docker 是一个构建、发布和运行分布式应用程序的开放平台。这是用围棋写的。它于 2013 年首次发布，由 Docker，Inc .开发。

Docker 用于运行名为“容器”的包。容器相互隔离，并与操作系统隔离。这些比虚拟机更轻量级，因为它们不使用主机来运行操作系统。

容器化是一种部署和运行应用程序的方式，它运行独立的服务，这些服务在 Linux 内核上本地运行。可以在 Docker 中为每个容器手动设置内存。

Docker 用于简化配置，并确保平稳持续的集成和部署流程。可以为开发、登台和生产环境指定特定的容器。根据 Docker 手册，容器在生产中的真正实现是作为服务运行，使用`docker-compose.yml`文件进行设置。这是一个 YAML 文件，它定义了 Docker 容器在生产中的行为。

Docker 最大的一个优点是，它可以被使用不同操作系统的团队用来构建项目，而无需担心软件冲突。

### **安装**

*   Ubuntu: `sudo apt install docker`
*   红帽:`yum install docker-ce`
*   Windows / macOS: [下载](https://www.docker.com/get-started)
*   Linux:

```
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```

#### **更多信息:**

*   下载和文档请查看 docker 官方网站: [Docker 官方网站](https://www.docker.com/)