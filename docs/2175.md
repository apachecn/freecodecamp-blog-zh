# JavaScript 是什么？JS 编程语言的定义

> 原文：<https://www.freecodecamp.org/news/what-is-javascript-definition-of-js/>

JavaScript 是一种动态编程语言，用于 web 开发、web 应用程序、游戏开发等等。它允许你在网页上实现动态特性，而这仅仅用 HTML 和 CSS 是做不到的。

许多浏览器使用 JavaScript 作为在 web 上做动态事情的脚本语言。每当你看到一个点击显示的下拉菜单、添加到页面的额外内容、动态改变页面上的元素颜色等等，你就会看到 JavaScript 的效果。

## 如果没有 JavaScript，网络会是什么样子？

如果没有 JavaScript，你在网上只能看到 HTML 和 CSS。仅仅这些就将您限制在少数几个网页实现中。你的网页的 90%(如果不是更多的话)将是静态的，你只有像 CSS 提供的动画一样的动态变化。

## JavaScript 如何让事情变得动态

HTML 定义了 web 文档的结构和其中的内容。CSS 为 web 文档上提供的内容声明了各种样式。

HTML 和 CSS 通常被称为标记语言，而不是编程语言，因为它们的核心是为文档提供几乎没有动态的标记。

另一方面，JavaScript 是一种动态编程语言，支持数学计算，允许您动态地将 HTML 内容添加到 [DOM](https://thewebfor5.com/p/javascript/the-dom/) ，创建动态样式声明，从另一个网站获取内容，等等。

在我们深入研究 JavaScript 如何做所有这些事情之前，让我们先看一个简单的例子。

看看这支笔:[https://codepen.io/Dillion/full/XWjvdMG](https://codepen.io/Dillion/full/XWjvdMG)

在 codepen 中，您会看到当您在输入字段中键入内容时，文本会显示在屏幕上。JavaScript 使这成为可能。你不能用 HTML，CSS，或者两者一起得到这个。

JavaScript 能做的远不止我在本文中能介绍的。但是为了让您开始使用 JS，我们来看看:

*   如何在 HTML 中使用 JavaScript
*   数据类型
*   变量
*   评论
*   功能

## 如何在 HTML 中使用 JavaScript

就像 CSS 一样，JavaScript 可以以多种方式在 HTML 中使用，例如:

### 1.内联 JavaScript

这里，在一些特殊的基于 JS 的属性中的 HTML 标记中有 JavaScript 代码。

例如，HTML 标签有[事件属性](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Event_handlers)，允许你在事件被触发时执行一些代码。我的意思是:

```
<button onclick="alert('You just clicked a button')">Click me!</button> 
```

这是内联 JavaScript 的一个例子。`onclick`的值可以是一些匹配计算，是对 DOM 的动态添加——任何语法有效的 JavaScript 代码。

### 2.内部 JavaScript，带有`script`标签

就像 HTML 页面中用于样式声明的`style`标签一样，`script`标签也适用于 JavaScript。下面是它的使用方法:

```
<script>
	function(){
	    alert("I am inside a script tag")
	}
</script> 
```

### 3.外部 JavaScript

您可能希望将 JavaScript 代码放在不同的文件中。外部 JavaScript 允许这样做。对于这样的用例，下面是如何做到的:

```
<!-- index.html -->
<script src="./script.js"></script> 
```

```
// script.js
alert("I am inside an external file"); 
```

`script`标签的`src`属性允许您为 JavaScript 代码应用一个源代码。这个引用很重要，因为它通知浏览器也获取`script.js`的内容。

`script.js`可以和`index.html`在同一个目录，也可以从其他网站获取。对于后者，您需要传递完整的 URL ( `https://.../script.js`)。

注意到`.js`扩展了吗？那是 JavaScript 文件的扩展，就像 HTML 有`.html`一样。

既然我们已经研究了将 JavaScript 应用到 HTML 的方法，那么让我们来看看 JavaScript 的一些特性。

## JavaScript 中的数据类型

在 JavaScript 中，数据必须是某种类型。JavaScript 需要知道这一点，以便知道如何将它用于其他数据，或者如何对这些数据进行操作。

以下是 JavaScript 支持的基本数据类型:

*   数字(例如，`6`、`7`、`8.9`):您可以对其应用算术运算(如加法)和许多其他运算
*   字符串(如`"javascript"`、`'a long sentence'`、`a short paragraph`):单引号(`'...'`)、双引号(`"..."`)和反斜线(`...`)之间的任何内容。单引号和双引号没有区别，但是反引号有更多的功能，例如:
    *   在字符串中插值变量，像这样:`My name is ${name}`。`name`这里是一个变量，注入到字符串中。
    *   多行字符串。对于普通的引号，您需要添加像`\n`这样的转义字符来换行，但是反勾号允许您在另一行上继续您的字符串，就像这样:

```
let str = `I am a
    multiline string`; 
```

*   布尔型(只能有两个值:`true`或`false`):更像是 yes ( `true`或 no ( `false`)
*   数组(例如，`[1, 2, "hello", false]`):一组数据(可以是任何类型，包括数组)，用逗号分隔。索引从 0 开始。访问这样一个组的内容可以是这样的:`array[0]`，在这个例子中它将返回`1`，因为它是第一项。
*   对象(例如`{name: 'javascript', age: 5}`):也是一组数据，但采用`key:value`对的形式。`key`必须是一个字符串，值可以是任何类型，包括另一个对象。访问群组内容是通过按键完成的，例如`obj.age`或`obj["age"]`将返回`5.`
*   Undefined(该类型支持的唯一数据是`undefined`):该数据可以显式地赋给一个变量，或者隐式地(通过 JavaScript)赋给一个已经声明但没有赋值的变量。在本文的后面，我们将研究变量声明和值赋值。
*   Null(该类型支持的唯一数据是`null` ): Null 表示没有值。它包含一个值，但不是一个真正的值，而是 null。
*   函数(例如，`function(){ console.log("function") }`):函数是一种数据类型，它在被调用时调用一段代码。本文稍后将详细介绍函数。

JavaScript 数据类型理解起来可能有点复杂。你可能听说过数组和函数也是对象，确实如此。

理解这一点需要理解 JavaScript 原型的本质。但是，在基本层面上，这些是您在 JavaScript 中首先需要了解的数据类型。

## JavaScript 中的变量

变量是任何数据类型的值的容器。它们保存值，因此当使用变量时，JavaScript 使用它们代表的值进行操作。

变量可以声明，也可以赋值。当你声明一个变量时，你是这样做的:

```
let name; 
```

对于上面的，`name`已经声明了，但是还没有值。

正如您在数据类型部分所期望的，JavaScript 自动将值`undefined`赋给`name`。因此，如果您试图在任何地方使用`name`，那么`undefined`将被用于该操作。

下面是给变量赋值的含义:

```
let name;
name = "JavaScript"; 
```

现在如果用`name`，就代表`JavaScript`。

声明和赋值可以在一行中完成，如下所示:

```
let name = "JavaScript"; 
```

为什么是`let`？你可能问过自己，原因如下:JavaScript 支持三种变量声明方法，分别是:

*   操作符:自从 JavaScript 出现以来，它就一直存在。您可以声明变量并给它们赋值，这些值稍后可以在代码中更改。我的意思是:

```
var name = "JavaScript";
name = "Language"; 
```

*   `let`操作符:这也与`var`非常相似——它声明变量并为变量赋值，这些变量可以在代码中稍后更改。这些操作员之间的主要区别是`var`提升这些变量，而`let`不提升。提升的概念可以用下面的代码来简单解释:

```
function print() {
	console.log(name);
	console.log(age);
	var name = "JavaScript";
	let age = 5;
}

print(); 
```

调用`print`函数(`print()`)时，第一个`console.log`打印`undefined`，而第二个`console.log`抛出错误“初始化前无法访问`age`”。

这是因为`name`变量的声明被提升到函数的顶部，变量的赋值停留在同一行，而`age`的声明和赋值停留在同一行。

下面是上面的代码是如何编译的:

```
function print() {
	var name;
	console.log(name);
	console.log(age);
	name = "JavaScript";
	let age = 5;
}

print(); 
```

提升问题可能会意外发生，这就是为什么你应该使用`let`而不是`var`。

*   操作符:这也不提升变量，但是它做了另外一件事:它确保一个变量除了在初始化时被赋值之外，不能被赋予另一个值。

例如:

```
let name = "JavaScript"
name = "Language" // no errors

const age = 5
age = 6 // error, cannot reassign variable 
```

## JavaScript 中的注释

就像 HTML 一样，有时我们可能想在不需要执行的代码旁边添加注释。

在 JavaScript 中，我们可以通过两种方式做到这一点:

*   用单行注释，像这样:`// a single line comment`
*   或者用多行注释，像这样:

```
/*
a multi
line comment
*/ 
```

## JavaScript 中的函数

通过函数，您可以存储一段代码，这段代码可以在代码的其他地方使用。假设您想在代码的不同位置打印“JavaScript”和“Language”。不要这样做:

```
console.log("JavaScript")
console.log("Language")

// some things here

console.log("JavaScript")
console.log("Language")

// more things here

console.log("JavaScript")
console.log("Language") 
```

您可以这样做:

```
function print() {
    console.log("JavaScript")
    console.log("Language")
}

print()

// some things here

print()

// more things here

print() 
```

通过这种方式，我们将重复的代码块存储在一个函数中，这个函数可以在我们需要的任何地方使用。但这还不是全部。假设我们想求三个数的平均值。这方面的代码应该是:

```
let num1 = 5
let num2 = 6
let num3 = 8
let average = (num1 + num2 + num3) / 3 
```

在函数之外这样做可能没什么坏处，但是如果我们不得不在很多地方这样做呢？然后，我们会有一个这样的函数:

```
function findAverage(n1, n2, n3) {
    let aver = (n1 + n2 + n3) / 3
    return aver
}

let num1 = 5
let num2 = 6
let num3 = 8
let average = findAverage(num1, num2, num3)

// later on, somewhere else
let average2 = findAverage(...)

// later on, somewhere else
let average3 = findAverage(...) 
```

你会注意到在`findAverage`的声明中，我们在括号中有`n1, n2, n3`。这些是参数，它们充当函数被调用时提供的值的**占位符**。

代码块使用这些占位符来寻找平均值，关键字`return`从函数中返回平均值。

占位符使您的函数可重用，这样不同时间的不同值可以传递给函数，以使用相同的逻辑。

## 结论

JavaScript 还有很多我们可以讨论的特性，但是我希望这篇文章已经为您提供了一个清晰的起点，让您可以更进一步。现在你应该知道什么是语言，以及如何在网络上使用它。

在本文中，我们研究了如何将 JavaScript 代码添加到 HTML 文件中，JavaScript 支持的不同类型的数据，充当值容器的变量，如何用 JavaScript 编写注释，以及如何声明和使用函数。

从这里有很多地方可以去，但是我建议接下来学习一下 DOM 以及 JavaScript 如何与它交互。