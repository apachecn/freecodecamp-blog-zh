# 如何开始使用 Curl 以及为什么:动手介绍

> 原文：<https://www.freecodecamp.org/news/how-to-start-using-curl-and-why-a-hands-on-introduction-ea1c913caaaa/>

卢西亚诺·斯特里卡

# 如何开始使用 Curl 以及为什么:动手介绍

![7T-1Ovw5prQKYXj9z1kBWfRLv9xXDmRLax9T](img/15bcc403781780a3ebc048dcff7316a4.png)

A beautiful beast for a beautiful program. Source: [Pixabay](https://pixabay.com/en/animal-fox-canine-cold-cute-1850186/)

无论是在将 API 部署到生产环境之前测试它的输出，还是简单地从网站获取响应(例如，检查它是否关闭)，Curl 几乎无处不在。

作为一名数据科学家，我不得不不时使用它。然而，大多数情况下，我最终只是替换了在我团队的 Slack 通道中复制粘贴的 curl 命令的参数。

我决定，如果我想充分发挥它的潜力，我需要更好地理解这个强大的工具，现在我在这里分享我在这个 curl 教程中发现的一些最有趣的事情。

如果你有任何想要添加的提示或技巧，请在评论中提出来，因为我对这个工具的理解还处于早期阶段。

### Curl:有什么好处？

Curl 是一个命令行工具，它允许我们从 shell 发出 HTTP 请求。它还涵盖了许多其他协议，如 FTP，尽管它们超出了本教程的范围。

它的名字[代表“客户端 URL”](https://en.wikipedia.org/wiki/CURL)，由瑞典开发者 Daniel Stenberg 开发。这是一个开源项目，如果你想做贡献，你可以在[这里](https://github.com/curl/curl)找到它的代码。

您可以从您最喜欢的终端调用它，它通常预装在基于 Linux 的操作系统中。否则在 Linux 上可以通过 *apt-get* 正常下载，在 Mac 上可以通过 *brew* 下载。

### 调用 GET 方法

在其最基本的形式中，curl 命令将如下所示:

```
curl http://www.dataden.tech
```

curl 的默认行为是在给定的 url 上调用 HTTP GET 方法。这样，该命令的程序输出将是站点在 GET 上返回的整个 HTTP 响应的主体(在本例中是 HTML ),它将被写成在 *stdout* 上给出的形式。

如果您希望在不离开 shell 的情况下通读一个响应，我建议至少将它放到一个少于*的*命令中，以便能够轻松地滚动输出。

很多时候，我们希望将响应的内容定向到一个文件中。这是通过 *-o* 参数完成的，就像这样:

```
curl -o output.html www.dataden.tech
```

这相当于:

```
curl www.dataden.tech > output.html
```

或者，您可以用一个 *-s* 参数指定您希望调用 curl 的站点的 URL，如下所示:

```
curl -s http://www.dataden.tech
```

允许你改变论点的顺序。

您还可以使用*–next*来指定多个 url，尽管官方文档建议在不同的命令中对每个 URL 调用 curl。

### 向 URL 发布内容

有时你想测试一个 API 是否工作正常，通常需要向它发送参数。

我们通常通过 POST 方法来实现，传递一些带有所有必需参数的 JSON。使用 curl 有很多方法可以做到这一点。

您可以像这样传递参数值:

```
curl --data "name=John&surname=Doe" http://www.dataden.tech
```

或者像普通的 JSON 一样:

```
curl --data '{"name":"John","surname":"Doe"}' \http://www.dataden.tech
```

使用*–数据*等同于使用 *-d，*，两者都会使方法自动变为 POST。然而，我们也可以使用 *-X* 标志(*–请求*)来指定我们想要调用哪个方法:

```
curl -X "POST" \-d "name=John&surname=Doe" http://www.example.com
```

### 获取网站的标题

有时候，我们只需要快速查看网站是否还在运行，而不是真的想要加载整个潜在的大量响应。其他时候，头存储重要的配置。

curl 也解决了这两个用例。我们可以使用*–include*(*-I*)参数来包含头部，使用*–head*(*-I*-那是大写的‘I’-)来只包含头部(调用 HEAD 方法)。

### 设置您的用户代理值

现在我已经介绍了基础知识，让我带你看一些我发现的可以用 curl 做的最酷的事情。

*用户代理*参数让你指定你正在使用的设备和浏览器版本，以防使站点呈现不同。
有了这个，你可以在笔记本电脑上看到网站的移动版，反之亦然。

从安全角度来看，这可能会引发一些问题。直到现在，我才知道假装使用不同的设备(甚至不使用虚拟机)是多么容易，而在反欺诈工作中，我明白了为什么这可能是一个问题。

也就是说，只要你把它用在好的地方，这是一种从平板电脑、移动设备或笔记本电脑上查看网站外观的绝佳方式。

这里有一个直接来自官方文档的例子(尽管用户代理列表在网上很容易找到)。

```
curl --user-agent "Mozilla/4.73 [en] (X11; U; Linux 2.2.15 i686)" www.example.com
```

### 用 Curl 计时连接

我开始了解 curl 的另一个原因是，我想知道我的网站到底需要多长时间来响应。

虽然基本文档没有涉及到它，但是稍微搜索一下就发现了这个命令，我发现它非常有用:

```
curl -w "%{time_total}\n" -o /dev/null -s www.example.com
```

这将简单地输出从给定域获取响应所花费的总时间。

更一般地说，*-w(–write-out)*参数接受一个特殊的格式化字符串，并以格式化的方式用响应的不同属性填充保留的关键字。所有关键字及其各自的值都可以在[命令的手册页](https://curl.haxx.se/docs/manpage.html)中找到。

### 进一步阅读

如果你想了解这个广泛的主题，这里有一些你可能会感兴趣的链接:

*   [用户代理列表](https://developers.whatismybrowser.com/useragents/explore/)不同设备和浏览器的用户代理参数汇编。
*   Curl 的[官方文档](https://curl.haxx.se/docs/httpscripting.html)。
*   Curl 的[手册页](https://curl.haxx.se/docs/manpage.html)。

### 最后

我希望这篇介绍对您有所帮助，并且在您离开本教程时，至少知道这个方便的命令的基本知识。

正如我之前说过的，我还在学习，并且会欣赏任何其他关于程序使用的有趣知识。这同样适用于对我目前所写内容的任何反馈。

如果我犯了什么错误，或者有什么地方你认为我可以说得更清楚，请让我知道。

希望很快能再见到你，编码快乐！

*在 [Medium](http://www.medium.com/@strikingloo) 和 [Twitter](http://www.twitter.com/strikingloo) 上关注我，了解我的教程、技巧和文章。如果你喜欢这篇文章，可以考虑把它分享给一个 web 开发人员朋友(或者作为一种告诉他们学习 curl 的被动积极的方式)。*

*原载于[www . dataden . tech](http://www.dataden.tech/programming/how-start-using-curl/)2018 年 10 月 7 日。*