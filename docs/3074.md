# 在 JavaScript 中搜索数组的四种不同方法

> 原文：<https://www.freecodecamp.org/news/4-methods-to-search-an-array/>

JavaScript 中有不同的方法可以用来搜索数组中的项目。选择哪种方法取决于您的具体用例。

例如，您是否希望获取数组中满足特定条件的所有项目？是否要检查是否有符合条件的项目？是否要检查数组中是否有特定的值？还是想找到数组中值的索引？

对于所有这些用例，JavaScript 的 Array.prototype 方法已经涵盖了。在本文中，我们将讨论四种可以用来在数组中搜索条目的方法。这些方法是:

1.  过滤器
2.  发现
3.  包含
4.  IndexOf

让我们逐一讨论一下。

### Array.filter()

我们可以使用 Array.filter()方法在数组中查找满足特定条件的元素。例如，如果我们想要获取一个大于 10 的数字数组中的所有项目，我们可以这样做:

```
const array = [10, 11, 3, 20, 5];

const greaterThanTen = array.filter(element => element > 10);

console.log(greaterThanTen) //[11, 20]
```

使用 array.filter()方法的语法如下:

```
let newArray = array.filter(callback);
```

在哪里

*   `newArray`是返回的新数组
*   `array`是调用 filter 方法的数组
*   `callback`是应用于数组中每个元素的回调函数

如果数组中没有满足条件的项，则返回一个空数组。你可以在这里阅读更多关于[这个方法的内容。](https://sarahchima.com/blog/javascript-array-filter/)

有些时候，我们不需要满足某个条件的所有元素。我们只需要一个符合条件的元素。在这种情况下，您需要 find()方法。

### Array.find()

我们使用 Array.find()方法来查找满足特定条件的第一个元素。就像 filter 方法一样，它将回调作为参数，并返回满足回调条件的第一个元素。

让我们对上面例子中的数组使用 find 方法。

```
const array = [10, 11, 3, 20, 5];

const greaterThanTen = array.find(element => element > 10);

console.log(greaterThanTen)//11
```

array.find()的语法是

```
let element = array.find(callback);
```

回调是对数组中的每个值执行的函数，它有三个参数:

*   `element` -被迭代的元素(必需)
*   `index` -当前元素的索引/位置(可选)
*   `array`—`find`被调用的数组(可选)

但是请注意，如果数组中没有满足条件的条目，它将返回`undefined`。

但是，如果您想检查某个元素是否在数组中呢？你是怎么做到的？

### Array.includes()

includes()方法确定数组是否包含某个值，并根据需要返回 true 或 false。

所以在上面的例子中，如果我们想检查 20 是否是数组中的一个元素，我们可以这样做:

```
const array = [10, 11, 3, 20, 5];

const includesTwenty = array.includes(20);

console.log(includesTwenty)//true
```

你会注意到这个方法和我们考虑过的其他方法之间的区别。此方法接受一个值而不是一个回调作为参数。以下是 includes 方法的语法:

```
const includesValue = array.includes(valueToFind, fromIndex)
```

在哪里

*   `valueToFind`是数组中要检查的值(必选)，以及
*   `fromIndex`是数组中要开始搜索元素的索引或位置(可选)

为了理解指数的概念，让我们再来看看我们的例子。如果我们想检查数组中除了第一个元素以外的其他位置是否包含 10，我们可以这样做:

```
const array = [10, 11, 3, 20, 5];

const includesTenTwice = array.includes(10, 1);

console.log(includesTenTwice)//false
```

### Array.indexOf()

indexOf()方法返回数组中给定元素的第一个索引。如果数组中不存在该元素，则返回-1。

让我们回到我们的例子。让我们在数组中找到 3 的索引。

```
const array = [10, 11, 3, 20, 5];

const indexOfThree = array.indexOf(3);

console.log(indexOfThree)//2
```

它的语法类似于`includes`方法。

```
const indexOfElement = array.indexOf(element, fromIndex)
```

在哪里

*   `element`是数组中要检查的元素(必需的)，并且
*   `fromIndex`是数组中要开始搜索元素的索引或位置(可选)

需要注意的是，`includes`和`indexOf`方法都使用严格等式(' === ')来搜索数组。如果值是不同的类型(例如“4”和“4”)，它们将分别返回`false`和`-1`。

## 摘要

使用这些数组方法，您不需要使用 for 循环来搜索数组。根据您的需要，您可以决定哪种方法最适合您的用例。

以下是何时使用每种方法的总结:

*   如果你想在一个数组中找到满足特定条件的所有项目，使用`filter`。
*   如果您想检查至少一个项目是否满足特定条件，请使用`find`。
*   如果你想检查一个数组是否包含一个特定的值，使用`includes`。
*   如果你想在一个数组中找到一个特定项目的索引，使用`indexOf`。

**想在我发表新文章时得到通知吗？[点击这里](https://mailchi.mp/69ea601a3f64/join-sarahs-mailing-list)。**