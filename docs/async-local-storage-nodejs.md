# Node.js v14 中的异步本地存储是什么？

> 原文：<https://www.freecodecamp.org/news/async-local-storage-nodejs/>

Node.js 14 现已发布，该版本引入了异步本地存储支持。现在你可能会想，“嗯，好吧。本地存储已经存在了一段时间，“但这一次，情况有所不同。

首先，我们讨论的是节点运行时，而不是浏览器。因此，拥有“本地存储”这种类似浏览器的概念对 Node 来说并没有什么意义。事实是，它也不是您可能想到的本地存储。那是什么？

## 引入异步本地存储–用于异步任务的存储

考虑一个像 Apache 这样运行 PHP 的 web 服务器来托管一个网站。当 PHP 接收到来自客户端的请求时，web 服务器会确保释放一个新线程。它让那个线程为那个特定的请求管理所有的资源、局部变量、函数调用栈等等。简单易行。但是 JavaScript 出现了一个问题。

JavaScript 是单线程的——这意味着你不能让多个 JS 线程在同一个父进程下一起运行。但是不要让这欺骗了你——在处理 web 服务器请求方面，JS 和 Java 后端这样的成熟解决方案一样快(甚至更快)。

这是怎么发生的？嗯，这是另一篇文章的内容。但是这里重要的是**节点是单线程的**，所以你没有**线程本地存储**的好处。线程本地存储只不过是特定线程本地的变量和函数——在我们的例子中，用于处理网页上的特定用户。

## 为什么单线程是个问题？

在这种情况下，单线程是一个问题，因为只要节点没有耗尽事件循环中的所有同步操作，它就会一口气执行同步代码。然后，它将检查事件和回调，并在必要时执行代码。

在 Node 中，一个简单的 HTTP 请求只不过是一个由`http`库向节点发出的处理请求的事件——因此它是异步的。

现在，假设您想要将一些数据与这个异步操作相关联。你会怎么做？

嗯，你可以创建某种“全局”变量，并将你的特殊数据赋给它。然后，当另一个请求来自同一个用户时，您可以使用全局变量来读取之前存储的内容。

但是，当您手头有多个请求时，它会非常失败，因为 Node 不会串行执行异步代码(当然，这是异步的定义！).

让我们考虑一下这个伪代码(假设节点运行时):

```
server.listen(1337).on('request', (req) => {
  // some synchronous operation (save state)
  // some asynchronous operation
  // some asynchronous operation
})
```

考虑这一系列事件:

1.  用户 1 在端口 1337 上访问服务器
2.  节点开始运行同步操作代码
3.  当节点运行同步代码时，另一个用户 2 访问了服务器。
4.  节点将继续执行同步代码，同时处理第二个 HTTP 请求的请求在任务队列中等待。
5.  当 Node 完成同步操作并开始异步操作时，它将它放在任务队列中，然后开始处理任务队列中的第一个任务——第二个 HTTP 请求。
6.  这一次它运行的是同步代码，但是代表用户 2 的请求。当用户 2 的同步代码完成后，它将恢复用户 1 的异步执行，依此类推。

现在，如果您希望在调用特定于特定用户的异步代码时将特定数据持久化，该怎么办呢？这里是您使用**async storage——节点中异步流的存储。**

考虑这个直接来自官方文件的例子:

```
const http = require('http');
const { AsyncLocalStorage } = require('async_hooks');

const asyncLocalStorage = new AsyncLocalStorage();

function logWithId(msg) {
  const id = asyncLocalStorage.getStore();
  console.log(`${id !== undefined ? id : '-'}:`, msg);
}

let idSeq = 0;
http.createServer((req, res) => {
  asyncLocalStorage.run(idSeq++, () => {
    logWithId('start');
    // Imagine any chain of async operations here
    setImmediate(() => {
      logWithId('finish');
      res.end();
    });
  });
}).listen(8080);

http.get('http://localhost:8080');
http.get('http://localhost:8080');
// Prints:
//   0: start
//   1: start
//   0: finish
//   1: finish
```

这个例子只显示了`idSeq`对相应请求的“粘性”。您可以想象 express 每次是如何用正确的用户填充`req.session`对象的。以类似的方式，这是一个低级的 API，在节点中使用了一个低级的构造，叫做`async_hooks`，它仍然是实验性的，但是知道它很酷！

## 表演

在你试着在生产中推广它之前，请注意——如果不是绝对需要，我不会建议任何人这么做。这是因为它会对您的应用程序产生不可忽视的性能影响。这主要是因为`async_hooks`的底层 API 仍然是 WIP，但是情况应该会逐渐改善。

## 结论

基本就是这样！非常简单地介绍一下 Node 14 中的 AsyncStorage 是什么，以及它的总体思路是什么。如果您想了解 Node.js 的所有新特性，请观看此视频:

[https://www.youtube.com/embed/osFz798wIaQ?feature=oembed](https://www.youtube.com/embed/osFz798wIaQ?feature=oembed)

此外，如果你是早期采用者，尝试一下[code damn](https://codedamn.com)——一个开发者学习和联系的平台。我已经推出了一些可爱的功能供您尝试！敬请关注。

和平！