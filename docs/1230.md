# JavaScript split()一个字符串-字符串到数组的 JS 方法

> 原文：<https://www.freecodecamp.org/news/javascript-split-a-string-string-to-array-js-method/>

如果您需要将一个字符串拆分成一个子字符串数组，那么您可以使用 JavaScript `split()`方法。

在本文中，我将介绍 JavaScript `split()`方法并提供代码示例。

## split()方法的基本语法

下面是 JavaScript `split()`方法的语法。

```
str.split(optional-separator, optional-limit)
```

可选分隔符是一种模式，它告诉计算机每个拆分应该在哪里发生。

可选的 limit 参数是一个正数，它告诉计算机返回的数组值中应该有多少个子字符串。

## JavaScript split()方法代码示例

在第一个例子中，我有一个字符串`"I love freeCodeCamp"`。如果我使用不带分隔符的`split()`方法，那么返回值将是整个字符串的数组。

```
const str = 'I love freeCodeCamp';

str.split();
// return value is ["I love freeCodeCamp"]
```

### 使用可选分隔符参数的示例

如果我想改变它，使字符串被分割成单独的字符，那么我需要添加一个分隔符。分隔符将是一个空字符串。

```
const str = 'I love freeCodeCamp';

str.split('');
// return value ["I", " ", "l", "o", "v", "e", " ", "f", "r", "e", "e", "C", "o", "d", "e", "C", "a", "m", "p"]
```

请注意空格在返回值中是如何被视为字符的。

如果我想改变它，所以字符串被分割成单个的单词，那么分隔符将是一个空字符串与一个空格。

```
const str = 'I love freeCodeCamp';

str.split(' ');
// return value ["I", "love", "freeCodeCamp"]
```

### 使用可选极限参数的示例

在这个例子中，我将使用 limit 参数返回一个数组，该数组只包含句子`"I love freeCodeCamp"`的第一个单词。

```
const str = 'I love freeCodeCamp';

str.split(' ',1);
// return value ["I"]
```

如果我把极限值改为 0，那么返回值将是一个空数组。

```
const str = 'I love freeCodeCamp';

str.split(' ',0);
//return value []
```

## 应该使用 split()方法反转字符串吗？

反向字符串练习是一个非常流行的编码挑战。一种常见的解决方法是使用`split()`方法。

在本例中，我们有字符串“freeCodeCamp”。如果我们想反转这个单词，那么我们可以将`split()`、`reverse()`和`join()`方法链接在一起，返回新的反转后的字符串。

```
const str = 'freeCodeCamp';

str.split('').reverse().join('');
//return value "pmaCedoCeerf"
```

`.split('')`部分将字符串分割成一个字符数组。

`.reverse()`部分在适当的位置反转字符数组。

`.join('')`部分将数组中的字符连接在一起，并返回一个新字符串。

对于这个例子，这种方法似乎工作得很好。但是在一些特殊的情况下，这是行不通的。

让我们来看看 [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split)中提供的例子。

如果我们试图颠倒字符串“马纳纳·mañana”，那么就会导致意想不到的结果。

```
const str = 'mañana mañana'
const reversedStr = str.split('').reverse().join('')

console.log(reversedStr)
// return value would be "anãnam anañam"
```

请注意波浪号(~)是如何放在字母`"a"`上，而不是倒过来的单词中的`"n"`。发生这种情况是因为我们的字符串包含所谓的字素。

字素簇是一系列符号的组合，产生一个单一的字符，人类可以在屏幕上阅读。当我们试图使用这些类型的字符反转字符串时，计算机可能会错误地解释这些字符，并产生一个不正确的反转字符串版本。

如果我们只分离拆分方法，你可以看到计算机是如何拆分每个字符的。

```
const str = 'mañana mañana'

console.log(str.split(''))
//["m", "a", "ñ", "a", "n", "a", " ", "m", "a", "n", "̃", "a", "n", "a"]
```

如果您使用这些特殊字符，您可以在您的项目中使用[包](https://github.com/mathiasbynens/esrever)来修复这个问题并正确地反转字符串。

## 结论

JavaScript `split()`方法用于将一个字符串拆分成一个子字符串数组。

下面是 JavaScript `split()`方法的语法。

```
str.split(optional-separator, optional-limit)
```

可选分隔符是一种模式，它告诉计算机每个拆分应该在哪里发生。

可选的 limit 参数是一个正数，它告诉计算机返回的数组值中应该有多少个子字符串。

您可以使用 split 方法来反转字符串，但是在一些特殊情况下这是行不通的。如果您的字符串包含字素簇，那么结果可能会产生一个不正确的反向单词。

您也可以选择在反转字符串之前使用 spread 语法来拆分字符串。

```
const str = 'mañana mañana'
console.log([...str].reverse().join(""))
```

我希望您喜欢这篇文章，并祝您的 JavaScript 之旅好运。