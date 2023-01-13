# JavaScript Number to String——如何使用 to String 将 Int 转换成 String

> 原文：<https://www.freecodecamp.org/news/javascript-number-to-string-how-to-use-tostring-to-convert-an-int-into-a-string/>

`toString()`方法是 JavaScript `Number`对象的内置方法，允许您将任何`number`类型值转换为其`string`类型表示。

## 如何在 JavaScript 中使用 toString 方法

要使用`toString()`方法，只需对一个`number`值调用该方法。下面的例子展示了如何将数字值`24`转换成它的字符串表示。注意`str`变量的值是如何用双引号括起来的:

```
var num = 24;
var str = num.toString();

console.log(num); // 24
console.log(str); // "24"
```

Convert a number to string with toString() method

您也可以对一个`number`值立即调用`toString()`方法，但是您需要添加圆括号`()`来将该值括起来，否则 JavaScript 将响应一个`Invalid or unexpected token`错误。

`toString()`方法也可以转换浮点数和负数，如下所示:

```
24.toString(); // Error: Invalid or unexpected token
(24).toString(); // "24"
(9.7).toString(); // "9.7"
(-20).toString(); // "-20"
```

Convert any type of numbers with toString() method

最后，`toString()`方法也接受`radix`或`base`参数。`radix`允许您将十进制数字系统(基数为 10)中的数字转换为代表其他数字系统中的数字的字符串。

用于转换的有效数字系统包括:

*   有两个数字 0 和 1 的二进制系统(基数为 2)
*   有 3 个数字 0、1 和 2 的三进制系统(基数为 3)
*   四进制(基数为 4)，有 4 个数字，0、1、2 和 3
*   以此类推，直到十六进制(基数为 36)，该系统由阿拉伯数字 0 到 9 和拉丁字母 A 到 Z 组合而成

```
Number.toString(radix);
```

The syntax for toString() method, accepting radix parameter

`radix`参数接受值范围从`2`到`36`的`number`类型数据。下面是一个将十进制数`5`转换成二进制数(以 2 为基数)的例子:

```
var str = (5).toString(2);

console.log(str); // "101"
```

Converting decimal number to binary number with toString() method

上面代码中的十进制数`5`被转换成与`101`等价的二进制数，然后转换成一个字符串。

## 如何通过 toString()方法使用其他数据类型

除了转换`number`类型之外，`toString()`方法也可用于将其他数据类型转换成它们的字符串表示。

例如，您可以将一个`array`类型转换成它的`string`表示，如下所示:

```
var arr = [ "Nathan", "Jack" ];
var str = arr.toString();

console.log(str); // "Nathan,Jack"
```

Convert an array to string with toString() method

或者从`boolean`型到`string`型如下图所示:

```
var bool = true;
var str = bool.toString();

console.log(str); // "true"
```

但是我认为你最常使用的是`toString()`方法将一个`number`转换成一个`string`，而不是其他的。我平时也是这样:)

## **感谢阅读本教程**

你可能也会对我写过的其他 JavaScript 教程感兴趣，包括[用`toFixed()`方法](https://sebhastian.com/javascript-tofixed/)舍入数字、[用](https://sebhastian.com/javascript-absolute-value-math-abs/) `[Math.abs](https://sebhastian.com/javascript-absolute-value-math-abs/)()`计算绝对值。这是两个最常见的 JavaScript 问题。

我还有一个关于 web 开发教程的[免费简讯](https://sebhastian.com/newsletter/)(大部分是 JavaScript 相关的)。