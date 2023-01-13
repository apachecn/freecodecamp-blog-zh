# JavaScript 字符串。用正则表达式替换()示例

> 原文：<https://www.freecodecamp.org/news/javascript-string-replace-example-with-regex/>

正则表达式(也称为 RegEx 或 RegExp)是一种分析文本的强大方法。使用 RegEx，可以在匹配特定字符(例如 JavaScript)或模式(例如 NumberStringSymbol - 3a&)的点匹配字符串。

在 JavaScript 中，`.replace`方法用于字符串，用字符替换部分字符串。它经常被这样使用:

```
const str = 'JavaScript';
const newStr = str.replace("ava", "-");
console.log(newStr);
// J-Script 
```

正如您在上面看到的，replace 方法接受两个参数:要替换的字符串，以及该字符串将被替换为什么。

这就是正则表达式的用武之地。

上面`.replace`的用途是有限的:要替换的字符是已知的——“ava”。如果我们关心的是一种模式呢？也许，一个数字，两个字母，和单词“foo”或者三个符号一起使用？

与`RegEx`一起使用的`.replace`方法可以实现这一点。`RegEx`可以有效地用来再造图案。因此，将这与`.replace`结合起来意味着我们可以替换模式，而不仅仅是精确的字符。

## 如何在 JavaScript 中使用`RegEx`和`.replace`

要使用 regex，`replace`的第一个参数将被替换为 RegEx 语法，例如`/regex/`。这种语法充当一种模式，在这种模式下，匹配它的字符串的任何部分都将被替换为新的子字符串。

这里有一个例子:

```
// matches a number, some characters and another number
const reg = /\d.*\d/
const str = "Java3foobar4Script"
const newStr = str.replace(reg, "-");
console.log(newStr);
// "Java-Script" 
```

字符串`3foobar4`与正则表达式`/\d.*\d/`匹配，因此被替换。

如果我们想在多个地方进行替换会怎么样？

`Regex`已经提供了带`g`(全球)标志的，同样可以用于`replace`。方法如下:

```
const reg = /\d{3}/g
const str = "Java323Scr995ip4894545t";
const newStr = str.replace(reg, "");
console.log(newStr);
// JavaScrip5t
// 5 didn't pass the test :( 
```

正则表达式匹配正好是 3 个连续数字的字符串部分。`323`匹配，`995`匹配，`489`匹配，`454`匹配。但是最后一个`5`不符合这个模式。

结果是`JavaScrip5t`显示了模式是如何正确匹配的，并用新的子字符串(空字符串)替换。

也可以使用大小写标志- `i`。这意味着您可以替换不区分大小写的模式。下面是它的使用方法:

```
const reg1 = /\dA/
const reg2 = /\dA/i
const str = "Jav5ascript"
const newStr1 = str.replace(reg1, "--");
const newStr2 = str.replace(reg2, "--");
console.log(newStr1) // Jav5ascript
console.log(newStr2) // Jav--script 
```

`..5a..`与第一个语法不匹配，因为默认情况下 RegEx 区分大小写。但是使用了`i`标志，如第二个语法所示，字符串如预期的那样被替换。

## 如何对正则表达式使用 Split

`split`也用`RegEx`。这意味着您不仅可以在匹配精确字符的子字符串处拆分字符串，还可以拆分模式。

让我们快速浏览一下:

```
const regex = /\d{2}a/;
const str = "Hello54 How 64aare you";
console.log(str.split(regex))
// ["Hello54 How ", "are you"] 
```

字符串在`64a`处为`split`，因为该子字符串匹配指定的正则表达式。

**注意**`split`中的全球旗- `g` -是不相关的，不像`i`旗和其他旗。这是因为`split`在正则表达式匹配的几个点上分割字符串。

## 包扎

使 JavaScript 中的字符串更加有效、强大和有趣。

你不仅仅局限于精确的字符，还包括模式和多次替换。在本文中，我们通过几个例子看到了它们是如何协同工作的。

为 RegEx 干杯？