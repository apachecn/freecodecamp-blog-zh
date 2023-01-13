# 揭开动态编程的神秘面纱

> 原文：<https://www.freecodecamp.org/news/demystifying-dynamic-programming-24fbdb831d3a/>

作者:Prajwal M

有很多关于如何成为软件开发人员的优质文章。他们教你编程、开发和使用库。但是在**算法**和**数据结构** *的教育方面却做得很少。*不管你有多擅长开发，没有**算法**和**数据结构** *、*的知识，你就不能被录用*。*

学习流行的算法像**矩阵链乘法，背包或者旅行商** **算法**是不够的。面试官问的问题就像你在竞争性编程网站上发现的问题。要解决这样的问题，你需要对这些概念有一个很好的、坚定的理解。

#### **什么是动态编程？**

根据[维基百科](https://en.wikipedia.org/wiki/Dynamic_programming)的说法，动态编程是通过以[递归](https://en.wikipedia.org/wiki/Recursion)的方式将复杂的问题分解成更简单的子问题来简化它。这篇文章将教你:

```
-> Identify subproblems
-> Learn how to solve subproblems
-> Identify that subproblems are repetitive 
-> Identify that subproblems have optimal substructure property
-> Learn to cache/store results of sub problems
-> Develop a recursive relation to solve the problem
-> Use top-down and bottom-up approach to solve the problem
```

#### 我将使用哪种语言？

我知道大多数人精通或者有 JavaScript 编码的经验。另外，一旦你学会了 JavaScript，就很容易把它转换成 Java 代码。Python 或 C++也是如此。诀窍是用你最喜欢的语言理解问题。因此我选择使用 JavaScript。

这篇文章是关于算法的，更具体地说是关于动态编程的。这通常被认为是一个棘手的话题。如果你坚持到这篇文章的结尾，我相信你可以自己解决许多动态编程问题？。

### **问题陈述**

```
Problem: Given an integer n, find the minimum number of steps to reach integer 1.

At each step, you can:

Subtract 1,

Divide by 2, if it is divisible by 2

Divide by 3, if it is divisible by 3
```

所有的动态编程问题都有一个**开始状态。Y** 你必须通过**转变**经过**个中间状态**来达到**目标**。在一本典型的教科书中，你会经常听到术语**子问题**。它与**状态**相同。这些术语可以互换使用。在本文中，我将使用术语状态，而不是术语子问题。

什么是子问题或状态？子问题/状态是原问题的一个较小的实例。用于解决原问题和子问题的方法是相同的。

> 有些问题会给你指定状态转换的规则。这就是这样一个问题。这个问题说你可以从 n 开始移动到 n-1，n/2 或 n/3。另一方面，有一些问题不会指定状态转换。你将不得不自己去发现它们。我将在另一篇文章中讨论这类问题。

这里，

```
Start state -> n
Goal -> 1
Intermediate states -> any integer number between 1 and n
```

给定一个状态(开始或中间)，**你总是可以移动到固定数量的状态。**

```
from n you can move to :

n -> n-1 

if n % 2 == 0:
   n -> n/2

if n % 3 == 0:
   n -> n/3

example:

from 3 you can move to,
3 -> 3-1 = 2
3 -> 3/3 = 1

from 4 you can move to,
4 -> 4-1 = 3
4 -> 4/2 = 2
```

在动态规划优化问题*，*中，你必须确定**从起点到目标的哪个状态会给你一个最优解。**

```
For n = 4:

approach one:
4 -> 3 -> 2 -> 1

approach two:
4 -> 2 -> 1 

approach three:
4 -> 3 -> 1
```

这里，在三种方法中，方法二和方法三是最佳的，因为它们需要最少的移动/转换。方法一是最糟糕的，因为它需要更多的移动。

#### **教科书术语解释**

```
Repetitive subproblems : You will end up solving the same problem more than once.

for n = 5
example:
5 -> 4 -> 3 -> 1
5 -> 4 -> 2 -> 1
5 -> 4 -> 3 -> 2 -> 1

observe here that 2 -> 1 occurs two times. 
also observe that 5 -> 4 occurs three times.

Optimal Substructure : Optimal solutions to subproblems give optimal solution to the entire problem

example:
2 -> 1 is optimal 
3 -> 1 is optimal 

when I am at 4,
4 -> 3 -> 2 -> 1 and 4 -> 3 -> 1 is possible
but the optimal solution of 4 is 4 -> 3 -> 1\. The optimal solution of four comes from optimal solution of three (3 -> 1).

similarly,
4 -> 3 -> 2 -> 1 and 4 -> 2 -> 1 is possible
but the optimal solution of 4 is 4 -> 2 -> 1\. The optimal solution of four comes from optimal solution of two (2 -> 1).

now 5,
The optimal solution of 5 depends on optimal solution to 4.
5 -> 4 -> 2 -> 1 and 5 -> 4 -> 3 -> 1 are optimal.
```

你应该如何利用重复的子问题和最优子结构来为我们服务？

> 我们将只解决一次子问题，并以最优方式解决每个子问题。

```
we will solve the subproblems 3 -> 1 and 2 -> 1 only once and optimally.

Now for 4 we will solve only once by 4 -> 3 -> 1 and optimally. You can also solve as 4 -> 2 -> 1 but that is left to you. 

Finally for 5 we will solve only once by 5 - > 4 -> 3 -> 1 and optimally.
```

在实践中，您将使用一个数组来存储子问题的最优结果。这样，当你不得不再次求解子问题时，你可以从数组中得到值，而不是再次求解。本质上，你现在只解决了一次子问题。

#### **如何衡量最优性**

通过使用一个叫做**的东西，花费**。从一个状态转移到另一个状态总是有代价的。成本可以是零或一个有限的数字。给出最优成本的一组移动/转换就是最优解。

```
In 5 -> 4 -> 3 -> 1 
for 5 -> 4 cost is 1 
for 4 -> 3 cost is 1
for 3 -> 1 cost is 1

The total cost of 5 -> 4 -> 3 -> 1 is the total sum of 3.

In In 5 -> 4 -> 3 -> 2 -> 1
for 5 -> 4 cost is 1
for 4 -> 3 cost is 1 
for 3 -> 2 cost is 1
for 2 -> 1 cost is 1
The total cost of 5 -> 3 -> 2 -> 1 is the total sum of 4.

The optimal solution of 5 -> 4 -> 3 -> 1 has a cost of three which is the minimum. Hence we can see that optimal solutions have optimal costs
```

**递归关系:**所有的动态规划问题都有递归关系。**一旦你定义了一个递归关系，解决方法就是把它翻译成代码。**

```
For the above problem, let us define minOne as the function that we will use to solve the problem and the cost of moving from one state to another as 1.

if n = 5,
solution to 5 is cost + solution to 4
recursive formulae/relation is 
minOne(5) = 1 + minOne(4) 

Similarly,
if n = 6,
recursive formulae/relation is
minOne(6) = min(             
              1 + minOne(5),
              1 + minOne(3),
              1 + minOne(2) )
```

### **代码**

动态编程问题可以通过**自顶向下**的方法或者**自底向上**的方法来解决。

```
Top Down : Solve problems recursively. 
for n = 5, you will solve/start from 5, that is from the top of the problem.
It is a relatively easy approach provided you have a firm grasp on recursion. I say that this approach is easy as this method is as simple as transforming your recursive relation into code.

Bottom Up : Solve problems iteratively.
for n = 5, you will solve/start from 1, that is from the bottom of the problem.
This approach uses a for loop. It does not lead to stack overflow as in recursion. This approach is also slightly more optimal.
```

#### **哪种方法更好？**

这取决于你的舒适程度。两者给出了相同的解决方案。在非常大的问题中，自底向上是有益的，因为它不会导致堆栈溢出。如果您选择输入 10000，自顶向下的方法将给出超出的最大调用堆栈大小，但是自底向上的方法将给出解决方案。

但是请记住，你不能完全消除递归思维。无论使用何种方法，您都必须定义一个递归关系。

#### **自下而上的方法**

```
/*
Problem: Given an integer n, find the minimum number of steps to reach integer 1.
At each step, you can:
Subtract 1,
Divide by 2, if it is divisible by 2 
Divide by 3, if it is divisible by 2 
*/

// bottom-up
function minOneBottomUp(n) {

    const cache = [];
    // base condition
    cache[1] = 0;

    for (i = 2; i <= n; i++) {

        // initialize a , b and c to some very large numbers
        let a = 1000, b = 1000, c = 1000;

        // one step from i -> i-1
        a = 1 + cache[i - 1];

        // one step from i -> i/2 if i is divisible by 2
        if (i % 2 === 0) {
            b = 1 + cache[i / 2];
        }

        // one step from i -> i/3 if i is divisible by 3
        if (i % 3 === 0) {
            c = 1 + cache[i / 3];
        }

        // Store the minimum number of steps to reach i
        cache[i] = Math.min(a, b, c);
    }

    // return the number minimum number of steps to reach n
    return cache[n];
}

console.log(minOneBottomUp(1000));
```

```
Line 11 : The function that will solve the problem is named as minOneBottomUp. It takes n as the input.

Line 13 : The array that will be used to store results of every solved state so that there is no repeated computation is named cache. Some people like to call the array dp instead of cache. In general, cache[i] is interpreted as the minimum number of steps to reach 1 starting from i.

Line 15 : cache[1] = 0 This is the base condition. It says that minimum number of steps to reach 1 starting from 1 is 0.

Line 17 - 37 : For loop to fill up the cache with all states from 1 to n inclusive.

Line 20 : Initialize variables a, b and c to some large number. Here a represents minimum number of steps. If I did the operation n-1, b represents the minimum number of steps. If I did the operation n/2, c represents the minimum number of steps. If I did the operation n/3\. The initial values of a, b and c depends upon the size of the problem.

Line 23 : a = 1 + cache[i-1]. This follows from the recursive relation we defined earlier.

Line 26 - 28: if(i % 2 == 0){
                  b = 1 + cache[i/2];
              }

This follows from the recursive relation we defined earlier.

Line 31 - 33: if(i % 3== 0){
                  c= 1 + cache[i/3];
              }
This follows from the recursive relation we defined earlier.

Line 36 : This the most important step.
cache[i] = Math.min(a, b, c). This essentially determines and stores which of a, b and c gave the minimum number of steps.

Line 40 : All the computations are completed. Minimum steps for all states from 1 to n is calculated. I return cache[n](minimum number of steps to reach 1 starting from n) which is the answer we wanted.

Line 43 : Testing code. It returns a value of 9
```

#### **自上而下的方法**

```
/*
Problem: Given an integer n, find the minimum number of steps to reach integer 1.
At each step, you can:
Subtract 1,
Divide by 2, if it is divisible by 2 
Divide by 3, if it is divisible by 2 
*/

// top-down
function minOne(n, cache) {

    // if the array value at n is not undefined, return the value at that index
    // This is the heart of dynamic programming 
    if (typeof (cache[n]) !== 'undefined') {
        return cache[n];
    }

    // if n has reached 1 return 0
    // terminating/base condition
    if (n <= 1) {
        return 0;
    }

    // initialize a , b and c to some very large numbers
    let a = 1000, b = 1000, c = 1000;

    // one step from n -> n-1
    a = 1 + minOne(n - 1, cache);

    // one step from n -> n/2 if n is divisible by 2
    if (n % 2 === 0) {
        b = 1 + minOne(n / 2, cache);
    }

    // one step from n -> n/3 if n is divisible by 3
    if (n % 3 === 0) {
        c = 1 + minOne(n / 3, cache);
    }

    // Store the minimum number of steps to reach n 
    return cache[n] = Math.min(a, b, c);

}

const cache = [];
console.log(minOne(1000, cache));
```

```
Line 11 : The function that will solve the problem is named as minOne. It takes n and cache as the inputs.

Line 15 - 16 : It checks if for a particular state the solution has been computed or not. If it is computed it returns the previously computed value. This is the top-down way of not doing repeated computation.

Line 21 - 23 : It is the base condition. It says that if n is 1 , the minimum number of steps is 0.

Line 26 :  Initialize variables a, b and c to some large number. Here a represents minimum number of steps if I did the operation n-1, b represents the minimum number of steps if I did the operation n/2 and c represents the minimum number of steps if I did the operation n/3\. The initial values of a, b and c depends upon the size of the problem.

Line 29 : a = 1 + minOne(n-1, cache). This follows from the recursive relation we defined earlier.

Line 32 - 34 : if(n % 2 == 0){
                  b = 1 + minOne(n/2, cache);
              }
This follows from the recursive relation we defined earlier.

Line 37 - 39 : if(n % 3== 0){
                  c = 1 + minOne(n/3, cache);
              }
This follows from the recursive relation we defined earlier.

Line 42 : return cache[n] = Math.min(a, b, c) . This essentially determines and stores which of a, b and c gave the minimum number of steps.

Line 48 - 49 : Testing code. It returns a value of 9
```

#### **时间复杂度**

在动态规划问题中，时间复杂度是**唯一状态/子问题的数量*每个状态花费的时间**。

在这个问题中，对于给定的 n，有 **n** 个唯一的状态/子问题。为了方便起见，每个状态都被说成是在一个恒定的时间内求解的。因此时间复杂度是 **O(n * 1)。**

这可以很容易地通过我们在自底向上方法中使用的 for 循环进行交叉验证。我们看到我们只用了一个 for 循环来解决问题。因此时间复杂度是 O(n)或线性的。

这就是动态编程的力量。它可以有效地解决如此复杂的问题。

#### **空间复杂度**

我们使用一个称为 cache 的数组来存储 n 个状态的结果。因此，数组的大小为 n。因此，空间复杂度为 **O(n)** 。

#### **作为时空权衡的 DP**

动态编程利用空间来更快地解决问题。在这个问题中，我们是用 **O(n)** 空间来解决 **O(n)** 时间中的问题。因此，我们用空间换取速度/时间。因此，它被恰当地称为**时空权衡。**

### 包扎

我希望这篇文章能够揭开动态编程的神秘面纱。我知道通读整篇文章可能是痛苦和艰难的，但是动态编程是一个艰难的话题。掌握它需要大量的练习。

我将发表更多的文章来揭示不同类型的动态编程问题。我还将发表一篇关于如何将回溯解决方案转化为动态编程解决方案的文章。

如果你喜欢这个帖子，请鼓掌支持？(你可以到 50 岁)跟随我来到✌️.中部你可以在 [LinkedIn](https://www.linkedin.com/in/prajwalm/) 上和我联系。你也可以在 [Github](https://github.com/PrajwalM2212) 上关注我。