# JavaScript For Loop——如何在 JS 中遍历一个数组

> 原文：<https://www.freecodecamp.org/news/how-to-loop-through-an-array-with-for-loop-in-javascript/>

循环是计算机编程中的一条指令，它允许应用程序重复一个过程，直到满足特定的条件。

例如，假设您想要遍历一个姓名列表，并在屏幕上输出每个姓名。如果不使用循环，最终会重复代码来输出每个名字。但是通过一个循环，您可以单独输出每个名字，而无需重复任何代码。

本文将带您了解如何使用 JavaScript 中五种最常见的循环遍历数组。

## **JavaScript 中什么是数组？**

JavaScript 中的数组是一个变量，可以用来存储多个元素。它通常以方括号的形式包含不同的元素、字符串和整数(或者既包含字符串又包含整数)。

## **JavaScript 中什么是循环？**

循环是一个条件语句，用于运行一系列指令，直到满足指定的条件。给它一个语句，该语句重复执行一个代码块，并在满足规定的条件时结束执行。

让我们使用 JavaScript 中最常用的一个循环来遍历这个数组-`let name = ['Dennis', 'Precious', 'Evelyn']`，。

## ****JavaScript 中如何用 For 循环遍历数组****

`for`循环是一个语句，当条件不满足时重复执行一段代码，当条件满足时终止执行。

现在让我们使用`for` loop 方法遍历一个数组。

我建议你仔细阅读这篇文章，不要错过重要的细节。但是如果您急着使用`for` loop 语句遍历数组，您可以查看下面的语法。

### ****快速作循环解释:****

这是使用带有示例的`for` loop 语句遍历数组的快捷方式。

语法:

```
let name = ['Dennis', 'Precious', 'Evelyn'];
for (let i = 0; i < name.length; i++)
 {    
     console.log(name[i]);
}
```

这是输出结果:

```
Dennis
Precious
Evelyn
```

### ****循环是如何工作的——更详细的****

这里我们将通过一个例子来理解什么是`for`循环以及如何使用这个语句来循环遍历一个数组。

要运行一个`for`循环语句，你必须知道它总是需要有三个主表达式，并且总是用分号隔开。

语法:

```
for (expression1; expression2; expression3)
 {  
  //code to be executed
}
```

*   expression1:这是`initialization`表达式，您可以用`var`或`let`关键字来声明一个`for`循环计数器变量，比如`var i = 0`或`let i = 0`。
*   表达式 2:这是`condition`表达式，返回 true 或 false。它决定了`for`循环应该继续执行还是结束执行。

记住`for`循环语句在条件不满足时继续执行代码块，只有在条件满足时才终止循环。

*   表达式 3:这是`update`表达式，每当满足`for`循环条件时，通常用于递增(`++i`)或递减(`--i`)循环计数器变量。

现在我们已经理解了`for`循环语句表达式，让我们来看一个例子。

语法:

```
const arrayName = ['Dennis', 'Precious', 'Evelyn']
for (let i = 0; i < arrayName.length; i++) {
	console.log(arrayName[i]);
}
```

只要不满足条件，上面的语法就会遍历`name`数组和`console.log ()`数组中的字符串。

根据上面的语法:

*   我们首先初始化计数器变量`let i = 0;`。
*   然后我们给循环一个条件，一旦计数器变量(`i`)的值小于(`<`)名称(`name.length;`)的长度，就终止循环。
*   您使用关键字`length`来检查字符串的长度。

以下是输出结果:

```
Dennis
Precious
Evelyn
```

## ****结论****

在本教程中，我们讨论了 JavaScript 中 for 循环的基本原理以及数组的定义。我们还学习了如何使用 JavaScript 的“for”循环语句来遍历数组。

感谢阅读。