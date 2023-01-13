# JavaScript 拼接——如何使用？splice() JS 数组方法

> 原文：<https://www.freecodecamp.org/news/javascript-splice-how-to-use-the-splice-js-array-method/>

`splice()`方法是 JavaScript 数组对象的内置方法。它允许您通过删除现有元素或用新元素替换现有元素来更改数组的内容。

此方法修改原始数组，并将移除的元素作为新数组返回。

在本教程中，您将学习如何使用`splice()`方法移除、添加或替换数组元素。让我们先从移除数组中的元素开始。

## 如何使用 splice()删除数组元素

例如，假设您有一个名为`months`的数组，但是数组中有一些日期名称，如下所示:

```
let months = ["January", "February", "Monday", "Tuesday"];
```

A mixed array of month and day names

您可以使用`splice()`方法从`months`方法中删除日期名称，同时将其添加到一个新数组中:

```
let months = ["January", "February", "Monday", "Tuesday"];
let days = months.splice(2);

console.log(days); // ["Monday", "Tuesday"]
```

Creating an array of days

`splice()`方法需要至少一个参数，即拼接操作开始的`start`索引。在上面的代码中，数字`2`被传递给该方法，这意味着`splice()`将开始从索引`2`中移除元素。

您还可以通过传递第二个名为`removeCount`的`number`参数来定义要从数组中移除多少个元素。例如，要仅删除一个元素，您可以像这样传递数字`1`:

```
let months = ["January", "February", "Monday", "Tuesday"];
let days = months.splice(2, 1);

console.log(days); // ["Monday"]
console.log(months); // ["January", "February", "Tuesday"]
```

Remove only one element from the array

当您省略`removeCount`参数时，`splice()`将删除从`start`索引到数组末尾的所有元素。

## 如何使用 splice()移除和添加数组元素

该方法还允许您在删除操作之后立即添加新元素。您只需要在删除计数之后传递想要添加到数组中的元素。

`splice()`方法的完整语法如下:

```
Array.splice(start, removeCount, newItem, newItem, newItem, ...)
```

Complete array splice() method syntax

以下示例显示了如何在将“三月”和“四月”添加到`months`数组的同时删除“星期一”和“星期二”:

```
let months = ["January", "February", "Monday", "Tuesday"];
let days = months.splice(2, 2, "March", "April");

console.log(days); // ["Monday", "Tuesday"]
console.log(months); // ["January", "February", "March", "April"] 
```

Using splice() to both remove and add elements to an array

## 如何添加新的数组元素而不删除任何元素

最后，您可以通过将数字`0`传递给`removeCount`参数来添加新元素而不删除任何元素。当没有元素被移除时，splice 方法将返回一个空数组。您可以选择是否将返回的空数组存储到变量中。

以下示例显示了如何在不删除任何元素的情况下，在`"February"`旁边添加新元素`"March"`。因为`splice()`方法返回一个空数组，所以不需要存储返回的数组:

```
let months = ["January", "February", "Monday", "Tuesday"];
months.splice(2, 0, "March");

console.log(months); 
// ["January", "February", "March", "Monday", "Tuesday"]
```

The splice() method called without returning any elements

## 结论

您刚刚学习了`splice()`方法的工作原理。干得好！

`splice()`方法主要在需要删除或添加新元素时使用。在某些情况下，您也可以使用它来分隔一个数组，其中有混合的内容，如上述情况。

当您从数组中移除`0`元素时，该方法将简单地返回一个空数组。你总是可以自由地将返回的数组赋给一个变量或者忽略它。

## **感谢阅读本教程**

如果你想学习更多关于 JavaScript 的知识，你可能想看看我在 sebhastian.com 的网站，在那里我发表了超过 100 篇关于用 JavaScript 编程的教程。

教程包括字符串操作、日期操作、数组和对象方法、JavaScript 算法解决方案等等。

请务必[检查一下](https://sebhastian.com/)😉