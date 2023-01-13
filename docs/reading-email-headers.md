# 邮件标题里有什么，你为什么要关心？

> 原文：<https://www.freecodecamp.org/news/reading-email-headers/>

您是否收到过来自陌生电子邮件地址的垃圾邮件或网络钓鱼邮件？也许有人为你提供了一次免费旅行，让你给他们发送比特币以换取个人照片，或者只是给你发了一封不想要的营销邮件？

你想知道那些邮件是从哪里来的吗？看到垃圾邮件发送者欺骗您的电子邮件地址，想知道他们是如何做到的？

电子邮件欺骗，或者使电子邮件看起来好像来自不同的地址(例如，看起来来自 whitehouse.gov，但实际上是来自骗子)是非常容易的。

核心电子邮件协议没有任何认证方法，这意味着“发件人”地址基本上只是一个填空。

通常当你收到一封邮件时，它看起来像这样:

```
From: Name <name@gmail.com>
Date: Tuesday, July 16, 2019 at 10:02 AM
To: Me <Me@freecodecamp.com>
```

下面是主题和信息。

但是你怎么知道那封邮件真正来自哪里呢？难道没有额外的数据可以分析吗？

我们正在寻找的是完整的电子邮件标题-你在上面看到的只是部分标题。这些数据将为我们提供一些额外的信息，比如邮件来自哪里，如何到达你的收件箱。

如果你想查看自己的邮件标题，下面是如何在 [Outlook](https://support.office.com/en-us/article/view-internet-message-headers-in-outlook-cd039382-dc6e-4264-ac74-c048563d212c) 和 [Gmail](https://support.google.com/mail/answer/29436?hl=en) 上查看它们的方法。大多数邮件程序都以类似的方式运行，简单的谷歌搜索会告诉你如何查看其他邮件服务的标题。

在本文中，我们将看到一组真实的头(尽管它们被大量编辑——我已经更改了主机名、时间戳和 IP 地址)。

我们将从上到下阅读邮件头，但是请注意，每个新服务器都会将其邮件头添加到邮件正文的顶部。这意味着我们将从最终消息传输代理(MTA)读取每个报头，并向下工作到第一个接受消息的 MTA。

## 内部转移

```
Received: from REDACTED.outlook.com (IPv6 Address) by REDACTED.outlook.com with HTTPS via REDACTED.OUTLOOK.COM; Fri, 25 Oct 2019 20:16:39 +0000
```

第一跳显示了一条 HTTPS 线，这意味着服务器没有通过标准的 SMTP 接收消息，而是通过在 web 应用程序上接收的输入创建消息。

```
Received: from REDACTED.outlook.com (IPv6Address) by REDACTED.outlook.com (IPv6Address) with Microsoft SMTP Server (version=TLS1_2, cipher=TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384) id 15.1.1358.20; Fri, 25 Oct 2019 20:16:38 +0000

Received: from REDACTED.outlook.com (IPv6Address) by REDACTED.outlook.office365.com (IPv6Address) with Microsoft SMTP Server (version=TLS1_2, cipher=TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384) id 15.20.2385.20 via Frontend Transport; Fri, 25 Oct 2019 20:16:37 +0000 Authentication-Results: spf=softfail (sender IP is REDACTEDIP)smtp.mailfrom=gmail.com; privatedomain.com; dkim=pass (signature was verified)header.d=gmail.com;privatedomain.com; dmarc=pass action=noneheader.from=gmail.com;compauth=pass reason=100Received-SPF: SoftFail (REDACTED.outlook.com: domain of transitioning gmail.com discourages use of IPAddress as permitted sender)
```

前两个标题块是内部邮件传输。您可以知道这些邮件是由 Office365 服务器(outlook.com)接收的，并在内部路由到正确的收件人。

您还可以知道邮件是通过加密的 SMTP 发送的。您知道这一点是因为标题列出了“with Microsoft SMTP Server ”,然后指定了它正在使用的 TLS 版本以及特定的密码。

第三个标题块标志着从本地邮件服务器到邮件过滤服务的转变。您知道这一点是因为它是“通过前端传输”的，这是一个特定于 Microsoft-Exchange 的协议(因此它不是严格的 SMTP)。

该块还包括一些电子邮件检查。Outlook.com 的标题在这里详细介绍了他们的 SPF/DKIM/DMARC 结果。SPF 软故障意味着该 IP 地址无权代表 gmail.com 发送电子邮件。

“dkim=pass”意味着电子邮件来自其声称的发件人，并且(很可能)在传输过程中没有被更改。

DMARC 是一组规则，告诉邮件服务器如何解释 SPF 和 DKIM 结果。通过可能意味着电子邮件继续发送到其目的地。

关于 SPF、DKIM 和 DMARC 的更多信息，请查看本文。

## 内部/外部过渡

```
Received: from Redacted.localdomain.com (IP address) byredacted.outlook.com (IP address) with Microsoft SMTPServer (version=TLS1_2, cipher=TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384) id15.20.2305.15 via Frontend Transport; Fri, 25 Oct 2019 20:16:37 +0000

Received-SPF: None (Redacted.localdomain.com: no senderauthenticity information available from domain ofsender@gmail.com) identity=xxx; client-ip=IPaddress;receiver=Redacted.localdomain.com;envelope-from="sender@gmail.com";x-sender="sender@gmail.com"; x-conformance=sidf_compatible

Received-SPF: Pass (Redacted.localdomain.com: domain ofsender@gmail.com designates sending IP as permittedsender) identity=mailfrom; client-ip=IPaddress2;receiver=Redacted.localdomain.com;envelope-from="sender@gmail.com";x-sender="sender@gmail.com"; x-conformance=sidf_compatible;x-record-type="v=spf1"; x-record-text="v=spf1ip4:35.190.247.0/24 ip4:64.233.160.0/19 ip4:66.102.0.0/20ip4:66.249.80.0/20 ip4:72.14.192.0/18 ip4:74.125.0.0/16ip4:108.177.8.0/21 ip4:173.194.0.0/16 ip4:209.85.128.0/17ip4:216.58.192.0/19 ip4:216.239.32.0/19 ~all"
```

这是谷歌的 SPF 记录——告诉接收服务器这封来自 gmail.com 的邮件来自谷歌认可的服务器。

```
Received-SPF: None (redacted.localdomain.com: no senderauthenticity information available from domain ofpostmaster@redatedgoogle.com) identity=helo;client-ip=IPaddress; receiver=Redacted.localdomain.com;envelope-from="sender@gmail.com";x-sender="postmaster@.google.com";x-conformance=sidf_compatibleAuthentication-Results-Original: Redacted@localdomain.com; spf=Nonesmtp.pra=sender@gmail.com; spf=Pass smtp.mailfrom=sender@gmail.com;spf=None smtp.helo=postmaster@redacted.google.com; dkim=pass (signatureverified) header.i=@gmail.com; dmarc=pass (p=none dis=none) d=gmail.comIronPort-SDR: IronPort-PHdr: =X-IronPort-Anti-Spam-Filtered: trueX-IronPort-Anti-Spam-Result: =X-IronPort-AV: ;d="scan"X-Amp-Result: SKIPPED(no attachment in message)X-Amp-File-Uploaded: False
```

这显示了一些额外的 SPF/DKIM/DMARC 检查，以及来自 IronPort 扫描的结果。

Ironport 是一种流行的电子邮件过滤器，许多公司使用它来查找垃圾邮件、病毒和其他恶意电子邮件。它扫描电子邮件中的链接和附件，并确定电子邮件是否是恶意的(并且应该被丢弃)，它是否可能是合法的并且应该被发送，或者它是否是可疑的(在这种情况下，它可以在正文中附加一个标题，告诉用户要警惕该电子邮件。

```
Received: from redacted.google.com ([IPAddress])by Redacted.localdomain.com with ESMTP/TLS/ECDHE-RSA-AES128-GCM-SHA256; Fri, 25 Oct 2019 16:16:36 -0400

Received: by redacted.google.com with SMTP idfor recipient@localdomain.com; Fri, 25 Oct 2019 13:16:35 -0700 (PDT)

X-Received: by IPv6:: with SMTP id; Fri, 25 Oct 2019 13:16:35 -0700 (PDT) Return-Path: sender@gmail.com

Received: from senderssmacbook.fios-router.home (pool-.nycmny.fios.verizon.net. [IP address redacted])by smtp.gmail.com with ESMTPSA id redacted IP(version=TLS1 cipher=ECDHE-RSA-AES128-SHA bits=128/128);Fri, 25 Oct 2019 13:16:34 -0700 (PDT)

Received: from senderssmacbook.fios-router.home (pool-.nycmny.fios.verizon.net. [IP address redacted])by smtp.gmail.com with ESMTPSA id redacted IP(version=TLS1 cipher=ECDHE-RSA-AES128-SHA bits=128/128);Fri, 25 Oct 2019 13:16:34 -0700 (PDT)
```

此部分显示了电子邮件从发件人的初始设备通过 gmail 的路由系统到达收件人的 outlook 环境的内部中继。从这里我们可以看到，最初的发送者来自一台 Macbook，使用的是家用路由器，威瑞森·菲奥斯在纽约。

这是显示电子邮件从发件人到收件人的路径的跃点的终点。除此之外，你会看到邮件的正文(以及你通常看到的标题，如“发件人:”和“收件人”等。)，可能带有一些基于媒体类型和电子邮件客户端的格式(例如 MIME 版本、内容类型、边界等)。它还可能包含一些用户代理信息，即发送消息的设备类型的详细信息。

在这种情况下，我们已经知道发送设备是 Macbook，因为苹果公司的命名惯例，但它也可能包含 CPU 类型、版本的详细信息，甚至是设备上安装的浏览器和版本。

在某些情况下，但不是所有情况下，它可能还包含发送设备的 IP 地址(尽管许多提供商会在没有传票的情况下隐藏该信息)。

## 邮件标题能告诉你什么？

电子邮件标题有助于识别电子邮件何时不是从其声称的发件人发出的。它们可以提供发送者的一些信息——尽管这通常不足以确定真正的发送者。

执法部门通常可以使用这些数据从正确的 ISP 处传唤信息，但我们其他人大多只能使用这些数据来帮助通知调查，通常是网络钓鱼。

恶意服务器或黑客可以伪造报头，这使得这个过程变得更加困难。如果不联系每台服务器的所有者并单独验证您的电子邮件中的邮件头是否与它们的 SMTP 日志匹配(这是一项艰苦且耗时的工作)，您将无法确定邮件头是否准确(除了您自己的邮件服务器附加的邮件头)。

如果不联系每台服务器的所有者，并单独验证电子邮件中的邮件头是否与它们的 SMTP 日志相匹配(这是一项艰苦而耗时的工作),您将无法确定邮件头是否都是准确的..

DKIM、DMARC 和 SPF 都可以帮助这个过程，但它们并不完美，没有它们，根本就没有验证。

不想分析自己的头？[这个网站](https://mailheader.org/)会帮你做到。