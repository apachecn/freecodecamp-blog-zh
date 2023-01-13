# 什么是算法？计算机科学初学者的算法定义

> 原文：<https://www.freecodecamp.org/news/what-is-an-algorithm-definition-for-beginners/>

如果你是一名学生，想要学习计算机科学，或者你正在学习编码，那么你可能听说过算法。简单来说，算法就是执行特定动作的一组指令。

与普遍的看法相反，算法不是需要非常高级的知识才能实现的代码。同时，我也不会说一个算法容易实现。有些可以，但这取决于你想做什么。

最后，提高算法水平的最好方法是定期练习。

在这篇文章中，你将学习所有关于算法的知识，这样你就可以在下次遇到算法时做好准备，或者自己写一个。我也会分享一些 freeCodeCamp 的资源，帮助你学习如何用不同的语言写算法。

## 我们将涵盖的内容

*   [到底什么是算法？](#whatexactlyisanalgorithm)
*   [为什么需要算法？](#whydoyouneedanalgorithm)
*   [算法类型](#typesofalgorithms)
*   [哪种编程语言最适合写算法？](#whichprogramminglanguageisbestforwritingalgorithms)(重命名)
*   [学习算法的资源](#resourcesforlearningalgorithms)
*   [结论](#conclusion)

## 到底什么是算法？

算法是解决已知问题的一组步骤。大多数算法都是按照下面的**四个步骤**执行的:

*   听取意见
*   访问输入并确保它是正确的
*   展示结果
*   终止(算法停止运行的阶段)

算法的某些步骤可能会重复运行，但最终，**终止**是结束算法的原因。

例如，下面的算法按降序对数字进行排序。它循环遍历指定的数字，直到按降序排列，然后在没有更多数字要排序时终止:

```
function sortNumbersInDescendingOrder(nums) {
  let sortedNums = false;
  while (!sortedNums) {
    sortedNums = true;
    for (let i = 1; i < nums.length; i++) {
      if (nums[i] > nums[i - 1]) {
        [nums[i], nums[i - 1]] = [nums[i - 1], nums[i]];
        sortedNums = false;
      }
    }
  }
  return nums;
}

const unsortedNums = [2, 3, 1, 6, 4, 5, 10, 9, 8, 7];
const sortedNums = sortNumbersInDescendingOrder(unsortedNums);

console.log(sortedNums); // [ 10, 9, 8, 7, 6, 5, 4, 3, 2, 1 ] 
```

例如，在理论基础上，将两个数相除并显示余数的算法可以通过以下步骤运行:

*   **步骤 1** :用户输入第一个和第二个数字——被除数和除数
*   **步骤 2** :为执行除法而编写的算法接收数字，然后在被除数和除数之间加上一个除法符号。它还检查余数。
*   **步骤 3** :将除法和余数的结果显示给用户
*   **步骤 4** :算法终止

下面是这种算法在 JavaScript 中的实现方式:

```
function divideTwoNums(num1, num2) {
  const result =  num1 / num2;
  const remainder = num1 % num2

  return `The result is ${result} remainder ${remainder}`
}

const result = divideTwoNums(21, 2)

console.log(result) // The result is 10.5 remainder 1 
```

如果有错误，算法可能不会运行，或者可能返回错误的输出。如果编写算法的程序员考虑了用户体验，那么错误处理程序可以向用户显示错误，并让他们知道该做什么。

## 为什么需要算法？

如果你是那些问“为什么是算法”的计算机科学学生之一，这里有一些你应该学习它们的原因:

**解决问题**:能够编写算法提高了你解决问题的能力。人们普遍认为，一旦你可以用一件事解决一个问题，你就可以用另一件密切相关的事来解决问题。所以，如果你能用 Python 解决问题，你就能用 JavaScript 解决问题。

**可扩展性**:一种算法可以帮助你的软件/应用/网站适当地响应需求。

**合理利用资源:**选择正确的算法可以确保合理利用内存、存储、网络等资源。

## 算法的类型

计算机科学中的算法可以大致分为搜索和排序算法:

*   排序–选择排序、冒泡排序、插入排序、合并排序、快速排序等等。
*   搜索–二分搜索法、指数搜索、跳跃搜索等等。

但是程序员经常使用许多类型的算法。以下是按类别组织的一些其他常见算法类型:

*   哈希——SHA-1 SHA-256
*   暴力——反复试验
*   分治–合并排序算法
*   贪婪——普里姆的算法，克鲁斯卡尔的算法
*   递归–计算机阶乘

## 哪种编程语言最适合写算法？

你可以用任何编程语言编写算法。使用一种语言比使用另一种语言没有好处。

每种语言都有其长处和短处，每种语言都有独特的语法和功能。因此，用一种语言写一个算法可能看起来与另一种语言不同。

但是算法是通用的概念。所以如果你能用 Python 写**冒泡排序**，你也应该能用 JavaScript 或 C#写。

## 学习算法的资源

以下是 freeCodeCamp YouTube 频道的一些视频，可以帮助你有效地学习算法:

*   [算法和数据结构教程——初学者全教程](https://www.youtube.com/watch?v=8hly31xKli0)
*   [Python 中的算法——初学者的完整课程](https://www.youtube.com/watch?v=fW_OS3LGB9Q)
*   [数据结构轻松进阶课程——来自谷歌工程师的完整教程](https://www.youtube.com/watch?v=RBSGKlAvoiM&list=PLR69o5h4xAwUgZHf8Y6V-KukXHBlbAcc1&index=2)
*   [动态编程——学会解决算法问题&编码挑战](https://www.youtube.com/watch?v=oBt53YbR9Kk&list=PLR69o5h4xAwUgZHf8Y6V-KukXHBlbAcc1&index=4)
*   [了解排序算法](https://www.youtube.com/watch?v=l7-f9gS8VOs&list=PLR69o5h4xAwUgZHf8Y6V-KukXHBlbAcc1&index=14)

此外，freeCodeCamp 上的交互式 [JavaScript 算法和数据结构认证](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)可以给你一个 JavaScript 算法思维的速成班。

## 结论

在本文中，我们回顾了什么是算法，它们的类型，以及学习算法的资源。

如果你读到这里，你应该做的下一件事是从本文列出的一个或多个资源开始学习算法。

感谢您的阅读。