# JavaScript 字符串比较——如何在 JS 中比较字符串

> 原文：<https://www.freecodecamp.org/news/javascript-string-comparison-how-to-compare-strings-in-js/>

您可能想要比较两个字符串，以了解哪个在字母顺序上更高或更低，或者查看它们是否相等。

你可以用很多方法做到这一点。在本文中，我将向您展示其中的两个。

## 1.如何使用 localeCompare 比较字符串

您可以使用`localeCompare`方法来比较当前地区的两个字符串。下面是语法:

```
string1.localeCompare(string2) 
```

`locaelCompare`退货:

*   1 如果`string1`比`string2`大(按字母顺序更高)
*   -1 如果`string1`比`string2`小(在字母顺序中较低)
*   如果`string1`和`string2`在字母顺序上相等，则为 0

以下是比较两个字符串的一些示例:

```
const string1 = "hello"
const string2 = "world"

const compareValue = string1.localeCompare(string2)
// -1 
```

它给出了`-1`,因为在英语地区，hello 中的 **h** 在世界上排在 **w** 之前(w 在字母顺序中比 h 靠后)

另一个例子:

```
const string1 = "banana"
const string2 = "back"

const compareValue = string1.localeCompare(string2)
// 1 
```

上面的比较给出了`1`，因为在英语区域中，香蕉中的 ba **n** 在后面的 ba **c** 之后。

再举一个例子:

```
const string1 = "fcc"
const string2 = "fcc"
const string3 = "Fcc"

const compareValue1 = string1.localeCompare(string2)
// 0

const compareValue2 = string1.localeCompare(string3)
// -1 
```

比较“fcc”和“fcc”得到`0`，因为它们在顺序上是相等的。“fcc”和“Fcc”给出`-1`是因为大写“F”大于小写“F”。

在某些浏览器中，它可能会返回 **-2** 或其他负值，而不是 **-1** 。因此，不要依赖于 **-1** 或 **1** ，而是依赖于负值(小于 0)或正值(大于 0)

## 2.如何使用数学运算符比较字符串

在比较字符串时，还可以使用数学运算符，如大于( **>** )、小于(< )和等于。

数学运算符的工作方式与`localeCompare`类似——根据字符串中字符的顺序返回结果。

使用前面的例子:

```
const string1 = "hello"
const string2 = "world"

console.log(string1 > string2)
// false 
```

`string1`不大于`string2`，因为 **h** 先于 **w** ，所以小于。

对于另一个例子:

```
const string1 = "banana"
const string2 = "back"

console.log(string1 > string2)
// true 
```

`string1`大于`string2`是因为 ba **n** 在 ba **c** k 之后

对于最后一个例子:

```
const string1 = "fcc"
const string2 = "fcc"
const string3 = "Fcc"

console.log(string1 === string2)
// true

console.log(string1 < string3)
// false 
```

`string1`等于(`===` ) `string2`，但`string1`不小于`string3`，与`localeCompare`形成对比。

用数学运算符，“fcc”大于“fcc”，但用`localeCompare`，`"fcc".localeCompare("Fcc")"`返回`-1`表示“Fcc”小于“Fcc”。

这种行为是我不推荐使用数学运算符来比较字符串的原因之一，尽管它有这样做的潜力。

我不推荐使用数学运算符的另一个原因是因为`"fcc" > "fcc"`和`"fcc" < "fcc"`是`false`。“fcc”等于“fcc”。因此，如果你依赖数学运算符，得到`false`的原因可能与你认为的不同。

因此，在众多比较字符串的方法中，使用`localCompare`是一种有效的方法，因为它可以用于不同的语言。

现在你知道了一个比较字符串的简单方法。编码快乐！