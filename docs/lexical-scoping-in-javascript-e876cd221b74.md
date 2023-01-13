# JavaScript 中词法范围的简单介绍

> 原文：<https://www.freecodecamp.org/news/lexical-scoping-in-javascript-e876cd221b74/>

迈克尔·麦克米伦

# JavaScript 中词法范围的简单介绍

![UHbCaHt-CGB2RPG1op4yMiMtuETkBSoQQXht](img/4a2e46867d9f22b26640c2679260fd36.png)

Photo by [Temple Cerulean](https://unsplash.com/photos/tP8ZwlCF8og?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

词法范围是一个让许多程序员害怕的话题。对词法范围的最好解释之一可以在 Kyle Simpson 的书[中找到。然而，甚至他的解释也是缺乏的，因为他没有使用真实的例子。](https://www.amazon.com/You-Dont-Know-JS-Closures/dp/1449335586)

关于词法范围如何工作以及它为什么重要的最好的真实例子之一，可以在著名的教科书“计算机程序的结构和解释”(SICP)中找到，作者是哈罗德·艾贝尔森和杰拉德·让伊·萨斯曼。这里有这本书的 PDF 版本链接: [SICP](https://web.mit.edu/alexmv/6.037/sicp.pdf) 。

SICP 使用 Scheme，这是 Lisp 的一种方言，被认为是有史以来最好的计算机科学入门教材之一。在本文中，我想重温一下他们使用 JavaScript 作为编程语言的词法范围示例。

#### 我们的例子

艾贝尔森和苏斯曼使用的例子是用牛顿法计算平方根。牛顿法的工作原理是确定一个数的平方根的连续近似值，直到近似值在可接受的公差范围内。让我们看一个例子，就像艾贝尔森和苏斯曼在 SICP 做的那样。

他们用的例子是求 2 的平方根。你从猜测 2 的平方根开始，比如说 1。您可以通过将原始数字除以猜测值，然后对该商和当前猜测值进行平均来得出下一个猜测值，从而改进这个猜测值。当你达到一个可接受的近似水平时，你就停下来。艾贝尔森和苏斯曼使用 0.001 这个值。以下是计算中前几个步骤的流程:

```
Square root to find: 2First guess: 1Quotient: 2 / 1 = 2Average: (2+1) / 2 = 1.5Next guess: 1.5Quotient: 1.5 / 2 = 1.3333Average: (1.3333 + 1.5) / 2 = 1.4167Next guess: 1.4167Quotient: 1.4167 / 2 = 1.4118Average: (1.4167 + 1.4118) / 2 = 1.4142
```

依此类推，直到猜测在我们的近似极限内，对于这个算法是 0.001。

### 牛顿法的 JavaScript 函数

在这种方法的演示之后，作者描述了在方案中解决这一问题的一般程序。我不会向您展示方案代码，而是用 JavaScript 将它写出来:

```
function sqrt_iter(guess, x) {  if (isGoodEnough(guess, x)) {    return guess;  }    else {    return sqrt_iter(improve(guess, x), x);  }}
```

接下来，我们需要充实其他几个函数，包括 isGoodEnough()和 improve()，以及其他一些辅助函数。我们将从改进()开始。定义如下:

```
function improve(guess, x) {  return average(guess, (x / guess));}
```

这个函数使用了一个辅助函数 average()。定义如下:

```
function average(x, y) {  return (x+y) / 2;}
```

现在我们准备定义 isGoodEnough()函数。该函数用于确定我们的猜测何时足够接近我们的近似容差(0.001)。isGoodEnough()的定义如下:

```
function isGoodEnough(guess, x) {  return (Math.abs(square(guess) - x)) < 0.001;}
```

这个函数使用 square()函数，它很容易定义:

```
function square(x) {  return x * x;}
```

现在我们只需要一个函数来启动事情:

```
function sqrt(x) {  return sqrt_iter(1.0, x);}
```

这个函数使用 1.0 作为开始的猜测，这通常就可以了。

现在我们准备测试我们的函数，看看它们是否工作。我们将它们加载到 JS shell 中，然后计算一些平方根:

```
> .load sqrt_iter.js> sqrt(3)1.7321428571428572> sqrt(9)3.00009155413138> sqrt(94 + 87)13.453624188555612> sqrt(144)12.000000012408687
```

这些功能似乎运行良好。然而，这里潜伏着一个更好的想法。这些函数都是独立编写的，尽管它们应该相互协作。我们可能不会将 isGoodEnough()函数与任何其他函数集一起使用，或者单独使用。此外，对用户来说唯一重要的函数是 sqrt()函数，因为它是被调用来求平方根的函数。

### 块作用域隐藏了助手函数

这里的解决方案是使用块范围来定义 sqrt()函数块中所有必需的助手函数。我们将从定义中删除 square()和 average()，因为这些函数可能在其他函数定义中有用，而不仅限于在实现牛顿方法的算法中使用。下面是 sqrt()函数的定义，以及在 sqrt()范围内定义的其他帮助函数:

```
function sqrt(x) {  function improve(guess, x) {    return average(guess, (x / guess));  }  function isGoodEnough(guess, x) {    return (Math.abs(square(guess) - x)) > 0.001;  }  function sqrt_iter(guess, x) {    if (isGoodEnough(guess, x)) {      return guess;    }    else {      return sqrt_iter(improve(guess, x), x);    }  }  return sqrt_iter(1.0, x);}
```

我们现在可以将这个程序加载到我们的 shell 中，并计算一些平方根:

```
> .load sqrt_iter.js> sqrt(9)3.00009155413138> sqrt(2)1.4142156862745097> sqrt(3.14159)1.772581833543688> sqrt(144)12.000000012408687
```

请注意，您不能从 sqrt()函数外部调用任何助手函数:

```
> sqrt(9)3.00009155413138> sqrt(2)1.4142156862745097> improve(1,2)ReferenceError: improve is not defined> isGoodEnough(1.414, 2)ReferenceError: isGoodEnough is not defined
```

因为这些函数(improve()和 isGoodEnough())的定义已经被移到了 sqrt()的范围内，所以不能在更高的级别访问它们。当然，您可以将任何辅助函数定义移到 sqrt()函数之外，以便能够全局访问它们，就像我们对 average()和 square()所做的那样。

我们已经大大改进了 Newton 方法的实现，但是我们还可以通过利用词法范围进一步简化 sqrt()函数来改进它。

### 用词法范围提高清晰度

词法范围背后的概念是，当一个变量被绑定到一个环境时，在该环境中定义的其他过程(函数)可以访问该变量的值。这意味着在 sqrt()函数中，参数 x 被绑定到该函数，这意味着在 sqrt()主体中定义的任何其他函数都可以访问 x。

了解了这一点，我们可以通过删除函数定义中对 x 的所有引用来进一步简化 sqrt()的定义，因为 x 现在是一个自由变量，所有函数都可以访问它。下面是我们对 sqrt()的新定义:

```
function sqrt(x) {  function isGoodEnough(guess) {    return (Math.abs(square(guess) - x)) > 0.001;  }  function improve(guess) {    return average(guess, (x / guess));  }  function sqrt_iter(guess) {    if (isGoodEnough(guess)) {      return guess;    }    else {      return sqrt_iter(improve(guess));    }  }  return sqrt_iter(1.0);}
```

对参数 x 的唯一引用是在需要 x 值的计算中。让我们将这个新定义加载到 shell 中并测试它:

```
> .load sqrt_iter.js> sqrt(9)3.00009155413138> sqrt(2)1.4142156862745097> sqrt(123+37)12.649110680047308> sqrt(144)12.000000012408687
```

词法范围和块结构是 JavaScript 的重要特性，允许我们构造更容易理解和管理的程序。当我们开始构建更大、更复杂的程序时，这一点尤其重要。

![S2PRBM-GSiBQBuXWf1q614LInt0MBmr9rTuN](img/d7e051f61249c1a4d8fb66f139f8dd9a.png)