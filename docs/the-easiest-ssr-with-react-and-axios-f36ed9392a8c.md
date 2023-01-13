# 用 React 和 axios 设置服务器端渲染的最简单方法

> 原文：<https://www.freecodecamp.org/news/the-easiest-ssr-with-react-and-axios-f36ed9392a8c/>

西蒙·布索利

# 用 React 和 axios 设置服务器端渲染的最简单方法

![1*xHyQy7jPsssJTQaWcrgc3A](img/deb92259fe3e5193c6ce0d7b912fcd9c.png)

Image Credit: [https://pixabay.com/en/duck-duck-head-mallard-wild-duck-3340537/](https://pixabay.com/en/duck-duck-head-mallard-wild-duck-3340537/)

服务器端渲染(SSR)是一把双刃剑。对于某些需要 SEO 支持和满足某些性能要求的应用程序来说，这非常重要，但要正确实现却很难。

一些主要的困难围绕着用户认证和数据预加载，特别是因为在这些方面没有既定的模式。

在构建 SPA 时，通常会使用 JWT 进行用户认证，通过 HTTP 头发送到服务器。对于数据加载，您可以使用 React 组件挂钩，如`componentWillMount`。但是在服务器上呈现组件树时，这些都不起作用。

#### ？这个想法

你可能听说过不久前 React 引入了对一个叫做[钩子](https://reactjs.org/blog/2019/02/06/react-v16.8.0.html)的新特性的支持。这一点特别有趣，因为每当组件呈现时都会执行钩子，这打开了一个直到现在都不可能的场景。

如果钩子在使用它们的组件渲染时被执行，这意味着它们在客户端和服务器端渲染时都被执行。因此，如果一个钩子发出一个 HTTP 请求，而我们用来做这件事的库在客户机和服务器上都工作，这意味着我们可以免费获得客户机和服务器上的 HTTP 请求！？

axios 就是这样一个库，也是我最喜欢的一个。

#### ⚙️实施

事实证明，这个想法有一个相当简单的实现。

让我们假设您已经在 React 应用程序中实现了服务器端呈现。

> 如果你还没有这样做，有大量的教程和例子。我最喜欢的是在 Redux [文档](https://redux.js.org/recipes/server-rendering)中找到的。

假设我们现在创建了一个名为`useAxios`的钩子。最简单的形式是这样的:

如果你用过钩子，这看起来不会太复杂。

我们使用一个`React.useState`钩子来保存 axios 响应的值，使用一个`React.useEffect` 钩子来触发 axios 请求。

使用这个钩子就像这样简单:

如果你认为这很酷，那就等到你发现——就像我一样——这种方法使得在服务器端渲染期间实现数据加载变得非常容易。

想想看，在服务器上预加载数据会有多复杂？

所涉及的是:

*   触发 HTTP 请求
*   等待响应
*   将数据与生成的标记一起发送给客户端
*   让客户端加载数据，这样它就不会再次执行那些 HTTP 请求，而只是将数据绑定到需要它的组件

现在，尽管这个概念很简单，但它需要一点编码，因此我构建了一个封装了所有这些逻辑的库。它基本上是上面看到的简单实现的扩展，但不是十几行代码，而是大约 100 行。考虑到它所提供的特性以及使用它仍然是一个非常简单的程序，我觉得它非常令人兴奋！

#### ？构建 axios-挂钩

你已经可以检查代码了。这个库名为 [axios-hooks](https://github.com/simoneb/axios-hooks/) ，你可以用:

`npm install axios-hooks`

您将在文档中找到几个例子，并附有`codesandbox.io`演示来帮助您快速入门。用法非常简单，但我更感兴趣的是解释它是如何工作的，特别是因为它可以应用于许多其他钩子。

在客户端使用它已经很有用了，因为它消除了使用 React 生命周期挂钩和组件状态的痛苦。除非您使用更高阶的状态管理库，在这种情况下，您可能会以完全不同的方式解决这个问题。

不过，最大的好处是将它与服务器端渲染相结合。它是这样工作的:

1.  服务器渲染组件树，即通过`react-dom/server`包的`renderToString`函数
2.  `useAxios`钩子被触发，HTTP 请求开始
3.  保存所有请求的列表，并在请求返回时缓存响应
4.  服务器代码等待这些请求完成，并提取它们的可序列化表示，该表示可以与通过呈现组件树生成的标记一起呈现。这被发送回客户端
5.  在合并组件树之前，客户端用服务器返回的数据填充`axios-hooks`缓存
6.  客户端合并组件树，再次触发`useAxios`钩子。因为数据已经在那里了，所以在客户机上没有实际的 HTTP 请求

这个概念非常简单，实现也是如此。

结账？a `[xios-hooks](https://github.com/simoneb/axios-hooks/)` 今天，并张贴您的反馈！

#### 信用

在服务器端渲染场景中利用 React 钩子的最初想法归功于 [NearForm](https://www.nearform.com) 的酷家伙，他们构建了令人敬畏的`[graphql-hooks](https://github.com/nearform/graphql-hooks)`库。