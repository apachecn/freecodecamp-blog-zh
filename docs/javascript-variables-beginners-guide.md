# JavaScript 变量 var、const 和 let 初学者指南

> 原文：<https://www.freecodecamp.org/news/javascript-variables-beginners-guide/>

变量是任何编程语言中的一个基本概念。在 JavaScript 中，可以使用关键字 var、const 或 let 来声明变量。

在本文中，您将了解我们为什么使用变量，如何使用变量，以及 const、let 和 var 之间的区别。

## **JavaScript 中变量有什么用途？**

在编码的上下文中，数据是我们在计算机程序中使用的信息。例如，你的 Twitter 用户名是一段数据。

大部分编程都是关于操作或显示数据的。为了做到这一点，程序员需要一种存储和跟踪数据的方法。让我们用一个例子来证明这一点。

首先，我们将打开我们的 JavaScript 控制台。要在 Chrome 上启动 JavaScript 控制台，可以在 Windows 和 Linux 上使用快捷键 Ctrl + Shift + J。对于 Mac，使用 Cmd+Option+j .****

一旦控制台启动，想想你的狗或猫的当前年龄(或任何类似的数字，如果你没有宠物)，并输入到控制台。

```
4
```

如果我们想再次引用这个数字呢？我们得再打一遍。

我们需要一种方法来引用这段数据，这样我们就可以在整个程序中重用它。

## **在 JavaScript 中引入变量**

一个有用的类比是将变量视为我们价值观的标签。想象一个装有蓝莓的容器，上面有一个标签，标着蓝莓。在这个例子中，变量*蓝莓*指向一个值，这个值就是蓝莓本身。

让我们声明一个变量 age，并使用赋值操作符(等号)将我们的值 4 赋给这个变量。我们将使用 var 关键字。

```
var age = 4
```

变量是程序员如何给一个值命名的，这样我们就可以重用它，更新它，或者简单地跟踪它。变量可以用来存储任何 JavaScript 类型。

既然我们已经将这个值赋给了变量 age，我们可以在以后引用这个值。如果您现在在控制台中键入变量 age，您将得到返回给您的值 4。

## 如何在 JavaScript 中使用 var 关键字

JavaScript 中的关键字是保留字。当你使用 var 关键字时，你是在告诉 JavaScript 你将声明一个变量。

使用 var 关键字时，可以重新分配变量。我们将通过首先声明一个新变量 name 并将其赋值给 Madison 来演示这一点。

```
var name = 'Madison'
```

接下来，我们将重新分配这个变量，使其指向另一个名字 Ben 的值。

```
name = 'Ben'
```

现在如果你运行`console.log(name)`你会得到本的输出。

使用 var 关键字时，也可以声明没有初始值的变量。

```
var year
```

这里，我们声明了一个变量`year`，但是它没有指向任何值。稍后，如果我们想让它指向一个值，我们可以使用赋值操作符。

```
Year = 2020
```

现在我们的变量 year 将指向 2020 年的值。

最初创建 JavaScript 时，声明变量的唯一方式是使用 var 关键字。

在最近对 JavaScript (ECMAScript2015)的更新中，`const`和`let`被创建为声明变量的其他关键字。

为了解释为什么需要它们，我们将看看 var 关键字的问题。为了研究这些问题，我们将学习什么是作用域。

## 什么是范围？

作用域指的是在我们的代码中变量可以使用的地方。当一个变量*是全局作用域*时，这意味着它在程序中的任何地方都是可用的。让我们看一个例子。

将以下代码输入到您的控制台中。

```
var name = ‘Bob’
function printName() {
    console.log(name)
}
printName()
```

这里我们已经创建并调用了一个函数 printName，它将打印名称 var，`Madison`的值。你会在你的控制台上看到这个。

因为我们的 var 是在函数之外创建的，所以它是全局范围的。这意味着它在代码中的任何地方都是可用的，包括任何函数内部。这就是为什么我们的函数 printName 可以访问名称 var。

现在让我们创建一个函数范围的变量。这意味着变量只能在创建它的函数内部访问。下一个例子与上面的代码非常相似，但是变量的位置不同。

```
function printYear() {
 var year = 2020
}
console.log(year)
```

现在在我们的控制台中，我们将得到一个错误:`year is not defined.`这是因为 var year 是函数范围的。也就是说，它只存在于创建它的函数内部。除了函数之外，我们无法访问它，而函数就是我们运行 console.log 时试图访问它的地方。

函数范围的变量对程序员很有帮助，因为我们经常想创建只在某个函数内部有用或需要的变量。创建全局变量也会导致错误或失误。

现在我们对作用域有了一个基本的了解，我们可以回到我们对 var 关键字问题的讨论。

## JavaScript 中 var 关键字的问题

让我们看另一个例子。

我们将创建一个变量，`age`。接下来，我们将编写一个 if 语句来检查年龄是否有值，如果有，则返回一个两倍于年龄的 console.log。

这是一个简化的例子，但是我们将首先检查年龄是否有一个值，因为我们想确保我们添加的是一个有效值。

```
var age = 27
If (age) {
 var doubleAge = age + age
 console.log(`Double your current age is ${yearPlusTwenty}`)
}
```

现在在我们的控制台中，你会看到`Double your current age is 47`。

我们的变量`doubleAge`现在是一个全局变量。如果你输入`doubleAge`到你的控制台，你会看到你可以访问它。

```
doubleAge
47
```

如前所述，用 var 关键字创建的变量是函数范围的。函数作用域的变量仅存在于创建它们的函数内部。

但是由于`doubleAge`变量不在函数内部，这意味着它的作用域是全局的。也就是说，`doubleAge`变量现在可以在我们代码的任何地方使用。

问题是，`doubleAge`只是我们在`if statement`中使用过的一个变量，我们不需要它在代码中到处都可用。它已经“泄漏”到创建它的 if 语句之外，尽管我们并不需要它这样做。

```
var age = 27
if (age) {
 //We need our doubleAge var only in this block of code in between our curley brackets. 
    var doubleAge = age + age
    console.log(`Double your current age is ${yearPlusTwenty}`)

}

doubleAge
47
//our doubleAge var is available outside of these curly brackets, on the global sbope.
```

如果我们有办法创建一个只存在于 if 语句中的变量，那就太好了。换句话说，就是位于花括号之间的代码块。

```
var age = 27
If (age) {
 //We want our variable to only exist here, where we will use it
 var doubleAge = age + age
 console.log(`Double your current age is ${yearPlusTwenty}`)
}
```

为了帮助解决这个问题，JavaScript 中引入了 const 和 let 关键字。

## 如何在 JavaScript 中使用 const 关键字

与 var 类似，但有几个很大的区别。

首先，`const`是*块*作用域，而 var 是*函数*作用域。

什么是**块**？

*块*是指开合支架之间的任何空间。乍一看，这似乎令人困惑。让我们写出前面的例子，但是这次在声明我们的`doubleAge`变量时使用 const 而不是 Let。

```
var age = 27
If (age) {
 const doubleAge = age + age
 console.log(`Double your current age is ${yearPlusTwenty}`)
}
```

现在，在你的控制台中输入`doubleAge`并回车。你应该得到一个错误，`doubleAge is not defined.`这是因为 const 是块范围的:*它只存在于它被定义的块中。*

`doubleAge`变量被“困”在定义它的两个花括号里。那些括号内的代码可以访问 doubleAge，但是括号外的代码不能。

通过使用`const`而不是`var`，我们之前的问题得到了解决。我们的`doubleAge`风险值不再不必要地“泄露”到我们的全球范围内。相反，它只存在于创建它的块中。

块范围的变量如何在函数的上下文中工作？为了了解这一点，让我们创建并调用一个函数，`returnX.`

```
function returnX() {
 const x = 1
 return x
}
returnX()
```

通过调用这个函数`returnX`，我们可以看到我们的函数返回 x 的值，也就是 1。

如果我们接下来输入`x`，我们将返回`referenceError: x is not defined`。这是因为函数也被认为是块，所以我们的常量`x`只存在于函数中。

关于 const，接下来要知道的是它只能声明一次。在您的控制台中键入以下代码:

```
const y = 1
const y = 2
```

您应该会看到一个错误，`Identifier 'x' has already been declared.`

这是 var 和 const 的区别。虽然 const 会给你一个错误，让你知道你已经声明了这个变量，但 var 关键字不会。

```
var x = 1
var x = 2
```

变量`x`将正确无误地指向`2`的值。作为一名程序员，这可能会给你带来错误，因为你可能并不想把你的值重新赋给一个新的变量。因此，使用 const 可以帮助你，因为如果你不小心试图重新分配一个变量，你会收到一个错误。

这是作为在 JavaScript 中创建变量的更新和更好的方式引入的`const`关键字的优势。然而，当你*想要更新你的变量的时候呢？*

*让我们看一个例子来说明为什么我们要这样做。*

*让我们声明一个变量`adult`，并将其设置为`false`。我们还将创建一个`age`变量并将其设置为`20`。*

*`const adult = false`*

*`const age = 20.`*

*假设我们想要检查一个用户的年龄，如果年龄超过 18 岁，就将我们的成人变量设置为 false。我们可以写一个 if 语句来做这件事。*

```
*`if (age > 18) {
 adult = true   
}`*
```

*当我们运行这段代码时会发生什么？*

*在这里我们将看到`Error: Assignment to constant variable.`*

*这是因为，按照`const`的规则，我们不能重新声明这个变量。也就是说，我们的变量`age`已经指向 true 的值，我们现在不能将它指向其他的值。*

*如果我们再次打印出`adult`，我们可以看到它保持不变，仍然保存着`false`的值。*

*我们不能重新分配我们的`age`变量，而`const`正在按预期工作。然而，如果我们*想让*重新分配这个变量呢？*

*程序员经常希望能够重新声明他们的变量。*

*这就是我们的第三个关键字，let，出现的地方。*

## *如何在 JavaScript 中使用 let 关键字*

*首先让我们回顾一下`let`和`const`是如何相似的。*

*`Let`和`const`一样，是块范围的。如果在我们上面的`doubleAge`例子中用 let 替换 const，效果是一样的。*

*然而，`let`与`const`在根本上是不同的。用`let`关键字声明的变量可以重新声明，而用`const`关键字创建的变量则不能。让我们看一个例子。*

*使用我们上面的例子，用 let 替换 const。我们将把年龄变量保存为一个值为`20`的`const`。*

```
*`let adult = false
const age = 20
if (age > 18) {
    adult = true
}`*
```

*现在，如果我们键入`adult`，我们将看到`true`的输出，而不是像以前那样得到一个错误。*

*通过使用`let`关键字，我们已经更新了我们的变量，使其指向我们想要的`true`的值。有时在编程中，我们会希望根据收到的某些数据来更新变量。我们可以使用`let`来做到这一点。*

## *包扎*

*总的来说，我们已经知道变量在我们的计算机程序中是用来跟踪和重用数据的。作用域指的是在我们的代码中变量可以使用的地方。*

*可以使用 var、const 或 let 声明变量。Var 是函数范围的，而 const 和 let 是块范围的。Const 变量不能被重新赋值，而 let 变量可以。*

*Var、const 和 let 一开始可能会令人困惑。它可以帮助你阅读不同的教程，也可以用不同的方式测试你自己的代码来巩固你的理解。*

*拥有坚实的 var、const 和 let 基础不仅在 JavaScript 职业生涯的开始阶段，而且在整个过程中都将对你有所帮助。*

### *感谢您的阅读！*

*如果你喜欢这篇文章，请注册[我的电子邮件列表](https://madisonkanna.us14.list-manage.com/subscribe/post?u=323fd92759e9e0b8d4083d008&id=033dfeb98f)，在那里我会发送我的最新文章，并宣布我的编码图书俱乐部的会议。*

*如果你对这篇文章有任何反馈或问题，欢迎发微博给我@ [madisonkanna。](https://twitter.com/Madisonkanna)*