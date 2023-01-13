# 如何使用 Ping 命令识别基本的互联网问题

> 原文：<https://www.freecodecamp.org/news/how-to-identify-basic-internet-problems-with-ping/>

下次你给服务台打电话时，你想用你的网络知识让他们惊叹吗？使用您现有的 Mac、Windows 或 Linux 计算机中内置的名为“ping”的命令，将有助于确定基本的连接问题。

好吧，这可能不足以让你的团队成员惊叹，但是他们会感谢你开始了调试过程。请记住，您的支持人员是调试专家，因此在他们指导您完成故障诊断序列时，请遵循他们的指示。

## **TL；博士:**

您可以使用 Mac OS X、Windows 或 Linux 电脑内建的`ping`命令来确定基本的网络连接问题。这可以帮助您解决问题和/或获得有价值的调试信息，作为致电支持之前的第一步。

阅读下面关于如何从你的 Mac OS X 或 Windows 机器启动命令行窗口和运行`ping`的详细信息。

## **`ping`命令:**

`ping`命令是验证另一台计算机可以接收来自您的信息的简单方法。最初的作者[迈克·穆斯](https://en.wikipedia.org/wiki/Mike_Muuss)，实际上[以潜水艇发出的探测水中物体的“乒”声](https://en.wikipedia.org/wiki/Ping_%28networking_utility%29#History)命名了这个程序。如果 ping 的回声回来，这意味着有东西在那里。事实上，`ping`使用了“[互联网控制消息协议回应请求](https://en.wikipedia.org/wiki/Internet_Control_Message_Protocol)”作为其底层软件设计的一部分。

在其最简单的形式中，`ping`命令提供了两条有价值的信息，即消息是否被回显(`64 bytes from…`)以及接收回消息需要多长时间(例如`time=6.396 ms`)。

根据您使用的电脑类型，您甚至可以获得包含最小值、最大值、平均值等信息的摘要。

响应时间以“毫秒”显示，即 1/1000 秒。10ms 或更短的响应时间非常快，但是值通常在 100ms 范围内。在 200 毫秒以上，你可能会注意到你有一个缓慢的连接。

## **一切正常时:**

这是我的 Mac OS X 电脑在马拉西亚一切正常时的反应:

```
MacBook-Pro:~ ajm$ ping Google.com
PING google.com (216.58.196.46): 56 data bytes
64 bytes from 216.58.196.46: icmp\_seq=0 ttl=55 time=6.396 ms
64 bytes from 216.58.196.46: icmp\_seq=1 ttl=55 time=6.368 ms
64 bytes from 216.58.196.46: icmp\_seq=2 ttl=55 time=26.773 ms
64 bytes from 216.58.196.46: icmp\_seq=3 ttl=55 time=6.984 ms
^C
--- google.com ping statistics ---
4 packets transmitted, 4 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 6.368/11.630/26.773/8.746 ms
```

当一切正常时，我在 Windows 电脑上的反应是这样的:

```
C:\Users\BJM>ping Google.com
Pinging google.com [216.58.196.46] with 32 bytes of data:
Reply from 216.58.196.46: bytes=32 time=6ms TTL=128
Reply from 216.58.196.46: bytes=32 time=15ms TTL=128
Reply from 216.58.196.46: bytes=32 time=6ms TTL=128
Reply from 216.58.196.46: bytes=32 time=6ms TTL=128
Ping statistics for 216.58.196.46:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 6ms, Maximum = 15ms, Average = 8ms
```

从这些例子中可以看出，连接非常好，平均响应时间不到 10 毫秒。

### **出问题时(三个例子):**

如果我无法连接到`Google.com`会发生什么？例如#1，我从墙上拔下我的路由器，模拟到我的 Mac 的断开的网络连接，并重新运行命令。我注意到的第一件事是，命令响应的时间要长得多(T2 ):

```
MacBook-Pro:~ ajm$ ping google.com
ping: cannot resolve google.com: Unknown host
MacBook-Pro:~ ajm$
```

或者，例如#2，根据连接失败的确切原因:

```
PING google.com (216.58.196.46): 56 data bytes
Request timeout for icmp\_seq 0
Request timeout for icmp\_seq 1
Request timeout for icmp\_seq 2
^C
```

有时，如果我有一个特别古怪的连接，我会看到这些信息的混合。例如#3，我可以通过将我的 Mac 电脑连接到街对面的公共 Wi-Fi 连接来模拟这种情况:

```
PING google.com (216.58.196.206): 56 data bytes
64 bytes from 216.58.196.206: icmp\_seq=0 ttl=57 time=273.655 ms
64 bytes from 216.58.196.206: icmp\_seq=1 ttl=57 time=808.546 ms
64 bytes from 216.58.196.206: icmp\_seq=2 ttl=57 time=179.613 ms
Request timeout for icmp\_seq 3
Request timeout for icmp\_seq 4
64 bytes from 216.58.196.206: icmp\_seq=5 ttl=57 time=374.612 ms
Request timeout for icmp\_seq 6
ping: sendto: No route to host
Request timeout for icmp\_seq 7
ping: sendto: No route to host
Request timeout for icmp\_seq 8
^C
```

在第一次测试中，`ping`告诉我，我的机器甚至找不到`Google.com`的互联网地址(IP `216.58.196.46`)。在第二次测试中，我的电脑记住了谷歌的 IP 地址，但实际上无法访问谷歌服务器(`Request timeout`)。在第三个测试中，`sendto: No route to host`意味着网络设备知道谷歌服务器在哪里，但数字路径上的某些东西被破坏了。

## **Mac 用户:如何运行`ping`命令:**

在 Mac 上，你通常从终端命令行运行`ping`。要启动终端，请点按桌面右上角的 OS X 聚光灯放大镜图标:

![Mac Spotlight](img/504c95be83997100eaaa727005f8b4f6.png)

当搜索窗口出现时，键入“终端”，突出显示“终端-实用程序”，然后双击(或点击

返回

):

![Mac Terminal Launch](img/775c38ce249e7501d3bceb546be3ff88.png)

这将启动终端命令窗口，您可以输入命令`ping Google.com`,如我的示例所示:

![Mac Command Line](img/12f2a645660f123e7fc046b2ddb1518d.png)

****重要 Mac 提示**** :如果你不告诉`ping`命令停止，它将永远运行。为此，请按

`control`

键(键盘右下角)和

`c`

钥匙。这将使用 Control-C ( `^C`)中断测试，并返回命令行控制。对于 Windows 用户，该命令将在几次迭代后自行停止。

## **Windows 用户:如何运行`ping`命令:**

在 Windows 版本 10、8.1、8 和 7 之间，打开命令提示符的方式有所不同。这里有一个关于如何打开命令提示符的很好的指南。例如，在 Windows 7 机器上，点击 Windows 左下角的“开始”图标，选择“命令提示符”并双击(或点击

`enter`

):

![Win Terminal Launch](img/f38ba21205fc686c95d674f7eab3aaca.png)

这将启动命令窗口，您可以输入示例中所示的命令`ping Google.com`:

![Win Command Line](img/bc90e042a127a1c84cde91e1aecc8b21.png)

现在您已经知道如何使用`ping`命令，您可以对您的网络连接进行基本的故障诊断。只要有一点创意，您就可以与当地的 it 支持人员或了解您的网络拓扑和 IP 地址的人员(例如，`ping`路由器，`ping`您的 ISP)合作，进一步确定网络问题。