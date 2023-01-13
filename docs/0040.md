# 如何在 JavaScript 中生成一定范围内的随机数

> 原文：<https://www.freecodecamp.org/news/generate-random-number-within-a-range-in-javascript/>

假设您想要生成一个介于 10 和 15 之间的随机数——在 JavaScript 中如何实现呢？我将在本文中通过例子向您展示如何操作。

在 JavaScript 中，`Math`对象的`random`方法返回随机数。但是这有一个范围限制。让我们看看如何利用这种方法来求解不同的范围。

我制作了这篇文章的视频版本，你可以[在这里](https://www.youtube.com/watch?v=oUZVKzXVJaE)用来补充你的学习。

## 如何在 JavaScript 中使用`Math.random`

`random`方法返回一个在`0`和`1`之间的随机浮点数。下面是一个代码示例:

```
Math.random()
// 0.26636355538480383

Math.random()
// 0.6272624945940997

Math.random()
// 0.05992852707853347 
```

从结果中，你可以看到三个介于 0 和 1 之间的随机数。现在让我们求解其他范围。

## 如何在 JavaScript 中获得一个范围内的随机数

我们将有一个可用于不同范围的函数:

```
function getRandom(min, max) {
  // code here
} 
```

该函数采用`min`(范围的最低参数)和`max`(范围的最高参数)参数。现在让我们用这个范围和`Math.random`来得到一个随机数:

```
function getRandom(min, max) {
  const floatRandom = Math.random()

  const difference = max - min

  // random between 0 and the difference
  const random = Math.round(difference * floatRandom)

  const randomWithinRange = random + min

  return randomWithinRange
} 
```

下面是函数中发生的情况:

*   首先，我们使用`Math.random()`得到一个随机的浮点数
*   接下来，我们找出最高和最低范围之间的差异
*   接下来，我们评估一个介于 0 和范围差之间的随机数

为了得到这个随机数，我们将差值乘以从`Math.random`中得到的随机数，并对结果应用`Math.round`以将数字四舍五入为最接近的整数。

因此，例如，如果`Math.random`返回 **0.3** 并且范围之间的差是 **5** ，则将它们相乘得到 **1.5** 。然后使用`Math.round`使其成为介于 0 和 5 之间的 **2** (差值)。

再比如:如果`Math.random`返回 **0.9** ，指定范围之差为 **8** ，将它们相乘得到 **7.2** 。然后使用`Math.round`使其成为 **7** ，介于 0 和 8 之间(差值)。

现在我们有了一个介于 0 和差值之间的随机数，我们可以把这个随机数加到最小范围。这样做给我们一个在最小和最大范围内的结果。

我们将这个结果赋给`randomWithinRange`并从函数中返回。现在让我们来看看正在使用的函数:

```
console.log(getRandom(10, 15))
// 14

console.log(getRandom(10, 15))
// 11

console.log(getRandom(10, 15))
// 12

console.log(getRandom(10, 15))
// 15 
```

这里，我们使用 **10** 的一个`min`和 **15** 的一个`max`。我们用这些参数调用函数四次，你可以看到结果是在给定范围内的随机数。

让我们看另一个正在使用的函数的例子:

```
console.log(getRandom(180, 450))
// 215

console.log(getRandom(180, 450))
// 386

console.log(getRandom(180, 450))
// 333

console.log(getRandom(180, 450))
// 442 
```

这里我们用的是 **180** 的一个`min`和 **450** 的一个`max`。同样，你可以看到我们的函数是如何产生随机数的。

## 包扎

如果您需要生成一个特定范围内的随机数，我希望这篇文章已经向您展示了如何操作。

在本文中，我解释了`Math.random`的范围限制，它返回一个介于 0 和 1 之间的随机数。我还向您展示了如何利用这种数学方法来创建一个可重用的函数，用于在您选择的任何范围内生成随机数。

如果你觉得这篇文章有帮助，请分享。