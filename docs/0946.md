# 如何 SSH 到 Docker 容器——安全 Shell 与 Docker Attach

> 原文：<https://www.freecodecamp.org/news/how-to-ssh-into-a-docker-container-secure-shell-vs-docker-attach/>

容器是今天运行应用程序的基础。而最流行的容器技术叫做 [Docker](https://www.docker.com/) 。

了解如何 SSH 到容器对于在本地操作系统或远程设置上使用、调试和操作容器是至关重要的。

本文将描述实现这一点的不同方法及其限制。

## 方法 1–使用`docker exec`连接到运行中的容器

在容器中获取 shell 最常见和最有用的命令是`docker exec -it`。它在容器中运行一个新命令，允许您启动一个新的交互式 shell:

```
# start a container
$ docker run --name nginx --rm -p 8080:80  -d nginx

# create and connect to a bash shell in the container
$ docker exec -it nginx bash

root@a84ad71521b1:/#
```

您可以通过按`control + d`或键入`exit`退出当前 shell。

它之所以有效是因为:

*   `docker exec`运行新命令，
*   `-i`添加一个标准输入流，
*   `-t`添加终端驱动程序，
*   `-it`结合了`-i`和`-t`，允许您与流程进行交互。

‌‌It 可能发生在您的容器没有安装 bash 的情况下。如果您不能启动 bash，您可以尝试启动一个`sh` -shell:

```
docker exec -it nginx sh
```

‌‌Also，集装箱里可能没有装炮弹。因此，在普通的 docker Universum 中，没有办法在那个容器中获得一个 shell，也不能启动任何 shell。‌‌

根据您的容器编排工具，您可能仍然能够访问容器。例如，[distroles 容器](https://github.com/GoogleContainerTools/distroless)越来越受欢迎，以减少容器的占用空间。如果你使用 Kubernetes，一个[临时容器](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/)特性允许你调试没有外壳的容器以及崩溃的容器。

## 方法 2–使用`docker attach`连接到运行中的容器

> `docker attach`命令使用容器的 id 或名称将终端的标准输入、输出和错误附加到正在运行的容器上。(消息来源[docker.com](https://docs.docker.com/engine/reference/commandline/attach/))

实际上，这意味着您输入的所有内容都将被转发到容器，并且打印的所有内容都将显示在您的控制台上。

现在，您将连接到运行的容器:

```
$ docker attach nginx                              

172.17.0.1 - - [18/Mar/2022:08:37:14 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/97.0.4692.71 Safari/537.36" "-" 
```

运行这个的时候可以打开 [http://localhost:8080](http://localhost:8080) 。因为容器在打开的每个页面上打印访问日志，所以您将在终端上看到输出。

此外，输入也被转发到容器。因此，如果你按下`control + c`，这将触发一个杀死信号，你的容器将关闭。

```
$ docker attach nginx                              
172.17.0.1 - - [18/Mar/2022:08:37:14 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/97.0.4692.71 Safari/537.36" "-"
^[[A^C2022/03/18 08:39:50 [notice] 1#1: signal 2 (SIGINT) received, exiting
2022/03/18 08:39:50 [notice] 32#32: exiting 
```

分离可能很棘手，因为`control + c`也用于分离。为了能够在不停止容器的情况下分离，可以通过禁用转发信号来连接到容器:

```
# start the container again
docker run --name nginx --rm -p 8080:80  -d nginx

# attach with proxying signals
docker attach --sig-proxy=false nginx
```

## 结论

要使用普通 docker 命令连接到容器，可以使用`docker exec`和`docker attach`。

更受欢迎，因为您可以运行一个新命令来生成一个新的 shell。您可以像在本地环境中一样检查流程、文件和操作。

`docker attach`更受限制，因为它将终端的标准输入、输出和错误附加到正在运行的容器的主进程中。

我希望你喜欢这篇文章。

如果你喜欢它，觉得有必要给我一点掌声，或者只是想和我保持联系，[在 Twitter 上关注我](https://twitter.com/sesigl)。

我在易贝·克莱南泽根公司工作，这是世界上最大的机密公司之一。顺便说一下，[我们正在招聘](https://jobs.ebayclassifiedsgroup.com/ebay-kleinanzeigen)！

### 参考

*   [如何 SSH 到正在运行的 Docker 容器并运行命令](https://phoenixnap.com/kb/how-to-ssh-into-docker-container)
*   [如何在不停止 Docker 容器的情况下将其拆下](https://www.cloudsavvyit.com/14226/how-to-detach-from-a-docker-container-without-stopping-it/#:~:text=Docker%20supports%20a%20keyboard%20combination,alive%2C%20keeping%20your%20container%20running.)
*   [码头工人附上文件](https://docs.docker.com/engine/reference/commandline/attach/)
*   码头执行文件