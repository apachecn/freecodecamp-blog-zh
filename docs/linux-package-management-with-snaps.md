# 使用快照进行 Linux 包管理

> 原文：<https://www.freecodecamp.org/news/linux-package-management-with-snaps/>

管理 Linux 机器——尤其是远程机器——的很大一部分是管理和安装软件。

当本地应用程序出现问题，或者文件系统出现问题需要修复时，您通常会希望推送更新，而不必跋涉数英里坐在物理屏幕前。

当然，很多问题可以通过 Bash 脚本来解决，但是仍然有很多用例无法替代传统的二进制文件。

假设您的一些远程系统需要安装新的应用程序，以便使用这些计算机的团队成员能够执行一些业务功能。能够利用主要 Linux 存储库系统之一(如 Debian 或 RPM)的集成和自动化可以使您的管理任务变得更加容易。

在本文中，我们将探索一个相对较新的独立包管理系统:Snap。

正如 Linus Torvalds 不厌其烦地提醒我们的那样，许多 Linux 软件管理系统的问题在于有太多的 Linux 软件管理系统。

多年来，应用程序开发甚至 Linux 的采用变得越来越复杂。如果你想让你的软件进入 RPM 系统，你在准备软件上投入的所有时间和工作，比如说，Debian repos，对你没有帮助。这对 SUSE 的 zypper 经理也没什么帮助。

正如我在[my plural sight course:Linux System Maintenance and trouble shooting](https://pluralsight.pxf.io/VMKQj)中所展示的那样，解决软件孤岛问题的一个有希望的解决方案是使用它们自己的独立环境来分发应用程序，这些环境可以在任何 Linux 发行版上工作。

在这个年轻且不断发展的领域，两大标准是 AppImage 和 snap。我们从快照开始。

## 使用捕捉

snap 系统——在赞助 Ubuntu 的 Canonical 公司的指导下——将每个单独的应用程序安装在自己的虚拟分区中。所有这些循环分区肯定会把 df 命令的输出搞得一团糟，但是它们也代表了一种在任何和所有 Linux 安装上分发单一版本软件的合理方法。

```
$ df
Filesystem     1K-blocks      Used Available Use% Mounted on
udev             7101884         0   7101884   0% /dev
tmpfs            1432092      3936   1428156   1% /run
/dev/sda2      479152840 183520724 271222724  41% /
tmpfs            7160452    329336   6831116   5% /dev/shm
tmpfs               5120         4      5116   1% /run/lock
tmpfs            7160452         0   7160452   0% /sys/fs/cgroup
/dev/loop2           384       384         0 100% /snap/gnome-characters/539
/dev/loop4         56320     56320         0 100% /snap/core18/1705
/dev/loop5         56320     56320         0 100% /snap/core18/1754
/dev/loop3        145664    145664         0 100% /snap/slack/23
/dev/loop0          2560      2560         0 100% /snap/gnome-calculator/730
/dev/loop6         15360     15360         0 100% /snap/aws-cli/130
[...]
/dev/loop21       521216    521216         0 100% /snap/onlyoffice-desktopeditors/38
/dev/loop22       145664    145664         0 100% /snap/slack/22
/dev/loop23       185472    185472         0 100% /snap/spotify/36
/dev/loop25        96128     96128         0 100% /snap/core/8935
/dev/loop26       319104    319104         0 100% /snap/onlyoffice-desktopeditors/43
/dev/loop27         1152      1152         0 100% /snap/drawing/16
/dev/loop24        56192     56192         0 100% /snap/gtk-common-themes/1502
/dev/loop31         2560      2560         0 100% /snap/gnome-calculator/748
/dev/sda1         523248      6152    517096   2% /boot/efi
tmpfs            1432088        12   1432076   1% /run/user/121
tmpfs                100         0       100   0% /var/lib/lxd/shmounts
tmpfs                100         0       100   0% /var/lib/lxd/devlxd
tmpfs            1432088        68   1432020   1% /run/user/1000 
```

在这个演示中，我将向您展示如何打包一个基于 GitHub 的应用程序。有了这样一个包，理论上你可以把它提交到官方的 snap store，如果被接受，地球上的任何人都可以免费获得。

现在，我可以假装不知疲倦地工作，找出从命令行完成所有这些工作的最佳方法——但这并不完全诚实。实际上，这一点也不诚实。

事实上，我只是使用了 snapcraft.io 网站上的“第一次快照”教程，让你选择一种语言，然后有帮助地指导你完成这个过程的每一步。最后，它向您展示了如何将您的快照提交到官方快照商店。

我将从命令行带您完成这个过程，但是，如果您是为自己而做的，那么查看网站以确保没有任何变化可能是有意义的。

让我们开始吧。您首先需要确保正确安装了 virtual machine manager Multipass，因为这是 snap 用来创建虚拟机(将在其中构建映像)的工具。自然，Multipass 本身可以作为快照使用。

同样，您将需要 snapcraft 包。安装 snapcraft 后，您应该使用“hash -r”来刷新您的 shell 将查找已知程序的位置列表。

```
$ sudo snap install multipass --classic
$ sudo snap install snapcraft --classic
$ hash -r 
```

当我使用 Python 作为我的语言时，教程为我提供了一个到 GitHub 站点的链接，该站点是一个名为 OfflineIMAP 的开源 Python 电子邮件备份项目。就此而言，不要觉得自己局限于 Python。显然，您可以用自己的项目来代替这个例子。

当我在本地克隆了项目后，我会将 cd 放入新的 offlineimap 目录。接下来，我将使用 wget 下载 YAML 配置文件的特定于 Python 的特殊版本。

由于目录中已经有一个同名的文件，这个文件将获得一个替代名称，所以我将通过更改新文件的名称来覆盖旧的副本。然后，我们将打开该文件，并编辑花括号中单词“name”出现的三个位置。我需要用我真正想用的名字来替换它们。

```
$ git clone https://github.com/snapcraft-docs/offlineimap
$ cd offlineimap
$ wget https://snapcraft.io/first-snap/python/snapcraft.yaml 
```

从这里开始，运行“snapcraft”将负责打包过程。这可能是一个漫长的过程，尤其是如果您需要的软件(如 Multipass)尚未安装和设置。您可能会看到一些错误，但是安装脚本很可能会动态地自动修复它们。

当这些都消失后，您可以使用常规的“snap install”命令在本地安装 snap，但您需要添加- devmode 和- dangerous，因为这不是官方支持的 snap，所以从技术上讲，没有人知道当您启动它时会发生什么。

您可以通过运行“snap list”来证明它已安装，然后通过运行 test-offlineimap-mysnap 命令并使用-h 获得帮助屏幕来确认一切正常。

享受这个软件——我知道这种电子邮件备份是我多年来一直想做的事情。

```
$ snapcraft
$ sudo snap install --devmode --dangerous *.snap
$ snap list
$ test-offlineimap-mysnap -h 
```

如果您有兴趣了解如何在您的 Linux 环境中管理快照，您可能也会喜欢我的“[如何管理 Ubuntu 快照:没人告诉您的事情](https://www.freecodecamp.org/news/managing-ubuntu-snaps/)”和“ [snapd 使管理 Nextcloud 成为一个快照](https://www.freecodecamp.org/news/snapd-nextcloud/)”文章。

## 与其他包管理器一起工作

我们刚刚看了很多快照。但也许现在是承认我遗漏了替代软件包管理器世界中其他一些大玩家的最佳时机，特别是 [Flatpak](https://flatpak.org/setup/) 和 [AppImages](https://appimage.org/) 。

我在这里对 AppImages 进行了深入的讨论，但是在这里简单介绍一下 Flatpak 也不为过。

Flatpak 的主要目标是让开发人员将他们的应用程序构建到一个包中，然后将它们分发到任何 Linux 发行版中。作为最终用户，您可以使用常规的软件管理器安装 Flatpak 系统，比如 Ubuntu 上的 Apt 或 CentOS 上的 Yum。Flatpak 默认安装在 Fedora 上。从那以后，一切都很顺利。解决了所有的问题，不是吗？

也许吧。最近有一些关于 Flatpak 的基础设计中可能存在的(和重大的)安全缺陷的批评。我会让你自己决定。

**在我的【bootstrap-it.com】有更多的书籍、课程和文章形式的管理知识。**