# JavaScript 数组方法——如何在 JS 中使用 every()和 some()

> 原文：<https://www.freecodecamp.org/news/learn-the-every-and-some-array-methods-in-javascript/>

在 JavaScript 中，`every`和`some`帮助您测试数组中的每个元素或某些元素是否为真。

在本文中，我将向您展示如何使用这些有用的数组方法。

## 目录

*   1T4`every()`和`some()`如何工作——概述
*   2 [参数`every`和`some`](#parameters-of-every-and-some)
    *   2.1T3`predicate`
    *   2.2[可选`thisArg`](#optional-thisarg)
*   3 [边例为`every`和`some`](#edge-cases-for-every-and-some)
    *   3.1 [在空数组上调用`every`和`some`会怎样？](#what-happens-when-every-and-some-is-called-on-an-empty-array)
    *   3.2 [不存在的元素被忽略](#non-existing-elements-are-ignored)
    *   3.3 [谓词中的数组变异](#mutating-the-array-in-the-predicate)
*   4 [给你一个挑战](#a-challenge-for-you)

[Made with FTG](https://ashutoshbw.github.io/ftg/)

## 1`every()`和`some()`如何工作——概述 [#](#how-every-and-some-work-–-an-overview)

首先我们需要一些数据来测试。为简单起见，让我们考虑一组数字:

```
const nums = [34, 2, 48, 91, 12, 32];
```

现在假设我们想测试数组中的每个数字是否都小于`100`。使用`every`我们可以很容易地测试它，如下所示:

```
nums.every(n => n < 100);
// true
```

又短又甜！你可以这样想这里发生了什么:

*   `every`从左到右循环数组元素。
    *   对于每次迭代，它调用给定的函数，将当前数组元素作为它的第一个参数。
    *   循环继续，直到函数返回一个 **[假值](https://www.ashutoshbiswas.dev/blog/truthy-and-falsy/)** 。在这种情况下，`every`返回`false`，否则返回`true`。

`some`的工作方式与`every`非常相似:

*   `some`从左到右循环数组元素。
    *   对于每次迭代，它调用给定的函数，将当前数组元素作为它的第一个参数。
    *   循环继续，直到函数返回一个 **[真值](https://www.ashutoshbiswas.dev/blog/truthy-and-falsy/)** 。在这种情况下，`some`返回`true`，否则返回`false`。

现在让我们使用`some`来测试数组中的某个数字是否是奇数:

```
nums.some(n => n % 2 == 1);
// true
```

真的是这样！`91`奇。

但这并不是故事的结尾。这些方法更有深度。让我们开始吃吧。

## 2`every``some`[#](#parameters-of-every-and-some)的参数

使用`every`和`some`数组方法的方式完全一样。它们有相同的一组参数，这些参数也表示相同的东西。所以一下子学会它们是非常容易的。

我们已经使用了这些方法的第一个参数，它是一个函数。我们把这个函数叫做*谓词*。

> 在计算机科学中， **[谓词](https://www.baeldung.com/cs/predicates)** 是返回布尔值作为答案的一组参数的函数。JavaScript 将我们赋予`every` / `some`的函数视为*谓词*。我们可以返回我们希望的任何类型的值，但是这些值被视为布尔值，所以通常称这个函数为谓词。

它们还有一个可选的第二个参数来控制非箭头谓词中的`this`。我们称之为`thisArg`。

因此，您可以通过以下方式调用这些方法:

```
arr.every(predicate)
arr.every(predicate, thisArg)

arr.some(predicate)
arr.some(predicate, thisArg)
```

下面我们来详细看看`predicate`和可选的`thisArg`。

### 2.1 `predicate` [#](#predicate)

通过`predicate`、`every` / `some`不仅可以访问当前数组元素，还可以通过其参数访问其索引和原始数组，如下所示:

*   **第一个参数**:当前数组元素。
*   **第二个参数**:当前元素的索引。
*   **第三个参数**:调用`every` / `some`的数组本身。

在前面的例子中，我们只看到了第一个参数的作用。让我们通过定义另外两个参数来捕捉索引和数组。这一次，假设我们有一些 t 恤数据来测试它们是否都有`freeCodeCampe`标志:

```
let tshirts = [
  { size: "S", color: "black", logo: "freeCodeCamp" },
  { size: "S", color: "white", logo: "freeCodeCamp" },
  { size: "S", color: "teal",  logo: "freeCodeCamp" },
  { size: "M", color: "black", logo: "freeCodeCamp" },
  { size: "M", color: "white", logo: "freeCodeCamp" },
  { size: "M", color: "teal",  logo: "freeCodeCamp" },
  { size: "L", color: "black", logo: "freeCodeCamp" },
  { size: "L", color: "white", logo: "freeCodeCamp" },
  { size: "L", color: "teal",  logo: "freeCodeCamp" },
];

tshirts.every((item, i, arr) => {
  console.log(i);
  console.log(arr);
  return item.logo == "freeCodeCamp";
})
```

在您的控制台上尝试一下，看看输出结果。别忘了和`some`一起玩耍。

### 2.2 可选`thisArg`

如果在任何情况下都需要在谓词中有一个特定的`this`值，可以用`thisArg`来设置。注意，只适用于非箭头谓词，因为箭头函数没有`this`绑定。

如果省略此参数，谓词(非箭头函数)内的`this`照常工作，即:

*   在严格模式下`this`是`undefined`。
*   在草率模式下`this`是**全局对象**，它是浏览器中的`window`和节点中的`global`。

我想不出`thisArg`有什么好的用例。但是我认为它的存在是好的，因为现在你可以控制谓词中的`this`。因此，即使有一天需要它，你也会知道有一种方法。

如果你对`thisArg`的用法有什么好主意，请在 Twitter 上告诉我:)

## 3 边例为`every`和`some` [#](#edge-cases-for-every-and-some)

### 3.1 在空数组上调用`every`和`some`会发生什么？ [#](#what-happens-when-every-and-some-is-called-on-an-empty-array)

有时您想要测试的数组可能是空的。例如，你从一个 API 中获取一个数组，它可以在不同的时间包含任意数量的元素，包括零个元素。

对于`every`的情况，一个`true`返回值可能意味着两件事:

*   如果数组的元素多于零，那么数组的所有元素都满足谓词。
*   该数组没有元素。

因此，如果我们愿意，我们可以在谓词内部做一些疯狂的事情，如下所示:

![wth-what-the-hell-is-going-on](img/6d1f6222e6832a481ca1a694a533a060.png)

```
const myCatsBankAccounts = [];
myCatsBankAccounts.every(account => account.balance > elonMusk.totalWealth) 
```

并且仍然得到`true`作为返回值！

如果数组为空，JavaScript 直接返回`true`，而不调用谓词。

这是因为在逻辑中，你可以说任何关于空集元素的事情，这被认为是真的，或者更准确地说是空无一物的真。这种逻辑在日常使用中可能看起来毫无意义，但这就是逻辑的工作方式。阅读上面链接的 wiki 页面以了解更多信息。

因此，如果你得到`true`作为`every`的返回值，你应该知道数组可能是空的。

另一方面，`some`在空数组上直接返回`false`,没有任何对`predicate`的调用，也没有任何奇怪之处。

### 3.2 不存在的元素被忽略 [#](#non-existing-elements-are-ignored)

如果你的数组有如下的漏洞，它们会被`every` / `some`忽略:

```
const myUntiddyArry = [1,,,3,,42];
```

### 3.3 谓词中的数组变异 [#](#mutating-the-array-in-the-predicate)

我不会在这里讨论这种情况，因为在大多数情况下，改变原始数组只会使事情变得复杂，并为 bug 留出更多空间。

如果你真的需要或者感兴趣，请阅读 [spec](https://tc39.es/ecma262/multipage/indexed-collections.html#sec-array.prototype.every) 中的注释了解详情。

## 4 对你的挑战 [#](#a-challenge-for-you)

在 JavaScript 中用`some`表示`every`，用`every`表示`some`。

我希望你也能感受到我发现这段关系时的无限喜悦和惊奇！

<details style="align-self: flex-start; margin: 1.5em 0 1.5em; width: 100%;"><summary>Solution</summary>

让我们一步一步来。首先让我们试着用`some`来表达`every`:

*   对于数组的每个元素，谓词为真。
*   对于数组的某些元素，谓词不为真，这是不成立的。

我们可以把它翻译成 JavaScript，如下所示:

```
const myEvery = (arr, predicate) => !arr.some(e => !predicate(e));

```

现在我们用`every`来表示`some`。和以前差不多。只是`some`换成了`every`。试着理解正在发生的事情:

*   对于数组的某些元素，谓词为真。
*   对于数组中的每个元素，谓词都不为真，这是不对的。

在 JavaScript 中:

```
const mySome = (arr, predicate) => !arr.every(e => !predicate(e));

```

注意，当`arr`为空时，上述实现也可以工作。为了简单起见，我排除了`predicate`和`thisArg`的其他参数。如果你有兴趣，试着自己添加这些细节。在这个过程中，你可能会学到一个或几个东西！</details> 

感谢阅读！我希望这篇文章是有帮助的。查看我的其他文章[这里](https://www.freecodecamp.org/news/author/ashutoshbw/)。让我们在[推特](https://twitter.com/ashutoshbw)上连线。