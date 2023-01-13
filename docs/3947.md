# 基本 SQL 命令——您应该知道的数据库查询和语句列表

> 原文：<https://www.freecodecamp.org/news/basic-sql-commands/>

SQL 代表结构化查询语言。SQL 命令是用于与数据库通信以执行任务、功能和数据查询的指令。

SQL 命令可用于搜索数据库和执行其他功能，如创建表、向表中添加数据、修改数据和删除表。

如果您打算使用 SQL，下面是您应该知道的基本 SQL 命令(有时称为子句)的列表。

### **从**中选择 and

查询的`SELECT`部分决定在结果中显示哪些数据列。还可以应用一些选项来显示不是表列的数据。

下面的例子显示了“学生”表的三列`SELECT` ed `FROM`和一个计算列。数据库存储学生的 studentID、名字和姓氏。我们可以将名字和姓氏列组合起来创建全名计算列。

```
SELECT studentID, FirstName, LastName, FirstName + ' ' + LastName AS FullName
FROM student;
```

```
+-----------+-------------------+------------+------------------------+
| studentID | FirstName         | LastName   | FullName               |
+-----------+-------------------+------------+------------------------+
|         1 | Monique           | Davis      | Monique Davis          |
|         2 | Teri              | Gutierrez  | Teri Gutierrez         |
|         3 | Spencer           | Pautier    | Spencer Pautier        |
|         4 | Louis             | Ramsey     | Louis Ramsey           |
|         5 | Alvin             | Greene     | Alvin Greene           |
|         6 | Sophie            | Freeman    | Sophie Freeman         |
|         7 | Edgar Frank "Ted" | Codd       | Edgar Frank "Ted" Codd |
|         8 | Donald D.         | Chamberlin | Donald D. Chamberlin   |
|         9 | Raymond F.        | Boyce      | Raymond F. Boyce       |
+-----------+-------------------+------------+------------------------+
9 rows in set (0.00 sec)
```

### **创建表格**

就像它听起来那样:它在数据库中创建一个表。您可以指定表的名称以及表中应该包含的列。

```
CREATE TABLE table_name (
    column_1 datatype,
    column_2 datatype,
    column_3 datatype
);
```

### 改变表格

[`ALTER TABLE`](https://dev.mysql.com/doc/refman/5.7/en/alter-table.html) 改变表格的结构。以下是将列添加到数据库的方法:

```
ALTER TABLE table_name
ADD column_name datatype;
```

### 支票

`CHECK`约束用于限制一列中可以放置的数值范围。

如果您在单个列上定义了一个`CHECK`约束，它只允许该列的某些值。如果在表上定义了一个`CHECK`约束，它可以根据行中其他列的值来限制某些列中的值。

当创建“Persons”表时，下面的 SQL 在“Age”列上创建了一个`CHECK`约束。`CHECK`约束确保您不能有任何 18 岁以下的人。

```
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CHECK (Age>=18)
);
```

要允许命名一个`CHECK`约束，并在多个列上定义一个`CHECK`约束，请使用以下 SQL 语法:

```
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    City varchar(255),
    CONSTRAINT CHK_Person CHECK (Age>=18 AND City='Sandnes')
);
```

### 在哪里

******(**** `AND`，`OR` ****，`IN`，`BETWEEN`，`LIKE` )******

`WHERE`子句用于限制返回的行数。

作为一个例子，首先我们将向您展示一个`SELECT`语句和结果*，没有*和`WHERE`语句。然后，我们将添加一个使用上述所有五个限定符的`WHERE`语句。

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

现在，我们将重复`SELECT`查询，但是我们将使用`WHERE`语句限制返回的行。

```
STUDENT studentID, FullName, sat_score, recordUpdated
FROM student
WHERE (studentID BETWEEN 1 AND 5 OR studentID = 8)
        AND
        sat_score NOT IN (1000, 1400);
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

### 更新

要更新表中的记录，可以使用`UPDATE`语句。

使用`WHERE`条件指定您想要更新的记录。一次可以更新一列或多列。语法是:

```
UPDATE table_name
SET column1 = value1, 
    column2 = value2, ...
WHERE condition;
```

下面是一个更新 Id 为 4 的记录名称的示例:

```
UPDATE Person
SET Name = “Elton John”
WHERE Id = 4;
```

还可以通过使用其他表中的值来更新表中的列。使用`JOIN`子句从多个表中获取数据。语法是:

```
UPDATE table_name1
SET table_name1.column1 = table_name2.columnA
    table_name1.column2 = table_name2.columnB
FROM table_name1
JOIN table_name2 ON table_name1.ForeignKey = table_name2.Key
```

以下是更新所有记录的经理的示例:

```
UPDATE Person
SET Person.Manager = Department.Manager
FROM Person
JOIN Department ON Person.DepartmentID = Department.ID
```

### **分组依据**

`GROUP BY`允许您合并行和聚合数据。

下面是`GROUP BY`的语法:

```
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name;
```

### 拥有

`HAVING`允许您过滤由`GROUP BY`子句聚集的数据，以便用户可以查看有限的记录集。

下面是`HAVING`的语法:

```
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name
HAVING COUNT(*) > value;
```

### AVG()

“Average”用于从 SQL 语句返回的行集中计算数值列的平均值。

下面是使用该函数的语法:

```
SELECT groupingField, AVG(num_field)
FROM table1
GROUP BY groupingField
```

下面是一个使用学生表的示例:

```
SELECT studentID, FullName, AVG(sat_score) 
FROM student 
GROUP BY studentID, FullName;
```

### 如同

`AS`允许您使用别名重命名列或表。

```
SELECT user_only_num1 AS AgeOfServer, (user_only_num1 - warranty_period) AS NonWarrantyPeriod FROM server_table
```

这会产生如下输出。

```
+-------------+------------------------+
| AgeOfServer | NonWarrantyPeriod      | 
+-------------+------------------------+
|         36  |                     24 |
|         24  |                     12 | 
|         61  |                     49 |
|         12  |                      0 | 
|          6  |                     -6 |
|          0  |                    -12 | 
|         36  |                     24 |
|         36  |                     24 | 
|         24  |                     12 | 
+-------------+------------------------+
```

还可以使用 AS 为表指定一个名称，以便在联接中更容易引用。

```
SELECT ord.product, ord.ord_number, ord.price, cust.cust_name, cust.cust_number FROM customer_table AS cust

JOIN order_table AS ord ON cust.cust_number = ord.cust_number
```

这会产生如下输出。

```
+-------------+------------+-----------+-----------------+--------------+
| product     | ord_number | price     | cust_name       | cust_number  |
+-------------+------------+-----------+-----------------+--------------+
|     RAM     |   12345    |       124 | John Smith      |  20          |
|     CPU     |   12346    |       212 | Mia X           |  22          |
|     USB     |   12347    |        49 | Elise Beth      |  21          |
|     Cable   |   12348    |         0 | Paul Fort       |  19          |
|     Mouse   |   12349    |        66 | Nats Back       |  15          |
|     Laptop  |   12350    |       612 | Mel S           |  36          |
|     Keyboard|   12351    |        24 | George Z        |  95          |
|     Keyboard|   12352    |        24 | Ally B          |  55          |
|     Air     |   12353    |        12 | Maria Trust     |  11          |
+-------------+------------+-----------+-----------------+--------------+
```

### 以...排序

`ORDER BY`为我们提供了一种按照`SELECT`部分中的一个或多个条目对结果集进行排序的方法。下面是一个 SQL 语句，按照全名降序对学生进行排序。默认的排序顺序是升序(`ASC`)，但是要以相反的顺序(降序)排序，您可以使用`DESC`。

```
SELECT studentID, FullName, sat_score
FROM student
ORDER BY FullName DESC;
```

### 数数

`COUNT`将计算行数，并将该计数作为结果集中的一列返回。

以下是您可以使用 COUNT 的示例:

*   对表中的所有行进行计数(不需要 group by)
*   计算数据子集的总数(需要语句的 Group By 部分)

此 SQL 语句提供所有行的计数。请注意，您可以使用“AS”为结果计数列命名。

```
SELECT count(*) AS studentCount FROM student; 
```

### 删除

`DELETE`用于删除表中的一条记录。

小心点。您可以删除表中的所有记录，也可以只删除几条记录。使用`WHERE`条件指定您想要删除的记录。语法是:

```
DELETE FROM table_name
WHERE condition;
```

下面是一个从 Person 表中删除 Id 为 3 的记录的示例:

```
DELETE FROM Person
WHERE Id = 3;
```

### 内部连接

`JOIN`也称为内部联接，选择两个表中具有匹配值的记录。

```
SELECT * FROM A x JOIN B y ON y.aId = x.Id
```

### **左连接**

A `LEFT JOIN`返回左表中的所有行，以及右表中匹配的行。即使右表中没有匹配项，也将返回左表中的行。左表中与右表中不匹配的行将拥有右表值的`null`。

```
SELECT * FROM A x LEFT JOIN B y ON y.aId = x.Id
```

### **右连接**

A `RIGHT JOIN`返回右表中的所有行，以及左表中匹配的行。与左连接相反，这将返回右表中的所有行，即使左表中没有匹配项。对于左表列，右表中与左表不匹配的行将具有`null`值。

```
SELECT * FROM A x RIGHT JOIN B y ON y.aId = x.Id 
```

### **完全外部连接**

A `FULL OUTER JOIN`返回在任一表中匹配的所有行。因此，如果左表中有与右表中不匹配的行，这些行将被包括在内。此外，如果右表中的行在左表中没有匹配项，这些行将被包括在内。

```
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
FULL OUTER JOIN Orders
ON Customers.CustomerID=Orders.CustomerID
ORDER BY Customers.CustomerName
```

### 插入

`INSERT`是一种将数据插入表格的方法。

```
INSERT INTO table_name (column_1, column_2, column_3) 
VALUES (value_1, 'value_2', value_3);
```

### 喜欢

`LIKE`在`WHERE`或`HAVING`(作为`GROUP BY`的一部分)中使用，当一列中包含某种字符模式时，将所选行限制为项目。

该 SQL 将选择以“Monique”开头或以“Greene”结尾的`FullName`学生。

```
SELECT studentID, FullName, sat_score, rcd_updated
FROM student 
WHERE 
    FullName LIKE 'Monique%' OR 
    FullName LIKE '%Greene'; 
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

您可以将`NOT`放在`LIKE`之前，以排除具有字符串模式的行，而不是选择它们。此 SQL 排除了全名列中包含“cer Pau”和“Ted”的记录。

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