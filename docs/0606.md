# JavaScript 首字母大写——如何用 JS 大写一个单词的首字母

> 原文：<https://www.freecodecamp.org/news/javascript-capitalize-first-letter-of-word/>

当你编码时，有很多方法可以将一个单词的第一个字母大写。你可以使用 CSS 和一些 JavaScript 方法。

在本文中，我将向您展示实现这一目标的一种方法。

用 JS 大写一个单词的首字母，需要了解三种字符串方法: **charAt** 、 **slice** 和 **toUpperCase** 。

## `charAt` JavaScript 字符串方法

使用此方法可以检索字符串中指定位置的字符。使用这种方法，我们可以检索单词中的第一个字母:

```
const word = "freecodecamp"

const firstLetter = word.charAt(0)
// f 
```

## `slice` JavaScript 字符串方法

您可以使用此方法从整个字符串中剪切出一个子字符串。我们会用这个方法把一个单词的剩余部分(不包括第一个字母)切掉:

```
const word = "freecodecamp"

const remainingLetters = word.substring(1)
// reecodecamp 
```

## `toUpperCase` JavaScript 字符串方法

`toUpperCase`是一个字符串方法，返回指定字符串的大写版本。我们将用它来大写第一个字母:

```
const firstLetter = "f"

const firstLetterCap = firstLetter.toUpperCase()
// F 
```

## JavaScript 中如何将单词的首字母大写

使用上面的三个字符串方法，我们将获得单词的第一个字符，将其大写，然后将其与剩余的切片部分连接起来。

这种方法会产生一个首字母大写的新单词。

下面是它的代码:

```
const word = "freecodecamp"

const firstLetter = word.charAt(0)

const firstLetterCap = firstLetter.toUpperCase()

const remainingLetters = word.slice(1)

const capitalizedWord = firstLetterCap + remainingLetters
// Freecodecamp
// F is capitalized 
```

上面代码的简短版本是:

```
const word = "freecodecamp"

const capitalized =
  word.charAt(0).toUpperCase()
  + word.slice(1)

// Freecodecamp
// F is capitalized 
```

感谢您的阅读，祝您编码愉快！