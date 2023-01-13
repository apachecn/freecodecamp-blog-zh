# 何时使用函数声明，何时使用函数表达式

> 原文：<https://www.freecodecamp.org/news/when-to-use-a-function-declarations-vs-a-function-expression-70f15152a0a0/>

#### 技术术语系列

您可能已经知道如何用这两种方式编写函数。`function doStuff() {}`和`() =>` {}是我们整天打字的字符。但是它们有什么不同，为什么要使用其中的一个？

**注:**JavaScript 中给出了例子。 *Y* 我们的*M*I leage*M*ay*V*与其他语言不同。

![qAG2vEw9KwotPTqwyfiaoNL9HMdE7l5VkayJ](img/b02c64cfcf71bd9dac6933c1838d15d3.png)

### 第一个区别:名字

当你创建一个名为的*函数时，那就是一个**函数声明**。该名称可能在**函数表达式**中被省略，使该函数成为“匿名的”。*

**函数声明:**

```
function doStuff() {};
```

**函数表达式:**

```
const doStuff = function() {}
```

我们经常看到匿名函数与 ES6 语法一起使用，如下所示:

```
const doStuff = () => {}
```

### 提升

提升是指函数和变量在代码“顶部”的可用性，而不是仅在它们被创建之后。这些对象是在编译时初始化的，可以在文件中的任何地方找到。

函数声明被提升，但函数表达式没有。

举个例子很容易理解:

```
doStuff();
```

```
function doStuff() {};
```

上面没有抛出错误，但这会:

```
doStuff();
```

```
const doStuff = () => {};
```

### 函数表达式的大小写

看起来，函数声明凭借其强大的提升特性，将会挤掉函数表达式的有用性。但是选择其中一个需要考虑何时何地需要这个功能。基本上，谁需要了解它？

调用函数表达式**以避免污染全局范围**。你的程序并不知道许多不同的函数，当你让它们匿名时，它们会被立即使用和遗忘。

#### 生活

名字— **立即调用函数表达式** —在这里几乎说明了一切。当一个函数在被调用的同时被创建，你可以使用一个 life，看起来像这样:

```
(function() => {})()
```

或者:

```
(() => {})()
```

要深入了解生命，请查看这篇综合文章。

#### 回收

在 JavaScript 中，传递给另一个函数的函数通常被称为“回调”。这里有一个例子:

```
function mapAction(item) {
  // do stuff to an item
}
array.map(mapAction)
```

```
array.map(mapAction)
```

这里的问题是`mapAction`将对您的整个应用程序可用——没有必要这样做。如果回调是一个函数表达式，那么它在使用它的函数之外是不可用的:

```
array.map(item => { //do stuff to an item })
```

或者

```
const mapAction = function(item) {
  // do stuff to an item
}
array.map(mapAction)
```

```
array.map(mapAction)
```

虽然`mapAction`将可用于代码*下面的*其初始化。

### 摘要

简而言之，当您想在全局范围内创建一个函数并使它在整个代码中可用时，请使用函数声明。使用函数表达式来限制函数的可用位置，保持全局范围的宽松，并保持清晰的语法。

### 参考

*   [函数声明](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function)，MDN 文档。
*   [函数表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/function)，MDN docs。
*   [吊装](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)，MDN 词汇表。

### 科技术语系列

在科技集会和会议上，有太多的短语被抛来抛去，假设每个人都已经熟悉了这些行话。我经常不理解行话。开发人员对我缺乏一点知识感到惊讶是很常见的。

事实是，我经常不知道该用什么词来形容它。作为人类，尤其是开发人员，我们喜欢拒绝那些不“说空话”的人，所以本系列文章是关于获得一个人“应该知道”的编程概念的坚实理解。

这是该系列的第二篇文章。第一个是[高阶函数](https://medium.freecodecamp.org/higher-order-functions-what-they-are-and-a-react-example-1d2579faf101)。当我去参加聚会和会议，假装知道我的技术伙伴们在谈论什么的时候，你会发现更多，但之后不得不回家去谷歌一下。