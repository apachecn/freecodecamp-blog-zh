# 关于如何使用 Fetch API 执行 HTTP 请求的实用 ES6 指南

> 原文：<https://www.freecodecamp.org/news/a-practical-es6-guide-on-how-to-perform-http-requests-using-the-fetch-api-594c3d91a547/>

在本指南中，我将向您展示如何使用 Fetch API (ES6+)对一个 [REST API](https://jsonplaceholder.typicode.com/) 执行 HTTP 请求，并给出一些您最有可能遇到的实际例子。

想快速查看 HTTP 示例吗？转到第 5 节。第一部分描述了处理 HTTP 请求时 JavaScript 的异步部分。

> **注**:所有例子都是用 ES6 写的，带有[箭头功能](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)。

当今 web/mobile 应用程序中的一个常见模式是从服务器请求或显示某种数据(如用户、帖子、评论、订阅、支付等)，然后通过使用 CRUD(创建、读取、更新或删除)操作来操纵它。

为了进一步操作一个资源，我们经常使用[这些 JS 方法](https://medium.freecodecamp.org/7-javascript-methods-that-will-boost-your-skills-in-less-than-8-minutes-4cc4c3dca03f)(推荐)，比如`.map()`、`.filter()`和`.reduce()`。

> 如果你想成为一名更好的 web 开发人员，开创自己的事业，教导他人，或者提高你的开发技能，我将每周发布关于最新 web 开发语言的提示和技巧。

### 这是我们要解决的问题

1.  处理 JS 的异步 HTTP 请求
2.  什么是 AJAX？
3.  为什么提取 API？
4.  获取 API 的快速介绍
5.  获取 API - CRUD 示例←好东西！

### 1.处理 JS 的异步 HTTP 请求

理解 JavaScript (JS)如何工作的最具挑战性的部分之一是理解如何处理异步请求，这需要理解承诺和回调如何工作。

在大多数编程语言中，我们习惯于认为操作是按顺序发生的。必须先执行第一行，然后才能继续下一行。这很有意义，因为这就是我们人类运作和完成日常任务的方式。

但是使用 JS，我们有多个操作在后台/前台运行，我们不能有一个每次等待用户事件就冻结的 web 应用程序。

> 将 JavaScript 描述为异步可能会产生误导。更准确的说，JavaScript 是同步单线程的，有各种回调机制。[阅读更多](https://stackoverflow.com/questions/2035645/when-is-javascript-synchronous)。

尽管如此，有时事情必须按顺序发生，否则会造成混乱和意想不到的结果。出于这个原因，我们可以使用承诺和回调来构建它。例如，在继续下一个操作之前验证用户凭证。

### 2.什么是 AJAX

AJAX 代表异步 JavaScript 和 XML，它允许在应用程序运行时通过与 web 服务器交换数据来异步更新网页。简而言之，它本质上意味着您可以更新 web 页面的一部分，而无需重新加载整个页面(URL 保持不变)。

> AJAX 是一个容易让人误解的名字。AJAX 应用程序可能使用 XML 来传输数据，但是将数据作为纯文本或 JSON 文本来传输也同样常见。
> —w3shools.com

#### 一路 AJAX？

我看到许多开发人员倾向于对在一个页面应用程序(SPA)中拥有一切感到非常兴奋，这导致了许多异步的痛苦！但幸运的是，我们有 Angular、VueJS 和 React 等库，这使得这个过程变得更加简单和实用。

总的来说，在应该重新加载整个页面还是页面的一部分之间取得平衡很重要。

在大多数情况下，就浏览器变得多么强大而言，页面重载工作得很好。过去，页面重新加载需要几秒钟(取决于服务器的位置和浏览器的功能)。但是今天的浏览器速度非常快，所以决定是执行 AJAX 还是页面重载并没有太大的区别。

我个人的经验是，用一个简单的搜索按钮创建一个搜索引擎比不用按钮要容易和快捷得多。在大多数情况下，客户并不关心它是 SPA 还是额外的页面重新加载。当然，不要误会我的意思，我确实喜欢水疗，但我们需要考虑一些权衡，如果我们处理有限的预算和缺乏资源，那么也许快速解决方案是更好的方法。

最后，这真的取决于用例，但我个人认为 SPAs 比简单的页面重新加载需要更多的开发时间和更多的麻烦。

### 3.为什么提取 API？

这允许我们对服务器执行声明性的 HTTP 请求。对于每个请求，它都会创建一个`Promise`，必须对其进行解析才能定义内容类型和访问数据。

现在 Fetch API 的好处是它完全被 JS 生态系统支持，也是 [MDN Mozilla docs](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 的一部分。最后但同样重要的是，它可以在大多数浏览器上开箱即用(除了 IE)。从长远来看，我猜它将成为调用 web APIs 的标准方式。

> 注意！我很清楚其他 HTTP 方法，比如将 Observable 与 RXJS 一起使用，以及它如何关注订阅/取消订阅方面的内存管理/泄漏等等。也许这将成为处理 HTTP 请求的新标准方式，谁知道呢？无论如何，在这篇文章中我只关注 Fetch API，但将来可能会写一篇关于 Observable 和 RXJS 的文章。

### 4.获取 API 的快速介绍

`fetch()`方法返回一个`Promise`，它解析来自`Request`的`Response`以显示状态(成功与否)。如果你曾经在你的控制台日志屏幕上看到这条消息`promise {}`,不要惊慌——它基本上意味着`Promise`工作正常，但正在等待解决。所以为了解决这个问题，我们需要`.then()`处理程序(回调)来访问内容。

简而言之，我们首先定义路径(**获取**)，其次从服务器请求数据(**请求**)，再次定义内容类型(**正文**)，最后但同样重要的是，我们访问数据(**响应**)。

如果你很难理解这个概念，不要担心。通过下面显示的例子，您将得到一个更好的概述。

```
The path we'll be using for our examples 
https://jsonplaceholder.typicode.com/users // returns JSON
```

### 5.获取 API - HTTP 示例

如果我们想要访问数据，我们需要两个`.then()`处理程序(回调)。但是如果我们想要操作资源，我们只需要一个`.then()`处理程序。然而，我们可以使用第二个来确保值已经被发送。

基本提取 API 模板: