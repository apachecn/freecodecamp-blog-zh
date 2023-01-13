# 如何在 JavaScript 中分割和拼接数组

> 原文：<https://www.freecodecamp.org/news/how-to-slice-and-splice-arrays-in-javascript-72bbca45006/>

`.slice()`和`.splice()`是 JavaScript 中类似的方法。他们看起来相似，听起来相似，做的事情也相似。由于这些原因，了解它们之间的区别是很重要的。此外，它们经常被使用，所以了解它们的用法对任何软件开发人员来说都是很好的早期学习。

在本文中，我们将看看如何在一个特定的算法脚本挑战中使用它们。我们将把一个数组中的元素插入到另一个数组中，并在不改变原始数组的情况下返回合并后的数组。

#### 算法指令

> 给你两个数组和一个索引。

> 使用数组方法`slice`和`splice`将第一个数组的每个元素依次复制到第二个数组中。

> 开始在第二个数组的索引`n`处插入元素。

> 返回结果数组。函数运行后，输入数组应该保持不变。

```
function frankenSplice(arr1, arr2, n) {
  return arr2;
}

frankenSplice([1, 2, 3], [4, 5, 6], 1);
```

#### 提供的测试案例

*   `frankenSplice([1, 2, 3], [4, 5], 1)`应该返回`[4, 1, 2, 3, 5]`。
*   `frankenSplice([1, 2], ["a", "b"], 1)`应该返回`["a", 1, 2, "b"]`。
*   `frankenSplice(["claw", "tentacle"], ["head", "shoulders", "knees", "toes"], 2)`应该返回`["head", "shoulders", "claw", "tentacle", "knees", "toes"]`。
*   第一个数组中的所有元素都应该按照原来的顺序添加到第二个数组中。
*   函数运行后，第一个数组应该保持不变。
*   函数运行后，第二个数组应该保持不变。

### 解决方案#1:。slice()，。splice()和 spread 运算符

#### 佩达克

**理解问题**:我们有一个输入，一个字符串。我们的输出也是一个字符串。最终，我们希望返回输入字符串，其中每个单词的第一个字母都要大写，而且只能是第一个字母。

**示例/测试用例**:我们提供的测试用例显示，我们应该只在每个单词的开头有一个大写字母。我们需要小写其余的。所提供的测试案例还表明，在用符号而不是空格分隔的奇怪复合词方面，我们没有遇到任何难题。这对我们来说是个好消息！

**数据结构**:我们将不得不把输入字符串转换成一个数组，以便分别操作每个单词。

让我们来聊聊`.slice()`和`.splice()`:

首先让我们解决`[.slice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice)`:

提取字符串的一部分，并作为新字符串返回。如果你在一个字符串上调用`.slice()`而没有传递任何附加信息，它将返回整个字符串。

```
"Bastian".slice()
// returns "Bastian"
```

这在这个算法脚本挑战中对我们很有用，因为指令告诉我们不应该直接修改输入数组。所以我们需要复制一份。

现在我们来看看`[.splice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)`:

通过删除或替换现有元素和/或添加新元素来更改数组的内容。

我们可以传递几个参数来决定从哪里开始删除，删除多少，插入什么。`start`是一个数字，告诉`.splice()`从哪个索引开始删除元素。`deleteCount`告诉`.splice()`要删除多少元素。

等一下！不想删东西怎么办？如果只是想插入元素呢？那很好。只需将`deleteCount`设置为零。现在我们可以开始添加项目了。只需用逗号分隔每个元素，就像这样`item1, item2, item3, item4`。

```
.splice(start, deleteCount, item1, item2, item3, etc.)
```

这个算法脚本挑战需要记住的另一个概念是[扩展操作符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)。ES6 给了我们 spread 操作符，看起来像椭圆——一行只有三个点。

当您希望将数组元素用作函数的参数时，最常使用 spread 运算符。这正是我们在这次挑战中要做的。我们不想将整个数组`arr1`插入到`arr2`中。我们想把`arr1`的每个元素都插入到`arr2`中。

**算法**:

1.  创建一个`arr2`的副本。
2.  从`n`指定的`arr2`中的索引开始，将`arr1`的所有元素插入到`arr2`中。
3.  返回组合数组。

**代码**:见下图！

```
function frankenSplice(arr1, arr2, n) {
  // Create a copy of arr2.
  let combinedArrays = arr2.slice()
  //                   [4, 5, 6]

  // Insert all the elements of arr1 into arr2 beginning
  // at the index specified by n. We're using the spread
  // operator "..." to insert each individual element of 
  // arr1 instead of the whole array.
  combinedArrays.splice(n, 0, ...arr1)
  //                   (1, 0, ...[1, 2, 3])
  //                   [4, 1, 2, 3, 5, 6]

  // Return the combined arrays.
  return combinedArrays
}

frankenSplice([1, 2, 3], [4, 5, 6], 1);
```

无注释:

```
function frankenSplice(arr1, arr2, n) {
  let combinedArrays = arr2.slice()
  combinedArrays.splice(n, 0, ...arr1)
  return combinedArrays
}

frankenSplice([1, 2, 3], [4, 5, 6], 1);
```

### 解决方案 2:。slice()，。splice()和 for 循环

#### 佩达克

**理解问题**:我们有一个输入，一个字符串。我们的输出也是一个字符串。最终，我们希望返回输入字符串，其中每个单词的第一个字母都要大写，而且只能是第一个字母。

**示例/测试用例**:我们提供的测试用例显示，我们应该只在每个单词的开头有一个大写字母。我们需要小写其余的。所提供的测试案例还表明，在用符号而不是空格分隔的奇怪复合词方面，我们没有遇到任何难题。这对我们来说是个好消息！

**数据结构**:我们将不得不把输入字符串转换成一个数组，以便分别操作每个单词。

让我们来聊聊`.slice()`和`.splice()`:

首先让我们解决`[.slice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice)`:

提取字符串的一部分，并作为新字符串返回。如果你在一个字符串上调用`.slice()`而没有传递任何附加信息，它将返回整个字符串。

```
"Bastian".slice()
// returns "Bastian"
```

这在这个算法脚本挑战中对我们很有用，因为指令告诉我们不应该直接修改输入数组。所以我们需要复制一份。

现在我们来看看`[.splice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)`:

通过删除或替换现有元素和/或添加新元素来更改数组的内容。

我们可以传递几个参数来决定从哪里开始删除，删除多少，插入什么。`start`是一个数字，告诉`.splice()`从哪个索引开始删除元素。`deleteCount`告诉`.splice()`要删除多少元素。等一下！不想删东西怎么办？如果只是想插入元素呢？那很好。只需将`deleteCount`设置为零。现在我们可以开始添加项目了。只需用逗号分隔每个元素，就像这样`item1, item2, item3, item4`。

```
.splice(start, deleteCount, item1, item2, item3, etc.)
```

与前面的解决方案不同，我们在这里不会使用 spread 操作符。我们将使用 for 循环来从`arr1`中一次提取一个元素，并将它们插入到`arr2`中。

这里的技巧是每次循环运行时将`n`加 1，否则`arr1`的元素在插入`arr2`时不会以正确的顺序结束。

**算法**:

1.  创建一个`arr2`的副本。
2.  使用 for 循环，从索引`n`开始将`arr1`的每个元素插入到`arr2`中。
3.  每次循环运行时，将`n`增加 1。
4.  返回组合数组。

**代码**:见下图！

```
function frankenSplice(arr1, arr2, n) {
  // Create a copy of arr2.
  let combinedArrays = arr2.slice()
  // Using a for loop, insert each element of arr1
  // into combinedArrays starting at index n.
  for (let i = 0; i < arr1.length; i++) {
      combinedArrays.splice(n, 0, arr1[i])
  //       [4, 5, 6].splice(1, 0, 1)
  //    [4, 1, 5, 6].splice(2, 0, 2)
  // [4, 1, 2, 5, 6].splice(3, 0, 3)
  // [4, 1, 2, 3, 5, 6]

  //  increment n by 1 each time the loop runs
      n++
  }
  // Return the combined arrays.
  return combinedArrays
}

frankenSplice([1, 2, 3], [4, 5, 6], 1);
```

无注释:

```
function frankenSplice(arr1, arr2, n) {
  let combinedArrays = arr2.slice()
  for (let i = 0; i < arr1.length; i++) {
    combinedArrays.splice(n, 0, arr1[i])
    n++
  }
  return combinedArrays
}

frankenSplice([1, 2, 3], [4, 5, 6], 1);
```

如果你有其他的解决方案和/或建议，请在评论中分享！

#### 本文是系列 [freeCodeCamp 算法脚本](https://medium.com/@DylanAttal/freecodecamp-algorithm-scripting-b96227b7f837)的一部分。

#### 本文引用 [freeCodeCamp 基本算法脚本:切片和拼接](https://learn.freecodecamp.org/javascript-algorithms-and-data-structures/basic-algorithm-scripting/slice-and-splice)

可以在 [Medium](https://medium.com/@DylanAttal) 、 [LinkedIn](https://www.linkedin.com/in/dylanattal/) 、 [GitHub](https://github.com/DylanAttal) 上关注我！