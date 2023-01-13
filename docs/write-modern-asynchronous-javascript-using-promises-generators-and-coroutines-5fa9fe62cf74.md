# 使用承诺、生成器和协同程序编写现代异步 Javascript

> 原文：<https://www.freecodecamp.org/news/write-modern-asynchronous-javascript-using-promises-generators-and-coroutines-5fa9fe62cf74/>

威廉·戈特沙克

# 使用承诺、生成器和协同程序编写现代异步 Javascript

![1*SYxxUFJirsj3BH1-LCsFZw](img/cc89e66eb04925db39c470108c9cc8cf.png)

How you might feel after learning about Promises, Generators, and Coroutines

多年来，“回调地狱”经常被认为是 Javascript 中管理并发性的最讨厌的设计模式之一。如果您已经忘记了它的样子，这里有一个在 Express 中变化和处理事务的例子:

#### 承诺应该拯救我们…

有人告诉我，通过将我们的异步函数包装在一个特殊的对象中，promises 将允许我们 Javascript 开发人员编写异步代码，就好像它是同步的一样。为了获得承诺的价值，我们称之为 ***。然后是*T3 或者**T5。把** 抓在许诺对象身上。但是当我们试图用承诺来重构上面的例子时会发生什么呢？**

因为回调中的每个函数都是有作用域的，所以我们不能访问第二个 ***中的用户对象。然后*** 回调。

所以经过一番挖掘，我找不到一个优雅的解决方案，但我找到了一个令人沮丧的方案:

> 只是缩进你的承诺，使他们有适当的范围。

取消我的承诺！？现在又回到末日金字塔了？

我认为嵌套的回调版本比嵌套的 promise 版本看起来更简洁，也更容易推理。

#### 异步等待将拯救我们！

***async*** 和 ***await*** 关键字将允许我们编写 javascript 代码，就像它是同步的一样。下面是用 ES7 中的这些关键字编写的代码:

不幸的是，包括 ***异步*** */ **等待*** 在内的大多数 ES7 特性还没有在本地实现，因此需要使用 transpiler。但是，您可以使用大多数现代浏览器以及 Node 版本 4+中已经实现的 ES6 特性编写看起来与上面的代码完全一样的代码。

#### 动态二人组:生成器和协程

生成器是一个很好的元编程工具。它们可以用于惰性评估、迭代内存密集型数据集以及使用 RxJs 等库对来自多个数据源的数据进行按需处理。

然而，我们不希望在生产代码中单独使用生成器，因为它们迫使我们随着时间的推移对一个过程进行推理。每次调用 next，我们都像 GOTO 语句一样跳回生成器。

协程理解这一点，并通过包装一个生成器和抽象掉所有的复杂性来补救这种情况。

#### 使用协程的 ES6 版本

协程允许我们一次一行地让 ***产生*** 我们的异步函数，使我们的代码看起来同步。

需要注意的是，我使用的是 Co 库。Co 的协程将立即执行生成器，而 Bluebird 的协程将返回一个函数，您必须调用该函数来运行生成器。

让我们建立一些使用协程的基本规则:

1.  任何一个 ***右边的函数产生*** 必须返回一个承诺。
2.  如果你想现在执行你的代码，使用 ***co*** 。
3.  如果你想稍后执行你的代码，使用 ***co.wrap*** 。
4.  一定要连锁一个 ***。在你的协程结束时捕捉*和**来处理错误。否则，您应该将代码包装在 try/catch 块中。
5.  蓝鸟的***promise . coroutine***相当于 Co 的 ***co.wrap*** 而不是其本身的 ***co*** 功能。

#### 如果我想同时运行多个进程，该怎么办？

您可以使用带有 yield 关键字的对象或数组，然后析构结果。

#### 您现在可以使用的库:

[**Promise.coroutine |蓝鸟**](http://bluebirdjs.com/docs/api/promise.coroutine.html)
[*蓝鸟是一个功能齐全的 JavaScript promises 库，性能无与伦比。*bluebirdjs.com](http://bluebirdjs.com/docs/api/promise.coroutine.html)[**co**](https://www.npmjs.com/package/co)
[*生成器异步控制流善*www.npmjs.com](https://www.npmjs.com/package/co)[**Babel*用于编写下一代 JavaScript 的编译器***](https://babeljs.io/)
[用于编写下一代 JavaScript 的编译器 Babel js . io](https://babeljs.io/)[**asynca wait**](https://www.npmjs.com/package/asyncawait)
[*异步*](https://www.npmjs.com/package/asyncawait)