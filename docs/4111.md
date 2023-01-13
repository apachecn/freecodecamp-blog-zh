# SQL Update 语句解释了:更新表的查询(包括 MySQL 示例)

> 原文：<https://www.freecodecamp.org/news/the-sql-update-statement-explained/>

## 更新查询能做什么

更新查询使 DBA 或使用 SQL 的程序员能够用一个命令更新许多记录。

重要的安全提示:在更改之前，请务必备份您要更改的内容。

本指南将:

*   向学生表中添加一个新字段
*   测试用学校分配的电子邮件地址更新该字段的逻辑
*   更新新字段。

这是我们开始这一过程时的学生表

```
SELECT * FROM student; 
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

### 改变表格并添加新字段

```
 ALTER TABLE `fcc_sql_guides_database`.`student` 
	ADD COLUMN `schoolEmailAdr` VARCHAR(125) NULL AFTER `programOfStudy`; 
```

执行 alter 后的 student 表。

```
mysql> SELECT FullName, sat_score, programOfStudy, schoolEmailAdr FROM student;
+------------------------+-----------+------------------+----------------+
| FullName               | sat_score | programOfStudy   | schoolEmailAdr |
+------------------------+-----------+------------------+----------------+
| Monique Davis          |       400 | Literature       | NULL           |
| Teri Gutierrez         |       800 | Programming      | NULL           |
| Spencer Pautier        |      1000 | Programming      | NULL           |
| Louis Ramsey           |      1200 | Programming      | NULL           |
| Alvin Greene           |      1200 | Programming      | NULL           |
| Sophie Freeman         |      1200 | Programming      | NULL           |
| Edgar Frank "Ted" Codd |      2400 | Computer Science | NULL           |
| Donald D. Chamberlin   |      2400 | Computer Science | NULL           |
| Raymond F. Boyce       |      2400 | Computer Science | NULL           |
+------------------------+-----------+------------------+----------------+
9 rows in set (0.00 sec) 
```

### 测试逻辑(非常重要的一步！)

```
SELECT FullName, instr(FullName," ") AS firstSpacePosition, 
concat(substring(FullName,1,instr(FullName," ")-1),"@someSchool.edu") AS schoolEmail
FROM student; 
```

```
+------------------------+--------------------+------------------------+
| FullName               | firstSpacePosition | schoolEmail            |
+------------------------+--------------------+------------------------+
| Monique Davis          |                  8 | Monique@someSchool.edu |
| Teri Gutierrez         |                  5 | Teri@someSchool.edu    |
| Spencer Pautier        |                  8 | Spencer@someSchool.edu |
| Louis Ramsey           |                  6 | Louis@someSchool.edu   |
| Alvin Greene           |                  6 | Alvin@someSchool.edu   |
| Sophie Freeman         |                  7 | Sophie@someSchool.edu  |
| Edgar Frank "Ted" Codd |                  6 | Edgar@someSchool.edu   |
| Donald D. Chamberlin   |                  7 | Donald@someSchool.edu  |
| Raymond F. Boyce       |                  8 | Raymond@someSchool.edu |
+------------------------+--------------------+------------------------+
9 rows in set (0.00 sec) 
```

关于 concat()的一个注意事项:在 MySQL 中，这个命令用于组合字符串，在其他 SQL 版本中不是这样(查看您的手册)。在这种用法中，它是这样工作的:直到但不包括第一个空格的全名字段的子字符串与“@someSchool.edu”组合。在现实世界中，这要复杂得多，您需要确保电子邮件地址是唯一的。

### 做更新

我们将假设这就是我们想要的，并用此信息更新表格:

```
UPDATE student SET schoolEmailAdr = concat(substring(FullName,1,instr(FullName," ")-1),"@someSchool.edu")
WHERE schoolEmailAdr is NULL; 
```

成功！

```
mysql> SELECT FullName, sat_score, programOfStudy, schoolEmailAdr FROM student;
+------------------------+-----------+------------------+------------------------+
| FullName               | sat_score | programOfStudy   | schoolEmailAdr         |
+------------------------+-----------+------------------+------------------------+
| Monique Davis          |       400 | Literature       | Monique@someSchool.edu |
| Teri Gutierrez         |       800 | Programming      | Teri@someSchool.edu    |
| Spencer Pautier        |      1000 | Programming      | Spencer@someSchool.edu |
| Louis Ramsey           |      1200 | Programming      | Louis@someSchool.edu   |
| Alvin Greene           |      1200 | Programming      | Alvin@someSchool.edu   |
| Sophie Freeman         |      1200 | Programming      | Sophie@someSchool.edu  |
| Edgar Frank "Ted" Codd |      2400 | Computer Science | Edgar@someSchool.edu   |
| Donald D. Chamberlin   |      2400 | Computer Science | Donald@someSchool.edu  |
| Raymond F. Boyce       |      2400 | Computer Science | Raymond@someSchool.edu |
+------------------------+-----------+------------------+------------------------+
9 rows in set (0.00 sec) 
```

与所有这些 SQL 内容一样，它们比本入门指南中的内容要多得多。

我希望这至少能给你足够的时间开始。

请参阅数据库管理器的手册，并亲自尝试不同的选项。

## 如何使用 Update 语句？

要更新表中的记录，可以使用`UPDATE`语句。

小心点。您可以更新表中的所有记录，也可以只更新一部分记录。使用`WHERE`条件指定您想要更新哪些记录。一次可以更新一列或多列。语法是:

```
UPDATE table_name
SET column1 = value1, 
    column2 = value2, ...
WHERE condition; 
```

下面是一个更新 Id 为 4 的记录名称的示例:

```
UPDATE Person
SET Name = “Elton John”
WHERE Id = 4; 
```

还可以通过使用其他表中的值来更新表中的列。使用`JOIN`子句从多个表中获取数据。语法是:

```
UPDATE table_name1
SET table_name1.column1 = table_name2.columnA
    table_name1.column2 = table_name2.columnB
FROM table_name1
JOIN table_name2 ON table_name1.ForeignKey = table_name2.Key 
```

以下是更新所有记录的经理的示例:

```
UPDATE Person
SET Person.Manager = Department.Manager
FROM Person
JOIN Department ON Person.DepartmentID = Department.ID 
```