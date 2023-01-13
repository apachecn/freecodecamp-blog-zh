# Docker 数据容器

> 原文：<https://www.freecodecamp.org/news/docker-data-containers/>

在 Docker 容器中管理数据的方法不止一种。向数据容器问好。

简单地说，数据容器的工作就是存储/管理数据。

与其他容器类似，它们由主机系统管理。然而，当您执行`docker ps`命令时，它们不会出现。

为了创建数据容器，我们首先创建一个具有众所周知的名称的容器，以供将来参考。我们使用 *busybox* 作为基础，因为它体积小，重量轻，以防我们想要探索和移动容器到另一个主机。

在创建容器时，我们还提供了一个卷`-v` 选项来定义其他容器将在哪里读/写数据。

```
$ docker create -v /config --name dataContainer busybox
```

容器就位后，我们现在可以将文件从本地客户机目录复制到容器中。

使用命令`docker cp`将文件复制到容器中。下面的命令将把 *config.conf* 文件复制到 *dataContainer* 的 *config* 目录中。

```
$ docker cp config.conf dataContainer:/config/
```

现在我们的数据容器有了我们的配置，我们可以在启动需要配置文件的依赖容器时引用容器。

通过使用神奇的`--volumes-from <container>`选项，我们可以使用正在启动的容器中其他容器的挂载卷。在这种情况下，我们将启动一个引用我们的数据容器的 Ubuntu 容器。当我们列出配置目录时，它将显示附加容器中的文件。

```
$ docker run --volumes-from dataContainer ubuntu ls/config
```

如果一个 */config* 目录已经存在，那么 volumes-from 将覆盖并成为所使用的目录。您可以将多个卷映射到一个容器。

* * *

### **进出口集装箱数据**

可以从容器中导入和导出数据，使用`docker export` 命令。

我们可以简单地通过将数据容器导出到一个. tar 文件来将它移动到另一台机器上。

```
$ docker export dataContainer > dataContainer.tar
```

同样，我们可以将数据容器重新导入 Docker。

```
$ docker import dataContainer.tar
```