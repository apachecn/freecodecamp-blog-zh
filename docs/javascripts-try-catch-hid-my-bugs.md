# JavaScript 的 try-catch 隐藏了我的 bug！

> 原文：<https://www.freecodecamp.org/news/javascripts-try-catch-hid-my-bugs/>

> (横幅照片由 [Thomas Smith](https://unsplash.com/@thomastasy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/bugs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄)

首先让我澄清一件事——JavaScript 是一门伟大的语言，这无可厚非。完全是我的错——我的错误处理心理模型不完整，这导致了麻烦。因此，这个职位。

但首先，让我给你一些背景。我正在编写一堆涉及第三方 API 的代码(具体来说，就是 [Stripe 的循环计费和订阅 API](https://stripe.com/docs/billing/quickstart))，并编写了一个包装类和一些服务器路由处理程序来响应来自前端 web 应用程序的请求。整个应用程序是 React +TypeScript + Node，带有一个 [Koa 服务器](https://koajs.com/)。

作为其中的一部分，我试图处理以下错误:

1.  条带的 API 引发的错误
2.  我的包装类抛出的错误，尤其是从数据库中获取用户数据时
3.  由上述原因引起的路由处理程序错误。

在开发过程中，我最常见的错误是服务器请求中的数据不完整，以及传递给 Stripe 的数据不正确。

为了帮助您可视化数据流，让我给你一些服务器端代码的背景知识。通常，函数调用链看起来是这样的:

*路由处理器- >条带包装器- >条带 API*

被调用的第一个函数将在路由处理程序中，然后在 Stripe 包装器类中，Stripe API 方法将在该类中被调用。因此，调用堆栈在底部有 Route-Handler(第一个调用的函数)，在顶部有 Stripe API 方法(最后一个调用的函数)。

问题是我不知道把我的错误处理放在哪里。如果我没有在服务器代码中放置一个错误处理程序，那么 node 将会崩溃(字面意思，退出执行！)并且前端将接收到错误 HTTP 响应(通常是 HTTP [5xx err0r](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/500) )。所以我在被调用的各种方法中放了几个`try-catch`处理程序，并在`catch`块中添加了日志记录语句。这样我就可以通过跟踪日志来调试错误。

调用逻辑的一个示例:

```
 function stripeAPI(arg){
    console.log('this is the first function')
    if(!arg) throw new Error('no arg!')
    // else
    saveToDb()
}

function stripeWrapper(){
    console.log('this is the second function, about to call the first function')
    try{
        stripeAPI()
    } catch(err) {
//         console.log(' this error will not bubble up to the first function that triggered the function calls!')
    }
}

function routeHandler(){
    console.log('this is the third  function, about to call the second function')
    stripeWrapper()
}

function callAll(){
    try{
       routeHandler() 
       return 'done'
    } catch (err){
       console.log('error in callAll():', err)
       return ' not done '
    }

}

callAll()
```

I personally use [repl.it](https://www.freecodecamp.org/news/javascripts-try-catch-hid-my-bugs/repl.it) for all my rapid script executions - check it out.

问题是什么？

1.  如果我没有记录这个错误，我*就失去了*这个错误！在上面的代码片段中，请注意，即使我在没有所需参数的情况下调用了`first()`，在`first`的定义中定义的错误也没有被抛出！此外，没有定义`saveToDb()`方法...然而这没有被抓住！如果你运行上面的代码，你会看到它返回‘done’——你不知道你的数据库没有更新，有什么地方出错了！☠️☠️☠️
2.  我的控制台有太多的日志，重复同样的错误。这也意味着在生产中，有过度的伐木...？
3.  代码看起来很难看。几乎和我的控制台一样丑。
4.  其他从事代码工作的人发现这令人困惑，是调试的噩梦。？

这些都不是好结果，都是可以避免的。

## 概念

所以，让我们先了解一些基础知识。我相信你知道他们，但有些人可能不知道，让我们不要离开他们！

一些基本术语:

**错误**——也称为“异常”，是指节点代码出错，程序立即退出。错误，如果不处理，将导致程序嘎然而止，丑陋的消息被喷入控制台，并带有一个长而可怕的错误堆栈跟踪消息。

**抛出***-*`throw`操作符是语言处理错误的方式。通过使用`throw`,您可以使用放在操作符后面的值生成一个异常。注意，`throw`之后的代码不会被执行——从这个意义上说，它就像是一个`return`语句。

**错误**——有一个 JavaScript [对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error)叫做`Error`。“抛出”错误是为了帮助程序员知道需要处理什么。把它想象成一个小小的定时炸弹？在函数调用链中从一个函数抛出到另一个函数。从技术上讲，您可以抛出任何数据，包括作为错误的 JavaScript 原语，但是抛出一个`Error`对象通常是个好主意。

您通常通过传入如下消息字符串来构造`Error`对象:`new Error('This is an error')`。而是简单地创造一个新的`Error`？对象是无用的，因为那只是工作的一半。你必须`throw`抓住它，这样它才能被抓住。这就是它变得有用的原因。

语言通常有一套标准的错误，[，](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Errors)，但是你可以用`new Error('this is my error message')`构造函数创建一个定制的错误消息，你的错误消息应该可以帮助你弄清楚发生了什么。关于[节点错误的更多信息。](https://nodejs.org/api/errors.html)

**接住** *-* 当有人向你扔东西时，你会这么做，对吗？即使有人扔给你一个这样的东西，你可能也会本能地这么做...？！

JavaScript 中的`catch`语句让你处理一个错误？会被扔出去。如果您没有捕捉到错误，那么错误就会“冒泡”(或者冒泡，这取决于您如何查看调用堆栈)，直到它到达第一个被调用的函数，在那里它会使程序崩溃。

在我的例子中，Stripe API 抛出的错误会一直冒泡到我的路由处理函数，除非我在中途的某个地方发现并处理它。如果我不处理错误，Node 将抛出一个`uncaughtException`错误，然后终止程序。

让我们回到我的例子:

**谓栈**

*路由处理器- >条带包装器- >条带 API*

**错误路径**

*条纹 API (* ？*扔在这里)——>API 包装器(*﹏*未被捕获)——>**路由处理器(*﹏仍*未被捕获)——>ccrraashh*？？？

我们希望避免应用崩溃，因为它会导致你的数据损坏，你的状态不一致，你的用户认为你的应用很糟糕。因此，仔细处理错误需要多层次的分析。

JavaScript 中有一些详细的错误处理指南，我最喜欢的一个是这里的，但是我将在这里为你总结我的主要经验。

## Try-Catch 语句

使用这些来优雅地处理错误，但是要小心*在哪里*和*在什么时候*。当错误被捕获并且没有被正确处理时，它们就会丢失。这个“冒泡”过程只发生在错误遇到`catch`语句之前。如果调用链中有一个`catch`语句拦截了错误，那么错误不会使应用程序崩溃，但是不处理错误会隐藏错误！然后它作为参数传递给`catch`，要求你在那里处理它。

```
try{
// code logic
} catch (error) {
// handle the error appropriately
} 
```

因此，当您必须调试错误时，在对您来说最有逻辑意义的时候抓住*和*并处理错误是非常重要的。人们很容易认为必须在它第一次出现时就捕捉它(最后一个被调用的函数正好位于调用堆栈的顶部)，但事实并非如此！

*Route-Handler - >条纹包装纸(不要在这里抓！)- >条纹 API*

如果我把我的`try-catch`放在直接调用 Stripe 的 API 的 Stripe 包装器中，那么我没有关于在哪里调用了我的 Stripe 包装器函数的*的信息。也许是处理程序，也许是我的包装中的另一个方法，也许是在另一个文件中！在这个简单的例子中，它显然是由 Route-Handler 调用的，但是在现实世界的应用程序中，它可以在多个地方被调用。*

相反，对我来说，将`try-catch`放在 Route-Handler 中是有意义的，这是导致错误的函数调用开始的第一个地方。这样，您就可以跟踪调用堆栈(也称为展开调用堆栈)并深入到错误中。如果我向 Stripe 发送坏数据，它将抛出一个错误，这个错误将通过我的代码传递，直到我捕获它。

但是当我发现它时，我需要正确地处理它，否则我可能会无意中隐藏这个错误。处理错误通常意味着决定我是否需要我的前端用户知道发生了错误(例如，他们的支付不起作用)，或者只是内部服务器错误(例如，Stripe 找不到我传递的产品 ID)，我需要优雅地处理这些错误，而不会绊倒我的前端用户和破坏节点代码。如果我向数据库中添加了不正确的内容，那么我现在应该清除这些错误的写入。

当处理错误时，记录它是一个好主意，这样我就可以监控应用程序在生产中的错误和故障，并有效地进行调试。所以最起码，处理应该包括在`catch`语句中记录错误。但是...

```
 function stripeAPI(arg){
    console.log('this is the first function')
    if(!arg) throw new Error('no arg!')
    // else
    saveToDb()
}

function stripeWrapper(){
    console.log('this is the second function, about to call the first function')
    try {
        stripeAPI()
    } catch(err) {
        console.log('Oops!  err will not bubble up to the first function that triggered the function calls!')
    }
}

function routeHandler(){
    console.log('this is the third  function, about to call the second function')
    stripeWrapper()
}

function callAll(){
    try {
       routeHandler() 
       return 'done'
    } catch (err){  
       console.log('error in callAll():', err)
       return ' not done '
    }

}

callAll()
```

...正如你在上面看到的，如果我在中间级别(我的 Stripe Wrapper 类)捕获并记录它，它不会到达`routeHandler`或`callAll`，我的应用程序也不会知道出错了。`callAll`仍然返回`done`，唯一证明出错的地方是日志语句:`'Oops!  err will not bubble up to to first function that triggered the function calls!'`。如果我们没有在那里放置一个日志语句，错误就会消失得无影无踪。

这是“错误隐藏”,它让调试变得很痛苦。如果我添加了一个`try-catch`但是没有在`catch`语句中做任何事情，我会防止我的程序崩溃。但是我也最终“隐藏”了这个问题！这通常会导致不一致的状态——我的部分服务器代码认为一切正常，并告诉我的前端。但是我的服务器代码的另一部分显示出了问题！

在这个简单的例子中，这很容易理解，但是想象一下在你的整个应用程序中嵌套很深的 called 这是一场噩梦！

如果您绝对需要在调用堆栈中间处理错误，那么一定要适当地重新抛出错误。这意味着用另一个`throw error`操作结束您的`catch`语句。这样，错误将再次被抛出，并继续“冒泡”到触发调用链的第一个函数(调用堆栈的底部),在那里它可以再次被正确处理。

下面是它的样子，在`stripeWrapper()`函数中只添加了一个小的重掷。运行代码，看看结果的不同，因为`callAll()`现在通过了错误！

```
function stripeWrapper(){
    console.log('this is the second function, about to call the first function')
    try{
        stripeAPI()
    } catch(err) {
        console.log('Oops!  err will not bubble up to to first function that triggered the function calls!')

        throw err  // add this to re-throw!

    }
}

function callAll(){
    try{
       routeHandler() 
       return 'done'
    } catch (err){  // catches the re-thrown error and prints it to console!
       console.log('error in callAll():', err)
       return ' not done '
    }

}
```

因为你在中间阶段抛出了错误，它到了外部边界，并在那里被捕获。代码返回`not done`，您可以调查为什么错误显示为‘no arg’。您还可以看到它从来没有执行过`saveToDb()`，因为错误是在代码执行之前抛出的！在你把东西保存到数据库*的情况下，这可能是一件好事，假设在那一点*之前没有错误。想象一下，将不该保存的东西保存到数据库中——这是数据库中的脏数据！？？？

所以，不要像我在编程早期所做的那样，简单地在调用栈中的每个步骤的*记录错误，然后重新抛出。这只是意味着当错误通过调用堆栈时，您将获得每个错误的多个日志！只在您可以最有效地处理错误的地方拦截错误，最好是在给定的调用链中拦截一次。*

一般来说，如果您将`try catch`语句放在位于调用堆栈底部的最外层(第一个调用)函数中，会很有帮助。您可以确定这是在抛出`uncaughtException`错误之前*出现错误的地方。这是一个捕捉、记录和处理它的好地方。*

要查看不使用`try-catch`时的处理差异，只需修改`callAll()`如下所示:

```
function callAll(){
    routeHandler()  

    // this won't run!
    console.log('This function is not contained inside a try-catch, so will crash the node program.')
}

callAll() 
```

您会注意到,`console.log`语句从未在这里运行过，因为当`routeHandler()`结束执行时程序崩溃了。

## 经验法则？？？

因此，让我们总结一些快速的规则，它们将涵盖你 90%以上的需求:

1.  不要在你的代码中乱放`try-catch`语句
2.  在一个给定的函数调用链中，尽可能只尝试`catch`一次
3.  尝试将`catch`放在最外面的边界——启动函数调用链的第一个函数(调用堆栈的底部)
4.  不要让你的`catch`语句为空，以此来阻止你的程序崩溃！如果你不处理它，很可能会导致你的前端和后端之间的状态不一致。这可能很危险，会导致糟糕的用户体验？！
5.  不要只在调用堆栈的中间使用`catch`语句，而不是在外部边界。这将导致错误“隐藏”在代码的中间，对你正确地调试或管理数据没有帮助。使用您代码的其他人会找到您的住处并切断您的互联网连接。
6.  在你需要知道的地方抓住它，在那里你可以有意义地做所有必要的事情来清理事情。

*条纹 API (* ？*抛到这里)——>API 包装器(*？*途经)- >* *路线处理员(*？*被抓、被处理、被记录)- >* ？？？

感谢阅读！

如果你想了解更多关于我的代码之旅，请查看[免费代码营播客](http://podcast.freecodecamp.org/)的[第 53 集](http://podcast.freecodecamp.org/53-zubin-pratap-from-lawyer-to-developer)，昆西(免费代码营的创始人)和我分享了我们作为职业改变者的经验，可能对你的旅程有所帮助。你也可以在 [iTunes](https://itunes.apple.com/au/podcast/ep-53-zubin-pratap-from-lawyer-to-developer/id1313660749?i=1000431046274&mt=2) 、 [Stitcher](https://www.stitcher.com/podcast/freecodecamp-podcast/e/59201373?autoplay=true) 和 [Spotify](https://open.spotify.com/episode/4lG0RGpzriG5vXRMgza05C) 上访问播客。

在接下来的几个月里，我还将举办一些 ama 和网络研讨会。如果您对此感兴趣，请点击[此处](http://www.matchfitmastery.com/)告知我。当然，你也可以在 [@ZubinPratap](https://twitter.com/zubinpratap) 给我发微博。