# SQL Union 和 Union All 运算符用语法示例解释

> 原文：<https://www.freecodecamp.org/news/sql-union-and-union-all-operators-explained-with-syntax-examples/>

在本指南中，我们将讨论 SQL 语句的 UNION 运算符部分。

UNION 运算符用于将多个 select 语句的结果组合成一个结果集。

SQL 语句的 Select 语句中必须有相同数量的列。

### 基本示例

SQL 语句

```
SELECT 'aaaaa'
UNION
SELECT 'bbbbbbbbb'; 
```

输出

```
+-----------+
| aaaaa     |
+-----------+
| aaaaa     |
| bbbbbbbbb |
+-----------+
2 rows in set (0.00 sec) 
```

### 使用学生表的示例

SQL 语句

```
SELECT StudentID, FullName FROM student WHERE studentID BETWEEN 1 AND 5
UNION
SELECT studentID, studentEmailAddr FROM `student-contact-info` WHERE studentID BETWEEN 7 AND 8; 
```

输出

```
+-----------+--------------------------------+
| StudentID | FullName                       |
+-----------+--------------------------------+
|         1 | Monique Davis                  |
|         2 | Teri Gutierrez                 |
|         3 | Spencer Pautier                |
|         4 | Louis Ramsey                   |
|         5 | Alvin Greene                   |
|         7 | Maximo.Smith@freeCodeCamp.org  |
|         8 | Michael.Roach@freeCodeCamp.ort |
+-----------+--------------------------------+
7 rows in set (0.00 sec) 
```

## SQL UNION ALL 运算符

UNION ALL 操作符是 UNION 操作符的扩展，它应该在输出中产生 A+B 行，但假设 A 和 B 是您的输入，简单地说，UNION ALL 不会进行重复数据删除。

### 基本语法

SQL 语句

```
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions]
UNION ALL
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions]; 
```

与所有这些 SQL 内容一样，它们比本入门指南中的内容要多得多。

我希望这至少能给你足够的时间开始。

请参阅数据库管理器的手册，并亲自尝试不同的选项。