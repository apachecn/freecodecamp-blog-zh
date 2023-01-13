# Docker 图像存储在哪里？Docker 容器路径解释

> 原文：<https://www.freecodecamp.org/news/where-are-docker-images-stored-docker-container-paths-explained/>

Docker 已被广泛采用，用于在生产中运行和扩展应用程序。此外，它还可以通过执行一个 Docker 命令来快速启动应用程序。

公司也在投入越来越多的精力来改进本地和远程 Docker 容器的开发，这也带来了很多好处。

您可以通过执行以下命令获得关于 Docker 配置的基本信息:

```
$ docker info

...
 Storage Driver: overlay2
 Docker Root Dir: /var/lib/docker
... 
```

输出包含关于您的存储驱动程序和 docker 根目录的信息。

## Docker 图像和容器的存储位置

Docker 容器由网络设置、宗卷和映像组成。Docker 文件的位置取决于您的操作系统。以下是最常用操作系统的概述:

*   Ubuntu: `/var/lib/docker/`
*   Fedora: `/var/lib/docker/`
*   Debian: `/var/lib/docker/`
*   视窗:`C:\ProgramData\DockerDesktop`
*   苹果电脑:`~/Library/Containers/com.docker.docker/Data/vms/0/`

在 macOS 和 Windows 中，Docker 在虚拟环境中运行 Linux 容器。因此，有一些额外的事情要知道。

### Mac 坞站

Docker 本身与 macOS 不兼容，因此使用 [Hyperkit](https://github.com/moby/hyperkit) 来运行虚拟映像。其虚拟图像数据位于:

`~/Library/Containers/com.docker.docker/Data/vms/0`

在虚拟映像中，该路径是默认的 Docker 路径`/var/lib/docker`。

您可以通过在虚拟环境中创建一个 shell 来调查 Docker 根目录:

```
$ screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty 
```

你可以通过按下 **Ctrl+a** ，然后按下 **k** 和 **y** 来杀死这个会话。

### Windows

在 Windows 上，Docker 有点小问题。有一些本机 Windows 容器的工作方式类似于 Linux 容器。Linux 容器运行在基于 Hyper-V 的最小虚拟环境中。

执行 linux 映像的配置和虚拟映像保存在默认的 Docker 根文件夹中。

`C:\ProgramData\DockerDesktop`

如果您检查常规映像，那么您将得到如下 linux 路径:

```
$ docker inspect nginx

...
"UpperDir": "/var/lib/docker/overlay2/585...9eb/diff"
... 
```

您可以通过以下方式连接到虚拟映像:

```
docker run -it --privileged --pid=host debian nsenter -t 1 -m -u -i sh
```

在那里，您可以转到引用的位置:

```
$ cd /var/lib/docker/overlay2/585...9eb/
$ ls -lah

drwx------    4 root     root        4.0K Feb  6 06:56 .
drwx------   13 root     root        4.0K Feb  6 09:17 ..
drwxr-xr-x    3 root     root        4.0K Feb  6 06:56 diff
-rw-r--r--    1 root     root          26 Feb  6 06:56 link
-rw-r--r--    1 root     root          57 Feb  6 06:56 lower
drwx------    2 root     root        4.0K Feb  6 06:56 work
```

## Docker 根文件夹的内部结构

在`/var/lib/docker`内部，存储着不同的信息。例如，容器、卷、构建、网络和集群的数据。

```
$ ls -la /var/lib/docker

total 152
drwx--x--x   15 root     root          4096 Feb  1 13:09 .
drwxr-xr-x   13 root     root          4096 Aug  1  2019 ..
drwx------    2 root     root          4096 May 20  2019 builder
drwx------    4 root     root          4096 May 20  2019 buildkit
drwx------    3 root     root          4096 May 20  2019 containerd
drwx------    2 root     root         12288 Feb  3 19:35 containers
drwx------    3 root     root          4096 May 20  2019 image
drwxr-x---    3 root     root          4096 May 20  2019 network
drwx------    6 root     root         77824 Feb  3 19:37 overlay2
drwx------    4 root     root          4096 May 20  2019 plugins
drwx------    2 root     root          4096 Feb  1 13:09 runtimes
drwx------    2 root     root          4096 May 20  2019 swarm
drwx------    2 root     root          4096 Feb  3 19:37 tmp
drwx------    2 root     root          4096 May 20  2019 trust
drwx------   15 root     root         12288 Feb  3 19:35 volumes 
```

### Docker 图像

最重的内容通常是图像。如果您使用默认存储驱动程序 overlay2，那么您的 Docker 图像将存储在`/var/lib/docker/overlay2`中。在那里，您可以找到不同的文件，这些文件表示 Docker 图像的只读层和包含您的更改的顶层。

让我们通过一个例子来探究其内容:

```
$ docker image pull nginx
$ docker image inspect nginx

[
    {
        "Id": "sha256:207...6e1",
        "RepoTags": [
            "nginx:latest"
        ],
        "RepoDigests": [
            "nginx@sha256:ad5...c6f"
        ],
        "Parent": "",
 ...
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 126698063,
        "VirtualSize": 126698063,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/585...9eb/diff:
                             /var/lib/docker/overlay2/585...9eb/diff",
                "MergedDir": "/var/lib/docker/overlay2/585...9eb/merged",
                "UpperDir": "/var/lib/docker/overlay2/585...9eb/diff",
                "WorkDir": "/var/lib/docker/overlay2/585...9eb/work"
            },
... 
```

**LowerDir** 包含图像的只读层。代表变化的读写层是**上层**的一部分。在我的例子中，NGINX **UpperDir** 文件夹包含日志文件:

```
$ ls -la /var/lib/docker/overlay2/585...9eb/diff

total 8
drwxr-xr-x    2 root     root    4096 Feb  2 08:06 .
drwxr-xr-x    3 root     root    4096 Feb  2 08:06 ..
lrwxrwxrwx    1 root     root      11 Feb  2 08:06 access.log -> /dev/stdout
lrwxrwxrwx    1 root     root      11 Feb  2 08:06 error.log -> /dev/stderr
```

**MergedDir** 代表 Docker 用来运行容器的 **UpperDir** 和 **LowerDir** 的结果。**工作目录**是 overlay2 的内部目录，应该为空。

### Docker 卷

可以将持久性存储添加到容器中，以在容器存在后更长时间内保存数据，或者与主机或其他容器共享卷。通过使用 **-v** 选项，可以使用卷启动容器:

```
$ docker run --name nginx_container -v /var/log nginx
```

我们可以通过以下方式获取有关已连接卷位置的信息:

```
$ docker inspect nginx_container

...
"Mounts": [
            {
                "Type": "volume",
                "Name": "1e4...d9c",
                "Source": "/var/lib/docker/volumes/1e4...d9c/_data",
                "Destination": "/var/log",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
... 
```

引用的目录包含来自 NGINX 容器位置`/var/log`的文件。

```
$ ls -lah /var/lib/docker/volumes/1e4...d9c/_data

total 88
drwxr-xr-x    4 root     root        4.0K Feb  3 21:02 .
drwxr-xr-x    3 root     root        4.0K Feb  3 21:02 ..
drwxr-xr-x    2 root     root        4.0K Feb  3 21:02 apt
-rw-rw----    1 root     43             0 Jan 30 00:00 btmp
-rw-r--r--    1 root     root       34.7K Feb  2 08:06 dpkg.log
-rw-r--r--    1 root     root        3.2K Feb  2 08:06 faillog
-rw-rw-r--    1 root     43         29.1K Feb  2 08:06 lastlog
drwxr-xr-x    2 root     root        4.0K Feb  3 21:02 nginx
-rw-rw-r--    1 root     43             0 Jan 30 00:00 w 
```

## 清理 Docker 使用的空间

建议使用 Docker 命令来清理未使用的容器。可以通过执行以下命令来清理容器、网络、映像和构建缓存:

```
$ docker system prune -a
```

此外，您还可以通过执行以下命令来删除未使用的卷:

```
$ docker volumes prune
```

## **总结**

Docker 是许多人的环境和工具的重要组成部分。有时候，Docker 感觉有点像魔术，它以一种非常聪明的方式解决问题，而不告诉用户事情是如何在幕后完成的。尽管如此，Docker 仍然是一个常规工具，它将其沉重的部分存储在可以打开和更换的位置。

有时，存储空间会很快填满。因此，检查其根文件夹是有用的，但不建议手动删除或更改任何文件。相反，prune 命令可用于释放磁盘空间。

我希望你喜欢这篇文章。如果你喜欢它，觉得需要一轮掌声，[在 Twitter 上关注我](https://twitter.com/sesigl)。我在易贝·克莱南泽根公司工作，这是全球最大的机密公司之一。顺便说一下，[我们正在招聘](https://jobs.ebayclassifiedsgroup.com/ebay-kleinanzeigen)！

快乐码头探索:)

## 参考

*   Docker 存储文件
    [https://docs.docker.com/storage/storagedriver/](https://docs.docker.com/storage/storagedriver/)
*   文档覆盖文件系统
    [https://www . kernel . org/doc/Documentation/file systems/Overlay fs . txt](https://www.kernel.org/doc/Documentation/filesystems/overlayfs.txt)