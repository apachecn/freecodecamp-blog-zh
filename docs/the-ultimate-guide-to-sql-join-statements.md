# SQL 连接语句终极指南:左、右、内、外

> 原文：<https://www.freecodecamp.org/news/the-ultimate-guide-to-sql-join-statements/>

## 什么是 SQL Join 语句？

在本指南中，我们将讨论 SQL 语句的 JOIN 部分。我们将介绍它的语法和 SQL 支持的不同类型的连接。

### 关注连接的 SQL 语法

```
SELECT col1, col2, col3, etc....
FROM  tableNameOne AS a
JOIN tableNameTwo AS b ON a.primeKey = b.primeKey 
etc... 
```

JOIN 语句可以是相同的 just JOIN 或 INNER JOIN，也可以是 LEFT JOIN(如下所述)。

### 不同类型的联接

*   (内部)联接
*   返回在两个表中都有匹配值的记录
*   左(外)连接
*   返回左表中的所有记录，以及右表中的匹配记录
*   右(外)连接
*   返回右表中的所有记录，以及左表中的匹配记录
*   完全(外部)连接
*   当左表或右表中有匹配项时，返回所有记录

### 加入

student 表将位于 FROM 子句中，因此它将是一个起始表或左表。

我们将把它加入到学生联系表或右表中。

您将看到所有学生都出现在联系表中。

如下表所示，studentID 9 在 student 表中，但不在 contact 表中，因此不会出现在连接中。

SQL 语句

```
SELECT a.studentID, a.FullName, a.programOfStudy,
b.`student-phone-cell`, b.`student-US-zipcode`
FROM student AS a
JOIN `student-contact-info` AS b ON a.studentID = b.studentID; 
```

“已加入”数据:

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

### 左连接

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

学生表列表

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
SELECT * from `student-contact-info` AS b; 
```

学生联系人或右表

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

### 使用示例

在本指南中，我们将讨论 SQL 右连接。

### 右连接

RIGHT JOIN 关键字返回右表(表 2)中的所有记录，以及左表(表 1)中的匹配记录。当没有匹配时，结果从左侧为空。

```
SELECT *
FROM table1
RIGHT JOIN table2
ON table1.column_name = table2.column_name; 
```

### 供参考的完整表格列表

食物或左桌数据

```
+---------+--------------+-----------+------------+
| ITEM_ID | ITEM_NAME    | ITEM_UNIT | COMPANY_ID |
+---------+--------------+-----------+------------+
| 1       | Chex Mix     | Pcs       | 16         |
| 6       | Cheez-It     | Pcs       | 15         |
| 2       | BN Biscuit   | Pcs       | 15         |
| 3       | Mighty Munch | Pcs       | 17         |
| 4       | Pot Rice     | Pcs       | 15         |
| 5       | Jaffa Cakes  | Pcs       | 18         |
| 7       | Salt n Shake | Pcs       |            |
+---------+--------------+-----------+------------+

company or RIGHT table data
``` text
+------------+---------------+--------------+
| COMPANY_ID | COMPANY_NAME  | COMPANY_CITY |
+------------+---------------+--------------+
| 18         | Order All     | Boston       |
| 15         | Jack Hill Ltd | London       |
| 16         | Akas Foods    | Delhi        |
| 17         | Foodies.      | London       |
| 19         | sip-n-Bite.   | New York     |
+------------+---------------+--------------+ 
```

要从 company 表中获取 company name，从 foods 表中获取 company ID、item name 列，可以使用以下 SQL 语句:

```
SELECT company.company_id,company.company_name,
company.company_city,foods.company_id,foods.item_name
FROM   company
RIGHT JOIN foods
ON company.company_id = foods.company_id; 
```

输出

```
COMPANY_ID COMPANY_NAME              COMPANY_CITY              COMPANY_ID ITEM_NAME
---------- ------------------------- ------------------------- ---------- --------------
18         Order All                 Boston                    18         Jaffa Cakes
15         Jack Hill Ltd             London                    15         Pot Rice
15         Jack Hill Ltd             London                    15         BN Biscuit
15         Jack Hill Ltd             London                    15         Cheez-It
16         Akas Foods                Delhi                     16         Chex Mix
17         Foodies.                  London                    17         Mighty Munch
NULL       NULL                      NULL                      NULL       Salt n Shake 
```