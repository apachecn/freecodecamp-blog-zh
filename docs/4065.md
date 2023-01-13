# JavaScript Date and Moment.js 终极指南

> 原文：<https://www.freecodecamp.org/news/the-ultimate-guide-to-javascript-date-and-moment-js/>

欢迎来到我们关于 JavaScript `Date` object 和 Moment.js 的终极指南。本教程将教你在项目中处理日期和时间所需要知道的一切。

## 如何创建一个`Date`对象

### 获取当前日期和时间

```
const now = new Date();

// Mon Aug 10 2019 12:58:21 GMT-0400 (Eastern Daylight Time)
```

### 获取带有单个值的日期和时间

```
const specifiedDate = new Date(2019, 4, 29, 15, 0, 0, 0);

// Wed May 29 2019 15:00:00 GMT-0400 (Eastern Daylight Time)
```

语法是`Date(year, month, day, hour, minute, second, millisecond)`。

请注意，月份是零索引的，从 1 月的 0 开始，到 12 月的 11 结束。

### 从时间戳中获取日期和时间

```
const unixEpoch = new Date(0);
```

这表示 1970 年 1 月 1 日星期四(UTC)的时间，即 Unix 纪元时间。Unix 纪元很重要，因为它是 JavaScript、Python、PHP 和其他语言和系统在内部用来计算当前时间的。

`new Date(ms)`返回纪元的日期加上您传入的毫秒数。一天有 86，400，000 毫秒，所以:

```
const dayAfterEpoch = new Date(86400000);
```

将于 1970 年 1 月 2 日星期五(UTC)返回。

### 从字符串中获取日期和时间

```
const stringDate = new Date('May 29, 2019 15:00:00');

// Wed May 29 2019 15:00:00 GMT-0400 (Eastern Daylight Time)
```

通过这种方式获取日期非常灵活。以下所有示例都返回有效的`Date`对象:

```
new Date('2019-06') // June 1st, 2019 00:00:00
new Date('2019-06-16') // June 16th, 2019
new Date('2019') // January 1st, 2019 00:00:00
new Date('JUNE 16, 2019')
new Date('6/23/2019')
```

您还可以使用`Date.parse()`方法返回自纪元(1970 年 1 月 1 日)以来的毫秒数:

```
Date.parse('1970-01-02') // 86400000
Date.parse('6/16/2019') // 1560610800000
```

### 设置时区

当传递没有设置时区的日期字符串时，JavaScript 假定日期/时间采用 UTC 格式，然后将其转换为浏览器的时区:

```
const exactBirthdate = new Date('6/13/2018 06:27:00');

console.log(exactBirthdate) // Wed Jun 13 2018 06:27:00 GMT+0900 (Korean Standard Time)
```

这可能会导致返回的日期相差几个小时的错误。为了避免这种情况，请将时区与字符串一起传递:

```
const exactBirthdate = new Date('6/13/2018 06:27:00 GMT-1000');

console.log(exactBirthdate) // Thu Jun 14 2018 01:27:00 GMT+0900 (Korean Standard Time)

/*
These formats also work:

new Date('6/13/2018 06:27:00 GMT-10:00');
new Date('6/13/2018 06:27:00 -1000');
new Date('6/13/2018 06:27:00 -10:00');
*/
```

您也可以传递一些(但不是全部)时区代码:

```
const exactBirthdate = new Date('6/13/2018 06:27:00 PDT');

console.log(exactBirthdate) // Thu Jun 14 2018 01:27:00 GMT+0900 (Korean Standard Time)
```

## `Date`对象方法

通常你不需要整个日期，只需要一部分，比如一天、一周或一个月。幸运的是，有很多方法可以做到这一点:

```
const birthday = new Date('6/13/2018 06:27:39');

birthday.getMonth() // 5 (0 is January)
birthday.getDate() // 13
birthday.getDay() // 3 (0 is Sunday)
birthday.getFullYear() // 2018
birthday.getTime() // 1528838859000 (milliseconds since the Unix Epoch)
birthday.getHours() // 6
birthday.getMinutes() // 27
birthday.getSeconds() // 39
birthday.getTimezoneOffset() // -540 (time zone offset in minutes based on your browser's location)
```

## 使用 Moment.js 简化日期处理

获得正确的日期和时间不是一件小事。每个国家似乎都有不同的日期格式，考虑到不同的时区和夏令时/夏令时，嗯，要花很多时间。这就是 Moment.js 的亮点——它使得解析、格式化和显示日期变得轻而易举。

要开始使用 Moment.js，通过`npm`这样的包管理器安装，或者通过 CDN 添加到你的站点。更多细节见 [Moment.js 文档](https://momentjs.com/docs/)。

### 用 Moment.js 获取当前日期和时间

```
const now = moment();
```

这将返回一个对象，其中包含基于浏览器位置的日期和时间，以及其他区域设置信息。类似于原生 JavaScript 的`new Date()`。

### 用 Moment.js 从时间戳中获取日期和时间

与`new Date(ms)`类似，您可以将从 epoch 开始的毫秒数传递给`moment()`:

```
const dayAfterEpoch = moment(86400000);
```

如果您想使用 Unix 时间戳获得以秒为单位的日期，可以使用`unix()`方法:

```
const dayAfterEpoch = moment.unix(86400);
```

### 用 Moment.js 从字符串中获取日期和时间

用 Moment.js 解析字符串中的日期很容易，该库接受 ISO 8601 或 RFC 2822 日期时间格式的字符串，以及 JavaScript `Date`对象接受的任何字符串。

建议使用 ISO 8601 字符串，因为这是一种被广泛接受的格式。以下是一些例子:

```
moment('2019-04-21');
moment('2019-04-21T05:30');
moment('2019-04-21 05:30');

moment('20190421');
moment('20190421T0530');
```

### 使用 Moment.js 设置时区

到目前为止，我们一直在本地模式下使用 Moment.js，这意味着任何输入都被认为是本地日期或时间。这类似于本地 JavaScript `Date`对象的工作方式:

```
const exactBirthMoment = moment('2018-06-13 06:27:00');

console.log(exactBirthMoment) // Wed Jun 13 2018 06:27:00 GMT+0900 (Korean Standard Time)
```

但是，要设置时区，必须先获取处于 *UTC 模式的 Moment 对象:*

```
const exactBirthMoment = moment.utc('2018-06-13 06:27:00');

console.log(exactBirthMoment) // Wed Jun 13 2018 15:27:00 GMT+0900 (Korean Standard Time)
```

然后，您可以使用`utcOffset()`方法调整时区差异:

```
const exactBirthMoment = moment.utc('2018-06-13 06:27:00').utcOffset('+10:00');

console.log(exactBirthMoment) // Wed Jun 13 2018 06:27:00 GMT+0900 (Korean Standard Time)
```

您也可以将 UTC 偏移量设置为数字或字符串:

```
moment.utc().utcOffset(10) // Number of hours offset
moment.utc().utcOffset(600) // Number of minutes offset
moment.utc().utcOffset('+10:00') // Number of hours offset as a string
```

要将命名时区(`America/Los_Angeles`)或时区代码(`PDT`)用于 Moment 对象，请查看 [Moment 时区](https://momentjs.com/timezone/)库。

### 用 Moment.js 格式化日期和时间

Moment.js 相对于原生 JavaScript `Date`对象的主要优势之一是格式化输出日期和时间非常容易。只需将`format()`方法链接到一个时刻日期对象，并将一个格式字符串作为参数传递给它:

```
moment().format('MM-DD-YY'); // "08-13-19"
moment().format('MM-DD-YYYY'); // "08-13-2019"
moment().format('MM/DD/YYYY'); // "08/13/2019"
moment().format('MMM Do, YYYY') // "Aug 13th, 2019"
moment().format('ddd MMMM Do, YYYY HH:mm:ss') // "Tues August 13th, 2019 19:29:20"
moment().format('dddd, MMMM Do, YYYY -- hh:mm:ss A') // "Tuesday, August 13th, 2019 -- 07:31:02 PM"
```

下表列出了一些常见的格式化标记:

| 投入 | 输出 | 描述 |
| YYYY | Two thousand and nineteen | 四位数年份 |
| YY | Nineteen | 两位数年份 |
| 嗯 | 八月 | 完整的月份名称 |
| 嗯 | 八月 | 缩写月份名 |
| abbr. 毫米（millimeter） | 08 | 两位数的月份 |
| M | eight | 一位数月份 |
| DDD | Two hundred and twenty-five | 一年中的某一天 |
| 直接伤害 | Thirteen | 一月中的某一天 |
| 做 | 13 号 | 一个月中的第几天 |
| 嗒嗒球拍 | 星期三 | 全天名称 |
| ddd | 结婚 | 缩写的日期名称 |
| 殿下 | Seventeen | 24 小时内的小时数 |
| 倍硬 | 05 | 12 小时内的小时数 |
| 毫米 | Thirty-two | 分钟 |
| 悬浮物 | Nineteen | 秒 |
| a | 上午/下午 | 午前或午后 |
| A | 上午/下午 | 大写的事前或事后利润 |
| 锯齿形 | +0900 | UTC 的时区偏移量 |
| X | One billion four hundred and ten million seven hundred and fifteen thousand six hundred and forty point five seven nine | Unix 时间戳(秒) |
| xx | 1410715640579 | Unix 时间戳(毫秒) |

查看 [Moment.js 文档](https://momentjs.com/docs/)了解更多格式化标记。

使用 JavaScript `Date`对象和 Moment.js 并不需要花费很多时间。现在，您应该已经知道了足够多的信息，可以开始使用这两种工具了。