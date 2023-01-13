# 为了乐趣和利益而入侵 Imgur

> 原文：<https://www.freecodecamp.org/news/hacking-imgur-for-fun-and-profit-3b2ec30c9463/>

作者内森

# 为了乐趣和利益而入侵 Imgur

所以你认为你的迷因是安全的…

![R2XhuRff6Qm04ONevEE3yQWiCljMUNY7irD-](img/fa45fdf6d960a4b3af5780b29a544ed3.png)

Sorry not sorry.

我早就想写这个了。这一切都要追溯到 2015 年 7 月，当时我决定在一个非常受欢迎的图片分享平台 [Imgur](http://imgur.com/) 中寻找漏洞。

我选择 Imgur 的原因是因为我经常访问该网站，并且我已经熟悉了该网站的工作方式。快速调查后，我设法找出了一些常见的漏洞:XSS、点击劫持和一大堆 CSRF 问题。

事实证明，报告这些问题有点困难。我能看到的唯一联系 Imgur 的方法是通过他们的支持系统，这个系统不适合报告安全问题。最终，在 8 月 1 日，我写了一份详细说明这些问题的报告，给 security@imgur.com 发了一封电子邮件，然后等待。但不会太久。

几个小时后，我收到了 Imgur 创始人兼首席执行官艾伦·沙夫(Alan Schaaf)的电子邮件，感谢我并给了我一些奖品作为奖励。在接下来的几天里，我继续向他们转发我发现的漏洞报告，其中一个后来让我赚了 5000 多美元。

事情平静了一段时间。我偶尔会检查是否有任何问题得到了解决，果然，很多问题都得到了解决。很高兴知道他们正在认真对待这个问题，并且我正在帮助保护这个平台。

然后，在九月下旬， [Imgur 被黑客攻击](https://www.reddit.com/r/technology/comments/3lw2g6/imgur_is_being_used_to_create_a_botnet_and_ddos/)。有人能够将带有恶意 Javascript 的 HTML 文件上传到 Imgur，并进而攻击 8chan 用户。

这个问题很快就被解决了，然而却被广泛报道，显然这是 Imgur 不希望发生的事情。作为回应，Imgur 终于在一周后推出了他们的[臭虫奖励计划](http://blog.imgur.com/2015/09/30/imgurs-security-bug-bounty-program/)。

这是个好消息，因为现在人们有了安全报告问题的方式。我再次联系艾伦，询问我的报告是否有资格获得奖金，他让我联系 Imgur 的一位开发人员，他确认我有资格，并让我创建一份关于 [HackerOne](https://hackerone.com/imgur) 的报告。对于我报告的所有漏洞——总共 20 多个——我的奖励是 50 美元。原因是“在大多数情况下，我们为 CSRF 报告提供赠品，所以你是这里的例外”。

在 Imgur 创建他们的 bug bounty 程序之前，Imgur 就被 CSRF 漏洞弄得千疮百孔。现在已经修复的一些功能包括:更新用户个人资料的功能，为给定的 [Imgur 应用程序生成新的客户端密码的功能，](https://imgur.com/account/settings/apps)以及更改给定 Imgur 应用程序的重定向 URL 的功能。这些是我最初报告的一些问题，所以只得到 50 美元的奖励是相当令人沮丧的。

然而，开发者很清楚:这个程序是新的，他们仍在学习和尝试改进它。考虑到这一点，我给了他一份我报告的问题清单，他确认报告中的问题比他意识到的要多得多。

在讨论问题和获得额外奖励之间的几周里，我发现了一些有趣的事情。

在我最初的研究中，我浏览了 Imgur 的 HTML 源代码，发现了以下代码片段:

```
$(function() { if(!/^([^:]+:)\/\/([^.]+\.)*imgur(-dev)?\.com(\/.*)?$/.test(document.referrer)) { Imgur.Util.jafoLog({ event: ‘galleryDisplay’, meta: { gallerySort: ‘viral’, galleryType: ‘hot’ }}); } });
```

这让我对 imgur-dev.com 进行了一些侦察，这是 Imgur 用于内部开发的领域。谷歌快速搜索 site:imgur-dev.com，发现谷歌已经将包含 imgur 开发版的子域“alan.imgur-dev.com”编入索引。然而，服务器当时关闭了。我可以查看页面的缓存副本，其中显示了一些有趣的信息，包括由 [Kohana 的 profiler](https://kohanaframework.org/3.1/guide/kohana/profiling) 输出的 SQL 查询中的数据库表和列名。

转眼到了 11 月，我决定再次看看 imgur-dev.com。嘣！alan.imgur-dev.com 充满活力，平易近人。我发现了什么？它本质上是 Imgur，正如你所知，只有几个用户和一些测试帖子。值得注意的是，这是一个开发者环境——我可以看到 stacktraces，其中包括 Imgur 的部分源代码、PHP 警告和通知、关于环境的详细信息、数据库查询和配置文件的完整路径。

有了这些细节，我在 HackerOne 上创建了另一个报告，并等待响应。两天后，我又看了一眼 alan.imgur-dev.com。这次我决定在 alan.imgur-dev.com 使用[子域名](https://github.com/TheRook/subbrute)看看是否有任何感兴趣的子域名。

开始扫描后不久，我找到了一个匹配:es.alan.imgur-dev.com。快速浏览后发现这是一个 Elasticsearch 服务器，已经有一段时间没有更新了。事实上，在搜索了一个弹性搜索漏洞后，我发现了[CVE-2014–3120](https://www.cve.mitre.org/cgi-bin/cvename.cgi?name=2014-3120)。

```
The default configuration in Elasticsearch before 1.2 enables dynamic scripting, which allows remote attackers to execute arbitrary MVEL expressions and Java code via the source parameter to _search.
```

很自然地，我抓起一个概念验证脚本，试着阅读/etc/passwd。答对了。文件读取成功。现在，安全的做法是报告问题并继续前进。这次不会。Imgur 正处于提供 50 美元奖励和赃物的阶段。我对此不满意。我决定更进一步，看看如果我想的话，我能有多糟糕。

我之前注意到的一个配置文件叫做“keys.php”。我决定试着像读/etc/passwd 一样读它，又一次成功了。这份文件包含了我所需要的一切。Fastly 和 MailChimp 的 API 键，移动应用 API 键，甚至 reCAPTCHA API 键。太好了。但这还不是全部…该文件还包含用于连接本地和远程 MySQL 服务器的凭证。哎呦。

现在的问题是，我不知道远程 MySQL 服务器的主机名，我不能远程连接到本地服务器。做什么？

我回去扫描子域名，又找到了一个。sql.alan.imgur-dev.com。哦，是的，你知道将要发生什么…

我访问了域名，受到了 phpMyAdmin 的问候。现在，通常你只需要输入用户名和密码就可以登录。幸运的是，我可以选择要连接的主机。一个是本地服务器，另一个是托管在 Amazon AWS 上的远程服务器。我选择了远程服务器，输入了凭证，并且拥有 Imgur 生产数据库的完全访问权限。

好了，现在你知道了，写起来很有趣…哦，你以为就这样了？还没完呢，伙计们。

配置文件还包含两组 Amazon [AWS 访问密钥](https://aws.amazon.com/developers/access-keys/)——一组用于开发，一组用于生产。那么我接下来做了什么？

没什么。

虽然我知道我可以使用这些密钥来启动新的服务器，SSH 到现有的服务器，或者完全摧毁它们，但我决定甚至不测试它们是否工作。

安全研究的一个重要部分是知道何时停止。

我已经证明了这个问题有多严重，并展示了恶意攻击者可以做些什么，同时又不至于过于粗心或侵扰。

这个问题需要尽快解决。我用我的新发现更新了 HackerOne 的报告，并紧急给艾伦发了电子邮件提醒他注意。不到半小时，服务器就离线了。第二天早上，艾伦回复我说，在他们想出下一步措施的时候，对服务器的访问被禁用了。他们的处理方式给我留下了难以置信的印象。

```
A security group configuration error allowed Imgur development environments to face the public internet. Typically these environments were protected behind a special endpoint which would open access to authenticated Imgur employees for a short time window. Since the development environments were configured in such a manner to make development easier, some keys and environment variables were exposed. While most of these pieces of sensitive information were limited to the development environments, some production information was also exposed. Since this report was published, security around development environments has been completely re-worked and they now reside behind a VPN.
```

12 月初，由于我报告的多个漏洞和高风险问题，我获得了 500 美元的奖励。不用说，我没有被打动。我决定给艾伦写一封电子邮件，给出我到目前为止对这个项目的反馈，并解释为什么 500 美元不够。

```
Bug bounties are like hiring mercenaries. Cheaper than a standing army of pentesters. But don't complain when they swap sides for more gold.
```

一周后，艾伦回复了，同意我提出的观点，并表示团队将很快就该计划召开会议。我很高兴听到我的反馈得到了重视。有时，直接与首席执行官交谈会有助于真正的变革。

再一次回到 2016 年 1 月，艾伦给我发了最后一封邮件。

```
Hey Nathan,Happy new year! I was finally able to sync up with my team and come to a conclusion on this. Since your exploit went above and beyond (contained several exploits all chained together, access to production data, etc), we want to go above and beyond for you too, and have agreed to offer you an additional $5,000\. This is so much higher than anything we've ever offered before, but again, this exploit was so much higher than anything that hasbeen found before, so I think it's deserving and I hope it's sufficient for you.Thanks so much for protecting us and properly reporting it to us. Best of luck in the new year.Best,Alan
```

这是一个非常好的消息。Imgur 不仅改变了他们的 bug 奖金计划，公平地支付给研究人员，额外的 5000 美元也极大地帮助了我和其他人。其中一半给了需要帮助的人，包括 [Lauri Love、](https://freelauri.com/)一名面临被引渡到美国的黑客，以及一位最近无家可归的密友。各种慈善机构和研究人员也从中受益。

我一直在参与 Imgur 的 bug bounty 计划，虽然它并不完美，但它对我自己和其他人都有很好的回应和回报。我希望其他团队可以从 Imgur 乐于接受反馈和改进中学习，因为围绕安全性的交流非常重要。

我的下一步是什么？我希望继续学习和努力，为子孙后代创造一个更安全的互联网环境。与 10 年后相比，互联网很小。战斗还没有结束，战争还在后头。

如果你正在寻找挑战，我强烈推荐你去看看 Imgur 的 bug bounty 程序和其他在 [HackerOne](https://hackerone.com/imgur) 上的程序。

你可以在推特上关注我，我在推特上谈论安全、糟糕的英国天气和迷因。是啊。你知道你想… :-)