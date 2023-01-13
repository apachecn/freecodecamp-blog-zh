# 用 let 和 const 提升 JavaScript 以及它与 var 有何不同

> 原文：<https://www.freecodecamp.org/news/javascript-let-and-const-hoisting/>

我以前认为提升只发生在用`var`声明的变量上。但是最近，我发现用`let`和`const`声明的变量也会发生这种情况。

我会在这篇文章中解释我的意思。

我也有这篇文章的视频版本，如果你感兴趣的话可以看看。

## 在 JavaScript 中提升如何与`var`一起工作

下面是提升如何作用于用`var`声明的变量:

```
console.log(number)
// undefined

var number = 10

console.log(number)
// 10 
```

`number`变量被提升到全局范围的顶部。这使得可以在声明变量的行之前正确地访问变量。

但是你会注意到这里只有变量声明(`var number`)被提升了——初始化(`= 10`)没有。因此，当您试图在声明`number`之前访问它时，您会得到默认的**初始化，这发生在变量**也就是`undefined`中。

然后，执行声明和初始化行，因此在此之后访问`number`将返回初始化的值， **10** 。

## 提升如何在 JavaScript 中与 let/const 一起工作

如果你尝试用`let`或`const`做同样的事情，会发生以下情况:

```
console.log(number)

let number = 10
// or const number = 10

console.log(number) 
```

您会得到一个错误消息: **ReferenceError:在初始化**之前无法访问“number”。

所以你可以在声明之前访问一个用 var 声明的变量而不会出错，但是你不能用`let`或`const`做同样的事情。

这就是为什么我一直认为提升只发生在 var 上，而不会发生在 let 或 const 上。

但是正如我所说的，我最近了解到用`let`或`const`声明的变量也被提升。让我解释一下。

看一下这个例子:

```
console.log(number2)

let number = 10 
```

我将一个名为`number2`的变量记录到控制台，并声明和初始化一个名为`number`的变量。

运行此代码会产生以下错误: **ReferenceError: number2 未定义**

在之前的错误和这个错误之间你注意到了什么？之前的错误说**引用错误:无法在初始化**前访问‘数字’，而这个新错误说**引用错误:数字 2 未定义**。

区别就在这里。前者说“初始化前不能访问”，后者说“未定义”。

后者意味着 JavaScript 不知道`number2`变量是什么，因为它没有被定义——事实上我们没有定义它。我们只定义了`number`。

但是前者没有说“未定义”，而是说“初始化前不能访问”。下面是代码:

```
console.log(number)
// ReferenceError: Cannot access 'number' before initialization

let number = 10

console.log(number) 
```

这意味着 JavaScript“知道”了`number`变量。它是怎么知道的？因为`number`被提升到全局范围的顶端。

但是为什么会出现错误呢？这澄清了`var`和`let` / `const`的提升行为之间的区别。

用`let`或`const`声明的变量被**提升，没有默认初始化**。所以在声明它们的行之前访问它们会抛出 **ReferenceError:在初始化**之前不能访问‘变量’。

但是用`var`声明的变量被**提升，默认初始化为未定义的**。所以在声明它们的行之前访问它们会返回`undefined`。

## 暂时死区

执行过程中`let` / `const`变量被提升但不可访问的那段时间有个名字:叫做**时间死区**。

同样，上面的代码:

```
console.log(number)

let number = 10

console.log(number) 
```

`number`变量在一个时间死区中，JavaScript 知道它的存在(因为它的声明被提升了)，但是它是不可访问的(因为它没有初始化)。

## 包扎

如果你像我一样，认为提升只适用于`var`而不适用于`let` / `const`，我希望这篇文章能澄清这个错误的假设。

正如我在本文中解释的那样，`let`和`const`变量是被提升的，只是它们没有默认的初始化。这使得它们不可访问(因为这样的变量处于时间死区中)。

另一方面，用`var`声明的变量用默认的初始化`undefined`提升。

希望你从这篇文章中学到了一些东西:)