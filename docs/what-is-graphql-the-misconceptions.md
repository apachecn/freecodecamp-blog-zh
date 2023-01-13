# GraphQL 是什么？常见的神话被揭穿。

> 原文：<https://www.freecodecamp.org/news/what-is-graphql-the-misconceptions/>

我喜欢谈论 GraphQL，尤其是和一直在使用 GraphQL 或者考虑采用 GraphQL 的人。人们的一个常见问题是，为什么有人想从 REST 迁移到 GraphQL。

有大量的资源讨论 REST 和 GraphQL 之间的区别，如果您对这两者的区别感兴趣，可以去看看。在这篇博文中，我想解决一些关于 GraphQL 的常见误解和问题。

## 如何从前端的 GraphQL 中获益？

作为一名前端工程师，我喜欢使用 GraphQL API，原因如下:

1.  您可以使用 GraphiQL 或 playground 立即测试查询和变异
2.  更少的数据意味着更轻的状态管理
3.  它通过解析器将繁重的工作转移给服务器
4.  它有最新的交互式文档

## 怎么比休息好？

1.  有一个端点可以获取所有资源。
2.  您可以避免过度获取数据(在只需要几个字段时获取太多字段)。
3.  您可以避免数据提取不足(必须调用多个 API，因为一个 API 不能返回所有需要的信息)。

## 误区:GraphQL 用于查询图形数据库。

GraphQL 可用于查询图形数据库，但这不是它唯一的用例。GraphQL 中的“graph”用于表示数据的类似图形的结构。您根据节点以及它们如何相互连接来建模数据。模式用于表示这种建模。

GraphQL 规范中没有强制数据源应该是图表的限制。

## 误解:GraphQL 只适用于基于图形的数据库或数据源。

认为需要重写数据库来采用 GraphQL 是一种误解。GraphQL 可以是任何数据源的包装器，包括数据库。GraphQL 是一个`query language for your API`——这意味着它是一个如何请求数据的语法。

## 神话:用解析器、查询和突变来获取数据有神奇的效果。

你需要准确定义他们每个人需要做什么。您将编写在查询被触发时被调用的函数，并为解析器编写函数，这些函数将准确地发送回您需要的数据，并知道要调用哪个 API。您将通过调用解析器来定义通过这些函数返回的数据。

## 神话:GraphQL 取代了 Redux 或任何状态管理库

Redux 是一个状态管理库。GraphQL 不是一个状态管理库。GraphQL 有助于获得更少的数据，这反过来导致客户端需要管理的数据更少，但它不是一个状态管理解决方案。

您仍然需要管理状态——尽管是以一种更轻量级的方式。像 Apollo 和 Relay 这样的客户端库可以用来管理状态，它们有内置的缓存。GraphQL 不是 Redux 的替代品——它有助于减少对它的需求。

## 误区:GraphQL 是一种数据库语言。

GraphQL 是一种编程语言——特别是一种查询语言。GraphQL 的规范定义了 GraphQL 运行时应该如何实现该语言，以及数据应该如何在客户机和服务器之间进行通信。

GraphQL 是用来索要数据的，可以用在从前端到后端的任何一层的多个地方。有一些数据库(如 DGraph)实现了 GraphQL 规范，允许客户端使用 GraphQL 来查询数据库。

## 误解:在 GraphQL 的实现中不能有 REST 端点。

您可以在应用程序中插入多个 REST 端点，甚至多个 GraphQL 端点。尽管拥有多个 REST 端点并不是最佳实践，但在技术上是可能的。

## 误解:GraphQL 很难在现有项目中引入。

GraphQL 可以插入到现有的项目中。您可以从业务逻辑的一个组件开始，插入一个 GraphQL 端点，并开始通过 GraphQL 获取数据。开始使用 GraphQL 并不需要放弃整个项目。

如果切换到 GraphQL 端点仍然是一项艰巨的任务，那么可以从使用解析器将 REST 端点屏蔽到 GraphQL 端点开始。

## 误区:GraphQL 只面向前端开发者

GraphQL 是平台无关的。在我看来，GraphQL 的好处之美是从内向外开始的——从后端到前端。

作为后端开发人员，您可以通过添加字段来扩展 API，而不必发布 API 的新版本。您不需要为不同的需求编写不同的端点，因为客户端可以获取他们需要的任何数据。

使用 GraphQL，您可以看到客户端正在使用哪些字段，这提供了强大的工具。

## 神话:GraphQL 将自己编写数据库查询，我只需要指定模式和它们之间的关系

根据您使用的 GraphQL 库，您可能需要编写数据库查询。然而，像 Neo4j 和 Prisma 这样的库也编写数据库查询，并从开发人员那里抽象出逻辑。包括解析器、查询和变异在内的一切都需要定义。

## 误区:GraphQL 是用来画图的。

经常有人认为 GraphQL 是一个绘图软件，比如 D3。GraphQL 不绘制图形。

## 误解:GraphQL 需要复杂的客户端，用简单的 HTTP 获取几乎不可能做到

## 神话:它取代了 ORM。

最近我们看到了很多 DB 和 GraphQL 的集成，但是 GraphQL 本身并不是这样。

我觉得 GraphQL 很牛逼！有许多库可以帮助用户开始使用 GraphQL。如果你对学习 GraphQL 感兴趣，[从文档开始](https://graphql.org/learn/)或[看看这个我刚接触 GraphQL 时发现很有帮助的 Udemy 课程](https://www.udemy.com/course/graphql-with-react-course/)。

> 我要宣战！
> 
> var 战；[#开发笑话](https://twitter.com/hashtag/DevJoke?src=hash&ref_src=twsrc%5Etfw)
> 
> — Shruti Kapoor (@shrutikapoor08) [December 12, 2019](https://twitter.com/shrutikapoor08/status/1205005069223022592?ref_src=twsrc%5Etfw)