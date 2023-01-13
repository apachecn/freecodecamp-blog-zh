# 当心 GraphQL 嵌套突变！

> 原文：<https://www.freecodecamp.org/news/beware-of-graphql-nested-mutations-9cdb84e062b5/>

*“我有一个巧妙的计划…”*

很久以前，我偶然发现了通过在返回类型中嵌套操作来组织 GraphQL 突变的概念。想法是这些操作会使父实体变异。

基本想法是这样的:

```
input AddBookInput {
            ISBN: String!
            title: String!
        }

input RemoveBookInput {
            bookId: Int!
        }

input UpdateBookInput {
          ISBN: String!
          title: String!
      }

type AuthorOps {
          addBook(input: AddBookInput!): Int
          removeBook(input: RemoveBookInput! ): Boolean
          updateBook(input: UpdateBookInput!): Book
      }

type Mutation {
        Author(id: Int!): AuthorOps
      }
```

我使用过几次这种技术，没有不良影响，但我很幸运。问题出在哪里？

[一位读者让我](https://medium.com/@anddoutoi/hey-jeff-bca074856669)去[了解 GraphQL GitHub 网站上的一个问题](https://github.com/graphql/graphql-js/issues/221)，据说**嵌套突变**的执行顺序没有保证。啊哦。在上面的例子中，我肯定希望在尝试对同一本书进行 **updateBook** ()操作之前，发生 **addBook** ()突变。唉，只有所谓的**根** **突变**才能保证按顺序执行。

### 问题的例证

假设我有一个消息队列，我希望消息按照接收的顺序存储在那里。有些消息需要更长的时间来处理，所以我使用了一种变异来保证消息按顺序处理:

```
type Query {
  noop: String!
}

type Mutation {
  message(id: ID!, wait: Int!): String!
}
```

解析器记录消息到达的时间，然后在返回变异结果之前等待给定的时间:

```
const msg = (id, wait) => new Promise(resolve => {
  setTimeout(() => {

console.log({id, wait})
    let message = `response to message ${id}, wait is ${wait} seconds`;

resolve(message);
  }, wait)
})

const resolvers = {
  Mutation: {
    message: (_, {id, wait}) => msg(id, wait),
  }
}
```

现在开始试运行。我希望确保控制台日志消息的顺序与变异请求的顺序相同。请求如下:

```
mutation root {
  message1: message(id: 1, wait: 3000)
  message2: message(id: 2, wait: 1000)
  message3: message(id: 3, wait: 500)
  message4: message(id: 4, wait: 100)
}
```

回应是:

```
{
  "data": {
    "message1": "response to message 1, wait is 3000 seconds",
    "message2": "response to message 2, wait is 1000 seconds",
    "message3": "response to message 3, wait is 500 seconds",
    "message4": "response to message 4, wait is 100 seconds"
  }
}
```

控制台日志显示:

```
{ id: '1', wait: 3000 }
{ id: '2', wait: 1000 }
{ id: '3', wait: 500 }
{ id: '4', wait: 100 }
```

太好了！即使第二条和后续消息比前一条花费的时间更少，它们也会按照收到的顺序进行处理。换句话说，突变是按顺序执行的。

#### 美中不足的是

现在让我们嵌套这些操作，看看会发生什么。首先我定义了一个 **MessageOps** 类型，然后添加了一个**嵌套**变异:

```
const typeDefs = `
type Query {
  noop: String!
}

type MessageOps {
  message(id: ID!, wait: Int!): String!
}

type Mutation {
  message(id: ID!, wait: Int!): String!
  Nested: MessageOps
}`
```

我的变异现在通过嵌套解析器，返回 MessageOps，然后我用它来执行我的消息变异:

```
mutation nested {
  Nested {
    message1: message(id: 1, wait: 3000)
    message2: message(id: 2, wait: 1000)
    message3: message(id: 3, wait: 500)
    message4: message(id: 4, wait: 100)
  }
}
```

与我们之前看到的非常相似，对突变请求的响应看起来也几乎相同:

```
{
  "data": {
    "Nested": {
      "message1": "response to message 1, wait is 3000 seconds",
      "message2": "response to message 2, wait is 1000 seconds",
      "message3": "response to message 3, wait is 500 seconds",
      "message4": "response to message 4, wait is 100 seconds"
    }
  }
}
```

唯一的区别是响应被打包在一个嵌套的 JSON 对象中。可悲的是，控制台揭示了一个悲惨的故事:

```
{ id: '4', wait: 100 }
{ id: '3', wait: 500 }
{ id: '2', wait: 1000 }
{ id: '1', wait: 3000 }
```

它揭示了消息是无序处理的:处理最快的消息首先被发布。

好吧。[在我最初帖子的代码](https://github.com/JeffML/graphql-crud2)中，我实际上做了一些更像下面这样的事情:

```
mutation nested2 {
  Nested {
    message1: message(id: 1, wait: 3000)
  }
  Nested {
    message2: message(id: 2, wait: 1000)
  }
  Nested {
    message3: message(id: 3, wait: 500)
  }
  Nested {
    message4: message(id: 4, wait: 100)
  }
}
```

也许这行得通？每个变异操作都在它自己的嵌套根变异中，所以我们可能期望嵌套变异顺序执行。该响应与之前的响应相同:

```
{
  "data": {
    "Nested": {
      "message1": "response to message 1, wait is 3000 seconds",
      "message2": "response to message 2, wait is 1000 seconds",
      "message3": "response to message 3, wait is 500 seconds",
      "message4": "response to message 4, wait is 100 seconds"
    }
  }
}
```

但是控制台日志也是如此:

```
{ id: '4', wait: 100 }
{ id: '3', wait: 500 }
{ id: '2', wait: 1000 }
{ id: '1', wait: 3000 }
```

#### 这是怎么回事？

“问题”是 GraphQL 执行嵌套变异，返回一个具有进一步变异方法的对象。不幸的是，一旦该对象被返回，GraphQL 就开始处理下一个变异请求，不知道在请求中还有更多变异操作要执行。

GraphQL 非常简单，但是简单是有代价的。可以想象，嵌套突变也可以得到支持，比如说通过添加一个**类型的变异器(其必然结果是 [****输入**** 类型](https://graphql.org/graphql-js/mutations-and-input-types/))，GraphQL 会将其视为突变操作的扩展。目前，变异请求中没有足够的信息来知道嵌套操作也是变异函数。**

### **组织 GraphQL 变异，第 2 部分**

**您仍然可以将这种技术用于顺序不相关的操作，但是如果违反了这种假设，那么这种假设可能会变得脆弱并且难以调试。或许模式拼接或者 T2 编织提供了一个答案。我希望在以后的文章中探索这些方法。**

**本文使用的完整 NodeJS 应用程序可以在[这里](https://github.com/JeffML/nested_mutations)找到。**