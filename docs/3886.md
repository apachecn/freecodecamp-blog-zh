# 如何使用 PSSH 在多个 Linux 主机上运行命令

> 原文：<https://www.freecodecamp.org/news/running-commands-linux-hosts-using-pssh/>

我相信你已经听说过，最近所有的酷孩子都在玩编排自动化。但是你知道为什么吗？首先，现代微服务工作负载消耗的资源变得越来越复杂，部署到的实例比以往任何时候都多。其次，越来越多的资源是虚拟的，而不是物理的——所以它们中的许多只会存在几分钟甚至几秒钟。

所有这一切意味着，即使你想登录到你的许多服务器的每一个，它只是没有意义。事实上，在大多数情况下，这甚至是不可能的。相反，您将运行许多聪明的脚本。用来运行这些脚本的工具通常被称为 orchestrators。

我相信你至少遇到过一两个编配俱乐部的成员。除了 Ansible，还有 Terraform，Chef，Puppet 等。但是也有一些底层的工具可以作为核心 Linux 工具的附件，比如 SSH。虽然，看到它如何在 Windows 上运行，当然还有 macOS，我不确定把 SSH 称为“Linux”工具是否正确。

其中一个 ssh 插件是一个名为 pssh 的工具集——代表并行 SSH。这就是我们在这篇文章中将要学习的内容——这篇文章摘自我的新 [Pluralsight 课程，Linux 系统优化](https://pluralsight.pxf.io/RqrJb)。

不过现在，我要告诉你一些关于我正在使用的实验室的情况，这样你就可以更容易地在家里复制和跟随。我有三个 Ubuntu [LXD 容器](https://www.freecodecamp.org/news/linux-containers-lxc-lxd/)在运行。我们所有操作的基础将使用 IP 地址 10.0.3.140，而我们将远程调配的两个主机节点将使用 10.0.3.93 和 10.0.3.43。

我们要做的一切都假设我们已经获得了从我的基本容器到两个节点的无密码 SSH 访问。如果你不确定如何做到这一点，你可以在 Pluralsight 上查看我的[协议深度探讨:SSH 和 Telnet 课程](https://pluralsight.pxf.io/9DYVe)的 SSH 模块。如果你很急，[这个红帽教程](https://www.redhat.com/sysadmin/passwordless-ssh)会带你去同一个地方。

在 Ubuntu 上安装 pssh 简单快捷:`sudo apt install pssh`。在 CentOS 上不会变得更难。

我创建了一个名为 sshhosts.txt 的简单主机清单文件，它只包含我的两个节点的 IP 地址:

```
$ less sshhosts.txt
10.0.3.93
10.0.3.43 
```

现在，我将运行 pssh parallel-ssh 命令，在我的主机上执行一个命令。

```
$ parallel-ssh -i -h sshhosts.txt df -ht ext4 
```

-i 告诉程序以交互方式运行-否则我们不会看到任何命令输出。-h 指向我称为 sshhosts.txt 的 hosts 文件，命令本身将是旧的 Unix 实用程序 df。这将返回连接到系统的驱动器列表，以及它们的挂载点和使用信息。这里的-h 将以人类可读的单位显示磁盘空间，t 将限制对格式化为 ext4 的驱动器的访问。

我为什么关心 ext4 业务？因为 Ubuntu 使用 snap 包管理器，每个 snap 都创建自己的虚拟设备。那又怎样？嗯，我不想为了得到报告实际使用情况的真实驱动器，而不得不梳理十几个报告空闲空间为 0 的虚拟设备。

```
$ parallel-ssh -i -h sshhosts.txt df -ht ext4
[1] 22:02:00 [SUCCESS] 10.0.3.43
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2       457G  131G  304G  30% /
[2] 22:02:00 [SUCCESS] 10.0.3.93
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2       457G  131G  304G  30% / 
```

这就对了。我的两个节点的完整磁盘空间信息。我相信你已经注意到信息是一样的。这是因为这两个容器都运行在我的工作站上，所以据他们所知，他们都可以完全访问我自己的驱动器。

对于我的下一个技巧，我将从我的每个节点收集/etc/group 文件。这种操作有助于快速监控节点的安全状态。您可以添加一个脚本来解析传入的数据，并在出现任何异常时向您发出警报。

在开始之前，我将在本地创建一个名为 host-files 的目录。然后我将使用`parallel-slurp`命令——它的名字很好地描述了它的功能。同样，-h 指向主机文件。`-L`将 host-files 目录设置为写入我们将要生成的数据的目标位置，`/etc/group`是我们想要获取的远程文件，`group`是我们想要在本地分配数据的名称。

```
mkdir host-files
parallel-slurp -h sshhosts.txt -L host-files/ /etc/group group 
```

完成后，您的 host-files 目录将包含以每个节点的 IP 地址命名的子目录。如您所见，有一个名为“group”的文件，其中包含来自每个节点的/etc/group 数据。

```
$ tree host-files/
host-files/
├── 10.0.3.43
│   └── group
└── 10.0.3.93
    └── group 
```

嘘，还有其他的好吃的吗？没错。运行`apropos`会给出整个列表。

```
$ apropos parallel
parallel-nuke (1)    - parallel process kill program
parallel-rsync (1)   - parallel process kill program
parallel-scp (1)     - parallel process kill program
parallel-slurp (1)   - parallel process kill program
parallel-ssh (1)     - parallel ssh program 
```

*本文基于我的 [Pluralsight 课程“Linux 系统优化”的内容](https://pluralsight.pxf.io/RqrJb)在[bootstrap-it.com](https://bootstrap-it.com)有更多管理方面的书籍、课程和文章。*