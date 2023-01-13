# JavaScript 字符串格式——在 JS 中格式化字符串

> 原文：<https://www.freecodecamp.org/news/javascript-string-format-how-to-format-strings-in-js/>

JavaScript 有许多字符串方法可以用来格式化字符串。在本文中，我将向您展示一些最常用的方法。

## 如何使用`toLowerCase()`字符串方法

顾名思义，使用`toLowerCase()` string 方法将字符串转换成小写形式。

此方法不影响原始字符串。它获取原始字符串并返回一个新字符串，该字符串是小写版本。

这里有一个例子:

```
const string = "HeLLo woRld"

const lowercased = string.toLowerCase()

console.log(string)
// HeLLo woRld

console.log(lowercased)
// hello world 
```

如您所见，新字符串的所有字母都是小写的。

## 如何使用`toUpperCase()`字符串方法

与第一种方法类似，`toUpperCase`是一种字符串方法，用于将字符串转换成大写形式。

它也不影响原始字符串。

这里有一个例子:

```
const string = "HeLLo woRld"

const uppercased = string.toUpperCase()

console.log(string)
// HeLLo woRld

console.log(uppercased)
// HELLO WORLD 
```

使用原来的字符串，它返回一个新的字符串，这是大写版本。

## 如何使用`replace()`字符串方法

使用`replace` string 方法用子串替换字符串的一部分。这样，您将格式化字符串，以便可以修改它。

这里有一个关于`replace`方法如何工作的例子:

```
const string = "Hello world"

const modified = string.replace("world", "developers")

console.log(string)
// Hello world

console.log(modified)
// Hello developers 
```

如上所述，`replace`方法将“world”子串替换为“developers”。它也不影响原始字符串。

您也可以使用正则表达式代替字符串作为替换符:

```
const string = "Hello world"

const modified = string.replace(/o/g, "--")

console.log(string)
// Hello world

console.log(modified)
// Hell-- w--rld 
```

使用 regex 模式全局匹配“o”字符，您可以看到修改后的字符串包含双连字符，代替了字符串中的“o”。

## 如何使用`trim()`字符串方法

`trim`方法通过删除字符串开头和结尾的空格来修改字符串。

它不会修改原始字符串。相反，它返回一个去掉了空格的新字符串。

这里有一个例子:

```
const string = "  H ell  o world  "

const modified = string.trim()

console.log(string)
//   H ell  o world 

console.log(modified)
// H ell  o world 
```

可以看到，只有开头和结尾的空格被去掉了，字母之间的空格没有去掉。

空白包括空格、制表符、分隔线等等。

## 包扎

在 JavaScript 中，还有多种方法可以格式化或修改字符串。在这篇文章中，我分享了你可以使用的四种最常用的方法:`toUpperCase`、`toLowerCase`、`replace`和`trim`。

这些方法不影响原始字符串，但会返回以特定方式格式化的新字符串。

你可以在这里了解更多关于 JavaScript 中[有用的字符串方法。](https://dillionmegida.com/p/10-useful-string-methods-in-javascript/)