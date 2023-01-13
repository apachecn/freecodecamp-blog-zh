# JavaScript Range——如何用？JS ES6 中的 from()

> 原文：<https://www.freecodecamp.org/news/javascript-range-create-an-array-of-numbers-with-the-from-method/>

`.from()`方法是 JavaScript ES6 中`Array`对象的静态方法。它从类似数组或可迭代的对象(如`map`和`set`)创建一个新的浅拷贝数组实例。

此方法从任何具有长度属性的对象中返回一个数组。您可以使用它来创建指定范围内的数字数组。

在本文中，您将了解到`.from()`静态方法是什么，它是如何工作的，以及如何用 JavaScript 创建一系列数字。

如果你赶时间，这里有一个方法可以帮你找到产品系列:

```
const arrayRange = (start, stop, step) =>
    Array.from(
    { length: (stop - start) / step + 1 },
    (value, index) => start + index * step
    );

console.log(arrayRange(1, 5, 1)); // [1,2,3,4,5] 
```

你可以继续阅读这篇短文来理解它是如何工作的。

## JavaScript 中的`.from()`方法是如何工作的

方法从任何类似数组或可迭代的对象中返回一个数组。该方法接受一个强制参数和两个其他可选参数:

```
// Syntax
Array.from(arraylike, mapFunc, thisArg) 
```

*   `arraylike` -要转换为数组的类数组或可迭代对象。
*   `mapFunc` -这是一个可选参数。对每个元素调用 Map 函数。
*   `thisArg` -该值在作为`this`执行`mapFunc`时使用。它也是可选的。

为了了解这是如何工作的，让我们使用`Array.from()`方法从一个字符串创建一个数组:

```
let newArray = Array.from("freeCodeCamp");

console.log(newArray); // ["f","r","e","e","C","o","d","e","C","a","m","p"] 
```

在上面的例子中，`Array.from()`方法返回了一个字符串数组。还可以使用方法从任何具有长度属性的对象返回一个数组，该属性指定对象中的元素数。

```
let arrayLike = {0: 1, 1: 2, 2: 3, length: 3};
let newArray = Array.from(arrayLike);

console.log(newArray); // [1,2,3] 
```

您还可以引入为每个元素调用的 map 函数。例如，如果您想通过将每个数组项乘以一个特定的数字来操作每个数组项:

```
let arrayLike = {0: 1, 1: 2, 2: 3, length: 3};
let newArray = Array.from(arrayLike, x => x * 2);

console.log(newArray); // [2,4,6] 
```

**注意:** `.from()`是一个静态方法，这也是它使用`Array`类名的原因。只能当`Array.from()`不能当`myArray.from()`，这里的`myArray`是一个数组。它将返回 undefined。

## 如何用`.from()`方法创建一个数字序列

`Array.from()`方法使您可以使用 map 函数创建一个数字序列:

```
let newArray = Array.from({ length: 7 }, (value, index) => index);

console.log(newArray); // [0,1,2,3,4,5,6] 
```

上面的方法创建了一个 7 个元素的数组，默认情况下用`undefined`初始化。但是使用 map 函数，现在使用的是索引值，而不是它的实际值 undefined。

如果您使用它的实际值，您将获得一个包含 7 个元素(基于长度)的数组，其值为 undefined:

```
let newArray = Array.from({ length: 7 }, (value, index) => value);

console.log(newArray); 
// returns [undefined,undefined,undefined,undefined,undefined,undefined,undefined] 
```

## 如何用`.from`方法创建一个数字范围

现在你知道了如何创建一个包含一系列数字的数组。但是当您创建一个范围时，您希望这些数字从一个指定的值开始，到一个指定的值结束。例如，在 4 和 8 之间的数字是 4，5，6，7，8。

您还可以指定是否需要指定范围内的奇数或偶数数组。所有这些都可以用`Array.from()`方法实现。

```
const arrayRange = (start, stop, step) =>
    Array.from(
    { length: (stop - start) / step + 1 },
    (value, index) => start + index * step
    ); 
```

在上面的代码中，类似数组的对象的长度是通过从范围中的第一个数字减去最后一个数字，然后除以步长加 1 来定义的。这将给出数组中元素的确切数目。

在 map 函数中，起始编号被添加到每个元素的索引中(记住，该值总是未定义的)，并乘以步长值。这个映射函数为每个元素运行，并帮助计算每个元素的值。

让我们用几个例子来试试这个方法:

```
// Generate numbers between range 2 and 7
let range = arrayRange(2, 7, 1);
console.log(range); // [2,3,4,5,6,7]

// Generate even numbers between range 2 and 7
let evenRange = arrayRange(2, 7, 2);
console.log(evenRange); // [2,4,6]

// Generate odd numbers between range 1 and 5
let oddRange = arrayRange(1, 5, 2);
console.log(oddRange); // [1,3,5] 
```

## 包扎

在本文中，您已经学习了如何用`Array.from()`方法创建一个数字数组。您还学习了`Array.from()`方法是如何工作的。

请记住，在 JavaScript 中还有其他选项来创建一系列数字——在本教程中，我们只关注`.from()`。

祝编码愉快！