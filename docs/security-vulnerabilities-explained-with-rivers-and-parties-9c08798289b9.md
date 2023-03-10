# 用 Rivers 和 Parties 解释安全漏洞

> 原文：<https://www.freecodecamp.org/news/security-vulnerabilities-explained-with-rivers-and-parties-9c08798289b9/>

安德里亚·赞恩

# 用 Rivers 和 Parties 解释安全漏洞

![qnoYvN2KWQ1xhEszadE8PAuTg9FKPoAcbY1G](img/d88387734ba35aab9ec26b0a726ed346.png)

安全漏洞可能学起来很无聊。但是你仍然需要学习它们，除非你想让某个黑客删除你所有的生产数据库。为了让它更有趣一点，我试着从日常生活的角度来解释 3 个主要的弱点。所以不要再拖延了，让我们开始吧。

### 中间人攻击

当您打开网站时，您正在连接到服务器。你可以把这种联系想象成一条河，而数据(例如 Twitter 中的 Tweets)是漂浮在河下游的瓶子中的消息。

如果亚历克斯(服务员)想给你发一份晚餐邀请，他必须把它放在一个瓶子里，然后顺流而下。但是如果约翰(攻击者)把瓶子从河里拿出来，把信息改成侮辱，然后又把它放回河里呢？你将无法识别你收到的信息不是由亚历克斯发送的！

这叫做**中间人攻击**。

为了解决这个问题，你和 Alex 可以决定颠倒字符的顺序来写邮件。例如，**的秘密消息**变成了**的秘密消息**。

约翰不知道你用来生成密码的**方法**，所以他无法理解信息内容，也无法在你不注意的情况下更改信息内容。

这就是 **HTTPS** 协议所做的，只是用了一种更奇特的方法。

### DoS 和 DDoS

另一种方式可以看到服务器就像你家的收件箱。你收到邮件，阅读并回复。

如果约翰开始给你写一大堆邮件怎么办？你将无法及时回复亚历克斯的晚餐邀请，因为你将忙于回复约翰发来的所有其他垃圾邮件。

这被称为**拒绝服务攻击**，简称 DoS。

减轻这种情况的一种方法是在打开邮件之前阅读邮件顶部的发件人。如果是约翰，那就不用拆信了。这样你就不需要回复约翰，可以专注于处理严肃的事情，比如亚历克斯的晚餐邀请。

这就是 **IP 黑名单**简而言之，只有数字发送者互联网协议地址。

不幸的是，约翰说服了许多其他邪恶的人给你发垃圾邮件。所以现在你不能简单地丢弃约翰的邮件，因为有很多人给你写信。

这是一种**分布式拒绝服务(DDoS)** 并且很难对付。

处理这种情况的一种方法是只接收来自 Alex 的邮件。不幸的是，你的其他朋友不能给你写信，因为你也会丢弃他们的邮件。但是非常时期需要非常手段。但是逐渐地，你可以增加你想要接收邮件的合法用户的数量。

这被称为 **IP 白名单**，可以用来减轻 DDoS 攻击的影响，但这不是一个完美的解决方案。

DDoS 攻击很难对付，幸运的是它们也很难组织，因为你需要很多人帮助你。但是随着攻击者利用易受攻击的 IOT 设备、错误配置的服务器和 DDoS 租用服务来发起 DDoS 攻击，发起这种攻击变得非常容易。

### 注射

假设亚历克斯决定和一些朋友组织一次聚会。他准备了一份邀请函模板:

> 下周六我要举办一个派对，你要来吗？如果可能的话，带一些[此处为食品留出空白处]。汤姆

他还决定接受对食物的建议，所以他在学校的自助餐厅留了一个意见箱。然后他无意识地从每张请柬的空白处抄下一条建议。

这些是建议:

*   焦炭
*   炸薯条
*   意大利面制品
*   橘子。我还想告诉你里克是个笨蛋

你看到这是怎么回事了吗？汤姆的一个朋友会收到这条信息

> 下周六我要举办一个派对，你要来吗？如果可能的话，带些橘子来。我还想告诉你里克是个笨蛋。汤姆

汤姆的朋友会认为整个信息都是汤姆写的，包括关于里克的部分！那个留下食物建议的家伙(我想我们知道他的名字)刚刚在亚历克斯的邀请中**注入了**一条信息。

为了避免**注入**,只要简单地验证(用技术行话来说**转义**)你从用户那里接受的东西不是来自可信的来源。

### 在你离开之前

如果你的名字是约翰，我欠你一个道歉，但留下来，我保证在下一篇文章中你会是好的。

我希望你喜欢这篇文章。别忘了你可以？最多 50 次！