# SQL Where 子句解释了:In、Between、Like 和其他示例

> 原文：<https://www.freecodecamp.org/news/the-sql-where-clause-explained/>

## 什么是 SQL Where 子句？

### `WHERE`条款(和/或、`IN`、`BETWEEN`和`LIKE`)

`WHERE`子句用于限制返回的行数。

在这种情况下，所有这五个将被用来是一些什么可笑的`WHERE`条款。

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

将显示的行是…

*   学生 id 介于 1 和 5 之间(包括 1 和 5)
*   `OR`  studentID = 8

这里有一个更新的查询，其中任何具有 SAT 分数的记录都不会出现在这个列表中(1000，1400):

```
select  studentID, FullName, sat_score, recordUpdated
from    student
where   (studentID between 1 and 5 or studentID = 8)
        and
        sat_score NOT in (1000, 1400); 
```

```
+-----------+----------------------+-----------+---------------------+
| studentID | FullName             | sat_score | rcd_updated         |
+-----------+----------------------+-----------+---------------------+
|         1 | Monique Davis        |       400 | 2017-08-16 15:34:50 |
|         2 | Teri Gutierrez       |       800 | 2017-08-16 15:34:50 |
|         4 | Louis Ramsey         |      1200 | 2017-08-16 15:34:50 |
|         5 | Alvin Greene         |      1200 | 2017-08-16 15:34:50 |
|         8 | Donald D. Chamberlin |      2400 | 2017-08-16 15:35:33 |
+-----------+----------------------+-----------+---------------------+
5 rows in set (0.00 sec) 
```

*与所有这些 SQL 内容一样，它们比本入门指南中的内容要多得多。

我希望这至少能给你足够的时间开始。

请参阅数据库管理器的手册，并亲自尝试不同的选项。