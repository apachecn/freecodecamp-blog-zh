# JavaScript 中的字符串到数字——在 JS 中将字符串转换为 Int

> 原文：<https://www.freecodecamp.org/news/string-to-number-in-javascript-convert-a-string-to-an-int-in-js/>

当你编程时，你经常需要在数据类型之间切换。例如，您可能需要将字符串转换为数字。

在处理信息时，将一种数据类型转换为另一种数据类型的能力为您提供了极大的灵活性。

JavaScript 有不同的内置方法将字符串转换或造型为数字。

在本文中，您将学习一些用于将字符串转换为数字的内置 JavaScript 方法，以及介绍(或复习！)到 JavaScript 中字符串和数字的基本工作原理。

以下是我们将要介绍的内容:

1.  [JavaScript 中的字符串是什么？](#string-js)
2.  [JavaScript 中的数字是什么？](#number-js)
3.  [如何在 JavaScript 中检查一个值的数据类型？](#typeof)
4.  [如何使用`parseInt()`函数](#parseint)将字符串转换成数字
5.  [如何使用`parseFloat()`函数](#parsefloat)将字符串转换成数字
6.  [如何使用`Number()`函数](#Number)将字符串转换成数字
7.  [如何使用`Math`函数将字符串转换成数字](#math-functions)
8.  [如何通过`1`](#multiplication) 将一个字符串乘、除转换成一个数字
9.  [如何使用一元运算符`+`将字符串转换成数字](#unary)

## JavaScript 中的字符串是什么？

字符串是通过文本进行通信的有效方式，例如存储和操作文本。它们是所有编程语言中最基本的数据类型之一。

JavaScript 中的字符串是一种原始数据类型。这意味着它们是默认内置在语言中的。

字符串是零个或多个字符值的有序序列。具体来说，它是一个或多个字符的序列，可以是字母、数字或符号(如标点符号)。

通常，如果数据值用引号(如单引号或双引号)括起来，就可以判断它是否是字符串。

具体来说，有三种方法可以在 JavaScript 中创建字符串:

*   使用单引号。
*   通过使用双引号。
*   通过使用反勾号。

以下是如何使用单引号创建字符串:

```
// string created using single quotes ('')
let favePhrase = 'Hello World!'; 
```

以下是如何使用双引号创建字符串:

```
// string created using double quotes ("")
let favePhrase = "Hello World!"; 
```

下面是如何使用反勾号创建字符串:

```
// string created using backticks (``)
let favePhrase = `Hello World!`; 
```

在 JavaScript 中创建字符串的最后一种方法也称为模板文字。

## JavaScript 中的数字是什么？

数字让您可以表示数值，并执行数学运算和计算。

JavaScript 中的数字是一种原始数据类型——就像字符串一样。

与其他编程语言不同，您不需要指定要创建的数字类型。例如，您不需要说明数字是整数还是浮点数。

在 JavaScript 中，语言内置了几种不同类型的数字(正数和负数):

*   整数。整数是不包括小数部分的数值，也称为整数。
*   彩车。浮点数是一个带小数点的数字，小数点后至少有一个数字。
*   指数可以是整数或浮点数，后面跟一个`e`。`e`表示将一个数乘以`10`的给定幂。
*   二进制数(也称为基数为 2 的数)。二进制是仅由两个数字组成的数字系统:`0`和`1`。它用 8 位代表一个字节。该数字以一个`0`开头，后面是一个`b`，再后面是一个 8 位数字。
*   八进制数(也称为八进制数)。一个八进制数以一个`0`开始，后面是范围从`0 - 7`的八进制数字。
*   十六进制数(也称为 16 进制数)。一个十六进制数以一个`0`开头，后跟一个`x`或`X`。之后，可以有范围从`0 - 9`的十六进制数字和范围从`A - F`(或`a - f`)的字母组合。字母`A - F`与数值`10 -15`相关联。

```
// integer
let num = 47;

// float
let num = 47.32;

// exponential - to represent large numbers
let num = 477e2;  // equal to multiplying 477 to 10 to the power of 2 (or 100) which results in 47700

// exponential - to represent small numbers
let num = 477e-2;  // equal to dividing 477 to 10 to the power of 2 (or 100) which results in 4.77

// binary
let num = 0b1111;    // stands for 15

// octal
let num = 023; // stands for 19

// hexadecimal
let num = 0xFF; // stands for 255 
```

需要注意的是数字*不应该用引号括起来——这会自动使它们成为一个字符串。*

```
// this is a string not a number!
let num = '7'; 
```

## 如何在 JavaScript 中检查一个值的数据类型？

为了避免任何错误并在 JavaScript 中仔细检查值的数据类型，请使用`typeof`操作符。

前面我提到过引号中的数字是字符串。

您可以通过执行以下操作自行检查:

```
let num = '7';
console.log(typeof num)

// string 
```

## 如何使用`parseInt()`函数将字符串转换成数字

`parseInt()`函数的一般语法如下:

```
parseInt(string, radix) 
```

`parseInt()`函数接受两个参数:一个字符串作为第一个参数，一个基数作为第二个可选参数。

字符串是需要转换成数字的值。

基数指定您要使用的数学数字系统以及将返回的数字的基数，无论该数字是基数 2(或二进制)、基数 8(或八进制)、基数 10(十进制)还是基数 16(或十六进制)。

如果不包括基数，则默认为`10`(十进制值)。

```
let num = '7';

let strToNum = parseInt(num, 10);

console.log(strToNum);
console.log(typeof strToNum);

// 7
// number 
```

如果字符串包含字母和数字呢？它将只返回字符串中的数字:

```
let num = '7 cats 7';

let strToNum = parseInt(num, 10);

console.log(strToNum);
console.log(typeof strToNum);

// 7
// number 
```

当`parseInt()`遇到一个非数字字符时，它会忽略该字符以及其后的所有字符，即使后面还有更多数字。

需要记住的是，如果字符串*不是*以数字开头，那么将返回`NaN`(T1 的简称)。

```
let num = 'h7';

let strToNum = parseInt(num, 10);

console.log(strToNum);

// NaN 
```

`parseInt()`函数将从字符串的位置`0`开始，确定该位置的字符是否可以转换成数字。如果不能，函数返回`NaN`,即使字符串后面包含数字。

如果你有一个包含浮点数的字符串呢？`parseInt()`函数会将其四舍五入，并返回一个整数:

```
let num = '7.77';

let strToNum = parseInt(num, 10);

console.log(strToNum);

// returns 7 instead of 7.77 
```

如果是这种情况，并且您想要执行文字转换，最好使用`parseFloat()`函数来代替。

## 如何使用`parseFloat()`函数将字符串转换成数字

`parseFloat()`函数的一般语法如下:

```
parseFloat(string) 
```

`parseFloat()`函数的语法和行为与`parseInt()`函数相似。主要区别是`parseFloat()`只接受一个参数，不接受基数作为参数。

`parseFloat()`函数接受一个字符串作为它唯一的参数，并返回一个浮点数——一个带小数点的数字。

当您想要保留数字的小数部分而不仅仅是整数部分时，使用`parseFloat()`函数。

以上一节中的同一个例子为例，下面是如何使用`parseFloat()`重写它:

```
let num = '7.77';

let strToNum = parseFloat(num);

console.log(strToNum);

// 7.77 
```

与`parseInt()`一样，`parseFloat()`函数将只返回第一个数字，并忽略任何非数字字符:

```
let num = '7.77 cats 7.77';

let strToNum = parseFloat(num);

console.log(strToNum);

// 7.77 
```

就像`parseInt()`一样，如果第一个字符不是一个有效的数字，`parseFloat()`函数将返回`NaN`而不是一个数字，因为它不能将它转换成一个数字:

```
let num = 'h7.77';

let strToNum = parseFloat(num);

console.log(strToNum);

// NaN 
```

## 如何使用`Number()`函数将字符串转换成数字

`Number()`函数的一般语法如下:

```
Number(string) 
```

`Number()`函数与`parseInt()`和`parseFloat()`函数的区别在于，`Number()`函数试图将整个字符串一次转换成一个数字。解析方法将一个字符串一段一段地转换成一个数字，它们逐个地、一次一个地遍历组成字符串的字符。

让我们举一个你之前看到的使用`parseInt()`的例子:

```
let num = '7 cats 7';

let strToNum = parseInt(num, 10);

console.log(strToNum);

// 7 
```

当`parseInt()`遇到非数字字符时，它就结束转换。

下面是同一个例子如何使用`Number()`函数:

```
let num = '7 cats 7';

let strToNum = Number(num);

console.log(strToNum);

// NaN 
```

由于`Number()`试图一次将整个字符串转换并定型为一个数字，所以它返回`NaN`,因为它遇到了非数字字符，因此无法转换为数字。

如果字符串包含非数字字符，当您希望转换失败时，`Number()`函数是一个很好的选择。

另外需要注意的是，与您之前看到的`parseInt()`函数相比，`Number()`函数在遇到小数时不会返回整数。

```
let num = '7.77';

let strToNum = Number(num);

console.log(strToNum);

// 7.77 
```

## 如何使用`Math`函数将字符串转换成数字

`Math`对象是一个内置的 JavaScript 对象。您可以使用它的一些方法，比如`Math.round()`、`Math.floor()`和`Math.ceil()`，将字符串转换成数字。

在使用数学方法进行类型转换时，需要注意和记住的一点是，当你处理浮点数时，它们会将它们转换成整数，而浮点数会丢失小数部分。

这些方法会将字符串转换为最接近的等效整数。

`Math.round()`函数将字符串转换为数字，并将其四舍五入为最接近的整数:

```
let num = '7.5';

let strToNum = Math.round(num);

console.log(strToNum);

// 8 
```

如果`num`的值等于`7.4`，我会得到如下结果:

```
let num = '7.4';

let strToNum = Math.round(num);

console.log(strToNum);

// 7 
```

如果字符串包含非数字字符，`Math.round()`返回`NaN`。

```
let num = '7.5a';

let strToNum = Math.round(num);

console.log(strToNum);

// NaN 
```

`Math.floor()`函数将一个字符串转换成一个数字，并将其向下*舍入到最接近的整数:*

```
let num = '7.87';

let strToNum = Math.floor(num);

console.log(strToNum);

// 7 
```

如果字符串包含非数字字符，`Math.floor()`返回`NaN`。该函数的工作方式是尝试将整个字符串转换为数字，然后计算结果，这意味着该字符串必须是有效的字符串才能工作:

```
let num = '7.87a';

let strToNum = Math.floor(num);

console.log(strToNum);

// NaN 
```

`Math.ceil()`函数与`Math.floor()`相反，因为它将字符串转换为数字，并将其*向上*舍入为最接近的整数:

```
let num = '7.87';

let strToNum = Math.ceil(num);

console.log(strToNum);

// 8 
```

与前面的例子类似，`Math.ceil()`函数在遇到字符串中的非数字值时将返回`NaN`:

```
let num = '7.87a';

let strToNum = Math.ceil(num);

console.log(strToNum);

// NaN 
```

## 如何用`1` 乘除将字符串转换成数字

乘以`1`是将字符串转换为数字的最快方法之一:

```
let convertStringToInt = "7" * 1;

console.log(convertStringToInt);
console.log(typeof convertStringToInt);

// 7
// number 
```

如果你想在一个浮点数上执行类型转换，乘以`1`保留小数位:

```
let convertStringToInt = "7.1" * 1;

console.log(convertStringToInt);
console.log(typeof convertStringToInt);

// 7.1
// number 
```

如果字符串包含非数字字符，将返回`NaN`:

```
let convertStringToInt = "7a" * 1;

console.log(convertStringToInt);

// NaN 
```

这种将字符串转换成整数的方式也适用于将字符串除以`1`:

```
let convertStringToInt = "7" / 1

console.log(convertStringToInt);
console.log(typeof(convertStringToInt));

// 7
// number 
```

在这一点上，还值得一提的是，当您试图将`1`添加到字符串以将其转换为整数时会发生什么。如果你尝试这样做，你会得到这样的结果:

```
let convertStringToInt = "7" + 1;

console.log(convertStringToInt);
console.log(typeof convertStringToInt);

// 71
// string 
```

在上面的例子中，`1`与字符串`"7"`连接在一起，这意味着它与字符串并排放置。

## 如何使用一元运算符`+`将字符串转换成数字

使用一元运算符`+`也是将字符串转换成数字的最快方法之一。

您将加号运算符`+`放在字符串前面，它将字符串转换为整数:

```
let convertStringToInt = +"7";

console.log(convertStringToInt);
console.log(typeof convertStringToInt);

// 7
// number 
```

..或者一个浮动:

```
let convertStringToInt = +"7.77";

console.log(convertStringToInt);

// 7.77 
```

与您看到的将字符串转换为数字的其他方法类似，整个字符串必须只包含数字字符，一元运算符`+`才能起作用。如果字符串不代表数字，那么它将返回`NaN`:

```
let convertStringToInt = +"7a";

console.log(convertStringToInt);

// NaN 
```

## 结论

现在你知道了！现在，您已经知道了在 JavaScript 中将字符串转换为数字的一些方法。

要了解更多关于 JavaScript 的知识，请访问 freeCodeCamp 的 [JavaScript 算法和数据结构认证](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)。

这是一个免费的、经过深思熟虑的、结构化的课程，你可以在其中进行互动学习。最后，您还将构建 5 个项目来获得您的认证并巩固您的知识。

感谢阅读！*