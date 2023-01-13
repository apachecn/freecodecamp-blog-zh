# SQL 视图:Replace View 语句的语法示例

> 原文：<https://www.freecodecamp.org/news/the-sql-replace-view-statement-example-syntax/>

视图是在一个或多个表中显示数据的数据库对象。用于创建视图的相同 SQL 语句也可用于替换现有视图。

本指南将更新(替换)现有的视图“programming-students-v ”,使用一个稍微不同的名称。

安全提示:在对模式进行更改之前，请务必备份模式。

### 通用系统

```
CREATE OR REPLACE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition; 
```

### 用于创建视图和当前数据的 SQL

```
create view `programming-students-v` as
select FullName, programOfStudy 
from student 
where programOfStudy = 'Programming'; 
```

```
select * from `programming-students-v`; 
```

当前数据:

```
+-----------------+----------------+
| FullName        | programOfStudy |
+-----------------+----------------+
| Teri Gutierrez  | Programming    |
| Spencer Pautier | Programming    |
| Louis Ramsey    | Programming    |
| Alvin Greene    | Programming    |
| Sophie Freeman  | Programming    |
+-----------------+----------------+
5 rows in set (0.00 sec) 
```

现有视图的列表:

```
SHOW FULL TABLES IN fcc_sql_guides_database WHERE TABLE_TYPE LIKE 'VIEW'; 
```

```
+-----------------------------------+------------+
| Tables_in_fcc_sql_guides_database | Table_type |
+-----------------------------------+------------+
| programming-students-v            | VIEW       |
| students-contact-info_v           | VIEW       |
| students_dropme_v                 | VIEW       |
+-----------------------------------+------------+
3 rows in set (0.00 sec) 
```

### 替换视图

```
create or replace view `programming-students-v` as
select FullName, programOfStudy, sat_score 
from student 
where programOfStudy = 'Programming'; 
```

```
select * from `programming-students-v`; 
```

注意:视图现在显示了 sat_score。

```
+-----------------+----------------+-----------+
| FullName        | programOfStudy | sat_score |
+-----------------+----------------+-----------+
| Teri Gutierrez  | Programming    |       800 |
| Spencer Pautier | Programming    |      1000 |
| Louis Ramsey    | Programming    |      1200 |
| Alvin Greene    | Programming    |      1200 |
| Sophie Freeman  | Programming    |      1200 |
+-----------------+----------------+-----------+ 
```

注意:视图列表没有改变，我们的视图被替换了。

```
mysql>  SHOW FULL TABLES IN fcc_sql_guides_database WHERE TABLE_TYPE LIKE 'VIEW';
+-----------------------------------+------------+
| Tables_in_fcc_sql_guides_database | Table_type |
+-----------------------------------+------------+
| programming-students-v            | VIEW       |
| students-contact-info_v           | VIEW       |
| students_dropme_v                 | VIEW       |
+-----------------------------------+------------+
3 rows in set (0.00 sec) 
```

与所有这些 SQL 内容一样，它们比本入门指南中的内容要多得多。

我希望这至少能给你足够的时间开始。请参阅数据库管理器的手册，并亲自尝试不同的选项。