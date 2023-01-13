# 组织 GraphQL 突变

> 原文：<https://www.freecodecamp.org/news/organizing-graphql-mutations-653306699f3d/>

清理污垢。

***更新(2018 年 5 月 7 日):*** Anders Ringqvist(评论)发现[一份问题报告](https://github.com/graphql/graphql-js/issues/221)称**在使用这种方法时会导致问题**。请参见[我的跟进帖子](https://www.freecodecamp.org/news/beware-of-graphql-nested-mutations-9cdb84e062b5/)。

—

GraphQL 模式的巨大差异存在于查询和变异之间。查询方法从数据源读取数据，例如 SQL 数据库或文件系统，甚至是远程服务。尽管查询可以并发执行，但突变却不能。

突变必须顺序执行，因为下一个突变操作可能依赖于前一个突变存储或更新的数据。例如，记录必须在更新之前创建。因此，突变必须按顺序执行。这就是为什么查询和变异在 GraphQL 中有自己的名称空间。

查询是 CRUD 中的“R”(创建、读取、更新和删除)。本文中的代码基于一个 [Launchpad 示例](https://launchpad.graphql.com/1jzxrj179)。在 Launchpad 代码中，定义了一个查询，在给定作者 ID 的情况下，该查询将返回作者的文章。我已经在关于[测试 GraphQL 接口](https://medium.freecodecamp.org/mocking-graphql-with-graphql-tools-42c2dd9d0364)的帖子中扩展过这个例子。在那篇文章中，我添加了书籍，在这里我将扩展这个想法。

### 作者帖子

突变是 CRUD 中的 CUD。上面链接的 Launchpad 例子有一个`**upvotePost**`突变，增加了一篇文章的投票数(一个更新操作)。

```
Mutation: {
    upvotePost: (_, { postId }) => {
      const post = find(posts, { id: postId });
      if (!post) {
        throw new Error(`Couldn't find post with id ${postId}`);
      }
      post.votes += 1;
      return post;
    },
  },
```

为了实现向下投票，我简单地创建了一个类似的`**downvotePost**`变异:

```
Mutation: {
...

  downvotePost: (_, { postId }) => {
      const post = find(posts, { id: postId });
      if (!post) {
        throw new Error(`Couldn't find post with id ${postId}`);
      }
      post.votes -= 1;
      return post;
    },
  },
```

这不完全是一种干的方式。逻辑的主体可以放入一个带有参数的外部函数中，以增加或减少投票。

此外，我想摆脱`upvotePost`和`downvotePost`的命名，而是依靠一个上下文，如`**Post.upvote()**`和`**Post.downvote()**`。这可以通过让突变方法返回一组影响给定 Post 的操作来实现。

`PostOps`是一种定义为:

```
type PostOps {
          upvote(postId: Int!): Post
          downvote(postId: Int!): Post
      }
```

名词`**Post**`已经从该方法的动词名词名称中删除，因为它是多余的。解析器代码通过`PostOps`在 Post 环境中运行:

```
const voteHandler = (postId, updown) => {
    return new Promise((resolve, reject) => {
        const post = posts.find(p => p.id === postId);
        if (!post) {
            reject(`Couldn't find post with id ${postId}`);
        }
        post.votes += updown;
        resolve(post);
    })
};

const PostOps =
    ({
        upvote: ({
            postId
        }) => voteHandler(postId, 1),
        downvote: ({
            postId
        }) => voteHandler(postId, -1)
    });
```

resolver.js

您会注意到我在解析器中使用了一个新的 Promise，尽管从技术上讲，这个例子并不需要它。尽管如此，大多数应用程序异步获取数据，所以…习惯的力量？

现在，不是直接在根级别调用突变方法，而是在`Post`的上下文中调用:

```
mutation upvote {
  Post {
    upvote(postId: 3) {
      votes
    }
  }
}
```

这将返回:

```
{
  "data": {
    "Post": {
      "upvote": {
        "votes": 2
      }
    }
  }
}
```

到目前为止，一切顺利。通过将`**postId**` 参数移到顶层，这些方法可以变得更加枯燥:

```
extend type Mutation {
        Post
(postId: Int!): PostOps
}

type PostOps {
          upvote: Post
          downvote: Post
      }
```

`PostOp`解析器将保持不变:它们仍然接受一个`postId`参数，但是该参数从`Post`传递到`PostOps`。下一个例子将详细解释这是如何工作的。

### 作者和书籍

我的应用程序中的作者不仅发表文章，而且有些人还写了书。我想对作者的图书列表执行传统的创建、更新和删除操作。那么`AuthorOps`是:

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
```

在 GraphQL 中，[突变将自己的输入类型](http://graphql.org/graphql-js/mutations-and-input-types/)作为参数。对于具有自动生成的 id 的实体来说，这通常是必要的。在查询类型中，作者 ID 可能是必需的，但是在 AuthorInput 类型中，它不是也不可能是必需的(ID 是生成的)。

在这种情况下，ISBN 是非生成的图书 ID，因此包含在`CreateBookInput`中。书也有作者。这是从哪里来的？结果是`authorId`从调用创建操作的上下文中被传递给`addBook`解析器，即`AuthorOps`:

```
extend type Mutation {
        Post: PostOps
        Author(id: Int!): AuthorOps
      }
```

`AuthorOps`的解析器看起来像:

```
const addBook = (book, authorId) => {
    console.log("addBook", book, authorId)
    return new Promise((resolve, reject) => {
        book.authorId = authorId
        books.push(book)
        resolve(books.length)
    })
}

const removeBook = (book, authorId) => {
    return new Promise((resolve, reject) => {
        books = books.filter(b => b.ISBN !== book.ISBN && b.authorId === authorId);
        resolve(books.length)
    })
}

const updateBook = (book, authorId) => {
    return new Promise((resolve, reject) => {
        let old = books.find(b => b.ISBN === book.ISBN && b.authorId === authorId);
        if (!old) {
            reject(`Book with ISBN = ${book.ISBN} not found`)
            return
        }
        resolve(Object.assign(old, book))
    })
}

const AuthorOps = (authorId) => ({
    addBook: ({
        input
    }) => addBook(input, authorId),
    removeBook: ({
        input
    }) => removeBook(input, authorId),
    updateBook: ({
        input
    }) => updateBook(input, authorId)
})
```

现在让我们创建一本书并更新它:

```
mutation addAndUpdateBook {
  Author(id: 4) {

addBook(input: {ISBN: "922-12312455", title: "Flimwitz the Magnificent"})
  }
  Author(id: 4) {

updateBook(input: {ISBN: "922-12312455", title: "Flumwitz the Magnificent"}) {
      authorId
      title
    }
  }
}
```

回应是:

```
{
  "data": {
    "Author": {
      "addBook": 4,
      "updateBook": {
        "authorId": 4,
        "title": "Flumwitz the Magnificent"
      }
    }
  }
}
```

#### “书”呢？

您可能会注意到，实际上有一个子上下文在起作用。请注意，我们将突变命名为`**addBook**`、`**updateBook**`、`**removeBook**`。我可以在模式中反映这一点:

```
type AuthorOps {
     Book: BookOps
}

type BookOps {
     add(input: AddBookInput!): Int
     remove(input: RemoveBookInput! ): Boolean
     update(input: UpdateBookInput!): Book
}
```

没有什么可以阻止您添加任意深度的上下文，但是请注意，每次使用这种技术时，返回的结果都会嵌套得更深:

```
>>> RESPONSE >>>
{
  "data": {
    "Author": {
       "Book": {

          "add": 4,
          "update": {
             "authorId": 4,
             "title": "Flumwitz the Magnificent"
          }
        }
     }
  }
}
```

这与 GraphQL 查询返回的结构非常相似，但是对于变异操作来说，深层次结构可能会碍事:您必须“深入挖掘”才能确定您的变异操作是否成功。在某些情况下，更平的回应可能更好。尽管如此，在几个高层次的背景下，突变的浅层组织似乎比没有要好。

这篇文章的工作源代码可以在我的 Github 账号上找到[。](https://github.com/JeffML/graphql-crud2)