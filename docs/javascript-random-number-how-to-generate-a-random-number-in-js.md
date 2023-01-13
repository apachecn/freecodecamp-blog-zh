# JavaScript 随机数——如何在 JS 中生成随机数

> 原文：<https://www.freecodecamp.org/news/javascript-random-number-how-to-generate-a-random-number-in-js/>

当使用 JavaScript 程序时，有时您可能需要生成一个随机数。

例如，您可能希望在开发 JavaScript 游戏(如猜数字游戏)时生成一个随机数。

JavaScript 有许多处理数字和执行数学计算的内置方法。其中一种方法是`Math.random()`方法。

在本文中，您将学习如何使用`Math.random()`方法来检索随机数。

以下是我们将要介绍的内容:

1.  [介绍`Math`对象](#intro)
2.  [`Math.random()`语法故障](#syntax)
    1.  [如何用指定的`max`](#max) 生成随机小数
    2.  [如何生成指定`min`和`max`范围](#range)的随机小数
    3.  [如何用指定的`max`](#integer-max) 生成随机整数
    4.  [如何生成具有指定`max`和](#max-inclusive)的随机整数
    5.  [如何生成指定`min`和`max`包含范围](#range-inclusive)的随机整数

## 如何在 JavaScript 中使用数学——对`Math`对象的介绍

JavaScript 有内置的静态对象`Math`，它允许你执行数学计算和操作。

`Math`对象有一些内置的方法，使得执行这些操作更加准确。

`Math`对象方法的一般语法如下:

```
Math.methodName(number); 
```

一些最流行的方法是:

*   `Math.round()`
*   `Math.ceil()`
*   `Math.floor()`
*   `Math.random()`

让我们看一些如何实现这些方法的例子。

如果你想将一个数字四舍五入到最接近的整数，使用`Math.round()`方法:

```
console.log(Math.round(6.2)); // 6

console.log(Math.round(6.3)); // 6

console.log(Math.round(6.5)); // 7

console.log(Math.round(6.8)); // 7 
```

如果您想将一个数字**向上**舍入到最接近的整数，请使用`Math.ceil()`方法:

```
console.log(Math.ceil(6.2));  // 7

console.log(Math.ceil(6.3));  // 7

console.log(Math.ceil(6.5));  // 7

console.log(Math.ceil(6.8));  // 7 
```

如果您想将一个数字**向下**舍入到最接近的整数，请使用`Math.floor()`方法:

```
console.log(Math.floor(6.2));  // 6

console.log(Math.floor(6.3));  // 6

console.log(Math.floor(6.5));  // 6

console.log(Math.floor(6.8)); // 6 
```

如果你想生成一个**随机**数，使用`Math.random()`方法:

```
console.log(Math.random());

// You will not get the same output

//first time running the program:
// 0.4928793139100267 

//second time running the program:
// 0.5420802533292215

// third time running the program:
// 0.5479835477696466 
```

## JavaScript 中的`Math.random()`方法是什么？-语法故障

`Math.random()`方法的语法如下:

```
Math.random(); 
```

该方法不接受任何参数。

默认情况下，该方法返回一个值，该值是一个在`0`和`1`之间的**随机十进制(或浮点)**数。

需要注意的是`0`是包含性的，而`1`是*而不是*包含性的。

因此，它将返回一个大于或等于`0`的值，并且总是小于且从不等于`1`。

### 如何在 JavaScript 中使用`Math.random()`生成具有指定`max`限制的随机小数

正如您到目前为止所看到的，默认情况下，`Math.random()`生成的数字很小。

如果你想生成一个随机的十进制数，从`0`开始，包括`0`，并且*大于`1`的*呢？为此，您指定一个**最大值**数。

具体来说，您需要将这个`max`数乘以来自`Math.random()`的随机数。

例如，如果你想在`0`和`10`之间产生一个随机数，你可以这样做:

```
console.log(Math.random() * 10);

// You won't get the same output
//9.495628210218175 
```

在上面的例子中，我用`Math.random()`得出的数字乘以`max`数字(`10`)，这个数字将作为一个限制。

请记住，在这种情况下，随机数将介于`0`和`10`之间——这意味着大于或等于`0`，小于且决不等于`10`。

### 如何在 JavaScript 中使用`Math.random()`生成指定`min`和`max`范围的随机小数

如果您想在指定的数字范围内生成一个随机的十进制数，该怎么办呢？

您在上一节中看到了如何指定一个`max`，但是如果您*不想让*的范围从`0`开始(这是默认的开始范围)，该怎么办？为此，您还可以指定一个`min`。

在两个值(或范围)之间生成小数的一般语法如下所示:

```
Math.random() * (max - min) + min; 
```

让我们举以下例子:

```
// specify a minimum - where the range will start
let min = 20.4;

// specify a maximum - where the range will end
let max = 29.8; 
```

我创建了两个变量，`min`和`max`——其中`min`是范围内最小的数字，`max`是最大的数字。

接下来，我使用前面看到的语法在该范围内生成一个随机数:

```
let min = 20.4;
let max = 29.8;

let randomNum = Math.random() * (max - min) + min;

console.log(randomNum);

// You won't get the same output
// 23.309418058783486 
```

这里需要注意的是，`min`是包含性的，所以随机数将大于或等于`20.4`，而`max`是*而不是*包含性的，所以结果将总是一个小于且永远不等于`29.8`的数。

### 如何在 JavaScript 中使用`Math.random()`生成具有指定`max`限制的随机整数

到目前为止，您已经看到了如何生成随机的*十进制*数。

也就是说，有一种方法可以使用`Math.random()`生成随机的*整数*。

您需要将`Math.random()`计算的结果传递给前面看到的`Math.floor()`方法。

一般语法如下:

```
Math.floor(Math.random()); 
```

`Math.floor()`方法将把随机小数向下舍入到最接近的整数(或整数)。

除此之外，您还可以指定一个`max`号。

因此，举例来说，如果你想在`0`和`10`之间生成一个随机数，你可以这样做:

```
console.log(Math.floor(Math.random() * 10));

// You won't get the same output
// 0 
```

在本例中，我将`max`数字(在本例中是`10`)乘以来自`Math.random()`的结果，并将计算结果传递给`Math.floor()`。

这里需要注意的是，随机数将介于`0`和`10`之间。因此，这些数字可以大于或等于`0`，也可以小于或不等于`10`。

### 如何在 JavaScript 中使用`Math.random()`生成具有指定`max`包含极限的随机整数

在上一节的例子中，我在数字`0`(包含)和`10`(不包含)之间生成了一个随机数。

到目前为止，您已经看到您无法生成一个等于指定的`max`的随机数。

如果要生成一个包含指定最大值的随机数呢？

对此的解决方案是在计算过程中添加`1`。

所以，让我们重温一下上一节的代码:

```
console.log(Math.floor(Math.random() * 10)); 
```

您现在可以像这样重写代码:

```
console.log(Math.floor(Math.random() * 10) + 1);

// You will not get the same output
// 10 
```

这里需要注意的是，这段代码会生成一个在`1`(不是`0`)和`10`——包括`10`之间的随机整数。

### 如何在 JavaScript 中使用`Math.random()`生成具有指定`min`和`max`包含范围的随机整数

到目前为止，您已经看到了如何使用您指定的 inclusive `max`生成一个随机整数。

正如你在上面的例子中看到的，包含数字`max`的代码使用数字`1`而不是`0`作为起始数字。

也就是说，有一种方法可以让你指定一个包含数字的范围。为此，您需要指定一个`min`和`max`。

在上一节中，您看到了如何在指定范围内生成一个随机数，代码是类似的。唯一的变化是将结果传递给`Math.floor()`方法，将小数向下舍入到最接近的整数。

所以，一般代码如下:

```
Math.floor(Math.random() * (max - min) + min); 
```

要使范围*包含*，您需要执行以下操作:

```
console.log(Math.floor(Math.random() * (max - min + 1)) + min); 
```

因此，要在数字`0`(含)*和* `10`(含)之间生成一个随机数，您需要编写以下代码:

```
let min = 0;
let max = 10;

console.log(Math.floor(Math.random() * (max - min + 1)) + min);

//You will not get the same output
// 0 
```

## 结论

现在你知道了！现在你知道了`Math.random()`方法是如何工作的，以及使用它的一些方法。

要了解更多关于 JavaScript 的知识，请访问 freeCodeCamp 的 [JavaScript 算法和数据结构认证](https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/)。

这是一个免费的、经过深思熟虑的、结构化的课程，你可以在其中进行互动学习。最后，您还将构建 5 个项目来获得您的认证并巩固您的知识。

感谢阅读！