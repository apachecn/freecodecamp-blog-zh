# JavaScript 中给句子加标题的三种方法

> 原文：<https://www.freecodecamp.org/news/three-ways-to-title-case-a-sentence-in-javascript-676a9175eb27/>

*本文基于自由代码阵营基础算法脚本* [标题案例一句话](https://www.freecodecamp.com/challenges/title-case-a-sentence) *。*

**在这个算法**中，我们想改变一串文本，使它在每个单词的开头总是有一个大写字母。

在这篇文章中，我将解释三种方法。第一次使用 FOR 循环，第二次使用 map()方法，第三次使用 replace()方法。

#### 算法挑战

> 返回提供的字符串，每个单词的第一个字母大写。确保单词的其余部分是小写的。在这个练习中，你还应该大写连接词，如“the”和“of”。

#### 提供的测试案例

*   ***titleCase("我是小茶壶")*** 应该返回一个字符串。
*   ***titleCase(《我是小茶壶》)*** 应该返回“我是小茶壶”。
*   ***titleCase("矮胖")*** 应返回"矮胖"。
*   ***titleCase("这里是我的把手这里是我的壶嘴")*** 应该返回"这里是我的把手这里是我的壶嘴"。

### 1.带有 FOR 循环的句子

对于这个解决方案，我们将使用 String.prototype.toLowerCase()方法、String.prototype.split()方法、String.prototype.charAt()方法、String.prototype.slice()方法和 Array.prototype.join()方法。

*   **toLowerCase()** 方法返回转换成小写的调用字符串值
*   **split()** 方法通过将字符串分割成子字符串，将字符串对象分割成一个字符串数组。
*   方法从字符串中返回指定的字符。
*   **slice()** 方法提取一段字符串并返回一个新的字符串。
*   **join()** 方法将一个数组的所有元素连接成一个字符串。

我们需要在 **split()** 方法的括号之间添加一个空格，

```
var strSplit = "I'm a little tea pot".split(' ');
```

这将输出一组分隔的单词:

```
var strSplit = ["I'm", "a", "little", "tea", "pot"];
```

如果您不在括号中添加空格，您将得到以下输出:

```
var strSplit = ["I", "'", "m", " ", "a", " ", "l", "i", "t", "t", "l", "e", " ", "t", "e", "a", " ", "p", "o", "t"];
```

我们将连接

```
str[i].charAt(0).toUpperCase()
```

—它将在 FOR 循环中大写当前字符串的索引 0 字符—

和

```
str[i].slice(1)
```

—它将从索引 1 提取到字符串的末尾。

出于规范化的目的，我们将整个字符串设置为小写。

#### 带注释:

```
 function titleCase(str) {
  // Step 1\. Lowercase the string
  str = str.toLowerCase();
  // str = "I'm a little tea pot".toLowerCase();
  // str = "i'm a little tea pot";

  // Step 2\. Split the string into an array of strings
  str = str.split(' ');
  // str = "i'm a little tea pot".split(' ');
  // str = ["i'm", "a", "little", "tea", "pot"];

  // Step 3\. Create the FOR loop
  for (var i = 0; i < str.length; i++) {
    str[i] = str[i].charAt(0).toUpperCase() + str[i].slice(1); 
  /* Here str.length = 5
    1st iteration: str[0] = str[0].charAt(0).toUpperCase() + str[0].slice(1);
                   str[0] = "i'm".charAt(0).toUpperCase()  + "i'm".slice(1);
                   str[0] = "I"                            + "'m";
                   str[0] = "I'm";
    2nd iteration: str[1] = str[1].charAt(0).toUpperCase() + str[1].slice(1);
                   str[1] = "a".charAt(0).toUpperCase()    + "a".slice(1);
                   str[1] = "A"                            + "";
                   str[1] = "A";
    3rd iteration: str[2] = str[2].charAt(0).toUpperCase()   + str[2].slice(1);
                   str[2] = "little".charAt(0).toUpperCase() + "little".slice(1);
                   str[2] = "L"                              + "ittle";
                   str[2] = "Little";
    4th iteration: str[3] = str[3].charAt(0).toUpperCase() + str[3].slice(1);
                   str[3] = "tea".charAt(0).toUpperCase()  + "tea".slice(1);
                   str[3] = "T"                            + "ea";
                   str[3] = "Tea";
    5th iteration: str[4] = str[4].charAt(0).toUpperCase() + str[4].slice(1);
                   str[4] = "pot".charAt(0).toUpperCase() + "pot".slice(1);
                   str[4] = "P"                           + "ot";
                   str[4] = "Pot";                                                         
    End of the FOR Loop*/
  }

  // Step 4\. Return the output
  return str.join(' '); // ["I'm", "A", "Little", "Tea", "Pot"].join(' ') => "I'm A Little Tea Pot"
}

titleCase("I'm a little tea pot");
```

#### 无注释:

```
function titleCase(str) {
  str = str.toLowerCase().split(' ');
  for (var i = 0; i < str.length; i++) {
    str[i] = str[i].charAt(0).toUpperCase() + str[i].slice(1); 
  }
  return str.join(' ');
}
titleCase("I'm a little tea pot");
```

### 2.用 map()方法处理一个句子

对于这个解决方案，我们将使用 Array.prototype.map()方法。

*   **map()** 方法创建一个新的数组，其结果是对该数组中的每个元素调用一个提供的函数。使用 map 将为数组中的每个元素按顺序调用一次提供的回调函数，并根据结果构造一个新的数组。

在应用 map()方法之前，我们将小写并拆分字符串，如前一示例所示。

我们不使用 FOR 循环，而是将 map()方法作为条件应用于上一个示例中的同一个连接。

```
(word.charAt(0).toUpperCase() + word.slice(1));
```

#### 带注释:

```
 function titleCase(str) {
  // Step 1\. Lowercase the string
  str = str.toLowerCase() // str = "i'm a little tea pot";

  // Step 2\. Split the string into an array of strings
           .split(' ') // str = ["i'm", "a", "little", "tea", "pot"];

  // Step 3\. Map over the array
           .map(function(word) {
    return (word.charAt(0).toUpperCase() + word.slice(1));
    /* Map process
    1st word: "i'm"    => (word.charAt(0).toUpperCase() + word.slice(1));
                          "i'm".charAt(0).toUpperCase() + "i'm".slice(1);
                                "I"                     +     "'m";
                          return "I'm";
    2nd word: "a"      => (word.charAt(0).toUpperCase() + word.slice(1));
                          "a".charAt(0).toUpperCase()   + "".slice(1);
                                "A"                     +     "";
                          return "A";
    3rd word: "little" => (word.charAt(0).toUpperCase()    + word.slice(1));
                          "little".charAt(0).toUpperCase() + "little".slice(1);
                                "L"                        +     "ittle";
                          return "Little";
    4th word: "tea"    => (word.charAt(0).toUpperCase() + word.slice(1));
                          "tea".charAt(0).toUpperCase() + "tea".slice(1);
                                "T"                     +     "ea";
                          return "Tea";
    5th word: "pot"    => (word.charAt(0).toUpperCase() + word.slice(1));
                          "pot".charAt(0).toUpperCase() + "pot".slice(1);
                                "P"                     +     "ot";
                          return "Pot";                                                        
    End of the map() method */
});

 // Step 4\. Return the output
 return str.join(' '); // ["I'm", "A", "Little", "Tea", "Pot"].join(' ') => "I'm A Little Tea Pot"
}

titleCase("I'm a little tea pot");
```

#### 无注释:

```
function titleCase(str) {
  return str.toLowerCase().split(' ').map(function(word) {
    return (word.charAt(0).toUpperCase() + word.slice(1));
  }).join(' ');
}
titleCase("I'm a little tea pot");
```

### 3.使用 map()和 replace()方法的句子

对于这个解决方案，我们将继续使用 Array.prototype.map()方法，并添加 String.prototype.replace()方法。

*   **replace()** 方法返回一个新的字符串，其中一个模式的部分或全部匹配被替换。

在我们的例子中，replace()方法的模式将是一个被新的替换所替换的字符串，并将被视为一个逐字的字符串。我们也可以用一个**正则表达式**作为模式来求解这个算法。

在应用 map()方法之前，我们将小写并拆分字符串，如第一个示例所示。

#### 带注释:

```
 function titleCase(str) {
  // Step 1\. Lowercase the string
  str = str.toLowerCase() // str = "i'm a little tea pot";

  // Step 2\. Split the string into an array of strings
           .split(' ') // str = ["i'm", "a", "little", "tea", "pot"];

  // Step 3\. Map over the array
           .map(function(word) {
    return word.replace(word[0], word[0].toUpperCase());
    /* Map process
    1st word: "i'm" => word.replace(word[0], word[0].toUpperCase());
                       "i'm".replace("i", "I");
                       return word => "I'm"
    2nd word: "a" => word.replace(word[0], word[0].toUpperCase());
                     "a".replace("a", "A");
                      return word => "A"
    3rd word: "little" => word.replace(word[0], word[0].toUpperCase());
                          "little".replace("l", "L");
                          return word => "Little"
    4th word: "tea" => word.replace(word[0], word[0].toUpperCase());
                       "tea".replace("t", "T");
                       return word => "Tea"
    5th word: "pot" => word.replace(word[0], word[0].toUpperCase());
                       "pot".replace("p", "P");
                       return word => "Pot"                                                        
    End of the map() method */
});

 // Step 4\. Return the output
 return str.join(' '); // ["I'm", "A", "Little", "Tea", "Pot"].join(' ') => "I'm A Little Tea Pot"
}

titleCase("I'm a little tea pot");
```

#### 无注释:

```
function titleCase(str) {
  return str.toLowerCase().split(' ').map(function(word) {
    return word.replace(word[0], word[0].toUpperCase());
  }).join(' ');
}
titleCase("I'm a little tea pot");
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

[**JavaScript 中查找字符串中最长单词的三种方法**
*本文基于自由代码营基础算法脚本《查找字符串中最长单词》。*](https://www.freecodecamp.org/news/three-ways-to-find-the-longest-word-in-a-string-in-javascript-a2fb04c9757c/)

[**使用 JavaScript 找到数组中最大数的三种方法**
*在这篇文章中，我将解释如何解决自由代码营的“返回数组中最大数”挑战。本…*](https://www.freecodecamp.org/news/three-ways-to-return-largest-numbers-in-arrays-in-javascript-5d977baa80a1/)

如果你有自己的解决方案或任何建议，请在下面的评论中分享。

或者可以关注我的 [**中**](https://medium.com/@sonya.moisset)**[Twitter](https://twitter.com/SonyaMoisset)[Github](https://github.com/SonyaMoisset)**和[**LinkedIn**](https://www.linkedin.com/in/sonyamoisset)**。**

∞天啊！

### 资源

*   [toLowerCase()方法— MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase)
*   [toUpperCase()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase)
*   [charAt()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charAt)
*   [slice()方法— MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/String/slice)
*   [split()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split)
*   [join()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join)
*   [for — MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Statements/for)
*   [map()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
*   [replace()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace)