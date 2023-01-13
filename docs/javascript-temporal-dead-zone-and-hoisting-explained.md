# JavaScript 中的时间死区(TDZ)和提升——用例子解释

> 原文：<https://www.freecodecamp.org/news/javascript-temporal-dead-zone-and-hoisting-explained/>

时间死区和提升是 JavaScript 中的两个基本术语。但是如果你不恰当地接近它们，理解它们是如何工作的很容易让你困惑。

但是不要烦恼！本文旨在帮助您很好地掌握这两个术语。

所以放松，拿起你最喜欢的一杯咖啡，让我们从 TDZ 开始吧。

## JavaScript 中的时间死区到底是什么？

一个**时间死区(TDZ)** 是一个变量在计算机用一个值完全初始化它之前不可访问的块区域。

*   一个[块](https://www.codesweetly.com/code-block/)是一对大括号(`{...}`)，用于对多个语句进行分组。
*   [初始化](https://www.codesweetly.com/declaration-initialization-invocation-in-programming/)发生在你给一个变量赋值一个初始值的时候。

假设您试图在变量完全初始化之前访问它。在这种情况下，JavaScript 会抛出一个`ReferenceError`。

所以，为了防止 JavaScript 抛出这样的错误，你必须记住从时间死区之外访问你的变量。

但是 TDZ 到底在哪里开始和结束呢？下面就来了解一下。

## 时间死区的确切范围在哪里？

块的时间死区从块的局部范围的开始处开始。当计算机用一个值完全初始化你的变量时，它结束。

**这里有一个例子:**

```
{
  // bestFood’s TDZ starts here (at the beginning of this block’s local scope)
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  console.log(bestFood); // returns ReferenceError because bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  let bestFood = "Vegetable Fried Rice"; // bestFood’s TDZ ends here
  // bestFood’s TDZ does not exist here
  // bestFood’s TDZ does not exist here
  // bestFood’s TDZ does not exist here
}
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-sjp8hk?file=index.js)

在上面的代码片段中，块的 TDZ 从开始的花括号(`{`)开始，在计算机用字符串值`"Vegetable Fried Rice"`初始化`bestFood`时结束。

当您运行该代码片段时，您将看到`console.log()`语句将返回一个`ReferenceError`。

JavaScript 将返回一个`ReferenceError`,因为我们在完成初始化之前使用了`console.log()`代码来访问`bestFood`。换句话说，我们在时间死区内调用了`bestFood`。

然而，以下是在完成初始化后成功访问`bestFood`的方法:

```
{
  // TDZ starts here (at the beginning of this block’s local scope)
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  let bestFood = "Vegetable Fried Rice"; // bestFood’s TDZ ends here
  console.log(bestFood); // returns "Vegetable Fried Rice" because bestFood’s TDZ does not exist here
  // bestFood’s TDZ does not exist here
  // bestFood’s TDZ does not exist here
}
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-utknki?file=index.js)

现在，考虑这个例子:

```
{
  // TDZ starts here (at the beginning of this block’s local scope)
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  // bestFood’s TDZ continues here
  let bestFood; // bestFood’s TDZ ends here
  console.log(bestFood); // returns undefined because bestFood’s TDZ does not exist here
  bestFood = "Vegetable Fried Rice"; // bestFood’s TDZ does not exist here
  console.log(bestFood); // returns "Vegetable Fried Rice" because bestFood’s TDZ does not exist here
}
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-uyxf4y?file=index.js)

可以看到上面代码片段中的第一个`console.log`代码返回了`undefined`。

JavaScript 返回了`undefined`，因为我们在使用[调用](https://www.codesweetly.com/declaration-initialization-invocation-in-programming/#what-does-invocation-mean-in-programming)之前没有给`bestFood`赋值。因此，JavaScript 将其值默认为`undefined`。

请记住，当[声明](https://www.codesweetly.com/declaration-initialization-invocation-in-programming/#what-exactly-does-declaration-mean)变量时，您必须为其指定一个值。除了这个例外，`let`变量的所有其他时间死区原则也适用于`const`。但是，`var`的工作原理不同。

## Var 的 TDZ 与 Let 和 Const 变量有何不同？

`var`、`let`和`const`变量的时间死区的主要区别在于它们的 TDZ 何时结束。

例如，考虑以下代码:

```
{
  // bestFood’s TDZ starts and ends here
  console.log(bestFood); // returns undefined because bestFood’s TDZ does not exist here
  var bestFood = "Vegetable Fried Rice"; // bestFood’s TDZ does not exist here
  console.log(bestFood); // returns "Vegetable Fried Rice" because bestFood’s TDZ does not exist here
  // bestFood’s TDZ does not exist here
  // bestFood’s TDZ does not exist here
}
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-gcn7j5?file=index.js)

当您运行上面的代码片段时，您会看到第一个`console.log`语句将返回`undefined`。

`console.log`语句成功返回值(`undefined`，因为 JavaScript 自动将`undefined`赋给一个被提升的`var`变量。

换句话说，当计算机提升一个`var`变量时，它会自动用值`undefined`初始化该变量。

相比之下，每当 JavaScript 提升变量时，它不会用任何值初始化一个`let`(或`const`)变量。相反，变量仍然是死的，不可访问的。

因此，`let`(或`const`)变量的 TDZ 在 JavaScript 用声明期间指定的值完全初始化它时结束。

然而，`var`变量的 TDZ 在提升后立即结束——而不是在变量用声明期间指定的值完全初始化时。

但是“吊装”到底是什么意思呢？下面就来了解一下。

## JavaScript 中的提升到底是什么意思？

**提升**是指 JavaScript 在程序执行过程中赋予变量、类、函数声明更高的优先级。

提升在任何其他代码之前进行计算机进程声明。

**注意:**提升并不意味着 JavaScript 重新排列或移动代码。

提升只是给了 JavaScript 声明更高的特异性。因此，它使计算机在分析程序中的任何其他代码之前，首先读取和处理声明。

例如，考虑以下代码片段:

```
{
  // Declare a variable:
  let bestFood = "Fish and Chips";

  // Declare another variable:
  let myBestMeal = function () {
    console.log(bestFood);
    let bestFood = "Vegetable Fried Rice";
  };

  // Invoke myBestMeal function:
  myBestMeal();
}

// The code above will return:
"Uncaught ReferenceError: Cannot access 'bestFood' before initialization"
```

[**在 StackBlitz** 上试一下](https://stackblitz.com/edit/js-ymmkih?file=index.js)

根据计算机执行每个代码的优先顺序，上面的代码片段返回了一个`ReferenceError`。

换句话说，程序的[声明](https://www.codesweetly.com/declaration-initialization-invocation-in-programming#what-exactly-does-declaration-mean)比[初始化](https://www.codesweetly.com/declaration-initialization-invocation-in-programming#what-does-initialization-mean)、[调用](https://www.codesweetly.com/declaration-initialization-invocation-in-programming#what-does-invocation-mean-in-programming)和其他代码具有更高的优先级。

让我们一步一步地浏览 JavaScript 如何执行上面的代码片段。

## JavaScript 提升是如何一步一步工作的

下面是 JavaScript 如何执行前面的代码片段的演示。

### 1.JavaScript 解析了第一个`bestFood`声明

```
let bestFood // This is the first bestFood declaration in the program
```

第一个`bestFood`变量声明是计算机分析的第一个代码。

注意，在计算机读取了`bestFood`变量声明后，JavaScript 自动将变量保存在*临时死区*中，直到它被完全初始化。

因此，在完成初始化之前访问`bestFood`的任何尝试都会返回一个`ReferenceError`。

### 2.计算机解析了`myBestMeal`变量声明

```
let myBestMeal
```

变量声明是 JavaScript 分析的第二个代码。

在计算机读取了`myBestMeal`变量声明之后，JavaScript 立即自动将变量保存在一个临时死区中，直到它被完全初始化。

因此，在完成初始化之前访问`myBestMeal`的任何尝试都会返回一个`ReferenceError`。

### 3.计算机初始化了`bestFood`变量

```
bestFood = "Fish and Chips";
```

计算机的第三步是用`“Fish and Chips”`字符串值初始化`bestFood`。

因此，此时调用`bestFood`将返回`“Fish and Chips”`。

### 4.JavaScript 初始化了`myBestMeal`变量

```
myBestMeal = function() {
  console.log(bestFood);
  let bestFood = "Vegetable Fried Rice";
};
```

第四，用指定函数初始化`myBestMeal`的 JavaScript。因此，如果您在此时调用了`myBestMeal`,函数就会返回。

### 5.电脑调用了`myBestMeal`的功能

```
myBestMeal();
```

调用`myBestMeal`的函数是计算机的第五个动作。

调用之后，计算机处理函数块中的每个代码。然而，这些声明比其他代码具有更高的优先级。

### 6.JavaScript 解析了函数的`bestFood`声明

```
let bestFood // This is the second bestFood declaration in the program
```

JavaScript 的第六个任务是分析函数的`bestFood`变量声明。

分析之后，JavaScript 自动将变量保存在一个临时死区中，直到它完成初始化。

因此，在完成初始化之前访问`bestFood`的任何尝试都会返回一个`ReferenceError`。

### 7.计算机解析了函数的`console.log`语句

```
console.log(bestFood);
```

最后，计算机读取了`console.log`语句——该语句指示系统将`bestFood`的内容记录到浏览器的控制台。

然而，记住计算机还没有完全初始化函数的`bestFood`变量。因此，变量当前处于时间死区。

因此，系统试图访问该变量时会返回一个`ReferenceError`。

**注意:**`ReferenceError`返回后，计算机停止读取函数代码。因此，JavaScript 没有用`"Vegetable Fried Rice"`初始化函数的`bestFood`变量。

## 包装它

让我们完整地看一下之前的程序演练:

```
let bestFood // 1\. JavaScript parsed the first bestFood declaration

let myBestMeal // 2\. the computer parsed myBestMeal variable declaration

bestFood = "Fish and Chips"; // 3\. the computer initialized the bestFood variable

myBestMeal = function () {
  console.log(bestFood);
  let bestFood = "Vegetable Fried Rice";
}; // 4\. JavaScript initialized myBestMeal variable

myBestMeal(); // 5\. the computer invoked myBestMeal’s function

let bestFood // 6\. JavaScript parsed the function’s bestFood declaration

console.log(bestFood); // 7\. the computer parsed the function’s console.log statement

Uncaught ReferenceError // bestFood’s invocation returned an Error
```

您可以看到 JavaScript 在其他代码之前处理了程序的声明。

在程序中其他代码之前解析声明就是我们所说的“提升”。

## 概观

本文讨论了 JavaScript 中时间死区和提升的含义。我们还用例子来说明它们是如何工作的。

感谢阅读！

### 这里有一个有用的资源:

我写了一本关于 React 的书！

*   这是初学者友好✔
*   它有✔的现场代码片段
*   它包含可扩展的项目✔
*   ✔有很多容易理解的例子

[React 解释清楚](https://amzn.to/30iVPIG)的书就是你理解 ReactJS 所需要的全部。

[![React Explained Clearly Book Now Available at Amazon](img/01a810717269c181d904ccbfc9224fa0.png)](https://amzn.to/30iVPIG)