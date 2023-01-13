# JavaScript 中重复字符串的三种方式

> 原文：<https://www.freecodecamp.org/news/three-ways-to-repeat-a-string-in-javascript-2a9053b93a2d/>

在这篇文章中，我将解释如何解决 freeCodeCamp 的“[重复一串重复一串](https://www.freecodecamp.com/challenges/repeat-a-string-repeat-a-string)*”*挑战。这包括将一个字符串重复一定的次数。

我将介绍三种方法:

1.  使用 while 循环
2.  使用递归
3.  使用 ES6 重复()方法

### 算法挑战描述

> *重复给定的字符串`num`(第一个参数)次(第二个参数)。如果`num`不是正数，则返回空字符串。*

```
function repeatStringNumTimes(str, num) {
  return str;
}
repeatStringNumTimes("abc", 3);
```

### 提供的测试案例

```
repeatStringNumTimes("*", 3) should return "***".

repeatStringNumTimes("abc", 3) should return "abcabcabc".

repeatStringNumTimes("abc", 4) should return "abcabcabcabc".

repeatStringNumTimes("abc", 1) should return "abc".

repeatStringNumTimes("*", 8) should return "********".

repeatStringNumTimes("abc", -2) should return "".
```

### 方法 1:用 While 循环重复一个字符串

只要指定的条件评估为 true，while 语句就会执行其语句。

while 语句如下所示:

```
while (condition)
  statement
```

在每次通过循环之前评估一个条件。如果条件为真，则执行该语句。如果条件为假，则在 while 循环之后继续执行任何语句。

只要条件为真，就会执行该语句。解决方案如下:

```
 function repeatStringNumTimes(string, times) {
  // Step 1\. Create an empty string that will host the repeated string
  var repeatedString = "";

  // Step 2\. Set the While loop with (times > 0) as the condition to check
  while (times > 0) { // As long as times is greater than 0, the statement is executed
    // The statement
    repeatedString += string; // Same as repeatedString = repeatedString + string;
    times--; // Same as times = times - 1;
  }
  /* While loop logic
                      Condition       T/F       repeatedString += string      repeatedString        times
    First iteration    (3 > 0)        true            "" + "abc"                  "abc"               2
    Second iteration   (2 > 0)        true           "abc" + "abc"               "abcabc"             1
    Third iteration    (1 > 0)        true          "abcabc" + "abc"            "abcabcabc"           0
    Fourth iteration   (0 > 0)        false
    }
  */

  // Step 3\. Return the repeated string
  return repeatedString; // "abcabcabc"
}

repeatStringNumTimes("abc", 3);
```

同样，不加评论:

```
function repeatStringNumTimes(string, times) {
  var repeatedString = "";
  while (times > 0) {
    repeatedString += string;
    times--;
  }
  return repeatedString;
}
repeatStringNumTimes("abc", 3);
```

### 方法 2:使用条件和递归重复一个字符串

递归是一种迭代操作的技术，它让一个函数反复调用自己，直到得到一个结果。为了让递归正常工作，必须包含一些关键特性。

*   第一个是*:这是一个语句，通常在类似`if`的条件子句中，它停止递归。*
*   *第二个是一个 ***递归案例*** :这是递归函数自身被调用的语句。*

*解决方案如下:*

```
*`function repeatStringNumTimes(string, times) {
  // Step 1\. Check if times is negative and return an empty string if true
  if (times < 0) {
    return "";
  }

  // Step 2\. Check if times equals to 1 and return the string itself if it's the case.
  if (times === 1) {
    return string;
  }

  // Step 3\. Use recursion
  else {
    return string + repeatStringNumTimes(string, times - 1); // return "abcabcabc";
  }
  /* 
    First Part of the recursion method
    You need to remember that you won’t have just one call, you’ll have several nested calls
                     times       string + repeatStringNumTimes(string, times - 1)
      1st call         3                 "abc" + ("abc", 3 - 1)
      2nd call         2                 "abc" + ("abc", 2 - 1)
      3rd call         1                 "abc" => if (times === 1) return string;
      4th call         0                  ""   => if (times <= 0) return "";
    Second part of the recursion method
      4th call will return      ""
      3rd call will return     "abc"
      2nd call will return     "abc"
      1st call will return     "abc"
    The final call is a concatenation of all the strings
    return "abc" + "abc" + "abc"; // return "abcabcabc";
  */
}
repeatStringNumTimes("abc", 3);`*
```

*同样，不加评论:*

```
*`function repeatStringNumTimes(string, times) {
  if(times < 0) 
    return "";
  if(times === 1) 
    return string;
  else 
    return string + repeatStringNumTimes(string, times - 1);
}
repeatStringNumTimes("abc", 3);`*
```

### *方法 3:使用 ES6 repeat()方法重复一个字符串*

*对于此解决方案，您将使用 String.prototype.repeat()方法:*

*   *`**repeat()**`方法构造并返回一个新的字符串，该字符串包含调用它的字符串的指定数量的副本，这些副本连接在一起。*

*解决方案如下:*

```
 *`function repeatStringNumTimes(string, times) {
  //Step 1\. If times is positive, return the repeated string
  if (times > 0) { // (3 > 0) => true
    return string.repeat(times); // return "abc".repeat(3); => return "abcabcabc";
  }

  //Step 2\. Else if times is negative, return an empty string if true
  else {
    return "";
  }
}

repeatStringNumTimes("abc", 3);`*
```

*同样，不加评论:*

```
*`function repeatStringNumTimes(string, times) {
  if (times > 0)
    return string.repeat(times);
  else
    return "";
}
repeatStringNumTimes("abc", 3);`*
```

*您可以使用**三元运算符**作为 if/else 语句的快捷方式，如下所示:*

```
*`times > 0 ? string.repeat(times) : "";`*
```

*这可以理解为:*

```
*`if (times > 0) {    
    return string.repeat(times);
} else {
    return "";
}`*
```

*然后，您可以在函数中返回三元运算符:*

*我希望这能对你有所帮助。这是我关于 freeCodeCamp 算法挑战的“如何解决 FCC 算法”系列文章的一部分，其中我提出了几种解决方案，并一步一步地解释了幕后发生的事情。*

*[**JavaScript 中确认字符串结尾的两种方法**](https://medium.freecodecamp.com/two-ways-to-confirm-the-ending-of-a-string-in-javascript-62b4677034ac)
*[在本文中，我将解释如何解决 freeCodeCamp 的“确认结尾”挑战。](https://www.freecodecamp.org/news/two-ways-to-confirm-the-ending-of-a-string-in-javascript-62b4677034ac/)**

*[**JavaScript 中反串的三种方式**](https://medium.freecodecamp.com/how-to-reverse-a-string-in-javascript-in-3-different-ways-75e4763c68cb)
*[本文基于自由代码阵营的基本算法脚本“反串”](https://www.freecodecamp.org/news/how-to-reverse-a-string-in-javascript-in-3-different-ways-75e4763c68cb/)**

*[**JavaScript 中阶乘一个数的三种方法**](https://medium.freecodecamp.com/how-to-factorialize-a-number-in-javascript-9263c89a4b38)
*[本文基于自由代码阵营基本算法脚本“阶乘一个数”](https://www.freecodecamp.org/news/how-to-factorialize-a-number-in-javascript-9263c89a4b38/)**

***[JavaScript 中检查回文的两种方法](https://www.freecodecamp.org/news/two-ways-to-check-for-palindromes-in-javascript-64fea8191fd7/)**
*[本文基于自由代码阵营基本算法脚本“检查回文”。](https://www.freecodecamp.org/news/two-ways-to-check-for-palindromes-in-javascript-64fea8191fd7/)**

***[JavaScript 中查找字符串中最长单词的三种方法](https://www.freecodecamp.org/news/three-ways-to-find-the-longest-word-in-a-string-in-javascript-a2fb04c9757c/)**
*[本文基于自由代码营基本算法脚本“查找字符串中最长单词”。](https://www.freecodecamp.org/news/two-ways-to-check-for-palindromes-in-javascript-64fea8191fd7/)**

***[JavaScript 中三种给 Case 造句的方式](https://www.freecodecamp.org/news/three-ways-to-title-case-a-sentence-in-javascript-676a9175eb27/)**
*[本文是基于自由代码阵营的基本算法脚本“给 Case 造句”。](https://www.freecodecamp.org/news/three-ways-to-title-case-a-sentence-in-javascript-676a9175eb27/)**

*如果你有自己的解决方案或任何建议，请在下面的评论中分享。*

*或者你可以在 [**中**](https://medium.com/@sonya.moisset)**[Twitter](https://twitter.com/SonyaMoisset)[Github](https://github.com/SonyaMoisset)**和 [**LinkedIn**](https://www.linkedin.com/in/sonyamoisset) 上关注我，就在你点击下面的绿心之后；-)*

*∞天啊！*

### *额外资源*

*   *[while 循环— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/while)*
*   *[重复()方法— MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)*
*   *[递归— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions#Recursion)*
*   *[三元运算符— MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)*