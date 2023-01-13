# 如何在 JavaScript 中声明数组——在 JS 中创建数组

> 原文：<https://www.freecodecamp.org/news/how-to-declare-an-array-in-javascript-creating-an-array-in-js/>

在 JavaScript 中，数组是最常用的数据类型之一。它在一个变量中存储多个值和元素。

这些值可以是任何数据类型，这意味着您可以在一个变量中存储字符串、数字、布尔值和其他数据类型。

在 JavaScript 中有两种标准的方法来声明数组。这些要么通过数组构造函数，要么通过文字符号。

如果你很急，下面是两种方式声明的数组的样子:

```
// Using array constructor
let array = new array("John Doe", 24, true);

// Using the literal notation
let array = ["John Doe", 24, true]; 
```

您可以继续阅读本文，以正确理解这些方法以及它们拥有的其他一些有趣的选项。

## 如何用文字符号声明数组

这是创建数组最流行和最简单的方法。这是一种更短、更简洁的声明数组的方式。

要用文字符号声明一个数组，你只需要用空括号定义一个新数组。看起来是这样的:

```
let myArray = []; 
```

您将把所有元素放在方括号中，并用逗号分隔每个项目或元素。

```
let myArray = ["John Doe", 24, true]; 
```

数组是零索引的，这意味着您可以从零开始访问每个元素或输出整个数组。

```
console.log(myArray[0]); // 'John Doe'
console.log(myArray[2]); // true
console.log(myArray); // ['John Doe', 24, true] 
```

## 如何用数组构造函数声明数组

您还可以使用数组构造函数来创建或声明数组。用`Array()`构造函数声明数组有很多技术细节。

正如您可以使用数组文字表示法在一个变量中存储具有不同数据类型的多个值一样，您也可以使用数组构造函数进行同样的操作。

```
let myArray = new Array();
console.log(myArray); // [] 
```

以上将创建一个新的空数组。您可以将值添加到新数组中，方法是将它们放在括号中，用逗号分隔。

```
let myArray = new Array("John Doe", 24, true); 
```

正如您之前所学的，您可以使用索引号来访问每个值，索引号从零(0)开始。

```
console.log(myArray[0]); // 'John Doe'
console.log(myArray[2]); // true
console.log(myArray); // ['John Doe', 24, true] 
```

当使用数组构造函数方法声明数组时，记住以下几点很重要。

*   当您将一个数字传递给数组构造函数时，它将用您输入的空值的数量填充数组。

```
let myArray = new Array(4);
console.log(myArray); // [,,,] 
```

但是当您传递单个字符串或任何其他数据类型时，它工作得很好。

```
let myArray = new Array(true);
console.log(myArray); // [true] 
```

*   添加`new`不是强制性的，因为`Array()`和`new Array()`执行相同的任务。

```
let myArray = Array("John Doe", 24, true); 
```

## 结束了！

在本文中，您已经学习了如何用 JavaScript 声明数组。重要的是要知道数组构造函数并没有真正被使用，因为它比使用数组文字符号要复杂得多。

你可以在这篇关于 [JavaScript 数组的文章中了解更多——如何在 JavaScript](https://www.freecodecamp.org/news/how-to-create-an-array-in-javascript/) 中创建一个数组，作者[杰西卡·威尔金斯](https://www.freecodecamp.org/news/author/jessica-wilkins/)

祝编码愉快！