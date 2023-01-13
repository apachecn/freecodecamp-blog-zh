# JavaScript 中反转字符串的三种方法

> 原文：<https://www.freecodecamp.org/news/how-to-reverse-a-string-in-javascript-in-3-different-ways-75e4763c68cb/>

**反转字符串**是技术轮面试中最常被问到的 JavaScript 问题之一。面试官可能会要求您编写不同的方法来反转一个字符串，或者他们可能会要求您不使用内置方法来反转一个字符串，或者他们甚至会要求您使用递归来反转一个字符串。

除了内置的 **reverse** 函数之外，可能有数十种不同的方法来实现这一点，因为 JavaScript 没有这样的函数。

下面是我在 JavaScript 中解决反转字符串问题的三种最有趣的方法。注意，本文基于 freeCodeCamp 基本算法脚本“[反转一个字符串](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/basic-algorithm-scripting/reverse-a-string)”。

#### 算法挑战

> 反转提供的字符串。
> *你可能需要先把字符串变成数组，然后才能反转。*
> *你的结果必须是一个字符串。*

```
function reverseString(str) {
    return str;
}
reverseString("hello");
```

#### 提供的测试案例

*   ***reverseString(“你好”)*** 应该变成“olleh”
*   ***reverse string(“Howdy”)***应该变成“ydwoH”
*   ***reverseString("来自地球的问候")*** 应返回" htraE morf sgniteerG "

### 1.用内置函数反转字符串

对于这个解决方案，我们将使用三种方法:String.prototype.split()方法、Array.prototype.reverse()方法和 Array.prototype.join()方法。

*   split()方法通过将字符串分割成子字符串来将字符串对象分割成字符串数组。
*   reverse()方法原地反转数组。第一个数组元素成为最后一个，最后一个成为第一个。
*   join()方法将数组中的所有元素连接成一个字符串。

```
function reverseString(str) {
    // Step 1\. Use the split() method to return a new array
    var splitString = str.split(""); // var splitString = "hello".split("");
    // ["h", "e", "l", "l", "o"]

    // Step 2\. Use the reverse() method to reverse the new created array
    var reverseArray = splitString.reverse(); // var reverseArray = ["h", "e", "l", "l", "o"].reverse();
    // ["o", "l", "l", "e", "h"]

    // Step 3\. Use the join() method to join all elements of the array into a string
    var joinArray = reverseArray.join(""); // var joinArray = ["o", "l", "l", "e", "h"].join("");
    // "olleh"

    //Step 4\. Return the reversed string
    return joinArray; // "olleh"
}

reverseString("hello");
```

#### 将三种方法链接在一起:

```
function reverseString(str) {
    return str.split("").reverse().join("");
}
reverseString("hello");
```

### 2.用递减 For 循环反转字符串

```
function reverseString(str) {
    // Step 1\. Create an empty string that will host the new created string
    var newString = "";

    // Step 2\. Create the FOR loop
    /* The starting point of the loop will be (str.length - 1) which corresponds to the 
       last character of the string, "o"
       As long as i is greater than or equals 0, the loop will go on
       We decrement i after each iteration */
    for (var i = str.length - 1; i >= 0; i--) { 
        newString += str[i]; // or newString = newString + str[i];
    }
    /* Here hello's length equals 5
        For each iteration: i = str.length - 1 and newString = newString + str[i]
        First iteration:    i = 5 - 1 = 4,         newString = "" + "o" = "o"
        Second iteration:   i = 4 - 1 = 3,         newString = "o" + "l" = "ol"
        Third iteration:    i = 3 - 1 = 2,         newString = "ol" + "l" = "oll"
        Fourth iteration:   i = 2 - 1 = 1,         newString = "oll" + "e" = "olle"
        Fifth iteration:    i = 1 - 1 = 0,         newString = "olle" + "h" = "olleh"
    End of the FOR Loop*/

    // Step 3\. Return the reversed string
    return newString; // "olleh"
}

reverseString('hello');
```

#### 无注释:

```
function reverseString(str) {
    var newString = "";
    for (var i = str.length - 1; i >= 0; i--) {
        newString += str[i];
    }
    return newString;
}
reverseString('hello');
```

### 3.用递归反转字符串

对于这个解决方案，我们将使用两种方法:String.prototype.substr()方法和 String.prototype.charAt()方法。

*   substr()方法返回从指定位置开始到指定字符数的字符串中的字符。

```
"hello".substr(1); // "ello"
```

*   charAt()方法从字符串中返回指定的字符。

```
"hello".charAt(0); // "h"
```

递归的深度等于字符串的长度。这种解决方案不是最好的，如果字符串很长并且堆栈大小是主要关注点，那么它会非常慢。

```
function reverseString(str) {
  if (str === "") // This is the terminal case that will end the recursion
    return "";

  else
    return reverseString(str.substr(1)) + str.charAt(0);
/* 
First Part of the recursion method
You need to remember that you won’t have just one call, you’ll have several nested calls

Each call: str === "?"        	                  reverseString(str.subst(1))     + str.charAt(0)
1st call – reverseString("Hello")   will return   reverseString("ello")           + "h"
2nd call – reverseString("ello")    will return   reverseString("llo")            + "e"
3rd call – reverseString("llo")     will return   reverseString("lo")             + "l"
4th call – reverseString("lo")      will return   reverseString("o")              + "l"
5th call – reverseString("o")       will return   reverseString("")               + "o"

Second part of the recursion method
The method hits the if condition and the most highly nested call returns immediately

5th call will return reverseString("") + "o" = "o"
4th call will return reverseString("o") + "l" = "o" + "l"
3rd call will return reverseString("lo") + "l" = "o" + "l" + "l"
2nd call will return reverserString("llo") + "e" = "o" + "l" + "l" + "e"
1st call will return reverserString("ello") + "h" = "o" + "l" + "l" + "e" + "h" 
*/
}
reverseString("hello");
```

#### 无注释:

```
function reverseString(str) {
  if (str === "")
    return "";
  else
    return reverseString(str.substr(1)) + str.charAt(0);
}
reverseString("hello");
```

#### 条件(三元)运算符:

```
function reverseString(str) {
  return (str === '') ? '' : reverseString(str.substr(1)) + str.charAt(0);
}
reverseString("hello");
```

### 关于如何在 JavaScript 中反转字符串的视频说明

[https://scrimba.com/scrim/cV6mkPUg?embed=freecodecamp,mini-header,no-sidebar](https://scrimba.com/scrim/cV6mkPUg?embed=freecodecamp,mini-header,no-sidebar)

**在 JavaScript 中反转一个字符串**是一个小而简单的算法，可以在技术电话筛选或技术面试中被问到。你可以走捷径来解决这个问题，或者用递归或者更复杂的方法来解决这个问题。

我希望这能对你有所帮助。这是我关于自由代码阵营算法挑战的“如何解决 FCC 算法”系列文章的一部分，其中我提出了几种解决方案，并一步一步地解释了幕后发生的事情。

[**JavaScript 中重复一个字符串的三种方式**
*在本文中，我将解释如何解决 freeCodeCamp 的“重复一个字符串重复一个字符串”挑战。这涉及…*](https://www.freecodecamp.org/news/three-ways-to-repeat-a-string-in-javascript-2a9053b93a2d/)

[**JavaScript 中确认字符串结尾的两种方法**
*在本文中，我将解释如何解决 freeCodeCamp 的“确认结尾”挑战。*](https://www.freecodecamp.org/news/two-ways-to-confirm-the-ending-of-a-string-in-javascript-62b4677034ac/)

[**JavaScript 中阶乘一个数的三种方法**
*本文基于自由代码阵营基本算法脚本“阶乘一个数”*](https://www.freecodecamp.org/news/how-to-factorialize-a-number-in-javascript-9263c89a4b38/)

[**JavaScript 中检查回文的两种方法**
*本文基于自由代码阵营基本算法脚本“检查回文”。*](https://www.freecodecamp.org/news/two-ways-to-check-for-palindromes-in-javascript-64fea8191fd7/)

[**JavaScript 中查找字符串中最长单词的三种方法**
*本文基于自由代码营基础算法脚本《查找字符串中最长单词》。*](https://www.freecodecamp.org/news/three-ways-to-find-the-longest-word-in-a-string-in-javascript-a2fb04c9757c/)

[**JavaScript 中三种给 Case 造句的方法**
*本文是基于自由代码阵营的基本算法脚本“给 Case 造句”。*](https://www.freecodecamp.org/news/three-ways-to-title-case-a-sentence-in-javascript-676a9175eb27/)

如果你有自己的解决方案或任何建议，请在下面的评论中分享。

或者你可以在 [**中**](https://medium.com/@sonya.moisset)**[Twitter](https://twitter.com/SonyaMoisset)[Github](https://github.com/SonyaMoisset)**和 [**LinkedIn**](https://www.linkedin.com/in/sonyamoisset) 上关注我，就在你点击下面的绿心之后；-)

∞天啊！

### 资源

*   [split()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split)
*   [反向()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)
*   [join()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join)
*   [String.length — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/length)
*   [substr()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr)
*   [charAt()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charAt)