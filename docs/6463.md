# React + Apollo:如何在重新提取查询后重定向

> 原文：<https://www.freecodecamp.org/news/react-apollo-how-to-redirect-after-refetching-a-query-a1e853e062e9/>

作者 Jun Hyuk Kim

# React + Apollo:如何在重新提取查询后重定向

![1nGG9CsCAEXi4Q5uwCPOPOMzF-pcOUv9ifpp](img/c6ce0d6e9bbfdf57cbfe847c66fc87f2.png)

GraphQL 炙手可热，这是有原因的。简而言之，它是一种查询语言，允许你从你的 API 中要求*确切的*你需要的东西。这减少了使用不同方法时可能出现的任何不必要的数据传输。

我在一个项目中使用了 GraphQL 后端。我决定使用 React 和 Apollo 客户端作为我的前端，与我的 GraphQL 后端进行通信。我很难弄清楚如何重新提取我的查询，然后用更新后的数据将我的页面重定向到主页面。这就是事情开始变得有点棘手的地方。

对我来说，问题是弄清楚突变实际上是如何被调用的，以及返回了什么。我们可以通过`graphql(mutation)(*YourComponent*)`到`this.props.mutate()`连接突变后访问它。这个函数返回一个承诺。我们可以链接`.then()`函数来调用变异后的函数。mutate 函数也可以接受变量进行变异。完整的示例如下所示:

```
this.props.mutate({  variables:{    title: this.state.title,    content: this.state.content  }})
```

这意味着我们的突变接受两个变量，标题和内容。当我们将它发送到后端服务器时，它们被传递到我们的突变中。假设我们的变化是添加一个注释，带有标题和内容。为了让事情更清楚，我将包括一个简单的例子来说明我们的突变是什么样子的:

```
const mutation = gql`  mutation AddNote($title: String, $content: String){    addNote(title:$title, content:$content){      title      content    }  }}`
```

```
// Our component should also be connected to Apollo client, so // something like this
```

```
export default graphql(mutation)(Component)
```

那么，这个函数发生后会发生什么呢？我们的后端接收到信息，变异就发生了。但是，我们的前端并不知道发生了突变。它不会重新获取我们之前获取的查询(在我们的例子中，可能是类似 fetchAllNotes 的东西)。这就是 mutate 函数非常方便的地方。我们可以传入一个名为`refetchQueries`的变量，它将重新获取我们要求的任何查询。

```
this.props.mutate({  variables:{    title: this.state.title,    content: this.state.content  },  refetchQueries:[{    query: fetchAllNotes  }]}).then(() => this.props.history.push('/notes'))
```

在这种情况下，我们告诉 Apollo 客户机在突变发生后重新提取`fetchAllNotesquery`。然后将用户重定向到 `/notes`目录(React-Router)。还记得我们的 mutate 函数返回一个承诺吗？这一切都应该工作，对不对？嗯……按照设计，阿波罗团队让`refetchQueries`在**和**语句同时发生。这意味着。那么语句可以出现在`refetchQueries`之前。这可能导致需要更新信息的组件没有被更新。

在这个特定的例子中，我们的用户将在发生之前*被重定向。该信息将不会被更新。这很棘手，因为 mutate 函数返回一个承诺。阿波罗团队通过*设计*使`refetchQueries` 可以和任何`.then`语句一起发生。那么，我们该如何处理呢？*

阿波罗团队意识到这可能是一个潜在的问题。他们提出了一个[解决方案](https://github.com/apollographql/apollo-client/pull/3169)，它允许`refetchQueries` 接受一个变量，这个变量允许它返回一个承诺，因此发生在任何`.then`语句之前。我们的代码应该是这样的:

```
this.props.mutate({  variables:{    title: this.state.title,    content: this.state.content  },  refetchQueries:[{    query: fetchAllNotes,    variables:{      awaitRefetchQueries: true    }  }]}).then(() => this.props.history.push('/notes'))
```

如果这对你有用，哇哦！看起来修复成功了！然而，这对我个人来说并不奏效。此外，因为它仅在较新版本的 Apollo 客户端上可用，所以在较旧版本的 Apollo 客户端上不可用。

我必须用 React 组件生命周期解决一些问题，以确保我的组件能够正确地呈现更新的数据。修复本身非常简单明了！在我的 Notes 组件上，它呈现注释并通过`graphql` 函数连接到`fetchAllNotes` 查询，我添加了一个快速修复来确保我的数据被正确呈现。

```
componentDidUpdate(prevProps){  if(prevProps.data.notes && prevProps.data.notes.length !==     this.props.data.notes.length){    // Logic to update component with new data  }}
```

基本上，我们说当组件更新时，我们想看看 notes 查询是否在之前完成(检查`prevProps.data.notes`是否存在)以及数据的长度是否改变。这允许我们的 React 组件在 refetch 查询完成后更新信息。

现在一切都正常了！希望`awaitRefetchQueries` 变量对您有用，并变得更为人所知，这是一个更好的解决方案。然而，很难找到如何正确使用`awaitRefetchQueries` 的例子/文档。目前，对 React 组件生命周期的良好理解足以帮助您绕过 Apollo + React 的“陷阱”!

请随时在评论中留下任何反馈或问题，我会尽我所能提供帮助。我绝不是专家，但我很乐意与你一起解决问题，并帮助解决它！