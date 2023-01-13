# SQL 日期–函数、查询、时间戳示例格式

> 原文：<https://www.freecodecamp.org/news/sql-date-function-query-timestamp-example-format/>

日期是任何编程语言不可或缺的一部分，SQL 也不例外。向 SQL 数据库中插入数据时，可以添加日期并根据该日期查询数据库。

在本文中，您将了解 SQL 中的日期函数以及如何用日期查询数据库。我们还会看看一些时间函数。

## 我们将涵盖的内容

*   [SQL 中的日期函数](#datefunctionsinsql)
    *   add date()
    *   [当前日期()](#current_date)
    *   [CURRENT _ TIME()；](#current_time)
    *   [当前 _ 时间戳()；](#current_timestamp)
    *   [现在()](#now)
    *   [日期](#date)
    *   [日期 _ 订阅](#date_sub)
    *   [日期差异](#datediff)
    *   [日](#day)
    *   [月](#month)
    *   [年](#year)
*   [如何基于日期查询数据库](#howtoqueryadatabasebasedondates)
*   [结论](#conclusion)

## SQL 中的日期函数

### `ADDDATE()`

ADDDATE()函数顾名思义就是给`date`或`datetime`添加一个间隔。

可以使用`ADDDATE()`函数，格式如下:`ADDDATE(date, INTERVAL value addunit)`。

*   是您正在处理的日期。对于 MySQL，日期格式是 YYY-MM-DD，并且是必需的。
*   `INTERVAL`是必需的关键字
*   `value`是一个整数，代表要添加的区间
*   `addunit`是区间应该代表的。即年、月、日、小时、分钟、秒和其他相关单位。

例如，运行下面的查询将返回“2022-10-22”。这意味着“2022-10-12”增加了 10 天。

```
SELECT ADDDATE("2022-10-12", INTERVAL 10 DAY); 
```

![ss1-2](img/0769359dd6ee20c7e65a91923e093f5e.png)

如果您愿意，您可以将它与月份或年份一起使用:

![ss2-2](img/d633151fd0b2f2f6f56b5e32ed4b964e.png)

### `CURRENT_DATE()`

CURRENT_DATE()函数准确地显示了它所说的内容——当前日期。它以 YYYY-MM-DD 格式返回日期。

例如，`SELECT CURRENT_DATE()`返回我开始写这篇文章的日期:
![ss3-1](img/cda4641c2f7a957423078c5c4e357b74.png)

### `CURRENT_TIME();`

CURRENT_TIME 函数显示当前时间。

```
SELECT CURRENT_TIME(); 
```

![ss4-1](img/94a7acec8dbb825cd64128a95fcbcf20.png)

### `CURRENT_TIMESTAMP();`

当前时间戳函数返回当前日期和时间。它是当前日期()和当前时间()的组合。

```
SELECT CURRENT_TIMESTAMP(); 
```

### `NOW()`

NOW()函数返回当前日期和时间。

```
SELECT NOW(); 
```

![ss5-1](img/226850b05f4db46314547760f814029a.png)

### `DATE`

您可以使用 DATE 函数提取时间戳的日期部分。

```
SELECT DATE("2022-11-14 12:00:00"); 
```

![ss6](img/7e5a1b6632cd8672098fa2eeb7e3a676.png)

### `DATE_SUB`

函数的作用是:从一个日期中减去一天、一个月或一年。在下面的查询中，我从开始写这篇文章的日期减去了 10 天:

```
SELECT DATE_SUB("2022-11-14", INTERVAL 10 DAY); 
```

![ss7](img/c168b8c38bd11876fec3630a6782753e.png)

### `DATEDIFF`

DATEDIFF()函数返回两个日期之间的天数。

```
SELECT DATEDIFF("2023-11-14", "2022-11-14"); 
```

![ss8](img/28efb201d6fdcffc723118a40413d770.png)

### `DAY`

该函数返回指定日期内的一天。

```
SELECT DAY("2022-11-14"); 
```

![ss9](img/551bd9d386eadd49ea2d30d2678873f5.png)

### `MONTH`

MONTH 函数返回指定日期中的月份。

```
SELECT MONTH("2022-11-14"); 
```

![ss10](img/8faf7558b5bf577741a2857cbd7da1f6.png)

### `YEAR`

YEAR 函数返回指定日期中的年份。

```
SELECT YEAR("2022-11-14"); 
```

![ss11](img/eb96993059f9722f930d197c19e6de73.png)

## 如何基于日期查询数据库

为了向您展示如何使用日期查询数据库，我将使用下表:

![ss12](img/77f6e8a2fa12780431b3d40405a6603b.png)

要在一个日期和另一个日期之间选择某个特定日期，您可以在指定日期时使用`BETWEEN`和`AND`关键字。

在下面的查询中，我选择了 2021 年添加到数据库中的所有项目:

```
SELECT *
FROM brands
WHERE date_added BETWEEN "2021-01-01" AND "2021-12-31"; 
```

![ss13-1](img/eeb2fac37c5249ad7c6d17a847546204.png)

结合`DATE_SUB()`和`NOW()`函数，我能够获得最近 3 个月添加到数据库中的项目:

```
SELECT *
FROM brands
WHERE date_added > DATE_SUB(NOW(), INTERVAL 3 MONTH); 
```

![ss14](img/491c3256672a20ac585d6ccbf3e7b016.png)

## 结论

本文向您展示了一些重要的函数，您可以使用这些函数在 SQL 中处理日期和查询数据库。

如果你觉得这篇文章有用，不要犹豫，与朋友和家人分享。

感谢阅读。