# 什么是递归？用 JavaScript 代码示例解释的递归函数

> 原文：<https://www.freecodecamp.org/news/what-is-recursion-in-javascript/>

递归是一种用于解决计算机问题的技术，它通过创建一个函数来调用自身，直到程序达到预期的结果。

本教程将帮助你了解递归，以及它与更常见的循环相比有何不同。

## 什么是递归？

假设您有一个记录数字 1 到 5 的函数。下面是如何使用递归来编写它:

```
function log(num){
    if(num > 5){
        return;
    }
    console.log(num);
    log(num + 1);
}

log(1);
```

A recursive function example

当你运行上面的代码时，只要变量`num`的值小于`5`，函数`log`就会简单地调用自己。

递归函数必须至少有一个停止调用自身的条件，否则函数将无限期地调用自身，直到 JavaScript 抛出错误。

阻止递归函数调用自身的条件被称为**基本情况**。在上面的`log`函数中，基本情况是`num`大于`5`。

## 为什么不用一个循环呢？

任何可以使用递归函数解决的问题，总会有另一种循环解决方案。上面的示例可以替换为以下代码:

```
for(let i = 1; i <= 5; i++){
    console.log(i);
}
```

A for loop alternative to the recursive function

像 JavaScript 这样的现代编程语言已经有了`for`和`while`语句作为递归函数的替代。但是有些语言像 Clojure 没有任何循环语句，所以你需要使用递归来重复执行一段代码。

另外，`for`循环要求你知道你将重复代码执行多少次。但是一个递归函数和一个`while`循环可以用来执行一段代码，而不知道需要重复多少次。你只需要知道停止执行的条件。

例如，假设您有如下任务:

*   在 1 到 10 之间随机选择一个数字，直到你得到数字 5。
*   记录在 random 方法返回 5 之前需要执行代码的次数。

下面是使用递归函数的方法:

```
function randomUntilFive(result = 0, count = 0){
    if(result === 5){
        console.log(`The random result: ${result}`);
        console.log(`How many times random is executed: ${count}`);
        return;
    }
    result = Math.floor(Math.random() * (10 - 1 + 1) + 1);
    count++;
    randomUntilFive(result, count);
}

randomUntilFive();
```

Recursively random a number until it returns 5

不能用`for`循环替换上面的代码，但是可以用`while`循环替换:

```
let result = 0;
let count = 0;

while (result !== 5) {
  result = Math.floor(Math.random() * (10 - 1 + 1) + 1);
  count++;
}

console.log(`The random result: ${result}`);
console.log(`How many times random is executed: ${count}`);
```

Replacing recursive function with the while loop

除了要求你使用递归解决问题的编码面试问题，你总能找到使用`for`或`while`循环语句的替代解决方案。

## 如何读取递归函数

乍一看，递归函数并不直观，也不容易理解。以下步骤将帮助您更快地阅读和理解递归函数:

*   在做任何事情之前，一定要确定函数的基本情况。
*   将参数传递给将立即到达基本用例的函数。
*   确定至少会执行一次递归函数调用的参数。

让我们使用上面的`randomUntilFive()`例子来尝试这些步骤。您可以在上面的`if`语句中确定该函数的基本情况:

```
function randomUntilFive(result = 0, count = 0){
    if(result === 5){
        // base case is triggered
    }
    // recursively call the function
}

randomUntilFive();
```

Identifying the base case of the recursive function

这意味着您可以通过将数字`5`传递到`result`参数中来达到基本情况，如下所示:

```
function randomUntilFive(result = 0, count = 0){
    if(result === 5){
        console.log(`The random result: ${result}`);
        console.log(`How many times random is executed: ${count}`);
        return;
    }
}

randomUntilFive(5);
```

Getting to the base case of the recursive function

虽然`count`参数不应该为零，但是将数字`5`作为参数传递给上面的函数调用就满足了第二步的要求。

最后，您需要找到一个至少会执行一次递归函数调用的参数。在上面的例子中，您可以传递除了`5`之外的任何数字，或者什么都不传递:

```
function randomUntilFive(result = 0, count = 0){
    if(result === 5){
        console.log(`The random result: ${result}`);
        console.log(`How many times random is executed: ${count}`);
        return;
    }
    result = Math.floor(Math.random() * (10 - 1 + 1) + 1);
    count++;
    randomUntilFive(result, count);
}

randomUntilFive(4); 
// any number other than five 
// will execute the recursive call
```

你就完了。现在你明白了，函数`randomUntilFive()`会递归地调用自己，直到`result`的值等于 5。

## 如何编写递归函数

编写递归函数与读取递归函数几乎是一样的:

*   创建一个带有基本用例的常规函数，该函数的参数可以到达
*   将参数传递给函数，立即触发基本情况
*   只传递一次触发递归调用的下一个参数。

假设您正在编写一个计算[阶乘](https://en.wikipedia.org/wiki/Factorial)的函数。这是五的阶乘:

**5*4*3*2*1 = 120**

首先，这个函数的基本情况是 1，所以让我们创建一个返回 1 的`factorial`函数:

```
function factorial(num){
    if(num === 1){
        return num;
    }

}

console.log(factorial(1));
```

The base case for factorial

现在进行第三步。我们需要在函数中得到一个递归调用，并且至少调用一次。因为阶乘计算在每次乘法时将数字减 1，所以您可以通过将`num-1`传递到递归调用中来模拟它:

```
function factorial(num){
    if(num === 1){
        return num;
    }
    return num * factorial(num-1) 
}

console.log(factorial(2));
```

The recursive factorial completed

现在你完了。您可以通过向调用传递 5 来测试该函数:

```
console.log(factorial(5));
```

Testing the factorial function

## 结论

您刚刚学习了什么是递归函数，以及它与常见的`for`和`while`循环语句的比较。递归函数必须始终至少有一个基本情况，以使其停止调用自身，否则将导致错误。

在读取递归函数时，您需要模拟一种情况，即在不执行递归调用的情况下立即执行基本用例。

一旦你有了基本情况，返回一步，尝试至少执行一次递归调用。这样，你的大脑将遍历递归代码，并直观地理解它做什么。

这同样适用于编写递归函数。总是首先创建基本用例，然后编写一个至少运行一次递归调用的参数。剩下的就容易多了。

## 感谢阅读本教程

如果你想了解更多，我写了关于[如何使用递归](https://sebhastian.com/fibonacci-recursion-javascript/)找到斐波那契数列，这是最常见的递归问题之一。

我还有一份关于 web 开发教程的免费每周简讯(大多与 JavaScript 相关)。