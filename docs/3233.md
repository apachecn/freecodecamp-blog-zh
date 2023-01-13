# JavaScript 数组排序——如何使用 JS 排序方法(代码示例)

> 原文：<https://www.freecodecamp.org/news/javascript-array-sort-tutorial-how-to-use-js-sort-methods-with-code-examples/>

在 JavaScript 中，我们可以使用一个名为 sort()函数的内置方法轻松地对数组元素进行排序。

但是，不同数组的数据类型(字符串、数字等)可能不同。这意味着单独使用 sort()方法并不总是合适的解决方案。

在这篇文章中，您将学习如何在 JavaScript 中使用 sort()方法对字符串和数字进行排序。

## 字符串数组

让我们从字符串开始:

```
const teams = ['Real Madrid', 'Manchester Utd', 'Bayern Munich', 'Juventus'];
```

当我们使用 sort()方法时，默认情况下，元素将按升序(A 到 Z)排序:

```
teams.sort(); 

// ['Bayern Munich', 'Juventus', 'Manchester Utd', 'Real Madrid']
```

如果您喜欢按降序对数组进行排序，则需要使用 reverse()方法:

```
teams.reverse();

// ['Real Madrid', 'Manchester Utd', 'Juventus', 'Bayern Munich']
```

## 数字数组

不幸的是，对数字进行排序并不那么简单。如果我们将 sort 方法直接应用于 numbers 数组，我们将看到一个意外的结果:

```
const numbers = [3, 23, 12];

numbers.sort(); // --> 12, 23, 3
```

### 为什么 sort()方法对数字不起作用

实际上它正在工作，但是这个问题发生是因为 JavaScript 按字母顺序排列数字。我来详细解释一下这个。

我们来想想 A=1，B=2，C=3。

```
const myArray = ['C', 'BC', 'AB'];

myArray.sort(); // [AB, BC, C]
```

举个例子，如果我们有三个字符串，分别是 C (3)、BC (23)和 AB(12)，JavaScript 会把它们按升序排序为 AB、BC 和 C，这是字母顺序正确的。

然而，JavaScript 将数字(再次按字母顺序)排序为 12、23 和 3，这是不正确的。

### 解决方案:比较功能

幸运的是，我们可以用一个基本的比较函数来支持 sort()方法，这个函数将完成这个任务:

```
function(a, b) {return a - b}
```

幸运的是，sort 方法可以按照正确的顺序对负值、零值和正值进行排序。当 sort()方法比较两个值时，它将值发送给我们的 compare 函数，并根据返回值对值进行排序。

*   如果结果是否定的，则 a 排在 b 之前。
*   如果结果是肯定的，b 排在 a 之前。
*   如果结果为 0，则没有任何变化。

我们需要做的就是在 sort()方法中使用 compare 函数:

```
const numbers = [3, 23, 12];

numbers.sort(function(a, b){return a - b}); // --> 3, 12, 23
```

如果我们想按降序对数字进行排序，这一次我们需要从第一个参数(a)中减去第二个参数(b):

```
const numbers = [3, 23, 12];

numbers.sort(function(a, b){return b - a}); // --> 23, 12, 3
```

## 包裹

正如我们所看到的，如果我们知道如何正确使用 sort()方法，在 JavaScript 中对数组元素进行排序是很容易的。我希望我的帖子能帮助你理解如何正确使用 JavaScript 中的 sort()方法。

如果你想了解更多关于网络开发的知识，欢迎访问我的 Youtube 频道。

感谢您的阅读！