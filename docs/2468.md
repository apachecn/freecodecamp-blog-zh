# 什么是 React 服务器组件？

> 原文：<https://www.freecodecamp.org/news/what-are-react-server-components/>

React 背后的团队认为，通过向开发人员展示已经很受欢迎的库的新功能来结束这一年是一个很好的方式。

12 月 21 日，该团队公布了一个展示这一新功能的[演讲](https://reactjs.org/blog/2020/12/21/data-fetching-with-react-server-components.html)，这个新功能被称为 React 服务器组件(RCS)。在演讲中，[丹·阿布拉莫夫](https://twitter.com/dan_abramov)、[劳伦·谭](https://twitter.com/sugarpirate_)、[约瑟夫·萨沃纳](https://twitter.com/en_JS)和[塞巴斯蒂安·马克伯格](https://twitter.com/sebmarkbage)解释了这一功能背后的原因及其一些使用案例。

请记住，这个特性是一个完整的实验，除了团队发布的 [RFC](https://github.com/reactjs/rfcs/blob/bf51f8755ddb38d92e23ad415fc4e3c02b95b331/text/0000-server-components.md) 之外，它甚至没有公开的文档。

> 我们本着透明的精神分享这项工作，并从 React 社区获得初步反馈。会有足够的时间，所以不要觉得你必须马上赶上来！

这是怎么回事？

让我们从澄清 React 服务器组件背后的一些主要概念开始，这样我们就可以理解这个提议和目前可用的一些类似技术。

## 什么是 React 服务器组件？

React 服务器组件(RSC)类似于**服务器端渲染(SSR)** ，但它们的工作方式略有不同。

基本上，SSR 接受一个 React 组件，并在发出请求时在服务器中呈现它。这将生成一个 HTML **字符串**，该字符串将被发送到浏览器并绘制在屏幕上。然后，如果需要，它会通过水合过程加载相关的 JavaScript。最后，它通过标准的应用程序周期:**客户端渲染**。

React 服务器组件类似于 Next.js 用 **getInitialProps** 做的事情。**服务器组件**可以获取数据并将数据传递给客户端组件，但是这项新技术更加*【动态】*。它允许您从服务器检索完整的组件树，并将其注入到客户端应用程序中，而不会丢失客户端的状态。

SSR 的另一个不同之处是，在这个实现中，JavaScript 代码在服务器中生成并呈现一个 HTML 字符串。这创建了网站的可见部分，一种 HTML 模板。然后，这个模板和交互性所需的 JavaScript 代码一起被发送到服务器。

这允许应用程序具有极快的初始加载和绘制速度。但另一方面，由于水合过程，它的交互式代码可能需要更多的时间。

服务器组件补充了 SSR，创建了一个中间地带的抽象，使呈现过程不需要增加 JavaScript 包的大小或代码。

换句话说，服务器组件不是作为 JavaScript 代码添加到包中的。这大大减少了浏览器需要解析和执行的 JavaScript 的数量——大约减少了 19%到 29%。

> [RFC]:如果我们将上面的例子移植到一个服务器组件上，我们可以为我们的特性使用完全相同的代码，但是避免将它发送到客户机——代码节省超过 240K(未压缩)。

## SSR 会被 React 服务器组件取代吗？

目前有一些元框架可以很好地实现 SSR 技术。而这个领域最知名的就是 [Next.js](https://nextjs.org) 。所以你可能会好奇——next . js 会被服务器组件取代吗？

不会，因为两者都涉及不同的解决方案和实现。RSC 的最初采用可能是通过像 Next 或 Gatsby 这样的元框架。

*   服务器组件永远不会到达客户端。React 使用的 SSR 实现将组件代码交付给客户端，从而增加了浏览器中代码的大小。
*   服务器组件可以在组件树的任何部分访问后端数据。像 Next.js 这样的当前解决方案可以通过使用方法`getServerProps()`以有限的方式访问这些数据(它有自己的限制-它只能在第一级页面中使用，不能从其他组件或一些第三方 npm 包中获取服务器数据，等等)。
*   可以重新获取服务器组件，同时将客户端状态保留在树中。这是可以做到的，因为传输机制不仅仅是 HTML，而是类似于 VDOM 节点定义。

## 想了解更多关于 React 服务器组件的信息吗？

推荐看[原话](https://reactjs.org/blog/2020/12/21/data-fetching-with-react-server-components.html)，看 [RFC](https://github.com/reactjs/rfcs/blob/bf51f8755ddb38d92e23ad415fc4e3c02b95b331/text/0000-server-components.md) ，看提案的 [demo](https://github.com/reactjs/server-components-demo) 。

记住:**你还不需要使用或学习**这个建议。但是关注一下，看看它是怎么演变发展的，也是好的。

![English-Footer-Social-Card](img/d5bb4a3324a2791da53bf108e5e199b4.png)

🐦[在推特上关注我](https://twitter.com/matiasfha) ✉️ J [加入我的时事通讯](https://matiashernandez.ck.page) ❤️ [支持我的工作](https://buymeacoffee.com/matiasfha)