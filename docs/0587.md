# JavaScript 字符串到目前为止——JS 中的日期解析

> 原文：<https://www.freecodecamp.org/news/javascript-string-to-date-date-parsing-in-js/>

日期是一个非常基本的概念。我们一直在使用它们。计算机一直在使用它们。但是使用 JavaScript 解析日期可能有点...嗯，有意思。

在本文中，我们将:

1.  讨论日期格式
2.  使用 JavaScript 将一个小的字符串转换成一个合适的日期对象。
3.  将日期对象解析为数字
4.  展示使用参数而不是字符串来创建日期对象的另一种方法。

日期很棘手，但使用起来也非常有用。一旦你花一点时间复习基础知识，你的自信就会增加。

![date](img/1768ac1e343595e9fa381b49cf0815e5.png)

## JavaScript 中的日期格式是什么？

当然是 ISO 8601！这是传递日期和时间数据的国际标准的名称。在 JavaScript 中处理日期时，我们需要使用这种格式

这是这种格式的样子。您对它已经很熟悉了——它只是将日期和时间组合成一大块信息，JavaScript 可以轻松处理。

```
// YYYY-MM-DDTHH:mm:ss.sssZ
// A date string in ISO 8601 Date Format
```

## 如何在 JavaScript 中使用`new Date()`构造函数

`new Date()`是 JavaScript 中创建新日期的构造函数。修卡！😂

![shocker](img/b41beff1f859f4fa59af1dd68cd35d50.png)

如果你没有传递任何东西到新的日期构造函数中，它会给你一个日期对象，无论当前日期和时间是什么。

```
new Date()

// Thu Jun 23 2022 20:35:51 GMT-0400 (Eastern Daylight Time)
```

A new date created without any arguments returns the current date and time.

请注意，除了月、日和年之外，date 对象还可以(并且通常应该)包含精确到毫秒的时间。

### 如何用字符串创建新的日期

您可以向`new Date()`传递一个日期字符串来创建一个日期对象。

创建日期对象时，不必指定时间。

`new Date('2022-06-13')`完全有效。然而，当您在控制台上记录这个新日期时，您会看到一个时间将被自动分配，即使我们没有声明它。

```
let aDate = new Date('2022-06-13')

// Sun Jun 12 2022 20:00:00 GMT-0400 (Eastern Daylight Time)
```

A string date without a declared time will still be assigned one.

这会在矩阵中产生分裂，最好包含一个完整的日期。例如，由于本地系统时间用于解释日期，根据您的计算机所处的位置，您可能会从相同的非特定日期获得不同的结果。

![glitch](img/7c0f8ada8f1e58ea6f681fa0b50dd5d0.png)

因此，在向`new Date()`传递字符串时，使用完整的日期，包括小时:分钟.毫秒。

大写字母 **T** 将日期部分与时间部分分开，如下所示:

```
new Date('2022-05-14T07:06:05.123')

// Sat May 14 2022 07:06:05 GMT-0400 (Eastern Daylight Time)
```

Use the full day and time when possible

### 如何用数字创建新的日期

您也可以将一个数字传递给一个`new Date()`构造函数。下面是关于这些数字代表什么的更多信息——例如，`new Date(1656033105000)`将返回一个合法的日期:

```
console.log(Date(1656033105000))

// Thu Jun 2022 21:12:06 GMT-0400 (Eastern Daylight Time)
```

Giant numbers represent dates too

### 如何创建带参数的新日期

下面还有更多...您还可以向`new Date()`传递多达七个参数，从而创建一种更简单的方式向日期构造函数表示日期和时间。

```
new Date(2022,03,14,07,33,245)

// Thu Apr 14 2022 07:37:05 GMT-0400 (Eastern Daylight Time)
```

A descending list of arguments starting with the year and ending with milliseconds

## 什么是 Date.parse()？

因此，如果您在 date 对象上使用 parse 方法，会发生一件有趣的事情。它吐出一个巨大的数字。

`Date.parse()`告诉我们自 1970 年 1 月 1 日以来已经过去的毫秒数。这在比较多个日期时很有帮助。当日期转换成数字而不是字符串时，比较和测量日期的差异会更容易。

```
let anotherDate = new Date(2012,07,12,12,00,234)

Date.parse(anotherDate)

// 1344787434000
```

Date.parse returns the number of milliseconds since January 1, 1970

## 用参数或字符串表示的日期哪个更好？

约会时，为了长期的成功，要学会好好辩论。在 JavaScript 中使用日期时，为了长期的成功，使用参数而不是字符串。

`new Date(2022, 00, 12, 8, 01, 33, 456)`

这比用字符串创建日期要简单一些。参数只是以降序输入，以年份开始，以毫秒结束。

唯一棘手的地方是:月份是零索引的。所以，一月是 00。

```
new Date(2022,00,12,8,01,33,456)

// Wed Jan 12 2022 08:01:33 GMT-0500 (Eastern Standard Time)
```

Don't forget that the month is zero indexed when using arguments for date construction

## 如何更深入地了解 Javascript 日期

这仅仅触及了日期对象的表面。查看 [MDN，深入了解](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)。和所有事物一样，那里有一个信息宝库。

![deep](img/48c69d6528761d9cc03f015e877f0b93.png)

不过，你现在已经掌握了基本知识。去付诸实践吧。您现在知道了如何用 JavaScript 的`new Date()`创建一个日期对象。您可以通过不向构造函数传递任何内容来获取当前日期和时间，也可以传递一个字符串、一个数字或参数。

## 感谢阅读

感谢阅读！我在这里写设计和开发:[https://blog.eamonncottrell.com/](https://blog.eamonncottrell.com/)

你可以在推特和 T2【LinkedIn】上找到我。

祝你愉快！

![thank-you](img/1449e0894e988a5a2e4c391113ac647b.png)