# Node.js 服务器端 JavaScript–Node 是做什么用的？

> 原文：<https://www.freecodecamp.org/news/node-js-server-side-javascript-what-is-node-used-for/>

Ryan Dahl 在 2009 年发布的 Node.js 扩大了开发人员使用 JavaScript 的范围。在此之前，您只能在客户端(浏览器)或 web 应用程序的前端使用 JavaScript。

使用 Node.js，开发人员可以创建服务器端应用程序、命令行工具等等。

本文不是关于如何使用 Node.js 的速成课程(在本文的最后一节可以找到相关的参考资料)。相反，它是对 Node.js 是什么、它的特性以及它的用途的介绍。

## Node.js 是什么？

Node.js 是一个开源的 JavaScript 运行时环境，允许开发人员在服务器上运行 JavaScript 代码。

如果这对你来说太复杂而无法理解，那么你应该这样想:Node.js 是运行在浏览器之外的 JavaScript 在服务器上。

注意 Node.js 不是一种编程语言——它是一种工具。

## Node.js 有什么特别之处？

在这一节中，我们将讨论一些使 Node.js 使用起来很酷的特性。

目的不是将 Node.js 与其他后端技术进行比较，而是帮助您理解它的一些功能。

### 单线程和异步

Node.js 执行任务(接收请求和发回响应)很快，因为它具有单线程和异步的特性。

让我们解释一下上面的一些术语。

对于单线程，这意味着 Node.js 有一个单一的资源来处理请求。多线程后端技术为每个新请求分配一个新线程。

您可以将线程视为向多人提供服务的人。一个非常受欢迎的真实例子是餐馆。我们将结合 Node.js 的异步部分进一步解释这个例子。

Node.js 是异步的，因为它可以同时处理多个请求。让我们回到餐馆的例子。

一位顾客来到一家餐馆，坐下来等待服务员。服务员来到顾客的桌子前，接受他们的订单。然后，订单被送到厨房。

但是服务员不会等到订单准备好了才继续下一个顾客。当准备好的时候，他们会带着顾客点的菜回来——同时，服务员继续下一个顾客，重复同样的过程。

上面的例子类似于 Node.js 的工作原理。它能够使用单线程异步处理多个请求(无需等待一个请求完成后再转移到下一个请求)。

因此，当请求的响应准备就绪时，它会被发送回客户端。

Node.js 的单线程和异步特性使其非常快速，非常适合构建数据密集型实时应用程序。

### JavaScript 无处不在

作为 web 开发人员使用 Node.js 的另一个优势是可以在 web 应用的前端和后端使用 JavaScript。

在 Node.js 发布之前，web 开发人员必须学习一种不同的编程语言来构建他们的 web 应用程序的后端。

当然，一些开发人员仍然使用不同的语言作为他们的后端，但是 Node.js 使得只使用一种语言——JavaScript——变得很容易。

### 快速执行时间

Node.js 建立在 Google 的 V8 JavaScript 引擎之上，该引擎具有非常高的性能。这使得节点可以快速执行请求。

### 跨平台兼容性

Node.js 支持许多主要平台。所以你可以写你的代码，它可以在 Windows，MacOS，LINUX，UNIX 甚至一些移动设备上运行。

## 节点是做什么用的？

下面是一些你可以用 Node.js 做的很酷的事情:

*   创建 HTTP web 服务器。
*   动态生成网页。
*   收集表单数据并将其发送到数据库。
*   创建、读取、更新和删除存储在数据库中的数据。
*   创建 API。
*   构建命令行工具。
*   读取、写入、移动、删除和打开/关闭服务器上的文件。

## 摘要

在本文中，我们讨论了 Node.js。我们首先看看它到底是什么。

然后我们讨论了使 Node.js 脱颖而出的一些特性。

最后，我们看到了如何使用 Node.js 的列表。

### 如何学习 Node.js

现在，您已经对 Node.js 是什么、它的特性以及它的用途有了一个简单的介绍，下面是一些您可以用来学习如何使用 Node.js 的资源:

*   [freeCodeCamp 的后端开发和 API 认证](https://www.freecodecamp.org/learn/back-end-development-and-apis/)。您将学习如何使用 Node.js 和 npm 编写后端应用程序。您还将使用 Express 框架以及 MongoDB 和 Mongoose 库构建 web 应用程序。
*   freeCodeCamp.org YouTube 频道的 8 小时课程将教你 Node.js 和 Express。
*   [freeCodeCamp.org YouTube 频道](https://www.youtube.com/watch?v=qwfE7fSVaZM&list=RDCMUC8butISFwT-Wl7EV0hUK0BQ&index=2)的 10 小时专题课程。你将根据从上述 8 小时课程中获得的知识构建四个项目。

编码快乐！