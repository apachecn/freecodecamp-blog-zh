# 无服务器有其缺陷。以下是避免它们的方法。

> 原文：<https://www.freecodecamp.org/news/the-serverless-series-mistakes-you-should-avoid-9ec1ca6b9dff/>

尼古拉斯·道

# 无服务器有其缺陷。以下是避免它们的方法。

![oqbuJP3C3Gww0qSIAUrUm3zo8H4ku1558rBV](img/6c51f5e814859812141adae5ca5a6e0b.png)

Photo by [Luiz Hanfilaque](https://unsplash.com/photos/7RtM37cLJ3c?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/mistake?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在这篇文章中，我将分享我在过去一年中使用无服务器技术为悉尼的一家科技咨询公司构建移动和网络应用的经验。对于每个弊端，我也会推荐一个或多个解决方案。

#### 1.FaaS -连接池限制

FaaS 的谈话很少提到这个限制。云提供商将 FaaS 推销为一种可以无限扩展的解决方案。虽然这可能适用于函数本身，但是函数所依赖的大多数资源都不是无限可伸缩的。

关系数据库支持的并发连接数是这些有限资源之一。FaaS 对连接池的不友好使得这个问题变得如此严重。

事实上，正如我之前提到的，函数的每个实例都存在于其独立的无状态环境中。这意味着当它连接到一个关系数据库(例如 PostgreSQL、MySQL、Oracle)时，它最有可能使用连接池来避免与您的数据库来回重新连接。

您的关系数据库只能管理一定数量的并发连接(通常默认值是 20)。生成 20 个以上的函数实例会很快耗尽数据库连接，阻止其他系统访问它。

因此，如果您的函数需要使用连接池与关系数据库通信，我建议避免任何 FaaS。如果您需要使用连接池，那么有几个选项可用:

*   请改用 BaaS。
*   一些关系数据库如 PostgreSQL 提供了插件，可以通过复用可用并发连接数来解决这个问题。

#### 2.FaaS -不支持 WebSockets

这个很明显。但是对于那些认为他们可以鱼和熊掌兼得的人来说，你不能指望在一个从设计上来说是短暂的系统上维护一个 WebSocket。如果你正在寻找一个无服务器的 WebSocket，那么你现在需要使用像 Zeit 这样的 BaaS。

或者，如果您试图创建一个无服务器的 GraphQL API，那么可以通过使用 [AWS AppSync](https://aws.amazon.com/appsync/) 来使用订阅(它依赖于 WebSockets)。一篇很好的文章更详细地解释了这个用例，这篇文章是[用无服务器的](https://hackernoon.com/running-a-scalable-reliable-graphql-endpoint-with-serverless-db16e42dc266)运行一个可伸缩的&可靠的 GraphQL 端点。

#### 3.FaaS —冷启动

像 [AWS Lambda](https://aws.amazon.com/lambda) 这样的 FaaS 解决方案在解决 Map-Reduce 挑战时已经证明了巨大的收益(例如，[利用 AWS Lambda 进行大规模图像压缩](https://medium.com/squad-engineering/leveraging-aws-lambda-for-image-compression-at-scale-a01afd756a12))。但是，如果您试图提供对 HTTP 请求等事件的快速响应，您需要考虑函数预热所需的时间。

您的功能存在于一个虚拟环境中，需要根据其接收的流量进行扩展(这是您无法控制的)。这个繁殖过程需要几秒钟，在您的函数由于低流量而空闲后，它将需要再次繁殖。

当我在 Google Cloud Functions 上部署一个相对复杂的 reporting REST API 时，我明白了这一点。该 API 是微服务重构工作的一部分，旨在分解我们庞大的整体式 web API。我从一个低流量端点开始，这意味着该功能经常处于空闲状态。由该微服务支持的报告在第一次被访问时变得很慢。

为了解决这个问题，我把我们的微服务从谷歌云功能(FaaS)转移到了 Zeit Now (BaaS)。那次迁移让我可以一直保持至少一个实例(在我的下一篇文章中会有更多关于 Zeit Now 的内容:为什么我们现在喜欢 Zeit &什么时候在 FaaS 使用它)。

#### 4.FaaS -长寿的过程，不要打扰！

AWS Lambda 和 Google Cloud 功能的运行时间分别不会超过 5 分钟和 9 分钟。如果您的业务逻辑是一个长期运行的任务，那么您现在必须转向像 Zeit 这样的 BaaS。

有关 FaaS 限制的更多详情，请参考 [AWS Lambda 配额](https://docs.aws.amazon.com/lambda/latest/dg/limits.html)和[谷歌云功能配额](https://cloud.google.com/functions/quotas)。

#### 5.BaaS 和 FaaS -放松基础设施控制

如果您的产品要求需要对您的基础设施进行某种程度的控制，那么无服务器很可能会让您陷入困境。这种问题的例子可能是:

*   微服务部署流程编排。以无数无服务器的微服务告终将很快成为部署的噩梦，尤其是如果它们需要一起或按域进行版本控制的话。
*   控制每台服务器的生命周期以节省成本。
*   在多台服务器上长时间运行任务。
*   控制底层服务器操作系统的确切版本，或者安装应用程序所需的特定库。
*   控制您的应用或数据的精确地理复制，以确保全球一致和快速的性能(在某些情况下，有一些方法可以克服这一点。查看[在一小时内构建一个无服务器的多区域、主动-主动后端解决方案](https://read.acloud.guru/building-a-serverless-multi-region-active-active-backend-36f28bed4ecf)。

无服务器在上述所有用例中可能都有不足。然而，正如我之前所讨论的，无服务器只是 PaaS 的扩展。为了尽可能地专注于编写代码，而不是过多地担心底层基础设施的可伸缩性和可靠性，利用最新的 PaaS 容器化策略，如 [Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine/) 可以让您非常接近无服务器提供的功能。

#### 6.BaaS & FaaS -合规与安全

无服务器共享所有与云相关的常见抱怨。您正在将对基础架构的控制权交给一个或多个第三方。根据供应商的不同，无服务器可能会也可能不会为您的业务提供合适的 SLA 和安全级别。

从合规性和安全性的角度来看，无服务器是可行还是不可行，实际上取决于您的具体情况。许多文章详细讨论了这个话题(比如[无服务器安全的状态](https://read.acloud.guru/the-state-of-serverless-security-fall-2017-fb2b8936044f))。

### 结论

无服务器不是银弹。你能从中获得什么取决于你对它的了解。好的一点是入门门槛很低，你应该很快就能精通。

### 接下来…

当然，Serverless 也有局限性。所有技术方案都有。现在的问题是我们如何克服它们。在我的下一篇文章中，我将写下我的团队和我提出的解决这些限制的建议:“为什么我们现在喜欢 Zeit 什么时候在 FaaS 使用它”。

如果你对接下来发生的事情感兴趣，请关注我。

此无服务器系列中的当前帖子:

*   你真的知道什么是无服务器吗？
*   [无服务器对技术领先地位的影响](https://hackernoon.com/the-serverless-series-automating-it-engineers-reshaping-tech-leadership-788cf9b625d5)
*   [无服务器如何实现 IT 工程师自动化](https://hackernoon.com/the-serverless-series-automating-it-engineers-reshaping-tech-leadership-788cf9b625d5)

该系列的后续文章:

*   为什么我们现在爱用 Zeit&来对付 FaaS
*   无服务器事件驱动架构:自然的契合
*   如何管理无服务器的反压？
*   在不到 2 分钟的时间内在无服务器上运行 GraphQL