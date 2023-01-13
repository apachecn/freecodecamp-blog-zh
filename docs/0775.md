# 如何在 JavaScript 中将字符串转换成数字

> 原文：<https://www.freecodecamp.org/news/how-to-convert-a-string-to-a-number-in-javascript/>

使用 JavaScript 将字符串转换成数字有很多种方法。但是这在代码中是什么样子的呢？

在本文中，我将向您展示 11 种将字符串转换为数字的方法。

## 如何在 JavaScript 中使用`Number()`函数将字符串转换成数字

将字符串转换成数字的一种方法是使用`Number()`函数。

在这个例子中，我们有一个名为`quantity`的字符串，其值为`"12"`。

```
const quantity = "12";
```

如果我们在`quantity`上使用了`typeof`操作符，那么它将返回字符串的类型。

```
console.log(typeof quantity);
```

![Screen-Shot-2022-05-01-at-9.50.17-AM](img/5dfb0455cbb47675a2c1f5e2db5f0159.png)

我们可以像这样使用`Number`函数将`quantity`转换成一个数字:

```
Number(quantity)
```

我们可以通过再次使用`typeof`操作符来检查它现在是一个数字。

```
console.log(typeof Number(quantity));
```

![Screen-Shot-2022-05-01-at-9.57.35-AM](img/b81e93d7539576b92cb89b791521d2f4.png)

如果您试图传入一个不能转换成数字的值，那么返回值将是`NaN`(不是数字)。

```
console.log(Number("awesome"));
```

![Screen-Shot-2022-05-01-at-10.00.34-AM](img/6f012e61f70ade13482c77cc9e4b425c.png)

## 如何在 JavaScript 中使用 **`parseInt()`** 函数将字符串转换成数字

另一种将字符串转换成数字的方法是使用 **`parseInt()`** 函数。这个函数接受一个字符串和一个可选的基数。

基数是一个介于 2 和 36 之间的数，它代表数字系统中的基数。例如，基数 2 表示二进制，而基数 10 表示十进制。

我们可以使用前面的`quantity`变量将字符串转换成数字。

```
const quantity = "12";

console.log(parseInt(quantity, 10));
```

如果我试图将`quantity`变量改为`"12.99"`会发生什么？使用 **`parseInt()`** 的结果会是数字 12.99 吗？

```
const quantity = "12.99";

console.log(parseInt(quantity, 10));
```

![Screen-Shot-2022-05-01-at-10.45.08-AM](img/9a7fd3329e7797ae4f2130b22098108e.png)

如您所见，结果是一个四舍五入的整数。如果你想返回一个浮点数，那么你需要使用 **`parseFloat()`。**

## 如何在 JavaScript 中使用 **`parseFloat()`** 函数将字符串转换成数字

**`parseFloat()`** 函数将接受一个值并返回一个浮点数。浮点数的例子有 12.99 或 3.14。

如果我们修改前面的例子，使用`parseFloat()`，那么结果将是浮点数 12.99。

```
const quantity = "12.99";

console.log(parseFloat(quantity));
```

![Screen-Shot-2022-05-01-at-10.55.03-AM](img/089a184601810d4705b7296b2472bcf7.png)

如果字符串中有前导或尾随空格，那么`parseFloat()`仍然会将该字符串转换成浮点数。

```
const quantity = "   12.99    ";

console.log(parseFloat(quantity));
```

![Screen-Shot-2022-05-01-at-11.05.35-AM](img/055a3b4e83c3a10264351b5427c0c91c.png)

如果字符串中的第一个字符不能转换成数字，那么`parseFloat()`将返回`NaN`。

```
const quantity = "F12.99";

console.log(parseFloat(quantity));
```

![Screen-Shot-2022-05-01-at-11.08.33-AM](img/e298eb45a571db28960814d39dee0d7c.png)

## 如何在 JavaScript 中使用一元加号运算符(`+`)将字符串转换成数字

一元加号运算符(`+`)将字符串转换成数字。运算符将位于操作数之前。

```
const quantity = "12";

console.log(+quantity);
```

![Screen-Shot-2022-05-01-at-11.14.51-AM](img/a4ef0e588b5c24080024b57f6e64b48c.png)

我们也可以使用一元加号运算符(`+`)将字符串转换成浮点数。

```
const quantity = "12.99";

console.log(+quantity);
```

![Screen-Shot-2022-05-01-at-11.16.38-AM](img/ab716df7423c4a57f16389b5bb697de0.png)

如果字符串值不能转换成数字，那么结果将是`NaN`。

```
const quantity = "awesome";

console.log(+quantity);
```

![Screen-Shot-2022-05-01-at-11.18.10-AM](img/686db146f72eb59dd55cda6d1ab24572.png)

## 如何在 JavaScript 中将字符串乘以数字 1 转换成数字

将字符串转换成数字的另一种方法是使用基本的数学运算。您可以将字符串值乘以 1，它将返回一个数字。

```
const quantity = "12";

console.log(quantity * 1);
```

![Screen-Shot-2022-05-01-at-11.42.58-AM](img/b6ad4c766ce50380021bc082d73b3509.png)

正如您所看到的，当我们将`quantity`值乘以 1 时，它返回数字 12。但是这是如何工作的呢？

在这个例子中，JavaScript 将我们的字符串值转换成一个数字，然后执行数学运算。如果字符串不能转换成数字，那么数学运算将不起作用，它将返回`NaN`。

```
const quantity = "awesome";

console.log(quantity * 1);
```

![Screen-Shot-2022-05-01-at-11.18.10-AM](img/686db146f72eb59dd55cda6d1ab24572.png)

这个方法也适用于浮点数。

```
const quantity = "10.5";

console.log(quantity * 1);
```

![Screen-Shot-2022-05-01-at-11.56.19-AM](img/7ff18d2d485ba47525609c64569f839d.png)

## 如何在 JavaScript 中通过将字符串除以数字 1 来将字符串转换为数字

您也可以将字符串除以 1，而不是乘以 1。JavaScript 将我们的字符串值转换成一个数字，然后执行数学运算。

```
const quantity = "10.5";

console.log(quantity / 1);
```

![Screen-Shot-2022-05-01-at-12.08.37-PM](img/702ee99bbf6870d55c66215a399507fa.png)

## 如何在 JavaScript 中通过从字符串中减去数字 0 将字符串转换为数字

另一种方法是从字符串中减去 0。像以前一样，JavaScript 将字符串值转换成数字，然后执行数学运算。

```
const quantity = "19";

console.log(quantity - 0);
```

![Screen-Shot-2022-05-01-at-12.11.59-PM](img/f4c089ed27c9eb965c880aaf3f0b8fcf.png)

## 如何在 JavaScript 中使用按位 NOT 运算符(`~`)将字符串转换成数字

按位非运算符(`~`)将反转操作数的位，并将该值转换为 32 位有符号整数。32 位有符号整数是用 32 位(或 4 个字节)表示整数的值。

如果我们对一个数使用一个按位非运算符(`~`)，那么它将执行这个操作:-(x + 1)

```
console.log(~19);
```

![Screen-Shot-2022-05-01-at-12.20.18-PM](img/a370d9bb0a23c10bd9a26422272d359e.png)

但是如果我们使用两个按位 NOT 运算符(`~`)，那么它会将我们的字符串转换成一个数字。

```
const quantity = "19";

console.log(~~quantity);
```

![Screen-Shot-2022-05-01-at-12.28.16-PM](img/b6689503c7a598b21229c9e658be16ab.png)

这个方法不适用于浮点数，因为结果将是一个整数。

```
const quantity = "19.99";

console.log(~~quantity);
```

![Screen-Shot-2022-05-01-at-12.31.16-PM](img/c3a598259521604c6841dc88ae68cc91.png)

如果您试图对非数字字符使用此方法，那么结果将是零。

```
const quantity = "awesome";

console.log(~~quantity);
```

![Screen-Shot-2022-05-01-at-12.32.45-PM](img/1d5c72b59107f36c420718fe24d954fb.png)

这种方法确实有其局限性，因为对于被认为太大的值，它会开始中断。确保您的数字在有符号的 32 位整数值之间是很重要的。

```
const quantity = "2700000000";

console.log(~~quantity);
```

![Screen-Shot-2022-05-01-at-12.36.16-PM](img/bfb00b05ed09cb95723dbcde82dc06b6.png)

要了解更多关于位非运算符(`~`)的信息，请阅读[文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Bitwise_NOT)。

## 如何在 JavaScript 中使用 **`Math.floor()`** 函数将字符串转换成数字

另一种将字符串转换成数字的方法是使用 **`Math.floor()`** 函数。这个函数会将数字向下舍入到最接近的整数。

```
const quantity = "13.4";

console.log(Math.floor(quantity));
```

![Screen-Shot-2022-05-01-at-12.44.53-PM](img/b950febc6d773d28e0fa36f0e6409c05.png)

就像前面的例子一样，如果我们试图使用非数字字符，那么结果将是`NaN`。

```
const quantity = "awesome";

console.log(Math.floor(quantity));
```

![Screen-Shot-2022-05-01-at-12.46.08-PM](img/61becf37043785d9833bcbb9b95c9720.png)

## 如何在 JavaScript 中使用 **`Math.ceil()`** 函数将字符串转换成数字

`Math.ceil()`函数会将一个数字向上舍入到最接近的整数。

```
const quantity = "7.18";

console.log(Math.ceil(quantity));
```

![Screen-Shot-2022-05-01-at-12.48.15-PM](img/97c602e7059388d90f87e8b4ca6c9d9e.png)

## 如何在 JavaScript 中使用 **`Math.round()`** 函数将字符串转换成数字

**`Math.round()`** 函数会将数字四舍五入到最接近的整数。

如果我的值是 6.3，那么 **`Math.round()`** 将返回 6。

```
const quantity = "6.3";

console.log(Math.round(quantity));
```

![Screen-Shot-2022-05-01-at-12.50.20-PM](img/4a594bab8f18bfa1252719ef33d3e96d.png)

但是如果我把那个值改成 6.5，那么 **`Math.round()`** 就会返回 7。

```
const quantity = "6.5";

console.log(Math.round(quantity));
```

![Screen-Shot-2022-05-01-at-12.51.35-PM](img/3aa9545e18f8f0c5a6b33f57b6fbc49f.png)

## 如何在 JavaScript 中将字符串转换成数字的视频说明

[https://scrimba.com/scrim/co2894c679bc693326603ac73?embed=freecodecamp,mini-header](https://scrimba.com/scrim/co2894c679bc693326603ac73?embed=freecodecamp,mini-header)

## 结论

在本文中，我向您展示了使用 JavaScript 将字符串转换为数字的 11 种方法。

下面是文章中讨论的 11 种不同的方法。

1.  使用`Number()`功能
2.  使用`parseInt()`功能
3.  使用`parseFloat()`功能
4.  使用一元加号运算符(`+`)
5.  将字符串乘以数字 1
6.  将字符串除以数字 1
7.  从字符串中减去数字 0
8.  使用按位非运算符(`~`)
9.  使用`Math.floor()`功能
10.  使用`Math.ceil()`功能
11.  使用`Math.round()`功能

我希望您喜欢这篇文章，并祝您的 JavaScript 之旅好运。