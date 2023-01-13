# 使用 GraphQL 工具+编辑图形 SQL

> 原文：<https://www.freecodecamp.org/news/mocking-graphql-with-graphql-tools-42c2dd9d0364/>

#### 如何用真实的值模拟您的 GraphQL API

在我的上一篇文章[，](https://medium.com/@jefflowery/declarative-graphql-with-graphql-tools-cd1645f94fc)中，我采用了最初的 Apollo LaunchPad [帖子和作者 API](https://launchpad.graphql.com/1jzxrj179) ，并将其分解为域和组件。我想说明如何使用 [graphql 工具](https://github.com/apollographql/graphql-tools)来组织一个大型的 GraphQL 项目。

现在我希望 API 在我查询时返回模拟数据。怎么会？

### 原始来源

在最初的 Apollo Launchpad 示例中，我们使用静态数据结构和简单的映射解析器为查询提供输出。

例如，给定以下查询:

```
# Welcome to GraphiQL

query PostsForAuthor {
  author(id: 1) {
    firstName
    posts {
      title
      votes
    }
  }
}
```

输出将是:

```
{
  "data": {
    "author": {
      "firstName": "Tom",
      "posts": [
        {
          "title": "Introduction to GraphQL",
          "votes": 2
        }
      ]
    }
  }
}
```

resolvers 对象具有将作者映射到帖子的功能，反之亦然。不过，这并不是真正的模仿。

问题是，关系越多，实体变得越复杂，需要进入解析器的代码就越多。那么就需要提供更多的数据。

当谈到测试时，测试有时可能会揭示数据或解决方案中的问题。你真的想集中测试 API 本身。

### 使用模拟

有三个 Node.js 模块使得模拟 API 变得快速而简单。第一部分是`graphql-tools` 模块。使用该模块，第一步是将模块中的方法`addMockFunctionsToSchema` 要求或导入到根`schema.js`文件中:

```
import {
    makeExecutableSchema,
    addMockFunctionsToSchema
} from 'graphql-tools';
```

然后，在通过调用`createExecutableSchema`、**和**创建了一个可执行文件`schema`之后，您可以像这样添加您的模拟:

```
 addMockFunctionsToSchema({
        schema: executableSchema,
    })
```

下面是根`schema.js`的完整列表:

```
// This example demonstrates a simple server with some relational data: Posts and Authors. You can get the posts for a particular author,
// and vice-versa Read the complete docs for graphql-tools here: http://dev.apollodata.com/tools/graphql-tools/generate-schema.html

import {
    makeExecutableSchema,
    addMockFunctionsToSchema
} from 'graphql-tools';

import {
    schema as authorpostsSchema,
    resolvers as authorpostsResolvers
} from './authorposts';

import {
    schema as myLittleTypoSchema,
    resolvers as myLittleTypeResolvers
} from './myLittleDomain';

import {
    merge
} from 'lodash';

const baseSchema = [
    `
    type Query {
        domain: String
    }
    type Mutation {
        domain: String
    }
    schema {
        query: Query,
        mutation: Mutation
    }`
]

// Put schema together into one array of schema strings and one map of resolvers, like makeExecutableSchema expects
const schema = [...baseSchema, ...authorpostsSchema, ...myLittleTypoSchema]

const options = {
    typeDefs: schema,
    resolvers: merge(authorpostsResolvers, myLittleTypeResolvers)
}

const executableSchema = makeExecutableSchema(options);

addMockFunctionsToSchema({
    schema: executableSchema
})

export default executableSchema;
```

schema.js

那么产量是多少呢？执行与之前相同的查询会产生:

```
{
  "data": {
    "author": {
      "firstName": "Hello World",
      "posts": [
        {
          "title": "Hello World",
          "votes": -70
        },
        {
          "title": "Hello World",
          "votes": -77
        }
      ]
    }
  }
}
```

嗯，这有点傻。每一个字符串都是“Hello World”，投票都是否定的，每个作者总会恰好有两个帖子。我们会解决这个问题，但首先…

#### 为什么使用模拟？

单元测试中经常使用模拟来将被测试的功能与这些功能所依赖的依赖项分开。你想要测试的是功能(单元)，而不是整个复杂的功能。

在开发的早期阶段，模拟还有另一个目的:测试测试。在基本测试中，您首先要确保测试正确地调用了 API，并且返回的结果具有预期的结构、属性和类型。我想酷小孩把这叫做“形”。

这提供了比可查询数据结构更有限的测试，因为没有强制引用语义。 `id`毫无意义。尽管如此，模拟还是提供了一些东西来组织你的测试

### 现实嘲讽

有一个模块叫[休闲](https://github.com/boo1ean/casual)我很喜欢。它为许多常见的数据类型提供了合理的可变值。如果你在疲惫的同事面前展示你的新 API，看起来你确实做了一些特别的事情。

这里有一个要显示的模拟值的列表:

1.  作者的名字应该是**像名字一样的**
2.  文章标题应该是长度有限的可变文本
3.  投票应该是正数或零
4.  员额数目应在 1 至 7 个之间

首先要创建一个名为`mocks`的文件夹。接下来，我们将使用模拟方法向该文件夹添加一个`index.js`文件。最后，定制模拟将被添加到生成的可执行模式中。

**临时**库可以通过数据类型(`String, ID, Int, …`)或属性名生成值。此外，graphql-tools MockList 对象将用于改变列表中的项目数量——在本例中为`posts`。那我们开始吧。

`Import`既随意又模仿成`/mocks/index.js`:

```
import casual from 'casual';
import {
    MockList
} from 'graphql-tools';
```

现在让我们创建一个具有以下属性的默认导出:

```
export default {
    Int: () => casual.integer(0),

    Author: () => ({
        firstName: casual.first_name,
        posts: () => new MockList([1, 7])
    }),

    Post: () => ({
        title: casual.title
    })
}
```

`Int`声明负责我们模式中出现的所有整数类型，它将确保`Post.votes`为正数或零。

接下来，`Author.firstName`将是一个合理的名字。MockList 用于确保与每个作者相关的帖子数量在 1 到 7 之间。最后，casual 会为每个`Post`生成一个**lorem ipsum**T1。

现在，每次执行查询时，生成的输出都会发生变化。这看起来很可信:

```
{
  "data": {
    "author": {
      "firstName": "Eldon",
      "posts": [
        {
          "title": "Voluptatum quae laudantium",
          "votes": 581
        },
        {
          "title": "Vero quos",
          "votes": 85
        },
        {
          "title": "Doloribus labore corrupti",
          "votes": 771
        },
        {
          "title": "Qui nulla qui",
          "votes": 285
        }
      ]
    }
  }
}
```

### 生成自定义值

我只是触及了休闲能做什么的表面，但它是有据可查的，没有什么可补充的。

但是，有时有些值必须符合标准格式。我再介绍一个模块: [randexp](https://www.npmjs.com/package/randexp) 。

randexp 允许您生成符合您提供的正则表达式的值。例如，ISBN 号的格式如下:

**/ISBN-\d-\d{3}-\d{5}-\d/**

现在我可以将书籍添加到模式中，将书籍添加到作者中，并为每本书生成 ISBN 和书名:

```
// book.js
export default `
  type Book {
    ISBN: String
    title: String
}
```

mocks.js:

```
import casual from 'casual';
import RandExp from 'randexp';
import {
    MockList
} from 'graphql-tools';
import {
    startCase
} from 'lodash';

export default {
    Int: () => casual.integer(0),

Author: () => ({
        firstName: casual.first_name,
        posts: () => new MockList([1, 7]),
        books: () => new MockList([0, 5])
    }),

Post: () => ({
        title: casual.title
    }),

Book: () => ({
        ISBN: new RandExp(/ISBN-\d-\d{3}-\d{5}-\d/)
            .gen(),
        title: startCase(casual.title)
    })
}
```

这里有一个新的问题:

```
query PostsForAuthor {
  author(id: 1) {
    firstName
    posts {
      title
      votes
    }
    books {
      title
      ISBN
    }
  }
}
```

样本响应:

```
{
  "data": {
    "author": {
      "firstName": "Rosemarie",
      "posts": [
        {
          "title": "Et ipsum quo",
          "votes": 248
        },
        {
          "title": "Deleniti nihil",
          "votes": 789
        },
        {
          "title": "Aut aut reprehenderit",
          "votes": 220
        },
        {
          "title": "Nesciunt debitis mollitia",
          "votes": 181
        }
      ],
      "books": [
        {
          "title": "Consequatur Veniam Voluptas",
          "ISBN": "ISBN-0-843-74186-9"
        },
        {
          "title": "Totam Et Iusto",
          "ISBN": "ISBN-6-532-70557-3"
        },
        {
          "title": "Voluptatem Est Sunt",
          "ISBN": "ISBN-2-323-13918-2"
        }
      ]
    }
  }
}
```

这就是使用 graphql-tools 和一些其他有用模块进行模拟的基础。

注意:我在这篇文章中使用了一些片段。如果你想在一个更广阔的背景下理解，这里的示例代码是。

完整的源代码在 GitHub 上供你阅读。

如果你觉得这篇文章内容丰富，请帮我一把。