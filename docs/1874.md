# 如何用一行代码在 JavaScript 中格式化日期

> 原文：<https://www.freecodecamp.org/news/how-to-format-dates-in-javascript/>

很长一段时间以来，每当我需要用 JavaScript 格式化日期时，我都使用像`Date-fns`这样的库。但是每当我在需要 JavaScript 默认提供的简单日期格式的小项目中这样做时，就变得非常奇怪。

我发现大多数开发人员经常这样做。我认为这是最好的方法，直到我最近发现**我们并不总是需要使用库**来格式化 JavaScript **中的日期。**

如果您想尝试一下，下面是代码:👇

```
new Date().toLocaleDateString('en-us', { weekday:"long", year:"numeric", month:"short", day:"numeric"}) 
// "Friday, Jul 2, 2021"
```

在您自己的代码中尝试了这种方法并看到它的工作原理之后，让我们来理解它的工作原理，并学习一些其他的用一行代码在 JavaScript 中格式化日期的方法。

# 如何在 JS 中格式化日期

用 JavaScript 获取日期通常不是问题，但是对于初学者来说，格式化这些日期以适应您的项目可能会很麻烦。正因为如此，大多数人最终都会使用图书馆。

JavaScript 中最常用的获取日期的方法是`new Date()`对象。

默认情况下，当您在终端中运行`new Date()`时，它会使用浏览器的时区，并将日期显示为全文字符串，比如**Fri 2021 年 7 月 2 日 12:44:45 GMT+0100(英国夏令时)。**

但是在你的网页或应用程序中有这样的东西是不专业的，也不容易阅读。因此，这迫使您寻找更好的方法来格式化这些日期。

让我们来看看一些操作日期对象的方法。

# JavaScript 中的日期方法

有许多方法可以应用于 date 对象。您可以使用这些方法从 date 对象中获取信息。以下是其中的一些:

*   `getFullYear()`–获取四位数的年份(yyyy)
*   `getMonth()`–以数字形式获取月份(0-11)
*   `getDate()`–以数字形式获取日期(1-31)
*   `getHours()`–获取小时(0-23)

还有更多…

不幸的是，这些方法中的大多数仍然需要大量的代码来将日期转换成我们想要的格式。

例如，`new Date().getMonth()`将输出代表**七月的 6。**对于我来说，在我的项目中使用七月，我将需要像这样的长代码，这真的很麻烦:

```
const currentMonth = new Date();
const months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
console.log(months[currentMonth.getMonth()]);
```

让我们来看看两种方法，你可以用它们来以最好的方式格式化你的日期，这样你就可以在你的项目中使用它们。

## JavaScript 中的 toDateString()方法

JavaScript `toDateString()`方法使用以下格式以字符串形式返回 date 对象的日期部分:

1.  星期名称的前三个字母
2.  月份名称的前三个字母
3.  一个月中的第几天，如有必要，在左边填充一个零
4.  (至少)四位数年份，必要时在左边用零填充

```
new Date().toDateString();
//"Fri Jul 02 2021"
```

这种方法的一个主要缺点是我们无法按照我们想要的方式操作数据输出。

例如，它没有给我们根据我们的语言显示日期的能力。让我们来看看另一种方法，对我来说这仍然是最好的方法之一。

## JavaScript 中的 toLocaleDateString()方法

该方法使用本地约定将 date 对象作为字符串返回。它还接受选项作为参数，让您/您的应用程序自定义函数的行为。

**语法:**

```
toLocaleDateString()
toLocaleDateString(locales)
toLocaleDateString(locales, options)
```

让我们来看看一些示例及其输出:

```
const currentDate = new Date();

const options = { weekday: 'long', year: 'numeric', month: 'short', day: 'numeric' };

console.log(currentDate.toLocaleDateString('de-DE', options));
//Freitag, 2\. Juli 2021

console.log(currentDate.toLocaleDateString('ar-EG', options))
// الجمعة، ٢ يوليو ٢٠٢١

console.log(currentDate.toLocaleDateString('en-us', options));
//Friday, Jul 2, 2021
```

您也可以决定不使用区域设置或选项:

```
new Date().toLocaleDateString()
// "7/2/2021"
```

您也可以决定只使用区域设置。这将基于我的浏览器的时区输出与前面相同的信息。

```
new Date().toLocaleDateString('en-US')
"7/2/2021"
```

你也可以随意改变选项。有 4 个基本选项，分别是:

*   `weekday`–根据您想要的显示方式(短或长)输出星期几。
*   `year`–以数字形式输出年份
*   `month`–根据您希望的显示方式(短或长)输出一年中的月份。
*   `day`–最后，以数字形式输出日期

```
new Date().toLocaleDateString('en-us', { weekday:"long", year:"numeric", month:"short"}) // "Jul 2021 Friday"

new Date().toLocaleDateString('en-us', { year:"numeric", month:"short"})
// "Jul 2021"
```

# 结论

date 对象有大约七种格式化方法。这些方法中的每一种都为您提供了一个特定的值:

1.  `toString()`给你**Fri 2021 年 7 月 2 日 14:03:54 GMT+0100(英国夏令时)**
2.  `toDateString()`给你**Fri 2021 年 7 月 2 日**
3.  `toLocaleString()`给你**2021 年 7 月 2 日，下午 2:05:07**
4.  `toLocaleDateString()`给你**2021 年 7 月 2 日**
5.  `toGMTString()`给你 **Fri，2021 年 7 月 2 日 13:06:02 GMT**
6.  `toUTCString()`为您呈现 **Fri，2021 年 7 月 2 日 13:06:28 GMT**
7.  `toISOString()`给你 **2021-07-02T13:06:53.422Z**

如果您正在寻找更高级的日期格式，那么您将需要自己创建一个自定义格式。查看以下资源，帮助您了解如何创建自定义日期格式。

## 有用的资源

*   [JavaScript 中关于日期你需要知道的一切](https://css-tricks.com/everything-you-need-to-know-about-date-in-javascript/)
*   [JavaScript -如何用 JavaScript 格式化日期](https://www.tabnine.com/academy/javascript/how-to-format-date/)
*   [如何用 JavaScript 格式化日期](https://flaviocopes.com/how-to-format-date-javascript/)
*   [日期时间操作的权威指南](https://www.toptal.com/software/definitive-guide-to-datetime-manipulation)