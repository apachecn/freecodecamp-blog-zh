# 如何优雅地处理 Node.js API 客户端中的故障

> 原文：<https://www.freecodecamp.org/news/how-to-gracefully-handle-failures-in-a-node-js-api-client-605673cb72ab/>

罗杰·金

# 如何优雅地处理 Node.js API 客户端中的故障

![ZeWVg5Ffd0NyPPabxotdKvUV2q6pVs2FLX1A](img/cfcee0c9f9cd23bed093c4f8d4380cd3.png)

生活中有两个事实:你呼吸空气，你的程序会出错。

我们都经历过连接 Wi-Fi 出现问题，或者电话突然掉线。从统计数据来看，互联网间歇性故障总体上并不常见，但仍会发生。这意味着对于程序员来说，任何在网络上等待响应的事情都容易出错。

我是一个名为 [ButterCMS](https://buttercms.com/) 的“内容管理系统即服务”的架构师，所以我将使用这个工具来探索可靠性。它包括:

*   内容编辑者的托管仪表板
*   用于检索这些内容的 JSON API
*   用于将 ButterCMS 集成到本机代码中的 SDK。

数据库、逻辑和管理仪表板是通过 web API 提供的服务。问题是“对于 Node.js 客户机中不可避免的错误，您能做些什么？”客户端 API 上的错误是必然会发生的——最重要的是你做了什么。

在本文中，我将介绍我如何为 ButterCMS JavaScript [API 客户端](https://github.com/buttercms/buttercms-js)添加更好的故障处理。到本文结束时，您将有望更好地理解如何在自己的 API 客户机中处理失败。

### 基本异常处理

首先，让我们看一个从 Butter API 检索博客文章的示例 API 请求:

```
butter.post.retrieve('example-post')  .then(function onSuccess(resp) {    console.log(resp.data);  });
```

这是可行的，除了它让你对客户可能抛出的任何异常视而不见。注意客户端 API 使用[承诺](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)来获取博客数据。记住这一点，因为 JavaScript 通过承诺呈现出一个新的维度。

要使用承诺来处理异常，请在末尾加上一个`catch()`。

例如:

```
butter.post.retrieve('example-post')  .catch(function onError(error) {    console.log(error);  });
```

搞定了。JavaScript promise 为您处理所有错误并执行`onError()`回调。`error`对象包含了关于出错的非常有用的信息。

如果你仔细观察 ButterCMS 客户端 API，你会发现它使用了 [axios](https://github.com/mzabriskie/axios) 。Axios 是一个基于 promise 的 HTTP 客户端，可以在浏览器和 Node.js 中运行。

检查通过 promise 获得的 Axios 错误对象会发现以下错误对象:

```
{data:Object, status:401, statusText:'Unauthorized', headers:Object, config:Object}
```

HTTP 状态代码告诉我错误是什么。

### 更好的异常处理

您得到的错误类型将取决于 API 行为。例如，ButterCMS API 文档列出了所有可能的响应。根据要求，您可以获得 400、401 或 404。

处理这些异常的一种方法是以不同的方式处理每种状态。例如，您可以处理错误:

```
butter.post.retrieve('example-post')  .catch(function onError(error) {    if (error.status === 400) {      console.log('Bad request, often due to missing a required parameter.');    } else if (error.status === 401) {      console.log('No valid API key provided.');    } else if (error.status === 404) {      console.log('The requested resource doesn\'t exist.');    }  });
```

通过使用 HTTP 状态作为事实的来源，您可以随意解释错误的原因。

其他公司，如 [Stripe API 客户端](https://github.com/stripe/stripe-node)，用响应上的[错误类型](https://github.com/stripe/stripe-node/blob/master/lib/Error.js)来解决问题。错误代码`typestatus`告诉您响应中返回的是什么类型的错误。

尽管如此，还有最后一个问题。"当网络请求超时时会发生什么？"

对于客户端 API 来说，任何通过网络的请求都是非常危险的。网络连接有时是一种奢侈，人们负担不起。

让我们来看看当超时时你得到了什么样的错误异常。ButterCMS 客户端 API 的默认值为 3000 毫秒或 3 秒。

当异常处理程序超时时，看看这个错误对象:

```
{code:'ECONNABORTED', message:String, stack:String, timeout:3000}
```

像任何好的错误对象一样，它有大量关于异常的详细信息。注意，这个错误对象与我们之前看到的不同。一个明显的区别是`timeout` 属性。这有助于以独特的方式处理这种异常。

问题是，“有没有一种优雅的方式来处理这类异常？”

### 处理网络错误

一种想法是在请求失败后自动重试。任何等待网络响应的操作都可能失败。失败是由你无法直接控制的情况造成的。作为开发人员，掌控一切是件好事，但生活中也有许多例外。

一旦检测到错误，Polly-js 可以尝试重试动作。polly-js 库可以通过 JavaScript promise 处理异常。这个承诺在所有重试失败的情况下捕捉异常，并执行`catch()`。但是，我们决定不使用 polly-js，因为它是一个额外的依赖项，增加了客户端 API 的膨胀。

这里的一个设计原则是:“一点点复制粘贴比一个额外的依赖要好。”重试逻辑的大部分是最小的，并且正好有我们解决问题所需要的。

自动重试的关键是返回一个 JavaScript 承诺:

```
function executeForPromiseWithDelay(config, cb) {  return new Promise(function (resolve, reject) {    function execute() {      var original = cb();
```

```
original.then(function (e) {        resolve(e);      }, function (e) {        var delay = config.delays.shift();
```

```
if (delay && config.handleFn(e)) {          setTimeout(execute, delay);        } else {          reject(e);        }      });    }
```

```
execute();  });}
```

promise 封装了用于自动重试的`resolve`和`reject`回调。`config.handleFn()`回调计算出什么条件会导致它重试。`config.delays.shift()`将从列表中删除第一个项目，并延迟下一次尝试。

好消息是它可以在重试之前满足特定的条件。该库有一个`handle()`函数来设置评估条件的回调。你告诉它重试的次数，给出条件，以及最终的异常处理。

ButterCMS 客户端 API 具有开箱即用的`retry`功能。要启用自动重试，您需要:

```
butter.post.retrieve('example-post')  .handle(function onError(error) {    // Only retry on time out    return error.timeout;  })  .executeWithAutoRetry(3)  .then(function onSuccess(resp) {    console.log(resp.data);  })  .catch(function onTimeoutError(error) {    if (error.timeout) {      console.log('The network request has timed out.');    }  });
```

如果出现故障，`executeWithAutoRetry()`将交错后续请求并重试。例如，第一次尝试将失败，然后在第二次尝试之前等待 100 毫秒。如果第二次尝试失败，将在第三次尝试之前等待 200 毫秒。第三次尝试将在第四次也是最后一次尝试之前等待 400 毫秒。

使用 ButterCMS API 客户端，您现在有了一种处理基于承诺的异常的好方法。你所需要做的就是根据你的喜好进行配置。

### 结论

当谈到错误时，你要么把头埋在沙子里，要么优雅地处理意外。任何通过连接等待响应的客户端 API 都容易出现异常。当反常行为发生时，你可以选择做什么。

将异常视为不可预测的行为。除外，因为不可预测不代表不能提前准备。在处理异常时，重点是预测哪里出错了，而不是应用程序逻辑。

网络连接是故障的罪魁祸首之一。请务必提前做好准备，以便在连接失败的情况下对请求进行第二次更改。

这篇文章最初发表在我们的[博客](https://buttercms.com/blog/how-to-gracefully-handle-failures-in-a-nodejs-api-client) *上。*

更多类似的内容，请关注 ButterCMS 的 Twitter。