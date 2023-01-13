# 如何为 Linux 设置 Docker 和 Windows 子系统:一个爱情故事？？

> 原文：<https://www.freecodecamp.org/news/how-to-set-up-docker-and-windows-subsystem-for-linux-a-love-story-35c856968991/>

你有时会觉得自己是一位美丽的公主，被邪恶的巫师变成了一只青蛙吗？好像你不属于这里？我知道。我是一个害怕离开舒适的命令行的 UNIX 家伙。我的终点站是我的城堡。但是有时候我不得不使用微软视窗系统，我已经学会了一些应对的技巧。

对于我的日常终端需求，我已经安装了 Linux 的 Windows 子系统和 Ubuntu 发行版。最重要的是，我安装了 Linuxbrew，它可以帮助我管理我可能需要的任何第三方应用程序。这个组合出奇的好！我有一个漂亮的符号链接来访问我所有的“外部”(即 Windows 托管的)数据:`ln -s ~/external /mnt/c/Users/DoomHammer`并且大多数需求都是通过这种方式实现的。也就是说，除非我需要使用 Docker。

### Docker 有什么特别之处？

与我通常每天使用的大多数应用程序不同，Docker 是一个系统应用程序。这意味着它深深植根于系统中，需要一个真正的守护进程在主机上运行。在这种情况下，我所说的主机是指本机 Microsoft Windows。

是不是意味着不能从 WSL 内部使用 Docker？不一定。但是你需要更多的活动你的肌肉来达到目的。首先，你需要安装 Docker for Windows。有适用于 Windows Server 2016(及更高版本)的 Docker 企业版，也有适用于 Windows 10 专业版或企业版的社区版。而我却被 Windows 10 家庭版卡住了。

### 在 Windows 10 家庭版上使用 Docker

似乎让 Docker 在 Windows 10 家庭版上运行要稍微棘手一些。Docker 社区版需要 Hyper-V 支持，但家庭版不支持。这意味着我需要挖掘出 [Docker 工具箱](https://docs.docker.com/toolbox/toolbox_install_windows/)，这是一个依赖于 Docker Machine 和 Virtualbox 的旧发行版。但是在安装之后，Virtualbox 给了我一个提示，说它不可能运行虚拟化。

事实证明，我在 BIOS 中关闭了虚拟化设置。显然是出于安全考虑。我打开它，再次打开 Virtualbox，还是一样。这让我有点担心。在网上搜索了一下，我找到了查看`systeminfo`的建议。嗯，它清楚地显示*一些*管理程序正在运行。但不是 Virtualbox，当然也不是 Hyper-V，对吧？

令我惊讶的是，它一直是 Hyper-V。家庭版似乎缺少真正使用 Hyper-V 的工具，但这并不意味着管理程序本身没有运行。谢天谢地，关闭它只是一个`bcdedit /set hypervisorlaunchtype off`之遥。在我重启机器后，Virtualbox 迫不及待地开始工作。酷，给我得分！

### Docker 和 WSL，永远的好朋友？

我打开了 Docker Quickstart 终端。第一次运行时，它创建一个 Docker 机器(这就是它需要 Virtualbox 的原因)作为所有容器的主机。我输入`docker run --rm hello-world`，看着进度条，Docker 为我下载了合适的图像。又得分了！

现在，我想在我的 WSL 上使用 Docker，而不是`cmd.exe`。我该怎么做？幸运的是，WSL 可以访问本机 Windows 二进制文件。这意味着我可以运行`docker-machine.exe ls`来查看 Docker 工具箱创建的机器。它应该就在那里，简单地命名为`default`。如果状态不是“运行”，您可以用`docker-machine.exe start`启动它。每次你想运行 Docher 机器时，记住不像在`cmd.exe`中，扩展名(`.exe`)是强制的。

通常我们会调用`docker-machine.exe env`来设置环境变量。

不幸的是，它输出变量的格式被`cmd.exe`理解，而不是被任何像 bash 或 zsh 这样的 Bourne 兼容的 shell 理解。但是我们可以用`docker-machine.exe env --shell sh`改变这种行为。

嗯，差不多了。但是还有一件事悬而未决。证书路径被写成 Windows 路径。如何翻译成 WSL 理解的东西？[一段时间以来](https://github.com/MicrosoftDocs/WSL/releases/tag/17046)，WSL 有一个很好的实用程序叫做`wslpath`。多亏了这个工具，我们可以调用`export DOCKER_CERT_PATH=$(wslpath $DOCKER_CERT_PATH)`，我们**几乎**准备好了。

我们仍然需要 userland 工具。因此，使用您最喜欢的软件包管理器安装 Docker 引擎和 Docker Compose。对我来说，这意味着`brew install docker docker-compose`。之后，一个`docker run --rm hello-world`应该产生与 Docker 工具箱终端完全相同的结果。恭喜你！

### 就这些吗？

不，当然不是。您可能会很快注意到绑定挂载无法正常工作。这是因为 Docker 守护进程需要正确的 Windows 路径，而 WSL 路径很遗憾地不能自动转换。但是我们可以使用一些技巧来改善这种情况。

现在，你需要哪个版本取决于你运行的版本。点击 Win+R 并输入`winver`你应该会看到一个对话框，上面写着:

> 微软视窗系统
> 版本 18.03(操作系统内部版本 17133.73)

如果它实际上是 18.03 或更新版本，您可以编辑`/etc/wsl.conf`如下所示:

```
[automount]
root = /
options = "metadata"
```

这意味着 WSL 将在`/c/`下挂载 C: drive，而不是通常的`/mnt/c`。为什么这很重要？这很重要，因为这是 Docker 守护进程对 Windows 路径的期望。顺便说一下:保存文件后，您需要重新登录以使更改生效。

**警告**！如果你碰巧使用 [wsl-terminal](https://github.com/goreliu/wsl-terminal) 这个变化会打破它。在这种情况下，使用下一种方法。

如果您不想重新登录或者您坚持使用旧版本，另一种方法是将一个装载点绑定到另一个装载点，如下所示:

```
sudo mkdir /c
sudo mount --bind /mnt/c /c
```

更快，但只有登录后才可用。下次重新启动机器或将其添加到 shell 运行时配置中时，您必须重复这个步骤(比如`~/.bashrc`或`~/.zshrc`)。那是因为`/etc/fstab`在 WSL 上并不像预期的那样工作。

正如您可能已经注意到的，这意味着您现在可以使用 mounts 运行 Docker，但前提是您的卷在 Windows 文件系统内。由于命令行`docker`要求绝对路径，这应该没什么大不了的，但是使用 Docker Compose 你必须格外小心。它允许使用相对路径，这样以`./`开头的东西都不会工作。

如果你坚持要用 Docker 挂载 WSL 的文件系统，你可以尝试将所有的`./`替换为`/c/Users/$USERNAME/AppData/Local/lxss`以及项目的`$PWD`。在这种情况下`$USERNAME`不是指你的 WSL 用户名，而是你的 Windows one。

我认为在 Docker Compose 周围写一个包装器让它把工作目录变成这个`lxss`是聪明的，但是不幸的是 WSL 没有权利访问它。我认为这是理所当然的！

### 最后一堵墙

我们可以运行 Docker 并绑定数据目录。我们还能要什么？也许是做港口转运？与本地解决方案不同，通过 Docker Machine 使用 Docker 需要调用`$(docker-machine ip):$PORT`上的每个服务，而不是通常的`localhost:$PORT`。有一种方法可以绕过它，尽管不是很优雅:

```
#!/bin/sh

# This script uses Virtualbox Port Forwarding to make all Docker services
# available on Windows host under `localhost`

VBXMGMT=/c/Program\ Files/Oracle/VirtualBox/VBoxManage.exe

# List all the running container ids
docker ps -q | while read -r i; do
  # List all the ports bound by this container<Paste>
  for port in $(docker port "$i" | cut -d'-' -f1); do
    port_num=$(echo "${port}" | cut -d'/' -f1)
    port_type=$(echo "${port}" | cut -d'/' -f2)
    echo "Create rule natpf1 for ${port_type} port ${port_num}"
    "$VBXMGMT" controlvm "default" natpf1 "${port_type}-port${port_num},${port_type},,${port_num},,${port_num}"
  done
done
```

我相信每次运行一个新容器时，您都可以围绕 Docker 编写一个包装器来执行这个舞蹈。我承认我没有这样测试过，因为大多数时候我满足于转发单个端口。

我希望这能让你和 Docker 在 WSL 上的工作更加愉快。它确实为我做了这件事！

### 文献学

如果不是好心人分享知识，我是不会写这篇文章的。每次我遇到障碍时，我都可以寻找现有的解决方案。以下是帮助我撰写本指南的文章和帖子列表:

[**设置 Docker for Windows 和 WSL 完美工作**](https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly)
[*稍加调整 WSL (Windows Subsystem for Linux，也称为 Bash for Windows)就可以与 Docker 配合使用……*nickjanetakis.com](https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly)[**如何从 Windows 10 WSL 访问 linux/Ubuntu 文件？**](https://superuser.com/a/1110976)
[*超级用户是一个面向电脑爱好者和超级用户的问答网站。加入他们；只需要一分钟……*superuser.com](https://superuser.com/a/1110976)[**docker-machine 中的端口转发？**](https://stackoverflow.com/questions/32174560/port-forwarding-in-docker-machine)
[*你仍然可以从 docker 机器使用的 VirtualBox 中访问 VBoxManage 命令:vbox manage control VM…*stackoverflow.com](https://stackoverflow.com/questions/32174560/port-forwarding-in-docker-machine)