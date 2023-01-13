# JavaScript 获取当前日期——JS 中今天的日期

> 原文：<https://www.freecodecamp.org/news/javascript-get-current-date-todays-date-in-js/>

当您开发 web 应用程序时，您可能需要包含执行特定操作的当前日期。

例如，通过表单提交数据时，您可能希望包含数据的创建日期或表单的提交时间。

在本文中，我们将学习如何使用 JavaScript 轻松获取当前日期(今天的日期)。我们还将学习如何使用像 Moment.js 这样的外部库来实现这一点，moment . js 是一个流行的 JavaScript 日期库。

注意——一般来说，不建议使用外部库来完成这样的操作。但是，如果您已经在项目中安装了一个库，或者您正在将它用于应用程序中的其他操作，则可以使用它。

## 如何在 JavaScript 中获取当前日期

在 JavaScript 中，我们可以通过使用`new Date()`对象轻松获得当前日期或时间。默认情况下，它使用我们浏览器的时区，并将日期显示为一个完整的文本字符串，例如“Fri 2022 年 6 月 17 日 10:54:59 GMT+0100(英国夏令时)”包含当前日期、时间和时区。

```
const date = new Date();
console.log(date); // Fri Jun 17 2022 11:27:28 GMT+0100 (British Summer Time)
```

让我们看看如何从这个长字符串中只提取日期。我们将通过使用一些对日期对象进行操作的 JavaScript 方法，使它对用户来说更具可读性和可理解性。

### 如何使用 JavaScript 日期方法

date 对象支持许多日期方法，但是对于本文，我们只需要当前日期，并且只使用三种方法:

*   `getFullYear()`–我们将使用此方法获得四位数的年份(yyyy)，例如 2022 年。
*   `getMonth()`–以数字(0-11)的形式获取月份，例如 2 表示三月，因为它是一个基于零的指数(意味着它从 0 开始)。
*   `getDate()`–以数字形式获取日期(1-31)。

现在，让我们根据我们希望日期出现的格式将所有这些放在一起:

```
const date = new Date();

let day = date.getDate();
let month = date.getMonth() + 1;
let year = date.getFullYear();

// This arrangement can be altered based on how we want the date's format to appear.
let currentDate = `${day}-${month}-${year}`;
console.log(currentDate); // "17-6-2022"
```

**注意:**我们给`date.getMonth()`的值加 1，因为它是基于`0`的索引。假设我们不想在日期值之间使用破折号(-)，我们所要做的就是用我们喜欢的东西替换破折号。

### 如何使用 toJSON()方法

我们刚刚看到了如何使用日期方法获取当前日期。现在让我们看看如何使用`toJSON()`方法，除了时间格式`hh:mm:ss.ms`之外，该方法还以`yyyy-mm-dd`格式返回我们的日期。

```
let date = new Date().toJSON();
console.log(date); // 2022-06-17T11:06:50.369Z
```

因为我们只想要当前日期，所以我们可以这样使用`slice()`方法来获取前 10 个字符:

```
let currentDate = new Date().toJSON().slice(0, 10);
console.log(currentDate); // "2022-06-17"
```

### 如何使用 toLocaleDateString()

这是另一个简单的方法，它使用本地约定将日期对象作为字符串返回。例如，不同语言的日期格式不同，这个方法接受一个参数来纠正这种情况。

让我们从传递一个论点开始:

```
let date = new Date().toLocaleDateString();
console.log(date); // 6/17/2022
```

假设我们想要德国的时间:

```
let date = new Date().toLocaleDateString("de-DE");
console.log(date); // 17.6.2022
```

注意:我们可以在这里得到所有地区代码的列表。

### 如何使用 Moment.js

Moment.js 是每个人都可以使用的最流行的日期包之一，我们也可以用它来获取当前日期。

只要项目中安装了 Moment.js，您需要做的就是获取当前日期，如下所示:

```
var date = moment();

var currentDate = date.format('D/MM/YYYY');
console.log(currentDate); // "17/06/2022"
```

我们还可以根据我们希望日期的格式如何显示来操纵它。

## 结论

在本文中，我们了解了单独使用 JavaScript 或使用外部 JavaScript 库获取当前日期的各种方法。

你可以在这里阅读更多关于如何轻松格式化日期[的内容。](https://www.freecodecamp.org/news/how-to-format-dates-in-javascript/)