# 技术面试的问题解决模式:解释频率计数器模式

> 原文：<https://www.freecodecamp.org/news/solve-technical-interview-questions-using-frequency-counter/>

在我的上一篇文章中，我分享了我对如何准备软件开发人员面试的想法。

在这篇文章中，我将稍微改变一下思路，谈谈你可以用来解决技术面试中的问题的常见模式。我们将深入讨论*频率计数器*模式，以帮助您有效地处理它。

## 什么是“频率计数器”模式？

频率计数器模式使用一个对象或集合来收集值和这些值的频率。

这种模式通常与`array`或`string`一起使用，并允许您避免嵌套循环(二次时间复杂度`O(n²)`)。

## 什么时候应该使用频率计数器模式？

当您有多条数据需要相互比较时，频率计数器模式最有帮助。让我通过一个例子来看看频率计数器的作用。

## “相同平方”练习

*   编写一个名为`sameSquared`的函数，它接受两个数组
*   如果第一个数组中的每个值在第二个数组中都有其对应的平方值，则该函数应该返回`true`
*   值的频率必须相同

### 最佳结果是什么？

在我们的函数写好之后，我们应该期待我们的`sameSquared`函数返回这些值。

`sameSquared([1, 2, 3], [4, 1, 9]); *// true*`

`sameSquared([1, 2, 3], [1, 9]); *// false*`

`sameSquared([1, 2, 1], [4, 4, 1]); *// false*`

`sameSquared([2, 3, 6, 8, 8], [64, 36, 4, 9, 64]); *// true*`

### 入门指南

首先，使用关键字`function`，我们创建一个带有标识符`sameSquared`的函数:

```
function sameSquared() { 
```

我们的函数`sameSquared`需要两个参数，第一个数组和第二个数组。在这个例子中，我们传递这些值`[1, 2, 3]`和`[4, 1, 9]`。

```
function sameSquared(firstArr, secondArr) { 
```

### 检查边缘案例

在我们的功能块内部，我们想要处理一些边缘情况。首先，我们需要检查两个参数是否都有真值，即*不是* `null`、`undefined`等等。

我们可以通过使用`!`操作符来检查错误值。如果`firstArr`或`secondArr`为假，我们返回`false`。

```
function sameSquared(firstArr, secondArr) {
  if (!firstArr || !secondArr) return false;
```

我们要考虑的下一个边缘情况是确保两个数组的长度相同。如果它们不同，我们知道它们可以*而不是*包含等量的共享值。

通过检查两个参数的`length`属性，我们可以确定它们是否相同。如果不是，我们返回`false`。

```
function sameSquared(firstArr, secondArr) {
  if (!firstArr || !secondArr) return false;
  if (firstArr.length !== secondArr.length) return false;
```

### 建立一个“字典”来避免嵌套循环

我们需要跟踪至少一个数组中的所有值。为此，并且为了避免嵌套循环，我们可以将这些值存储在哈希表(对象)中。我叫我的`lookup`。

```
function sameSquared(firstArr, secondArr) {
  if (!firstArr || !secondArr) return false;
  if (firstArr.length !== secondArr.length) return false;

  const lookup = {};
```

使用一个`for of`循环，我们遍历`firstArr`。在`for of`块内部，我们将密钥分配给`value * value`的结果。

这个键/值对中的值将是一个*频率计数器*，它反映特定值在`firstArr`中被“看到”的次数。

首先，我们检查`lookup`是否包含`value * value`的条目，如果包含，我们将`1`添加到其中。如果没有，我们将值赋给`0`，然后加上`1`。

```
function sameSquared(firstArr, secondArr) {
  if (!firstArr || !secondArr) return false;
  if (firstArr.length !== secondArr.length) return false;

  const lookup = {};

  for (value of firstArr) {
    lookup[value * value] = (lookup[value * value] || 0) + 1;
  }
```

一旦`firstArr`完成循环，`lookup`应该包含这些值:

```
{
  1: 1,
  4: 1,
  9: 1
}
```

### 比较数组值

既然我们已经遍历了`firstArr`中的所有值，并将它们存储为各自的*平方*值，我们希望将这些值与`secondArr`中的值进行比较。

我们从创建另一个`for of`循环开始。在新的`for of`块中的第一行，我们写了一个条件语句来检查来自`secondArr`的当前值是否是`lookup`中的*而不是*。如果不是，我们停止循环并返回`false`。

如果来自`secondArr`的值在我们的`lookup`中，我们想要减少那个条目的值。我们可以通过使用`-=`赋值操作符来实现。

```
function sameSquared(firstArr, secondArr) {
  if (!firstArr || !secondArr) return false;
  if (firstArr.length !== secondArr.length) return false;

  const lookup = {};
  for (value of firstArr) {
    lookup[value * value] = (lookup[value * value] || 0) + 1;
  }
  for (secondValue of secondArr) {
    if (!lookup[secondValue]) return false;
      lookup[secondValue] -= 1;
    }
```

在我们循环完`secondArr`之后，我们的`lookup`应该有这些值:

```
{
  1: 0,
  4: 0,
  9: 0
}
```

### 结束我们的“相同平方”函数

如果我们完成了对`secondArr`的迭代而没有返回`false`，这意味着我们的`firstArr`包含了所有在`secondArr`中处于平方状态的值。因此，我们在`for of`循环之外返回`true`。

```
function sameSquared(firstArr, secondArr) {
  if (!firstArr || !secondArr) return false;
  if (firstArr.length !== secondArr.length) return false;

  const lookup = {};
  for (value of firstArr) {
    lookup[value * value] = (lookup[value * value] || 0) + 1;
  }
  for (secondValue of secondArr) {
    if (!lookup[secondValue]) return false;
    lookup[secondValue] -= 1;
  }
  return true;
}
```

让我给你看另一个在编码评估中非常常用的例子(所以你可能以前见过这个问题)。

## “伊桑格拉姆”演习

*   编写一个名为`isAnagram`的函数，它接受两个字符串
*   如果两个字符串参数是彼此的[变位词](https://en.wikipedia.org/wiki/Anagram)，那么函数应该返回`true`

### 最佳结果是什么？

在我们的函数写好之后，我们应该期待我们的`isAnagram`函数返回这些值。

`isAnagram(“silent”, “listen”); *// true*`

`isAnagram(“martin”, “nitram”); *// true*`

`isAnagram(“cat”, “tag”); *// false*`

`isAnagram(“rat”, “tar”); *// true*`

### 入门指南

首先，使用关键字`function`，我们创建一个带有标识符`isAnagram`的函数:

```
function isAnagram() { 
```

我们的函数`isAnagram`需要两个参数，第一个`string`和第二个`string`。在这个例子中，我们传递这些值，`silent`和`listen`。

```
function isAnagram(firstStr, secondStr) { 
```

### 检查边缘案例

在我们功能块的前几行，我们想处理一些边缘情况，就像第一个例子一样。

类似于`isAnagram`，我们需要检查两个参数都有真值，即*不是* `null`、`undefined`等等。我们可以通过使用`!`操作符来检查一个错误值。如果`firstStr`或`secondStr`为假，我们返回`false`。

```
function isAnagram(firstStr, secondStr) {
  if (!firstStr || !secondStr) return false;
```

我们要考虑的下一个边缘情况是确保两个数组的长度相同。如果它们不同，我们知道它们可以*而不是*包含相同数量的共享值。

通过检查两个参数的`length`属性，我们可以确定它们是否相同。如果不是，我们返回`false`

```
function isAnagram(firstStr, secondStr) {
  if (!firstStr || !secondStr) return false;
  if (firstStr.length !== secondStr.length) return false;
```

### 建立一个“字典”来避免嵌套循环

记住，我们使用的是频率计数器模式，我们需要跟踪至少一个数组中的所有值。现在我们知道处理这个问题的最好方法是将这些值存储在一个哈希表(对象)中。为了保持一致，我将再次调用我的`lookup`。

```
function isAnagram(firstStr, secondStr) {
  if (!firstStr || !secondStr) return false;
  if (firstStr.length !== secondStr.length) return false;

  const lookup = {};
```

使用一个`for of`循环，我们遍历`firstStr`。在`for of`块内部，我们将密钥分配给表达式`value * value`的结果。

这个键/值对中的值将是一个*频率计数器*，它反映特定值在`firstStr`中被“看到”的次数。

使用三元运算符，我们检查`lookup`是否包含`value * value`的条目，如果包含，我们使用`+=`赋值运算符将值增加`1`。如果不匹配，我们只需将值赋给`1`。

```
function isAnagram(firstStr, secondStr) {
	if (!firstStr || !secondStr) return false;
    if (firstStr.length !== secondStr.length) return false;

	const lookup = {};

	for (first of firstStr) {

    	lookup[first] ? (lookup[first] += 1) : (lookup[first] = 1);

  }
```

一旦`firstStr`完成循环，`lookup`应该包含这些值:

```
{
  s: 1,
  i: 1,
  l: 1,
  e: 1,
  n: 1,
  t: 1
}
```

### 比较数组值

既然我们已经遍历了`firstStr`中的所有值并存储了它们的值，我们希望将这些值与`secondStr`中的值进行比较。

我们从创建另一个`for of`循环开始。在新的`for of`块中的第一行，我们写了一个条件语句来检查来自`secondStr`的当前值是否是`lookup`中的*而不是*。如果不是，我们要停止迭代，返回`false`。

否则，如果来自`secondStr` *的值是我们的`lookup`中的*，我们想要减少那个条目的值。我们可以通过使用`-=`赋值操作符来实现。

```
function isAnagram(firstStr, secondStr) {
  if (!firstStr || !secondStr) return false;
  if (firstStr.length !== secondStr.length) return false;

  const lookup = {};

  for (first of firstStr) {
    lookup[first] ? (lookup[first] += 1) : (lookup[first] = 1);
  }

  for (second of secondStr) {
    if (!lookup[second]) return false;
    lookup[second] -= 1;
  }
```

在我们循环完`secondStr`之后，我们的`lookup`应该有这些值:

```
{
  s: 0,
  i: 0,
  l: 0,
  e: 0,
  n: 0,
  t: 0
}
```

### 结束我们的“isAnagram”函数

如果我们完成了对`secondStr`的迭代而没有返回`false`，这意味着我们的`firstStr`包含了`secondStr`中的所有值。因此，我们在`for of`循环之外返回`true`。

```
function isAnagram(firstStr, secondStr) {
  if (!firstStr || !secondStr) return false;
  if (firstStr.length !== secondStr.length) return false;

  const lookup = {};

  for (first of firstStr) {
    lookup[first] ? (lookup[first] += 1) : (lookup[first] = 1);
  }

  for (second of secondStr) {
    if (!lookup[second]) return false;
    lookup[second] -= 1;
  }
  return true;
}
```

## 概括起来

我希望这个关于频率计数器模式的深入概述对您有所帮助。既然你已经知道了这种模式是如何运作的，我相信你能够通过在更高层次上展示你的技能来给你的面试官留下深刻印象。

在我的下一篇文章中，我将讨论另一种常见的问题解决模式，称为滑动窗口。感谢阅读，祝面试愉快！