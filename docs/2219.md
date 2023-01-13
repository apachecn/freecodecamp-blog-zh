# GraphQL 与 REST——优势和代码示例比较

> 原文：<https://www.freecodecamp.org/news/graphql-vs-rest-benefits-and-code-example-comparisons/>

REST 并不是第一个通过网络发送信息的协议。但是十多年来，它一直主导着 API 领域。

最近，脸书设计的新人 GraphQL 变得越来越受欢迎。它旨在纠正 REST 的一些弱点，但没有一种技术是完美的。

与 REST 相比，GraphQL 的优势是什么，为什么在项目中会使用其中一个？

## REST APIs 的问题

首先，让我们讨论 REST 的一些弱点以及 GraphQL 试图如何解决它们。主要有三个原因:对服务器的过度访问、提取过量/不足，以及普遍缺乏灵活性。

### 使用 REST APIs 访问服务器的次数太多

假设我们正在创建一个社交媒体应用程序。提要可能会显示所有用户的最新帖子，以及用户名和个人资料图片。

例如，在 REST 中，您需要向 */api/posts* 发送一个 GET 请求来获取文章，这可能会返回一个 JSON 对象，其中包含文章标题、内容、标签、日期，可能还有用户 ID。

然后，您可能需要为每篇文章向 */api/users/:id/* 发送一个 GET 请求，以获取用户的用户名、头像和任何其他相关信息。

当您考虑到您可能为每个用户发出一个 GET 请求时，对于一个页面来说，这是一个很大的来回！

有了 GraphQL，您只需到服务器一趟，就能得到您需要的一切:

```
query {
    posts {
        title,
        content,
        tags,
        date,
        user {
        	username,
            avatar,
            catchphrase,
            favorite_dog
        }
    }
}
```

在小范围内，多次访问服务器没什么大不了的。但是一旦处理了大量的数据，将 API 调用减少到最少显然会让您受益匪浅。GraphQL 使这一点很容易实现。

### REST APIs 中的过量提取和不足提取

一个相关的问题是过度提取和提取不足。在 REST API 中，当您到达一个端点时，您将总是得到相同的数据，而不管您是否需要所有的数据。

比如说我们只需要某个人的用户名和头像。如果 */user/:id* 返回他们的用户名、头像、标语和最喜欢的狗的品种，无论你是否想要，你都将得到所有这些信息。

另一方面，您可以在抓取下结束*，这需要返回到服务器获取更多信息，如前一节所述。*

为了显示单个用户的帖子，我们需要用户信息和帖子的内容。如果我从用户端点获取用户，我仍然需要点击 posts 端点，并使用 userid 检索帖子。

```
// First we get the user's info
GET /api/users/42

{
    "username": "Mr. T",
    "avatar": "http://example.com/users/42/pic.jpg",
    "catchphrase": "I pity the fool",
    "favorite_dog": "beagle"
}

// Then we get their posts
GET /api/users/42/posts

{
    "posts": [{
        "title": "Hello World",
        "content": "Hi everyone!"
        "tags": "first post"
        "date": "July 1, 2020"
    }]
    // etc.
} 
```

正如我们在前面的例子中看到的，GraphQL 通过允许用户使用一个端点并且只获取他们需要的东西来解决这个问题。

### REST APIs 缺乏灵活性

扩展前一点，REST 依赖于创建符合前端需求的 API。如果您可以预测前端在到达特定端点时需要什么，那么您可以精确地定制检索到的数据以匹配该视图。

当视图相对静态时，这种方法很有效。但是，如果您的前端经常变化，您将需要一个在返回什么数据方面具有更大灵活性的 API。

类似地，如果您的 API 被各种具有不同需求的不同客户端使用，REST API 的不灵活性将不会满足您的目的。

GraphQL 通过允许检索不同的数据配置来提供这种灵活性。

```
// If I just need the username and avatar:

query {
	users {
    	username,
        avatar
    }
}

// If I need their favorite dog breed, too.

query {
    users {
        username,
        avatar,
        favorite_dog
    }
} 
```

## 应该用 Rest 还是 GraphQL？

从这篇文章来看，GraphQL 似乎总是比 REST 好，但事实未必如此。构建应用程序时所做的每个架构决策都有其利弊，这也不例外。

以下是一些需要考虑的事项:

### 如果需要好用的东西，选择 GraphQL。

正确使用 REST 有一个学习曲线，如果您还不知道，那么如果您使用 GraphQL，您将更容易创建一个优秀的 API。

> 通过使用 GraphQL，您通常会得到一个比在不理解其概念的情况下尝试构建 REST API 更好的 API。- [兹德内克“Z”内梅克](https://goodapi.co/blog/rest-vs-graphql)

### 如果您使用 GraphQL，请决定如何处理错误

REST APIs 能够更好地利用 HTTP 的错误报告特性。如果您不希望客户端错误返回 200 OK 状态(这在 GraphQL 中很常见)，那么您需要更多地考虑错误处理。

### 休息可能对微服务更好

如果你在后台使用微服务，REST 可能会更好地满足你的需求，因为它是用来分离关注点的。

如果您不需要使用可能用不同编程语言编写的不同的、完全不同的资源，GraphQL 的统一数据“图表”就非常有用，但是如果您有一个更加分布式的后端，就没那么有用了。

### 考虑缓存

缓存是 REST 内置的功能，但是您必须使用 GraphQL 来管理自己。如果您没有在适当的地方构建缓存，那么您从 GraphQL 更有针对性的获取中获得的所有提高的效率都可能被抹去。

## 结论

和所有事情一样，在决定使用 REST 还是 GraphQL 时，需要考虑一些权衡。为您的项目选择哪一个将取决于您的需求和资源。