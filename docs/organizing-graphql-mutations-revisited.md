# 如何组织 GraphQL 的变异

> 原文：<https://www.freecodecamp.org/news/organizing-graphql-mutations-revisited/>

# 背景

前一段时间[我写了一篇文章](https://www.freecodecamp.org/news/organizing-graphql-mutations-653306699f3d/)关于如何在层次结构中组织 GraphQL 的变异，很像[有时对查询](https://blog.hasura.io/graphql-and-tree-data-structures-with-postgres-on-hasura-dfa13c0d9b5f/#fetching-comments)所做的。

过了一会儿，Anders Ringqvist 告诉我，我的方法有一个问题:它不能保证突变会按顺序执行。这很重要，因为与查询不同，突变会修改状态，并且一个突变可能需要完成前一个突变才能正确操作。

在核实这一点后，我写了一篇[后续文章](https://www.freecodecamp.org/news/beware-of-graphql-nested-mutations-9cdb84e062b5/)作为各种撤回。

# 绝不放弃

马修·拉尼根最近联系了我，并提出了一个非常简单的基于承诺的机制来确保“嵌套”突变的执行顺序。它非常优雅，似乎没有任何副作用。

# 该机制

下面是一个对图书集合进行操作的平面突变集的示例:

```
mutation {
  addBook(ISBN: "978-3-16-148410-0", title: "Schematics of the Illudium Q-36 Explosive Space Modulator", author: "Martian, Marvin") {
    references {
      title
    }
  },
  updateBook(ISBN: "978-3-16-148410-0", title: "Overview of the Illudium Q-36 Explosive Space Modulator") {
     success
  }
} 
```

以下是为使用单个图书实体而重写的相同操作:

```
mutation {
  Book(ISBN: "978-3-16-148410-0") {
    add(title: "Schematics of the Illudium Q-36 Explosive Space Modulator", author: "Martian, Marvin") {
      references {
        title
      }
    },
    update(title: "Overview of the Illudium Q-36 Explosive Space Modulator") {
     success
    }
} 
```

后一个例子的问题是，我不能确定 **add** 会在 **update** 之前执行。既然我可以更新一本还没加的书，那就成问题了。

# 解决方案

Mathew 优雅而简单的方法将变异操作封装在父类中。当构造这个类时，它会创建一个包含[承诺](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)的**承诺**字段。该承诺立即得到解决，因此任何随后的 **then** 条款将立即被援引。

# 一个例子

```
class Sequential {  
   constructor() { this.promise = Promise.resolve() }
} 
```

顺序类封装了变异。它实际上并没有变异任何东西，它只是一个变异操作的容器。接下来让我们添加一个变异操作:

```
const msg = (id, wait) => new Promise(resolve => {
  setTimeout(() => {
    console.log({id, wait})
    let message = `response to message ${id}, wait is ${wait} seconds`;
    resolve(message);
  }, wait)
})

class Sequential {
  constructor() {
    this.promise = Promise.resolve()
  }

  message({id, wait}) {
    this.promise = this.promise.then(() => msg(id, wait))
    return this.promise
  }} 
```

注意，**消息**突变等待当前承诺解决。由于在构造函数中调用的承诺会被立即解析，如果是第一个被调用的变异，那么`this.promise.then(...)`语句将立即执行*。如果不是第一次突变，那么它会等待前一次突变的解决。下面的代码将会清楚地说明这一点。*

注意，外部的 **msg()** 函数也返回一个承诺。它被编写为异步行为，仅当传入的等待时间过期时才进行解析。这种延迟执行的方法在测试过程中非常方便。

## 该模式

GraphQL 模式非常基本:

```
type Query {
  noop: String!
}

type MessageOps {
  message(id: ID!, wait: Int!): String!
}

type Mutation {
  Sequential: MessageOps
} 
```

我可以根据需要向 MessageOps 添加任意多的操作，并在 Sequential 类中实现这些操作，如前所示。GraphQL 服务器需要至少一个查询定义，因此 **noop** 查询(什么也不做)履行了这一义务。

## 解析器

解析器的代码很简单:

```
const resolvers = {
  Mutation: {
    Sequential: () => new Sequential(),
  }
} 
```

## 死刑

现在我们可以执行我的突变操作，看看会发生什么。有两件事需要注意:1)突变调用的响应，以及 2)控制台输出。

后者很重要，因为只有控制台输出会指示突变被调用和完成的顺序。

测试如下:

```
mutation sequential {
  Sequential {
    message1: message(id: 1, wait: 3000)
    message1a: message(id: 11, wait: 2500)
  }
  Sequential {
    message2: message(id: 2, wait: 1000)
    message2a: message(id: 22, wait: 750)
  }
  Sequential {
    message3: message(id: 3, wait: 500)
    message3a: message(id: 33, wait: 250)
  }
  Sequential {
    message4: message(id: 4, wait: 100)
    message4a: message(id: 44, wait: 50)
  }
} 
```

回应如下:

```
{
  "data": {
    "Sequential": {
      "message1": "response to message 1, wait is 3000 seconds",
      "message1a": "response to message 11, wait is 2500 seconds",
      "message2": "response to message 2, wait is 1000 seconds",
      "message2a": "response to message 22, wait is 750 seconds",
      "message3": "response to message 3, wait is 500 seconds",
      "message3a": "response to message 33, wait is 250 seconds",
      "message4": "response to message 4, wait is 100 seconds",
      "message4a": "response to message 44, wait is 50 seconds"
    }
  }
} 
```

这看起来好像一切都是按顺序发生的，但这可能具有欺骗性:返回的 JSON 中结果的顺序与变异调用的顺序相匹配，但该顺序可能与变异完成的顺序不同。每次调用 msg()的控制台输出都会列出实际的执行顺序:

```
{ id: '1', wait: 3000 }
{ id: '11', wait: 2500 }
{ id: '2', wait: 1000 }
{ id: '22', wait: 750 }
{ id: '3', wait: 500 }
{ id: '33', wait: 250 }
{ id: '4', wait: 100 }
{ id: '44', wait: 50 } 
```

因此，我们有证据表明，突变实际上是以正确的顺序发生的:msg #1(需要 3000 毫秒)在 msg #44(只需要 50 毫秒)之前完成执行。这是因为每个变异操作只有在前一个变异完成时才会被调用。瞧啊。

在我等待被告知下一个“问题”时，请在这里随意查看完整的 github 项目[。它包含一些额外的好东西，你可以运行你自己的 GraphQL 变异(见 **testMutations.gql** 开始)。](https://github.com/JeffML/nested_mutations)