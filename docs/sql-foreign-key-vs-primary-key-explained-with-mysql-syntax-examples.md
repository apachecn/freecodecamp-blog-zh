# 用 MySQL 语法示例解释 SQL 外键与主键

> 原文：<https://www.freecodecamp.org/news/sql-foreign-key-vs-primary-key-explained-with-mysql-syntax-examples/>

外键是用于链接两个表的键。具有外键约束的表(也称为“子表”)连接到另一个表(也称为“父表”)。该连接位于子表的外键约束和父表的主键之间。

外键约束用于帮助维护表之间的一致性。例如，如果父表记录被删除，而子表有记录，系统也可以删除子记录。

通过要求在子表中输入的每个记录都有一个父表记录，它们还有助于防止在子表中输入不准确的数据。

### 使用示例

在本指南中，我们将仔细查看学生(家长)和学生联系人(孩子)表。

### 父表的主键

注意，student 表有一个 studentID 的一列主键。

```
SHOW index FROM student; 
```

```
+---------+------------+----------+--------------+-------------+
| Table   | Non_unique | Key_name | Seq_in_index | Column_name |
+---------+------------+----------+--------------+-------------+
| student |          0 | PRIMARY  |            1 | studentID   |
+---------+------------+----------+--------------+-------------+
1 row in set (0.00 sec) (some columns removed on the right for clarity) 
```

### 子表的主键和外键

学生联系信息表有一个主键，也是学生 ID。这是因为两个表之间存在一对一的关系。换句话说，我们期望每个学生只有一个学生和一个学生联系记录。

```
SHOW index FROM `student-contact-info`; 
```

```
+----------------------+------------+----------+--------------+-------------+
| Table                | Non_unique | Key_name | Seq_in_index | Column_name |
+----------------------+------------+----------+--------------+-------------+
| student-contact-info |          0 | PRIMARY  |            1 | studentID   |
+----------------------+------------+----------+--------------+-------------+
1 row in set (0.00 sec) (some columns removed on the right for clarity) 
```

```
SELECT concat(table_name, '.', column_name) AS 'foreign key',
concat(referenced_table_name, '.', referenced_column_name) AS 'references'
FROM information_schema.key_column_usage
WHERE referenced_table_name IS NOT NULL
AND table_schema = 'fcc_sql_guides_database' 
AND table_name = 'student-contact-info'; 
```

```
+--------------------------------+-------------------+
| foreign key                    | references        |
+--------------------------------+-------------------+
| student-contact-info.studentID | student.studentID |
+--------------------------------+-------------------+
1 row in set (0.00 sec) 
```

### 使用学生父表和联系人子表的示例报表

```
SELECT a.studentID, a.FullName, a.programOfStudy,
b.`student-phone-cell`, b.`student-US-zipcode`
FROM student AS a
JOIN `student-contact-info` AS b ON a.studentID = b.studentID; 
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
+-----------+------------------------+------------------+--------------------+--------------------+ 
```

### 结论

外键约束是一个很好的数据完整性工具。花时间好好学习它们。

与所有这些 SQL 内容一样，它们比本入门指南中的内容要多得多。

我希望这至少能给你足够的时间开始。

请参阅数据库管理器的手册，并亲自尝试不同的选项。