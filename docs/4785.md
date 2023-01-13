# 如何在 JavaScript 中找到数组中数字所属的索引

> 原文：<https://www.freecodecamp.org/news/how-to-find-the-index-where-a-number-belongs-in-an-array-in-javascript-9af8453a39a8/>

在编写算法时，排序是一个非常重要的概念。有各种各样的排序:冒泡排序、壳排序、块排序、梳状排序、鸡尾酒排序、gnome 排序——[这些不是我编的](https://en.wikipedia.org/wiki/Sorting_algorithm)！

这个挑战让我们得以一窥各种奇妙的世界。我们必须从最小到最大对一个数字数组进行排序，并找出一个给定的数字在数组中的位置。

#### 算法指令

> 返回一个值(第二个参数)排序后应插入数组(第一个参数)的最低索引。返回值应该是一个数字。

> 例如，`*getIndexToIns([1,2,3,4], 1.5)*`应该返回`*1*`，因为它大于`*1*`(索引 0)，但小于`*2*`(索引 1)。

> 同样，`*getIndexToIns([20,3,5], 19)*`应该返回`*2*`,因为一旦数组被排序，它看起来就像`*[3,5,20]*`和`*19*`小于`*20*`(索引 2)并且大于`*5*`(索引 1)。

```
function getIndexToIns(arr, num) {
  return num;
}

getIndexToIns([40, 60], 50);
```

#### 提供的测试案例

*   `getIndexToIns([10, 20, 30, 40, 50], 35)`应该返回`3`。
*   `getIndexToIns([10, 20, 30, 40, 50], 35)`应返回一个数字。
*   `getIndexToIns([10, 20, 30, 40, 50], 30)`应该返回`2`。
*   `getIndexToIns([10, 20, 30, 40, 50], 30)`应返回一个数字。
*   `getIndexToIns([40, 60], 50)`应该返回`1`。
*   `getIndexToIns([40, 60], 50)`应返回一个数字。
*   `getIndexToIns([3, 10, 5], 3)`应该返回`0`。
*   `getIndexToIns([3, 10, 5], 3)`应返回一个数字。
*   `getIndexToIns([5, 3, 20, 3], 5)`应该返回`2`。
*   `getIndexToIns([5, 3, 20, 3], 5)`应返回一个数字。
*   `getIndexToIns([2, 20, 10], 19)`应该返回`2`。
*   `getIndexToIns([2, 20, 10], 19)`应返回一个数字。
*   `getIndexToIns([2, 5, 10], 15)`应该返回`3`。
*   `getIndexToIns([2, 5, 10], 15)`应返回一个数字。
*   `getIndexToIns([], 1)`应该返回`0`。
*   `getIndexToIns([], 1)`应返回一个数字。

### 解决方案#1:。sort()，。索引 Of()

#### 佩达克

**理解问题**:我们有两个输入，一个数组，一个数字。我们的目标是在输入数字被排序到输入数组中之后，返回它的索引。

**例子/测试用例**:freeCodeCamp 的好心人并没有告诉我们输入数组应该按照哪种方式排序，但是提供的测试用例明确了输入数组应该按照从少到多的顺序排序。

请注意，在最后两个提供的测试用例中有一个边缘用例，其中输入数组是一个空数组。

**数据结构**:由于我们最终返回的是一个索引，坚持使用数组对我们来说是可行的。

我们将利用一个叫做`[.indexOf()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)`的漂亮方法:

`.indexOf()`返回数组中元素出现的第一个索引，如果元素不存在，则返回`-1`。例如:

```
let food = ['pizza', 'ice cream', 'chips', 'hot dog', 'cake']
```

```
food.indexOf('chips')// returns 2food.indexOf('spaghetti')// returns -1
```

我们还将在这里使用`[.concat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)`而不是`[.push()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)`。为什么？因为当你使用`.push()`向一个数组添加一个元素时，它会返回新数组的长度。当您使用`.concat()`向数组添加元素时，它会返回新数组本身。例如:

```
let array = [4, 10, 20, 37, 45]
```

```
array.push(98)// returns 6array.concat(98)// returns [4, 10, 20, 37, 45, 98]
```

**算法**:

1.  将`num`插入`arr`中。
2.  从最少到最多排序。
3.  返回`num`的索引。

**代码**:见下图！

```
function getIndexToIns(arr, num) {
  // Insert num into arr, creating a new array.
     let newArray = arr.concat(num)
  //             [40, 60].concat(50)
  //             [40, 60, 50]

  // Sort the new array from least to greatest.
     newArray.sort((a, b) => a - b)
  // [40, 60, 50].sort((a, b) => a - b)
  // [40, 50, 60]

  // Return the index of num which is now
  // in the correct place in the new array.
     return newArray.indexOf(num);
  // return [40, 50, 60].indexOf(50)
  // 1
}

getIndexToIns([40, 60], 50);
```

没有局部变量和注释:

```
function getIndexToIns(arr, num) {
  return arr.concat(num).sort((a, b) => a - b).indexOf(num);
}

getIndexToIns([40, 60], 50);
```

### 解决方案 2:。sort()，。findIndex()

#### 佩达克

**理解问题**:我们有两个输入，一个数组，一个数字。我们的目标是在输入数字被排序到输入数组中之后，返回它的索引。

**例子/测试用例**:freeCodeCamp 的好心人并没有告诉我们输入数组应该按照哪种方式排序，但是提供的测试用例明确了输入数组应该按照从少到多的顺序排序。

此解决方案需要考虑两种边缘情况:

1.  如果输入数组为空，那么我们需要返回`0`，因为`num`将是该数组中唯一的*元素，因此位于索引`0`。*
2.  *如果从最小到最大排序，`num`属于`arr`的最末端，那么我们需要返回`arr`的长度。*

***数据结构**:由于我们最终返回的是一个索引，坚持使用数组对我们来说是可行的。*

*让我们来看看`[.findIndex()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)`将如何帮助解决这一挑战:*

*`.findIndex()`返回满足所提供测试函数的数组中第一个元素的索引。否则，它返回-1，表示没有元素通过测试。例如:*

```
*`let numbers = [3, 17, 94, 15, 20]
numbers.findIndex((currentNum) => currentNum % 2 == 0)
// returns 2
numbers.findIndex((currentNum) => currentNum > 100)
// returns -1`*
```

*这对我们很有用，因为我们可以使用`.findIndex()`来比较我们的输入`num`和我们的输入`arr`中的每一个数字，并按照从最小到最大的顺序找出它适合的位置。*

***算法**:*

1.  *如果`arr`是空数组，则返回`0`。*
2.  *如果`num`属于已排序数组的末尾，则返回`arr`的长度。*
3.  *否则，如果`arr`从最小到最大排序，则返回索引`num`。*

***代码**:见下图！*

```
*`function getIndexToIns(arr, num) {
  // Sort arr from least to greatest.
    let sortedArray = arr.sort((a, b) => a - b)
  //                  [40, 60].sort((a, b) => a - b)
  //                  [40, 60]

  // Compare num to each number in sortedArray
  // and find the index where num is less than or equal to 
  // a number in sortedArray.
    let index = sortedArray.findIndex((currentNum) => num <= currentNum)
  //            [40, 60].findIndex(40 => 50 <= 40) --> falsy
  //            [40, 60].findIndex(60 => 50 <= 60) --> truthy
  //            returns 1 because num would fit like so [40, 50, 60]

  // Return the correct index of num.
  // If num belongs at the end of sortedArray or if arr is empty 
  // return the length of arr.
    return index === -1 ? arr.length : index
}

getIndexToIns([40, 60], 50);`*
```

*没有局部变量和注释:*

```
*`function getIndexToIns(arr, num) {
  let index = arr.sort((a, b) => a - b).findIndex((currentNum) => num <= currentNum)
  return index === -1 ? arr.length : index
}

getIndexToIns([40, 60], 50);`*
```

*如果你有其他的解决方案和/或建议，请在评论中分享！*

#### *本文是系列 [freeCodeCamp 算法脚本](https://medium.com/@DylanAttal/freecodecamp-algorithm-scripting-b96227b7f837)的一部分。*

#### *本文参考 [freeCodeCamp 基础算法脚本:我属于哪里](https://learn.freecodecamp.org/javascript-algorithms-and-data-structures/basic-algorithm-scripting/where-do-i-belong)。*

*可以在 [Medium](https://medium.com/@DylanAttal) 、 [LinkedIn](https://www.linkedin.com/in/dylanattal/) 、 [GitHub](https://github.com/DylanAttal) 上关注我！*