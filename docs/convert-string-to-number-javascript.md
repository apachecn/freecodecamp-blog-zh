# 如何在 JavaScript 中将字符串转换成数字(附示例)

> 原文：<https://www.freecodecamp.org/news/convert-string-to-number-javascript/>

## **将字符串转换成数字**

函数解析一个字符串参数并返回一个指定基数的整数(数学数字系统中的基数)。

```
parseInt(string, radix);
```

### **参数**

```
string
```

要分析的值。如果`string`参数不是一个字符串，那么它被转换成一个字符串(使用`ToString`抽象操作)。字符串参数中的前导空格将被忽略。= radix 一个介于 2 和 36 之间的整数，表示上述字符串的基数(数学数字系统中的基数)。指定`10`为人类常用的十进制数字系统。请始终指定此参数，以消除读者的困惑并保证可预测的行为。当未指定基数时，不同的实现会产生不同的结果，通常默认值为 10。返回值从给定字符串中解析出的整数。如果第一个字符不能转换成数字，则返回`NaN`。

### **描述**

`parseInt`函数将其第一个参数转换成一个字符串，解析它，并返回一个整数或`NaN`。如果不是`NaN`，返回值将是第一个参数作为指定基数(base)中的一个数字的整数。例如，基数 10 表示从十进制数、8 八进制数、16 十六进制数等进行转换。对于`10`以上的基数，字母表中的字母表示大于 9 的数字。例如，对于十六进制数(基数为 16)，使用`A`到`F`。

如果`parseInt`遇到一个不是指定基数中的数字的字符，它会忽略它和所有后续字符，并返回解析到该点的整数值。`parseInt`将数字截断为整数值。允许有前导空格和尾随空格。

因为一些数字在它们的字符串表示中包含了`e`字符(例如`6.022e23`，所以当用于非常大或非常小的数字时，使用`parseInt`来截断数值会产生意想不到的结果。`parseInt`不应作为`Math.floor()`的替代品。

如果基数为`undefined`或 0(或不存在)，JavaScript 假定如下:

*   如果输入`string`以“0x”或“0X”开始，基数是 16(十六进制),字符串的剩余部分被解析。
*   如果输入`string`以“0”开头，则基数为 8(八进制)或 10(十进制)。具体选择哪种基数取决于具体实现。ECMAScript 5 指定使用 10(十进制)，但并非所有浏览器都支持这一点。因此，在使用 parseInt 时，一定要指定基数。
*   如果输入`string`以任何其他值开始，基数是 10(十进制)。
*   如果第一个字符不能转换为数字，parseInt 将返回 NaN。

出于算术目的，NaN 值不是任何基数的数字。可以调用 isNaN 函数来确定 parseInt 的结果是否为 NaN。如果将 NaN 传递给算术运算，运算结果也将是 NaN。

要将数字转换为特定基数的字符串文字，请使用 intValue.toString(radix)。

### **例题**

使用`parseInt`下面的例子都返回`15`:

```
parseInt(' 0xF', 16);
parseInt(' F', 16);
parseInt('17', 8);
parseInt(021, 8);
parseInt('015', 10);   // parseInt(015, 10); will return 15
parseInt(15.99, 10);
parseInt('15,123', 10);
parseInt('FXX123', 16);
parseInt('1111', 2);
parseInt('15 * 3', 10);
parseInt('15e2', 10);
parseInt('15px', 10);
parseInt('12', 13);
```

下面的例子都返回`NaN`:

```
parseInt('Hello', 8); // Not a number at all
parseInt('546', 2);   // Digits are not valid for binary representations
```

下面的例子都返回`-15`:

```
parseInt('-F', 16);
parseInt('-0F', 16);
parseInt('-0XF', 16);
parseInt(-15.1, 10)
parseInt(' -17', 8);
parseInt(' -15', 10);
parseInt('-1111', 2);
parseInt('-15e1', 10);
parseInt('-12', 13);
```

下面的例子都返回`4`:

```
parseInt(4.7, 10);
parseInt(4.7 * 1e22, 10); // Very large number becomes 4
parseInt(0.00000000000434, 10); // Very small number becomes 4
```

以下示例返回`224`:

```
parseInt('0e0', 16);
```

#### **更多信息:**

[MDN 上的 parse int](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators)

*   [parseInt()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) 和 [parseFloat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseFloat) 如果可能的话，尝试将字符串转换成数字。例如，`var x = parseInt("100"); // x = 100`
*   [Number()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/number) 将转换为一个可以表示该值的数字。这包括从 UTC 1970 年 1 月 1 日午夜开始的毫秒数，布尔值 1 或 0，不能转换为可识别数字的值将变成 NaN。它代表的不是一个数字，从技术上来说也是一个数字！