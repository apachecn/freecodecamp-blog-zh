# JavaScript 日期格式——如何在 JS 中格式化日期

> 原文：<https://www.freecodecamp.org/news/javascript-date-format-how-to-format-a-date-in-js/>

JavaScript 是开发网站或 web 应用程序时使用的三种基本 web 技术之一。

在创建这些网页时，出于某种原因，您可能会在某个时候需要使用日期——比如显示什么时候上传、存储等等。

在本文中，您将学习如何在 JavaScript 中格式化日期，并了解如何使用一个流行的 JavaScript 日期库(称为 [moment.js](https://momentjs.com/) )来格式化日期。

## 如何在 JavaScript 中获取日期

在 JavaScript 中，使用`new Date()`或`Date()`构造函数来获取日期(当前日期或特定日期)。

`new Date()`构造函数返回一个新的`Date`对象，而`Date()`构造函数返回当前日期和时间的字符串表示。

```
let stringDate = Date();
console.log(stringDate); // "Tue Aug 23 2022 14:47:12 GMT-0700 (Pacific Daylight Time)"

let objectDate = new Date();
console.log(objectDate); // Tue Aug 23 2022 14:47:12 GMT-0700 (Pacific Daylight Time) 
```

这些完整格式的日期包括日、月、年、分钟、小时和其他您并不总是需要的信息。

您主要关心的是如何将这些日期值格式化为可读格式的返回日期，您可以将其嵌入到您的 web 页面中，以便每个人都能理解。

## 如何用 JavaScript 格式化日期

格式化日期取决于你和你的需要。在某些国家，月份先于日期，然后是年份(06/22/2022)。在其他情况下，这一天在月份之前，然后是年份(22/06/2022)，等等。

不管是哪种格式，您都希望分解 date 对象值，并为您的 web 页面获取必要的信息。

JavaScript 日期方法可以做到这一点。有许多这样的方法，但是因为您只关心本文中的日期，所以您只需要其中的三种:

*   `getFullYear()`–您将使用此方法获得四位数的年份(yyyy)。比如 2022 年。
*   `getMonth()`–您将使用此方法获取 0-11 之间的月份，每个数字代表从 1 月到 12 月的月份。例如，`2`将是三月的索引，因为它是从零开始的索引(意味着它从`0`开始)。
*   `getDate()`–您将使用此方法以 1-31 之间的数字形式获取日期(这不是一个索引，而是确切的日期值，因此它从 1 而不是 0 开始)。

**注意:**这些方法只能应用于或者只能与返回日期对象的`new Date()`构造函数一起工作。

```
let objectDate = new Date();

let day = objectDate.getDate();
console.log(day); // 23

let month = objectDate.getMonth();
console.log(month + 1); // 8

let year = objectDate.getFullYear();
console.log(year); // 2022 
```

您会注意到`1`被添加到上面的`month`值中。这是因为月份是由`0`索引的。数值从`0`开始。这意味着`7`将意味着八月而不是`8`。

至此，您已经能够从 date 对象中提取所有日期值。现在，您可以按照自己喜欢的格式组织它们:

```
let format1 = month + "/" + day + "/" + year;
console.log(format1); // 7/23/2022

let format2 = day + "/" + month + "/" + year;
console.log(format2); // 23/7/2022

let format3 = month + "-" + day + "-" + year;
console.log(format3); // 7-23-2022

let format4 = day + "-" + month + "-" + year;
console.log(format4); // 23-7-2022 
```

在上面的示例中，您使用加号运算符连接了这些值。您还可以利用模板文字来连接:

```
let format1 = `${month}/${day}/${year}`;
console.log(format1); // 7/23/2022

let format2 = `${day}/${month}/${year}`;
console.log(format2); // 23/7/2022

let format3 = `${month}-${day}-${year}`;
console.log(format3); // 7-23-2022

let format4 = `${day}-${month}-${year}`;
console.log(format4); // 23-7-2022 
```

现在，您已经看到了您可能想要格式化日期的可能方式。

另一种情况是，如果月份和日期值是单个数值(从 1 到 9)，您希望它前面有 0。为此，您需要在设置日期格式之前添加一个条件来处理此问题:

```
if (day < 10) {
    day = '0' + day;
}

if (month < 10) {
    month = `0${month}`;
}

let format1 = `${month}/${day}/${year}`;
console.log(format1); // 07/23/2022 
```

对用 JavaScript 格式化日期的其他方法感兴趣吗？查看我的文章“如何用一行代码 在 JavaScript 中格式化日期”。

## 如何用 Moment.js 在 JavaScript 中格式化日期

[Moment.js](https://momentjs.com/) 是一个 JavaScript 日期和时间库，您可以使用它来快速格式化您的日期，而无需用这么多行代码来处理逻辑。

要使用这个库，您必须使用文档中您的[首选选项在您的项目中安装这个包。](https://momentjs.com/)

对于本指南，您只关心如何使用 Moment.js 格式化日期:

```
let date = moment().format();

console.log(date); // 2022-08-23T16:50:22-07:00 
```

要格式化这个日期/时间值，您只需输入您喜欢的格式，它将返回格式化后的日期，如下所示:

```
let dateFormat1 = moment().format('D-MM-YYYY');
console.log(dateFormat1); // 23-08-2022

let dateFormat2 = moment().format('D/MM/YYYY');
console.log(dateFormat2); // 23/08/2022

let dateFormat3 = moment().format('MM-D-YYYY');
console.log(dateFormat3); // 08-23-2022

let dateFormat4 = moment().format('MM/D/YYYY');
console.log(dateFormat4); // 08/23/2022 
```

## 结论

本文教您如何用 JavaScript 格式化日期，无论是从头开始还是使用 moment.js 库。

在小项目中使用库时要小心，因为库会增加项目的大小。这是因为这些库被设计来处理更多的操作。但是要使用任何最小的操作，您仍然必须安装整个库。

总是建议您从头开始执行像这样的简单操作。也就是说，除非您被迫使用该库，或者该库已经安装，或者您正在处理一个需要一些复杂格式的大型项目。

祝编码愉快！