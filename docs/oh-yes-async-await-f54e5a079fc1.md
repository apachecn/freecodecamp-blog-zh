# 哦是的！异步/等待

> 原文：<https://www.freecodecamp.org/news/oh-yes-async-await-f54e5a079fc1/>

提哥·洛佩斯·费雷拉

# 哦是的！异步/等待

![1*AiFDaw_q4DMvkKBZI2zaKw](img/96722a0ed3ce34e60a2b10f2a6e9223c.png)

**async** / **await** 是声明异步函数的新 JavaScript 语法。它建立在承诺的基础上，但是更容易使用。

对承诺的全面解释超出了本文的范围。如果您不熟悉 JavaScript 中的 Promises，请参见[使用 promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) 了解更多信息。不一定要成为承诺专家，但是一个好的介绍会帮助你了解**异步** / **等待**。

这里有一个如何写和使用承诺的快速提示。

### 承诺

承诺代表了一种价值，这种价值在现在、将来或者(可能)永远不会实现。

承诺状态可以是下列状态之一:

*   **待定** —承诺既没有解决，也没有拒绝。它代表了承诺的初始状态。
*   **已解决** —被承诺包裹的操作成功完成。
*   **被拒绝** —操作失败。

`getRandomWithPromise()`定义一个用随机数值解决的承诺。`setTimeout()`模拟异步任务(如 HTTP 请求)的延迟。

这里有一个我们如何使用`getRandomWithPromise()`的例子。

### 异步/等待

![1*Dp72fOGQa4WJy7M786c9ew](img/2f9dc99f843257ba12ba04599a4b2003.png)

**async** / **await** 是一个简化异步代码的关键字+操作符对。

*   **async** 声明函数是异步的。
*   **await** 是用来等待承诺实现的运算符。它只能在**异步**函数中使用。

让我们构建一个例子，使用`getRandomWithAsync()`函数和**异步** / **等待**。

首先要注意的是关键字 async 声明函数是异步的

**等待**操作员暂停`getRandomWithPromise()`直到承诺兑现。

当履行诺言时可以是:

**resolved** —表示**等待**返回解析后的值。

**被拒绝**——意味着**等待**将抛出被拒绝的值。

因为承诺可能抛出意外错误，所以将我们的代码包装在一个 **try** / **catch** 块中是很重要的。

注意，`getRandomWithAsync()`的主体读起来像是一个同步函数。这就是**异步** / **等待**的优势之一。这使得代码逻辑容易理解，即使它在做复杂的工作。

不再需要缩进，因为有了[承诺链](https://javascript.info/promise-chaining)。

### 等待

重要的是要记住 **await** 只能在**异步**函数中使用。否则，你会得到一个语法错误。

这就是如何将 **await** 与一个立即调用的函数表达式([life](https://developer.mozilla.org/en-US/docs/Glossary/IIFE))一起使用。

### 班级

我们也可以在类内部创建**异步**函数。

### 多重承诺

如果在继续之前我们有不止一个承诺要履行呢？

我们可以通过两种方式来实现——顺序或并发。

### 连续的

承诺 b 只有在承诺 a 兑现后才执行。所以函数执行时间是承诺 a 和 b 的执行时间之和。

这可能是一个主要的性能问题。好消息是我们可以同时运行两个承诺来节省时间。

### 同时发生的

我们可以通过修改代码来并行运行这两个承诺。如果您请求随机数并保存承诺，它们将同时运行。我们通过在不同的表达式中使用 **await** 来等待两个承诺完成。当它们都完成时，显示结果

函数执行时间等于需要最长时间的承诺。

### 同时(有承诺.全部)

![1*XmtzVJeT4cYJuwAXmZk5Lw](img/ffb1d40b410e3246fa72bb0adc7fbdd3.png)

我们还可以使用`Promise.all`来实现并发。

其中一个优势是`Promise.all`具有快速失效行为。如果一个承诺失败了，那就去承诺吧。所有人都不会等待另一个承诺的实现。它会立即拒绝。

### 等待然后开始

等待 T2 操作符的使用并不局限于承诺。 **await** 会将任何非承诺值转换为承诺值。它通过将值包装到`Promise.resolve`中来实现。

**await** 可以和任何有`.then()`方法的对象一起使用。这个物体也被称为*然后是*。

### 结论

我们现在有了来自 JavaScript 的新的**异步** / **等待**语法来编写异步代码。

**async** 是指定函数是异步的关键字。

**await** 是用来等待承诺实现的运算符。

语法 **async** / **await** 使得异步代码看起来像同步代码。这使得代码更容易阅读和理解。

记住，承诺会产生意想不到的错误。重要的是将代码包装在一个 **try** / **catch** 块中来处理它们。

您可以用两种方式处理多个承诺:顺序或并发。并发具有优势，因为承诺可以并行运行。

最后，操作员**等待**的不仅仅是承诺。我们可以通过一个`.then()`方法将它用于任何对象(即一个 thenable)。

![1*g6-Vw7Ar5l1jNanUX_DGrA](img/022b2881e596caea13e857aeb5291fd5.png)

### 感谢什么？

*   [Brian Terlson](https://twitter.com/bterlson) 因为他的[异步函数命题](https://github.com/tc39/ecmascript-asyncawait)
*   [尼古拉斯·贝瓦夸](https://twitter.com/nzgb)为这个[pony foo——理解 JavaScript 的异步等待](https://ponyfoo.com/articles/understanding-javascript-async-await)
*   [MDN —异步功能](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)
*   致所有[冒险时光](https://www.youtube.com/watch?v=68dkSmglu4Y)粉丝

*一定要看看我在 ES6 上的文章*

[**揭秘 ES6 Iterables &迭代器**](https://medium.freecodecamp.org/demystifying-es6-iterables-iterators-4bdd0b084082)
[*让我们揭秘 JavaScript 与数据结构交互的新方式。*medium.freecodecamp.org](https://medium.freecodecamp.org/demystifying-es6-iterables-iterators-4bdd0b084082)[**让我们来探讨一下 ES6 生成器**](https://medium.freecodecamp.org/lets-explore-es6-generators-5e58ed23b0f1)
[*生成器，又名 iterables 的一种实现。*medium.freecodecamp.org](https://medium.freecodecamp.org/lets-explore-es6-generators-5e58ed23b0f1)