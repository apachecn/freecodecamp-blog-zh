# GraphQL 应用中的五个常见问题(以及如何解决它们)

> 原文：<https://www.freecodecamp.org/news/five-common-problems-in-graphql-apps-and-how-to-fix-them-ac74d37a293c/>

萨沙·格里菲

![otGc4zlSYDhfVWzfyANc1ZOX5s3cJbJMF4J7](img/61cebbead1a798007da6b33ced33d8ea.png)

# GraphQL 应用中的五个常见问题(以及如何解决它们)

#### 学习释放 GraphQL 的力量，同时避免其缺点

GraphQL 最近风靡一时，理由很充分:这是一种优雅的方法，解决了许多与传统 REST APIs 相关的问题。

然而，如果我告诉你 GraphQL 没有自己的问题，那我就是在撒谎。如果你不小心，这些问题可能不仅会导致代码库膨胀，甚至会导致应用程序速度大幅下降。

我在谈论这样的问题:

*   **模式复制**
*   **服务器/客户端数据不匹配**
*   **多余的数据库调用**
*   **表现不佳**
*   **样板文件过量**

我敢打赌，你的应用程序至少会受到其中一个问题的困扰。好消息是，没有一个是不治之症！

对于每一个问题，我将描述问题，然后解释我如何在 [Vulcan](http://vulcanjs.org) 中解决它，这是一个我在过去一年中一直致力于的 React/GraphQL 开源框架(你应该去看看！).但是希望无论你是否使用 Vulcan，你都能够将相同的策略应用到你自己的代码库中。

![WzpNm95jVV1teXE08i6UcUQ3UmjxuQrX7d8E](img/914099bfe929e553ebf0ec4bfa64153b.png)

### 问题:模式重复

从头开始编写 GraphQL 后端代码时，您首先意识到的一件事是，它涉及许多相似但不完全相同的代码，尤其是在涉及模式时。

也就是说，数据库需要一个模式，GraphQL 端点需要另一个模式。写差不多同样的东西两次不仅令人沮丧，而且你现在有了两个独立的真相来源，你需要不断保持同步。

#### 解决方案:GraphQL 模式生成

GraphQL 生态系统中已经出现了许多解决这个问题的方案。例如，[PostgreSQL](https://www.graphile.org/postgraphile/)从你的 PostgreSQL 数据库中生成一个 GraphQL 模式， [Prisma](https://www.prismagraphql.com/) 也会帮助你为你的查询和变异生成类型。

我还记得听到 GraphQL 团队的 [Laney Zamore & Adam Kramer 描述他们如何从 PHP 类型定义中直接生成 GraphQL 模式。](https://www.youtube.com/watch?v=XfHOrfTyJJw)

对于 Vulcan，我独立地偶然发现了一个非常相似的解决方案。我使用 [SimpleSchema](https://github.com/aldeed/simple-schema-js) 将我的模式描述为 JavaScript 对象，我开始只是将 JavaScript 的`String`类型转换为 GraphQL `String`，`Number`转换为`Int`或`Float`，等等。

所以这个 JavaScript 字段:

```
title: {  type: String}
```

会变成这个 GraphQL 字段:

```
title: String
```

但是当然，GraphQL 模式也可以有定制类型:`User`、`Comment`、`Event`等等。

我不想给模式生成步骤添加太多的魔法，所以我想出了[字段解析器](http://docs.vulcanjs.org/field-resolvers.html)，一种让您指定这些定制类型的简单方法。因此这个 JavaScript 字段:

```
userId{  type: String,  resolveAs: {    fieldName: 'user',    type: 'User',    resolver: document => {      return Users.findOne(document.userId)    }  }}
```

变成了:

```
user: User
```

如您所见，我们也在字段上定义了实际的[解析器](http://graphql.org/learn/execution/)函数，因为它也与 GraphQL 字段直接相关。

因此，无论您是使用 PostGraphile 之类的东西，还是编写自己的模式生成代码，我都鼓励您在自己的应用程序中避免模式重复。

当然，您也可以使用托管服务，如 [Graphcool](https://www.graph.cool/) 来使用他们的仪表板管理您的模式，并完全绕过这个问题。

![czkjIXuQcTg3x1rNV41WWK8U7BWTiF-UV5xm](img/de36ff7e730630d189e618b1e7adf06a.png)

### 问题:服务器/客户端数据不匹配

正如我们刚刚看到的，您的数据库和 GraphQL API 将具有不同的模式，这些模式会转换成不同的文档形状。

因此，当一个刚从数据库中取出的`post`将有一个`userId`属性时，通过 API 取出的同一个`post`将有一个`user`属性。

这意味着在客户机上获得文章作者的名字看起来像:

```
const getPostNameClient = post => {  return post.user.name}
```

但是在服务器上，情况就完全不同了:

```
const getPostNameServer = post => {  const postAuthor = Users.findOne(post.userId)  return postAuthor.name}
```

当您试图在客户机和服务器之间共享代码以简化您的代码库时，这可能是一个问题。除此之外，这意味着您错过了 GraphQL 在服务器上进行数据查询的好方法。

最近，当我试图建立一个系统来生成每周时事通讯时，我感到很痛苦:每个时事通讯由多个帖子和评论组成，以及关于它们作者的信息；换句话说，这是 GraphQL 的完美用例。但是这个简讯生成是在*服务器*上发生的，这意味着我没有办法查询我的 GraphQL 端点…

#### 解决方案:服务器到服务器的 GraphQL 查询

或者我有吗？事实证明，您可以很好地运行服务器到服务器的 GraphQL 查询！只需[将您的 GraphQL 可执行模式传递给`graphql`函数](http://graphql.org/graphql-js/graphql/#graphql)，以及您的 GraphQL 查询:

```
const result = await graphql(executableSchema, query, {}, context, variables);
```

在 Vulcan 中，[我将这种模式概括为一个`runQuery`助手](http://docs.vulcanjs.org/server-queries.html)，并且我还为每个集合添加了`queryOne`函数。这些函数就像 MongoDB 的`findOne`一样，只是它们返回通过 GraphQL API 获取的文档:

```
const user = await Users.queryOne(userId, {  fragmentText: `    fragment UserFragment on User {      _id      username      createdAt      posts{        _id        title      }    }  `});
```

服务器到服务器的 GraphQL 查询帮助我极大地简化了代码。它让我将我的新闻稿生成调用从一堆连续的数据库调用和循环重构为一个 GraphQL 查询:

```
query NewsletterQuery($terms: JSON){  SiteData{    title  }  PostsList(terms: $terms){    _id    title    url    pageUrl    linkUrl    domain    htmlBody    thumbnailUrl    commentsCount    postedAtFormatted    user{      pageUrl      displayName    }    comments(limit: 3){      user{        displayName        avatarUrl        pageUrl      }      htmlBody      postedAt    }  }}
```

这里的要点是:不要把 GraphQL 仅仅看作一个纯粹的客户机-服务器协议。GraphQL 可以用于在任何情况下查询数据，包括客户端到客户端的 [Apollo 链接状态](https://github.com/apollographql/apollo-link-state)或者甚至在静态构建过程中与 [Gatsby](https://www.gatsbyjs.org/) 的连接。

![FuGWs40558hc3tICTQLvg0tjEZC1B3CsngvM](img/da138ec19b189f4193cb27b0c34cbf6d.png)

### 问题:多余的数据库调用

想象一个帖子列表，每个帖子都有一个用户。您现在想要显示其中的 10 篇文章，以及它们的作者姓名。

对于典型的实现，这意味着两次数据库调用。一个获取 10 篇文章，一个获取与这些文章对应的 10 个用户。

但是 GraphQL 呢？假设我们的帖子有一个带有自己的解析器的`user`字段，我们仍然有一个初始数据库调用来获取帖子列表。但是我们现在有一个额外的调用来获取每个解析器的每个用户*，总共有 **11 个**数据库调用！*

现在想象一下，每个帖子也有 5 条评论，每条评论都有作者。我们的电话数量现已激增至:

*   1 关于员额清单
*   帖子作者 10 个
*   5 条意见的每个子列表 10 条
*   评论作者 50

对总计 **71 个**数据库的调用来显示*单个视图*！

没有人想向他们的老板解释为什么主页需要 25 秒才能加载。谢天谢地，有一个解决方案:[数据加载器](https://github.com/facebook/dataloader)。

#### 解决方案:数据加载器

Dataloader 将允许您批处理和缓存数据库调用。

*   **批处理**意味着如果 Dataloader 发现您多次点击同一个数据库表，它会将所有调用一起批处理。在我们的例子中，10 个帖子作者和 50 个评论作者的调用都将被批量处理成一个调用。
*   **缓存**意味着如果 Dataloader 检测到两个帖子(或者一个帖子和一个评论)有相同的作者，它将重用它已经在内存中的用户对象，而不是进行新的数据库调用。

实际上，您并不总能实现完美的缓存和批处理，但是 Dataloader 仍然是一个巨大的帮助。

Vulcan 使使用 Dataloader 变得格外容易。开箱即用，[每个 Vulcan 模型都包括数据加载器函数](http://docs.vulcanjs.org/performance.html#Caching-amp-Batching)，作为“普通”MongoDB 查询函数的替代。

所以除了`collection.findOne`和`collection.find`，还可以用`collection.loader.load`和`collection.loader.loadMany`。

一个限制是 Dataloader 仅在使用文档 id 进行查询时有效。因此，您可以使用它来查询 ID 已知的文档，但是如果您想要查询最近创建的帖子，您仍然需要访问数据库。

![GxlQow63LtMbXmI1doulB29kJ9Bvpfmg3DbZ](img/845469bce494ffc7ede5b50b52f00aec.png)

### 问题:性能差

即使启用了 Dataloader，复杂视图仍然可以触发多个数据库调用，这反过来会导致加载时间变慢。

这可能会令人沮丧:一方面，您希望充分利用 GraphQL 的图形遍历特性(“显示帖子作者的评论作者…”等)。).但另一方面，你也不希望你的应用程序变得很慢，没有反应。

#### 解决方案:查询缓存

不过有一个解决方案，就是缓存整个 GraphQL 查询响应。与 Dataloader 不同，data loader 的范围仅限于当前请求(意味着它将只缓存同一查询中的文档*)，我在这里说的是在一段时间内缓存整个查询。*

阿波罗引擎是一个很好的方法。这是一个托管服务，提供关于 GraphQL 查询的分析，但它也有一个非常有用的[缓存功能](https://www.apollographql.com/docs/engine/caching.html)。

Vulcan 带有内置的引擎集成，您只需要将您的 API 密钥添加到您的设置文件中。然后，您可以将`enableCache: true`参数添加到 GraphQL 查询中，以使用 Engine 缓存它们。

或者，使用 Vulcan 的内置[数据加载高阶组件](http://docs.vulcanjs.org/resolvers.html#Higher-Order-Components):

```
withList({  collection: Posts,   enableCache: true})(PostsList)
```

这种方法的美妙之处在于，您可以轻松地控制哪些查询被缓存，哪些不被缓存，甚至对于同一个解析器也是如此。例如，您可能希望缓存您经常访问的主页上的最近文章列表，而不是您的归档页面上的完整文章列表。

最后一点:缓存可能并不总是可行的。例如，对于频繁更改的数据或依赖于当前登录用户的数据，这是不可取的。

![biuavoAHWIt5n3bDypomKTulQZywt2NZyN0L](img/bc1469df9ac0d9e6f6d14cff9f6fdd8d.png)

### 问题:样板文件过量

这绝不是 GraphQL 应用程序独有的问题，但它们确实通常需要您编写大量类似的样板代码。

通常，向您的应用添加新模型(例如`Comments`)将涉及以下步骤:

*   编写解析器来获取注释列表。
*   编写一个高阶组件(也称为容器)来加载注释列表。
*   或者，编写一个解析器，通过 ID 或 slug 以及相应的高阶组件获得单个注释。
*   编写用于插入新注释、编辑注释和删除注释的变体。
*   添加相应的表单和表单处理代码。

那是一大堆垃圾！

#### 解决方案:通用解析器、突变和高阶组件

Vulcan 的方法是为您提供智能、易用的通用选项。您将获得:

*   [默认解析器](http://docs.vulcanjs.org/resolvers.html#Default-Resolvers)用于显示文件列表和单个文件。
*   [预制高阶组件](http://docs.vulcanjs.org/resolvers.html#Higher-Order-Components)用于加载文件列表或单个文件。
*   [用于插入、编辑和删除文档的默认变异解析器](http://docs.vulcanjs.org/mutations.html#Default-Mutations)。
*   [生成的表单](http://docs.vulcanjs.org/forms.html)基于您的模式，适用于上述所有情况。

这些都是以足够通用的方式编写的，它们可以与任何开箱即用的新模型一起工作。

平心而论，这种“一刀切”的方法并非没有缺点。例如，因为查询是由通用的高阶组件动态生成的，所以使用[静态查询](https://dev-blog.apollodata.com/5-benefits-of-static-graphql-queries-b7fa90b0b69a)有点困难。

但这种策略仍然是一个很好的开始方式，至少在你有时间将应用程序的每个部分重构为一个更定制的解决方案之前是如此。

GraphQL 仍然相对较新，这意味着当每个人都忙于赞美它的优点时，很容易忽略构建 GraphQL 应用程序所涉及的真正挑战。

谢天谢地，这些挑战都有解决方案，我们讨论得越多(顺便说一下，Vulcan Slack 是一个很好的地方)!)，这些解决方案就会变得越好！