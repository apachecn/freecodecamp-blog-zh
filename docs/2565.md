# 如何在 Linux Web 服务器上快速跟踪 PDF 访问

> 原文：<https://www.freecodecamp.org/news/quickly-track-pdf-access-linux-web-server/>

有可能追踪你网站的用户点击下载 pdf 或 jpg 等二进制文件的次数吗？是的，有可能。容易吗？我原本不这么认为。我错了。

故事开始于我在我的 [Bootstrap IT 网站](https://bootstrap-it.com/davidclinton/keeping-up)上为我的新书*优化登陆页面的时候，保持:你不能错过的所有重大技术趋势的背景*。

我想提供这本书的样本章节的 PDF 文件。但是我也想知道有多少人真正下载了它。

现在让我们后退一步。Google Analytics 是一项免费服务，它使用插入到 HTML 文件中的代码片段来收集和显示你的文件被访问的频率。

谷歌分析的魔力和问题就在于你的用户能透露多少信息。我在《保持健康》一书中讨论了这项服务涉及的一些隐私问题。我也提到了我对自己在自己的网站上使用这项服务感到至少有点内疚。

无论如何，谷歌分析本身并不能告诉你太多关于你的基于网络的 pdf 文件是如何被使用的。当然，有一些技巧可以绕过这个问题。

传统的方法包括设置[谷歌标签管理器](https://marketingplatform.google.com/about/tag-manager/)，定制你使用的请求 URL 的语法，或者，如果你的网站使用 WordPress 软件，使用 [Monster Insights 插件](https://www.monsterinsights.com/)。这些都可以工作，但需要一个相当陡峭的学习曲线。

但我是 Linux 系统管理员。而且，我从来没有忘记提醒我周围的人，最好的系统管理员是懒惰的。学习曲线？这听起来很像工作。在我的监督下不会发生。

所以事情是这样的。显然，我的网络服务器运行的是 Linux。并且，在幕后，HTTP 流量由 Apache 处理。这意味着我的网站上发生的一切都将被 Apache 记录下来。

一切。我所需要知道的关于我的 PDF 示例章节的内容，只需要从我的本地工作站运行一行 Bash 代码:

```
echo "cd /var/log/apache2 && grep -nr KeepingUpSampleChapter" \
   | ssh -i PrivateKey.pem LoginName@bootstrap-it.com 
```

我们来分析一下。引号中的两个命令中的第一个(`cd /var/log/apache2`)将把我们移动到 Linux 服务器上的/var/log/apache2/目录，apache 在这里写日志。这不是火箭科学。

在那个目录中会有多个感兴趣的文件。这是因为与常规访问和错误相关的消息被保存到不同的文件中，并且因为文件轮换策略意味着这些文件也可能有多个版本。所以我将使用`grep`在所有未压缩文件中搜索`KeepingUpSampleChapter`字符串。`KeepingUpSampleChapter`当然是 PDF 文件名的一部分。

然后，我将该命令通过管道传输到 SSH，SSH 将连接到我远程服务器并执行该命令。下面是成功运行的单个条目的样子(出于隐私考虑，我删除了请求者的 IP 地址):

```
other_vhosts_access.log.1:12200:bootstrap-it.com:443 <requester's IP Address> - - [01/Dec/2020:16:39:36 -0500] "GET /davidclinton/KeepingUpSampleChapter.pdf?pdf=SamplePDF HTTP/1.1" 200 65146 "https://bootstrap-it.com/davidclinton/keeping-up/" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/84.0.4147.105 Safari/537.36" 
```

我们可以看到:

*   条目出现的日志文件(`other_vhosts_access.log.1`)
*   请求者的 IP 地址(修订)
*   时间戳告诉我们文件被访问的确切时间
*   文件在服务器文件系统上的相对位置(`/davidclinton/KeepingUpSampleChapter.pdf`)
*   发出请求的 URL(`https://bootstrap-it.com/davidclinton/keeping-up/`)
*   和用户运行的浏览器

信息量很大。如果我们只是想知道文件被下载了多少次*,我们可以简单地通过管道将输出发送给`wc`命令，它将告诉我们关于输出的三件事:行数、字数和包含的字符数。该命令如下所示:*

```
*`echo "cd /var/log/apache2 && grep -nr KeepingUpSampleChapter | wc" \
   | ssh -i PrivateKey.pem LoginName@bootstrap-it.com`* 
```

*这种方法可能有一个限制。如果您的网站很忙，日志文件会频繁滚动，通常一天不止一次。默认情况下，在第一次翻转后，文件使用`gz`算法进行压缩，不能被`grep`读取。*

*`zgrep`命令在处理这样的文件时不会有任何问题，但是这个过程可能会花费很长时间。您可能会考虑编写一个简单的定制脚本来解压缩每个`gz`文件，然后针对其内容运行常规的`grep`。那将是你的项目。*

*****************在我的[bootstrap-it.com](https://bootstrap-it.com/davidclinton)还有更多关于管理的书籍、课程和文章。*****************