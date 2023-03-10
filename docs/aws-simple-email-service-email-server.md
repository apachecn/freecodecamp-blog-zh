# 如何使用亚马逊简单电子邮件服务(SES)来取代基于服务器的电子邮件服务器

> 原文：<https://www.freecodecamp.org/news/aws-simple-email-service-email-server/>

一个晴朗的日子，不知什么原因，我的 Ubuntu 18.04 [商务服务器](https://bootstrap-it.com)停止向我的 g mail 地址转发邮件。

就在前一天。转发我在本地服务器帐户的主目录中创建的文件，我使用这些帐户作为 email - like /home/office/。转发——我们兴高采烈地把所有发往我公司地址的邮件都转到了我日常使用的 Gmail 账户。然后他们突然停了下来。

当我发现有问题时，我立即查看我的服务器日志。/var/log/mail.err 发出了迷人的信息，其中包括:

```
status=deferred (delivery temporarily suspended: connect to alt2.gmail-smtp-in.l.google.com[219.8.202.27]:25: Connection timed out)
```

检查服务器邮箱告诉我有邮件进来，但 Postfix 无法与 Gmail 建立连接，无法将邮件转发到我的地址。

很自然地，我重启了 Postfix，但这并没有帮助。

```
sudo systemctl restart postfix
```

我确认没有任何东西阻止邮件从我的服务器的端口 25 (SMTP)发出。然后我检查以确保我的域名没有被列入黑名单(有很多在线工具可以帮你做到这一点)，并通过从命令行运行 dig 来查看我的 MX 记录的状态:

```
dig MX bootstrap-it.com
```

什么都没做。一切似乎都没问题。

经过几次令人沮丧的故障排除会议后，我放弃了，并认为我应该尝试一些完全不同的东西。

作为一名 AWS 解决方案架构师，并且已经为 Wiley/Sybex 合著了两本关于 AWS 的书(一本是云从业者考试指南(T1 ),另一本是 T2 解决方案架构师助理考试(T3 ))),难道我不应该愿意并且能够构建自己的 AWS 工具堆栈来满足我在云中的电子邮件服务器需求吗？

事实证明，我既愿意，也——经过一些认真的研究和反复试验——能够。完成这项工作需要:

*   创建一个 S3 桶来存储收到的电子邮件。
*   创建一个简单的通知服务(SNS)主题，以便在每次有新邮件时向我发送通知。
*   配置亚马逊的简单电子邮件服务(SES)来接管我的电子邮件域(bootstrap-it.com)并处理收到的邮件。这涉及到将 MX 记录添加到 Route 53(管理我的域的地方),并将 se 指向我的域；添加并验证我希望 SES 控制的每个电子邮件地址；然后告诉 SES 向我的 S3 桶发送新消息，同时触发 SNS 主题的警报。
*   假设您还想通过该服务发送电子邮件，那么配置 SES 使用域名密钥识别邮件(DKIM)对您的外发邮件进行签名也是一个好主意。

我不打算在这里详细描述所有这些步骤。有很多优秀的文档可以用来做这件事。但是我将简要地提到一些您可能会遇到的棘手问题。

您必须为您使用的每个域向 DNS 托管区域添加 MX 记录。即使您的域是在 Amazon 的 Route 53 中管理的，您也需要为您的记录提供一个值。

您使用什么作为该值将取决于您的 SES 资源所在的 AWS 区域。在我的例子中，它看起来像这样:

```
10 inbound-smtp.us-east-1.amazonaws.com
```

SNS 通知将以一长串文本的形式到达，其中只包含一些有用但难以阅读的简短信息。这将足以识别垃圾邮件，但你通常需要更多的信息比你会在这里找到。我用通知作为提醒，告诉我在我的 S3 桶里有新邮件。

如果一个月只查看一两次，通过 AWS 管理控制台在你的 S3 桶中查看电子邮件本身并不是世界末日。但是如果他们的速度比你快，你就需要找到一种更好的方式来访问和阅读你的信息。

然而，创建一个自动化该过程的协议实际上是一个本地操作系统问题，需要一套完全不同的工具。我自己用 AWS CLI 和一个很酷的 Bash 脚本解决了这个问题。如果你想知道我是如何做到的，[点击这篇文章](https://www.freecodecamp.org/news/bash-script-download-view-from-s3-bucket/)。

在我的[bootstrap-it.com](https://bootstrap-it.com)有更多关于管理的书籍、课程和文章。