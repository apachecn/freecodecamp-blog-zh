# 无法读取未定义的属性“split”

> 原文：<https://www.freecodecamp.org/news/cannot-read-property-split-of-undefined-error/>

如果您曾经使用过 JavaScript 的`split`方法，很有可能会遇到下面的错误:`TypeError: Cannot read property 'split' of undefined`。

您会收到此错误有几个原因。很可能这只是对`split`如何工作以及如何遍历数组的基本误解。

例如，如果您尝试提交以下代码用于[查找字符串挑战](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/basic-algorithm-scripting/find-the-longest-word-in-a-string)中最长的单词:

```
function findLongestWord(str) { 
  for(let i = 0; i < str.length; i++) {
    const array = str.split(" ");
    array[i].split("");
  }
}

findLongestWord("The quick brown fox jumped over the lazy dog");
```

它将抛出`TypeError: Cannot read property 'split' of undefined`错误。

### `split`法

当在字符串上调用`split`时，它根据作为参数传入的分隔符将字符串分割成子字符串。如果一个空字符串作为参数传递，`split`将每个字符视为一个子字符串。然后，它返回一个包含子字符串的数组:

```
const testStr1 = "Test test 1 2";
const testStr2 = "cupcake pancake";
const testStr3 = "First,Second,Third";

testStr1.split(" "); // [ 'Test', 'test', '1', '2' ]
testStr2.split(""); // [ 'c', 'u', 'p', 'c', 'a', 'k', 'e', ' ', 'p', 'a', 'n', 'c', 'a', 'k', 'e' ]
testStr3.split(","); // [ 'First', 'Second', 'Third' ] 
```

查看 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) 了解更多关于`split`的细节。

### 用例子解释这个问题

知道`split`方法返回什么以及你能期望多少子串是解决[这个挑战](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/basic-algorithm-scripting/find-the-longest-word-in-a-string)的关键。

让我们再看一下上面的代码，看看为什么它不能像预期的那样工作:

```
function findLongestWord(str) { 
  for(let i = 0; i < str.length; i++) {
    const array = str.split(" ");
    array[i].split("");
  }
}

findLongestWord("The quick brown fox jumped over the lazy dog"); 
```

像这样把`str`分割成一个数组(`const array = str.split(" ");`)会像预期的那样工作并返回`[ 'The',   'quick',   'brown',   'fox',   'jumped',   'over',   'the',   'lazy',   'dog' ]`。

但是仔细看看`for`循环。不使用`array`的长度作为迭代`i`的条件，而是使用`str.length`。

`str`是“敏捷的棕色狐狸跳过了懒惰的狗”，如果你登录`str.length`到控制台，你会得到 44。

`for`循环体中的最后一条语句导致了错误:`array[i].split("");`。`array`的长度是 9，所以`i`会很快超过`array`的最大长度:

```
function findLongestWord(str) { 
  for(let i = 0; i < str.length; i++) {
    const array = str.split(" ");
    console.log(array[i]);
    // array[0]: "The"
    // array[1]: "quick"
    // array[2]: "brown"
    // ...
    // array[9]: "dog"
    // array[10]: undefined
    // array[11]: undefined
  }
}

findLongestWord("The quick brown fox jumped over the lazy dog"); 
```

调用`array[i].split("");`将每个字符串分割成子字符串是一种有效的方法，但是当它经过`undefined`时会抛出`TypeError: Cannot read property 'split' of undefined`。

### 如何用`split`求解查找字符串中最长的单词

让我们快速浏览一下如何解决这个问题的伪代码:

1.  将`str`分割成一个单词数组
2.  创建一个变量来跟踪最大单词长度
3.  遍历单词数组，将每个单词的长度与上面提到的变量进行比较
4.  如果当前单词的长度大于存储在变量中的长度，则用当前单词长度替换该值
5.  一旦每个单词的长度与最大单词长度变量进行了比较，就从函数中返回该数字

首先，将`str`分割成一个单词数组:

```
function findLongestWordLength(str) {
  const array = str.split(" ");
}
```

创建一个变量来跟踪最长的单词长度，并将其设置为零:

```
function findLongestWordLength(str) {
  const array = str.split(" ");
  let maxWordLength = 0;
}
```

现在`array`的值是`['The', 'quick', 'brown', 'fox', 'jumped', 'over', 'the', 'lazy', 'dog']`，你可以在你的`for`循环中使用`array.length`:

```
function findLongestWordLength(str) {
  const array = str.split(" ");
  let maxWordLength = 0;

  for (let i = 0; i < array.length; i++) {

  }
}
```

遍历单词数组，检查每个单词的长度。请记住，字符串也有一个`length`方法，您可以调用它来轻松获得字符串的长度:

```
function findLongestWordLength(str) {
  const array = str.split(" ");
  let maxLength = 0;

  for (let i = 0; i < array.length; i++) {
    array[i].length;
  }
}
```

使用`if`语句检查当前单词的长度(`array[i].length`)是否大于`maxLength`。如果是，用`array[i].length`替换`maxLength`的值:

```
function findLongestWordLength(str) {
  const array = str.split(" ");
  let maxLength = 0;

  for (let i = 0; i < array.length; i++) {
    if (array[i].length > maxLength) {
      maxLength = array[i].length;
    }
  }
}
```

最后，在函数结束时返回`maxLength`，在`for`循环之后:

```
function findLongestWordLength(str) {
  const array = str.split(" ");
  let maxLength = 0;

  for (let i = 0; i < array.length; i++) {
    if (array[i].length > maxLength) {
      maxLength = array[i].length;
    }
  }

  return maxLength;
}
```