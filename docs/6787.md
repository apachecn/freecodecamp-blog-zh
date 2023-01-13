# React 中的自定义错误页与 GraphQL 和错误边界

> 原文：<https://www.freecodecamp.org/news/how-to-handle-graphql-errors-with-react-error-boundaries-dd9273feda85/>

阿比·诺达

# React 中的自定义错误页与 GraphQL 和错误边界

![1*20J63XycbuDOp4NRTLKeMQ](img/293e2f02a8ef2446b79b4611c86a7785.png)

GitHub’s awesome 500 error page

如果你喜欢这篇文章，请通过查看[](https://pullreminders.com/?utm_source=medium&utm_medium=react-errors)**来支持我，这是一个 Slack 机器人，它会为 GitHub 拉请求向你的团队发送自动提醒。**

**我最近在使用 GraphQL 和 React 时遇到的一个挑战是如何处理错误。作为开发人员，我们以前可能在服务器呈现的应用程序中实现过默认的 500、404 和 403 页面，但是弄清楚如何用 React 和 GraphQL 做到这一点是很棘手的。**

**在这篇文章中，我将谈论我们的团队如何处理这个问题，我们实现的最终解决方案，以及 GraphQL 规范中有趣的教训。**

### **背景**

**我从事的项目是一个相当典型的 CRUD 应用程序，使用 GraphQL、 [Apollo Client](https://www.apollographql.com/docs/react/) 和 [express-graphQL](https://github.com/graphql/express-graphql) 构建在 React 中。我们希望通过向用户显示标准的错误页面来处理某些类型的错误，例如，服务器关闭。**

**我们最初的挑战是找出向客户传达错误的最佳方式。GraphQL 不使用像 500、400 和 403 这样的 HTTP 状态代码。取而代之的是，响应包含一个`errors`数组，其中有一列出错的事情(阅读 [GraphQL 规范](http://facebook.github.io/graphql/June2018/#sec-Response-Format)中关于`errors`的更多内容)。**

**例如，当服务器出现问题时，我们的 GraphQL 响应如下:**

**由于 GraphQL 错误响应返回 HTTP 状态代码 200，识别错误类型的唯一方法是检查`errors`数组。这似乎是一个糟糕的方法，因为 error `message`属性包含了在服务器上抛出的异常。 [GraphQL 规范](http://facebook.github.io/graphql/June2018/#sec-Errors)声明`message`的值是为开发人员准备的，但它并没有指定该值是否应该是人类可读的消息或者是被设计为以编程方式处理的东西:**

> **每个错误都必须包含一个带有关键消息的条目，该条目带有错误的字符串描述，旨在为开发人员理解和纠正错误提供指导。**

### **向 GraphQL 响应添加错误代码**

**为了解决这个问题，我们在错误对象中添加了标准化的错误代码，客户端可以使用这些代码以编程方式识别错误。这是受 Stripe 的 REST API 如何返回字符串[错误代码](https://stripe.com/docs/error-codes)以及人类可读消息的启发。**

**我们决定从三个错误代码开始:`authentication_error`、`resource_not_found`和`server_error`。**

**为了将这些添加到我们的 GraphQL 响应中，我们将自己的`formatError`函数传递给 express-graphql，该函数将服务器上抛出的异常映射到添加到响应中的标准代码。GraphQL 规范通常[不鼓励向错误对象](http://facebook.github.io/graphql/June2018/#example-fce18)添加属性，但是通过将这些条目嵌套在一个`extensions`对象中允许这样做。**

**我们的 GraphQL 响应错误很容易分类:**

**虽然我们开发了自己的方式向由 [express-graphql](https://github.com/graphql/express-graphql) 生成的响应添加代码，但 apollo-server 似乎提供了[类似的内置行为](https://www.apollographql.com/docs/apollo-server/v2/features/errors.html#Codes)。**

### **呈现带有 React 错误边界的错误页面**

**一旦我们找到了处理服务器错误的好方法，我们就把注意力转向了客户端。**

**默认情况下，每当我们遇到`server_error`、`authorization_error`或`authorization_not_found`时，我们希望我们的应用程序显示一个全局错误页面(例如，一个带有消息“糟糕，出错了”的页面)。然而，如果我们愿意，我们也希望能够灵活地处理特定组件中的错误。**

**例如，如果用户在搜索栏中输入一些东西，结果出错了，我们希望在上下文中显示错误消息，而不是闪到错误页面。**

**为了实现这一点，我们首先创建了一个名为`GraphqlErrorHandler`的组件，它将位于`apollo-client`的`Query`和`Mutation`组件及其要呈现的子组件之间。该组件检查响应中的错误代码，如果发现我们关心的代码，就会引发异常:**

**为了使用`GraphqlErrorHandler`，我们包装了阿波罗客户端的`Query`和`Mutation`组件:**

**然后，我们的特性组件使用我们自己的`Query`组件，而不是直接访问`react-apollo`:**

**既然我们的 React 应用程序在服务器返回错误时抛出异常，我们希望处理这些异常并将它们映射到适当的行为。**

**请记住，在前面我们的目标是默认显示全局错误页面(例如，带有消息“糟糕，出错了”的页面)，但是如果我们需要，仍然可以灵活地在任何组件中本地处理错误。**

**React [错误边界](https://reactjs.org/docs/error-boundaries.html)提供了一种奇妙的方式来做到这一点。错误边界是 React 组件，它可以捕捉子组件树中任何位置的 JavaScript 错误，因此您可以用自定义行为处理它们。**

**我们创建了一个名为`GraphqlErrorBoundary`的错误边界，它将捕捉任何与服务器相关的异常并显示适当的错误页面:**

**我们使用错误边界作为所有应用组件的包装器:**

**如果我们想处理组件中的错误而不是呈现错误页面，可以在组件树中更深的位置使用错误边界。**

**例如，如果我们想在前面的组件中自定义错误处理行为，它应该是这样的:**

### **总结**

**GraphQL 仍然相对较新，错误处理似乎是开发人员经常遇到的挑战。通过在 GraphQL 响应中使用标准化的错误代码，我们可以用一种有用而直观的方式向客户传达错误。在我们的 React 应用程序中，错误边界提供了一种很好的方式来标准化我们应用程序的错误处理行为，同时在我们需要时仍然具有灵活性。**