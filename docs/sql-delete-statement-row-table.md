# SQL Delete 语句-如何删除一行或一个表，用语法例子解释

> 原文：<https://www.freecodecamp.org/news/sql-delete-statement-row-table/>

要删除表中的记录，可以使用`DELETE`语句。

小心点。您可以删除表中的所有记录，也可以只删除几条记录。使用`WHERE`条件指定您想要删除哪些记录。语法是:

```
DELETE FROM table_name
WHERE condition; 
```

下面是一个从 Person 表中删除 Id 为 3 的记录的示例:

```
DELETE FROM Person
WHERE Id = 3; 
```

使用 DELETE 从给定的表中删除所有记录

```
DELETE * FROM Person
; 
```

或者根据您的 RDBMS，您可以使用 TRUNCATE TABLE 语句从表中删除所有记录，并且根据您的 RDBMS，可能允许也可能不允许回滚。删除是 DML，截断是 DDL。

```
TRUNCATE TABLE Person; 
```