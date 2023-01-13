# SQL Insert Into 和 Insert 语句:示例 MySQL 语法

> 原文：<https://www.freecodecamp.org/news/sql-insert-and-insert-into-statements-with-example-syntax/>

本文将带您了解如何在 SQL 中使用 Insert 和 Insert Into 语句。

## 如何在 SQL 中使用 Insert

插入查询是向表中插入数据的一种方式。假设我们已经使用创建了一个表

`CREATE TABLE example_table ( name varchar(255), age int)`

**示例 _ 表格**

姓名年龄

现在，为了向该表添加一些数据，我们将以如下方式使用 **INSERT** :

`INSERT INTO example_table (column1,column2) VALUES ("Andrew",23)`

**示例 _ 表格**

NameAgeAndrew23

甚至下面的方法也可以，但是指定哪个数据进入哪个列总是一个好的做法。

`INSERT INTO table_name VALUES ("John", 28)`

**示例 _ 表格**

NameAgeAndrew23John28

## 如何在 SQL 中使用 Insert Into

要在表中插入记录，可以使用`INSERT INTO`语句。

有两种方法可以做到这一点，如果你想只在某些列中插入值，你必须列出它们的名称，包括所有的强制列。语法是:

```
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...); 
```

另一种方法是向表中的所有列插入值，不需要指定列名。语法是:

```
INSERT INTO table_name 
VALUES (value1, value2, value3, ...); 
```

下面是一个以两种方式在表 Person 中插入记录的示例:

```
INSERT INTO Person
VALUES (1, ‘John Lennon’, ‘1940-10-09’, ‘M’); 
```

和

```
INSERT INTO Person(Id, Name, DateOfBirth, Gender)
VALUES (1, ‘John Lennon’, ‘1940-10-09’, ‘M’); 
```

一些 SQL 版本(例如 MySQL)支持一次插入多行。例如:

```
INSERT INTO Person(Id, Name, DateOfBirth, Gender)
VALUES (1, ‘John Lennon’, ‘1940-10-09’, ‘M’), (2, ‘Paul McCartney’, ‘1942-06-18’, ‘M’),
(3, ‘George Harrison’, ‘1943-02-25’, ‘M’), (4, ‘Ringo Starr’, ‘1940-07-07’, ‘M’) 
```

注意，整个原始查询保持不变——我们只是添加了用括号括起来并用逗号分隔的数据行。

## 您甚至可以在 Select 语句中使用 Insert Into。

您可以使用已经存储在数据库中的数据在表中插入记录。这只是数据的副本，并不影响源表。

`INSERT INTO SELECT`语句结合了`INSERT INTO`和`SELECT`语句，您可以使用任何想要的条件。语法是:

```
INSERT INTO table2 (column1, column2, column3, ...)
SELECT column1, column2, column3, ...
FROM table1
WHERE condition; 
```

这里有一个例子，在表 Person 中插入表 students 中的所有男学生。

```
INSERT INTO Person(Id, Name, DateOfBirth, Gender)
SELECT Id, Name, DateOfBirth, Gender
FROM Students
WHERE Gender = ‘M’ 
```