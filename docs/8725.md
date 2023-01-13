# JavaScript 中确认字符串结尾的两种方法

> 原文：<https://www.freecodecamp.org/news/two-ways-to-confirm-the-ending-of-a-string-in-javascript-62b4677034ac/>

在这篇文章中，我将解释如何解决 freeCodeCamp 的“[确认结局](https://www.freecodecamp.com/challenges/confirm-the-ending)*”*挑战。这包括检查字符串是否以特定的字符序列结尾。

我将介绍两种方法:

1.  使用 substr()方法
2.  使用 endsWith()方法

### 算法挑战描述

> *检查一个字符串(第一个参数，`str`)是否以给定的目标字符串(第二个参数，`target`)结束。*
> 
> *这个挑战可以用 ES2015 推出的`.endsWith()`方法解决。但是为了这个挑战的目的，我们希望您使用 JavaScript substring 方法。*

```
function confirmEnding(string, target) {
  return string;
}
confirmEnding("Bastian", "n");
```

### 提供的测试案例

```
confirmEnding("Bastian", "n") should return true.

confirmEnding("Connor", "n") should return false.

confirmEnding("Walking on water and developing software from a specification are easy if both are frozen", "specification") should return false.

largestOfFour([[4, 9, 1, 3], [13, 35, 18, 26], [32, 35, 97, 39], [1000000, 1001, 857, 1]]) should return [9, 35, 97, 1000000].

confirmEnding("He has to give me a new name", "name")should return true.
confirmEnding("Open sesame", "same") should return true.

confirmEnding("Open sesame", "pen") should return false.

confirmEnding("If you want to save our world, you must hurry. We dont know how much longer we can withstand the nothing", "mountain") should return false.

Do not use the built-in method .endsWith() to solve the challenge.
```

### 方法 1:用内置函数确认字符串的结尾——用 substr()

对于此解决方案，您将使用 String.prototype.substr()方法:

*   `**substr()**`方法返回从指定位置开始到指定字符数的字符串中的字符。

为什么用`string.substr(-target.length)`？

如果 target.length 为负，substr()方法将从字符串的末尾开始计数，这就是您在这个代码挑战中想要的。

您不希望使用`string.substr(-1)`来获取字符串的最后一个元素，因为如果目标长于一个字母:

```
confirmEnding("Open sesame", "same")
```

…目标根本不会回来。

所以这里`string.substr(-target.length)`将得到字符串‘Bastian’的最后一个索引，也就是‘n’。

然后你检查`string.substr(-target.length)`是否等于目标(真或假)。

```
 function confirmEnding(string, target) {
  // Step 1\. Use the substr method
  if (string.substr(-target.length) === target) {

  // What does "if (string.substr(-target.length) === target)" represents?
  // The string is 'Bastian' and the target is 'n' 
  // target.length = 1 so -target.length = -1
  // if ('Bastian'.substr(-1) === 'n')
  // if ('n' === 'n')

  // Step 2\. Return a boolean (true or false)
    return true;
  } else {
    return false;
  }
}

confirmEnding('Bastian', 'n');
```

无注释:

```
 function confirmEnding(string, target) {
  if (string.substr(-target.length) === target) {
    return true;
  } else {
    return false;
  }
}
confirmEnding('Bastian', 'n');
```

您可以使用**三元运算符**作为 if 语句的快捷方式:

```
(string.substr(-target.length) === target) ? true : false;
```

这可以理解为:

```
if (string.substr(-target.length) === target) {
    return true;
} else {
    return false;
}
```

然后在函数中返回三元运算符:

```
 function confirmEnding(string, target) {
  return (string.substr(-target.length) === target) ? true : false;
}
confirmEnding('Bastian', 'n');
```

您还可以通过返回以下条件来重构代码，使其更加简洁:

```
function confirmEnding(string, target) {
  return string.substr(-target.length) === target;
}
confirmEnding('Bastian', 'n');
```

### 方法 2:用内置函数确认字符串的结尾——用 endsWith()

对于这个解决方案，您将使用 String.prototype.endsWith()方法:

*   `endsWith()`方法确定一个字符串是否以另一个字符串的字符结尾，根据情况返回`true`或`false`。此方法区分大小写。

```
function confirmEnding(string, target) {
  // We return the method with the target as a parameter
  // The result will be a boolean (true/false)
  return string.endsWith(target); // 'Bastian'.endsWith('n')
}
confirmEnding('Bastian', 'n');
```

我希望这能对你有所帮助。这是我关于 freeCodeCamp 算法挑战的“如何解决 FCC 算法”系列文章的一部分，其中我提出了几种解决方案，并一步一步地解释了幕后发生的事情。

[**JavaScript 中重复一个字符串的三种方式**
*在本文中，我将解释如何解决 freeCodeCamp 的“重复一个字符串重复一个字符串”挑战。这涉及…*](https://www.freecodecamp.org/news/three-ways-to-repeat-a-string-in-javascript-2a9053b93a2d/)

[**JavaScript 中反串的三种方式**
*本文是基于自由代码阵营基本算法脚本的“反串”*](https://www.freecodecamp.org/news/how-to-reverse-a-string-in-javascript-in-3-different-ways-75e4763c68cb/)

[**JavaScript 中阶乘一个数的三种方法**
*本文基于自由代码阵营基本算法脚本“阶乘一个数”*](https://www.freecodecamp.org/news/how-to-factorialize-a-number-in-javascript-9263c89a4b38/)

[**JavaScript 中检查回文的两种方法**
*本文基于 Free Code Camp Basic 算法脚本“检查回文”。*](https://www.freecodecamp.org/news/two-ways-to-check-for-palindromes-in-javascript-64fea8191fd7/)

[**JavaScript 中查找字符串中最长单词的三种方法**
*本文基于自由代码营基础算法脚本《查找字符串中最长单词》。*](https://www.freecodecamp.org/news/three-ways-to-find-the-longest-word-in-a-string-in-javascript-a2fb04c9757c/)

[**JavaScript 中三种给 Case 造句的方法**
*本文是基于自由代码阵营的基本算法脚本“给 Case 造句”。*](https://www.freecodecamp.org/news/three-ways-to-title-case-a-sentence-in-javascript-676a9175eb27/)

如果你有自己的解决方案或任何建议，请在下面的评论中分享。

或者你可以在 [**中**](https://medium.com/@sonya.moisset)**[Twitter](https://twitter.com/SonyaMoisset)[Github](https://github.com/SonyaMoisset)**和 [**LinkedIn**](https://www.linkedin.com/in/sonyamoisset) 上关注我，就在你点击下面的绿心之后；-)

∞天啊！

### 额外资源

*   [substr()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr)
*   [endsWith()方法— MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith)
*   [三元运算符— MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)