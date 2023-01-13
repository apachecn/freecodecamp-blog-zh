# JavaScript 中的 spread 运算符和 rest 参数介绍(ES6)

> 原文：<https://www.freecodecamp.org/news/spread-operator-and-rest-parameter-in-javascript-es6-4416a9f47e5e/>

乔安娜·高登

# JavaScript 中的 spread 运算符和 rest 参数介绍(ES6)

#### spread 运算符和 rest 参数都写为三个连续的点(…)。他们还有什么共同点吗？

![xVf5V7-nilHXYj2WtcgAgE8QNW-DY6I23NS5](img/54d8f5aa251aa94a73ca03f1d992e62c.png)

[source](https://unsplash.com/photos/8AJuvX4hzws)

### 扩展运算符(…)

扩展运算符是在 ES6 中引入的。它提供了将[可迭代对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators#Iterators)扩展成多个元素的能力。这到底意味着什么？让我们检查一些例子。

```
const movies = ["Leon", "Love Actually", "Lord of the Rings"];console.log(...movies);
```

印刷品:

> 莱昂其实很喜欢《魔戒》

```
const numbers = new Set([1, 4, 5, 7]);console.log(...numbers);
```

印刷品:

> 1 4 5 7

您可能会注意到，第一个示例中的数组和第二个示例中的集合都被扩展成了各自的元素(分别是字符串和数字)。你可能会问，这有什么用？

最常见的用法可能是组合数组。如果您在 spread 操作符出现之前必须这样做，您可能会使用`concat()`方法。

```
const shapes = ["triangle", "square", "circle"];const objects = ["pencil", "notebook", "eraser"];const chaos = shapes.concat(objects);console.log(chaos);
```

印刷品:

> 【“三角形”、“正方形”、“圆形”、“铅笔”、“笔记本”、“橡皮擦”】

这还不算太糟，但是 spread 操作符提供了一种快捷方式，这也使您的代码看起来更具可读性:

```
const chaos = [...shapes, ...objects];console.log(chaos);
```

印刷品:

> 【“三角形”、“正方形”、“圆形”、“铅笔”、“笔记本”、“橡皮擦”】

如果我们尝试在没有扩展操作符的情况下做同样的事情*，我们会得到以下结果:*

```
const chaos = [shapes, objects];console.log(chaos);
```

印刷品:

> [数组(3)，数组(3)]

这里发生了什么？我们没有合并数组，而是得到了一个`chaos`数组，其中`shapes`数组位于索引 0，而`objects`数组位于索引 1。

![FWmzdtMxBzeGEB1G5byEABuOfQOA6QkCu82f](img/f6b803600c13cdc694573d97606f5ecc.png)

[source](https://unsplash.com/photos/DJpJDdeCrag)

### rest 参数(…)

您可以将 rest 参数视为 spread 运算符的反义词。正如 spread 操作符允许您将数组扩展为单个元素一样，rest 参数允许您将元素捆绑回数组中。

#### 将数组的值赋给变量

让我们看看下面的例子:

```
const movie = ["Life of Brian", 8.1, 1979, "Graham Chapman", "John Cleese", "Michael Palin"];const [title, rating, year, ...actors] = movie;console.log(title, rating, year, actors);
```

印刷品:

> 《布莱恩的一生》，8.1，1979 年，[《格雷厄姆·查普曼》，《约翰·克立斯》，《麦克·帕林》]

rest 参数让我们获取`movie`数组的值，并使用[析构](https://medium.com/@aska.gaudyn/destructuring-in-javascript-es6-ee963292623a)将它们分配给几个单独的变量。这样`title`、`rating`和`year`被分配了数组中的前三个值，但是真正神奇的地方是`actors`。多亏了 rest 参数，`actors`以数组的形式被赋予了`movie`数组的剩余值。

#### 可变函数

变元函数是接受不定数量参数的函数。一个很好的例子是`sum()`函数:我们无法预先知道有多少参数将被传递给它:

```
sum(1, 2);sum(494, 373, 29, 2, 50067);sum(-17, 8, 325900);
```

在 JavaScript 的早期版本中，这种函数将使用 [arguments 对象、](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments)来处理，这是一个类似数组的对象，可以作为每个函数中的局部变量。它包含传递给函数的所有参数值。让我们看看如何实现`sum()`函数:

```
function sum() {  let total = 0;    for(const argument of arguments) {    total += argument;  }  return total;}
```

它确实有效，但远非完美:

*   如果你看一下`sum()`函数的定义，它没有任何参数。这很容易让人误解。
*   如果你不熟悉 arguments 对象，这可能很难理解(比如:`arguments`到底是从哪里来的？！)

下面是我们如何用 rest 参数编写相同的函数:

```
function sum(...nums) {  let total = 0;    for(const num of nums) {    total += num;  }  return total;}
```

请注意，`for...in`回路也已经被`[for...of](https://medium.freecodecamp.org/javascript-iterators-17ab32c3cae7)` [回路](https://medium.freecodecamp.org/javascript-iterators-17ab32c3cae7)所取代。我们立刻使我们的代码更加易读和简洁。

哈利路亚 ES6！

你是刚开始编程之旅吗？这里有一些你可能会感兴趣的文章:

*   编码训练营适合你吗？
*   [转行真的可能吗？](https://medium.com/datadriveninvestor/is-career-change-really-possible-c29c9a0d791c)
*   [为什么 Ruby 是开始编程的绝佳语言](https://medium.com/@aska.gaudyn/why-ruby-is-a-great-language-to-start-programming-with-2f17e0c2907a)