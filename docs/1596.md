# JavaScript toLowerCase()——如何在 JS 中将字符串转换成小写和大写

> 原文：<https://www.freecodecamp.org/news/javascript-tolowercase-how-to-convert-a-string-to-lowercase-and-uppercase-in-js/>

本文解释了如何将字符串转换成小写和大写字符。

我们还将学习如何只将单词的首字母大写，以及如何将句子中每个单词的首字母大写。

我们开始吧！

## 如何在 JavaScript 中使用`toLowerCase()`方法

方法将一个字符串转换成小写字母。

该方法的一般语法如下所示:

```
String.toLowerCase() 
```

`toLowerCase()`方法不接受任何参数。

JavaScript 中的字符串是不可变的 T2。`toLowerCase()`方法将指定的字符串转换成仅由小写字母组成的新字符串，并返回该值。

这意味着旧的原始字符串不会以任何方式被更改或影响。

```
let myGreeting = 'Hey there!';

console.log(myGreeting.toLowerCase());

//output
//hey there! 
```

字符串`myGreeting`只由一个大写字母组成，它被转换成小写字母。

任何已经是小写的字母都不会受到`toLowerCase()`方法的影响，只会影响大写字母。这些信件保留了它们的原始形式。

以下示例中的字符串全部由大写字母组成。当应用`toLowerCase()`方法时，它们都被转换成小写。

```
const  anotherGreeting = 'GOOD MORNING!!';

console.log(anotherGreeting.toLowerCase());
//output
//good morning!! 
```

## 如何在 JavaScript 中使用`toUpperCase()`方法

`toUpperCase()`方法类似于`toLowerCase()`方法，但是它将字符串值转换成大写。

调用该方法的一般语法如下所示:

```
String.toUpper() 
```

它不接受任何参数。

由于 JavaScript 中的字符串是不可变的，`toLowerCase()`方法不会改变指定字符串的值。

而是返回一个新值。指定的字符串被转换为新的字符串，其内容仅由全部大写字母组成。这意味着现在将有两个字符串:原始字符串和新转换的大写字符串。

```
console.log('I am shouting!'.toUpperCase());

//output
//I AM SHOUTING! 
```

当调用`toLowerCase()`方法时，字符串中的任何大写字母都不会受到影响，并且保持不变。

### 如何在 JavaScript 中只大写字符串的第一个字母

如果你想让一个字符串的第一个字母成为大写字母呢？

下面是一个简单的例子，向您展示了一种方法。

假设有一个名为`myGreeting`的变量，字符串值为`hello`，全是小写字母。

```
let myGreeting = 'hello'; 
```

您首先通过使用其索引来定位并提取该字符串的第一个字母。然后对这个特定的字母调用`toUpperCase()`方法。

提醒一下，JavaScript(和大多数编程语言)中的索引是从`0`开始的，所以第一个字母的索引是`0`。

将该操作保存在一个名为`capFirstLetter`的新变量中。

```
let capFirstLetter = myGreeting[0].toUpperCase();

console.log(capFirstLetter);
// returns the letter 'H' in this case 
```

接下来，您希望隔离并删除第一个字符，保留字符串的其余部分。

一种方法是使用`slice()`方法。这将创建一个新字符串，从指定的索引开始，直到单词的末尾。

您希望从第二个字母开始，直到值的末尾。

在这种情况下，您应该传递给`slice()`的参数是`1`的索引，因为这是第二个字母的索引。

这样，第一个字符被完全排除。返回一个新字符串，不包含它，但包含剩余的字符-减去第一个字母。

然后将操作保存到一个新变量中。

```
let restOfGreeting = myGreeting.slice(1);

console.log(restOfGreeting);
//returns the string 'ello' 
```

通过连接将两个新变量组合在一起，可以得到一个只有第一个字母大写的新字符串。

```
let newGreeting = capFirstLetter + restOfGreeting;

console.log(newGreeting);
//Hello 
```

另一种方法是将上面的步骤组合起来，并将它们隔离在一个函数中。

该函数只创建一次。然后，该函数返回一个首字母大写的新字符串。

您需要编写的代码量大大减少，同时还能够将任何字符串作为参数传入，而无需编写重复的代码。

```
function capFirst(str) {
     return str[0].toUpperCase() + str.slice(1);
 }

console.log(capFirst('hello'));
//output 
//Hello 
```

### 如何将 JavaScript 中每个单词的首字母大写

但是如何让一个句子中每个单词的第一个字母大写呢？

上一节显示的方法不起作用，因为它不处理多个单词，只处理句子中的一个单词。

假设你有一个类似下面的句子。你要把句子中的每个单词都大写。

```
let learnCoding = 'learn to code for free with freeCodeCamp'; 
```

第一步是把句子分成单个的单词，然后分别处理每个单词。

为此，您使用`split()`方法并传递一个空格作为参数。这意味着句子中的每一个空格都被传递到一个新的数组中。

它根据空格分割句子。

创建一个新变量并存储新数组。

```
let splitLearnCoding = learnCoding.split(" ");

console.log(splitLearnCoding); 
//['learn', 'to', 'code', 'for', 'free', 'with', 'freeCodeCamp'] 
```

现在从那句话开始，有了一个新的单词数组，允许你单独操作每个单词。

因为现在有了一个新的数组，所以可以使用`map()`方法迭代数组中的每一项。

在`map()`方法中，您使用上一节中显示的相同过程来单独获取每个单词，大写第一个字母，然后返回单词的其余部分。

```
let capSplitLearnCoding = splitLearnCoding.map(word => {
    return word[0].toUpperCase() + word.slice(1);
})

console.log(capSplitLearnCoding);
//['Learn', 'To', 'Code', 'For', 'Free', 'With', 'FreeCodeCamp'] 
```

现在每个单词的第一个字母都是大写的。

现在剩下的就是将数组中的单词再次组合成一个句子。

为此，您可以使用`join()`方法并传递一个空格作为参数。

```
let learnCodingNew = capSplitLearnCoding.join(" ");

console.log(learnCodingNew);
//Learn To Code For Free With FreeCodeCamp 
```

如上一节所示，您还可以创建一个函数来组合所有这些步骤。然后，您将能够传递任何字符串作为参数，并且其中的每个第一个单词都是大写的。

```
function capFirstLetterInSentence(sentence) {
    let words = sentence.split(" ").map(word => {
        return word[0].toUpperCase() + word.slice(1);
    })
    return words.join(" ");
}

console.log(capFirstLetterInSentence("i am learning how to code"));
//I Am Learning How To Code 
```

## 结论

现在你知道了！这就是在 JavaScript 中使用`toLowerCase()`和`toUpperCase()`方法的方式。

你学习了如何将一个单词的首字母大写，以及句子中每个单词的首字母大写。

如果你想学习 JavaScript，更好的理解语言，freeCodeCamp 有[免费的 JavaScript 认证](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)。

您将作为语言的绝对初学者从基础开始，然后前进到更复杂的主题，如面向对象编程、函数式编程、数据结构、算法和有用的调试技术。

最后，您将构建五个项目来实践您的技能。

感谢阅读，祝学习愉快！