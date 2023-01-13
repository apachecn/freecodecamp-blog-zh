# 如何在 JavaScript 中交换两个数组元素——在 JS 中交换元素

> 原文：<https://www.freecodecamp.org/news/swap-two-array-elements-in-javascript/>

当您使用数组时，有时您需要在 JavaScript 中交换数组中的两个元素。

也许您正在处理一个算法问题，比如冒泡排序算法，您需要比较两个值，然后在条件为真时交换它们。

除此之外，许多其他情况可能需要交换数组元素。

如果你还不明白我说的“交换”是什么意思，这里有一个例子。假设您有一个数字数组，并且您想要将索引`1`处的元素 **-2** 与索引`0`处的元素 **12** 进行交换，如下图所示:

![s_B08002E277AA9EEDFC6B2A9D153D5485AA9F9B4567270177E695FBE6032A1880_1664460747763_Untitled.drawio+14](img/1445a6feb91e6438f9d3baf4d961e1e6.png)

如果您还不熟悉 JS 是如何做这些事情的，那么用 JavaScript 实现这一点可能会令人困惑。你可能也在寻找更好的方法来处理这个问题。

本文将教你三种方法:使用临时变量、析构和使用`splice()`数组方法。

## 如何用临时变量交换两个数组元素

要交换元素，您可以使用一个临时变量并经历三个步骤。

第一步是创建一个临时变量来保存第一个元素的值。第二步是将第一个元素的值设置为第二个元素的值。第三步是将第二个元素的值设置为临时变量中的值。

```
let myArray = [12, -2, 55, 68, 80];

const temp = myArray[0];
myArray[0] = myArray[1];
myArray[1] = temp;

console.log(myArray); // [-2,12,55,68,80] 
```

您还可以创建一个可重用的函数来处理这个问题，通过这个函数，您可以指定想要交换的数组和两个索引。

```
const swapElements = (array, index1, index2) => {
    let temp = array[index1];
    array[index1] = array[index2];
    array[index2] = temp;
};

let myArray = [12, -2, 55, 68, 80];
swapElements(myArray, 0, 1);
console.log(myArray); // [-2,12,55,68,80] 
```

## 如何通过析构交换两个数组元素

一个更好的交换数组元素的方法是析构，因为它只用一行代码就完成了这项工作。

您只需创建一个以特定顺序包含这两个元素的新数组，然后将它赋给一个以相反顺序包含这两个元素的新数组。

```
let myArray = [12, -2, 55, 68, 80];

[myArray[0], myArray[1]] = [myArray[1], myArray[0]];

console.log(myArray); // [-2,12,55,68,80] 
```

您还可以创建一个可重用的函数来处理这个问题，通过这个函数，您可以指定想要交换的数组和两个索引。

```
const swapElements = (array, index1, index2) => {
    [myArray[index1], myArray[index2]] = [myArray[index2], myArray[index1]];
};

let myArray = [12, -2, 55, 68, 80];
swapElements(myArray, 0, 1);
console.log(myArray); // [-2,12,55,68,80] 
```

## 如何用`Splice()`方法交换两个数组元素

最后，可以使用`splice()`数组方法。您可以使用此方法从数组中移除一个或多个元素，并用任何指定的元素替换这些元素。

```
// Syntax
array.splice(index, howmany, element1, ....., elementX) 
```

例如，如果您有一个数组，并且想要删除一个特定的元素，您将指定它的 id 和想要删除的元素的数量。在我们的情况下，它只是一个。

```
let myArray = [12, -2, 55, 68, 80];

myArray.splice(1, 1);
console.log(myArray); // 12,55,68,80] 
```

此外，如果您想用另一个元素替换删除的元素，您的代码将如下所示:

```
let myArray = [12, -2, 55, 68, 80];

myArray.splice(1, 1, 32);
console.log(myArray); // [12,32,55,68,80] 
```

但是如果你想交换两个元素，你会得到这样的东西:

```
let myArray = [12, -2, 55, 68, 80];

myArray[0] = myArray.splice(1, 1, myArray[0])[0];

console.log(myArray); // [-2,12,55,68,80] 
```

您还可以创建一个可重用的函数来处理这个问题，通过这个函数，您可以指定要交换的数组和两个索引:

```
const swapElements = (array, index1, index2) => {
    myArray[index1] = myArray.splice(index2, 1, myArray[index1])[0];
};

let myArray = [12, -2, 55, 68, 80];
swapElements(myArray, 0, 1);
console.log(myArray); // [-2,12,55,68,80] 
```

## 包扎

在本文中，您学习了在 JavaScript 中交换数组元素的三种方法。

您可以使用任何方法，但是最好使用 ES6 析构方法，因为它对每个人来说都更容易理解和使用。

祝编码愉快！