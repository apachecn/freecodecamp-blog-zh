# 用示例语法解释的 SQL In 语句

> 原文：<https://www.freecodecamp.org/news/the-sql-in-statement-explained-with-example-syntax/>

## IN 运算符定义的

在`WHERE`或`HAVING`(作为`GROUP BY`的一部分)中使用了`IN`操作符，将选中的行限制在一个列表的项目中。

以下是当前完整的学生列表，用于与`WHERE`子句结果集进行比较:

```
select studentID, FullName, sat_score, rcd_updated from student; 
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

列表中将显示 SAT 分数为(1000，2400)的行:

```
select studentID, FullName, sat_score, rcd_updated
from student
where sat_score in (1000, 2400); 
```

```
+-----------+------------------------+-----------+---------------------+
| studentID | FullName               | sat_score | rcd_updated         |
+-----------+------------------------+-----------+---------------------+
|         3 | Spencer Pautier        |      1000 | 2017-08-16 15:34:50 |
|         7 | Edgar Frank "Ted" Codd |      2400 | 2017-08-16 15:35:33 |
|         8 | Donald D. Chamberlin   |      2400 | 2017-08-16 15:35:33 |
|         9 | Raymond F. Boyce       |      2400 | 2017-08-16 15:35:33 |
+-----------+------------------------+-----------+---------------------+
4 rows in set (0.00 sec) 
```