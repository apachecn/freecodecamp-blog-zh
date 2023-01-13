# 在 JavaScript 中比较数组——如何在 JS 中比较两个数组

> 原文：<https://www.freecodecamp.org/news/how-to-compare-arrays-in-javascript/>

当用 JavaScript 处理逻辑时，您可能需要比较两个数组，看它们是否相等。

实际上，这并不困难，因为你会认为我们可以很容易地使用宽松的等式(double equals - `==`)或严格的等式(triple equals - `===`)。但不幸的是，您不能在这种情况下使用它们。

```
let array1 = [11, 22, 33];
let array2 = [11, 22, 33];

console.log(array1 == array2); //false
console.log(array1 === array2); //false 
```

这是因为 JavaScript 数组有一种对象类型:

```
let arrayType = typeof(array1);
console.log(arrayType); //"Object" 
```

对象的比较不是基于它们的值，而是基于变量的引用:

```
console.log(array1[0] == array1[0]); //true
console.log(array1[1] === array1[1]); //true 
```

但这不是你想要的。相反，您希望能够直接比较两个数组并只返回一个布尔值，而不必逐个检查每个元素。

在本文中，您将学习在 JavaScript 中比较两个数组的各种方法，以确定它们是否相似。

## 如何通过转换为字符串来比较两个数组

可以用来比较两个数组的一种常见且非常简单的方法是首先将这些数组转换为字符串形式。

您可以使用两种不同的方法:您可以决定使用`JSON.stringify()`方法将数组转换为 JSON 文本，或者使用`.toString()`方法将数组作为字符串返回。

**注意:**两种方法不同，如下图所示:

```
let array = [11, 22, 33];
console.log(JSON.stringify(array)); //"[11,22,33]"
console.log(array.toString()); //"11,22,33" 
```

### 方法一:如何使用`JSON.stringify()`

该方法允许您通过将数组转换为 JSON 字符串来序列化每个数组。然后，您可以比较这两个 JSON 字符串。

```
let array1 = [11, 22, 33];
let array2 = [11, 22, 33];

console.log(JSON.stringify(array1) === JSON.stringify(array2)); //true 
```

我们还可以创建一个可重用的函数，帮助我们比较传递给它的任意两个数组:

```
const compareArrays = (a, b) => {
  return JSON.stringify(a) === JSON.stringify(b);
};

let array1 = [11, 22, 33];
let array2 = [21, 22, 23];
let array3 = [11, 22, 33];

console.log(compareArrays(array1, array2)); //false
console.log(compareArrays(array1, array3)); //true 
```

### 方法二:如何使用`.toString()`

与`JSON.stringify()`类似，该方法将任何数据类型转换为字符串，并且可以类似地将对象转换为字符串。

```
let array1 = [11, 22, 33];
let array2 = [11, 22, 33];

console.log(array1.toString() === array2.toString()); //true 
```

您还可以创建一个可重用的函数，帮助您比较传递给它的任意两个数组:

```
const compareArrays = (a, b) => {
  return a.toString() === b.toString();
};

let array1 = [11, 22, 33];
let array2 = [21, 22, 23];
let array3 = [11, 22, 33];

console.log(compareArrays(array1, array2)); //false
console.log(compareArrays(array1, array3)); //true 
```

**注意:**你应该使用`JSON.stringify()`方法，因为它只序列化你的 JavaScript 数组。该数组仍然采用数组的形式，但它只是被解析为数组的字符串版本。

## 如何通过截取两个数组的值来比较它们

上述方法在某些情况下有所欠缺。例如，当一个数组的值为`null`而另一个数组的值为`undefined`时，当我们使用严格比较时，我们默认得到`false`，这是正确的:

```
console.log(null === undefined); //false 
```

但是当我们使用`JSON.Strigify()`或`.toString()`方法时，你会得到`true`，这是不应该的:

```
let array1 = [11, null, 33];
let array2 = [11, undefined, 33];

console.log(JSON.stringify(array1) === JSON.stringify(array2)); //true
console.log(array1.toString() === array2.toString()); //true 
```

更好的方法是比较数组的长度，然后遍历并比较数组的每个元素。

### 方法 1:使用`every()`

方法帮助你为数组的每个元素执行一个函数。该功能被称为回叫功能。它可以访问一些基本参数，如元素、索引等，我们可以在函数中使用这些参数:

```
// Syntax
array.every((currentValue, index, arr)=> { ... }) 
```

在这个方法中，我们将首先测试两个数组的长度是否相当。然后，我们将遍历一个数组，并使用它的索引将它的元素与第二个数组中的元素进行比较:

```
const compareArrays = (a, b) =>
  a.length === b.length &&
  a.every((element, index) => element === b[index]);

let array1 = [11, 22, 33];
let array2 = [21, 22, 23];
let array3 = [11, 22, 33];

console.log(compareArrays(array1, array2)); //false
console.log(compareArrays(array1, array3)); //true 
```

而当我们把 null 和 undefined 作为数组元素的一部分时，它将能够检测出它们是不一样的:

```
const compareArrays = (a, b) =>
  a.length === b.length && a.every((element, index) => element === b[index]);

let array1 = [11, null, 33];
let array2 = [21, 22, 23];
let array3 = [11, undefined, 33];

console.log(compareArrays(array1, array2)); //false
console.log(compareArrays(array1, array3)); //false 
```

### 方法 2:使用 for 循环

every()方法有更好的语法。然而，我们可以实现该方法的另一种方式是使用其他迭代方法，如`for`循环、`forEach()`或`map()`以及`if`语句。这些对于新手来说更容易掌握。

```
const compareArrays = (a, b) => {
  if (a.length !== b.length) return false;
  else {
    // Comparing each element of your array
    for (var i = 0; i < a.length; i++) {
      if (a[i] !== b[i]) {
        return false;
      }
    }
    return true;
  }
};

let array1 = [21, null, 33];
let array2 = [21, 22, 23];
let array3 = [21, undefined, 33];
let array4 = [21, 22, 23];

console.log(compareArrays(array1, array2)); //false
console.log(compareArrays(array1, array3)); //false
console.log(compareArrays(array2, array4)); //true 
```

在上面的两种方法中，您将首先检查长度，因为如果长度不相等，它自动意味着两个数组不相等，然后返回`false`。

如果长度相等，那么我们开始检查每个元素。只要两个数组中同一`index`上的两个元素不同，它就返回`false`。

## 包扎

本文介绍了如何使用两种主要方法在 JavaScript 中比较两个数组。

这些方法是在比较之前将数组转换为字符串，或者您可以循环检查它们的值是否相似，以便进行更详细的比较。

**注意:**double equals 检查两个值是否相等，triple equals 检查两个值及其数据类型是否相等。你可以在这里阅读更多关于[两种类型的平等](https://www.freecodecamp.org/news/javascript-triple-equals-sign-vs-double-equals-sign-comparison-operators-explained-with-examples/)。

祝编码愉快！