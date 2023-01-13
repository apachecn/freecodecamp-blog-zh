# 如何删除所有 Docker 图像——Docker 清理指南

> 原文：<https://www.freecodecamp.org/news/how-to-remove-all-docker-images-a-docker-cleanup-guide/>

在今天的技术世界里，容器无处不在。容器管理最流行的技术是 [Docker](https://www.docker.com/) 。它使使用容器变得容易，并帮助您轻松启动和运行应用程序。

不幸的是，这可能会占用大量磁盘空间，最终会导致磁盘满。

在设备或服务器上使用 Docker 并不重要。本指南向您展示了如何分析已用磁盘空间和清理不同的 Docker 资源。

你所需要的只是一个正在运行的 Docker 守护进程和一个终端。

## 如何分析 Docker 使用了多少空间

您可以通过运行以下命令来查看使用了多少空间:

```
$ docker system df

TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          61        16        21.1GB    15.25GB (72%)
Containers      69        0         12.26MB   12.26MB (100%)
Local Volumes   3         2         539.1MB   50.04MB (9%)
Build Cache     76        0         1.242GB   1.242GB
```

您可以通过使用详细选项`-v`获得更多信息:

```
$ docker system df -v

REPOSITORY        TAG   IMAGE ID     CREATED       SIZE      SHARED 
teamatldocker/jira    e50b8390945c   4 weeks ago     842.3MB   0B       
vw                    ed9e125a8925   2 months ago    1.659GB   134.8MB 

Containers space usage:

CONTAINER ID   IMAGE                    COMMAND                   SIZE 
94e03a4a17d0   teamatldocker/jira       "/sbin/tini -- /usr/…"    1.4MB 

Local Volumes space usage:

VOLUME NAME                     LINKS     SIZE
play-with-jira_postgresqldata   1         84.19MB   
play-with-jira_jiradata         1         404.8MB

Build cache usage: 1.242GB

CACHE ID       CACHE TYPE     SIZE      CREATED        LAST USED 
oxil5sdicb91   regular        135MB     2 months ago   2 months ago  
kxz13fmdbodg   regular        13B       2 months ago   2 months ago 
nysus21ej7pf   regular        0B        2 months ago   2 months ago
```

如您所见，您可以获得以下信息:

*   图像空间使用，
*   容器空间使用，
*   本地卷空间使用情况，以及
*   构建缓存使用。

## 如何清理 Docker 中的所有内容

您可以清理 Docker 中的所有内容或特定资源，如图像、容器卷或构建缓存。

要尽可能清除正在使用的组件，请运行以下命令:

```
$ docker system prune -a
```

`-a`包括未使用的和悬空的容器。不提供`-a`只会删除悬挂的图像，即与任何其他图像都没有关系的未标记图像。

如果您想清理大多数 Docker 资源，但仍保留标记的图像，您可以执行以下命令:

```
$ docker system prune
```

这就是快速释放磁盘空间所需的全部内容。此外，您可以单独清理组件。

以下是一些更有用的命令:

### 清理未使用的和悬挂的图像

```
$ docker image prune
```

### 仅清理悬挂的图像

```
$ docker image prune -a
```

### 清理停止的容器

```
$ docker container prune
```

### 清理未使用的卷

```
$ docker volume prune
```

## 如何持续有效地管理您使用的 Docker 空间

你可以每天或者在启动时运行一些东西。要跳过通常的提示，您需要将`-f`添加到您想要自动运行的命令中。

请记住，这将导致您更频繁地下载图像，因为您经常删除 Docker 资源。

如果您没有磁盘空间问题，那么不要担心。只要当大量的 Docker 磁盘占用引起你的注意时，就清理一下。

## 结论

如今，使用`docker`命令清理 Docker 磁盘空间有很多种方法。如果您想定期清理 Docker 资源，您甚至可以自动执行这些命令。

我希望你喜欢这篇文章。

如果你喜欢它，觉得有必要给我一点掌声，或者只是想和我保持联系，[在 Twitter 上关注我](https://twitter.com/sesigl)。

我在易贝·克莱南泽根公司工作，这是世界上最大的机密公司之一。顺便说一下，[我们正在招聘](https://jobs.ebayclassifiedsgroup.com/ebay-kleinanzeigen)！