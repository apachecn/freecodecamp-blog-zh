# JavaScript 字符串到布尔值——如何在 JS 中解析布尔值

> 原文：<https://www.freecodecamp.org/news/javascript-string-to-boolean/>

当您操作数据、从表单接收值以及以其他方式处理数据时，这些值可能会采用不正确的数据类型。

假设您希望您的值是一个带有`true`或`false`的布尔值，但是它被存储为一个字符串——“真”或“假”。对于您的预期目的来说，使用变得具有挑战性，因此您必须首先将这些布尔字符串值转换为实际的布尔值。

在本文中，您将学习如何使用 JavaScript 中的不同方法将字符串转换为布尔值。如果你赶时间，你可以这样做:

```
// Using identity operator
console.log((boolString === "true")); // true / false

// Using Regex
console.log((/true/).test(boolString)); // true 
```

如果你不着急，让我们了解每一个方法和更多。

## 如何用恒等运算符(`===`)将字符串解析为布尔值

严格运算符是标识运算符的另一个名称。如果被比较的两个值相同，它将只返回`true`。这意味着他们的字母大小写——以及其他一切——也必须相同。否则将返回`false`。

在这种情况下，您希望将字符串转换为布尔值，这意味着您将把它与字符串“true”进行比较。如果两个值相同，则返回布尔值`true`，否则返回布尔值`false`。

```
let boolString = "true"; 
let boolValue = (boolString === "true"); 
console.log(boolValue); // true 
```

这是一个严格的等式运算符，将严格执行字母大小写比较:

```
let boolString = "True"; 
let boolValue = (boolString === "true"); 
console.log(boolValue); // false 
```

您可以使用`toLowerCase()`方法来解决这个问题，因此它首先将字符串值转换为适合您的比较的字母大小写，然后进行比较。

```
let boolString = "True"; 
let boolValue = (boolString.toLowerCase() === "true"); 
console.log(boolValue); // true 
```

另一个与 identity 操作符非常相似的方法是 regex 方法，在这里可以测试两个值是否匹配。

## 如何用正则表达式将字符串解析为布尔值

Regex 代表正则表达式。这是一个庞大的编程主题，您可以使用 regex 作为模式来匹配和测试字符串字符组合。

一个非常简单的 regex 指南会告诉你，表达式放在两个斜杠(`/`)之间。例如，如果您想测试真实的字符串值，您应该这样做:

```
let boolString = "true"; 
let boolValue = (/true/).test(boolString);
console.log(boolValue); // true 
```

这也是区分大小写的:

```
let boolString = "True"; 
let boolValue = (/true/).test(boolString);
console.log(boolValue); // false 
```

您必须在正则表达式的末尾添加`i`标志，以允许不区分大小写的匹配。

```
let boolString = "True"; 
let boolValue = (/true/i).test(boolString);
console.log(boolValue); // true 
```

## 如何用双非运算符(`*!!*`)将字符串解析为布尔值

您还应该知道如何使用单个 NOT 运算符，您可以使用它来反转结果。

当您在字符串前面添加单个 NOT 运算符时，它将返回`true`或`false`。如果是空字符串，则返回`true`，否则返回`false`:

```
let stringValue1 = !'true';
let stringValue2 = !'';

console.log(stringValue1); // false
console.log(stringValue2); // true 
```

这不是你想要的。相反，您希望将字符串转换为布尔值，这意味着当字符串为空时，它应该返回`false`，而在其他情况下，它应该返回`true`。

这时可以使用双重非逻辑运算符。您可以使用它来反转单个 NOT 运算符的结果:

```
let stringValue1 = !!'true';
let stringValue2 = !!'';

console.log(stringValue1); // true
console.log(stringValue2); // false 
```

您使用这个方法将任何字符串值转换为布尔值**。当它为空时，它返回`false`。否则返回`true`。**

这种方法的一个缺点是不能将字符串`"false"`转换成布尔值`false`。只有当它是一个空字符串时，它才会返回`false`。

## 如何用布尔包装器将字符串解析为布尔值

JavaScript 布尔对象代表一个布尔值。这个方法就像 double NOT 操作符一样。

```
// Syntax
Boolean() 
```

当您将一个字符串值传递给布尔对象时，它将计算为`true`，但是当您传递一个空字符串时，它将计算为`false`。

```
let stringValue1 = Boolean('true');
let stringValue2 = Boolean('');

console.log(stringValue1); // true
console.log(stringValue2); // false 
```

这种方法的唯一限制是，如果您在引号之间添加空格来表示字符串，它将返回`true` —这意味着它将它视为一个字符串。

```
let stringValue = Boolean(' ');

console.log(stringValue); // true 
```

**注意:**当你转换一串`"false"`时，你会期望它返回一个布尔值`false`。但这将返回`true`，因为它只在为空字符串时返回`false`。

## 包扎

在本文中，您已经学习了如何将字符串值转换为布尔值。涵盖所有场景的最佳方法是等式运算符，而布尔对象和 double 逻辑 NOT 具有更好的语法。

祝编码愉快！