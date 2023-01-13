# JavaScript 中的参数与实参——有什么区别？

> 原文：<https://www.freecodecamp.org/news/what-is-the-difference-between-parameters-and-arguments-in-javascript/>

JavaScript 是 web 开发中最流行的编程语言之一。

当 JavaScript 函数在代码中的另一个位置被调用时，该函数中传递的动态值可能会改变。我们用来命名这些数据的关键字是参数和自变量，但是一些开发人员混淆了它们。

在本文中，您将了解参数和实参，它们是什么，以及何时何地使用它们。

### 目录

*   JavaScript 函数简介
*   如何在函数中使用参数和实参
*   论据的力量
*   结论

## JavaScript 函数简介

JavaScript 编程的基本构件之一是函数。它是为执行特定任务而设计的代码块。

函数是可重用的代码，你可以在程序的任何地方使用。它们消除了一直重复相同代码的需要。

要以一种强大的方式使用函数，您可以在函数中传递值来使用它们。

下面是一个函数示例:

```
function add(){
	return 2 + 3
}

add()
```

这是一个函数声明，函数名是`add`，在带`add()`的函数后被调用。该函数的结果将是 5。

让我们在函数中引入参数和自变量。

## 如何在函数中使用参数和实参

现在看看我们的函数代码:

```
function add(x, y){
	return x + y
}

add(2, 3)
```

我们在这里引入了 x 和 y，并改变了 2 和 3 的位置。x 和 y 是参数，而 2 和 3 是这里的自变量。

**参数**是函数中的变量之一。当一个方法被调用时，**参数**是传递给方法参数的数据。

当用`add(2, 3)`调用函数时，参数 2 和 3 分别被赋给 x 和 y。这意味着在函数中，x 将被替换为 2，y 将被替换为 3。

如果用不同的参数调用函数，同样适用。参数就像函数参数的占位符。

## 论据的力量

当我们想让函数更加可重用，或者想让另一个函数内部的调用函数更加强大时，我们可以更有效地使用参数。

这里有一个例子:

```
function add(x, y){
	return x + y
}

function multiply(a, b, c){ // a = 1, b = 2, c = 3
	const num1 = add(a, b) // num1 = add(1, 2) = 3
	const num2 = add(b, c) // num2 = add(2, 3) = 5

	return num1 * num2 // 15
}

multiply(1, 2, 3)
// returns 15
```

第一个函数`add()`有两个参数，x 和 y。该函数返回两个参数的和。

第二个函数`multiply()`有三个参数:在函数内部，声明了两个变量，`num1`和`num2`。`num1`将存储`add(a, b)`的结果值，`num2`将存储`add(b, c)`的结果值。最后，`multiply`函数将返回`num1`乘以`num2`的值。

`multiply`用三个参数调用，分别是 1、2 和 3。`add(a, b)`将是`add(1, 2)`，它将返回 3。`add(b, c)`将是`add(2, 3)`，它将返回 5。

`num1`的值为 3，而`num2`的值为 5。`num1 * num2`将返回 15。

参数在`multiply`函数中传递，也用作`add`函数的参数。

## 结论

使用参数和实参有时会令人困惑，尤其是在您刚刚开始学习它们的时候。但是，如果您首先正确地了解什么是函数以及它是如何工作的，您将很容易理解参数和自变量。

感谢阅读这篇文章。如果你喜欢它，考虑分享它来帮助其他开发者。

你可以通过 [Twitter](https://twitter.com/iamsegunajibola) 、 [LinkedIn](https://www.linkedin.com/in/segunajibola/) 和 [GitHub](https://github.com/segunajibola) 联系我。

快乐学习。