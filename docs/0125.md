# JavaScript 中的强制和类型转换——用代码示例解释

> 原文：<https://www.freecodecamp.org/news/coercion-and-type-conversion-in-javascript/>

强制是一种自动类型转换，当您想要执行某些操作时，它会在 JavaScript 中发生。我会在这篇文章中解释什么是强制。

## 什么是类型转换？

顾名思义，类型转换是将一个值从一种类型转换为另一种类型的过程。

JavaScript 中的值可以是不同的类型。你可以有一个数字、字符串、对象、布尔值——你能想到的。有时，您可能希望将数据从一种类型转换为另一种类型，以适应特定的操作。

类型转换可以是隐式的(在代码执行期间自动完成)，也可以是显式的(由开发人员完成)。

隐式类型转换也被称为(也更常被称为)强制，而显式类型转换也被称为**类型转换**。我们来详细看看这两个转换。

如果你喜欢的话，我还有这个教程的视频版本。

## 什么是隐式类型转换(强制)？

有些操作你可以尝试用 JavaScript 来执行，但实际上是不可能的。例如，看看下面的代码:

```
const sum = 35 + "hello" 
```

在这里，您试图添加一个数字和一个字符串。实际上，这是不可能的。您只能将数字( **sum** )相加，或者将字符串( **concatenate** )相加。

如果你试着运行这段代码，会发生什么呢？

JavaScript 是一种弱类型语言。JavaScript 不会抛出错误，而是强制一个值的类型符合另一个值的类型，以便可以执行操作。

在这种情况下，使用带有数字和字符串的 **+** 符号，数字被强制转换为字符串，然后使用 **+** 符号进行连接操作。

```
const sum = 35 + "hello"

console.log(sum)
// 35hello

console.log(typeof sum)
// string 
```

这是一个强制示例，其中一个值的类型被强制为适合另一个值，以便操作可以继续。

有了加号，数字转换成字符串(而不是字符串转换成数字)更理想。这是因为一个等价于字符串的数字是`NaN`，但是一个等价于数字的字符串，比如说`15`，是`"15"`，所以**连接**两个字符串比**将**和`NaN`相加更有意义。

看下面另一个例子:

```
const times = 35 * "hello"

console.log(times)
// NaN 
```

这里，我们用时间 ***** 来表示一个数字和一个字符串。没有涉及乘法的字符串操作，所以在这里，理想的强制是从字符串到数字(因为数字有与乘法兼容的操作)。

但是由于一个字符串(在本例中为`"hello"`)被转换成一个数字(即`NaN`)并且这个数字被乘以`35`，所以最终结果是`NaN`。

强制通常是由不同数据类型之间使用的不同运算符引起的:

```
const string = ""
const number = 40
const boolean = true

console.log(!string)
// true - string is coerced to boolean `false`, then the NOT operator negates it

console.log(boolean + string)
// "true" - boolean is coerced to string "true", and concatenated with the empty string

console.log(40 + true)
// 41 - boolean is coerced to number 1, and summed with 40 
```

一个非常常见的导致强制的操作符是**松散相等操作符** ( **==** ，或者双等于)。

## 双重平等和强制

在 JavaScript 中，既有双重相等运算符( **==** ，称为**宽松相等运算符**，也有三重相等运算符( **===** ，称为**严格相等运算符**)。您使用两个运算符来比较值的相等性。

### 松散相等运算符如何工作

**松散相等运算符**进行松散检查。它检查值是否相等。类型不是这个操作符的重点，只有值才是主要因素。

这里我指的是 **20** ，一个`number`类型的值，和**“20”**，一个`string`类型的值，当你使用双等式时是相等的:

```
const variable1 = 20
const variable2 = "20"

console.log(variable1 == variable2)
// true 
```

尽管类型不相等，但运算符返回`true`，因为值相等。这里发生的是**强制**。

当您对不同类型的值使用**松散相等运算符**时，首先发生的是强制。同样，在进行比较之前，这是将一个值转换为适合另一个值的类型的地方。

在这种情况下，**字符串“20”**被转换为数字类型(即`20`)，然后与另一个值进行比较，两者相等。

另一个例子:

```
const variable1 = false
const variable2 = ""

console.log(variable1 == variable2)
// true 
```

这里，`variable1`是值**假**(布尔型)，而`variable2`是值 **""** (空字符串，字符串型)。将这两个变量与双倍相等的结果进行比较`true`。这是因为空字符串被强制为布尔类型(即 **false** )。

### 严格相等运算符如何工作

这个操作符进行严格的检查——也就是说，它严格检查比较的值和类型。这里不发生类型强制，所以没有意外的答案。以下是上面的例子:

```
const variable1 = 20
const variable2 = "20"

console.log(variable1 === variable2)
// false

const variable3 = false
const variable4 = ""

console.log(variable3 === variable4)
// false 
```

在`variable1`和`variable2`的情况下，它们的值相同，但是类型不一样。于是三重平等回归`false`。

在`variable3`和`variable4`的情况下，它们具有相同的值(如果一个被转换为另一个的类型)但是类型不相同，所以三重等式这次也返回`false`。

## 什么是显式类型转换(类型转换)？

这里，您显式地将一个值从一种类型转换为另一种类型。这也可以让你成功执行某个操作。

要显式转换类型，可以使用类型`Constructors`。例如，要将数字转换为字符串:

```
const number = 30

const numberConvert = String(number)

console.log(numberConvert)
// "30"

console.log(typeof numberConvert)
// string 
```

另一个例子是将数字转换为布尔值:

```
const number = 30

const numberConvert = Boolean(number)

console.log(numberConvert)
// true

console.log(typeof numberConvert)
// boolean 
```

还有一个例子，将布尔值转换为字符串:

```
const boolean = false

const booleanConvert = String(boolean)

console.log(booleanConvert)
// "false"

console.log(typeof booleanConvert)
// string 
```

在这些例子中，我们显式地将一个值从一种类型转换成另一种类型。在什么情况下你需要这样做？

当您不知道值的类型时，这很有用。例如，来自 API 的数据。假设一个 API 被配置为返回一个字符串，可能是“50 ”,您希望使用严格的等式将它与一个数字进行比较，如下所示:

```
const apiData = {
  rate: "50"
}

console.log(apiData.rate === 50)
// false 
```

在这种情况下，在进行检查之前，您希望首先确保该值是一个显式的数字类型(而不是依赖双相等来触发强制):

```
const apiData = {
  rate: "50"
}

const rate = Number(apiData.rate)

console.log(rate === 50)
// true 
```

## 包扎

因为 JavaScript 是一种弱类型语言，所以有时会出现意外的类型转换。当您尝试在不同类型的值之间使用一些运算符时，这种情况会隐式发生。然后 JavaScript 试图“帮助”你，而不是得到一个错误。JavaScript 就像...

“哦，我想他们想键入一个字符串，但他们却键入了一个数字。在执行操作之前，让我们帮助他们将其转换为字符串。他们会很感激的😇"

实际上不是这样😂但我希望你能明白。

在本文中，我们已经通过例子看到了 JavaScript 中类型转换的工作方式——隐式和显式。

虽然强制有时会有所帮助，但它会导致意外的错误，尤其是在使用**松散相等运算符**比较值时。这就是为什么建议总是使用**严格相等运算符**来比较值。

另外，[使用 TypeScript](https://www.freecodecamp.org/news/an-introduction-to-typescript/) 可以帮助您避免不可预测的错误，因为您可以确保变量是您想要的数据类型。