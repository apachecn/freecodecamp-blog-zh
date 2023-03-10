# HTTP 如何工作及其重要性——用简单的英语解释

> 原文：<https://www.freecodecamp.org/news/how-the-internet-works/>

想象你的房子是一台巨大的电脑。你的家庭地址由数字组成，而不是古迪逊街或第四大道。例如:112.231.31.20。

就像在一部未来电影中一样，你的城市很大程度上由天空中的高科技机器人组成，这些机器人挨家挨户地传递信息并做出回应。

明白了吗？

## 互联网工作原理概述

稍微简化一下，这就是在浏览器中键入网址时发生的情况:

*   它会找到您想要发送请求的“房子”的地址
*   它使用机器人邮递员发送请求
*   它耐心地等待机器人邮递员的回应

现在，所有这些都从作为最终用户的您那里抽象出来了。你在浏览器中输入网址，网页就出现在你眼前——像变魔术一样。

像任何足够先进的技术一样，没有这些抽象，普通用户将无法使用互联网。

大多数时候，你不需要担心某样东西是如何工作的——你只需要知道它是工作的。

但是对于某些主题来说，深入一点细节是有帮助的，或者只是满足好奇心。

通过阅读这篇文章，你不会成为互联网技术细节方面的专家——这将花费更多的时间和精力——但你会获得一个鸟瞰图和更好的理解。

如果你确实想了解更多，我在 YouTube 上有一个更深入的播放列表。

## 消息传递系统

从本文开头的比喻中，我们了解到互联网是由被传递的消息组成的。在大多数情况下，这些消息是使用所谓的 HTTP 协议发送的。

协议。这是一个可怕的词。那是一个目光呆滞，关闭你的浏览器类型的词。所以让我们把它分解成更简单的术语。

> 协议只是协议的一个花哨的说法。

我们用一个类比来说清楚。

假设你和你最好的朋友互相留秘密信息。当你发现你家门口有一张写着“ballfoot”的纸时，你知道你的朋友想在今晚 20:00 和你一起踢足球。

你知道这一点是因为你同意在送到你家的一张纸上的单词“ballfoot”代表邀请你去玩。

现在，一个问题出现了，当你开始给你的其他朋友留纸条“ballfoot”而没有告诉他们秘密的意思。他们不知道如何处理这些信息。

他们会在自己家门口找到这张纸条，搔搔头，然后在自己的客厅里继续演奏堡垒之夜。你和你的另一个朋友会把球传给你。来来回回。来来回回。直到无聊到无法忍受，你们都回家。

但不一定非要这样。如果你告诉你的朋友“ballfoot”是什么意思呢？现在，你的每一个朋友都会知道并分享这样一个约定:写有“ballfoot”的纸条意味着在 20:00 出现在当地的球场上踢足球。

成功。

本质上，这就是 HTTP 协议所代表的。我们已经同意，如果我们以特定的方式发送消息，服务器将理解它，并给出响应。

### 消息的结构

让我们仔细看看 HTTP 协议。它由请求和响应组成。简单地说，你请求一些东西，然后从一个叫做服务器的东西那里得到一个回答。

在我们继续之前，让我们从头开始修改我们的比喻，以便更好地理解 HTTP 请求/响应周期。

还记得机器人挨家挨户传递信息吗？现在想象所有这些机器人都属于某个人。

你有自己的私人机器人，你可以让它带着消息去任何地址(IP 地址)。一旦你的机器人带着你的信息到达给定的地址，它会进入并大胆地宣布它有信息要传递。然后它会说出信息。

为了打个比方，想象一下房子(服务器)的门就像《指环王》中莫莉亚矿井的入口。只有单词说对了，门才会打开让你进去。

在这种情况下，只有当你的机器人以特定的方式说出信息时，他们才会收到回复信息并带回给你。

这就是 HTTP 协议在起作用。有一组预定义的规则来指导请求和响应消息的外观。

此时，您可能想知道这些消息是从哪里来的。当你在浏览器中输入网址时，你当然不会自己写。

这都是由浏览器自动为您处理的。当您输入一个地址时，您的浏览器会负责为您编写 HTTP 请求消息，并将其发送到服务器。HTTP 请求消息如下所示:

```
GET / HTTP/1.1
Host: google.com
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) 
Version/11.0 Mobile/15A372 Safari/604.1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,
image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
...etc
```

看起来很吓人吧？

好在浏览器为我们做到了这一点。

让我们仔细看看刚才的第一行:`GET / HTTP/1.1`。这一行让你的机器人去谷歌的房子，并说，“我可以请接收你在你的网站根的任何东西吗？”(这意味着我们想取回的是 www.google.com 的东西，而不是 www.google.com/home.的)

所以现在我们已经以正确的方式将我们的消息传递到了 Google 的房子(服务器)。门亮了，然后打开。

在里面你会看到另一个机器人。在它后面是一系列标有类似`GET / HTTP/1.1`和`GET /search HTTP/1.1`的文字的锁箱。如果你的请求与这些锁箱中的一个相匹配，机器人将打开它并将里面的东西交给你的机器人，这将促使它迅速返回给你响应。

### 回应

您得到的回复看起来会像这样:

```
HTTP/1.1 200 OK
Date: Mon, 27 Jul 2009 12:28:53 GMT
Server: Apache/2.2.14 (Win32)
Last-Modified: Wed, 22 Jul 2009 19:15:56 GMT
Content-Length: 88
Content-Type: text/html
Connection: Closed
```

现在，你将永远看不到这个响应，除非你真的想在浏览器的开发工具中检查它。但是不管怎样，你收到了。

接下来会发生什么取决于您收到什么样的响应，以及服务器的锁箱里有什么。

在许多情况下，您得到的回报是一个 HTML 文档。HTML 表示网页的结构，并定义浏览器应该显示的内容。

如果你去 www.google.com，你会收到一个 HTML 文件，它定义了 google.com 网站在你的浏览器中的显示方式。

如果您有时间，这段 11 分钟的视频将深入探讨 HTTP 请求和响应:

[https://www.youtube.com/embed/videoseries?list=PL_kr51suci7XVVw4SJLZ0QQBAsL2K8Opy](https://www.youtube.com/embed/videoseries?list=PL_kr51suci7XVVw4SJLZ0QQBAsL2K8Opy)

## 结论

在这篇文章中，我们回顾了互联网是如何工作的，以及我们如何使用 HTTP 在互联网上进行通信。

我们了解到，HTTP 协议是用来在互联网上的浏览器和服务器之间进行通信的，它由一个公认的发送和接收请求的标准组成。

我们还探讨了拥有这样的交流标准的重要性，以及拥有一个普遍认可的标准的好处。

要了解互联网是如何工作的，以及你能收到什么样的回应，还有很多方面。

如果您有时间，这段 18 分钟的视频将教您如何构建 web 服务器，它将回顾本文中涉及的许多主题，并介绍一些新的主题:

[https://www.youtube.com/embed/videoseries?list=PL_kr51suci7XVVw4SJLZ0QQBAsL2K8Opy](https://www.youtube.com/embed/videoseries?list=PL_kr51suci7XVVw4SJLZ0QQBAsL2K8Opy)

现在你应该对互联网上的交流有了一个大致的了解。

如果你认为其他人也能从这篇文章中受益，请广而告之。如果你想知道我什么时候发布更多内容，你可以[订阅我的 YouTube 频道](https://www.youtube.com/channel/UCZTeUahnA2GMoo_YpTBFo9A)，或者你可以在 Twitter 上关注我[@ fose Berg](https://twitter.com/foseberg)。