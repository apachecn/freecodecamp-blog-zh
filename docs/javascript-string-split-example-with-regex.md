# JavaScript 字符串。使用正则表达式的 Split()示例

> 原文：<https://www.freecodecamp.org/news/javascript-string-split-example-with-regex/>

在 JavaScript 中，使用正则表达式来匹配字符中的模式。将这个方法与`.split()`字符串方法结合起来，可以给你更多的拆分能力。

字符串构造函数有[许多有用的方法](https://dillionmegida.com/p/10-useful-string-methods-in-javascript/)，其中之一是`split()`方法。您可以使用此方法通过断点将字符串拆分为子字符串数组。

下面是它的常用方法:

```
const string = "How is everything going?"

const breakpoint = " "

const splitted = string.split(breakpoint);

// [ 'How', 'is', 'everything', 'going?' ] 
```

使用空格(" ")作为断点，`split`方法在这些断点处拆分字符串。

这里的断点是一个固定的字符。如果您想基于一个模式进行拆分，该怎么办？比如数字字符或者符号空间？然后您可以使用 regex 的`split`方法来实现这一点。

## 如何使用正则表达式？在 JavaScript 中拆分

`split`方法接受一个参数——断点。这个断点决定了拆分应该发生的点。此断点可以是字符串或正则表达式模式。

下面是一个使用正则表达式模式的例子:

```
const string = "How is $everything g$oing?"

const breakpoint = /\$e|\$o/

const splitted = string.split(breakpoint)

// [ 'How is ', 'verything g', 'ing?' ] 
```

正则表达式模式匹配美元符号后接字母“e”(`$e`)或美元符号后接字母“o”(`$o`)。

split 方法使用与该模式匹配的字符作为断点，正如您所看到的那样，“$everything”中的“$e”和“g$oing”中的“$o”作为断点将字符串分割成子字符串。

您不需要在正则表达式中应用全局标志`g`，因为 split 方法已经将正则表达式模式的所有出现作为断点。

## 包扎

使用`split`方法，您不仅需要使用文字字符串将字符串分割成数组。您可以使用正则表达式作为断点，匹配更多的字符来拆分字符串。

strings 中的`replace`方法也支持 regex 模式。本文来看看: [JavaScript 字符串。将()示例替换为正则表达式](https://www.freecodecamp.org/news/javascript-string-replace-example-with-regex/)