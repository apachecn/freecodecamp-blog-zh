# Docker 缓存–如何进行干净的映像重建并清除 Docker 的缓存

> 原文：<https://www.freecodecamp.org/news/docker-cache-tutorial/>

容器使您能够以一种可移植的方式打包您的应用程序，这种方式可以在许多环境中运行。最受欢迎的容器平台是 [Docker](https://www.docker.com/) 。

本教程将解释如何使用 Docker 构建缓存为您带来优势。

## Docker 构建缓存

构建图像应该快速、高效、可靠。Docker 图像的概念来自不可变的层。您执行的每个命令都会生成一个新层，其中包含与前一层相比所做的更改。

所有先前构建的层都将被缓存，并可重复使用。但是，如果您的安装依赖于外部资源，Docker 缓存可能会导致问题。

## 如何利用 Docker 构建缓存

为了理解 Docker 构建缓存问题，让我们构建一个简单的定制 [nginx](https://nginx.org/en/) Docker 应用程序。在构建映像之前，创建一个 Dockerfile 文件来更新库并添加一个自定义起始页:

```
FROM nginx:1.21.6

# Update all packages
RUN apt-get update && apt-get -y upgrade

# Use a custom startpage
RUN echo '<html><bod>My Custom Startpage</body></html>' > /usr/share/nginx/html/index.html
```

您现在可以构建 Docker 映像了:

```
$  docker build -t my-custom-nginx .

=> [1/3] FROM docker.io/library/nginx:1.21.6@sha256:e12...  5.8s
=> [2/3] RUN apt-get update && apt-get -y upgrade           3.6s
=> [3/3] RUN echo '<html><bod>My Custom Startpage...        0.2s

=> exporting to image                                       0.1s
=> exporting layers                                         0.1s
=> writing image                                            0.0s
=> naming to docker.io/library/my-custom-nginx

[+] Building 11.3s (7/7) FINISHED
```

在这个例子中，为了可读性，我删除了一些输出。如果您第一次构建映像，您会发现这需要相当长的时间，在我的例子中就是`11.3s`。

一个很长的执行步骤是`apt-get update && apt-get -y upgrade`取决于有多少依赖项被更新和你的网速有多快。它检查操作系统上的软件包更新，如果有，就安装它们。

现在，您再次执行它，并从 Docker 构建缓存中受益:

```
$ docker build -t my-custom-nginx .

=> [1/3] FROM docker.io/library/nginx:1.21.6@sha256:e1211ac1…   0.0s
=> CACHED [2/3] RUN apt-get update && apt-get -y upgrade        0.0s
=> CACHED [3/3] RUN echo '<html><bod>My Custom Startpage...     0.0s

=> exporting to image                                           0.0s
=> exporting layers                                             0.0s
=> writing image                                                0.0s
=> naming to docker.io/library/my-custom-nginx

Building 1.1s (7/7) FINISHED
```

这一次，映像构建非常快，因为它可以重用所有以前构建的映像。当您在 docker 文件中自定义起始页时，您会看到缓存行为是如何受到影响的:

```
FROM nginx:1.21.6

# Update all packages
RUN apt-get update && apt-get -y upgrade

# Use a custom startpage
RUN echo '<html><bod>New Startpage</body></html>' > /usr/share/nginx/html/index.html
```

现在，再次构建图像:

```
$ docker build -t my-custom-nginx .

=> [1/3] FROM docker.io/library/nginx:1.21.6@sha256:e1211ac1…   0.0s
=> CACHED [2/3] RUN apt-get update && apt-get -y upgrade        0.0s
=> [3/3] RUN echo '<html><bod>My Custom Startpage...            0.2s

=> exporting to image                                           0.0s
=> exporting layers                                             0.0s
=> writing image                                                0.0s
=> naming to docker.io/library/my-custom-nginx

Building 2.1s (7/7) FINISHED
```

这次它只重建了最后一层，因为它发现`RUN`命令已经改变。但是，它重复使用了密集的第二个构建步骤，并且没有更新操作系统依赖性。

缓存行为是智能的。一旦 1 个步骤需要重新构建，每个后续步骤都要重新构建。因此，最好将经常变化的部分放在`Dockerfile`的末尾，以便重用之前的构建层。

不过，您可能想要强制重新构建缓存图层以强制进行包更新。强制重建可能是必要的，因为您希望保持应用程序的安全，并使用最新的可用更新。

## 如何使用 Docker Build `--no-cache`选项

禁用构建缓存可能有不同的原因。通过使用`--no-cache`选项，您可以从基础图像重建图像，而无需使用缓存层。

```
$ docker build -t my-custom-nginx .

=> CACHED [1/3] FROM docker.io/library/nginx:1.21.6@sha256:...  0.0s
=> [2/3] RUN apt-get update && apt-get -y upgrade               3.5s
=> [3/3] RUN echo '<html><bod>My Custom Startpage...            0.2s

=> exporting to image                                           0.1s
=> exporting layers                                             0.0s
=> writing image                                                0.0s
=> naming to docker.io/library/my-custom-nginx

Building 5.5s (7/7) FINISHED
```

建造并使用了新的层。这次`docker build`运行两个命令，这是一个全有或全无的方法。要么提供执行所有命令的`--no-cache`选项，要么尽可能多地缓存。

## 如何使用 Docker 参数破坏缓存

另一个选项允许在 docker 文件中提供一个小的起点。您需要像这样编辑 docker 文件:

```
FROM nginx:1.21.6

# Update all packages
RUN apt-get update && apt-get -y upgrade

# Custom cache invalidation
ARG CACHEBUST=1

# Use a custom startpage
RUN echo '<html><bod>New Startpage</body></html>' > /usr/share/nginx/html/index.html
```

您在 docker 文件中想要实施重建的位置添加一个`CACHEBUST`参数。现在，您可以构建 Docker 映像并提供一个始终不同的值，该值会导致以下所有命令重新运行:

```
$ docker build -t my-custom-nginx --build-arg CACHEBUST=$(date +%s) .

=> [1/3] FROM docker.io/library/nginx:1.21.6@sha256:e1211ac1...    0.0s
=> CACHED [2/3] RUN apt-get update && apt-get -y upgrade           0.0s
=> [3/3] RUN echo '<html><bod>My Custom Startpage...               0.3s

=> exporting to image                                              0.0s
=> exporting layers                                                0.0s
=> writing image                                                   0.0s
=> naming to docker.io/library/my-custom-nginx

Building 1.0s (7/7) FINISHED
```

通过提供`--build-arg CACHEBUST=$(date +%s)`，您将参数设置为一个始终不同的值，这将导致所有后续层重建。

## 摘要

Docker 的构建缓存是一个方便的特性。由于重用了以前创建的层，它加快了 Docker 构建的速度。

您可以使用`--no-cache`选项来禁用缓存，或者使用定制的 Docker build 参数来强制从某个步骤开始重建。

了解 Docker 构建缓存是非常有用的，它将使您在构建 Docker 容器时更加高效。

我希望你喜欢这篇文章。

如果你喜欢它，觉得有必要给我一点掌声，或者只是想保持联系，[在 Twitter 上关注我](https://twitter.com/sesigl)。

我在易贝·克莱南泽根公司工作，这是世界上最大的机密公司之一。顺便说一下，[我们正在招聘](http://ebay-kleinanzeigen.de/careers)！

## 参考

*   码头 arg 指令
*   [Stackoverflow:禁用特定运行命令的缓存](https://stackoverflow.com/questions/35134713/disable-cache-for-specific-run-commands)
*   [bael dung:Docker 构建缓存如何工作以及何时不使用它](https://www.baeldung.com/linux/docker-build-cache)