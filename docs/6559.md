# GraphQL API 类型解析器的一个主要的 5 行效率黑客

> 原文：<https://www.freecodecamp.org/news/a-5-line-major-efficiency-hack-for-your-graphql-api-type-resolvers-b58438b62864/>

by Vampiire

# GraphQL API 类型解析器的一个主要的 5 行效率黑客

![1*_5uB_PojUyPvVW0UkpMDUw](img/4c2cd6fe87faeff207362c5862e250bc.png)

Photo by [Karsten Würth (@inf1783)](https://unsplash.com/photos/ZKWgoRUYuMk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/efficient?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

使用 Apollo Server 和 Postgres-Sequelize，我们将创建一个概念证明，利用解析器函数的 info 参数将类型查询*的数据库负载减少 94%。

如果你已经熟悉了 Apollo 服务器解析器签名及其`info`参数，并且想要 [**跳到 hack，点击这里**](https://medium.com/@vampiire/a-5-line-major-efficiency-hack-for-your-graphql-api-type-resolvers-b58438b62864#a693) 。感谢我好奇的朋友[斯隆·布兰特利·瓜尔特尼](https://www.freecodecamp.org/news/a-5-line-major-efficiency-hack-for-your-graphql-api-type-resolvers-b58438b62864/undefined)向我透露了`info`参数的潜力。

### 背景—解析器函数签名

Apollo Server 从其文档中提供了以下解析器函数签名[:](https://www.apollographql.com/docs/graphql-tools/resolvers#Resolver-function-signature)

```
fieldName(obj, args, context, info) { result }
```

早在 4 月份，我就在签名上写了一些注释，以便教给一些不熟悉 GraphQL / Apollo Server 的队友。下面是我(稍微)修改过的签名版本:

```
(instance, arguments, context, info) { ...returning data... }
```

#### `**instance**`

> *obj / root / instance*

> `ex:`
> `Type: User`
> `Model: User, instance is ‘user’`
> `Type custom field resolver:`
> `user => user.property/.relationshipGette`【r】

#### `**arguments**`

> *参数/输入对象*
> *—查询或变异的参数*
> *—通常以输入类型对象的形式[在模式中定义]*
> *—输入是可重用的对象，可能包含许多字段，其中的一个子集适用于每个解析器*
> *—灵活的输入，可用于查询和变异解析器*
> *—解析器中的析构允许输入对象字段的选择性*

> `ex:`
> `UserInput: { id, username, avatar, githubID }`
> `resolver: (root, { id }) => User.findById(id);`
> `// destructures 1 of 4 UserInput fie` lds

#### `**context**`

> *context/CTX*
> *—context 对象在运行时注入 app.js*
> *的 Apollo 中间件声明中—它是解析器参数*
> *中最通用的一个—允许您传入诸如实用函数、数据库模型、认证用户等内容*
> *—通过在上下文中传递这些内容，您不再需要模型和助手的“require”语句。可以从解析器直接访问它们。*
> *—通常定义为嵌套对象，每个子上下文都有自己的对象*

#### `**info**`

> 不知道？

…直到一次偶然与斯隆的谈话导致了对这个**无用的**参数的讨论。Sloan 提到它包含了关于传入查询的信息。这让我开始提高分解器的效率。

### 信息参数

`info`对象保存了关于整个 API 模式的细节，以及我假设 Apollo Server 用于处理的其他信息。特别是，它保存了关于查询本身的信息—特别是请求的类型字段集。

#### 续集和严重低效的故事

事实证明，当 Sequelize(我相信每隔一个或/DM*)解析行或文档时，它会完整地解析它们。在前端/接收端，数据确实被调整到规格。但是在后端，这个过程似乎是:

`DB query for **entire** row/doc` → `map / custom resolver requested fields` → `filter data and resolve requested fields`

我在 Sequelize 上运行的一个用户查询的 Postgres echo 证明了这一点:

```
SELECT "id", "role", "email", ...wtf Sequelize..., "timezone", "country_id", "city_id", "created_at", "updated_at" FROM "users" AS "User" WHERE "User"."github_username" = 'the-vampiire' LIMIT 1;
```

Postgres 查询**选择** **所有 17 个字段**，而 API 查询本身只请求 1:

`user(username:”the-vampiire”) { id }`

#### 深入研究信息对象

稍微钻研一下`info`对象，我就能够到达我的目标:请求的字段。我的理论是，如果我知道这些字段，我可以将它们作为 Sequelize 查询对象的`attributes`属性传入，以减少 Postgres 服务器上的负载。

`info.fieldNodes[].selectionSet.selections[].name.value`

#### 笔记

*   `fieldNodes`是一个数组，`0th`元素是第一个查询。我猜这是一个支持批量查询的数组。
*   `selections`是一个数组，包含该类型的每个请求字段的对象
*   字段名本身隐藏在`selections.name.value`下

### 特雷·哈克

那么这一切意味着什么呢？用几行代码，一个模型(当然是从上下文来的！)，以及`info`的论点，我写了下面的实用程序和概念证明。它使用 query 对象的 Sequelize `attributes`属性只从请求的列中获取数据。

使用这个实用程序导致查询的大小减少了 94%。当然，这与请求的字段数量成比例。

使用 GraphQL API 的主要已知好处之一是，它允许前端请求所需形状和大小的有效负载。

使用我的实用程序允许后端通过类似地减少数据库服务器上的负载来反映相同的好处。

`mapAttributes`函数的第一行和最后一行确保只将直接映射的模型字段作为`attributes`传递给查询对象。

这可以防止因请求模型中不作为列存在的字段而导致的错误。

这可能是由需要自定义类型字段解析器(如类型关系或自定义类型字段名称)的字段引起的。

#### 概念证明

`User Type query: user(username:”the-vampiire”) { id }`

`before` →查询用户表的全部 17 个字段

```
SELECT "id", "role", "email", ...wtf Sequelize..., "timezone", "country_id", "city_id", "created_at", "updated_at" FROM "users" AS "User" WHERE "User"."github_username" = 'the-vampiire' LIMIT 1;
```

`after` →只查询单个请求字段

```
Executing (default): SELECT "status", "id" FROM "users" AS "User" WHERE "User"."github_username" = 'the-vampiire' LIMIT 1;
```

![1*uY5yPukkf28oxZUtcQVXFw](img/0e51e32f9ab8f5e20897b3dc60a272bf.png)

This hack is officially abided by The Dude

### 警告*

*   我没有用 Mongoose 或者其他流行的或者/DMs 测试过这个，但是原则上效果应该是一样的。`mapAttributes`函数和查询对象只需要一些定制。
*   我没有用请求不同字段的批处理查询对此进行测试(这可能会影响`info`的`fieldNodes`属性)。
*   不适用于重命名字段的自定义类型字段解析器
*   收益将根据相应模型上请求的类型字段与总字段的比率进行调整。

—鞋面