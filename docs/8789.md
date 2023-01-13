# 伟大的，丑陋的

> 原文：<https://www.freecodecamp.org/news/firebase-the-great-the-meh-and-the-ugly-a07252fbcf15/>

皮尔·博弗

# 伟大的，丑陋的

![JEd0ZDCmvO20CnZPaFX7xifOsMstUcT90eDs](img/d5e6412c18ec16f31f676ba44c8371bc.png)

当谷歌在 2016 年 5 月的谷歌 I/O 上宣布 Firebase 时，我们立即投入了这项工作。

我们正在开发一个单页 React 应用程序，需要通过 Cordova 在移动设备上运行，通过 electronic 在桌面上运行。所以 Firebase 对我们来说似乎是一个神奇的解决方案。

现在，经过 7 个月几乎每天都使用 Firebase 的工作，我准备分享我们的经验。

### 伟大的

#### 实时数据

是啊，太棒了。通过一点管道和一些数据绑定魔法，您可以将视图与数据连接起来，当数据改变时，它们会神奇地改变。

在我们的体验中，性能一直很好，尽管 Firebase 是为数百万用户设计的，所以我们甚至没有触及到它的皮毛。

我们的用户仍然对一切运行的速度印象深刻。

#### 类固醇上的静态托管

Firebase 主机附带免费的 CDN 和 SSL——都运行在谷歌云平台上。这意味着向全球任何数量的用户提供文件服务都不会有任何问题。

如果你正在为你的下一个单页应用或静态网站寻找零配置主机，我真的会考虑 Firebase 作为一个选择，即使你不使用谷歌的任何其他服务。

#### 超级大国

Firebase 还为您提供了许多非常容易集成的服务和 SDK，例如:

*   OAuth 认证
*   文件存储器
*   数据库备份
*   自动缩放
*   用于部署和其他任务的 CLI
*   自由层

### Meh

#### 控制台

它很漂亮，可以让你做很多事情，但是它没有*那么*有用。

数据库管理器实际上是一个美化了的 JSON 编辑器。就其本身而言很棒，但它不是我所期望的成熟的解决方案。如果您来自 WorkBench、Postico、Mongotron，甚至 PHPMyAdmin，这将是一个不错的玩具。

控制台的另一个非常有限的方面是缺乏详细的日志或分析。考虑到谷歌对数据的痴迷，我们谈论的这件事看起来很奇怪。当然，对于数据库的使用情况，您会得到一个很好的图表，但是除非您自己实现一个解决方案，否则没有办法知道一个文件从存储中下载了多少次。

#### 无服务器？

Firebase 就是静态托管+ API，仅此而已。这个限制并不是世界末日。您可以通过使用 Node.js 服务器作为 Firebase 的另一个客户端来轻松解决这个问题，您很可能需要它来执行许多常见任务，如创建缩略图、向用户发送电子邮件等。

显然，将有可能在 Firebase 上使用谷歌云功能(T1 )(仍在 Alpha 版本中),但谁知道什么时候。也许这个会在 Google I/O 2017 上公布。

**(编辑 2017 年 3 月** : Firebase 刚刚宣布[Firebase 的 Google Cloud 功能](https://firebase.googleblog.com/2017/03/introducing-cloud-functions-for-firebase.html))

(**编辑 2018 年 5 月**:查看[我对 Firebase 云功能的评测](https://medium.com/@Pier/firebase-cloud-functions-the-great-the-meh-and-the-ugly-c4562c6dc65d)

#### 定义安全性规则

Firebase 使用 JSON 文件和字符串形式的 Javascript 代码来定义数据库和存储的规则。

```
{  "rules": {    "users": {      "$uid": {        ".write": "$uid === auth.uid"      }    }  }}
```

这并不像听起来那么糟糕，因为你可以使用[螺栓](https://github.com/firebase/bolt)来减少这个过程的痛苦。尽管如此，即使在使用 Bolt 时，一旦超出了几十个简单的规则，这个文件就变得不可维护了。

像[梦工厂](https://www.dreamfactory.com/)和[图形酷](https://www.graph.cool/)这样的服务给你一个合适的工具去做，而不会失去理智。

#### 专有技术

当脸书决定关闭 Parse 时，许多项目发现自己陷入了困境。老实说，我怀疑 Firebase 会发生这种情况，但我可以理解为什么不愿意将您的技术堆栈与第三方平台即服务结合起来。

#### 没办法在本地发展

如果你经常旅行或居住在一个连接不良的国家，考虑你不能使用本地安装。你不能只使用 Docker 或 Node 并启动你的 API。

### 丑陋的

#### 有限的 Javascript SDK

Firebase 中有许多功能只在他们的 iOS 和 Android SDKs 中实现。

最突出的一点是使用 Javascript 时缺乏离线持久性。如果设备暂时失去连接，您的 web、混合或反应式应用程序将继续工作。但是一旦你关闭应用程序或标签，你的数据将会消失。实现具有持久性的缓存取决于您。这真的是一项艰巨的任务，尤其是在手机上。

Javascript SDK 甚至没有缓存数据的方法(不确定是 iOS 还是 Android)。如果你加载了`/products`然后又需要那个数据，你将不得不重新加载它，除非你在后台手动保持一个连接。实现这一点并不难，但是，为什么 Firebase 不提供一种*神奇的*方式来实现呢？

#### 无法正确查询您的数据

你可以做一些非常基本的过滤和分页，但除此之外，你只能靠自己了。

真的吗？谷歌提供的数据服务没有搜索或过滤功能？

是啊。真的。

如果你想实现搜索功能，你必须下载所有的数据并在客户端完成，使用我之前描述过的服务器，或者实现第三方服务，比如 [Elastic](https://www.elastic.co/) 。

Firebase 的开发者说这是设计使然，所以他们可以保证高性能。好的。但是为什么不让我们用户来决定我们是否有能力为我们的用例支付性能价格呢？

是啊，忘了用你的数据做连接或任何有点花哨的事情吧。这让我想到…

#### 哑数据建模

> 处理与 NoSQL 的关系是困难的，处理与伊朗的关系是痛苦的。巴蒂斯特·贾明

他所说的。

Firebase 数据库基本上只是一个巨大的 JSON 文件。没有办法声明*一对多*或*多对多*关系。实际上，这意味着你最终会到处复制你的数据。

乍一看，这似乎没那么糟糕。毕竟，把用户的名字放在聊天消息里很方便，不是吗？

```
{ “author”:”Pepito Flores”, “message”:”I want a taco!”, “time”: 1484269756951}
```

当你不得不实际编辑 Pepito 的名字时，问题就来了，因为你将不得不在你使用它的所有地方修改它**，而不仅仅是在`/users`。**

告诉你的用户他们不能编辑他们的名字通常不是一个可行的选择，所以:

1.  在许多情况下，向 Firebase 写入和编辑数据的客户端代码将变成 Italian Food。
2.  至少可以说，记录在哪里复制了数据是很困难的。

此外，由于许多 NoSQL 数据库，如 [MongoDB](https://docs.mongodb.com/manual/core/data-model-design/#data-modeling-referencing) 或 [RethinkDB](https://www.rethinkdb.com/docs/table-joins/) 已经找到了解决这个问题的方法，我很难相信谷歌不能以至少合理的性能解决这个问题。

### TL；速度三角形定位法(dead reckoning)

Firebase 非常适合简单的项目或开发需要实时数据的小功能。例如聊天或通知系统。这些是你在 YouTube 上看到的令人印象深刻的 30 分钟演示。如果你的数据是具有简单结构的*事物*的流，比如一个在线多人游戏的服务，它也能很好地工作。

使用 Firebase，任何具有更复杂数据需求的事情都变得困难甚至不可能。常规的数据库查询在大多数情况下比实时数据更有价值，尽管看到事物的变化令人印象深刻，但你可能并不需要它们。

像所有事情一样，为工作选择合适的工具。

### 附录:什么样的 Firebase 才是令人敬畏的

1.  真正的查询能力。搜索，连接，所有的一切。
2.  像 MongoDB 或 RethinkDB 这样的参考。
3.  用 Javascript 实现真正的离线持久化。
4.  给我 *moar* 分析。
5.  缓存 API。

![OsPFhH5ZqUDHJmO5x3-RALnjXyyOZETdlcGs](img/1a927fc614391b146242b953594700e5.png)

仅此而已。

### 附录 2: moar 信息

如果你正在阅读这篇文章，你可能会把 Firebase 作为一名开发人员或 CTO 来评估。这里有一些其他的文章可以帮助你决定 Firebase 是否适合你，以及是否值得投入额外的开发时间进行评估。

[**Firebase:好的、坏的、丑陋的- RaizException - Raizlabs 开发者博客**](https://www.raizlabs.com/dev/2016/12/firebase-case-study/)
[*作为 Raizlabs 软件开发人员工作的一部分，我们不断评估使用的最新开发工具…*www.raizlabs.com](https://www.raizlabs.com/dev/2016/12/firebase-case-study/)[**不使用 Firebase 的理由**](https://crisp.im/blog/why-you-should-never-use-firebase-realtime-database/)
[*构建实时应用程序是当今的标准。在 Crisp，我们在生产中使用 Firebase 超过 9 个月，从…* crisp.im](https://crisp.im/blog/why-you-should-never-use-firebase-realtime-database/) 开始