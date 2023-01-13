# 在 JavaScript 中查找字符串中最长单词的三种方法

> 原文：<https://www.freecodecamp.org/news/three-ways-to-find-the-longest-word-in-a-string-in-javascript-a2fb04c9757c/>

*本文是基于自由代码营的基础算法脚本“* [找到字符串中最长的单词](https://www.freecodecamp.com/challenges/find-the-longest-word-in-a-string) *”。*

**在这个算法**中，我们想要查看每个单词，并计算每个单词中有多少个字母。然后，比较计数以确定哪个单词包含的字符最多，并返回最长单词的长度。

在这篇文章中，我将解释三种方法。第一次使用 FOR 循环，第二次使用 sort()方法，第三次使用 reduce()方法。

#### 算法挑战

> 返回所给句子中最长单词的长度。
> 
> 你的回答应该是一个数字。

#### ***提供测试用例***

*   ***【findlengestword】(那只敏捷的棕色狐狸跳过了那只懒狗)*** 应该返回一个数字
*   ***【findlengestword】(那只敏捷的棕色狐狸跳过了那只懒狗)*** 应该返回 6
*   ***findlengestword("愿原力与你同在")*** 应回 5
*   ***【findlengestword("谷歌做个桶滚")*** 应该返回 6
*   ***findlengest word("一只空载燕子的平均空速是多少")*** 应该返回 8
*   ***【findlengestword】(“如果我们尝试一个超长的词，比如耳鼻喉科怎么办”)*** 应该返回 19

```
function findLongestWord(str) {
  return str.length;
}
findLongestWord("The quick brown fox jumped over the lazy dog");
```

### 1.用 FOR 循环查找最长的单词

对于这个解决方案，我们将使用 String.prototype.split()方法

*   **split()** 方法通过将字符串分割成子字符串，将字符串对象分割成一个字符串数组。

我们需要在 **split()** 方法的括号之间添加一个空格，

```
var strSplit = “The quick brown fox jumped over the lazy dog”.split(‘ ‘);
```

这将输出一组分隔的单词:

```
var strSplit = [“The”, “quick”, “brown”, “fox”, “jumped”, “over”, “the”, “lazy”, “dog”];
```

如果您不在括号中添加空格，您将得到以下输出:

```
var strSplit = 
[“T”, “h”, “e”, “ “, “q”, “u”, “i”, “c”, “k”, “ “, “b”, “r”, “o”, “w”, “n”, “ “, “f”, “o”, “x”, “ “, “j”, “u”, “m”, “p”, “e”, “d”, “ “, “o”, “v”, “e”, “r”, “ “, “t”, “h”, “e”, “ “, “l”, “a”, “z”, “y”, “ “, “d”, “o”, “g”];
```

```
function findLongestWord(str) {
  // Step 1\. Split the string into an array of strings
  var strSplit = str.split(' ');
  // var strSplit = "The quick brown fox jumped over the lazy dog".split(' ');
  // var strSplit = ["The", "quick", "brown", "fox", "jumped", "over", "the", "lazy", "dog"];

  // Step 2\. Initiate a variable that will hold the length of the longest word
  var longestWord = 0;

  // Step 3\. Create the FOR loop
  for(var i = 0; i < strSplit.length; i++){
    if(strSplit[i].length > longestWord){ // If strSplit[i].length is greater than the word it is compared with...
	longestWord = strSplit[i].length; // ...then longestWord takes this new value
     }
  }
  /* Here strSplit.length = 9
     For each iteration: i = ?   i < strSplit.length?   i++  if(strSplit[i].length > longestWord)?   longestWord = strSplit[i].length
     1st iteration:        0             yes             1   if("The".length > 0)? => if(3 > 0)?     longestWord = 3
     2nd iteration:        1             yes             2   if("quick".length > 3)? => if(5 > 3)?   longestWord = 5   
     3rd iteration:        2             yes             3   if("brown".length > 5)? => if(5 > 5)?   longestWord = 5   
     4th iteration:        3             yes             4   if("fox".length > 5)? => if(3 > 5)?     longestWord = 5  
     5th iteration:        4             yes             5   if("jumped".length > 5)? => if(6 > 5)?  longestWord = 6 
     6th iteration:        5             yes             6   if("over".length > 6)? => if(4 > 6)?    longestWord = 6 
     7th iteration:        6             yes             7   if("the".length > 6)? => if(3 > 6)?     longestWord = 6
     8th iteration:        7             yes             8   if("lazy".length > 6)? => if(4 > 6)?    longestWord = 6 
     9th iteration:        8             yes             9   if("dog".length > 6)? => if(3 > 6)?     longestWord = 6 
     10th iteration:       9             no               
     End of the FOR Loop*/

  //Step 4\. Return the longest word
  return longestWord; // 6
}

findLongestWord("The quick brown fox jumped over the lazy dog");
```

#### 无注释:

```
function findLongestWord(str) {
  var strSplit = str.split(' ');
  var longestWord = 0;
  for(var i = 0; i < strSplit.length; i++){
    if(strSplit[i].length > longestWord){
	longestWord = strSplit[i].length;
     }
  }
  return longestWord;
}
findLongestWord("The quick brown fox jumped over the lazy dog");
```

### 2.用 sort()方法查找最长的单词

对于这个解决方案，我们将使用 Array.prototype.sort()方法按照某种排序标准对数组进行排序，然后返回该数组第一个元素的长度。

*   方法对数组中的元素进行排序并返回数组。

在我们的例子中，如果我们只是对数组排序

```
var sortArray = [“The”, “quick”, “brown”, “fox”, “jumped”, “over”, “the”, “lazy”, “dog”].sort();
```

我们将得到以下输出:

```
var sortArray = [“The”, “brown”, “dog”, “fox”, “jumped”, “lazy”, “over”, “quick”, “the”];
```

在 Unicode 中，数字在大写字母之前，大写字母在小写字母之前。

我们需要按照某种排序标准对元素进行排序，

```
[].sort(function(firstElement, secondElement) {     return secondElement.length — firstElement.length; })
```

其中将第二个元素的长度与数组中第一个元素的长度进行比较。

```
function findLongestWord(str) {
  // Step 1\. Split the string into an array of strings
  var strSplit = str.split(' ');
  // var strSplit = "The quick brown fox jumped over the lazy dog".split(' ');
  // var strSplit = ["The", "quick", "brown", "fox", "jumped", "over", "the", "lazy", "dog"];

  // Step 2\. Sort the elements in the array
  var longestWord = strSplit.sort(function(a, b) { 
    return b.length - a.length;
  });
  /* Sorting process
    a           b            b.length     a.length     var longestWord
  "The"      "quick"            5            3         ["quick", "The"]
  "quick"    "brown"            5            5         ["quick", "brown", "The"]  
  "brown"    "fox"              3            5         ["quick", "brown", "The", "fox"]
  "fox"      "jumped"           6            3         ["jumped", quick", "brown", "The", "fox"]
  "jumped"   "over"             4            6         ["jumped", quick", "brown", "over", "The", "fox"]
  "over"     "the"              3            4         ["jumped", quick", "brown", "over", "The", "fox", "the"]
  "the"      "lazy"             4            3         ["jumped", quick", "brown", "over", "lazy", "The", "fox", "the"]
  "lazy"     "dog"              3            4         ["jumped", quick", "brown", "over", "lazy", "The", "fox", "the", "dog"]
  */

  // Step 3\. Return the length of the first element of the array
  return longestWord[0].length; // var longestWord = ["jumped", "quick", "brown", "over", "lazy", "The", "fox", "the", "dog"];
                                // longestWord[0]="jumped" => jumped".length => 6
}

findLongestWord("The quick brown fox jumped over the lazy dog");
```

#### 无注释:

```
function findLongestWord(str) {
  var longestWord = str.split(' ').sort(function(a, b) { return b.length - a.length; });
  return longestWord[0].length;
}
findLongestWord("The quick brown fox jumped over the lazy dog");
```

### 3.用 reduce()方法找到最长的单词

对于这个解决方案，我们将使用 Array.prototype.reduce()。

*   **reduce()** 方法对一个累加器和数组中的每个值(从左到右)应用一个函数，将其减少为一个值。

reduce()对数组中的每个元素执行一次回调函数。

你可以提供一个初始值作为第二个参数来减少，这里我们将添加一个空字符串" "。

```
[].reduce(function(previousValue, currentValue) {...}, “”);
```

```
function findLongestWord(str) {
  // Step 1\. Split the string into an array of strings
  var strSplit = str.split(' ');
  // var strSplit = "The quick brown fox jumped over the lazy dog".split(' ');
  // var strSplit = ["The", "quick", "brown", "fox", "jumped", "over", "the", "lazy", "dog"];

  // Step 2\. Use the reduce method
  var longestWord = strSplit.reduce(function(longest, currentWord) {
    if(currentWord.length > longest.length)
       return currentWord;
    else
       return longest;
  }, "");

  /* Reduce process
  currentWord      longest       currentWord.length     longest.length    if(currentWord.length > longest.length)?   var longestWord
  "The"             ""                  3                     0                              yes                          "The"
  "quick"           "The"               5                     3                              yes                         "quick"
  "brown"           "quick"             5                     5                              no                          "quick"
  "fox"             "quick"             3                     5                              no                          "quick"
  "jumped"          "quick"             6                     5                              yes                         "jumped"
  "over"            "jumped"            4                     6                              no                          "jumped"
  "the"             "jumped"            3                     6                              no                          "jumped"
  "lazy"            "jumped"            4                     6                              no                          "jumped"
  "dog"             "jumped"            3                     6                              no                          "jumped"
  */

  // Step 3\. Return the length of the longestWord
  return longestWord.length; // var longestWord = "jumped" 
                             // longestWord.length => "jumped".length => 6
}

findLongestWord("The quick brown fox jumped over the lazy dog");
```

#### 无注释:

```
function findLongestWord(str) {
  var longestWord = str.split(' ').reduce(function(longest, currentWord) {
    return currentWord.length > longest.length ? currentWord : longest;
  }, "");
  return longestWord.length;
}
findLongestWord("The quick brown fox jumped over the lazy dog");
```

我希望这能对你有所帮助。这是我关于自由代码阵营算法挑战的“如何解决 FCC 算法”系列文章的一部分，其中我提出了几种解决方案，并一步一步地解释了幕后发生的事情。

[**JavaScript 中重复一个字符串的三种方式**
*在本文中，我将解释如何解决 freeCodeCamp 的“重复一个字符串重复一个字符串”挑战。这涉及…*](https://www.freecodecamp.org/news/three-ways-to-repeat-a-string-in-javascript-2a9053b93a2d/)

[**JavaScript 中确认字符串结尾的两种方法**
*在本文中，我将解释如何解决 freeCodeCamp 的“确认结尾”挑战。*](https://www.freecodecamp.org/news/two-ways-to-confirm-the-ending-of-a-string-in-javascript-62b4677034ac/)

[**JavaScript 中反串的三种方式**
*本文是基于自由代码阵营基本算法脚本的“反串”*](https://www.freecodecamp.org/news/how-to-reverse-a-string-in-javascript-in-3-different-ways-75e4763c68cb/)

[**JavaScript 中阶乘一个数的三种方法**
*本文基于自由代码阵营基本算法脚本“阶乘一个数”*](https://www.freecodecamp.org/news/how-to-factorialize-a-number-in-javascript-9263c89a4b38/)

[**JavaScript 中检查回文的两种方法**
*本文基于自由代码阵营基本算法脚本“检查回文”。*](https://www.freecodecamp.org/news/two-ways-to-check-for-palindromes-in-javascript-64fea8191fd7/)

[**JavaScript 中三种给 Case 造句的方法**
*本文是基于自由代码阵营的基本算法脚本“给 Case 造句”。*](https://www.freecodecamp.org/news/three-ways-to-title-case-a-sentence-in-javascript-676a9175eb27/)

[**使用 JavaScript 找到数组中最大数的三种方法**
*在这篇文章中，我将解释如何解决自由代码营的“返回数组中最大数”挑战。本…*](https://www.freecodecamp.org/news/three-ways-to-return-largest-numbers-in-arrays-in-javascript-5d977baa80a1/)

如果你有自己的解决方案或任何建议，请在下面的评论中分享。

或者你可以在 [**中**](https://medium.com/@sonya.moisset)**[Twitter](https://twitter.com/SonyaMoisset)[Github](https://github.com/SonyaMoisset)**和 [**LinkedIn**](https://www.linkedin.com/in/sonyamoisset) 上关注我，就在你点击下面的绿心之后；-)

∞天啊！

### 资源

*   [split()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split)
*   [sort()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
*   [reduce() — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
*   [String.length — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/length)
*   [for — MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Statements/for)