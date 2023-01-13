# 命令行命令–CLI 教程

> 原文：<https://www.freecodecamp.org/news/command-line-commands-cli-tutorial/>

Windows 命令行是 Windows PC 上最强大的实用程序之一。有了它，你可以直接与操作系统交互，做许多图形用户界面(GUI)中没有的事情。

在本文中，我将向您展示可以在 Windows 命令行上使用的 40 个命令，它们可以增强您作为 Windows 用户的信心。

注意:在使用我将向你展示的命令时，你必须小心。这是因为有些命令会对您的 Windows PC 产生持久的负面或正面影响，直到您重置它。

此外，其中一些命令要求您以管理员身份打开命令提示符。
![ss5-1](img/62f24281d4fdb5a4905bddc74d9f6b47.png)

## Windows 命令行命令

### `powershell start cmd -v runAs`–以管理员身份运行命令提示符

以管理员身份输入该命令会打开另一个命令提示符窗口:
![ss1-1](img/499c9aeb957ab8409d73e2495b6e68cd.png)

### `driverquery`–列出所有已安装的驱动程序

访问所有驱动程序很重要，因为它们经常会导致问题。

这就是这个命令的作用——它甚至向您显示您在设备管理器中找不到的驱动程序。
![ss2-1](img/1da5a5fd75b79d4dc6a6ab1deaf92909.png)

### `chdir`或`cd`–将当前工作目录切换到指定目录

![ss3-1](img/826248132b882d9126e52568e1156618.png)

### `systeminfo`–显示您电脑的详细信息

如果您想查看在 GUI 中看不到的关于您的系统的更多详细信息，这是适合您的命令。
![ss4-1](img/26549ee8c5cc86d3c8439d37526c4ef4.png)

### `set`–显示您电脑的环境变量

![ss5-2](img/b29c4838ceb395328e6bab300c653dde.png)

### `prompt`–更改输入命令前显示的默认文本

默认情况下，命令提示符显示您的用户帐户的 c 盘路径。

您可以使用`prompt`命令用语法`prompt prompt_name $G` :
![ss6-1](img/91a42450b70a68563525c74a65e04929.png)来更改默认文本

**注意:B** :如果你不把`$G`加到命令后面，你就不会在文本前面看到大于号。

### `clip`–将项目复制到剪贴板

例如，`dir | clip`将当前工作目录的所有内容复制到剪贴板。
![ss7](img/63a06b8b3120b8bf0084d38fca95fa43.png)

你可以输入`clip /?`然后打`ENTER`看看怎么用。

### `assoc`–列出程序及其相关的扩展名

![ss8](img/d1e0bb9caa365fc7f4c0afc4e8d1aa60.png)

### `title`–使用格式`title window-title-name`更改命令提示符窗口标题

![ss9](img/41f88ded16b1180daa95e08ceab37a71.png)

### `fc`–比较两个相似的文件

如果您是一名程序员或作者，并且希望快速了解两个文件之间的不同之处，您可以输入以下命令，然后输入这两个文件的完整路径。比如`fc “file-1-path” “file-2-path”`。
![ss10](img/8bb0da1875928768a62c4f78ab7fe4ed.png)

### `cipher`–擦除可用空间并加密数据

在 PC 上，您和其他用户仍然可以访问已删除的文件。所以，从技术上讲，它们并没有被删除。

您可以使用 cipher 命令来清除驱动器上的数据并加密这些文件。
![ss11](img/ad7a3cbb24f43d189d922da347614edb.png)

### `netstat -an`–显示打开的端口、它们的 IP 地址和状态

![ss12](img/43e117d9b2a992740049b95d22916e4d.png)

### `ping`–显示网站 IP 地址，让您知道传输数据和获得响应需要多长时间

![ss13](img/84bf7ef79cda98dbeb0dbcb22dcf1c5d.png)

### `color`–改变命令提示符的文本颜色

输入`color attr`查看你可以改变的颜色:
![ss14](img/d478a970555370b68b485c7c9411ade1.png)

输入`color 2`，终端颜色变为绿色:
![ss15](img/cd6aaa8249fd1019febdd48c3a46c4b6.png)

### `for /f "skip=9 tokens=1,2 delims=:" %i in ('netsh wlan show profiles') do @echo %j | findstr -i -v echo | netsh wlan show profiles %j key=clear`–显示所有 Wi-Fi 密码

![ss16](img/365348c73bbc29a8569d69355dec88da.png)

### `ipconfig`–显示关于电脑 IP 地址和连接的信息

![ss17](img/0ffafed25a677addce46bf0d47cae66c.png)

该命令还具有诸如`ipconfig /release`、`ipconfig /renew`和`ipconfig /flushdns`的扩展名，您可以使用这些扩展名对互联网连接问题进行故障诊断。

### `sfc`–系统文件检查器

此命令扫描您的计算机上损坏的文件并修复它们。您可以用来运行扫描的命令的扩展名是`/scannow`。
![ss18](img/474aeec1aa3325e931a70511cb05f108.png)

### `powercfg`–控制可配置的电源设置

您可以使用此命令及其多个扩展名来显示有关电脑电源状态的信息。

您可以输入`powercfg help`来显示这些扩展名。
![ss19](img/02ecca3f0757a517e5e3c9e83ad95bd9.png)

例如，您可以使用`powercfg /energy`生成电池健康报告。
![ss20](img/6ecf8be87b71bce68559bb904fef88c0.png)

`powercfg /energy`命令将生成一个包含报告的 HTML 文件。你可以在`C:\Windows\system32\energy-report.html`中找到 HTML 文件。

### `dir`–列出目录中的项目

![ss21](img/71f3a67b5a4b8f0c2e6117ac90766ed8.png)

### `del`–删除文件

![ss22](img/ac3586c56e0d9f7e8dd1157a2e5af47c.png)

### `attrib +h +s +r folder_name`–隐藏文件夹

你可以通过在命令行中输入`attrib +h +s +r folder_name`然后按下`ENTER`来隐藏一个文件夹。

要再次显示文件夹，执行命令`– attrib -h -s -r folder_name`。
![ss23](img/94990f49414291fc79fab040589efa84.png)

### `start website-address`–从命令行登录网站

![ss24](img/cee28ac35c7f6a393f7278b204d22f07.png)


### `tree`–显示当前目录或指定驱动器的树

![ss26](img/68e0840b5a89ceb4cc25ccdc5a853a00.png)

### `ver`–显示操作系统的版本

![ss27](img/f82b947f28f356ab188792a80a5602c4.png)

### `tasklist`–显示打开的程序

你可以用这个命令做和任务管理器一样的事情:
![ss28](img/db16d30a449af4866d13274aee402ae9.png)
下一个命令告诉你如何关闭一个打开的任务。

### `taskkill`–终止正在运行的任务

要终止任务，运行`taskkill /IM "task.exe" /F`。比如`taskkill /IM "chrome.exe" /F`:


### `date`–显示和更改当前日期

![ss30](img/ca4363ac914758df1e3ddebb906ee846.png)

### `time`–显示和改变当前时间

![ss31](img/35a45f8d2bcad0e2cfd2e54de9f0ba86.png)

### `vol`–显示当前驱动器的序列号和标签信息

![ss32](img/9f6a2ae938a802e325bfa4105b720570.png)

### `dism`–运行部署映像服务管理工具

![ss33](img/277b65a49bc221fcf3ec936d02df32dd.png)

### `CTRL + C`–停止命令的执行

### `-help`–提供其他命令的指南

例如，`powercfg -help`显示了如何使用`powercfg`命令
![ss34](img/8d15f832e7eafb922c568a7046ad38b6.png)

### `echo`–显示自定义消息或来自脚本或文件的消息

![ss35](img/2c4886d135cc9b67f3c956aab632ce71.png)

您也可以使用`echo`命令创建一个语法为`echo file-content > filename.extension`的文件。


### `mkdir`–创建一个文件夹

![ss37](img/e5d66d4ab2aeb2adc757eeea141e74a7.png)

### `rmdir`–删除文件夹

![ss38](img/337e1a67b37aceccb23687f39145f07e.png)

**注意:**文件夹必须为空，此命令才能生效。

### `more`–显示文件的更多信息或内容

![ss39](img/1b1772568e06f6c9148358c2e5d92716.png)

### `move`–将文件或文件夹移动到指定的文件夹

![ss40](img/328cfcbf575bb4d9cfb74a47cdb845d7.png)

### `ren`–使用语法`ren filename.extension new-name.extension`重命名文件

![ss41-1](img/a0b211dd11052ee4b750dab16c3958a7.png)

### `cls`–清除命令行

如果您输入了几个命令，而命令行堵塞，您可以使用`cls`清除所有条目及其输出。
![cls](img/d0c6e252a464ffb2d970c7ad6b2a57d1.png)

### `exit`–关闭命令行

### `shutdown`–关闭、重启、休眠、睡眠电脑

您可以从命令行关闭、重启、休眠和睡眠您的电脑。

在命令行中输入`shutdown`,这样就可以看到可以用来执行动作的扩展。例如，shutdown /r 将重新启动计算机。
![ss42](img/f01ad686fee04d9d1aac28fe3dc71c55.png)

## 结论

本文向您展示了几个“未知对多”命令，您可以使用它们来访问 Windows PC 上的隐藏功能。

同样，使用这些命令时要小心，因为它们会对操作系统产生持久的影响。

如果您发现这些命令很有帮助，请与您的朋友和家人分享这篇文章。

如果你知道我没有列出的另一个有用的命令，[在 Twitter 上告诉我吧](https://twitter.com/Ksound22)。我会添加它，并提及您作为来源。