# 斐波那契数列——用 Python、JavaScript、C++、Java 和 Swift 解释

> 原文：<https://www.freecodecamp.org/news/the-fibonacci-sequence-in-5-different-programming-languages-1c6514c749e5/>

保罗·帕万

根据定义，斐波纳契数列是一个整数序列，其中前两个数字之后的每个数字都是前面两个数字的和。为了简化:

0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, …

它在数学甚至交易中有很多应用(是的，你没看错:交易)，但这不是本文的重点。我今天的目标是向你展示如何在五种不同的编程语言中使用递归函数计算这一系列数字中的任何一项。

递归函数是那些基本上自己调用自己的函数。

我想指出的是，这并不是最好的方法——事实上，它可以被认为是最基本的方法。这是因为计算级数的更大项所需的计算能力是巨大的。在大多数语言中，调用函数的次数会导致堆栈溢出。

尽管如此，出于本教程的目的，让我们开始吧。

首先，让我们考虑一下代码会是什么样子。它将包括:

一个递归函数 F (F 代表斐波那契):计算下一项的值。

没有别的:我警告过你这是很基本的。

我们的函数将把 *n* 作为输入，它将引用我们想要计算的序列的第 *n* 项。所以，F(4)应该返回序列的第四项。

让我们计划一下。不管使用何种语言，代码应该如下所示:

[`function F(n)  if n = 0`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[`   return 0  if n = 1`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[`   return 1  else`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[`   return F(n-1) + F(n-2)`](https://unsplash.com?utm_source=medium&utm_medium=referral)

注意:数列的第 0 项会被认为是 0，所以第一项会是 1；第二，1；第三，2；诸如此类。你明白了。

我们先分析一下函数。如果它得到 0 作为输入，它返回 0。如果它得到 1，它返回 1。如果它得到 2…那么，在这种情况下，它将落入 else 语句，该语句将为术语 2–1(1)和 2–2(0)再次调用该函数。这将返回 1 和 0，两个结果相加，返回 1。完美。

现在你可以明白为什么递归函数在某些情况下是个问题了。假设你想要序列的第 100 项。该函数将为第 99 和第 98 项调用自身，这些项将为第 98 和第 97 项、第 97 和第 96 项再次调用该函数，以此类推。那将会非常慢。

但好消息是它确实有效！

所以让我们从不同的语言开始。为了让你的阅读体验更好，我不会给出太多细节(其实一点细节都没有)。反正没有太多细节。

让我们开始吧:

### 计算机编程语言

[`def F(n):  if n == 0:`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[`   return 0  if n == 1:`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[`   return 1  else:`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[`   return F(n-1) + F(n-2)`](https://unsplash.com?utm_source=medium&utm_medium=referral)

### 迅速发生的

[`func F(_ n: Int) -> Int {  if n == 0 {    return 0`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[` }  if n == 1 {    return 1`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[` }  else {    return F(n-1) + F(n-2)`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[` }}`](https://unsplash.com?utm_source=medium&utm_medium=referral)

### Java Script 语言

[`function F(n) {  if(n == 0) {    return 0;`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[` }  if(n == 1) {    return 1;`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[` }  else {    return F(n-1) + F(n-2);`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[` }}`](https://unsplash.com?utm_source=medium&utm_medium=referral)

### Java 语言(一种计算机语言，尤用于创建网站)

[`public static int F(int n) {  if(n == 0) {    return 0;`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[` }  if(n == 1) {    return 1;`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[` }  else {    return F(n-1) + F(n-2);`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[` }}`](https://unsplash.com?utm_source=medium&utm_medium=referral)

### C++

[`int F(int n) {  if(n == 0) {    return 0;`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[` }  if(n == 1) {    return 1;`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[` }  else {    return F(n-1) + F(n-2);`](https://unsplash.com?utm_source=medium&utm_medium=referral)
[` }}`](https://unsplash.com?utm_source=medium&utm_medium=referral)

仅此而已。我选择这些语言仅仅是基于受欢迎程度——或者至少是因为这 5 种是我最常用的语言，它们没有特定的顺序。在我看来，它们可以按照语法难度进行分类，从 Python(最简单)到 C++(最难)。但这取决于你的个人观点和你对每种语言的体验。

我希望你喜欢这篇文章，如果你有任何问题/建议或者只是想说声嗨，请在下面评论！