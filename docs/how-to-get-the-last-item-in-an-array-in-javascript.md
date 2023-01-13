# 如何在 JavaScript 中获取数组中的最后一项

> 原文：<https://www.freecodecamp.org/news/how-to-get-the-last-item-in-an-array-in-javascript/>

当你用 JavaScript 编程时，你可能需要获取数组中的最后一项。在本教程中，我们将通过两种不同的方式来做到这一点。

## 如何获取 JavaScript 数组中的最后一项

### 方法 1:使用索引定位

如果知道数组的长度，请使用索引定位。

让我们创建一个数组:

```
const animals = [‘cat’, ‘dog’, ‘horse’]
```

这里我们可以看到这个数组的最后一项是`horse.`

要获得最后一项，我们可以根据它的索引来访问最后一项:

`const finalElement = animals[2]`；

JavaScrip 数组是零索引的。这就是为什么我们可以用`animals[2].`来访问`horse`元素，如果你不确定零索引是什么意思，我们将在本教程的后面讨论它。

如果知道数组的长度，这种获取数组中最后一项的方法是可行的。

但是如果不知道数组的长度呢？这个数组，`animals`，很小。但是你可以有另一个数组，里面有几十个元素，你可能不知道它的长度。

### 方法 2:当你不知道数组长度时

在不知道数组长度的情况下，获取数组中的最后一项:

`const lastItem = animals[animals.length - 1]`；

`lastItem`变量现在保存了`horse.`的值

让我们分析一下上面一行发生了什么。首先，让我们来看看控制台日志`animals.length:`

`console.log(animals.length);`

如果您不熟悉`length`属性，它会返回这个数组的长度。这会打印出`3`，因为数组中有`3`项。

前面我们了解到 JavaScript 数组是零索引的。这只是意味着在 JavaScript 数组中，你从零开始计数，而不是从一开始计数。我们可以通过查看我们的`animals`数组来了解这一点。`cat`位于索引`0`，`dog`位于索引`1`，`horse`位于索引`2`。

你可能还是会困惑。我们刚刚了解到`animals.length`会告诉我们一个数组中有多少项。我们还了解到，对于 JavaScript 数组，我们从零开始计数，而不是从一开始计数。但是这如何解释为什么我们可以通过使用`animals[animals.length - 1]?`获得数组中的最后一项呢

让我们想象一下，JS 数组不是零索引的，我们从 1 开始计数，这是我们在现实世界中计数的方式。

使用`animals`数组，我们可以快速开始计数，并假设第一个项目`cat`的索引为 1，`dog`的索引为`2`，而`horse`的索引为`3`。这里的关键是，在基于 1 的索引中，最后一项的索引是数组的长度。如果数组长度为 3，那么数组中最后一个元素的索引为`3`。所以`animals[3]`会评估为`horse`。

然而 JavaScript 数组是从`0.`开始的，所以如果我们想计算 JavaScript 数组中最后一项的索引，我们可以从数组长度中减去 1:

`animals[animals.length - 1]`；

在括号内，`animals.length - 1`的值为`2`。现在我们有了数组中的最后一项。

在本文中，我们学习了两种获取 JavaScript 数组中最后一项的方法。

感谢您的阅读！

如果你喜欢这篇文章，加入我的[编码俱乐部](https://madisonkanna.us14.list-manage.com/subscribe/post?u=323fd92759e9e0b8d4083d008&id=033dfeb98f)，在那里我们每周日一起应对编码挑战，并在学习新技术时相互支持。

****如果你对这篇文章有反馈或问题，或者在 Twitter 上找到我 [@madisonkanna](https://twitter.com/Madisonkanna) 。****