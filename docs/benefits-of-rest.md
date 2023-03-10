# 休息的好处——什么是休息，为什么你应该了解它

> 原文：<https://www.freecodecamp.org/news/benefits-of-rest/>

在本文中，我们将看一看表述性状态转移(REST)原则，以了解它们是什么以及应用它们可以获得什么好处。

我相信理解你为什么要学习某些东西是很重要的——包括休息。因此，让我们看看 REST 原则带来了什么。

## 什么是休息？

表述性状态转移(REST)是一种架构风格，由于其简单性和可伸缩性，近年来受到了广泛的欢迎。

在 REST 流行之前，SOAP 是通过 web 访问资源和通信的事实上的方式。

## 你为什么要关心休息？

在这一节中，我将讨论为什么 REST 原则很重要，以及为什么值得花力气去学习更多关于它们的知识。您还将学习如何将它们应用到您的后端项目中。

### 1) REST 易于理解和实现

REST 意味着在 HTTP 上工作(实际上 HTTP 受到 REST 的影响)。因此，它使用了我们大多数人都知道的 HTTP 动词，比如 GET、POST 和 PUT。

即使你不知道这些动词是关于什么的，它们的名字也是不言自明的。此外，客户机和服务器代码的明确分离使得不同的团队可以轻松地处理应用程序的不同部分(前端或后端)。

因为 REST 易于理解和实现，所以它有助于提高开发团队的生产力。如果你打算发布一个公共 API 给人们开发应用程序，它们也是很重要的。

许多人都知道 REST 和 HTTP，所以他们理解和使用您的 API 会容易得多。

![How to Keep Your Developer Team Happy: Lead Dev New York 2019 | Arc Blog](img/8ef054dbb3e02adfb4356f8d51c8311f.png)

Happy Developers

### 2) REST 使您的应用程序更具可伸缩性

REST 有助于提高应用程序的可伸缩性，主要有两个原因:

#### 没有州

正如我们将在下一节(REST 的原则)中看到的，REST 的核心原则之一是它在服务器端是无状态的。因此，每个请求将独立于前一个请求进行处理。

在具有服务器端状态或会话的应用程序中，可能为每个登录用户存储一个会话。这种会话数据很容易变得臃肿，并开始占用服务器上的大量资源。

另一方面，无状态服务器只在处理请求时占用资源(内存),请求一处理完就释放资源。

由于可伸缩性的当前趋势是水平伸缩(通常在云上)，存储服务器端会话也会使应用程序难以伸缩，因为它会产生一些难题。

例如，假设您有许多在负载平衡器后面运行的服务器。如果客户端在其第一个请求中到达服务器 1(服务器 1 现在拥有客户端的会话),并且在稍后的时间，由于服务器 1 上的负载，客户端到达服务器 2，而服务器 2 不知道存储在服务器 1 上的先前会话数据，会发生什么？当然，这个问题有解决方案，但是它增加了可伸缩性的难度。

#### 更快的数据交换格式

RESTful APIs 通常使用 JSON 作为数据交换格式。与 XML 相比，JSON 更加紧凑，尺寸也更小。它的解析速度也比 XML 快。([来源](http://ijcsn.org/IJCSN-2014/3-4/JSON-vs-XML-A-Comparative-Performance-Analysis-of-Data-Exchange-Formats.pdf))

虽然它们主要使用 JSON，但是也要记住 REST APIs 仍然能够通过使用 [Accept 头以不同的格式进行响应。](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept)

### REST 使缓存更容易

缓存是现代 web 应用程序的可伸缩性和性能的关键因素。一个完善的缓存机制(尽可能具有最好的命中率)可以大大减少服务器的平均响应时间。

REST 旨在让缓存变得更容易。因为服务器是无状态的，每个请求都可以单独处理，所以 GET 请求通常应该返回相同的响应，而不管之前的请求和会话。

这使得 GET 请求很容易被缓存，浏览器通常也这样对待它们。我们还可以使用 **Cache-Control** 和 **Expires** 头来缓存我们的 POST 请求。

### 4)休息是灵活的

我所说的灵活性是指它易于修改，并且能够满足许多客户对不同数据类型(XML、JSON 等)的需求。

客户端可以使用 **Accept** 头指定类型(正如我前面提到的), REST API 可以据此返回不同的响应。

另一个值得一提的机制是 [HATEOAS](https://www.wikiwand.com/en/HATEOAS#:~:text=Hypermedia%20as%20the%20Engine%20of,provide%20information%20dynamically%20through%20hypermedia.) 。如果您不知道这个术语，不要担心，它的基本意思是:在特定资源的服务器响应中返回相关的 URL。

看看这个来自维基百科的例子。客户端向银行 API 请求带有`account_number`的账户信息，并得到以下响应:

```
 {
    "account": {
        "account_number": 12345,
        "balance": {
            "currency": "usd",
            "value": 100.00
        },
        "links": {
            "deposit": "/accounts/12345/deposit",
            "withdraw": "/accounts/12345/withdraw",
            "transfer": "/accounts/12345/transfer",
            "close": "/accounts/12345/close"
        }
    }
}
```

这个服务器利用 HATEOAS 并返回相应动作的链接。这使得探索 API 变得非常容易，并且通过允许服务器改变端点也使它变得灵活。

可以这样想:如果服务器没有应用 HATEOAS，客户端需要硬编码端点，比如“/accounts/:account-id/deposit”。但是如果服务器把 URL 改为“/accounts/:account-id/deposit money”，客户端代码也需要更改。

在 HATEOAS 链接的帮助下，客户端可以通过解析这个 JSON 来检查链接，并轻松地发出请求。如果端点发生变化，他们将获得新的端点，而无需更改客户端代码。

关于这个话题的更多见解，你可以看看 Roy Fielding 自己的博客文章。

## 结论

在这篇文章中，我试图表达为什么我重视休息，为什么我认为你也应该重视休息。我希望看完这篇文章后，您现在对应用 REST 标准的原因更加清楚了。

这篇文章可以作为学习更多关于这个话题的动力。我有一些好消息:我计划在不久的将来写一些关于 REST 最佳实践和常见错误的文章。

如果你有兴趣可以关注或者订阅我的[博客](http://erinc.io/)。你也可以看看我以前在那里的帖子:)

如果你有任何问题或者想进一步讨论这个话题，你可以随时联系我。

新年快乐，感谢阅读。:)