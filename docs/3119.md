# JavaScript Date Now——如何在 JavaScript 中获取当前日期

> 原文：<https://www.freecodecamp.org/news/javascript-date-now-how-to-get-the-current-date-in-javascript/>

您构建的许多应用程序都有某种日期组件，无论是资源的创建日期，还是活动的时间戳。

处理日期和时间戳格式可能会让人精疲力尽。在本指南中，您将学习如何用 JavaScript 获取各种格式的当前日期。

## JavaScript 的日期对象

JavaScript 有一个内置的`Date`对象，它存储日期和时间，并提供处理它们的方法。

要创建`Date`对象的新实例，请使用`new`关键字:

```
const date = new Date();
```

`Date`对象包含一个`Number`,它表示自纪元即 1970 年 1 月 1 日以来经过的毫秒数。

您可以向`Date`构造函数传递一个日期字符串，为指定的日期创建一个对象:

```
const date = new Date('Jul 12 2011');
```

要获得当前年份，使用`Date`对象的`getFullYear()`实例方法。`getFullYear()`方法返回`Date`构造函数中指定日期的年份:

```
const currentYear = date.getFullYear();
console.log(currentYear); //2020
```

同样，也有获取当月当天和当月的方法:

```
const today = date.getDate();
const currentMonth = date.getMonth() + 1; 
```

`getDate()`方法返回当月的当前日期(1-31)。

方法返回指定日期的月份。关于`getMonth()`方法需要注意的一点是，它返回索引为 0 的值(0-11)，其中 0 代表一月，11 代表十二月。因此，添加 1 来规范化该月的值。

## 现在约会

`now()`是`Date`对象的静态方法。它返回以毫秒为单位的值，表示自 Epoch 以来经过的时间。您可以将从`now()`方法返回的毫秒数传递给`Date`构造函数，以实例化一个新的`Date`对象:

```
const timeElapsed = Date.now();
const today = new Date(timeElapsed);
```

## 格式化日期

您可以使用`Date`对象的方法将日期格式化为多种格式(GMT、ISO 等)。

`toDateString()`方法以人类可读的格式返回日期:

```
today.toDateString(); // "Sun Jun 14 2020"
```

`toISOString()`方法返回遵循 ISO 8601 扩展格式的日期:

```
today.toISOString(); // "2020-06-13T18:30:00.000Z"
```

`toUTCString()`方法以 UTC 时区格式返回日期:

```
today.toUTCString(); // "Sat, 13 Jun 2020 18:30:00 GMT"
```

`toLocaleDateString()`方法以区分地区的格式返回日期:

```
today.toLocaleDateString(); // "6/14/2020"
```

你可以在 [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)中找到`Date`方法的完整参考。

## 自定义日期格式化程序函数

除了上一节提到的格式，您的应用程序可能有不同的数据格式。它可以是`yy/dd/mm`或`yyyy-dd-mm`格式，或者类似的格式。

要解决这个问题，最好是创建一个可重用的函数，这样它就可以在多个项目中使用。

因此，在本节中，让我们创建一个实用函数，该函数将以函数参数中指定的格式返回日期:

```
const today = new Date();

function formatDate(date, format) {
	//
}

formatDate(today, 'mm/dd/yy');
```

您需要将字符串“mm”、“dd”、“yy”分别替换为参数中传递的格式字符串中的月、日和年值。

为此，您可以使用如下所示的`replace()`方法:

```
format.replace('mm', date.getMonth() + 1);
```

但是这将导致大量的方法链接，并且当您试图使函数更加灵活时，会使其难以维护:

```
format.replace('mm', date.getMonth() + 1)
    .replace('yy', date.getFullYear())
	.replace('dd', date.getDate());
```

除了链接方法，您还可以通过`replace()`方法利用正则表达式。

首先创建一个对象，表示子字符串的键值对及其各自的值:

```
const formatMap = {
	mm: date.getMonth() + 1,
    dd: date.getDate(),
    yy: date.getFullYear().toString().slice(-2),
    yyyy: date.getFullYear()
};
```

接下来，使用正则表达式来匹配和替换字符串:

```
formattedDate = format.replace(/mm|dd|yy|yyy/gi, matched => map[matched]);
```

完整的函数如下所示:

```
function formatDate(date, format) {
    const map = {
        mm: date.getMonth() + 1,
        dd: date.getDate(),
        yy: date.getFullYear().toString().slice(-2),
        yyyy: date.getFullYear()
    }

    return format.replace(/mm|dd|yy|yyy/gi, matched => map[matched])
}
```

您还可以在函数中添加格式化时间戳的功能。

## 结论

我希望您现在对 JavaScript 中的`Date`对象有了更好的理解。您还可以使用其他第三方库，如`datesj`和`moment`来处理应用程序中的日期。

直到下一次，保持安全，继续忙碌。