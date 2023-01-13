# 如何加快 Mac Docker 中共享文件的访问速度

> 原文：<https://www.freecodecamp.org/news/speed-up-file-access-in-docker-for-mac-fbeee65d0ee7/>

Docker 刚刚发布了一个本地 MacOS 运行时环境，可以轻松地在 MAC 上运行容器。他们修复了许多问题，但痛苦的事实是他们错过了一些重要的东西。对已装入卷的读写访问非常糟糕。

### 基准

我们可以通过执行以下命令来启动容器并写入已装载的卷:

1.  启动容器
2.  挂载当前目录
3.  将随机数据写入此目录中的文件

```
docker run --rm -it -v "$(PWD):/pwd" -w /pwd alpine time dd if=/dev/zero of=speedtest bs=1024 count=100000
```

让我们比较一下 Windows、Cent OS 和 Mac OS 的结果:

### Windows 10

```
100000+0 records in
100000+0 records out
real    0m 0.29s
user    0m 0.03s
sys     0m 0.21s
```

### 摘录

```
100000+0 records in
100000+0 records out
real    0m 0.21s
user    0m 0.02s
sys     0m 0.14s
```

### mac 操作系统

```
100000+0 records in
100000+0 records out
real 0m 19.32s
user 0m 0.42s
sys 0m 1.46s
```

所以获胜者是…写作 19 秒。对于阅读来说，情况非常相似。当你开发一个大的 dockerized 应用程序时，你就处于一个不利的位置。通常你会在你的源代码上工作，并期望构建不会变慢。但残酷的事实是，这需要很长时间。

这个 [GitHub 问题](https://github.com/docker/for-mac/issues/77)跟踪当前状态。有很多仇恨，所以最好听“成员”的，而不是阅读所有的挫折。

来自 Docker for Mac 团队的@dsheetz 解决了这个问题:

> 也许需要理解的最重要的事情是，**共享文件系统的性能是多维的**。这意味着，根据您的工作负载，您可能会在使用 Docker for Mac 中的文件系统服务器`osxfs`时体验到异常、足够或差的性能。文件系统 API 非常广泛(20-40 种消息类型)，具有许多复杂的语义，涉及磁盘状态、内存缓存状态和多个进程的并发访问。此外，`osxfs`集成了 OS X 的*f events*API 和 Linux 的 *inotify* API 之间的映射，后者在文件系统本身内部实现，这使得事情更加复杂(特别是缓存行为)。

> 在最高级别，文件系统性能有两个维度:*吞吐量*(读/写 IO)和*延迟*(往返时间)。在现代 SSD 上的传统文件系统中，应用程序通常可以预期几 GB/s 的吞吐量。对于大型顺序 IO 操作，`osxfs`可以实现大约 250 MB/s 的吞吐量，虽然不是本机速度，但对于大多数在 HDD 上运行良好的应用程序来说，这不会成为瓶颈。

> *延迟*是文件系统系统调用完成所需的时间。例如，一个线程在容器中发出*写*和继续写入字节数之间的时间。对于传统的基于数据块的文件系统，这种延迟通常低于 10μs(微秒)。使用`osxfs`，目前大多数操作的延迟约为 200μs 或慢 20 倍。对于需要多次连续往返的工作负载，这会导致明显的速度下降。为了减少延迟，我们需要缩短从 Linux 系统调用到 OS X 的数据路径。这需要依次调整数据路径中的每个组件-其中一些需要大量的工程工作。即使我们实现了 100μs/往返的巨大延迟减少，我们仍然“只能”看到性能翻倍。这是典型的性能工程，它需要大量的工作来分析减速和开发优化的组件。

许多人用不同的方法创造了变通方法。其中一些使用 nfs、Docker 中的 Docker、Unison 2 way sync 或 rsync。我尝试了一些解决方案，但是没有一个适用于我的 docker 容器，它包含一个巨大的 Java monolith。要么他们安装像流浪者这样的额外工具来减少痛苦。vagger 使用 nfs，但与本机读写性能相比，这仍然很慢。或者它们不可靠，难以设置和维护。

我后退了一步，再次思考根源问题。一个非常好的方法是 [docker-sync](https://github.com/EugenMayer/docker-sync) 。这是一个有很多选项的 ruby 应用程序。一个非常成熟的选择是基于 rsync 的文件同步。

### Rsync

Rsync 最初发布于 1996 年(20 年前)。它是用来在计算机系统间传输文件的。一个重要的用例是单向同步。

听起来不错…，对吗？

Docker-sync 支持 rsync 进行同步。开始时它工作正常，但是几天后我在我的主机和我的容器之间遇到了一些连接问题。

你知道当你想解决某件事却感觉很遥远的感觉吗？你意识到你不明白幕后发生了什么。

rsync 方法听起来不错。它解决了问题的根源:现在对挂载文件的操作非常慢。

我尝试了其他解决方案，但没有真正成功。

### 构建自定义图像

所以让我们试着弄脏我们的手。您在容器中启动一个 rsync 服务器，并使用 rsync 连接到它。这种方法在其他用例中可以工作很多年。

让我们设置一个 docker Centos 6 容器，其中安装并配置了 rsync 服务。

1.  文档文件

```
FROM centos:6
# install rsync
RUN yum update -y
RUN yum -y install rsync xinetd
# configure rsync
ADD ./rsyncd.conf /root/
RUN sed -i 's/disable[[:space:]]*=[[:space:]]*yes/disable = no/g' /etc/xinetd.d/rsync # enable rsync
RUN cp /root/rsyncd.conf /etc/rsyncd.conf
RUN /etc/rc.d/init.d/xinetd start
RUN chkconfig xinetd on
# create the dir that will be synced
RUN mkdir /home/share
# just to keep the container running
CMD /etc/rc.d/init.d/xinetd start && tail -f /dev/null
```

2.在存储库目录中构建容器。

```
docker build . -t docker-rsync
```

3.启动容器并将 rsync 服务器端口映射到特定的主机端口。

```
docker run -p 10873:873 docker-rsync
```

现在，我们需要同步我们的共享目录，并且一旦有任何变化，就同步任何变化。Rsync 只会在初始同步后同步更改。

```
# initial sync
rsync -avP ./share --delete rsync://localhost:10873/example/
# sync on change
fswatch -0 ./share | xargs -0 -n 1 -I {} rsync -avP ./share --delete rsync://localhost:10873/example/
```

一旦发生变化，Fswatch 就利用 rsync 与容器对话。我们不使用任何类型的 docker 卷安装。因此，所有文件操作都将保留在容器中，并且速度很快。每当我们改变一些东西，rsync 就用。当然，你可以使用所有的 rsync 特性，比如删除规则或者排除模式。

如果我们改变了一些东西(不管是一个小项目还是一个大项目),我们会看到这样的情况

```
2 files to consider
share/helloWorld.txt
           5 100%    0.00kB/s    0:00:00 (xfer#1, to-check=0/2)
sent 159 bytes  received 44 bytes  406.00 bytes/sec
total size is 5  speedup is 0.02
```

0.02 秒，太棒了！

Fswatch 在 Mac OS 上使用文件系统事件。因此仍然非常快，你可以调整它。例如，通过排除构建相关的目录，如*目标*或*节点模块*。

来源可在 [GitHub](https://github.com/Journerist/docker-rsync-example) 上获得。

对于小型项目，糟糕的性能不是一个关键问题。对于大型应用程序，rsync 是我们的英雄。好的旧工具，仍然可靠和重要。

尤其是对于所有喜欢 Mac OS 并且需要使用 VM 的人来说，他们知道这种痛苦。像命令键映射这样的问题很烦人。要么将它映射到 Windows 键，要么最终不再使用它。所以在 Mac OS 上，你使用 cmd+c 来复制一些东西，而在你的容器中，你使用 control。当然，您也可以将主机控件映射到 command，但这样一来，您又会遇到其他问题。当你可以在 Mac OS 中工作，而不是作为 Mac 用户在虚拟机中工作时，一切都会更好。

我希望你喜欢这篇文章。如果你喜欢它，觉得需要一轮掌声，[在 Twitter 上关注我](https://twitter.com/sesigl)。我在易贝·克莱南泽根公司工作，这是全球最大的机密公司之一。顺便说一下，[我们正在招聘](https://jobs.ebayclassifiedsgroup.com/ebay-kleinanzeigen)！

快乐编码:)