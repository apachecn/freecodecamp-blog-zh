# JavaScript 字符串方法终极指南- Split

> 原文：<https://www.freecodecamp.org/news/the-ultimate-guide-to-javascript-string-methods-split/>

`split()`方法根据作为输入传递的`separator`字符串，将原始字符串分成子字符串数组。原始字符串没有被`split()`修改。

## 句法

```
const splitStr = str.split(separator, limit);
```

*   `separator` -指示每个拆分应该发生的位置的字符串
*   `limit` -要查找的拆分数量的数字

## 示例:

```
const str = "Hello. I am a string. You can separate me.";
const splitStr = str.split("."); // Will separate str on each period character

console.log(splitStr); // [ "Hello", " I am a string", " You can separate me", "" ]
console.log(str); // "Hello. I am a string. You can separate me."
```

因为我们使用句点(`.`)作为`separator`字符串，所以输出数组中的字符串不包含句点——输出分隔字符串不包含输入`separator`本身。

您可以直接对字符串进行操作，而不用将它们存储为变量:

```
"Hello... I am another string... keep on learning!".split("..."); // [ "Hello", " I am another string", " keep on learning!" ]
```

此外，字符串分隔符不必是单个字符，它可以是任意字符组合:

```
const names = "Kratos- Atreus- Freya- Hela- Thor- Odin";
const namesArr = names.split("- "); // Notice that the separator is a dash and a space
const firstThreeNames = names.split("- ", 3);

console.log(namesArr) // [ "Kratos", "Atreus", "Freya", "Hela", "Thor", "Odin" ]
console.log(firstThreeNames); // [ "Kratos", "Atreus", "Freya" ]
```

## `split`的常见用途

一旦你掌握了基础知识，这个方法就非常有用。以下是`split()`的一些常见用例:

### 从句子中创建单词数组:

```
const sentence = "Ladies and gentlemen we are floating in space.";
const words = sentence.split(" "); // Split the sentence on each space between words

console.log(words); // [ "Ladies", "and", "gentlemen", "we", "are", "floating", "in", "space." ]
```

### 在单词中创建字母数组:

```
const word = "space";
const letters = word.split("");

console.log(letters); // [ "s", "p", "a", "c", "e" ]
```

### 颠倒单词中的字母:

因为`split()`方法返回一个数组，所以它可以与类似`reverse()`和`join()`的数组方法结合使用:

```
const word = "float";
const reversedWord = word.split("").reverse().join("");

console.log(reversedWord); // "taolf"
```

这就是你需要知道的一切，用最好的琴弦弹奏！