# JavaScript 大写——如何用？toUpperCase

> 原文：<https://www.freecodecamp.org/news/javascript-uppercase-how-to-capitalize-a-string-in-js-with-touppercase/>

在 JavaScript 中处理字符串时，您可以对它们执行不同的操作。

您可能对字符串执行的操作包括大写、转换为小写、在单词中添加符号等等。

在本文中，我将向您展示如何使用`.toUpperCase()` string 方法将字符串转换成大写字母。

## `.toUpperCase()`方法的基本语法

要使用`.toUpperCase()`方法，将您想要改为大写的字符串赋给一个变量，然后在它前面加上`.toUpperCase()`。

## 如何用？toUpperCase

如前所述，您可以将一个字符串赋给一个变量，然后使用`.toUpperCase()`方法将其大写

```
const name = "freeCodeCamp";
const uppercase = name.toUpperCase();
console.log(uppercase);

// Output: FREECODECAMP 
```

您也可以编写一个函数并在其中返回`.toUpperCase()`，这样当调用该函数时，一个声明的参数将被大写。

```
function changeToUpperCase(founder) {
  return founder.toUpperCase();
}

// calling the function 
const result = changeToUpperCase("Quincy Larson");

// printing the result to the console
console.log(result);

// Output: QUINCY LARSON 
```

**在上面的脚本中:**

*   我用占位符`founder`定义了一个名为`changeToUpperCase`的函数
*   通过函数中的 return 语句，我告诉函数我想让它做的是将我调用它时指定的任何参数都改为大写字母
*   然后，我将函数调用- `changeToUpperCase`分配给一个名为`result`的变量
*   在变量的帮助下，我能够将函数的结果打印到控制台

## 结论

当需要在 JavaScript 项目中大写字符串时，可以使用`.toUpperCase()`方法，全称是`String.prototype.toUpperCase()`。

如果你觉得这篇文章有帮助，请分享给你的朋友和家人。