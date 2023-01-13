# 如何用 Docker 将现有的应用程序放入容器中

> 原文：<https://www.freecodecamp.org/news/how-to-shove-an-existing-application-into-containers-with-docker-6dcbd6152fe4/>

丹尼尔·牛顿

# 如何用 Docker 将现有的应用程序放入容器中

![1*mxgRaQctGgv6qSSjNrq1JQ](img/62d60a269a87176d92458343fa100e5d.png)

[Hong Kong](https://pixabay.com/en/panorama-urban-landscape-town-center-3327550/) by [Marci Marc](https://pixabay.com/en/users/MarciMarc105-6128323/)

我终于抽出时间来学习如何使用 Docker，虽然我不知道 Docker 是什么，也不知道它能做什么。这是我尝试使用 Docker 的第一篇文章，也可能是我开始一个新项目(无论如何是 Java 或 Kotlin)时参考的内容。

这将是一个简短的帖子，采用现有的项目(从我的其他帖子之一)并改变它，使它可以在容器内运行。该项目是一个带有 MongoDB 数据库和 ActiveMQ 消息队列的 Spring Boot 应用程序。所有这些部件都是集装箱化的主要原料。

完成本文中概述的步骤，就不需要在本地安装 MongoDB 和 ActiveMQ。只要安装 Docker，你就可以开始了。在我看来，这本身已经是一个胜利了。

这里有到[代码](https://github.com/lankydan/spring-boot-jms)和相应的[博客文章](https://lankydanblog.com/2017/06/18/using-jms-in-spring-boot/)的链接。帖子涵盖了关于代码的所有信息。

最后一个评论:对于这篇文章的内容，我假设你已经安装了 Docker。如果你没有，那么 Docker 已经帮你搞定了。在这里你可以找到[支持的平台](https://docs.docker.com/install/#supported-platforms)，它提供了关于如何为你的特定机器安装 Docker 的更多链接。Docker 提供的[方向和设置](https://docs.docker.com/get-started/)页面可能也有帮助。

### 转换 Spring 应用程序

首先，Spring Boot 应用程序。

这是项目中唯一包含我们代码的部分。剩下的都是从别人的储存库中下载的图片。要开始让这个应用程序在容器中运行，我们需要创建一个指定图像内容的`Dockerfile`:

这取了`openjdk:8-jdk-alpine`的基图。这是应用程序的良好起点。它添加了从应用程序代码构建的 Jar(将其命名为`app.jar`)并公开了容器间通信的端口。最后一行定义了在容器中运行映像时执行的命令。这就是启动 Spring 应用程序的原因。

要从`Dockerfile`构建映像，运行下面的命令(假设您已经构建了应用程序代码):

```
docker build -t spring-boot-jms-tutorial .
```

现在有一个图像命名为`spring-boot-jms-tutorial` ( `-t`让我们来定义名称)。现在可以使用它来创建一个容器，该容器执行打包到图像的 Jar 中的代码:

```
docker run --name application -p 4000:8080 spring-boot-jms-tutorial
```

这将创建并运行一个从`spring-boot-jms-tutorial`图像构建的容器。它将容器命名为`application`,`-p`属性允许本地机器的端口映射到容器内部的端口。要访问容器的端口`8080`,我们只需在自己的机器上使用端口`4000`。

如果我们停止了这个容器并想再次运行它，我们应该使用命令:

```
docker start application
```

其中`application`是我们之前创建的容器的名称。如果再次使用`docker run`,它将创建另一个新容器，而不是重用现有的容器。实际上，因为我们为容器提供了一个名称，所以运行前面的相同的`run`命令会导致一个错误。

现在 Spring 应用程序成功地运行在一个容器中，但是日志看起来不是很好。让我们快速浏览一下，这样我们就知道下一步需要做什么了。

MongoDB 连接失败:

```
Exception in monitor thread while connecting to server mongocontainer:27017 com.mongodb.MongoSocketException: mongocontainer: Name does not resolve at com.mongodb.ServerAddress.getSocketAddress(ServerAddress.java:188) ~[mongodb-driver-core-3.6.4.jar!/:na] at com.mongodb.connection.SocketStreamHelper.initialize(SocketStreamHelper.java:59) ~[mongodb-driver-core-3.6.4.jar!/:na] at com.mongodb.connection.SocketStream.open(SocketStream.java:57) ~[mongodb-driver-core-3.6.4.jar!/:na] at com.mongodb.connection.InternalStreamConnection.open(InternalStreamConnection.java:126) ~[mongodb-driver-core-3.6.4.jar!/:na] at com.mongodb.connection.DefaultServerMonitor$ServerMonitorRunnable.run(DefaultServerMonitor.java:114) ~[mongodb-driver-core-3.6.4.jar!/:na] at java.lang.Thread.run(Thread.java:748) [na:1.8.0_171] Caused by: java.net.UnknownHostException: mongocontainer: Name does not resolve at java.net.Inet4AddressImpl.lookupAllHostAddr(Native Method) ~[na:1.8.0_171] at java.net.InetAddress$2.lookupAllHostAddr(InetAddress.java:928) ~[na:1.8.0_171] at java.net.InetAddress.getAddressesFromNameService(InetAddress.java:1323) ~[na:1.8.0_171] at java.net.InetAddress.getAllByName0(InetAddress.java:1276) ~[na:1.8.0_171] at java.net.InetAddress.getAllByName(InetAddress.java:1192) ~[na:1.8.0_171] at java.net.InetAddress.getAllByName(InetAddress.java:1126) ~[na:1.8.0_171] at java.net.InetAddress.getByName(InetAddress.java:1076) ~[na:1.8.0_171] at com.mongodb.ServerAddress.getSocketAddress(ServerAddress.java:186) ~[mongodb-driver-core-3.6.4.jar!/:na] ... 5 common frames omitted
```

ActiveMQ 也不存在:

```
Could not refresh JMS Connection for destination 'OrderTransactionQueue' - retrying using FixedBackOff{interval=5000, currentAttempts=1, maxAttempts=unlimited}. Cause: Could not connect to broker URL: tcp://activemqcontainer:61616\. Reason: java.net.UnknownHostException: activemqcontainer
```

我们将在下一节中对这些进行分类，这样应用程序就可以完整地工作。

在我们开始研究 Mongo 和 ActiveMQ 之前，还有最后一件事。

作为运行`mvn install`的一部分，`dockerfile-maven-plugin`也可以用来帮助构建容器。我选择不使用它，因为我不能让它与`docker-compose`正常工作。下面是一个使用插件的快速示例:

这允许我们替换`Dockerfile`中的几行:

此处添加了一行，并更改了一个现有行。`JAR_FILE`参数替换了由插件从`pom.xml`注入的 Jar 的原始名称。进行这些更改并运行`mvn install`，然后砰，你的容器就用所有需要的代码构建好了。

### 使用 MongoDB 映像

有一个 MongoDB 映像已经准备好，等待我们使用。它被理想地命名为`mongo`……你还期望什么？我们需要做的就是`run`图像并给它的容器命名:

```
docker run -d --name mongocontainer mongo
```

添加`-d`将在后台运行容器。容器的名称不仅仅是为了方便，因为 Spring 应用程序稍后将需要它来连接到 Mongo。

### 到 ActiveMQ 映像上

设置 ActiveMQ 就像 Mongo 一样简单。运行下面的命令:

```
docker run -d --name activemqcontainer -p 8161:8161 rmohr/activemq
```

这里的`8161`端口从容器映射到运行它的机器，允许从容器外部访问管理控制台。

### 把这一切联系在一起

如果你在阅读这篇文章时已经运行了所有这些命令，你会注意到`application`容器实际上并没有看到`mongocontainer`和`activemqcontainer`。这是因为它们不在同一个网络中运行。让他们交流并不困难，只需要一些额外的步骤。

默认情况下，Docker 创建一个桥接网络时，没有任何额外的配置。下面是如何做到这一点:

```
docker network create network
```

既然已经创建了网络(名为`network`)，那么需要修改之前运行的命令来创建连接到网络的容器。下面是前几节中用于创建容器的 3 个命令，每个命令都被修改以加入网络。

```
docker run -d --name mongocontainer --network=network mongodocker run -d --name activemqcontainer -p 8161:8161 --network=network rmohr/activemqdocker run --name application -p 4000:8080 --network=network spring-boot-jms-tutorial
```

一旦这些都运行了，整个应用程序就可以工作了。每个容器都可以看到对方。允许`application`容器连接到各自容器中的 MongoDB 和 ActiveMQ。

此时，一切正常。它的运行方式和我记忆中在我自己的笔记本电脑上设置好所有东西时一样。但是，这一次，除了 Docker 之外，没有在本地设置任何东西！

### 码头工人在编故事

我们本来可以就此打住，但是下一部分将使运行一切变得更加容易。Docker Compose 允许我们有效地将之前运行的所有命令放在一起，并在一个命令中启动所有容器及其网络。还有更多的设置，但最终，我认为输入的数量实际上减少了。

为此，我们需要创建一个`docker-compose.yml`文件:

与此版本的`Dockerfile`一起使用:

这几乎是所有需要做的事情。

是根据项目代码构建的 Spring 应用程序容器。容器的`build`属性告诉 Docker 基于在项目根目录中找到的项目`Dockerfile`构建图像。它将`JAR_FILE`参数传递给`Dockerfile`，将一些配置转移到这个文件中。

其他两个容器不需要太多设置。与前面的命令一样，它们的构建映像是指定的，`activemqcontainer`在其映射的端口周围添加配置。

最后一项配置在后台进行。创建一个网络，并将所有容器添加到其中。这消除了手动创建网络的需要。

剩下要做的就是运行`up`命令:

```
docker-compose up
```

这将构建并运行所有的容器。如果需要，构建应用程序代码映像。运行这个确切的命令会将所有容器日志输出到控制台，为此在后台添加`-d`标志:

```
docker-compose up -d
```

这样做之后，我们可以看看创建的容器和网络。跑步:

```
docker ps -a --format "table {{.Image}}\t{{.Names}}"
```

向我们展示了:

```
IMAGE                          NAMESmongo                          spring-boot-jms_mongocontainer_1spring-boot-jms_appcontainer   spring-boot-jms_appcontainer_1rmohr/activemq                 spring-boot-jms_activemqcontainer_1
```

和网络:

```
docker network ls
```

生产:

```
NETWORK ID       NAME                      DRIVER            SCOPE163edcfe5ada     spring-boot-jms_default   bridge            local
```

容器和网络的名称以项目名称为前缀。

### 结论

这就是它的全部内容…总之是一个简单的设置。我相信码头神还能做更多的事情，但我还不是其中之一。

最后，我们将我编写的一个现有应用程序放在一台机器上本地运行，并将所有内容都放入几个容器中。这意味着我们从一台需要设置好一切(安装了 MongoDB 和 ActiveMQ)的机器变成了一台可以通过使用容器和只需要安装 Docker 来跳过这一切的机器。Docker 然后为我们管理所有的依赖项。

我们研究了如何将应用程序的每个部分移动到一个容器中，然后用 Docker Compose 将它们捆绑在一起。最后，我们只需要一个命令，就可以从一无所有到运行应用程序所需的一切。

这篇文章中使用的代码可以在我的 [GitHub](https://github.com/lankydan/spring-boot-jms-docker) 上找到。

如果你觉得这篇文章很有帮助，你可以在 Twitter 上关注我，地址是 [@LankyDanDev](http://www.twitter.com/LankyDanDev) 来了解我的新文章。

我帖子中的观点和看法是我自己的，不代表埃森哲在任何问题上的观点。[查看丹·牛顿的所有帖子](https://lankydanblog.com/author/danknewton/)

*最初发布于 2018 年 9 月 2 日[lankydanblog.com](https://lankydanblog.com/2018/09/02/using-docker-to-shove-an-existing-application-into-some-containers/)。*