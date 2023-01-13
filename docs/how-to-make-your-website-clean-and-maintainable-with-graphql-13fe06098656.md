# 如何用 GraphQL 让你的网站整洁可维护

> 原文：<https://www.freecodecamp.org/news/how-to-make-your-website-clean-and-maintainable-with-graphql-13fe06098656/>

REST API 服务、SQL 数据库、markdown 文件、文本文件、SOAP 服务……您还能想到其他存储和交换数据和内容的方法吗？生产网站通常使用几种不同的服务和方法来存储数据，那么如何保持实现的整洁和可维护性呢？

每个 Node.js 网站，不管是单页应用还是常规网站，都需要连接到第三方服务或系统。至少它需要从 markdown 文件或无头 CMS 中获取内容。但是对其他服务的需求很快浮出水面。首先，它是一个联系表单——您需要存储它的提交内容。然后是全文搜索——您需要找到一种服务，使您能够创建索引并搜索它们。根据项目的大小，这个列表还会继续下去。

![1*zvX9FdKVxyLZxutsbo7Igw](img/b02db394b9a0c414dd3a5812241ba689.png)

这有什么问题？嗯，一开始没什么。当你被激励去完成一个项目时，你为每一个功能创建一个组件。通信被封装在各自的组件中，经过一些快速测试后，您很高兴它全部工作。客户很高兴项目在截止日期前交付，作为副作用，您也成为了内容即服务 API、表单提交服务和自动搜索索引重建方面的专家。

你很快就建立并运行了网站，所以你得到了提升！以及对项目及其细节的了解。

几周后，你的同事被要求对项目做一些修改。客户希望使用不同的搜索提供商，因为原来的提供商太贵了。开发人员还在开发另一个需要联系表单的项目，所以他们考虑使用相同的组件，但是将提交内容存储在不同的服务中。所以他们来找你，询问你实现的细节。

![1*Ek4R9z9_Rp9DP0mVcPhDcw](img/60dc0e90f0550b7ecff1cfbcea14c0f4.png)

[Image source](https://www.reddit.com/r/ProgrammerHumor/comments/8pdebc/only_god_and_i_knew/)

当你最终放弃搜索你的记忆时，他们将需要做和你最初做的一样的研究来找出实现。UI 与功能紧密耦合，以至于当他们想要重用组件时，他们可能会从头开始再次实现它们(并且可能会复制粘贴旧代码的一部分)。

![1*n1ukAvnQbjcoCCQTRsfa7w](img/4b8a968614f433e0692d2085412bf82d.png)

Decoupled infrastructure showing GraphQL communication and specific GraphQL resolvers

# 正确的抽象层次

那么我们如何避免这些问题来保持代码的可维护性和整洁呢？看一下上面的图，我把与第三方服务和 UI 的通信进行了划分。每个外部服务 API 的细节都在网站后端的中间件中实现。前端的组件都使用一种方式来获取和提交数据——graph QL。

# GraphQL

那么 GraphQL 是什么，为什么要用它来进行前端和后端的通信呢？GraphQL 是一种查询语言，一种协议，正是为了这个目的而建立的——将网站前端需要的数据从获取这些数据所需的查询中分离出来。从功能的角度来看，它类似于 REST API，因为它使您能够查询数据。更多信息请查看 GraphQL 主页。

主要的区别在于你索取数据的方式。假设一个新的项目开发人员的任务是创建一个博客页面。该页面应该显示存储在无头 CMS 中的博客文章。我使用的是 [Kentico Cloud](http://bit.ly/2QzUALM) ，这是一个内容即服务(CaaS)平台，允许您以清晰的层次结构存储各种类型的内容，并通过 REST API 获取内容。因此，使用 REST API 对数据的`GET`请求可能是这样的:https://deliver . kenticocloud . com/{ projectID }/items？system.type =博客 _ 帖子

示例响应将是:{
" items ":[
{
" system ":{
" id ":" 0282 e86e-8 c72–47 F3–9d3d-2 ACF 93 a 8986 b "，
...
" last _ modified ":" 2018–09–18t 10:38:19.8406343 z "
}，
" elements ":{
" Title ":{
" type ":" text "，
"name":"Title "，
"value ":"来自新开发者传道者的你好"
}，
"content":{
...
}
...
}
}
]
}

响应包含 JSON 格式的所有博客文章的数据。由于页面只显示博客文章的列表，许多返回的数据(以`content`字段开始)是多余的，因为我们不需要显示它们。为了节省带宽(通常需要付费)，开发人员需要使用额外的`columns`过滤器:https://deliver . kenticocloud . com/{ projectID }/items？system.type=blog_post &元素=标题、图片、摘要

他们需要知道 API 的细节，并且在构建查询时，可能会在另一个浏览器窗口中打开它的引用。

使用 GraphQL 获得相同的数据要容易得多。它的模式本身描述了前端能够呈现的内容。开发人员需要在图形符号中指定要获取的数据:query BlogPosts {
getBlogPosts {
elements {
title
image
teaser
}
}
}

**(在此找到更多 GraphQL 查询的例子** [**为什么是 GraphQL？**](http://bit.ly/2WVm1mr)**Shankar Raju 的文章。)**

现在，当您决定将内容存储从 headless CMS 切换到 markdown 文件或 SQL 数据库时，博客页面的实现不会改变。GraphQL 查询看起来还是一样的。

这怎么可能呢？让我们看一下引擎盖下面。前端实现与外部服务的分离是使用以下部分实现的:

*   GraphQL 模式
*   GraphQL 解析器
*   阿波罗服务器

![1*WSbvAW_j4esd86H3z2zpdw](img/072223916dbdfd30c26326edaf37eb81.png)

# GraphQL 模式

GraphQL 模式非常像类图。它指定了数据模型，如`BlogPost`或`FormSubmission`，以及 GraphQL 查询。

![1*AKWViU8GCa0RIebR5nWUgA](img/dc4fadb5c5bce9a87e2027866c7d740d.png)

上面你可以看到一个简单网站的数据模型模式的例子。注意还有像`SystemInfo`或者`AssetElement`这样未定义的类型。我在图中省略了它们，因为它们将由 headless CMS 类型生成器自动生成。

![1*Sy_9h3871bq5A2vH5x_WOg](img/954a32888319b47eef4ee1c126d2b6e8.png)

查询和变异(可能修改和存储数据的调用)然后描述如何获取和操作这些模型中的数据，比如获取`BlogPost`的数据或提交`FormSubmission`。它就像是网站中间数据层的类图。

![1*YLBKXWHdzSygxsCqNjjn-Q](img/d67a5d61c0af2a751d07729442c864de.png)

# 下决心者

解析器是上面定义的查询的实际实现，比如 MySQL 解析器、Kentico Cloud 解析器等等。它们被分配给模式的特定查询，并负责处理这些查询。因此，当前端组件想要使用 GraphQL 查询`getBlogPosts`获取博客文章时，服务器会选择并调用该查询的注册解析器(Kentico Cloud resolver)。解析器使用 headless CMS 的 REST API 获取 JSON 中的内容，并将其作为对象数组返回给组件。

![1*8AyVZndOfiIajt63M0bjuw](img/e47d45e9308dba277f1f9ad9e7feb119.png)

在这个简单的例子中，解析器与查询和变异 1:1 匹配，但是一个解析器可以与它所能处理的尽可能多的查询和变异进行签约。MySQL 解析器目前没有任何作用，但以后当网站功能增加时可能会派上用场，我们决定使用数据库在本地存储一些敏感的用户输入。

# 阿波罗将它们联系在一起

拼图的最后一块是阿波罗服务器。是胶水把所有这些部分连接起来。Apollo 是一个库，一个框架，它将 GraphQL schema 连接到 Node.js 中的 HTTP 服务器。我个人使用 Express 作为 HTTP 服务器，但你也可能[喜欢 Connect、Restify 或 Lambda](http://bit.ly/2IeLR1w) 。

Apollo 有两个部分——服务器和客户端。服务器充当 GraphQL 模式的主机，并处理 GraphQL 请求。因此，只要前端调用 GraphQL 查询，Apollo server 就会查找正确的解析器，等待它处理数据并传递它的响应。当您需要与一个本身不支持 GraphQL 的系统集成时，Apollo server 通常用作从任何服务接口到 GraphQL 的简单转换器。

Apollo client 是一个插入网站前端的模块，支持 GraphQL 查询的执行。

# 加快速度的样板文件

在本文中，我解释了如何分离关注点，隔离第三方服务连接器，并在不知道所有使用的服务的细节的情况下，使用 GraphQL 实现前端组件的快速开发。

我的[下一篇文章](http://bit.ly/2IsgznK)和[的现场演示](http://bit.ly/2GGHIB5)将更深入地探讨如何使用 Apollo 和 GraphQL 模式，展示如何定义模式和实现解析器。它还[展示了一个样板](http://bit.ly/2TGTmPW)，已经为你的开发设置好了所有这些工具。