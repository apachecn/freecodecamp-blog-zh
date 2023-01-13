# SQL Inner Join 命令:示例语法

> 原文：<https://www.freecodecamp.org/news/the-sql-inner-join-command-example-syntax/>

在本指南中，我们将讨论 SQL(内部)连接

### 解释连接命令(与内部连接相同)

student 表将位于 FROM 子句中，因此它将是一个起始表或左表。

我们将把它加入到学生联系表或右表中。您将看到所有学生都出现在联系表中。如下表所示，studentID 9 在 student 表中，但不在 contact 表中，因此不会出现在连接中。

SQL 语句

```
SELECT a.studentID, a.FullName, a.programOfStudy,
b.`student-phone-cell`, b.`student-US-zipcode`
FROM student AS a
INNER JOIN `student-contact-info` AS b ON a.studentID = b.studentID; 
```

“连接”数据

```
+-----------+------------------------+------------------+--------------------+--------------------+
| studentID | FullName               | programOfStudy   | student-phone-cell | student-US-zipcode |
+-----------+------------------------+------------------+--------------------+--------------------+
|         1 | Monique Davis          | Literature       | 555-555-5551       |              97111 |
|         2 | Teri Gutierrez         | Programming      | 555-555-5552       |              97112 |
|         3 | Spencer Pautier        | Programming      | 555-555-5553       |              97113 |
|         4 | Louis Ramsey           | Programming      | 555-555-5554       |              97114 |
|         5 | Alvin Greene           | Programming      | 555-555-5555       |              97115 |
|         6 | Sophie Freeman         | Programming      | 555-555-5556       |              97116 |
|         7 | Edgar Frank "Ted" Codd | Computer Science | 555-555-5557       |              97117 |
|         8 | Donald D. Chamberlin   | Computer Science | 555-555-5558       |              97118 |
+-----------+------------------------+------------------+--------------------+--------------------+ 
```

### 供参考的完整表格列表

学生表 SQL

```
SELECT a.studentID, a.FullName, sat_score, a.programOfStudy, schoolEmailAdr 
FROM student AS a; 
```

学生或左桌

```
+-----------+------------------------+-----------+------------------+------------------------+
| studentID | FullName               | sat_score | programOfStudy   | schoolEmailAdr         |
+-----------+------------------------+-----------+------------------+------------------------+
|         1 | Monique Davis          |       400 | Literature       | Monique@someSchool.edu |
|         2 | Teri Gutierrez         |       800 | Programming      | Teri@someSchool.edu    |
|         3 | Spencer Pautier        |      1000 | Programming      | Spencer@someSchool.edu |
|         4 | Louis Ramsey           |      1200 | Programming      | Louis@someSchool.edu   |
|         5 | Alvin Greene           |      1200 | Programming      | Alvin@someSchool.edu   |
|         6 | Sophie Freeman         |      1200 | Programming      | Sophie@someSchool.edu  |
|         7 | Edgar Frank "Ted" Codd |      2400 | Computer Science | Edgar@someSchool.edu   |
|         8 | Donald D. Chamberlin   |      2400 | Computer Science | Donald@someSchool.edu  |
|         9 | Raymond F. Boyce       |      2400 | Computer Science | Raymond@someSchool.edu |
+-----------+------------------------+-----------+------------------+------------------------+
9 rows in set (0.00 sec)

```sql
SELECT * FROM `student-contact-info` AS b; 
```

学生联系表或右表

```
+-----------+----------------------------------+--------------------+--------------------+
| studentID | studentEmailAddr                 | student-phone-cell | student-US-zipcode |
+-----------+----------------------------------+--------------------+--------------------+
|         1 | Monique.Davis@freeCodeCamp.org   | 555-555-5551       |              97111 |
|         2 | Teri.Gutierrez@freeCodeCamp.org  | 555-555-5552       |              97112 |
|         3 | Spencer.Pautier@freeCodeCamp.org | 555-555-5553       |              97113 |
|         4 | Louis.Ramsey@freeCodeCamp.org    | 555-555-5554       |              97114 |
|         5 | Alvin.Green@freeCodeCamp.org     | 555-555-5555       |              97115 |
|         6 | Sophie.Freeman@freeCodeCamp.org  | 555-555-5556       |              97116 |
|         7 | Maximo.Smith@freeCodeCamp.org    | 555-555-5557       |              97117 |
|         8 | Michael.Roach@freeCodeCamp.ort   | 555-555-5558       |              97118 |
+-----------+----------------------------------+--------------------+--------------------+
8 rows in set (0.00 sec) 
```

### 结论

与所有这些 SQL 内容一样，它们比本入门指南中的内容要多得多。

我希望这至少能给你足够的时间开始。

请参阅数据库管理器的手册，并亲自尝试不同的选项。