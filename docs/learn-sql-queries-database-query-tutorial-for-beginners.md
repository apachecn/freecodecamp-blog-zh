# 学习 SQL 查询——初学者数据库查询教程

> 原文：<https://www.freecodecamp.org/news/learn-sql-queries-database-query-tutorial-for-beginners/>

SQL 代表结构化查询语言，是一种用于管理数据库中数据的语言。SQL 由命令和声明性语句组成，这些命令和声明性语句充当数据库的指令，以便数据库可以执行任务。

您可以使用 SQL 命令在数据库中创建表，添加和更改大量数据，搜索数据以快速找到特定内容，或者删除所有表。

在本文中，我们将研究一些最常见的 SQL 命令，以及如何使用它们来有效地查询数据库——即请求特定的信息。

## 数据库的基本结构

在我们开始之前，您应该了解数据库的层次结构。

SQL 数据库是存储在表中的相关信息的集合。每个表都有描述其中数据的列和包含实际数据的行。字段是一行中的一段数据。所以要获取想要的数据，我们需要变得具体。

例如，一个远程公司可以有多个数据库。要查看他们的数据库的完整列表，我们可以键入`SHOW DATABASES;`并在`Employees`数据库上进行分区。

输出将如下所示:

```
+--------------------+
|     Databases      |
+--------------------+
| mysql              |
| information_schema |
| employees          |
| test               |
| sys                |
+--------------------+ 
```

一个数据库可以有多个表。以上面的例子为例，为了查看`employees`数据库中不同的表，我们可以做`SHOW TABLES in employees;`。表格可以是`Engineering`、`Product`、`Marketing`、`Sales`用于公司不同的团队。

```
+----------------------+
| Tables_in_employees  |
+----------------------+
| engineering          |
| product              |
| marketing            |
| sales                |
+----------------------+ 
```

所有的表都由描述数据的不同列组成。

要查看不同的列，请使用`Describe Engineering;`。例如，工程表可以包含定义单个属性的列，如`employee_id`、`first_name`、`last_name`、`email`、`country`和`salary`。

以下是输出结果:

```
+-----------+-------------------+--------------+
| Name      |         Null      |      Type    |  
+-----------+-------------------+--------------+
|EMPLOYEE_ID| NOT NULL          | INT(6)       |  
|FIRST_NAME | NOT NULL          |VARCHAR2(20)  |
|LAST_NAME  | NOT NULL          |VARCHAR2(25)  | 
|EMAIL      | NOT NULL          |VARCHAR2(255) |
|COUNTRY    | NOT NULL          |VARCHAR2(30)  |
|SALARY     | NOT NULL          |DECIMAL(10,2) |
+-----------+-------------------+--------------+ 
```

表格也由行组成，行是表格中的单个条目。例如，一行将包括`employee_id`、`first_name`、`last_name`、`email`、`salary`和`country`下的条目。这些行将定义并提供工程团队中一个人的信息。

## 基本 SQL 查询

您可以对数据进行的所有操作都遵循 CRUD 缩写。

CRUD 代表我们查询数据库时执行的 4 个主要操作:创建、读取、更新和删除。

我们`CREATE`数据库中的信息，我们`READ`/从数据库中检索信息，我们`UPDATE`/操纵它，如果我们愿意，我们可以`DELETE`它。

下面我们将从一些基本的 SQL 查询及其语法开始。

### SQL `CREATE DATABASE`语句

要创建一个名为`engineering`的数据库，我们可以使用下面的代码:

```
CREATE DATABASE engineering; 
```

### SQL `CREATE TABLE`语句

```
CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype
); 
```

该查询在数据库中创建一个新表。

它给了表一个名称，我们希望表拥有的不同列也被传入。

我们可以使用多种数据类型。最常见的有:`INT`、`DECIMAL`、`DATETIME`、`VARCHAR`、`NVARCHAR`、`FLOAT`和`BIT`。

根据上面的示例，这可能类似于以下代码:

```
CREATE TABLE engineering (
employee_id  int(6) NOT NULL,
first_name   varchar(20) NOT NULL,
last_name  varchar(25) NOT NULL,
email  varchar(255) NOT NULL,
country varchar(30),
salary  decimal(10,2) NOT NULL
); 
```

我们根据这些数据创建的表看起来类似于这样:

| 员工 id | 名字 | 姓氏 | 电子邮件 | 国家 | 薪水 |
| --- | --- | --- | --- | --- | --- |
| ﻿ |  |  |  |  |  |

### SQL `ALTER TABLE`语句

创建表后，我们可以通过添加另一列来修改它。

```
ALTER TABLE table_name 
ADD column_name datatype; 
```

例如，如果需要，我们可以通过键入以下命令向现有表中添加一个`birthday`列:

```
ALTER TABLE engineering
ADD  birthday date; 
```

现在我们的表看起来像这样:

| 员工 id | 名字 | 姓氏 | 电子邮件 | 国家 | 薪水 | 生日 |
| --- | --- | --- | --- | --- | --- | --- |
| ﻿ |  |  |  |  |  |  |

### SQL `INSERT`语句

这就是我们如何将数据插入表中并创建新行。这是 CRUD 中的`C`部分。

```
INSERT INTO table_name(column1, column2, column3,..) 
VALUES(value1, 'value2', value3,..); 
```

在`INSERT INTO`部分，我们可以指定想要用信息填充的列。

里面是我们想要存储的信息。这将在表中创建新记录，即新行。

每当我们插入字符串值时，它们都用单引号括起来，`''`。

例如:

```
INSERT INTO table_name(employee_id,first_name,last_name,email,country,salary) 
VALUES
(1,'Timmy','Jones','timmy@gmail.com','USA',2500.00);
(2,'Kelly','Smith','ksmith@gmail.com','UK',1300.00); 
```

该表现在看起来像这样:

| 员工 id | 名字 | 姓氏 | 电子邮件 | 国家 | 薪水 |
| --- | --- | --- | --- | --- | --- |
| one | 蒂米 | 琼斯 | [timmy@gmail.com](mailto:timmy@gmail.com) | 美利坚合众国 | Two thousand five hundred |
| Two | 凯利 | 史密斯（姓氏） | [ksmith@gmail.com](mailto:ksmith@gmail.com) | 英国 | One thousand three hundred |

### SQL `SELECT`语句

该语句从数据库中获取数据。这是 CRUD 的`R`部分。

```
SELECT  column1,column2
FROM    table_name; 
```

根据我们前面的示例，这看起来像下面这样:

```
SELECT first_name,last_name
FROM   engineering; 
```

输出:

```
+-----------+----------+
|FirstName  | LastName |
+-----------+----------+
| Timmy     | Jones    |
| Kelly     | Smith    |
+-----------+----------+ 
```

`SELECT`语句指向我们想要从中获取数据的特定列，我们想要在结果中显示该列。

`FROM`部分决定了表格本身。

这里有另一个`SELECT`的例子:

```
SELECT * FROM table_name; 
```

星号`*`将从我们指定的表中获取所有信息。

### SQL `WHERE`语句

`WHERE`让我们的查询更加具体。

如果我们想要过滤我们的`Engineering`表来搜索有特定薪水的雇员，我们将使用`WHERE`。

```
SELECT employee_id,first_name,last_name,email,country
FROM engineering
WHERE salary > 1500 
```

上一个示例中的表:

| 员工 id | 名字 | 姓氏 | 电子邮件 | 国家 | 薪水 |
| --- | --- | --- | --- | --- | --- |
| one | 蒂米 | 琼斯 | [timmy@gmail.com](mailto:timmy@gmail.com) | 美利坚合众国 | Two thousand five hundred |
| Two | 凯利 | 史密斯（姓氏） | [ksmith@gmail.com](mailto:ksmith@gmail.com) | 英国 | One thousand three hundred |

现在会有如下输出:

```
+-----------+----------+----------+----------------+------------+
|employee_id|first_name|last_name |email           |country     |
+-----------+----------+----------+----------------+------------+
|          1| Timmy    |Jones     |timmy@gmail.com | USA        |
+-----------+----------+----------+----------------+------------+ 
```

这将过滤并显示满足条件的结果——也就是说，它只向*和*显示薪水为`more than 1500`的人员的行。

### SQL `AND`、`OR`和`BETWEEN`操作符

这些操作符允许您通过向`WHERE`语句添加更多的条件来使查询更加具体。

`AND`操作符接受两个条件，它们都必须是`true`才能在结果中显示该行。

```
SELECT column_name
FROM table_name
WHERE column1 =value1
    AND column2 = value2; 
```

`OR`操作符接受两个条件，为了在结果中显示行，其中一个必须为真。

```
SELECT column_name
FROM table_name
WHERE column_name = value1
    OR column_name = value2; 
```

`BETWEEN`运算符过滤掉特定范围内的数字或文本。

```
SELECT column1,column2
FROM table_name
WHERE column_name BETWEEN value1 AND value2; 
```

我们也可以将这些运算符相互结合使用。

假设我们的桌子现在是这样的:

| 员工 id | 名字 | 姓氏 | 电子邮件 | 国家 | 薪水 |
| --- | --- | --- | --- | --- | --- |
| one | 蒂米 | 琼斯 | [timmy@gmail.com](mailto:timmy@gmail.com) | 美利坚合众国 | Two thousand five hundred |
| Two | 凯利 | 史密斯（姓氏） | [ksmith@gmail.com](mailto:ksmith@gmail.com) | 英国 | One thousand three hundred |
| three | 吉姆(人名) | 怀特（姓氏） | [jwhite@gmail.com](mailto:jwhite@gmail.com) | 英国 | One thousand two hundred point seven six |
| four | 何塞·路易斯 | 马丁内斯 martinez | [jmart@gmail.com](mailto:jmart@gmail.com) | 墨西哥 | One thousand two hundred and seventy-five point eight seven |
| five | 伊米莉亚 | 费希尔 | [emfis@gmail.com](mailto:emfis@gmail.com) | 德国 | Two thousand three hundred and sixty-five point nine |
| six | 戴尔芬 | 拉维妮（姓氏） | [lavigned@gmail.com](mailto:lavigned@gmail.com) | 法国 | Two thousand one hundred and eight |
| seven | 路易斯（号外乐团成员） | 迈耶 | [lmey@gmail.com](mailto:lmey@gmail.com) | 德国 | Two thousand one hundred and forty-five point seven |

如果我们使用如下语句:

```
SELECT * FROM engineering
WHERE  employee_id BETWEEN 3 AND 7
        AND 
        country = 'Germany'; 
```

我们会得到这样的输出:

```
+------------+-----------+-----------+----------------+--------+--------+
|employee_id | first_name| last_name | email          |country |salary  |
+------------+-----------+-----------+----------------+--------+--------+
|5           |Emilia     |Fischer    |emfis@gmail.com | Germany| 2365.90|
|7           |Louis      |Meyer      |lmey@gmail.com  | Germany| 2145.70|
+------------+-----------+-----------+----------------+--------+--------+ 
```

这将选择所有在`3 and 7` `AND`之间有一个`employee_id`的列都有一个国家。

### SQL `ORDER BY`语句

`ORDER BY`按照我们在`SELECT`语句中提到的列进行排序。

它对结果进行排序，并按照字母或数字的降序或升序显示它们(默认顺序是升序)。

我们可以用命令:`ORDER BY column_name DESC | ASC`来指定。

```
SELECT employee_id, first_name, last_name,salary
FROM engineering
ORDER BY salary DESC; 
```

在上面的例子中，我们对工程团队中员工的工资进行了排序，并以数字降序显示。

### SQL `GROUP BY`语句

让我们合并具有相同数据和相似性的行。

这有助于整理在表中多次出现的重复数据和条目。

```
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name; 
```

这里`COUNT(*)`分别计算每一行，并返回指定表格中的行数，同时保留重复的行。

### SQL `LIMIT`语句

`LIMIT`允许您指定结果中应该返回的最大*行数*。

这在处理大型数据集时很有帮助，因为大型数据集可能会导致查询需要很长时间才能运行。通过限制你得到的结果，它可以节省你的时间。

```
SELECT column1,column2
FROM table_name
LIMIT number; 
```

### SQL `UPDATE`语句

这就是我们如何更新表中的一行。这是垃圾的`U`部分。

```
UPDATE table_name 
SET column1 = value1, 
    column2 = value2 
WHERE condition; 
```

`WHERE`条件指定了您想要编辑的记录。

```
UPDATE engineering
SET    country = 'Spain'
WHERE   employee_id = 1 
```

我们之前的桌子:

| 员工 id | 名字 | 姓氏 | 电子邮件 | 国家 | 薪水 |
| --- | --- | --- | --- | --- | --- |
| one | 蒂米 | 琼斯 | [timmy@gmail.com](mailto:timmy@gmail.com) | 美利坚合众国 | Two thousand five hundred |
| Two | 凯利 | 史密斯（姓氏） | [ksmith@gmail.com](mailto:ksmith@gmail.com) | 英国 | One thousand three hundred |
| three | 吉姆(人名) | 怀特（姓氏） | [jwhite@gmail.com](mailto:jwhite@gmail.com) | 英国 | One thousand two hundred point seven six |
| four | 何塞·路易斯 | 马丁内斯 martinez | [jmart@gmail.com](mailto:jmart@gmail.com) | 墨西哥 | One thousand two hundred and seventy-five point eight seven |
| five | 伊米莉亚 | 费希尔 | [emfis@gmail.com](mailto:emfis@gmail.com) | 德国 | Two thousand three hundred and sixty-five point nine |
| six | 戴尔芬 | 拉维妮（姓氏） | [lavigned@gmail.com](mailto:lavigned@gmail.com) | 法国 | Two thousand one hundred and eight |
| seven | 路易斯（号外乐团成员） | 迈耶 | [lmey@gmail.com](mailto:lmey@gmail.com) | 德国 | Two thousand one hundred and forty-five point seven |

现在看起来像这样:

| 员工 id | 名字 | 姓氏 | 电子邮件 | 国家 | 薪水 |
| --- | --- | --- | --- | --- | --- |
| one | 蒂米 | 琼斯 | [timmy@gmail.com](mailto:timmy@gmail.com) | 西班牙 | Two thousand five hundred |
| Two | 凯利 | 史密斯（姓氏） | [ksmith@gmail.com](mailto:ksmith@gmail.com) | 英国 | One thousand three hundred |
| three | 吉姆(人名) | 怀特（姓氏） | [jwhite@gmail.com](mailto:jwhite@gmail.com) | 英国 | One thousand two hundred point seven six |
| four | 何塞·路易斯 | 马丁内斯 martinez | [jmart@gmail.com](mailto:jmart@gmail.com) | 墨西哥 | One thousand two hundred and seventy-five point eight seven |
| five | 伊米莉亚 | 费希尔 | [emfis@gmail.com](mailto:emfis@gmail.com) | 德国 | Two thousand three hundred and sixty-five point nine |
| six | 戴尔芬 | 拉维妮（姓氏） | [lavigned@gmail.com](mailto:lavigned@gmail.com) | 法国 | Two thousand one hundred and eight |
| seven | 路易斯（号外乐团成员） | 迈耶 | [lmey@gmail.com](mailto:lmey@gmail.com) | 德国 | Two thousand one hundred and forty-five point seven |

这将更新 id 为 1 的雇员的居住国列。

我们还可以用另一个带有`JOIN`的表中的值来更新一个表中的信息。

```
UPDATE table_name
SET table_name1.column_name1 = table_name2.column_name1
    table_name1.column_name2 = table_name2.column2
FROM table_name1
JOIN table_name2 
    ON table_name1.column_name = table_2.column_name; 
```

### SQL `DELETE`语句

`DELETE`是 CRUD 的`D`部分。我们就是这样从表中删除记录的。

基本语法如下所示:

```
DELETE FROM table_name 
WHERE condition; 
```

例如，在我们的`engineering`示例中，可能是这样的:

```
DELETE FROM engineering
WHERE employee_id = 2; 
```

这将删除工程团队中 id 为 2 的雇员的记录。

### SQL `DROP COLUMN`语句

要从表中删除特定的列，我们应该这样做:

```
ALTER TABLE table_name 
DROP COLUMN column_name; 
```

### SQL `DROP TABLE`语句

要删除整个表格，我们可以这样做:

```
DROP TABLE table_name; 
```

## 结论

在本文中，我们介绍了一些 SQL 初学者会用到的基本查询。

我们学习了如何创建表和行，如何收集和更新信息，以及如何删除数据。我们还将 SQL 查询映射到它们对应的 CRUD 操作。

感谢阅读和快乐编码！