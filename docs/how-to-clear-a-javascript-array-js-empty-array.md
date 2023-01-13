# 如何清除 JavaScript 数组-JS 空数组

> 原文：<https://www.freecodecamp.org/news/how-to-clear-a-javascript-array-js-empty-array/>

JavaScript 中有多种方法可以清空现有数组。清空数组意味着移除数组中的所有值。

在本文中，我将展示和解释两种清除数组的方法。

## 1.如何通过修改 Length 属性来清除数组

数组的长度属性是**可读**和**可写**。

当您读取属性(`array.length`)时，它返回数组的长度，即数组中值的数量。当你写属性的时候(也就是修改数组，像`array.length = 10`)，它改变了数组的长度和数组中值的个数。

如果修改后的长度小于原始长度，超出的值将被删除。我的意思是:

```
const array = [1, 2, 3]
array.length = 2

console.log(array)
// [1, 2] 
```

因为新长度小于原始长度，所以超出的值( **3** ，在本例中)将被删除。

但是，如果新长度大于原始长度，数组将被填充`undefined`值以弥补新长度:

```
const array = [1, 2, 3]
array.length = 4

console.log(array)
// [1, 2, 3, undefined] 
```

现在您已经了解了如何使用`length`属性来修改数组，下面是清空数组的方法:

```
const array = [1, 2, 3]
array.length = 0

console.log(array)
// [] 
```

长度为 0 时，数组中的每个值都被删除，数组变成空的。

## 2.如何通过重新赋值来清空数组

您可以为最初分配了非空数组的变量重新分配一个新值(空数组)。

如果你用`const`声明一个变量，你不能重新分配它:

```
const array = [1, 2, 3]
array = []

console.log(array) 
```

上面的代码会抛出一个 **TypeError:赋值给常量变量**的错误。但是如果您用`let`声明了这个变量，那么您可以用一个空数组值来重新分配它:

```
let array = [1, 2, 3]
array = []

console.log(array)
// [] 
```

现在，你有了空数组。

感谢您的阅读，祝您编码愉快！