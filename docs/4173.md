# SQL Like 操作符用示例语法解释

> 原文：<https://www.freecodecamp.org/news/sql-like-statements-explained-with-example-syntax/>

您可能以前遇到过 LIKE 运算符。在本文中，我们将解释如何使用它。

### LIKE 运算符定义

在`WHERE`或`HAVING`(作为`GROUP BY`的一部分)中使用了`LIKE`操作符，当一列中包含某种字符模式时，该操作符将选定的行限制为条目。

### 本指南将演示:

*   确定字符串是以给定的字符串模式开始还是结束
*   确定字符串中间是否存在模式
*   确定字符串是否不包含在该字符串中

### 列以给定的字符串模式开始或结束

该 SQL 将选择以“Monique”开头或以“Greene”结尾的`FullName`学生。

```
SELECT studentID, FullName, sat_score, rcd_updated
FROM student 
WHERE 
FullName LIKE 'Monique%' OR -- note the % at the end but not the beginning
FullName LIKE '%Greene'; -- note the % at the beginning but not the end 
```

```
+-----------+---------------+-----------+---------------------+
| studentID | FullName      | sat_score | rcd_updated         |
+-----------+---------------+-----------+---------------------+
|         1 | Monique Davis |       400 | 2017-08-16 15:34:50 |
|         5 | Alvin Greene  |      1200 | 2017-08-16 15:34:50 |
+-----------+---------------+-----------+---------------------+
2 rows in set (0.00 sec) 
```

### 字符串模式位于列的中间

该 SQL 将选择姓名中任何地方带有“ree”的学生。

```
SELECT studentID, FullName, sat_score, rcd_updated
FROM student 
WHERE FullName LIKE '%ree%'; -- note the % at the beginning AND at the end 
```

```
+-----------+----------------+-----------+---------------------+
| studentID | FullName       | sat_score | rcd_updated         |
+-----------+----------------+-----------+---------------------+
|         5 | Alvin Greene   |      1200 | 2017-08-16 15:34:50 |
|         6 | Sophie Freeman |      1200 | 2017-08-16 15:34:50 |
+-----------+----------------+-----------+---------------------+
2 rows in set (0.00 sec) 
```

### 列中没有字符串

您可以在 LIKE 之前放置“NOT ”,以排除具有字符串模式的行，而不是选择它们。此 SQL 排除了全名列中包含“cer Pau”和“Ted”的记录。

```
SELECT studentID, FullName, sat_score, rcd_updated
FROM student 
WHERE FullName NOT LIKE '%cer Pau%' AND FullName NOT LIKE '%"Ted"%'; 
```

```
+-----------+----------------------+-----------+---------------------+
| studentID | FullName             | sat_score | rcd_updated         |
+-----------+----------------------+-----------+---------------------+
|         1 | Monique Davis        |       400 | 2017-08-16 15:34:50 |
|         2 | Teri Gutierrez       |       800 | 2017-08-16 15:34:50 |
|         4 | Louis Ramsey         |      1200 | 2017-08-16 15:34:50 |
|         5 | Alvin Greene         |      1200 | 2017-08-16 15:34:50 |
|         6 | Sophie Freeman       |      1200 | 2017-08-16 15:34:50 |
|         8 | Donald D. Chamberlin |      2400 | 2017-08-16 15:35:33 |
|         9 | Raymond F. Boyce     |      2400 | 2017-08-16 15:35:33 |
+-----------+----------------------+-----------+---------------------+
7 rows in set (0.00 sec) 
```

这里是当前完整的学生列表，用于与上面的 where 子句结果集进行比较。

```
SELECT studentID, FullName, sat_score, rcd_updated FROM student; 
```

```
+-----------+------------------------+-----------+---------------------+
| studentID | FullName               | sat_score | rcd_updated         |
+-----------+------------------------+-----------+---------------------+
|         1 | Monique Davis          |       400 | 2017-08-16 15:34:50 |
|         2 | Teri Gutierrez         |       800 | 2017-08-16 15:34:50 |
|         3 | Spencer Pautier        |      1000 | 2017-08-16 15:34:50 |
|         4 | Louis Ramsey           |      1200 | 2017-08-16 15:34:50 |
|         5 | Alvin Greene           |      1200 | 2017-08-16 15:34:50 |
|         6 | Sophie Freeman         |      1200 | 2017-08-16 15:34:50 |
|         7 | Edgar Frank "Ted" Codd |      2400 | 2017-08-16 15:35:33 |
|         8 | Donald D. Chamberlin   |      2400 | 2017-08-16 15:35:33 |
|         9 | Raymond F. Boyce       |      2400 | 2017-08-16 15:35:33 |
+-----------+------------------------+-----------+---------------------+
9 rows in set (0.00 sec) 
```