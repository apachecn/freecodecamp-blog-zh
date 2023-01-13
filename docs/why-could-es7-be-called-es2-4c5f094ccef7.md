# 最新版本的 JavaScript 只有两个新特性。他们是这样工作的。

> 原文：<https://www.freecodecamp.org/news/why-could-es7-be-called-es2-4c5f094ccef7/>

提哥·洛佩斯·费雷拉

# 最新版本的 JavaScript 只有两个新特性。他们是这样工作的。

![1*ZxRdFZUQT6YcUVQFhTTNKQ](img/9655e0395a7241bd5ae99f1a52f02ad8.png)

先说 JavaScript 的最新版本:ECMAScript 2016(更俗称 ES7)。ES7 带来了两个新特性:`Array.prototype.includes()`和新的指数运算符:`**`。

### Array.prototype.includes()

我们使用`.indexOf()`来知道数组中的元素**是否存在**的日子已经一去不复返了。

关键词是 ***“存在。”***

如果我们想知道一个给定的元素出现在哪个索引处，那么`.indexOf()`就可以了。

但是如果我们的目标是知道一个给定的元素**是否存在于数组中的**，那么`.indexOf()`不是最好的选择。原因很简单:当查询某物的存在时，我们期望一个布尔值，**而不是一个数字**。

`Array.prototype.includes()`确实如此。它判断数组中是否存在给定的元素，如果存在，返回`true`，否则返回`false`。

### 纳入规范

```
Array.prototype.includes ( searchElement [ , fromIndex ] )
```

*   `searchElement` —要搜索的元素。
*   `fromIndex` *(可选)* —从其开始搜索的索引。

钻研[规范](https://www.ecma-international.org/ecma-262/7.0/)感觉就像在寻找力量。

![1*4KuLQRBf9qZkEGUsFKZnJg](img/2e316206f077fdbd116646afbfcc7ca9.png)

说明书上说:

让我们一步一步来，试着用例子来理解规范。

1.  这里的区别是元件 4 的位置。因为我们的第一个示例将 4 放在最后一个位置，所以 includes 将搜索整个数组。根据规范`.includes()`在找到`searchElement`后立即返回。这使得我们的第二次操作更快。
2.  与 [SameValueZero](https://www.ecma-international.org/ecma-262/7.0/#sec-samevaluezero) 算法和[严格相等比较](https://www.ecma-international.org/ecma-262/7.0/#sec-strict-equality-comparison)(由`.indexOf()`使用)的最大区别在于，它允许检测 **NaN** 元素。
3.  当找到元素时，它返回布尔值`true`，否则返回`false`。结果没有更多索引？
4.  与`.indexOf()`相反，`.includes()`不会跳过缺失的数组元素。相反，它将它们视为**未定义的**值。

你开始感受到力量了吗？

我们甚至还没碰过`fromIndex`。

让我们检查一下规格:

> 可选的第二个参数`fromIndex`默认为`0`(即搜索整个数组)。如果大于或等于数组的长度，则返回 **false** ，即不搜索数组。如果它是负的，它被用作从数组末尾的**偏移量**来计算`fromIndex`。如果计算出的索引小于`0`，则将搜索整个数组。

1.  如果没有提供`fromIndex`，则采用默认值`0`并搜索**整个**数组。
2.  当`fromIndex`的值大于数组长度时，`.includes()`立即返回 **false** 。
3.  当`fromIndex`为负时，其值计算为`array.length — fromIndex`。这在搜索最后一个元素时特别有用。例如，`fromIndex = -5`等同于搜索最后 5 个元素。
4.  当`fromIndex`计算值小于 0 时，为了避免`.includes()`中断，搜索整个数组。我宁愿打破？

好的—最后一个新功能…

### `The Exponential Operator (**)`

我们一直在等待有一天我们可以像玩加减乘除一样玩指数数字。

好吧，那一天到了。

操作员`**`的行为与`Math.pow()`完全相同。它返回第一个操作数的第二次幂的结果(例如 `x ** y`)。

就是这样！

你现在拥有了 **ES7** 的力量！好好用！

![1*owhYyEq_wSRyPN_OuyQXPQ](img/02cca7d8f87dae0b49ad7a835a490a10.png)

### 感谢什么？

*   [2ality.com](http://2ality.com/2016/01/ecmascript-2016.html)作者[阿克塞尔·劳施迈尔](https://twitter.com/rauschma)
*   [ECMAScript 2016 语言规范](https://www.ecma-international.org/ecma-262/7.0/)
*   致所有的希曼粉丝
*   [发布❤️的 freeCodeCamp](https://medium.freecodecamp.org/)

请务必查看我关于 ES6 的文章:

[**我们来探讨一下 ES6 生成器**](https://medium.freecodecamp.org/lets-explore-es6-generators-5e58ed23b0f1)
[*生成器，又名 iterables 的一种实现。*medium.freecodecamp.org](https://medium.freecodecamp.org/lets-explore-es6-generators-5e58ed23b0f1)[**哦对了！async/Await**](https://medium.freecodecamp.org/oh-yes-async-await-f54e5a079fc1)
[*async/Await 是声明异步函数的新 JavaScript 语法。*medium.freecodecamp.org](https://medium.freecodecamp.org/oh-yes-async-await-f54e5a079fc1)