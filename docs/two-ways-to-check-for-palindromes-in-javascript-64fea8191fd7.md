# JavaScript 中检查回文的两种方法

> 原文：<https://www.freecodecamp.org/news/two-ways-to-check-for-palindromes-in-javascript-64fea8191fd7/>

*本文基于自由代码营基础算法脚本“[检查回文](https://www.freecodecamp.com/challenges/check-for-palindromes)”。*

**回文**是一个单词、短语、数字或其他前后读起来相同的字符序列。“回文”一词最早是由英国剧作家[本·琼生](https://en.wikipedia.org/wiki/Ben_Jonson)在 17 世纪创造的，来自希腊语词根*佩林*(“再次”)和 *dromos* (“方式，方向”)。— *src。维基百科*

在本文中，我将解释两种方法，第一种是使用内置函数，第二种是使用 for 循环。

#### 算法挑战

> 如果给定的字符串是回文，则返回 true。否则，返回 false。回文是一个单词或句子向前和向后拼写都一样，忽略标点符号、大小写和间距。
> 
> **注。**你需要删除**所有非字母数字字符**(标点、空格和符号)，并把所有字符都变成小写，以便检查回文。
> 
> 我们将传递不同格式的字符串，比如“racecar”、“RaceCar”和“race CAR”等等。

```
function palindrome(str) {
  return true;
}
palindrome("eye");
```

#### *提供的测试用例*

*   ***回文(《赛车》)*** 应该还真
*   ***【回文】【非回文】*** 应返回假
*   ***回文(“一个人，一个计划，一条运河。【巴拿马】)***应该还真有
*   ***回文(《从来没有奇数或偶数》)*** 应该还真
*   ***回文【nope】***应该返回 false
*   ***回文【almostomla】***应该返回 false
*   ***回文(“我的年龄是 0，0 si ega ym。”)*** 应该还真有
*   ***回文(“1 只眼换 1 只眼”)*** 应假返回
*   ***回文(" 0 _ 0(:/-\:)0–0 ")***应返回真

### 我们将需要哪个**正则表达式**来通过最后一个测试用例？

正则表达式是用于匹配字符串中字符组合的模式。

当搜索匹配需要比直接匹配更多的东西时，该模式包括特殊字符。

```
To pass the last test case, we can use two Regular Expressions:

/[^A-Za-z0–9]/g  or

/[\W_]/g
```

**\W** 删除**所有非字母数字字符**:

*   **\W** 匹配任何非单词字符
*   **\W** 相当于【^a-za-z0–9_】
*   **\W** 匹配未括在括号中的任何内容

那是什么意思？

```
[^A-Z] matches anything that is not enclosed between A and Z

[^a-z] matches anything that is not enclosed between a and z

[^0-9] matches anything that is not enclosed between 0 and 9

[^_] matches anything that does not enclose _
```

但是在我们的测试用例中，我们需要回文("**0 _ 0(:/-\:)0–0**")来返回 **true** ，这意味着"**_(:/-\:)–【T5 ")必须匹配。**

我们将需要添加“ **_** ”来通过这个特定的测试用例。

```
We now have “\W_”
```

我们还需要为全局搜索添加 **g** 标志。

```
We finally have “/[\W_]/g”
```

> ***/[\W_]/g*** *纯粹是为了演示 RegExp 是如何工作的。**/[^a-za-z0–9]/g**是最容易选择的正则表达式**。***

### 1.用内置函数检查回文

对于此解决方案，我们将使用几种方法:

*   **toLowerCase()** 方法返回转换成小写的调用字符串值。
*   **replace()** 方法返回一个新的字符串，其中一个模式的部分或全部匹配被替换。我们将使用我们刚才创建的一个 RegExp。
*   **split()** 方法通过将字符串分割成子字符串，将字符串对象分割成一个字符串数组。
*   **reverse()** 方法在适当的位置反转数组。第一个数组元素成为最后一个，最后一个成为第一个。
*   **join()** 方法将一个数组的所有元素连接成一个字符串。

```
function palindrome(str) {
  // Step 1\. Lowercase the string and use the RegExp to remove unwanted characters from it
  var re = /[\W_]/g; // or var re = /[^A-Za-z0-9]/g;

  var lowRegStr = str.toLowerCase().replace(re, '');
  // str.toLowerCase() = "A man, a plan, a canal. Panama".toLowerCase() = "a man, a plan, a canal. panama"
  // str.replace(/[\W_]/g, '') = "a man, a plan, a canal. panama".replace(/[\W_]/g, '') = "amanaplanacanalpanama"
  // var lowRegStr = "amanaplanacanalpanama";

  // Step 2\. Use the same chaining methods with built-in functions from the previous article 'Three Ways to Reverse a String in JavaScript'
  var reverseStr = lowRegStr.split('').reverse().join(''); 
  // lowRegStr.split('') = "amanaplanacanalpanama".split('') = ["a", "m", "a", "n", "a", "p", "l", "a", "n", "a", "c", "a", "n", "a", "l", "p", "a", "n", "a", "m", "a"]
  // ["a", "m", "a", "n", "a", "p", "l", "a", "n", "a", "c", "a", "n", "a", "l", "p", "a", "n", "a", "m", "a"].reverse() = ["a", "m", "a", "n", "a", "p", "l", "a", "n", "a", "c", "a", "n", "a", "l", "p", "a", "n", "a", "m", "a"]
  // ["a", "m", "a", "n", "a", "p", "l", "a", "n", "a", "c", "a", "n", "a", "l", "p", "a", "n", "a", "m", "a"].join('') = "amanaplanacanalpanama"
  // So, "amanaplanacanalpanama".split('').reverse().join('') = "amanaplanacanalpanama";
  // And, var reverseStr = "amanaplanacanalpanama";

  // Step 3\. Check if reverseStr is strictly equals to lowRegStr and return a Boolean
  return reverseStr === lowRegStr; // "amanaplanacanalpanama" === "amanaplanacanalpanama"? => true
}

palindrome("A man, a plan, a canal. Panama");
```

#### 无注释:

```
function palindrome(str) {
  var re = /[\W_]/g;
  var lowRegStr = str.toLowerCase().replace(re, '');
  var reverseStr = lowRegStr.split('').reverse().join(''); 
  return reverseStr === lowRegStr;
}
palindrome("A man, a plan, a canal. Panama");
```

### 2.用 for 循环检查回文

半索引(len/2)在处理大字符串时有好处。我们检查每个部分的结尾，并将 FOR 循环中的迭代次数除以 2。

```
function palindrome(str) {
 // Step 1\. The first part is the same as earlier
 var re = /[^A-Za-z0-9]/g; // or var re = /[\W_]/g;
 str = str.toLowerCase().replace(re, '');

 // Step 2\. Create the FOR loop
 var len = str.length; // var len = "A man, a plan, a canal. Panama".length = 30

 for (var i = 0; i < len/2; i++) {
   if (str[i] !== str[len - 1 - i]) { // As long as the characters from each part match, the FOR loop will go on
       return false; // When the characters don't match anymore, false is returned and we exit the FOR loop
   }
   /* Here len/2 = 15
      For each iteration: i = ?    i < len/2    i++    if(str[i] !== str[len - 1 - i])?
      1st iteration:        0        yes         1     if(str[0] !== str[15 - 1 - 0])? => if("a"  !==  "a")? // false
      2nd iteration:        1        yes         2     if(str[1] !== str[15 - 1 - 1])? => if("m"  !==  "m")? // false      
      3rd iteration:        2        yes         3     if(str[2] !== str[15 - 1 - 2])? => if("a"  !==  "a")? // false  
      4th iteration:        3        yes         4     if(str[3] !== str[15 - 1 - 3])? => if("n"  !==  "n")? // false  
      5th iteration:        4        yes         5     if(str[4] !== str[15 - 1 - 4])? => if("a"  !==  "a")? // false
      6th iteration:        5        yes         6     if(str[5] !== str[15 - 1 - 5])? => if("p"  !==  "p")? // false
      7th iteration:        6        yes         7     if(str[6] !== str[15 - 1 - 6])? => if("l"  !==  "l")? // false
      8th iteration:        7        yes         8     if(str[7] !== str[15 - 1 - 7])? => if("a"  !==  "a")? // false
      9th iteration:        8        yes         9     if(str[8] !== str[15 - 1 - 8])? => if("n"  !==  "n")? // false
     10th iteration:        9        yes        10     if(str[9] !== str[15 - 1 - 9])? => if("a"  !==  "a")? // false
     11th iteration:       10        yes        11    if(str[10] !== str[15 - 1 - 10])? => if("c" !==  "c")? // false
     12th iteration:       11        yes        12    if(str[11] !== str[15 - 1 - 11])? => if("a" !==  "a")? // false
     13th iteration:       12        yes        13    if(str[12] !== str[15 - 1 - 12])? => if("n" !==  "n")? // false
     14th iteration:       13        yes        14    if(str[13] !== str[15 - 1 - 13])? => if("a" !==  "a")? // false
     15th iteration:       14        yes        15    if(str[14] !== str[15 - 1 - 14])? => if("l" !==  "l")? // false
     16th iteration:       15        no               
    End of the FOR Loop*/
 }
 return true; // Both parts are strictly equal, it returns true => The string is a palindrome
}

palindrome("A man, a plan, a canal. Panama");
```

#### 无注释:

```
function palindrome(str) {
 var re = /[^A-Za-z0-9]/g;
 str = str.toLowerCase().replace(re, '');
 var len = str.length;
 for (var i = 0; i < len/2; i++) {
   if (str[i] !== str[len - 1 - i]) {
       return false;
   }
 }
 return true;
}
palindrome("A man, a plan, a canal. Panama");
```

我希望这能对你有所帮助。这是我关于自由代码阵营算法挑战的“如何解决 FCC 算法”系列文章的一部分，其中我提出了几种解决方案，并一步一步地解释了幕后发生的事情。

[**JavaScript 中确认字符串结尾的两种方法**
*在本文中，我将解释如何解决 freeCodeCamp 的“确认结尾”挑战。*](https://www.freecodecamp.org/news/two-ways-to-confirm-the-ending-of-a-string-in-javascript-62b4677034ac/)

[**JavaScript 中反串的三种方式**
*本文是基于自由代码阵营基本算法脚本的“反串”*](https://www.freecodecamp.org/news/how-to-reverse-a-string-in-javascript-in-3-different-ways-75e4763c68cb/)

[**JavaScript 中阶乘一个数的三种方法**
*本文基于自由代码阵营基本算法脚本“阶乘一个数”*](https://www.freecodecamp.org/news/how-to-factorialize-a-number-in-javascript-9263c89a4b38/)

[**JavaScript 中查找字符串中最长单词的三种方法**
*本文基于自由代码营基础算法脚本《查找字符串中最长单词》。*](https://www.freecodecamp.org/news/three-ways-to-find-the-longest-word-in-a-string-in-javascript-a2fb04c9757c/)

[**JavaScript 中三种给 Case 造句的方法**
*本文是基于自由代码阵营的基本算法脚本“给 Case 造句”。*](https://www.freecodecamp.org/news/three-ways-to-title-case-a-sentence-in-javascript-676a9175eb27/)

[**使用 JavaScript 找到数组中最大数的三种方法**
*在这篇文章中，我将解释如何解决自由代码营的“返回数组中最大数”挑战。本…*](https://www.freecodecamp.org/news/three-ways-to-return-largest-numbers-in-arrays-in-javascript-5d977baa80a1/)

如果你有自己的解决方案或任何建议，请在下面的评论中分享。

或者你可以在 [**中**](https://medium.com/@sonya.moisset)**[Twitter](https://twitter.com/SonyaMoisset)[Github](https://github.com/SonyaMoisset)**和 [**LinkedIn**](https://www.linkedin.com/in/sonyamoisset) 上关注我，就在你点击下面的绿心之后；-)

∞天啊！

### 资源

*   [正则表达式— MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Guide/Regular_Expressions)
*   [toLowerCase()方法— MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase)
*   [replace() — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace)
*   [split()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split)
*   [反向()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)
*   [join()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join)
*   [String.length — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/length)
*   [for — MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Statements/for)