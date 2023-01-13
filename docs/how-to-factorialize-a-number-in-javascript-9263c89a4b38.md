# JavaScript 中阶乘一个数的三种方法

> 原文：<https://www.freecodecamp.org/news/how-to-factorialize-a-number-in-javascript-9263c89a4b38/>

*本文基于自由代码阵营基本算法脚本“[阶乘一个数](https://www.freecodecamp.com/challenges/factorialize-a-number)”*

**在数学**中，非负整数 *n* 的阶乘可能是一个棘手的算法。在本文中，我将解释三种方法，第一种是递归函数，第二种是 while 循环，第三种是 for 循环。

在上一篇文章中我们已经看到了字符串的递归方法， [**如何在 JavaScript 中以 3 种不同的方式反转一个字符串？**](https://medium.com/@sonya.moisset/how-to-reverse-a-string-in-javascript-in-3-different-ways-75e4763c68cb#.ekpftot4d) 这次我们将把同样的概念应用在一个数字上。

#### 算法挑战

> 返回所提供整数的阶乘。
> 
> 如果整数用字母 n 表示，则一个阶乘是所有小于等于 n 的正整数的乘积
> 
> 阶乘常用简写符号 **n！**
> 
> 例如: **5！= 1 * 2 * 3 * 4 * 5 = 120**

```
 function factorialize(num) {
  return num;
}
factorialize(5);
```

#### *提供的测试用例*

*   ***【0】***阶乘应返回 1
*   *阶乘(5)应返回 120*
*   ****【10】****阶乘应返回 3628800**
*   *****【20】阶乘****应返回 2432902008176640000***

### ***什么是阶乘一个数字？***

***当你计算一个数的阶乘时，你是把这个数乘以每个连续的数减一。***

***如果你的数字是 5，你会有:***

```
***`5! = 5 * 4 * 3 * 2 * 1`***
```

***模式应该是:***

```
***`0! = 1
1! = 1
2! = 2 * 1
3! = 3 * 2 * 1
4! = 4 * 3 * 2 * 1
5! = 5 * 4 * 3 * 2 * 1`***
```

### ***1.用递归阶乘一个数***

```
***`function factorialize(num) {
  // If the number is less than 0, reject it.
  if (num < 0) 
        return -1;

  // If the number is 0, its factorial is 1.
  else if (num == 0) 
      return 1;

  // Otherwise, call the recursive procedure again
    else {
        return (num * factorialize(num - 1));
        /* 
        First Part of the recursion method
        You need to remember that you won’t have just one call, you’ll have several nested calls

        Each call: num === "?"        	         num * factorialize(num - 1)
        1st call – factorialize(5) will return    5  * factorialize(5 - 1) // factorialize(4)
        2nd call – factorialize(4) will return    4  * factorialize(4 - 1) // factorialize(3)
        3rd call – factorialize(3) will return    3  * factorialize(3 - 1) // factorialize(2)
        4th call – factorialize(2) will return    2  * factorialize(2 - 1) // factorialize(1)
        5th call – factorialize(1) will return    1  * factorialize(1 - 1) // factorialize(0)

        Second part of the recursion method
        The method hits the if condition, it returns 1 which num will multiply itself with
        The function will exit with the total value

        5th call will return (5 * (5 - 1))     // num = 5 * 4
        4th call will return (20 * (4 - 1))    // num = 20 * 3
        3rd call will return (60 * (3 - 1))    // num = 60 * 2
        2nd call will return (120 * (2 - 1))   // num = 120 * 1
        1st call will return (120)             // num = 120

        If we sum up all the calls in one line, we have
        (5 * (5 - 1) * (4 - 1) * (3 - 1) * (2 - 1)) = 5 * 4 * 3 * 2 * 1 = 120
        */
    }
}
factorialize(5);`***
```

#### ***无注释:***

```
***`function factorialize(num) {
  if (num < 0) 
        return -1;
  else if (num == 0) 
      return 1;
  else {
      return (num * factorialize(num - 1));
  }
}
factorialize(5);`***
```

### ***2.用 WHILE 循环阶乘一个数***

```
***`function factorialize(num) {
  // Step 1\. Create a variable result to store num
  var result = num;

  // If num = 0 OR num = 1, the factorial will return 1
  if (num === 0 || num === 1) 
    return 1; 

  // Step 2\. Create the WHILE loop 
  while (num > 1) { 
    num--; // decrementation by 1 at each iteration
    result = result * num; // or result *= num; 
    /* 
                    num           num--      var result      result *= num         
    1st iteration:   5             4            5             20 = 5 * 4      
    2nd iteration:   4             3           20             60 = 20 * 3
    3rd iteration:   3             2           60            120 = 60 * 2
    4th iteration:   2             1          120            120 = 120 * 1
    5th iteration:   1             0          120
    End of the WHILE loop 
    */
  }

  // Step 3\. Return the factorial of the provided integer
  return result; // 120
}
factorialize(5);`***
```

#### ***无注释:***

```
***`function factorialize(num) {
  var result = num;
  if (num === 0 || num === 1) 
    return 1; 
  while (num > 1) { 
    num--;
    result *= num;
  }
  return result;
}
factorialize(5);`***
```

### ***3.用 FOR 循环阶乘一个数***

```
***`function factorialize(num) {
  // If num = 0 OR num = 1, the factorial will return 1
  if (num === 0 || num === 1)
    return 1;

  // We start the FOR loop with i = 4
  // We decrement i after each iteration 
  for (var i = num - 1; i >= 1; i--) {
    // We store the value of num at each iteration
    num = num * i; // or num *= i;
    /* 
                    num      var i = num - 1       num *= i         i--       i >= 1?
    1st iteration:   5           4 = 5 - 1         20 = 5 * 4        3          yes   
    2nd iteration:  20           3 = 4 - 1         60 = 20 * 3       2          yes
    3rd iteration:  60           2 = 3 - 1        120 = 60 * 2       1          yes  
    4th iteration: 120           1 = 2 - 1        120 = 120 * 1      0          no             
    5th iteration: 120               0                120
    End of the FOR loop 
    */
  }
  return num; //120
}
factorialize(5);`***
```

#### ***无注释:***

```
***`function factorialize(num) {
  if (num === 0 || num === 1)
    return 1;
  for (var i = num - 1; i >= 1; i--) {
    num *= i;
  }
  return num;
}
factorialize(5);`***
```

***我希望这能对你有所帮助。这是我关于自由代码阵营算法挑战的“如何解决 FCC 算法”系列文章的一部分，其中我提出了几种解决方案，并一步一步地解释了幕后发生的事情。***

***[**JavaScript 中重复一个字符串的三种方式**
*在本文中，我将解释如何解决 freeCodeCamp 的“重复一个字符串重复一个字符串”挑战。这涉及…*](https://www.freecodecamp.org/news/three-ways-to-repeat-a-string-in-javascript-2a9053b93a2d/)***

**[**JavaScript 中确认字符串结尾的两种方法**
*在本文中，我将解释如何解决 freeCodeCamp 的“确认结尾”挑战。*](https://www.freecodecamp.org/news/two-ways-to-confirm-the-ending-of-a-string-in-javascript-62b4677034ac/)**

**[**JavaScript 中反串的三种方式**
*本文是基于自由代码阵营基本算法脚本的“反串”*](https://www.freecodecamp.org/news/how-to-reverse-a-string-in-javascript-in-3-different-ways-75e4763c68cb/)**

**[**JavaScript 中检查回文的两种方法**
*本文基于自由代码阵营基本算法脚本“检查回文”。*](https://www.freecodecamp.org/news/two-ways-to-check-for-palindromes-in-javascript-64fea8191fd7/)**

**[**JavaScript 中查找字符串中最长单词的三种方法**
*本文基于自由代码营基础算法脚本《查找字符串中最长单词》。*](https://www.freecodecamp.org/news/three-ways-to-find-the-longest-word-in-a-string-in-javascript-a2fb04c9757c/)**

**[**JavaScript 中三种给 Case 造句的方法**
*本文是基于自由代码阵营的基本算法脚本“给 Case 造句”。*](https://www.freecodecamp.org/news/three-ways-to-title-case-a-sentence-in-javascript-676a9175eb27/)**

**[**使用 JavaScript 找到数组中最大数的三种方法**
*在这篇文章中，我将解释如何解决自由代码营的“返回数组中最大数”挑战。本…*](https://medium.freecodecamp.com/three-ways-to-return-largest-numbers-in-arrays-in-javascript-5d977baa80a1)**

**如果你有自己的解决方案或任何建议，请在下面的评论中分享。**

**或者你可以在 [**中**](https://medium.com/@sonya.moisset)**[Twitter](https://twitter.com/SonyaMoisset)[Github](https://github.com/SonyaMoisset)**和 [**LinkedIn**](https://www.linkedin.com/in/sonyamoisset) 上关注我，就在你点击下面的绿心之后；-)**

**∞天啊！**