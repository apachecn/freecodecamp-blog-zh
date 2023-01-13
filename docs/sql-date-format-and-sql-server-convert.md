# SQL Server 日期格式和 SQL Server 转换举例说明

> 原文：<https://www.freecodecamp.org/news/sql-date-format-and-sql-server-convert/>

## SQL Convert 是做什么的？

它将一种数据类型转换为另一种数据类型。

### 句法

`CONVERT (_New Data Type, Expression, Style_)`

*   **新数据类型:**也要转换的新数据类型。例如:nvarchar、整数、小数、日期
*   **表达式:**要转换的数据。
*   **样式:**格式。例如:样式 110 是美国日期格式 mm-dd-yyyy

### 示例:将十进制数转换为整数

`SELECT CONVERT(INT, 23.456) as IntegerNumber`

![convert a decimal number to integer number](img/4de97104540238e0e2a3b2fdca456bc1.png)

注意:结果被截断。

### 示例:将字符串转换为日期

`SELECT CONVERT(DATE, '20161030') as Date`

![convert a string to a date type](img/58a31441e8742ad15d9cde24867cd83e.png)

### 示例:将十进制转换为字符串

`SELECT CONVERT(nvarchar, 20.123) as StringData`

![convert a decimal to a string](img/1601f02633a198cc49ed217d89761fba.png)

### 示例:将整数转换为小数

`SELECT CONVERT(DECIMAL (15,3), 13) as DecimalNumber`

![convert an integer to a decimal number](img/d70b3ef66eaeb939fbf9a86314ca6171.png)

### 示例:将字符串转换为美国日期格式的日期格式

`SELECT CONVERT(DATE, '20171030' , 110) To_USA_DateFormat`

![convert a string to date format in usa date style](img/8414f637a41894aa02304cf8e9bd97d0.png)