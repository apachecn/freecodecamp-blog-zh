# 如何在 JavaScript 中合并数组——在 JS 中合并数组

> 原文：<https://www.freecodecamp.org/news/how-to-merge-arrays-in-javascript-array-concatenation-in-js/>

JavaScript 中有多种方法可以合并数组。你可以使用长或短的方法。在本文中，我将展示其中的三种。

在 JavaScript 中使用数组时，有时会希望将多个数组组合在一起。例如，具有来自不同来源的相关数据的数组可以合并成一个数组。

您可以用不同的方式合并数组。让我们看看其中的一些，从我最喜欢的到我最不喜欢的。

如果你想用它来补充你的学习，这里有这篇文章的视频版本。

## 1.如何在 JavaScript 中使用 Spread 运算符

spread 运算符允许您将一个可迭代集合(对象或数组)扩展到另一个集合中。在数组上使用这个操作符，可以将数组的内容合并在一起。

这里有一个例子:

```
const array1 = [1, 2, 3]
const array2 = [4, 5, 6]

const merged = [...array1, ...array2]
// [1, 2, 3, 4, 5, 6] 
```

对于`merged`变量，我们创建一个新的数组，然后将`array1`和`array2`的值分布在其中。现在您可以看到`merged`数组包含了这些数组中的值。

您可以对多个数组使用此运算符:

```
const array1 = [1, 2, 3]
const array2 = [4, 5, 6]
const array3 = [7, 8, 9]

const merged = [...array2, ...array3, ...array1]
// [4, 5, 6, 7, 8, 9, 1, 2, 3] 
```

在这里的`merged`数组中，我们首先展开`array2`，然后是`array3`，最后是`array1`。

你可以在本文中了解更多关于这个算子的内容: [Spread 算子简化](https://dillionmegida.com/p/spread-operator-simplified/)。

## 2.如何在 JavaScript 中使用`Array.concat`

您使用数组的`concat`方法将一个数组的内容与新值组合起来形成一个新的数组。

这些新值可以是数字、字符串、布尔值、对象，甚至是数组。

该方法接受值列表作为参数:

```
array.concat(value1, value2, ..., valueN) 
```

通过将数组指定为参数，可以将现有数组与指定的数组合并，形成一个新数组。这里有一个例子:

```
const array1 = [1, 2, 3]
const array2 = [4, 5, 6]

const merged = array1.concat(array2)
// [1, 2, 3, 4, 5, 6] 
```

如您所见，`array1`的内容与`array2`的内容连接在一起，形成一个分配给`merged`的新数组。

您还可以传递多个数组进行合并:

```
const array1 = [1, 2, 3]
const array2 = [4, 5, 6]
const array3 = [7, 8, 9]

const merged = array2.concat(array3, array1)
// [4, 5, 6, 7, 8, 9, 1, 2, 3] 
```

在这个例子中，我们对`array2`使用了`concat`方法，这意味着`array2`的内容首先出现在合并后的数组中。

对于参数，我们首先传递`array3`，这意味着`array3`的内容是合并数组中的下一个，然后是`array1`的内容。

你可以在本文中了解更多关于`concat`的内容: [Array concat simplified](https://dillionmegida.com/p/array-concat/) 。

## 3.如何在 JavaScript 中使用`Array.push`

数组的`push`方法允许您将新值“推入”(添加)到数组的末尾。

```
array.push(value1, value2, ...valueN) 
```

使用这种方法，您可以将新数组推送到现有数组来创建合并过程。与我前面提到的方法不同，`push`方法修改了它所使用的数组。

这里有一个例子:

```
const array1 = [1, 2, 3]
const array2 = [4, 5, 6]

for(let i = 0; i < array2.length; i++) {
  array1.push(array2[i])
}

console.log(array1)
// [1, 2, 3, 4, 5, 6] 
```

这里，我们使用一个`for`循环来遍历`array2`的值，在每个循环中，我们将索引处的值推送到`array1`。

在循环结束时，您会看到`array1`被修改，包含来自`array2`的值。

除了`for`循环，您还可以使用带有`push`方法的`spread`操作符。由于`push`方法接受由逗号分隔的列表或参数，您可以在该方法中扩展另一个数组，它们都将被推送到应用该方法的数组中:

```
const array1 = [1, 2, 3]
const array2 = [4, 5, 6]

array1.push(...array2)

console.log(array1)
// [1, 2, 3, 4, 5, 6] 
```

您可以对多个阵列执行此操作:

```
const array1 = [1, 2, 3]
const array2 = [4, 5, 6]
const array3 = [7, 8, 9]

array3.push(...array2, ...array1)

console.log(array3)
// [7, 8, 9, 4, 5, 6, 1, 2, 3] 
```

在这里，我们在`array3`上调用`push`，然后将`array2`的值后接`array1`作为参数传入`array3`。

## 包扎

在本文中，我们看到了 JavaScript 中合并数组的三种方法。我特别喜欢`spread`操作符，因为它使用起来更简单。

当使用`push`时，注意，正如我提到的，它修改了使用它的数组(不像`concat`返回一个新数组)。如果不小心谨慎地使用它，可能会导致意想不到的结果。