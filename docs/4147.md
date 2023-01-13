# SQL Left Join 语句:示例语法

> 原文：<https://www.freecodecamp.org/news/sql-left-join-commands-example-syntax/>

## 在本指南中，我们将讨论 SQL 左连接。

在 JOIN 之前使用关键字 LEFT 会导致系统从 student(左)表开始，但是如果左表 student 没有行，将从右表返回 NULL。

注意，studentID 9 出现在这里，但是 contact 表中的数据显示为 NULL。

```
SELECT a.studentID, a.FullName, a.programOfStudy,
b.`student-phone-cell`, b.`student-US-zipcode`
FROM student AS a
LEFT JOIN `student-contact-info` AS b ON a.studentID = b.studentID; 
```

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
|         9 | Raymond F. Boyce       | Computer Science | NULL               |               NULL |
+-----------+------------------------+------------------+--------------------+--------------------+
9 rows in set (0.00 sec) 
```

### 供参考的完整表格列表

学生或左表 SQL

```
SELECT a.studentID, a.FullName, sat_score, a.programOfStudy, schoolEmailAdr 
FROM student AS a; 
```

学生或左表数据

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

student contact or RIGHT table SQL	
```sql
select * from `student-contact-info` as b; 
```

学生联系人或右表数据

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

与所有这些 SQL 内容一样，它们比本入门指南中的内容要多得多。

我希望这至少能给你足够的时间开始。

请参阅数据库管理器的手册，并亲自尝试不同的选项。