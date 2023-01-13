# JavaScript 将字符串转换为数字–JS 字符串转换为 Int 示例

> 原文：<https://www.freecodecamp.org/news/javascript-convert-string-to-number-js-string-to-int-example/>

当您处理来自不同来源的数据时，其中一些数据可能会以不正确的格式到达。在对数据执行某些操作之前，您需要更正这些格式。

这只是您想学习如何在 JavaScript 中将字符串转换成数字的众多原因之一。

在本文中，我们将通过研究各种方法并为每种方法提供示例来学习如何将字符串转换为数字。

在我们开始之前，区分字符串值的一个常用方法是，它总是用单引号或双引号括起来，而数字不是:

```
"John Doe" -> String
'John Doe' -> String
"12" -> String
12 -> Number 
```

假设我们将字符串存储在一个变量中。检查变量是否为字符串的一个好方法是使用`typeof`操作符:

```
let name = "John Doe";

console.log(typeof name) // "string" 
```

现在让我们学习如何将字符串转换成数字。

## 如何使用`Number()`函数将字符串转换成数字

Number 函数是一种功能强大的方法，可用于将字符串或其他值转换为数字类型。如果值不能被转换，这个方法也将返回`NaN`:

```
console.log(Number('212'))  // 212
console.log(Number("2124"))  // 2124
console.log(Number('0.0314E+2')); // 3.14

console.log(Number("Hello World"))  // NaN
console.log(Number(undefined))  // NaN 
```

这也适用于变量:

```
let age = "12";
let password = "John12";

console.log(Number(age)) // 12
console.log(Number(password)) // NaN 
```

这是最容易使用的方法之一，因为它也可以处理十进制值，并在不处理这些值的情况下返回这些值:

```
let answer = "12.0";
let answer = "12.0267";

console.log(Number(answer)) // 12.0
console.log(Number(answer)) // 12.0267 
```

## 如何使用`parseInt()`和`parseFloat()`函数将字符串转换成数字

`parseInt()`和`parseFloat()`函数都接受一个字符串作为参数，然后将该字符串转换成一个整数/数字。

您还可以使用`parseInt()`将非整数转换为整数，而`parseFloat()`是更强大的方法，因为它可以维护浮点数和一些数学逻辑:

```
console.log(parseInt('12')) // 12
console.log(parseInt('12.092')) // 12.092
console.log(parseInt('  3.14  ')) // 3
console.log(parseInt('0.0314E+2')) // 0
console.log(parseInt('John Doe')) // NaN

console.log(parseFloat('12')) // 12
console.log(parseFloat('12.092')) // 12.092
console.log(parseFloat('  3.14  ')) // 3.14
console.log(parseFloat('0.0314E+2')) // 3.14
console.log(parseFloat('John Doe')) // NaN 
```

像往常一样，这也适用于变量:

```
let age = "12";

console.log(parseInt(age)) // 12
console.log(parseFloat(age)) // 12 
```

注意:当字符串的字符不能转换成数字时，`parseFloat()`函数总是返回 NaN:

```
console.log(parseFloat('N0.0314E+2')) // NaN 
```

## 如何使用一元加号运算符(`+`)将字符串转换为数字

是将某物转换成数字的最快和最简单的方法之一。我说“某些东西”是因为它转换的不仅仅是数字和浮点数的字符串表示——它还可以处理非字符串值`true`、`false`和`null`或空字符串。

这种方法的一个优点(也是缺点)是，它不对数字执行任何其他操作，如向上舍入或将其转换为整数。

让我们来看一些例子:

```
console.log(+'100'); // 100
console.log(+'100.0373'); // 100.0373
console.log(+''); // 0
console.log(+null); // 0
console.log(+true); // 1
console.log(+false); // 0
console.log(+'John Doe'); // NaN
console.log(+'0.0314E+2'); // 3.14 
```

正如所料，这也适用于变量:

```
let age = "74";

console.log(+age); // 74 
```

如果您比较`ParseInt()`和加一元运算符，在某些情况下，您可能最终会使用加一元运算符而不是`parseInt()`方法。

例如，假设您正在获取随机值——假设某个 UUID 值在某些时候可能以数字开头，而在其他时候可能以字母开头。这意味着使用`parseInt()`函数可能有时会返回`NaN`,有时会返回第一个数字字符:

```
console.log(parseInt("cb34d-234ks-2343f-00xj")); // NaN
console.log(parseInt("997da-00xj-2343f-234ks")); // 997

console.log(+"cb34d-234ks-2343f-00xj"); // NaN
console.log(+"997da-00xj-2343f-234ks"); // NaN 
```

## 如何使用 JavaScript 数学方法将字符串转换成数字

另一种将字符串转换成数字的方法是使用一些 JavScript 数学方法。

您可以使用`floor()`方法，该方法会将传递的值向下舍入到最接近的整数。与`floor()`相反的`ceil()`方法，向上舍入到最接近的整数。最后一种`round()`方法，介于两者之间，只是将数字四舍五入到最接近的整数(根据接近程度向上或向下)。

### 如何使用`Math.floor()` JavaScript 方法将字符串转换成数字

就像我上面解释的，这将总是返回一个整数。假设我们传递一个浮点值——它会将值向下舍入到最接近的整数。如果我们将字母作为字符串或任何其他非整数字符传递，这将返回`NaN`:

```
console.log(Math.floor("14.5")); // 14
console.log(Math.floor("654.508")); // 654
console.log(Math.floor("0.0314E+2")); // 3
console.log(Math.floor("34d-234ks")); // NaN
console.log(Math.floor("cb34d-234ks-2343f-00xj")); // NaN 
```

### 如何使用`Math.ceil()` JavaScript 方法将字符串转换成数字

这是非常相似的，并且将只取整我们的浮点值以总是返回一个整数:

```
console.log(Math.ceil("14.5")); // 15
console.log(Math.ceil("654.508")); // 655
console.log(Math.ceil("0.0314E+2")); // 3
console.log(Math.ceil("34d-234ks")); // NaN 
```

### 如何使用`Math.round()` JavaScript 方法将字符串转换成数字

这两种方法的工作原理相似，但只是在舍入到最接近的整数后返回整数:

```
console.log(Math.round("14.5")); // 15
console.log(Math.round("654.508")); // 655
console.log(Math.round("0.0314E+2")); // 3
console.log(Math.round("34d-234ks")); // NaN 
```

上述所有数学方法也适用于变量:

```
let age = "14.5";

console.log(Math.floor(age)); // 14
console.log(Math.ceil(age)); // 15
console.log(Math.round(age)); // 15 
```

## 如何使用一些数学运算将字符串转换成数字

这不是一个真正的方法，但它值得了解。到目前为止，我们已经讨论了实现这种转换的直接方法，但在某些情况下，您可能只是想执行这些数学运算来帮助转换。

这包括乘以`1`，除以`1`，以及减去`0`。当我们对字符串执行这些操作时，它们将被转换成整数:

```
console.log("14.5" / 1); // 14.5
console.log("0.0314E+2" / 1); // 3.14

console.log("14.5" * 1); // 14.5
console.log("0.0314E+2" * 1); // 3.14

console.log("14.5" - 0); // 14.5
console.log("0.0314E+2" - 0); // 3.14 
```

像往常一样，这也适用于变量:

```
let age = "14.5";

console.log(age / 1); // 14.5
console.log(age * 1); // 14.5
console.log(age - 0); // 14.5 
```

## 结论

在本文中，我们研究了在 JavaScript 中将字符串转换成整数的各种方法和途径。

最好是意识到有许多方法存在，这样你就可以选择最适合你的方法，并在任何情况下应用它。