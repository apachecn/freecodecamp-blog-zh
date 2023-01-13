# 如何从头开始安装 Arch Linux

> 原文：<https://www.freecodecamp.org/news/installing-arch-linux-from-scratch-b595095ddd48/>

作者安德烈·贾马尔奇

在本文中，您将学习如何从头开始安装 Arch Linux 并且只需大约 5 分钟。所以让我们开始吧。

到今天为止，我愉快地使用 Arch Linux 作为我的主要操作系统已经差不多 3 年了，我不仅每天在我的笔记本电脑上使用它，还在我的[游戏 PC](https://medium.com/@WebReflection/a-gaming-pc-without-breaking-the-bank-b56c73bba1e7#.ktf73jt1x) 和许多[单板计算机](https://benja.io/)上使用它。

我使用我的[archibald . io](https://archibold.io/)安装程序已经有一段时间了，最近在对 Arch Linux 了解越来越多之后，我重新编写了它。

这篇文章是关于*我*回馈我从 [Arch Linux](https://www.archlinux.org/) 项目和它的社区中学到的一些东西，希望能简化那些想要拥抱这个令人敬畏的发行版的人的生活！

### 简而言之，引导 Linux 操作系统

你需要一个被识别为可引导的特殊分区，并且有一些自动识别的二进制文件能够告诉主板如何引导，以及从哪里引导。

在 Arch Linux 领域，基本上有 3 个主要参与者: [U-Boot](http://www.denx.de/wiki/U-Boot/WebHome) ，由 [Arch Linux ARM](https://archlinuxarm.org/) 端口使用的默认引导加载程序， [Syslinux](http://www.syslinux.org/wiki/index.php?title=The_Syslinux_Project) ，这是完全相同的 Arch Linux ISO 安装程序的首选，以及 [Grub](https://www.gnu.org/software/grub/) ，通常更容易在多引导系统上配置，这不属于本文的范围。

处理分区的工具通常是 2: **parted** ，当涉及到像 UEFI 分区和优化磁盘对齐这样的高级功能时，首选，或者是 **fdisk** ，它只是以一种“ *less scriptysh* 的方式工作。

#### 如何启动？

有几种方法可以告诉主板如何引导系统: [UEFI](https://en.wikipedia.org/wiki/Unified_Extensible_Firmware_Interface) 配置，它最适合运行在英特尔 CPU 上的 Windows 操作系统，但也可用于引导 Linux 发行版，以及传统 BIOS 兼容，它的功能比 UEFI 少，但它总是提供(并且是最广泛可用的)。

除此之外，虽然 UEFI 有一个安全引导模式，这基本上也是 Windows 特有的东西，但引导还可以包括能够启用或禁用 [EDD](https://en.wikipedia.org/wiki/INT_13H#EDD) 模式的特殊命令，这是一种很好的老式*增强型磁盘驱动器*技术，在引导时可能不需要，在某些情况下应该明确禁用，如在 VirtualBox 映像或 Gizmo 2 板等基于 AMD 的 SBC 中。

#### 启动什么？

只有一个整合的跨平台和通用文件系统，可悲的是，或者说值得庆幸的是，它是一个又老又肥的系统。

当然有更合适、更安全、更快、更硬件的选择，但是 FAT 和 loader 都是 UEFI 和 legacy 的安全选择。

简而言之，系统中最基本的分区数量应该是 2 个:一个胖的可引导分区，以及一个"*随便你想要的*分区。请注意 [ext4](https://en.wikipedia.org/wiki/Ext4) 对于日常任务来说仍然是一个非常有效的选择，但是有一些有效的替代方案可以考虑，但是不属于本文的范围。

#### 有交换吗？

如果你在一个超过 4GB 或 RAM 的系统上安装 Arch Linux，并且你不打算使用这样的系统来开发复杂的软件，我会说你不应该太担心有一个备份[交换](https://en.wikipedia.org/wiki/Paging)分区。

一般来说，我测试过的只有 2GB 内存的旧笔记本电脑在没有任何额外交换和像 [GNOME](https://www.gnome.org/) 这样的图形桌面的情况下表现很好。

然而，如果你想有一些额外的空间来构建更复杂的软件，并且你有超过 1GB 的内存，使用 1/4 的内存就可以了。

### 好的，但是安装 Linux 的所有命令在哪里呢？

这是这篇文章最精彩的部分，当你完成这一部分的时候，你就可以自我训练回答 archibold.io 安装程序将要问你的所有基本问题。

从网站下载 [Arch Linux ISO](https://www.archlinux.org/download/) ，用它来引导 VirtualBox，或者[把它烧录到 u 盘。按照这个 Wiki 页面](https://wiki.archlinux.org/index.php/USB_flash_installation_media)，你将拥有引导到终端并运行以下代码所需的一切:

```
$ bash <(curl -s archibold.io/base)
```

基本上就是这样了，这个过程会在你的机器上安装你能想到的最基本的 Arch Linux，或者像这篇文章上面的视频显示的那样，在一个 VirtualBox 上安装，以防你想先尝试一下。

### 好的，酷……但是我也想要一台台式机！

在这种情况下，一旦你启动到你的帐户，你可以检查你是否有一个互联网连接键入:

```
ip addr
```

如果您的 wifi 或以太网名称下没有显示任何内容，请按照以下说明操作:

```
# if not logged as root already, type 'su'su# use root as default password# now, in case you have a wired connectionip addr # to see the name of the adapterdhcpcd enp0s3 # where enp0s3 is just a made up name, use your one
```

```
# if it was wi-fi cardwifi-menu # and configure it
```

```
exit # to go back to your user
```

一旦你有一个互联网连接，你可以简单地使用另一个助手:

```
bash <(curl -s archibold.io/install/gnome)
```

后者将指导你安装最好的桌面环境。

#### 对吧..但是…

如果您在任何时候遇到了困难，请不要犹豫，在[开源 archibold 库](https://github.com/WebReflection/archibold.io/tree/gh-pages)中提交一个 bug，或者直接在这里问我问题。

我很确定我能回答大部分问题，所以…放马过来吧！:-)