# SQL Order By 语句:示例语法

> 原文：<https://www.freecodecamp.org/news/sql-order-by-statement-example-syntax/>

Order By 是一个 SQL 命令，允许您对 SQL 查询的结果进行排序。

### 订购依据(ASC，DESC)

ORDER BY 为我们提供了一种按照 SELECT 部分中的一项或多项对结果集进行排序的方法。下面是一个 SQL 语句，按照全名降序对学生进行排序。默认的排序顺序是升序(ASC ),但是要以相反的顺序(降序)排序，可以使用 DESC。

以下是查询

```
SELECT studentID, FullName, sat_score
FROM student
ORDER BY FullName DESC; 
```

这是结果数据，以一个漂亮的降序表格呈现。

```
+-----------+------------------------+-----------+
| studentID | FullName               | sat_score |
+-----------+------------------------+-----------+
|         2 | Teri Gutierrez         |       800 |
|         3 | Spencer Pautier        |      1000 |
|         6 | Sophie Freeman         |      1200 |
|         9 | Raymond F. Boyce       |      2400 |
|         1 | Monique Davis          |       400 |
|         4 | Louis Ramsey           |      1200 |
|         7 | Edgar Frank "Ted" Codd |      2400 |
|         8 | Donald D. Chamberlin   |      2400 |
|         5 | Alvin Greene           |      1200 |
+-----------+------------------------+-----------+
9 rows in set (0.00 sec) 
```

这里是未排序的、当前的、完整的学生列表，以便与上面的进行比较。

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

与所有这些 SQL 内容一样，它们比本入门指南中的内容要多得多。

我希望这至少能给你足够的时间开始。

请参阅数据库管理器的手册，并亲自尝试不同的选项。