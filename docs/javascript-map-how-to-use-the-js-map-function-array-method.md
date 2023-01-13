# JavaScript Map——如何使用 JS。map()函数(数组方法)

> 原文：<https://www.freecodecamp.org/news/javascript-map-how-to-use-the-js-map-function-array-method/>

有时，您可能需要获取一个数组，并对其元素应用一些过程，以便获得一个包含已修改元素的新数组。

您可以简单地使用内置的`Array.map()`方法，而不是使用循环手动迭代数组。

`Array.map()`方法允许你迭代一个数组并使用回调函数修改它的元素。回调函数将在数组的每个元素上执行。

例如，假设您有以下数组元素:

```
let arr = [3, 4, 5, 6];
```

A simple JavaScript array

现在假设您需要将数组的每个元素乘以`3`。您可以考虑使用如下的`for`循环:

```
let arr = [3, 4, 5, 6];

for (let i = 0; i < arr.length; i++){
  arr[i] = arr[i] * 3;
}

console.log(arr); // [9, 12, 15, 18]
```

Iterate over an array using for loop

但是您实际上可以使用`Array.map()`方法来实现相同的结果。这里有一个例子:

```
let arr = [3, 4, 5, 6];

let modifiedArr = arr.map(function(element){
    return element *3;
});

console.log(modifiedArr); // [9, 12, 15, 18]
```

Iterate over an array using map() method

`Array.map()`方法通常用于对元素进行一些修改，无论是像上面的代码中那样乘以一个特定的数字，还是执行您的应用程序可能需要的任何其他操作。

## 如何在对象数组上使用 map()

例如，您可能有一个对象数组，存储您朋友的`firstName`和`lastName`值，如下所示:

```
let users = [
  {firstName : "Susan", lastName: "Steward"},
  {firstName : "Daniel", lastName: "Longbottom"},
  {firstName : "Jacob", lastName: "Black"}
]; 
```

An array of objects

您可以使用`map()`方法迭代数组并连接`firstName`和`lastName`的值，如下所示:

```
let users = [
  {firstName : "Susan", lastName: "Steward"},
  {firstName : "Daniel", lastName: "Longbottom"},
  {firstName : "Jacob", lastName: "Black"}
];

let userFullnames = users.map(function(element){
    return `${element.firstName} ${element.lastName}`;
})

console.log(userFullnames);
// ["Susan Steward", "Daniel Longbottom", "Jacob Black"]
```

Use map() method to iterate over an array of objects

`map()`方法传递的不仅仅是一个元素。让我们看看`map()`传递给回调函数的所有参数。

## 完整的 map()方法语法

`map()`方法的语法如下:

```
arr.map(function(element, index, array){  }, this);
```

回调`function()`在每个数组元素上被调用，`map()`方法总是将当前的`element`、当前元素的`index`以及整个`array`对象传递给它。

`this`参数将在回调函数中使用。默认情况下，其值为`undefined`。例如，下面是如何将`this`值更改为数字`80`:

```
let arr = [2, 3, 5, 7]

arr.map(function(element, index, array){
	console.log(this) // 80
}, 80);
```

Assigning number value to map() method this argument

如果您感兴趣，也可以使用`console.log()`测试其他参数:

```
let arr = [2, 3, 5, 7]

arr.map(function(element, index, array){
    console.log(element);
    console.log(index);
    console.log(array);
    return element;
}, 80);
```

Logging the arguments to see the values

这就是你需要知道的关于`Array.map()`方法的全部内容。大多数情况下，您只会在回调函数中使用`element`参数，而忽略其他参数。这就是我在日常项目中通常会做的:)

## 如何使用 JS 的视频说明。map()函数

[https://scrimba.com/scrim/co93c4bf0af8e0ed2e162b818?pl=pKwWeHY?embed=freecodecamp,mini-header](https://scrimba.com/scrim/co93c4bf0af8e0ed2e162b818?pl=pKwWeHY?embed=freecodecamp,mini-header)

[https://scrimba.com/scrim/cLwqQ7uE?pl=pKwWeHY?embed=freecodecamp,mini-header](https://scrimba.com/scrim/cLwqQ7uE?pl=pKwWeHY?embed=freecodecamp,mini-header)

[https://scrimba.com/scrim/c7wmmJcv?pl=pKwWeHY?embed=freecodecamp,mini-header](https://scrimba.com/scrim/c7wmmJcv?pl=pKwWeHY?embed=freecodecamp,mini-header)

## ******感谢阅读本教程******

你可能也会对我写的其他 JavaScript 教程感兴趣，包括[如何对一组对象求和](https://sebhastian.com/javascript-sum-array-objects/)和[寻找回文字符串的方法](https://sebhastian.com/palindrome-javascript/)。它们是一些最常见的需要解决的 JavaScript 问题。

我还有一个关于 web 开发教程的[免费简讯](https://sebhastian.com/newsletter/)(大部分是 JavaScript 相关的)。