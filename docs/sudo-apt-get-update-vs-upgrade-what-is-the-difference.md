# sudo apt-get 更新与升级–有何区别？

> 原文：<https://www.freecodecamp.org/news/sudo-apt-get-update-vs-upgrade-what-is-the-difference/>

`sudo apt-get update`和`sudo apt-get upgrade`是两个命令，可以用来在 Debian 或基于 Debian 的 Linux 发行版中保持所有包的最新状态。

它们是 Linux 管理员和开发人员的常用命令，但是即使您不经常使用命令行，也很容易理解。

在本文中，我将深入探讨这两个命令的作用、使用方法以及一些常见问题。

## `sudo apt-get update`和`sudo apt-get upgrade`有什么区别？

主要的区别是`sudo apt-get update`从你的发行版的软件仓库和你可能配置的任何第三方仓库获取最新版本的包列表。换句话说，它会计算出每个包和依赖项的最新版本，但不会实际下载或安装任何更新。

`sudo apt-get upgrade`命令在您的系统上下载并安装每个过期软件包和依赖项的更新。但是仅仅运行`sudo apt-get upgrade`并不会自动升级过时的包——您仍然有机会检查更改并确认您想要执行升级。

## 如何使用`sudo apt-get update`命令

在基于 Debian 的 Linux 发行版(Debian、Ubuntu、Linux Mint、Kali Linux、Raspberry Pi OS 等等)中，打开一个终端窗口。

根据您的发行版，终端可能会有不同的名称，这取决于您如何打开它。例如，在 Ubuntu 和 Linux Mint 中，默认终端是 Gnome 终端，但可能会列在应用程序菜单中的终端下。

在终端中，在命令行中输入`sudo apt-get update`，输入您的管理员密码，然后按回车键。

如果有更新，您将看到类似如下的输出:

```
kris@pihole:~ $ sudo apt-get update
Hit:1 https://ftp.harukasan.org/raspbian/raspbian bullseye InRelease
Get:2 https://download.docker.com/linux/raspbian bullseye InRelease [26.7 kB]
Get:3 http://archive.raspberrypi.org/debian bullseye InRelease [23.7 kB]       
Get:4 http://packages.azlux.fr/debian buster InRelease [3,989 B]               
Get:5 http://archive.raspberrypi.org/debian bullseye/main armhf Packages [282 kB]
Get:6 http://packages.azlux.fr/debian buster/main armhf Packages [3,418 B]
Fetched 340 kB in 4s (94.8 kB/s)     
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
3 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

如果您想查看哪些包可以升级，运行`apt list --upgradable`:

```
kris@pihole:~ $ apt list --upgradable
Listing... Done
libcamera0/stable 0~git20220426+18e68a9b-1 armhf [upgradable from: 0~git20220303+e68e0f1e-1]
raspi-config/stable 20220425 all [upgradable from: 20220419]
rpi-eeprom/stable 13.13-1 armhf [upgradable from: 13.12-1]
```

但是如果您的发行版的软件仓库中没有新版本的软件包或依赖项，您将会看到如下输出:

```
kris@pihole:~ $ sudo apt-get update
Get:1 https://download.docker.com/linux/raspbian bullseye InRelease [26.7 kB]
Hit:2 https://ftp.harukasan.org/raspbian/raspbian bullseye InRelease           
Hit:3 http://packages.azlux.fr/debian buster InRelease                         
Hit:4 http://archive.raspberrypi.org/debian bullseye InRelease
Fetched 26.7 kB in 3s (8,789 B/s)
Reading package lists... Done
```

注意，没有提到可以升级的包，也没有关于运行`apt list --upgradable`的说明。

但这并不一定意味着你的系统上没有过时的软件，只是意味着你已经得到了最新版本的软件包列表。您可能已经多次运行`sudo apt-get update`。

你总是可以再次运行`apt list --upgradable`来看看是否有什么可以升级的。

或者您可以使用更现代的`sudo apt update`命令来代替。这个命令将始终显示可以升级的软件包数量，或者显示一个提示，说明一切都是最新的。

关于`apt`和`apt-get`、[之间的差异的更多信息，请查看](#what-s-the-difference-between-apt-get-and-apt)下面的部分。

## 如何使用`sudo apt-get upgrade`命令

在运行了`sudo apt-get update`命令之后，在同一个终端窗口中，键入`sudo apt-get upgrade`，如果需要的话输入你的密码，然后点击 enter。

然后，您将看到类似如下的输出:

```
kris@pihole:~ $ sudo apt-get upgrade
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Calculating upgrade... Done
The following packages will be upgraded:
  libcamera0 raspi-config rpi-eeprom
3 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Need to get 2,616 kB of archives.
After this operation, 1,596 kB of additional disk space will be used.
Do you want to continue? [Y/n] 
```

在输出的底部，您会看到将要升级的包:

```
The following packages will be upgraded:
  libcamera0 raspi-config rpi-eeprom
3 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

需要提取的数据量，以及升级后的软件包安装后将使用的存储空间量:

```
Need to get 2,616 kB of archives.
After this operation, 1,596 kB of additional disk space will be used.
```

最后，您会看到一个提示，询问您是否要继续升级:

```
Do you want to continue? [Y/n] 
```

您可以输入`y`、`Y`或`yes`继续升级，或者输入`n`、`N`或`no`退出`upgrade`命令。

如果您选择退出，您将看到如下输出:

```
kris@pihole:~ $ sudo apt-get upgrade
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Calculating upgrade... Done
The following packages will be upgraded:
  libcamera0 raspi-config rpi-eeprom
3 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Need to get 2,616 kB of archives.
After this operation, 1,596 kB of additional disk space will be used.
Do you want to continue? [Y/n] n
Abort.
```

如果您选择继续升级，您将看到如下所示的长输出:

```
kris@pihole:~ $ sudo apt-get upgrade
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Calculating upgrade... Done
The following packages will be upgraded:
  libcamera0 raspi-config rpi-eeprom
3 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Need to get 2,616 kB of archives.
After this operation, 1,596 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://archive.raspberrypi.org/debian bullseye/main armhf libcamera0 armhf 0~git20220426+18e68a9b-1 [548 kB]
Get:2 http://archive.raspberrypi.org/debian bullseye/main armhf raspi-config all 20220425 [30.3 kB]
Get:3 http://archive.raspberrypi.org/debian bullseye/main armhf rpi-eeprom armhf 13.13-1 [2,037 kB]
Fetched 2,616 kB in 3s (1,019 kB/s)   
Reading changelogs... Done
(Reading database ... 43496 files and directories currently installed.)
Preparing to unpack .../libcamera0_0~git20220426+18e68a9b-1_armhf.deb ...
Unpacking libcamera0:armhf (0~git20220426+18e68a9b-1) over (0~git20220303+e68e0f1e-1) ...
Preparing to unpack .../raspi-config_20220425_all.deb ...
Unpacking raspi-config (20220425) over (20220419) ...
Preparing to unpack .../rpi-eeprom_13.13-1_armhf.deb ...
Unpacking rpi-eeprom (13.13-1) over (13.12-1) ...
Setting up rpi-eeprom (13.13-1) ...
Setting up libcamera0:armhf (0~git20220426+18e68a9b-1) ...
Setting up raspi-config (20220425) ...
Processing triggers for man-db (2.9.4-2) ...
Processing triggers for libc-bin (2.31-13+rpt2+rpi1+deb11u2) ...
```

一旦完成，所有过时的包和依赖项都将被更新。

关于`sudo apt-get upgrade`命令要记住的一件重要事情是，它只升级它能升级的东西，而不删除任何东西。

例如，如果升级需要一个新的依赖项，`upgrade`命令会下载并安装它，但不会删除旧的依赖项。删除旧的依赖关系需要不同的命令。当您升级到新的内核版本时，您会经常看到这种情况。

如果您在升级后看到类似以下内容的消息:

```
The following packages were automatically installed and are no longer required:
  g++-8 gir1.2-mutter-4 libapache2-mod-php7.2 libcrystalhd3
Use 'sudo apt autoremove' to remove them.
```

你可以按照建议，用`sudo apt autoremove`去掉那些不必要的包。

## 如何通过`sudo apt-get upgrade`命令使用特殊选项

有许多特殊的选项或参数可以和`sudo apt-get upgrade`命令一起使用，但有两个特别突出:`--dry-run`和`--yes`。

### 如何使用`--dry-run`选项:

`--dry-run`(或者，`-s`或`--simulate`)选项模拟升级过程中会发生什么，但实际上不会改变系统上的任何东西:

```
kris@pihole:~ $ sudo apt-get upgrade --dry-run
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Calculating upgrade... Done
The following packages will be upgraded:
  libcamera0 raspi-config rpi-eeprom
3 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Inst libcamera0 [0~git20220303+e68e0f1e-1] (0~git20220426+18e68a9b-1 Raspberry Pi Foundation:stable [armhf])
Inst raspi-config [20220331] (20220425 Raspberry Pi Foundation:stable [all])
Inst rpi-eeprom [13.12-1] (13.13-1 Raspberry Pi Foundation:stable [armhf])
Conf libcamera0 (0~git20220426+18e68a9b-1 Raspberry Pi Foundation:stable [armhf])
Conf raspi-config (20220425 Raspberry Pi Foundation:stable [all])
Conf rpi-eeprom (13.13-1 Raspberry Pi Foundation:stable [armhf])
```

尽管 Debian 和基于 Debian 的发行版非常稳定，但是如果你想确保在升级过程中没有冲突，这个选项是有用的。

### 如何使用`--yes`选项:

如果安全的话，`--yes`(或者，`-y`或`--assume-yes`)选项会自动对任何提示回答“是”:

```
kris@pihole:~ $ sudo apt-get upgrade --yes
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Calculating upgrade... Done
The following packages will be upgraded:
  libcamera0 raspi-config rpi-eeprom
3 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Need to get 2,616 kB of archives.
After this operation, 1,596 kB of additional disk space will be used.
Get:1 http://archive.raspberrypi.org/debian bullseye/main armhf libcamera0 armhf 0~git20220426+18e68a9b-1 [548 kB]
Get:2 http://archive.raspberrypi.org/debian bullseye/main armhf raspi-config all 20220425 [30.3 kB]
Get:3 http://archive.raspberrypi.org/debian bullseye/main armhf rpi-eeprom armhf 13.13-1 [2,037 kB]
...
Processing triggers for libc-bin (2.31-13+rpt2+rpi1+deb11u2) ...
```

注意上面跳过了`Do you want to continue? [Y/n]`，所有包都升级了。

## 常见问题

### 什么是`sudo`和`apt-get`？

关于`sudo apt-get update`和`sudo apt-get upgrade`需要注意的一点是，这两个命令都由三部分组成:`sudo`、`apt-get`和`update`或`upgrade`。

`sudo`代表“超级用户 do”，允许你以 root 或管理员权限运行程序。

例如，重新启动系统需要超级用户/root 级别的权限，因此在终端中运行`reboot`可能会返回类似如下的错误:

```
Failed to set wall message, ignoring: Interactive authentication required.
Failed to reboot system via logind: Interactive authentication required.
Failed to open initctl fifo: Permission denied
Failed to talk to init daemon.
```

但是如果你运行`sudo reboot`，然后输入你的管理员密码，你将作为超级用户运行`reboot`命令，你的系统将立即重启。

`apt-get`是 Debian 和基于 Debian 的 Linux 发行版中的命令行工具，用于安装和管理软件包。

### `apt-get`和`apt`有什么区别？

是一个更现代的工具，用于在 Debian 和基于 Debian 的发行版上安装和管理应用程序。

在大多数情况下，`apt`和`apt-get`可以互换使用——`sudo apt update`和`sudo apt-get update`都可以更新系统上的软件包列表。

你会注意到的主要区别是`apt`更容易键入，它的输出通常更有用，并且它包括一些用户友好的功能，比如安装软件包时的进度条。

虽然本文中的大多数例子都使用了`apt-get`，但是我强烈建议您使用`apt`来代替。

### `sudo apt-get update`和`sudo apt-get upgrade`用起来安全吗？

是的，Debian 和基于 Debian 的发行版通常非常稳定，`update`和`upgrade`命令可以安全使用。这是因为包/依赖项的主要更新，以及发行版本身，一年只发布一次或两次。

缺点是，不像 Arch Linux 这样的前沿发行版，如果你想使用最新版本的包，你可能需要做一些额外的工作。您可能需要通过 PPA 配置第三方存储库，使用替代的打包系统，如 Flatpak 的 Snap，或者自己编译包。

但是稍微老一点的软件带来的稳定性是值得的，至少在我看来是这样。

### 你能链接`sudo apt-get update`和`sudo apt-get upgrade`命令吗？

您可能会想，运行`sudo apt-get update`，等待它完成，然后运行`sudo apt-get upgrade`难道不乏味吗？

虽然`sudo apt-get update`和`sudo apt-get upgrade`都运行得很快，但有时执行一串命令并在几分钟后检查它们更容易。

使用`&&`操作符，您可以像这样将多个命令链接在一起:

```
sudo apt-get update && sudo apt-get upgrade
```

使用`&&`操作符需要记住的重要一点是，只有在操作符之前的命令成功时，操作符之后的命令才会运行。

使用上面的例子，`sudo apt-get upgrade`只在`sudo apt-get update`成功时运行。如果出现某种错误，比如更新包列表时出现网络问题，那么`sudo apt-get update`就会被跳过。

### 什么是`sudo apt-get dist-upgrade`和`sudo apt full-upgrade`，用起来安全吗？

根据[这个堆栈溢出线程](https://askubuntu.com/questions/770135/apt-full-upgrade-versus-apt-get-dist-upgrade)的说法，这些命令在引擎盖下做着同样的事情——它们升级过时的包，也在必要时智能地移除一些包。

本质上，它们就像是`sudo apt-get upgrade`和`sudo apt autoremove`命令的组合。

在大多数情况下，运行这些命令*应该是安全的。*

但是很多人，包括我自己，都推荐用`sudo apt-get update`和`sudo apt-get upgrade`来代替。你有更多的机会回顾即将到来的变化，而且由于`upgrade`从不移除包，它的破坏性更小。

## `./thanks_for_reading.sh`

如果你发现`sudo apt-get update`和`sudo apt-get upgrade`上的分类有用，请与你的朋友分享，这样更多的人可以从中受益。

此外，请随时在 [Twitter](https://twitter.com/kriskoishigawa) 上联系我，告诉我你的想法。