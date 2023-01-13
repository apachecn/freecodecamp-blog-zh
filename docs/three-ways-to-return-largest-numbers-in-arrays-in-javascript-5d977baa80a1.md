# 使用 JavaScript 有三种方法可以找到数组中最大的数字

> 原文：<https://www.freecodecamp.org/news/three-ways-to-return-largest-numbers-in-arrays-in-javascript-5d977baa80a1/>

在这篇文章中，我将解释如何解决自由代码营的"[返回数组中最大的数字](https://www.freecodecamp.com/challenges/return-largest-numbers-in-arrays) *"* 的挑战。这包括从每个子数组中返回一个数字最大的数组。

我将介绍三种方法:

1.  使用 FOR 循环
2.  使用 reduce()方法
3.  使用 Math.max()

#### 算法挑战描述

> 从每个提供的子数组中返回一个包含最大数字的数组。为简单起见，所提供的数组将正好包含 4 个子数组。
> 
> 记住，可以用简单的 for 循环遍历一个数组，用数组语法 arr[i]访问每个成员。

```
function largestOfFour(arr) {
  return arr;
}
largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]); 
```

#### 提供的测试案例

```
largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]) should return an array.

largestOfFour([[13, 27, 18, 26], [4, 5, 1, 3], [32, 35, 37, 39], [1000, 1001, 857, 1]]) should return [27,5,39,1001].

largestOfFour([[4, 9, 1, 3], [13, 35, 18, 26], [32, 35, 97, 39], [1000000, 1001, 857, 1]]) should return [9, 35, 97, 1000000].
```

### **方法 1:用 For 循环返回数组中最大的数字**

以下是我的解决方案，嵌入的注释有助于您理解它:

```
 function largestOfFour(arr) {
   // Step 1\. Create an array that will host the result of the 4 sub-arrays
   var largestNumber = [0,0,0,0];

   // Step 2\. Create the first FOR loop that will iterate through the arrays
   for(var arrayIndex = 0; arrayIndex < arr.length; arrayIndex++) {
   /* The starting point, index 0, corresponds to the first array */

    // Step 3\. Create the second FOR loop that will iterate through the sub-arrays
    for(var subArrayIndex = 0; subArrayIndex < arr[arrayIndex].length; subArrayIndex++) {
    /* The starting point, index 0, corresponds to the first sub-array */

       if(arr[arrayIndex][subArrayIndex] > largestNumber[arrayIndex]) {

          largestNumber[arrayIndex] = arr[arrayIndex][subArrayIndex];

       /* FOR loop cycles
          arrayIndex => i
          subArrayIndex => j

       Iteration in the first array
          For each iteration: arr[i][j]           largestNumber[i]          if arr[i][j] > largestNumber[i]?     then largestNumber[i] = arr[i][j]
          First iteration:    arr[0][0] => 4      largestNumber[0] => 0     4 > 0? => TRUE                       then largestNumber[0] = 4
          Second iteration:   arr[0][1] => 5      largestNumber[0] => 4     5 > 4? => TRUE                       then largestNumber[0] = 5
          Third iteration:    arr[0][2] => 1      largestNumber[0] => 5     1 > 5? => FALSE                      then largestNumber[0] = 5
          Fourth iteration:   arr[0][3] => 3      largestNumber[0] => 5     3 > 5? => FALSE                      then largestNumber[0] = 5
          Fifth iteration:    arr[0][4] => FALSE  largestNumber[0] => 5                                          largestNumber = [5,0,0,0]
       Exit the first array and continue on the second one
       Iteration in the second array
          For each iteration: arr[i][j]            largestNumber[i]           if arr[i][j] > largestNumber[i]?     then largestNumber[i] = arr[i][j]
          First iteration:    arr[1][0] => 13      largestNumber[1] => 0      13 > 0? => TRUE                      then largestNumber[1] = 13
          Second iteration:   arr[1][1] => 27      largestNumber[1] => 13     27 > 13? => TRUE                     then largestNumber[1] = 27
          Third iteration:    arr[1][2] => 18      largestNumber[1] => 27     18 > 27? => FALSE                    then largestNumber[1] = 27
          Fourth iteration:   arr[1][3] => 26      largestNumber[1] => 27     26 > 27? => FALSE                    then largestNumber[1] = 27
          Fifth iteration:    arr[1][4] => FALSE   largestNumber[1] => 27                                          largestNumber = [5,27,0,0]
       Exit the first array and continue on the third one
       Iteration in the third array
          For each iteration: arr[i][j]            largestNumber[i]           if arr[i][j] > largestNumber[i]?     then largestNumber[i] = arr[i][j]
          First iteration:    arr[2][0] => 32      largestNumber[2] => 0      32 > 0? => TRUE                      then largestNumber[2] = 32
          Second iteration:   arr[2][1] => 35      largestNumber[2] => 32     35 > 32? => TRUE                     then largestNumber[2] = 35
          Third iteration:    arr[2][2] => 37      largestNumber[2] => 35     37 > 35? => TRUE                     then largestNumber[2] = 37
          Fourth iteration:   arr[2][3] => 39      largestNumber[2] => 37     39 > 37? => TRUE                     then largestNumber[2] = 39
          Fifth iteration:    arr[2][4] => FALSE   largestNumber[2] => 39                                          largestNumber = [5,27,39,0]
       Exit the first array and continue on the fourth one
       Iteration in the fourth array
          For each iteration: arr[i][j]            largestNumber[i]           if arr[i][j] > largestNumber[i]?     then largestNumber[i] = arr[i][j]
          First iteration:    arr[3][0] => 1000    largestNumber[3] => 0      1000 > 0? => TRUE                    then largestNumber[3] = 1000
          Second iteration:   arr[3][1] => 1001    largestNumber[3] => 1000   1001 > 1000? => TRUE                 then largestNumber[3] = 1001
          Third iteration:    arr[3][2] => 857     largestNumber[3] => 1001   857 > 1001? => FALSE                 then largestNumber[3] = 1001
          Fourth iteration:   arr[3][3] => 1       largestNumber[3] => 1001   1 > 1001? => FALSE                   then largestNumber[3] = 1001
          Fifth iteration:    arr[3][4] => FALSE   largestNumber[3] => 1001                                        largestNumber = [5,27,39,1001]
       Exit the FOR loop */
        }
    }
 }
 // Step 4\. Return the largest numbers of each sub-arrays
 return largestNumber; // largestNumber = [5,27,39,1001];
}

largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]);
```

这里没有我的评论:

```
 function largestOfFour(arr) {
   var largestNumber = [0,0,0,0];
   for(var arrayIndex = 0; arrayIndex < arr.length; arrayIndex++) {
    for(var subArrayIndex = 0; subArrayIndex < arr[arrayIndex].length; subArrayIndex++) {
       if(arr[arrayIndex][subArrayIndex] > largestNumber[arrayIndex]) {         
          largestNumber[arrayIndex] = arr[arrayIndex][subArrayIndex];
        }
    }
 }
 return largestNumber;
}
largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]);
```

### 方法 2:用内置函数返回数组中最大的数字——用 map()和 reduce()

对于这个解决方案，您将使用两种方法:Array.prototype.map()方法和 Array.prototype.reduce()方法。

*   **map()** 方法创建一个新的数组，其结果是对该数组中的每个元素调用一个提供的函数。使用 map 将为数组中的每个元素按顺序调用一次提供的回调函数，并根据结果构造一个新的数组。
*   **reduce()** 方法对一个累加器和数组中的每个值应用一个函数，以将其缩减为一个值。

**三元运算符**是唯一一个接受三个操作数的 JavaScript 运算符。该运算符用作 if 语句的快捷方式。

```
(currentLargestNumber > previousLargestNumber) ? currentLargestNumber : previousLargestNumber;
```

这也可以理解为:

```
if (currentLargestNumber > previousLargestNumber == true) {
    return currentLargestNumber;
} else {
    return previousLargestNumber;
}
```

以下是我的解决方案，带有嵌入式注释:

```
 function largestOfFour(mainArray) {
  // Step 1\. Map over the main arrays
  return mainArray.map(function (subArray){ // Step 3\. Return the largest numbers of each sub-arrays => returns [5,27,39,1001]

    // Step 2\. Grab the largest numbers for each sub-arrays with reduce() method
    return subArray.reduce(function (previousLargestNumber, currentLargestNumber) {

      return (currentLargestNumber > previousLargestNumber) ? currentLargestNumber : previousLargestNumber;

      /* Map process and Reduce method cycles
      currentLargestNumber => cLN
      previousLargestNumber => pLN
      Iteration in the first array
          For each iteration:     cLN         pLN       if (cLN > pLN) ?        then cLN        else pLN
          First iteration:         4           0        4 > 0? => TRUE              4             /
          Second iteration:        5           4        5 > 4? => TRUE              5             /
          Third iteration:         1           5        1 > 5? => FALSE             /             5
          Fourth iteration:        3           5        3 > 5? => FALSE             /             5
          Fifth iteration:         /           5                                               returns 5
       Exit the first array and continue on the second one
      Iteration in the second array
        For each iteration:     cLN         pLN       if (cLN > pLN) ?        then cLN        else pLN
        First iteration:        13           0        13 > 0? => TRUE            13              /
        Second iteration:       27          13        27 > 13? => TRUE           27              /
        Third iteration:        18          27        18 > 27? => FALSE           /             27
        Fourth iteration:       26          27        26 > 27? => FALSE           /             27
        Fifth iteration:         /          27                                                returns 27
      Exit the first array and continue on the third one
      Iteration in the third array
        For each iteration:     cLN         pLN       if (cLN > pLN) ?        then cLN        else pLN
        First iteration:        32           0        32 > 0? => TRUE            32              /
        Second iteration:       35          32        35 > 32? => TRUE           35              /
        Third iteration:        37          35        37 > 35? => TRUE           37              /
        Fourth iteration:       39          37        39 > 37? => TRUE           39              /
        Fifth iteration:         /          39                                                returns 39
      Exit the first array and continue on the fourth one
      Iteration in the fourth array
        For each iteration:     cLN         pLN       if (cLN > pLN) ?        then cLN        else pLN
        First iteration:        1000         0        1000 > 0? => TRUE         1000             /
        Second iteration:       1001       1000       1001 > 1000? => TRUE      1001             /
        Third iteration:        857        1001       857 > 1001 => FALSE        /             1001
        Fourth iteration:        1         1001       1 > 1001? => FALSE         /             1001
        Fifth iteration:         /         1001                                              returns 1001
      Exit the first array and continue on the fourth one */
    }, 0); // 0 serves as the context for the first pLN in each sub array
  });
}

largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]);
```

这是没有注释的:

```
 function largestOfFour(mainArray) {
  return mainArray.map(function (subArray){
    return subArray.reduce(function (previousLargestNumber, currentLargestNumber) {
      return (currentLargestNumber > previousLargestNumber) ? currentLargestNumber : previousLargestNumber;
    }, 0);
  });
}
largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]);
```

### 方法 3:用内置函数返回数组中最大的数字——用 map()和 apply()

对于这个解决方案，您将使用两种方法:Array.prototype.map()方法和 Function.prototype.apply()方法。

*   **apply()** 方法使用给定的 this 值和作为数组(或类似数组的对象)提供的参数调用函数。

您可以通过使用 **apply()** 方法将参数数组传递给函数，函数将执行数组中的项目。

这样的函数被称为**可变函数**，它们可以接受任意数量的参数，而不是固定的一个。

**Math.max()** 函数返回零个或多个数字中的最大值，我们可以传递任意数量的参数。

```
console.log(Math.max(4,5,1,3)); // logs 5
```

但是您不能像这样将一个数字数组传递给该方法:

```
var num = [4,5,1,3];
console.log(Math.max(num)); // logs NaN
```

这就是 **apply()** 方法有用的地方:

```
var num = [4,5,1,3];
console.log(Math.max.apply(null, num)); // logs 5
```

注意， **apply()** 的第一个参数设置了“ **this** ”的值，在这个方法中没有使用，所以传递了 **null** 。

既然有了返回数组中最大数字的方法，就可以用 **map()** 方法遍历每个子数组并返回所有最大数字。

以下是我的解决方案，带有嵌入式注释:

```
 function largestOfFour(mainArray) {
  // Step 1\. Map over the main arrays
  return mainArray.map(function(subArray) { // Step 3\. Return the largest numbers of each sub-arrays => returns [5,27,39,1001]

    // Step 2\. Return the largest numbers for each sub-arrays with Math.max() method
    return Math.max.apply(null, subArray);
  });
}

largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]);
```

没有评论:

```
 function largestOfFour(mainArray) {
  return mainArray.map(function(subArray) {
    return Math.max.apply(null, subArray);
  });
}
largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]);
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

[**JavaScript 中三种给 Case 造句的方法**
*本文是基于自由代码阵营的基本算法脚本“给 Case 造句”。*](https://www.freecodecamp.org/news/three-ways-to-title-case-a-sentence-in-javascript-676a9175eb27/)

如果你有自己的解决方案或任何建议，请在下面的评论中分享。

或者你可以在 [**中**](https://medium.com/@sonya.moisset)**[Twitter](https://twitter.com/SonyaMoisset)[Github](https://github.com/SonyaMoisset)**和 [**LinkedIn**](https://www.linkedin.com/in/sonyamoisset) 上关注我，就在你点击下面的绿心之后；-)

∞天啊！

### 额外资源

*   [for — MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Statements/for)
*   [array.length — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/length)
*   [map()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
*   [reduce()方法— MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)
*   [三元运算符— MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)
*   [apply()方法— MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
*   [Math.max() — MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/max)
*   [this — MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/this)