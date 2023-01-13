# JavaScript 日期比较——如何在 JS 中比较日期

> 原文：<https://www.freecodecamp.org/news/javascript-date-comparison-how-to-compare-dates-in-js/>

日期是开发人员在创建现实应用程序时最常用的数据类型之一。

但是通常，开发人员会纠结于这种数据类型，并最终使用像 Moment.js 这样的日期库来完成简单的任务，这些任务不值得安装整个包所带来的大的包大小。

在本文中，我们将学习如何在 JavaScript 中执行日期比较。如果你很快需要代码，这里是:

```
const compareDates = (d1, d2) => {
  let date1 = new Date(d1).getTime();
  let date2 = new Date(d2).getTime();

  if (date1 < date2) {
    console.log(`${d1} is less than ${d2}`);
  } else if (date1 > date2) {
    console.log(`${d1} is greater than ${d2}`);
  } else {
    console.log(`Both dates are equal`);
  }
};

compareDates("06/21/2022", "07/28/2021");
compareDates("01/01/2001", "01/01/2001");
compareDates("11/01/2021", "02/01/2022"); 
```

这将返回:

```
"06/21/2022 is greater than 07/28/2021"
"Both dates are equal"
"11/01/2021 is less than 02/01/2022" 
```

当我们想到 JavaScript 中的日期比较时，我们会想到使用 date 对象(`Date()`)，当然，这是可行的。

date 对象允许我们使用`>`、`<`、`=`或`>=`比较运算符来执行比较，但不允许使用`==`、`!=`、`===`和`!==`等相等比较运算符(除非我们将 date 方法附加到 date 对象)。

让我们从学习如何只使用 date 对象来执行比较开始，然后我们将看到如何使用 date 对象和 date 方法来执行相等比较。

## 如何用 JavaScript 中的 Date 对象进行日期比较

假设我们想用 JavaScript 比较两个日期。我们可以这样轻松地使用 Date 对象(`Date()`):

```
let date1 = new Date();
let date2 = new Date();

if (date1 > date2) {
  console.log("Date 1 is greater than Date 2");
} else if (date1 < date2) {
  console.log("Date 1 is less than Date 2");
} else {
  console.log("Both Dates are same");
} 
```

上面将返回两个日期是相同的，因为我们没有传递不同的日期:

```
"Both Dates are same" 
```

现在让我们传入不同的日期值:

```
let date1 = new Date();
let date2 = new Date("6/01/2022");

if (date1 > date2) {
  console.log("Date 1 is greater than Date 2");
} else if (date1 < date2) {
  console.log("Date 1 is less than Date 2");
} else {
  console.log("Both Dates are same");
} 
```

这将返回以下内容:

```
"Date 1 is greater than Date 2" 
```

幸运的是，当前两个条件失败时，上面的代码将等式作为最后一个选项。但是假设我们试着这样处理等式作为条件:

```
let date1 = new Date();
let date2 = new Date();

if (date1 === date2) {
  console.log("Both Dates are same");
} else {
  console.log("Not the same");
} 
```

它将返回以下错误信息:

```
"Not the same" 
```

## 如何用 JavaScript 执行相等比较

为了处理相等比较，我们使用 date 对象和返回毫秒数的`getTime()` date 方法。但是如果我们想要比较特定的信息，比如日、月等等，我们可以使用其他日期方法，比如`getDate()`、`getHours()`、`getDay()`、`getMonth()`和`getYear()`。

```
let date1 = new Date();
let date2 = new Date();

if (date1.getTime() === date2.getTime()) {
  console.log("Both  are equal");
} else {
  console.log("Not equal");
} 
```

这将返回:

```
"Both are equal" 
```

我们可以将不同的日期传递给 date 对象，以便进行比较:

```
let date1 = new Date("12/01/2021");
let date2 = new Date("09/06/2022");

if (date1.getTime() === date2.getTime()) {
  console.log("Both  are equal");
} else {
  console.log("Not equal");
} 
```

正如所料，这将返回:

```
"Not equal" 
```

注意:使用`getTime()`方法，我们可以使用所有比较操作符来执行所有形式的日期比较，这些比较操作符是`>`、`<`、`<=`、`>=`、`==`、`!=`、`===`和`!==`。

## 如何执行特定的日期比较

假设我们想要比较特定的日期值，比如年份。那么我们可以这样使用`.getYear()`日期方法:

```
let date1 = new Date("06/21/2022").getYear();
let date2 = new Date("07/28/2021").getYear();

if (date1 < date2) {
  console.log("Date1 is less than Date2 in terms of year");
} else if (date1 > date2) {
  console.log("Date1 is greater than Date2 in terms of year");
} else {
  console.log(`Both years are equal`);
} 
```

## 结论

在本文中，您已经学习了如何使用 date 对象在 JavaScript 中进行日期比较，而无需安装任何库。

编码快乐！