# JavaScript 正则表达式匹配示例——如何在字符串上使用 JS 替换

> 原文：<https://www.freecodecamp.org/news/javascript-regex-match-example-how-to-use-the-js-replace-method-on-a-string/>

正则表达式，缩写为 regex，有时也称为 regexp，是您可能知道的非常强大和有用的概念之一。但是它们可能会令人望而生畏，尤其是对于初级程序员。

不一定要这样。JavaScript 包括几个有用的方法，使得正则表达式的使用更加易于管理。在包含的方法中，`.match()`、`.matchAll()`和`.replace()`方法可能是您最常用的方法。

在本教程中，我们将详细介绍这些方法的来龙去脉，并看看为什么您可能会使用它们而不是其他包含的 JS 方法

## 正则表达式快速介绍

根据 MDN，正则表达式是“用于匹配字符串中字符组合的模式”。

这些模式有时可以包括特殊字符(`*`、`+`)、断言(`\W`、`^`)、组和范围(`(abc)`、`[123]`)，以及其他使 regex 如此强大但难以掌握的东西。

regex 的核心是寻找字符串中的模式——从测试字符串中的单个字符到验证电话号码是否有效，所有事情都可以用正则表达式来完成。

如果你是 regex 的新手，想在继续阅读之前练习一下，请查看我们的[交互式编码挑战](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/regular-expressions/using-the-test-method)。

## 如何使用`.match()`方法

因此，如果 regex 的目的是在字符串中寻找模式，你可能会问自己是什么让`.match()`方法如此有用？

与仅返回`true`或`false`的`.test()`方法不同，`.match()`将实际返回与您正在测试的字符串的匹配。例如:

```
const csLewisQuote = 'We are what we believe we are.';
const regex1 = /are/;
const regex2 = /eat/;

csLewisQuote.match(regex1); // ["are", index: 3, input: "We are what we believe we are.", groups: undefined]

csLewisQuote.match(regex2); // null 
```

这对于某些项目非常有用，尤其是如果您想要提取和操作正在匹配的数据而不改变原始字符串的话。

如果你只想知道是否找到了一个搜索模式，使用`.test()`方法——这样更快。

您可以从`.match()`方法中获得两个主要的返回值:

1.  如果有匹配，那么`.match()`方法将返回一个包含匹配的数组。我们稍后会对此进行更详细的讨论。
2.  如果不匹配，`.match()`方法将返回`null`。

你们中的一些人可能已经注意到了这一点，但是如果你看看上面的例子，`.match()`只匹配第一次出现的单词“are”。

很多时候你会想知道一个模式与你测试的字符串匹配的频率，所以让我们看看如何用`.match()`来做这件事。

### 不同的匹配模式

如果匹配的话，`.match()`返回的数组有两种不同的*模式*，因为没有更好的术语。

第一种模式是不使用全局标志(`g`)时，如上例所示:

```
const csLewisQuote = 'We are what we believe we are.';
const regex = /are/;

csLewisQuote.match(regex); // ["are", index: 3, input: "We are what we believe we are.", groups: undefined] 
```

在这种情况下，我们使用第一个匹配的数组以及原始字符串中匹配的索引、原始字符串本身和任何使用的匹配组。

但是假设您想要查看单词“are”在一个字符串中出现了多少次。为此，只需将全局搜索标志添加到正则表达式中:

```
const csLewisQuote = 'We are what we believe we are.';
const regex = /are/g;

csLewisQuote.match(regex); // ["are", "are"] 
```

您不会得到非全局模式中包含的其他信息，但是您会得到一个数组，其中包含您正在测试的字符串中的所有匹配项。

### 区分大小写

需要记住的重要一点是，正则表达式是区分大小写的。例如，假设您想要查看单词“we”在字符串中出现了多少次:

```
const csLewisQuote = 'We are what we believe we are.';
const regex = /we/g;

csLewisQuote.match(regex); // ["we", "we"] 
```

在这种情况下，您匹配的是一个小写的“w ”,后面跟着一个小写的“e ”,它只出现两次。

如果您希望单词“we”的所有实例都是大写或小写，您有几个选择。

首先，在用`.match()`方法测试字符串之前，可以对其使用`.toLowercase()`方法:

```
const csLewisQuote = 'We are what we believe we are.'.toLowerCase();
const regex = /we/g;

csLewisQuote.match(regex); // ["we", "we", "we"] 
```

或者，如果您想保留原来的大小写，可以将不区分大小写的搜索标志(`i`)添加到您的正则表达式中:

```
const csLewisQuote = 'We are what we believe we are.';
const regex = /we/gi;

csLewisQuote.match(regex); // ["We", "we", "we"] 
```

## 新的`.matchAll()`方法

现在您已经了解了所有关于`.match()`方法的知识，值得指出的是`.matchAll()`方法是最近引入的。

与返回数组或`null`的`.match()`方法不同，`.matchAll()`需要全局搜索标志(`g`，并返回迭代器或空数组:

```
const csLewisQuote = 'We are what we believe we are.';
const regex1 = /we/gi;
const regex2 = /eat/gi;

[...csLewisQuote.matchAll(regex1)]; 
// [
//   ["We", index: 0, input: "We are what we believe we are.", groups: undefined],
//   ["we", index: 12, input: "We are what we believe we are.", groups: undefined]
//   ["we", index: 23, input: "We are what we believe we are.", groups: undefined]
// ]

[...csLewisQuote.matchAll(regex2)]; // [] 
```

虽然这看起来只是一个更复杂的`.match()`方法，但是`.matchAll()`提供的主要优势是它可以更好地与捕获组一起工作。

这里有一个简单的例子:

```
const csLewisRepeat = "We We are are";
const repeatRegex = /(\w+)\s\1/g;

csLewisRepeat.match(repeatRegex); // ["We We", "are are"] 
```

`.match()`

```
const csLewisRepeat = "We We are are";
const repeatRegex = /(\w+)\s\1/g;

[...repeatStr.matchAll(repeatRegex)];

// [
//   ["We We", "We", index: 0, input: "We We are are", groups: undefined],
//   ["are are", "are", index: 6, input: "We We are are", groups: undefined],
// ] 
```

`.matchAll()`

虽然这只是皮毛，但是请记住，如果您使用的是`g`标志，并且想要得到`.match()`为单个匹配提供的所有额外信息(索引、原始字符串等等)，那么使用`.matchAll()`可能会更好。

## 如何使用`.replace()`方法

所以现在你知道了如何匹配字符串中的模式，你可能想对这些匹配做些有用的事情。

一旦找到匹配的模式，你最常做的一件事就是用别的东西替换那个模式。例如，您可能希望将“paidCodeCamp”中的“付费”替换为“免费”。Regex 是一个很好的方法。

因为`.match()`和`.matchAll()`返回关于每个匹配模式的索引信息，取决于您如何使用它，您可以使用它来做一些有趣的字符串操作。但是有一个更简单的方法——使用`.replace()`方法。

使用`.replace()`，您需要做的就是将您想要匹配的字符串或正则表达式作为第一个参数传递给它，将替换匹配模式的字符串作为第二个参数传递给它:

```
const campString = 'paidCodeCamp';
const fCCString1 = campString.replace('paid', 'free');
const fCCString2 = campString.replace(/paid/, 'free');

console.log(campString); // "paidCodeCamp"
console.log(fCCString1); // "freeCodeCamp"
console.log(fCCString2); // "freeCodeCamp" 
```

最好的部分是`.replace()`返回一个新的字符串，原来的保持不变。

类似于`.match()`方法，`.replace()`将只替换它找到的第一个匹配模式，除非你使用带有`g`标志的正则表达式:

```
const campString = 'paidCodeCamp is awesome. You should check out paidCodeCamp.';
const fCCString1 = campString.replace('paid', 'free');
const fCCString2 = campString.replace(/paid/g, 'free');

console.log(fCCString1); // "freeCodeCamp is awesome. You should check out paidCodeCamp."
console.log(fCCString2); // "freeCodeCamp is awesome. You should check out freeCodeCamp." 
```

与之前类似，无论您传递字符串还是正则表达式作为第一个参数，重要的是要记住匹配模式是区分大小写的:

```
const campString = 'PaidCodeCamp is awesome. You should check out PaidCodeCamp.';
const fCCString1 = campString.replace('Paid', 'free');
const fCCString2 = campString.replace(/paid/gi, 'free');

console.log(fCCString1); // "freeCodeCamp is awesome. You should check out PaidCodeCamp."
console.log(fCCString2); // "freeCodeCamp is awesome. You should check out freeCodeCamp." 
```

## 如何使用`.replaceAll()`方法

就像`.match()`有更新的`.matchAll()`方法一样，`.replace()`也有更新的`.replaceAll()`方法。

`.replace()`和`.replaceAll()`之间唯一真正的区别是，如果你使用带有`.replaceAll()`的正则表达式，你需要使用全局搜索标志:

```
const campString = 'paidCodeCamp is awesome. You should check out paidCodeCamp.';
const fCCString1 = campString.replaceAll('paid', 'free');
const fCCString2 = campString.replaceAll(/paid/g, 'free');

console.log(fCCString1); // "freeCodeCamp is awesome. You should check out freeCodeCamp."
console.log(fCCString2); // "freeCodeCamp is awesome. You should check out freeCodeCamp." 
```

使用`.replaceAll()`的真正好处是可读性更好，并且当您将一个字符串作为第一个参数传递给它时，它会替换所有匹配的模式。

就是这样！现在您已经知道了用 regex 和一些内置的 JS 方法匹配和替换部分字符串的基础知识。这些都是非常简单的例子，但是我希望它仍然展示了哪怕一点点 regex 的强大。

这有帮助吗？你如何使用`.match()`、`.matchAll()`、`.replace()`和`.replaceAll()`方法？在[推特](https://twitter.com/kriskoishigawa)上让我知道。