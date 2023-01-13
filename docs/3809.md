# 每个开发人员都应该知道的 forEach()和 map()的区别

> 原文：<https://www.freecodecamp.org/news/4-main-differences-between-foreach-and-map/>

JavaScript 有一些方便的方法可以帮助我们遍历数组。迭代最常用的两个是`Array.prototype.map()`和`Array.prototype.forEach()`。

但是我认为他们仍然有点不清楚，特别是对初学者来说。因为它们都进行迭代并输出一些东西。那么，有什么区别呢？

在本文中，我们将了解以下内容:

*   定义
*   返回值
*   链接其他方法的能力
*   易变性
*   性能速度
*   最后的想法

## 定义

`map`方法接收一个函数作为参数。然后将它应用于每个元素，并返回一个填充了调用所提供函数的结果的全新数组。

这意味着它返回一个新的数组，其中包含数组中每个元素的图像。它将总是返回相同数量的项目。

```
 const myAwesomeArray = [5, 4, 3, 2, 1]

myAwesomeArray.map(x => x * x)

// >>>>>>>>>>>>>>>>> Output: [25, 16, 9, 4, 1]
```

像`map`一样，`forEach()`方法接收一个函数作为参数，并为每个数组元素执行一次。然而，它返回的不是像`map`那样的新数组，而是`undefined`。

```
const myAwesomeArray = [
  { id: 1, name: "john" },
  { id: 2, name: "Ali" },
  { id: 3, name: "Mass" },
]

myAwesomeArray.forEach(element => console.log(element.name))
// >>>>>>>>> Output : john
//                    Ali
//                    Mass
```

## 1.返回值

`map()`和`forEach()`的第一个区别是返回值。`forEach()`方法返回`undefined`，而`map()`返回一个包含已转换元素的新数组。即使他们做同样的工作，返回值仍然不同。

```
const myAwesomeArray = [1, 2, 3, 4, 5]
myAwesomeArray.forEach(x => x * x)
//>>>>>>>>>>>>>return value: undefined

myAwesomeArray.map(x => x * x)
//>>>>>>>>>>>>>return value: [1, 4, 9, 16, 25] 
```

## 2.链接其他方法的能力

这些数组方法之间的第二个区别是,`map()`是可链接的。这意味着您可以在一个数组上执行一个`map()`方法后附加`reduce()`、`sort()`、`filter()`等等。

这是你不能用`forEach()`做的事情，因为正如你可能猜到的，它返回`undefined`。

```
const myAwesomeArray = [1, 2, 3, 4, 5]
myAwesomeArray.forEach(x => x * x).reduce((total, value) => total + value)
//>>>>>>>>>>>>> Uncaught TypeError: Cannot read property 'reduce' of undefined
myAwesomeArray.map(x => x * x).reduce((total, value) => total + value)
//>>>>>>>>>>>>>return value: 55 
```

## 3.易变性

一般来说，“变异”这个词的意思是改变、交替、修改或转化。在 JavaScript 世界里，它有着相同的含义。

可变对象是在创建后其状态可以被修改的对象。那么，`forEach`和`map`关于可变性呢？

嗯，根据 [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript):

不改变调用它的数组。(不过，`callback`可能会这么做)。

`map()`不会改变调用它的数组(尽管`callback`如果被调用，可能会这样做)。

*JavaScript 很诡异*。

![Gif](img/5f5607635658b0a8808ee25c7f3e29cf.png)

这里，我们看到一个非常相似的定义，我们都知道它们都接收一个`callback`作为参数。那么，哪一个依赖于不变性呢？

在我看来，这个定义并不明确。为了知道哪一个没有改变原始数组，我们首先要检查这两个方法是如何工作的。

`map()`方法返回一个全新的数组，其中包含转换后的元素和相同数量的数据。在`forEach()`的情况下，即使返回`undefined`，也会用`callback`对原数组进行变异。

因此，我们清楚地看到，`map()`依赖于不变性，而`forEach()`是一个 mutator 方法。

## 4.性能速度

关于性能速度，它们有一点不同。但是，这有关系吗？嗯，这取决于各种因素，比如你的电脑、你要处理的数据量等等。

你可以自己用下面这个例子或者用 [jsPerf](https://jsperf.com/) 来检验一下，看看哪个更快。

```
const myAwesomeArray = [1, 2, 3, 4, 5]

const startForEach = performance.now()
myAwesomeArray.forEach(x => (x + x) * 10000000000)
const endForEach = performance.now()
console.log(`Speed [forEach]: ${endForEach - startForEach} miliseconds`)

const startMap = performance.now()
myAwesomeArray.map(x => (x + x) * 10000000000)
const endMap = performance.now()
console.log(`Speed [map]: ${endMap - startMap} miliseconds`) 
```

# 最后的想法

和往常一样，`map()`和`forEach()`之间的选择将取决于您的用例。如果您计划更改、替换或使用数据，您应该选择`map()`，因为它会返回一个包含已转换数据的新数组。

但是，如果你不需要返回的数组，就不要使用`map()`——而是使用`forEach()`或者甚至是`for`循环。

希望这篇文章能澄清这两种方法之间的区别。如有更多不同之处，请在评论区分享，否则感谢阅读。

在我的博客上阅读更多我的文章

在 [Unsplash](https://unsplash.com/s/photos/different?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Franck V.](https://unsplash.com/@franckinjapan?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片