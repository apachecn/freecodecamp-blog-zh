# 如何在 JavaScript 中处理数字和日期

> 原文：<https://www.freecodecamp.org/news/numbers-and-dates-in-javascript/>

数字、日期和计时器是 JavaScript 的重要组成部分。在编写代码时，您需要知道如何使用它们。

我们经常忽略这些话题，因为许多文章没有讨论它们。因此，在这里，我们将深入探讨你可以使用的技术，并学习一些你可以在下一个项目中使用的有趣的东西。

## 如何在 JS 中使用数字

数字是一个原始的**包装对象**，用于将数据类型转换为数字。首先，我们将看到如何使用`Number`包装器对象将字符串转换成数字。

```
let str = "23";
console.log(typeof str); // string

let num = Number(str);
console.log(typeof num); // number
```

上面的例子展示了如何将字符串转换成数字。

使用类型强制还有另一种方法。我们只需在字符串前加上`+`符号，就可以将字符串转换成数字。

```
let str = "23";
console.log(typeof str); // string

// reassigning str variable
str = +"23";
console.log(typeof str);  // number
```

从上面的例子我们可以看到，有两种方法将字符串转换成数字。但是我们通常更喜欢`Number()`方法，因为它更清楚我们在做什么，也更精确。

### 如何使用 parseInt()和 parseFloat()

我们有一个函数`parseInt()`，它只能解析字符串中的整数部分。但是字符串应该以整数开头。我们只能解析字符串中第一次出现的整数。我们试着用一个例子来理解。

**我们可以以两种不同的方式使用解析:**

*   Number.parseInt()
*   parseInt()

第一种方法更好，因为它更精确。

```
let padding = "22px";
console.log(Number.parseInt(padding)); // 22

let margin = "16px 12px";
console.log(Number.parseInt(margin)); // 16

let body = "container 12px";
console.log(Number.parseInt(body)); // NaN
```

从上面的例子可以看出，您只能解析字符串中第一次出现的整数，并且字符串应该以数字开头。

也可以用`parseFloat()`用同样的规则解析浮点数。

```
let padding = "2.5rem";
console.log(Number.parseFloat(padding)); // 2.5

let margin = "1.5rem 2.5rem";
console.log(Number.parseFloat(margin)); // 1.5

let body = "container 1rem";
console.log(Number.parseFloat(body)); // NaN
```

这就是从字符串中提取整数或浮点数的方法。那么我们可以在哪里使用它呢？如果你熟悉 HTML 和 CSS，可以使用 parseInt()在 JavaScript 的帮助下提取 HTML/CSS 中提到的 text-size。提取这些信息后，您可以操作文本并根据填充、边距等更改元素的大小。

### 如何使用`Math`对象

数学是一个内置的对象，它有不同的方法和功能来帮助你进行数学计算。

请记住，数学只适用于数字类型。

数学中有很多方法和函数。但是在本教程中，我们将只看到其中的一部分。要了解更多信息，您可以访问[这里的文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)。

```
let num = 16;
console.log(Math.sqrt(num)); 
// Gives the square root of a number.

let arr = ['1',4,7,20,32,35,41,'45'];
console.log(Math.max(...arr)); // 45
// ...arr is destructuring the array in place.
// max uses type coercion to convert a string to a number 
// and givse back the maximum number in an array.

console.log(Math.min(...arr)); // 1
// min gives back the min integer in an array
```

对于初学者来说，有 4 种数学方法非常容易混淆。但是这里我们会尽可能的简化它们，以便更好的理解。

这四种方法是:

*   `trunc()`
*   `floor()`
*   `round()`
*   `ceil()`

#### Math.trunc()

此方法仅移除整数的小数部分。小数部分有多长不重要。

```
let height = 23.4;
let width = 23.6;

console.log(Math.trunc(height)); // 23
console.log(Math.trunc(width)); // 23
```

#### Math.floor()

当我们对浮点数使用下限法时，它会向下舍入到最接近的整数。

```
let height = 23.4;
let width = 23.6;

console.log(Math.floor(height)); // 23
console.log(Math.floor(width)); // 23
```

#### Math.round()

Math.round()舍入到浮点数的最近整数。如果浮点(小数点后的数字)低于 5，则向下舍入。如果浮点高于 5，则向上舍入。你可以在这里看到它是如何工作的:

```
let height = 23.4;
let width = 23.6;

console.log(Math.round(height)); // 23
console.log(Math.round(width)); // 24
```

#### Math.ceil()

Math.ceil()向上舍入到浮点的下一个整数。和 Math.floor()完全相反。

```
let height = 23.4;
let width = 23.6;

console.log(Math.ceil(height)); // 24
console.log(Math.ceil(width)); // 24
```

当我们在 JavaScript 中执行浮点运算时，我们经常会遇到小数精度的问题。让我们看看下面的例子:

```
let operation = 0.1 / 0.3;
console.log(operation); // 0.33333333333333337
```

这里有很多循环小数。我们可以控制这些循环小数，并使用`toFixed()`方法指定我们想要的小数位数。

```
let operation = 0.1 / 0.3;
console.log(operation); // 0.33333333333333337
console.log(operation.toFixed(1)); // 0.3
console.log(operation.toFixed(2)); // 0.33
```

## 如何在 JS 中处理日期

日期是 JavaScript 中的一个对象。我们可以在构造函数`new Date()`的帮助下计算一个日期。现在，date 对象包含一个以毫秒为单位的数字(我们从 1970 年 1 月 1 日开始计算毫秒)。

```
let now = new Date();
console.log(now); // current date

console.log(new Date() / 1000); // milliseconds from 1st Jan 1970
```

创建日期主要有四种方式:

```
// 1
let now = new Date();
console.log(now);

// 2
now = new Date("Aug 31 2022 11:45:45");
console.log(now);

// 3
now = new Date("Nov 14 2022");
console.log(now);

// 4
let birth = "01-05-1998";
console.log(new Date(birth)); // 05 Jan 1998
```

*   当您处理当前日期并希望在应用程序中动态更改日期和时间时，您会希望使用第一种方法。
*   当您想要处理用户提供的日期或存储的过去日期时，可以使用第二种方法。
*   第三种方法是第二种方法的一种简单的替代方法，并且稍微简单一些。
*   如果你想把一个字符串转换成日期/时间格式，你可以用第四种方法。

您可以使用这些方法中的任何一种。因为如果我们想要提取关于那个日期的任何信息，你必须使用预定义的方法，而这些方法不能用于字符串。

当我们创建一个新的日期()时，有 7 个数字，依次是年、月、日、小时、分钟、秒和毫秒。

我们可以用许多方法来获得不同的信息。

*   `getFullYear()`
*   `getMonth()`
*   `getDate()`
*   `getDay()`
*   `getHours()`
*   `getMinutes()`
*   `getSeconds()`
*   `getTime()`

```
let now = new Date();

console.log(now.getFullYear()); // gives full year.
console.log(now.getMonth()); // gives number of month starting from 0.
console.log(now.getDate()); // gives date.
console.log(now.getDay()); //  gives day with Monday as 0 and so on.
console.log(now.getHours()); // gives hours in 24 hour format.
console.log(now.getMinutes()); //  gives minutes
console.log(now.getSeconds()); // gives seconds
console.log(now.getTime()); // gives time passed from 1 Jan 1970 till now in milliseconds 
```

### 如何对日期执行操作

我们可以对日期执行不同的操作。如果有两个日期，我们想找出这两个日期之间的差异，我们可以执行减法。但是当我们得到结果时，我们得到的是时间戳的形式，我们必须将时间戳转换成天数。

```
let present = new Date('Aug 31 2020');
let past = new Date('Aug 31 1990');
let days  = present - past;
console.log(days); //  time stamp
console.log(Math.abs(new Date(days)/(1000*60*60*24))); // No of days
```

如果你想更精确地计算日期，你可以使用[**moment . js**](https://momentjs.com/)——但对于简单的项目和应用程序来说，可能没有必要。

## 包扎

我希望您现在理解了数字和日期在 JavaScript 中是如何工作的。感谢您的阅读！

您可以关注我:

*   [LinkedIn](https://www.linkedin.com/in/kedar-makode-9833321ab/)
*   [推特](https://twitter.com/Kedar__98)
*   [Instagram](https://www.instagram.com/kedar_98/)