# 如何使用 Push、Unshift 和 Concat 函数添加到一个数组中

> 原文：<https://www.freecodecamp.org/news/javascript-array-insert-how-to-add-to-an-array-with-the-push-unshift-and-concat-functions/>

JavaScript 数组是我最喜欢的数据类型之一。它们是动态的、易于使用的，并且提供了一大堆我们可以利用的内置方法。

然而，选项越多，就越难以决定应该使用哪一个。

在本文中，我将讨论一些向 JavaScript 数组添加元素的常用方法。

## 推送方法

您将遇到的第一个也可能是最常见的 JavaScript 数组方法是 *push()* 。push()方法用于将元素添加到数组的末尾。

假设您有一个元素数组，每个元素是一个字符串，代表您需要完成的任务。将较新的项目添加到数组的末尾是有意义的，这样我们可以先完成前面的任务。

让我们看看代码形式的示例:

```
const arr = ['First item', 'Second item', 'Third item'];

arr.push('Fourth item');

console.log(arr); // ['First item', 'Second item', 'Third item', 'Fourth item']
```

好了，push 给了我们一个很好很简单的语法，可以把一个条目添加到数组的末尾。

假设我们想一次添加两到三个项目到我们的列表中，我们会怎么做呢？事实证明， *push()* 可以接受一次添加多个元素。

```
const arr = ['First item', 'Second item', 'Third item'];

arr.push('Fourth item', 'Fifth item');

console.log(arr); // ['First item', 'Second item', 'Third item', 'Fourth item', 'Fifth item']
```

现在我们已经向数组中添加了一些任务，我们可能想知道数组中当前有多少项，以确定我们是否有太多的任务。

幸运的是，在添加元素后， *push()* 有一个带有数组长度的返回值。

```
const arr = ['First item', 'Second item', 'Third item'];

const arrLength = arr.push('Fourth item', 'Fifth item');

console.log(arrLength); // 5 
console.log(arr); // ['First item', 'Second item', 'Third item', 'Fourth item', 'Fifth item']
```

## 非移位方法

并非所有的任务都是平等的。您可能会遇到这样一种情况:您正在向阵列中添加任务，突然遇到一个比其他任务更紧急的任务。

是时候介绍我们的朋友 *unshift()* 了，它允许我们将项目添加到数组的开头。

```
const arr = ['First item', 'Second item', 'Third item'];

const arrLength = arr.unshift('Urgent item 1', 'Urgent item 2');

console.log(arrLength); // 5 
console.log(arr); // ['Urgent item 1', 'Urgent item 2', 'First item', 'Second item', 'Third item']
```

您可能会注意到，在上面的例子中，就像 *push()* 方法一样， *unshift()* 返回新的数组长度供我们使用。它还让我们能够一次添加多个元素。

## Concat 方法

concatenate(链接在一起)的缩写， *concat()* 方法用于将两个(或更多)数组连接在一起。

如果您还记得上面的内容， *unshift()* 和 *push()* 方法返回新数组的长度。 *concat()* ，另一方面，将返回一个全新的数组。

这是一个非常重要的区别，使得 *concat()* 在处理不希望发生变异的数组时非常有用(比如存储在 React 状态下的数组)。

下面是一个相当基本和简单的例子:

```
const arr1 = ['?', '?'];
const arr2 = ['?', '?'];

const arr3 = arr1.concat(arr2);

console.log(arr3); // ["?", "?", "?", "?"] 
```

假设您有多个想要连接在一起的阵列。别担心， *concat()* 就在那里拯救世界！

```
const arr1 = ['?', '?'];
const arr2 = ['?', '?'];
const arr3 = ['?', '?'];

const arr4 = arr1.concat(arr2,arr3);

console.log(arr4); // ["?", "?", "?", "?", "?", "?"] 
```

### 用 Concat 克隆

还记得我说过，当您不想改变现有数组时， *concat()* 会很有用吗？让我们看看如何利用这个概念将一个数组的内容复制到一个新的数组中。

```
const arr1 = ["?", "?", "?", "?", "?", "?"];

const arr2 = [].concat(arr1);

arr2.push("?");

console.log(arr1) //["?", "?", "?", "?", "?", "?"]
console.log(arr2) //["?", "?", "?", "?", "?", "?", "?"]
```

厉害！我们实际上可以使用 *concat()* 来“克隆”一个数组。

但是在克隆过程中有一个小问题。新数组是已复制数组的“浅表副本”。这意味着任何对象都是通过引用复制的**，而不是实际的对象。**

让我们看一个例子来更清楚地解释这个想法。

```
const arr1 = [{food:"?"}, {food:"?"}, {food:"?"}]

const arr2 = [].concat(arr1);

//change only arr2
arr2[1].food = "?";
arr2.push({food:"?"})

console.log(arr1) //[ { food: '?' }, { food: '?' }, { food: '?' } ]

console.log(arr2) //[ { food: '?' }, { food: '?' }, { food: '?' }, 
{ food: '?' } ]
```

尽管我们没有直接**对我们的原始阵列进行任何更改，但该阵列最终会受到我们在克隆阵列上所做更改的影响！**

有多种不同的方法可以正确地对阵列进行“深度克隆”,但我将把这作为家庭作业留给您。

## 关于如何使用 Push、Unshift 和 Concat 函数添加到数组的视频说明

[https://scrimba.com/scrim/cLwq7WCZ?pl=pd9ZLcW?embed=freecodecamp,mini-header,no-sidebar](https://scrimba.com/scrim/cLwq7WCZ?pl=pd9ZLcW?embed=freecodecamp,mini-header,no-sidebar)

[https://scrimba.com/scrim/cZa9DZsz?pl=pd9ZLcW?embed=freecodecamp,mini-header,no-sidebar](https://scrimba.com/scrim/cZa9DZsz?pl=pd9ZLcW?embed=freecodecamp,mini-header,no-sidebar)

## TL；速度三角形定位法(dead reckoning)

当你想在数组末尾添加一个元素时，使用 *push()。*如果你需要在数组的开头添加一个元素，试试 *unshift()。*您可以使用 *concat()将*和*的数组相加。*

向数组中添加元素当然还有很多其他的选择，我邀请你出去寻找一些更好的数组方法！

欢迎在 Twitter 上联系我，告诉我你最喜欢的向数组添加元素的方法。