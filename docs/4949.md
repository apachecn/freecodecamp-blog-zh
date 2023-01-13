# 如何在 JavaScript 中从数组中移除 falsy 值

> 原文：<https://www.freecodecamp.org/news/how-to-remove-falsy-values-from-an-array-in-javascript-e623dbbd0ef2/>

在 JavaScript 中有很多方法可以从数组中移除元素，但是从数组中移除所有 falsy 值的最简单的方法是什么呢？为了回答这个问题，我们将在算法脚本挑战的上下文中仔细研究真值与假值以及类型强制。

#### 算法指令

> 从数组中删除所有假值。

> JavaScript 中的 Falsy 值有`false`、`null`、`0`、`""`、`undefined`和`NaN`。

> 提示:尝试将每个值转换为布尔值。

#### 提供的测试案例

*   `bouncer([7, "ate", "", false, 9])`应该返回`[7, "ate", 9]`。
*   `bouncer(["a", "b", "c"])`应该返回`["a", "b", "c"]`。
*   `bouncer([false, null, 0, NaN, undefined, ""])`应该返回`[]`。
*   `bouncer([1, null, NaN, 2, undefined])`应该返回`[1, 2]`。

### 解决方案#1:。过滤器( )和布尔( )

#### 佩达克

**理解问题**:我们有一个输入，一个数组。我们的目标是从数组中移除所有的 falsy 值，然后返回数组。

freeCodeCamp 的好心人告诉我们，JavaScript 中的 falsy 值有`false`、`null`、`0`、`*""*`、`undefined`和`NaN`。

他们也给了我们一个重要的暗示！他们建议将数组中的每一个值都转换成一个布尔值来完成这个挑战。我觉得这是一个很好的暗示！

**示例/测试用例**:我们提供的测试用例告诉我们，如果输入数组只包含 falsy 值，那么我们应该只返回一个空数组。这很简单。

数据结构:我们将坚持使用数组。

再说说`[.filter()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)`:

创建一个新数组，包含所有通过测试的元素，测试由提供的函数实现。

换句话说，`.filter()`遍历数组中的每个元素，并保留通过某个测试的所有元素。数组中所有没有通过测试的元素都被过滤掉了，它们被删除了。

例如，如果我们有一个数字数组，并且我们只想要大于 100 的数字，我们可以使用`.filter()`来实现:

```
let numbers = [4, 56, 78, 99, 101, 150, 299, 300]numbers.filter(number => number > 100)// returns [ 101, 150, 299, 300 ]
```

让我们讨论一下将每个元素转换为布尔值的提示。这是一个很好的提示，因为我们可以使用`.filter()`返回只包含真值的数组。

我们将通过 [JavaScript 类型转换](https://www.w3schools.com/js/js_type_conversion.asp)来完成。

JavaScript 为我们提供了将一种数据类型转换成另一种数据类型的有用函数。`String()`转换为字符串，`Number()`转换为数字，`Boolean()`转换为布尔值。

例如:

```
String(1234)// returns "1234"
```

```
Number("47")// returns 47
```

```
Boolean("meow")// returns true
```

`Boolean()`是我们在这次挑战中要实现的功能。如果提供给`Boolean()`的参数是真的，那么`Boolean()`将返回`true.`如果提供给`Boolean()`的参数是假的，那么`Boolean()`将返回`false`。

这对我们很有用，因为我们从指令中知道，在 JavaScript 中只有`false`、`null`、`0`、`*""*`、`undefined`和`NaN`是 falsy。其他所有的价值都是真实的。我们知道，如果我们将输入数组中的每个值转换为布尔值，我们可以删除所有计算结果为`false`的元素，这将满足这个挑战的要求。

**算法**:

1.  确定`arr`中的哪些值是错误的。
2.  删除所有虚假值。
3.  返回只包含真值的新数组。

**代码**:见下图！

不带注释并删除局部变量:

如果你有其他的解决方案和/或建议，请在评论中分享！

#### 本文是系列 [freeCodeCamp 算法脚本](https://medium.com/@DylanAttal/freecodecamp-algorithm-scripting-b96227b7f837)的一部分。

#### 本文参考 [freeCodeCamp 基础算法脚本:Falsy Bouncer](https://learn.freecodecamp.org/javascript-algorithms-and-data-structures/basic-algorithm-scripting/falsy-bouncer) 。

可以在 [Medium](https://medium.com/@DylanAttal) 、 [LinkedIn](https://www.linkedin.com/in/dylanattal/) 、 [GitHub](https://github.com/DylanAttal) 上关注我！