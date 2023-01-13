# JavaScript 迭代器概述

> 原文：<https://www.freecodecamp.org/news/javascript-iterators-17ab32c3cae7/>

乔安娜·高登

# JavaScript 迭代器概述

#### 循环的 for，for…in 和 for…之间的区别

![tS2k4SixOdqRW2ZATfBIIOTtNTaqjv2hU3ba](img/cc070a7010fe1392f5d9b6c591b3ae21.png)

[source](https://unsplash.com/photos/kJAl5OPoRgQ)

迭代可能是编程中最常用的操作之一。它接受一组项目，并对序列中的每一个项目执行给定的操作。循环提供了一种快速简单的方法来重复做某事。

在 JavaScript 中，不同的循环机制允许您以不同的方式定义循环的开始和结束。对于哪种机制是最好的，没有简单的答案，因为不同的情况需要不同的方法，这意味着一种类型的循环比其他类型的循环更容易满足您的需求。

以下是您可以在 JavaScript 中使用的[循环:](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration)

*   do…while 语句
*   while 语句
*   有标号语句
*   break 语句
*   连续语句
*   for 语句
*   对于…在声明中
*   对于…的声明

让我们仔细看看最后 3 个。当您开始使用 JavaScript 时，它们很容易让人混淆，因为这些名称并没有让您更容易记住它们背后的机制。如果你对这些概念还不太清楚，我希望几个例子能让你明白。

![PjMWkwqImDEcv4L38v-POyEuuE8LndiFaRKz](img/07315fbeac882776f3a50472c28ee693.png)

[source](https://www.pexels.com/photo/red-surface-1412212/)

#### “for”循环

`for`循环是最常见的循环类型，你可能在其他编程语言中也遇到过，所以让我们快速回顾一下。

下面是基本语法:

```
for ([initialExpression]; [exit condition]; [incrementExpression]) {
  do something;
}
```

让我们举个例子:

```
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (let i = 0; i < numbers.length; i++) {
  console.log(numbers[i]);
}
```

打印 **:**

> 0
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9

这里到底发生了什么？重复一个`for`循环，直到指定的条件评估为假。在我们的例子中，我们从 0 开始一个计数器(变量`i`，从我们的`numbers`数组中打印一个数字，它位于数组的`i`索引处，最后递增计数器。我们还声明，当计数器不再小于数组的大小时，我们的循环应该中断(`numbers.length`)。

循环的最大缺点是必须跟踪计数器和退出条件。语法也不是很简单，要理解发生了什么，你只需要记住代码的每一部分代表什么。尽管`for`循环可能是遍历数组的实用解决方案，但通常还有更简洁的方法。

![nR8ENaf2lEQW1NzZH9sxELuxxwm3C1oFrNDf](img/08a8e7b83171795afc34af6e98f7e2ef.png)

[source](https://unsplash.com/photos/wKTF65TcReY)

#### for…in 循环

`for ... in`循环消除了`for`循环的两个主要弱点。不需要考虑计数器或退出条件。

```
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const index in numbers) {
  console.log(numbers[index]);
}
```

印刷品:

> 0
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9

这种解决方案的主要缺点是我们仍然需要使用索引来访问数组的元素。

另一个问题是`for ... in`循环遍历所有可枚举的属性。在实践中意味着什么？如果您需要向您的对象添加一个额外的方法(在我们的例子中是:array)，这个方法也会出现在您的循环中。

看看这个例子:

```
Array.prototype.decimalfy = function() {
  for (let i = 0; i < this.length; i++) {
    this[i] = this[i].toFixed(4);
  }
};

const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const index in numbers) {
  console.log(numbers[index]);
}
```

印刷品:

> 0
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9
> function(){
> for(设 I = 0；I<this . length；i++) {
> 这个[我] =这个[我]。toFixed(2)；
> }
> }

我想你会同意这不是我们想要的。在数组上循环时，尽量远离`for ... in`循环，以避免意外的惊喜。

![SGFyxaY3XVEM7kIYdpwHLHVREdS1C4cjKm3b](img/1969c3839bbd8b740898639c00ed65b7.png)

[source](https://pixabay.com/en/tiger-and-turtle-duisburg-looping-1940551/)

#### for … of 循环

for…of 循环是 JavaScript 中`for`循环家族的最新成员。

它结合了`for`循环和`for ... in` 循环的优点，允许您循环遍历任何类型的可迭代数据类型(=遵循[可迭代协议](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols))，比如字符串、数组、集合或映射。注意，默认情况下，对象(`{}`)是不可迭代的。

`for ... of`循环的语法与`for ... in`循环几乎相同:

```
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const number of numbers) {
  console.log(number);
}
```

印刷品:

> 0
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9

一个很大的优势是我们不再需要索引，可以直接访问元素或数组。它使`for ... of`循环成为整个 for 循环家族中最紧凑的。

此外，`for ... of`循环机制允许循环中断，这取决于您的条件。看看这个例子:

```
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

for (const number of numbers) {
  if (number % 2 !== 0) {
    continue;
  }
  console.log(number);
}
```

印刷品:

> 0
> 2
> 4
> 6
> 8

此外，向对象添加新方法不会影响`for ... of`循环:

```
Array.prototype.decimalfy = function() {
  for (i = 0; i < this.length; i++) {
    this[i] = this[i].toFixed(4);
  }
};
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
for (const number of numbers) {
  console.log(number);
}
```

印刷品:

> 0
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9

这使得`for ... of`循环成为所有循环中最有效的！

#### 附注:forEach 循环

同样值得一提的是一个`forEach`循环。但是请注意，`forEach()`是一个数组方法，因此不能用于其他 JavaScript 对象。如果您的情况满足以下两个条件，此方法会很有用:您希望循环遍历一个数组，并且不需要在该数组结束之前停止循环:

```
const numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];

numbers.forEach(function(number) {
  console.log(number);
});
```

印刷品:

> 0
> 1
> 2
> 3
> 4
> 5
> 6
> 7
> 8
> 9

我希望这些例子能帮助你理解 JavaScript 中所有不同的迭代机制。

你是刚开始编程之旅吗？这里有一些你可能会感兴趣的文章:

*   编码训练营适合你吗？
*   [转行真的可能吗？](https://medium.com/datadriveninvestor/is-career-change-really-possible-c29c9a0d791c)
*   [为什么 Ruby 是开始编程的绝佳语言](https://medium.com/@aska.gaudyn/why-ruby-is-a-great-language-to-start-programming-with-2f17e0c2907a)