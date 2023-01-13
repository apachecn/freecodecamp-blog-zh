# SQL Select 语句:Select Distinct、Select Into、Insert into 等语句的示例

> 原文：<https://www.freecodecamp.org/news/sql-select-statements/>

## Select 和 From 子句

查询的选择部分通常是确定在结果中显示哪些数据列。还可以应用一些选项来显示不是表列的数据。

此示例显示了从“student”表中选择的三列和一个计算列。数据库存储学生的 studentID、名字和姓氏。我们可以将名字和姓氏列组合起来创建全名计算列。

```
select studentID, FirstName, LastName, FirstName + ' ' + LastName as FullName
from student;
```

```
+-----------+-------------------+------------+------------------------+
| studentID | FirstName         | LastName   | FullName               |
+-----------+-------------------+------------+------------------------+
|         1 | Monique           | Davis      | Monique Davis          |
|         2 | Teri              | Gutierrez  | Teri Gutierrez         |
|         3 | Spencer           | Pautier    | Spencer Pautier        |
|         4 | Louis             | Ramsey     | Louis Ramsey           |
|         5 | Alvin             | Greene     | Alvin Greene           |
|         6 | Sophie            | Freeman    | Sophie Freeman         |
|         7 | Edgar Frank "Ted" | Codd       | Edgar Frank "Ted" Codd |
|         8 | Donald D.         | Chamberlin | Donald D. Chamberlin   |
|         9 | Raymond F.        | Boyce      | Raymond F. Boyce       |
+-----------+-------------------+------------+------------------------+
9 rows in set (0.00 sec)
```

*与所有这些 SQL 内容一样，它们比本入门指南中的内容要多得多。

我希望这至少能给你足够的时间开始。

请参阅数据库管理器的手册，并亲自尝试不同的选项。

## **SQL Select Distinct 语句**

### **简介**

这个关键字允许我们获得一列中唯一值的列表。本指南将证明这一点。

### **学生表中数据的完整显示**

```
USE fcc_sql_guides_database;
SELECT studentID, FullName, sat_score, programOfStudy, rcd_Created, rcd_Updated FROM student;
```

```
+-----------+------------------------+-----------+------------------+---------------------+---------------------+
| studentID | FullName               | sat_score | programOfStudy   | rcd_Created         | rcd_Updated         |
+-----------+------------------------+-----------+------------------+---------------------+---------------------+
|         1 | Monique Davis          |       400 | Literature       | 2017-08-16 15:34:50 | 2017-08-16 15:34:50 |
|         2 | Teri Gutierrez         |       800 | Programming      | 2017-08-16 15:34:50 | 2017-08-16 15:34:50 |
|         3 | Spencer Pautier        |      1000 | Programming      | 2017-08-16 15:34:50 | 2017-08-16 15:34:50 |
|         4 | Louis Ramsey           |      1200 | Programming      | 2017-08-16 15:34:50 | 2017-08-16 15:34:50 |
|         5 | Alvin Greene           |      1200 | Programming      | 2017-08-16 15:34:50 | 2017-08-16 15:34:50 |
|         6 | Sophie Freeman         |      1200 | Programming      | 2017-08-16 15:34:50 | 2017-08-16 15:34:50 |
|         7 | Edgar Frank "Ted" Codd |      2400 | Computer Science | 2017-08-16 15:35:33 | 2017-08-16 15:35:33 |
|         8 | Donald D. Chamberlin   |      2400 | Computer Science | 2017-08-16 15:35:33 | 2017-08-16 15:35:33 |
|         9 | Raymond F. Boyce       |      2400 | Computer Science | 2017-08-16 15:35:33 | 2017-08-16 15:35:33 |
+-----------+------------------------+-----------+------------------+---------------------+---------------------+
9 rows in set (0.00 sec)
```

### **获取研究领域列表**

```
SELECT DISTINCT programOfStudy FROM student;
```

```
+------------------+
| programOfStudy   |
+------------------+
| Literature       |
| Programming      |
| Computer Science |
+------------------+
3 rows in set (0.00 sec)
```

与所有这些 SQL 内容一样，它们比本入门指南中的内容要多得多。

我希望这至少能给你足够的时间开始。

请参阅数据库管理器的手册，并亲自尝试不同的选项。

## **SQL 选择进入语句**

`SELECT INTO`语句是一个查询，允许您创建一个*新的*表，并用一个`SELECT statement`的结果集填充它。要将数据添加到现有的表中，请参见 [INSERT INTO](https://guide.freecodecamp.org/sql/sql-select-into-statement/guides/src/pages/sql/sql-insert-into-select-statement/index.md) 语句。

当您将多个表格或视图中的数据合并到一个新表格中时，可以使用`SELECT INTO`。 ¹ 原表不受影响。

一般语法是:

```
SELECT column-names
  INTO new-table-name
  FROM table-name
 WHERE EXISTS 
      (SELECT column-name
         FROM table-name
        WHERE condition)
```

本例显示了一组从“供应商”表“复制”到一个名为 SupplierUSA 的新表中的表，该表保存了与列 country 值“USA”相关的表集。

```
SELECT * INTO SupplierUSA
  FROM Supplier
 WHERE Country = 'USA';
```

****结果**** : 4 行受影响 ^(2 行)

idcompanynamcontactnamecity country phone 2 new Orleans Cajun delights Shelley Burke new Orleans USA(100)555-48223 Kelly 奶奶的宅邸 Regina MurphyAnn arbor USA(313)555-573516 Bigfoot brewerie scheryl saylrbendusanull 19 new England Seafood cannery Robb MerchantBostonUSA(617)555-3267

请参阅数据库管理器的手册，并亲自尝试不同的选项。

## **SQL 插入语句**

要在表中插入记录，可以使用`INSERT INTO`语句。

有两种方法可以做到这一点，如果你想只在某些列中插入值，你必须列出它们的名称，包括所有的强制列。语法是:

```
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

另一种方法是向表中的所有列插入值，不需要指定列名。语法是:

```
INSERT INTO table_name 
VALUES (value1, value2, value3, ...);
```

下面是一个以两种方式在表 Person 中插入记录的示例:

```
INSERT INTO Person
VALUES (1, ‘John Lennon’, ‘1940-10-09’, ‘M’);
```

和

```
INSERT INTO Person(Id, Name, DateOfBirth, Gender)
VALUES (1, ‘John Lennon’, ‘1940-10-09’, ‘M’);
```

一些 SQL 版本(例如 MySQL)支持一次插入多行。例如:

```
INSERT INTO Person(Id, Name, DateOfBirth, Gender)
VALUES (1, ‘John Lennon’, ‘1940-10-09’, ‘M’), (2, ‘Paul McCartney’, ‘1942-06-18’, ‘M’),
(3, ‘George Harrison’, ‘1943-02-25’, ‘M’), (4, ‘Ringo Starr’, ‘1940-07-07’, ‘M’)
```

注意，整个原始查询保持不变——我们只是添加了由 paranthesis 包围并由逗号分隔的数据行。

## **SQL 插入 Select 语句**

您可以使用已经存储在数据库中的数据在表中插入记录。这只是数据的副本，并不影响源表。

`INSERT INTO SELECT`语句结合了`INSERT INTO`和`SELECT`语句，您可以使用任何想要的条件。语法是:

```
INSERT INTO table2 (column1, column2, column3, ...)
SELECT column1, column2, column3, ...
FROM table1
WHERE condition;
```

这里有一个例子，在表 Person 中插入表 students 中的所有男学生。

```
INSERT INTO Person(Id, Name, DateOfBirth, Gender)
SELECT Id, Name, DateOfBirth, Gender
FROM Students
WHERE Gender = ‘M’
```

## 其他 SQL 资源:

*   [SQL 和数据库全视频课程](https://www.freecodecamp.org/news/sql-and-databases-full-course/)
*   [你应该知道的基本 SQL 命令](https://www.freecodecamp.org/news/basic-sql-commands/)