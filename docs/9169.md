# 您应该立即试用 GraphQL 的 4 个理由

> 原文：<https://www.freecodecamp.org/news/introduction-to-graphql-1d8011b80159/>

吉多·施密茨

# 您应该立即试用 GraphQL 的 4 个理由

![1*9Lp6ioBWW7wD2EZTZ7Mh9w](img/068b1dd7197b02dc6dd9e8647ec0ade3.png)

My version of the GraphQL logo that adds some love to it

尽管我已经开发(RESTful)API 好几年了，但我已经开始成为 GraphQL 的忠实粉丝。

在这篇文章中，我将向您介绍 GraphQL，以及您相对于 REST 的优势。让我们开始吧。

> GraphQL 是一种数据查询语言和运行时，自 2012 年以来在脸书设计和使用，用于向移动和网络应用程序请求和提供数据。

#### **为什么图 QL？**

*   一个端点访问您的数据
*   在单个请求中仅检索客户需要的数据(灵活性)
*   无需为您的视图定制端点
*   无版本控制

想象一下，有一个带有新闻提要的移动应用程序。

通常，您的数据会随着时间而变化，您需要引入新版本的 API，同时维护旧版本。这是必要的，因为其他用户仍然依赖于旧版本，其中那些新的数据更改尚未实现。

GraphQL 的优势之一是，它通过使用[类型系统](http://graphql.org/docs/typesystem/)来支持数据模型的灵活性。类型系统描述了服务器可以返回哪些类型的对象。

让我们使用 GraphQL 的 JavaScript 实现来描述一个人的类型:

#### **自省系统**

GraphQL 的另一个很酷的地方是它的[自省系统](http://graphql.org/docs/introspection/)。这允许我们询问我们的服务器它支持哪些查询。将其与内置文档进行比较。如果您不知道有哪些类型可用，您可以通过这个简单的查询轻松地询问 GraphQL:

```
{  __schema {    types {      name    }  }}
```

因此，当一个新的 iOS 开发人员必须从您的 API 中检索数据时，您可以很容易地将其称为文档。随着模式的发展，由于类型系统的原因，这将始终是最新的。酷吧？

> 更好的是，有一个浏览器内 IDE 库可以用来探索你的 API，叫做 [**GraphiQL**](https://github.com/graphql/graphiql) 。

#### **从 GraphQL 服务器查询数据**

使用类型系统定义数据模型后，您可以在 GraphQL 服务器上执行查询。在服务器上执行查询之前，您必须定义一个根查询类型:

现在，您可以在我们的服务器上执行查询了:

```
query PersonQuery {  person {    firstName    lastName    friends {      firstName    }  }}
```

所以你向服务器询问这个人的类型和他的相关朋友。没有端点返回包含所有字段和某种
类型的**的资源数组？包括**查询参数。 ***正是我们客户需要的数据*** 。

假设你也有一个新闻类型。在 GraphQL 中，可以一次检索多个类型。这意味着您可以通过一个请求获得不同的资源:

```
query HomepageQuery {  person {    firstName    lastName  }  news(limit: 10) {    title    excerpt    createdAt    person {      firstName      lastName    }  }}
```

如果您希望您的服务器支持这个查询，您必须稍微扩展您的根查询类型(注意:如果您正在使用这个代码，不要忘记定义新闻类型？):

创建根查询类型后，我们必须将其添加到模式中:

```
new GraphQLSchema({ query: Query });
```

#### **将数据变更到 GraphQL 服务器**

因此，使用**查询**类型，我们可以从服务器检索数据。我们还可以用 GraphQL 添加、更新或删除数据。我们可以通过在服务器上执行一个**变异**来做到这一点，并让服务器返回我们执行的变异的值。看一下这个例子:

```
mutation addPerson {  createPerson(    firstName: 'John',    lastName: 'Doe'  ) {    firstName    lastName  }}
```

定义变异就像定义根查询类型一样。我们唯一需要做的就是将它添加到我们的模式中。因此，我们更新后的模式如下所示:

```
new GraphQLSchema({ query: Query, mutation: Mutation });
```

> 关于查询格式的更多信息可以在官方[文档](http://graphql.org/docs/queries/)中找到。

我希望我已经让您对 GraphQL 有了基本的了解。如有任何问题，欢迎发微博给我 [**@guidsen**](https://twitter.com/guidsen) 。

我还每周向我的订户发送关于 JavaScript & ReactJS 的问题。
[**订阅这里获取 JavaScript 知识**](https://www.getrevue.co/profile/guidsen) **。**

哦，点击？下面，所以其他人会看到这篇文章在媒体上。感谢阅读。

![1*prif7-04oPf8Dqo1gvSDsQ](img/45675c0b22e293d37aee8c65292fa5e9.png)