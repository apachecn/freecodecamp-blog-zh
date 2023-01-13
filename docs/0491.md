# 如何使用 flat()和 flatMap()方法在 JavaScript 中展平数组

> 原文：<https://www.freecodecamp.org/news/flat-and-flatmap-javascript-array-methods/>

在这篇文章中，我将解释如何使用 ES2019 (EcmaScript 2019)中引入的新数组方法—`flat()`和`flatMap()`。您可以使用这些方法来展平数组。

这些方法非常有用且易于使用。在你的下一个项目中使用这些方法真的会很酷。拿一杯咖啡，让我们开始喝吧。

## 先决条件

您应该熟悉 JavaScript 中数组的概念，以便理解本教程。

## 什么是数组？

数组对象是在不同的编程语言中使用的数据结构，用于存储不同的数据集合。

### JavaScript 中的数组示例

```
const array = [1, 2, 3, true, null, undefined];

console.log(array);

// expected output
1, 2, 3, true, null, undefined 
```

在上面的代码中，我们分配了一个名为 **`arr`** 的变量，并在其中存储了不同数据类型的不同元素/数据。

为 1 的第一个元素位于索引 0 处。最后一个元素 undefined 位于索引 5 处。

不要忘记数组对象是零索引的，这意味着第一个元素从零索引开始。

## JavaScript 数组的特征

*   数组是零索引的:这仅仅意味着数组中的第一个元素在索引 0 处。索引在这里是位置的意思。在 JavaScript 和其他编程语言中，正确的用词是 index 而不是 position。
*   **JavaScript 数组可以包含不同的数据类型:**这意味着 JavaScript 数组可以混合包含数字、字符串、布尔值、空值等等。
*   **JavaScript 创建浅层副本:**这意味着你可以创建一个数组的不同副本——也就是说不改变原始数组。

## 新的 JavaScript 数组方法

数组是 JavaScript 中用于存储数据的最流行的数据结构之一。

有不同的数组方法可以用来简化操作，让您的生活更轻松。这些方法包括`reduce()`、`filter()`、`map()`、`flat()`、`flatMap()`等等。

但是在本文中，我们不会讨论 JavaScript 中使用的所有数组方法。相反，在这里，我们将讨论新的`flat()`和`flatMap()`方法，您可以使用它们将原始数组转换成新数组。

## 如何在 JavaScript 中使用`flat`数组方法

您使用`flat`方法将一个原始数组转换成一个新数组。它通过收集并将一个数组中的子数组连接成一个数组来实现这一点。

关于这个数组方法的一个重要注意事项是，您可以根据数组的深度将原始数组计算到另一个数组中。这种方法非常有用，因为它使得计算数组的操作变得更加容易。

## `flat()`数组方法的例子

### 如何设置深度参数

```
array.flat(depth); 
```

默认情况下，深度值为 1，您可以将其留空。深度值采用数字作为其数据类型。

#### `flat()`数组方法示例 1

`array.flat(1)`等于`array.flat()`。

`array.flat(2);`

上面的深度是 **2** 。

```
const arr = ["mon", "tues", ["wed", "thurs", ["fri", "sat"]], "sun"] ;

console.log(arr.flat());

// expected output 
["mon", "tues", "wed", "thurs", Array ["fri", "sat"], "sun"]; 
```

那么这段代码中发生了什么呢？

首先，数组包含两个子数组`["wed", "thurs", ["fri", "sat"]]`。

我们在名为 **`arr`** 的数组上使用 **flat()** 方法来连接第一个子数组，因为我们没有在 **flat()** 方法中指定任何深度值。回想一下，默认深度值是 **1** 。

所以你可以猜测如果深度值是 2，数组对象会是什么，对吗？‌

#### `flat()`数组方法示例 2

```
const arr = [1, 2, [3, 4, 5], 6, 7];

console.log(arr.flat());

// expected output 
[1, 2, 3, 4, 5, 6, 7] 
```

在上面的代码中，数组包含一个名为`[3, 4, 5]`的子数组。

我们在名为 **`arr`** 的数组上使用 **flat()** 方法将两个数组连接成一个数组。

#### `flat()`数组方法示例 3

```
//depth 2 example 
const arr2 = [[[1, 2], 3, 4, 5]] ;

console.log(arr2.flat(2));

// expected output 
[1, 2, 3, 4, 5] 
```

在上面的代码中，名为 **`arr2`** 的数组包含两个子数组。

我们在`arr2`上使用 **flat()** 方法将两个数组连接成一个数组，因为在 flat(2)方法中使用了深度值 2。快速浏览一下上面的内容，看看深度值有什么作用。‌

array 方法是递归连接数组中元素的好方法。

## 如何在 JavaScript 中使用`flatMap`数组方法

`flatMap()`方法结合使用`map()`和`flat()`方法来执行操作。

`flatMap()`遍历一个数组的元素，并将元素连接成一级。`flatMap()`接受一个回调函数，该函数接受原始数组的当前元素作为参数。

### `flatMap()`数组方法示例

```
const arr3 = [1, 2, [4, 5], 6, 7, [8]] ;

console.log(arr3.flatMap((element) => element)); 

// expected output 
[1, 2, 4, 5, 6, 7, 8] 
```

在上面的代码中，名为 **`arr3`** 的数组包含两个不同的子数组。

我们在数组上使用了 **flatMap()** 方法，方法是传入一个回调函数，**(元素)= >元素**，该函数遍历数组，然后将数组连接成一个数组。

有时，您会遇到这样的情况:数组有多个深度，您决定将新数组更改为一个基面。那么您必须在使用`flatMap()`方法之后立即使用`flat()`方法。

### 这里有一个例子:

```
const arr4 = [1, 2, [3, [4, 5, [6, 7]]]] ;

console.log(arr4.flatMap((element) => element).flat(2)) ;

// expected output 
[1, 2, 3, 4, 5, 6, 7] 
```

在上面的代码中，名为 **`arr4`** 的数组包含三个子数组。

我们在数组上使用了 **flatMap()** 方法，方法是传入一个回调函数，**(元素)= >元素**，该函数遍历数组，然后连接数组。

我们使用了 **flat(2)** 方法，通过传入深度为 **2** 的值，进一步将数组连接成一个数组。

请确保在不通过 flat(2)方法的情况下练习上面的示例，并查看不同之处。

这就是开始使用这两个新的数组方法所需的全部内容。

## 摘要

在本文中，我简要讨论了什么是数组以及数组在 JavaScript 中有多有用。然后，我们了解了 ECMAScript 2019 中引入的两个新的重要数组方法，这两个方法允许您将原始数组更改为新数组。

这些新的数组方法是`flat()`和`flatMap()`方法。

使用`flat()`方法将子数组递归地连接成一个数组。`flat()`方法将深度值作为其参数，该参数是可选的，取决于您希望展平(连接)的数组的深度。默认情况下，`flat()`方法接受 1 作为深度。

另一方面，`flatMap()`的工作方式基本相同，只是它是`map()`和`flat()`方法的组合。`flatMap()`遍历数组中的元素并连接这些元素。

当你把一个原始数组变成一个新数组时，这两个新方法都很有用。它们值得你在下一个或大或小的项目中尝试。

### 其他有用的资源:

*   [平台映射上的 MDN 引用()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap)
*   [平台上的 MDN 参考值](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)()