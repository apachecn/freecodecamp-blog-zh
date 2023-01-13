# 常见的 SQL 面试问题:您的数据库备忘单

> 原文：<https://www.freecodecamp.org/news/sql-interview-questions/>

以下是一些在工作面试中最常被问到的 SQL 问题。

通过了解这些，你将为即将到来的技术面试做更好的准备。

### 什么是 SQL 中的内部连接？

如果没有指定联接，这是默认的联接类型。它返回两个表中至少有一个匹配项的所有行。

```
SELECT * FROM A x JOIN B y ON y.aId = x.Id 
```

### 什么是 SQL 中的左连接？

左连接返回左表中的所有行，以及右表中的匹配行。即使右表中没有匹配项，也将返回左表中的行。左表中与右表中不匹配的行将具有右表值的`null`。

```
SELECT * FROM A x LEFT JOIN B y ON y.aId = x.Id 
```

### 什么是 SQL 中的右连接？

右连接返回右表中的所有行，以及左表中的匹配行。与左连接相反，这将返回右表中的所有行，即使左表中没有匹配项。右表中与左表不匹配的行将具有左表列的`null`值。

```
SELECT * FROM A x RIGHT JOIN B y ON y.aId = x.Id 
```

### 什么是 SQL 中的完全连接？

完全联接返回在任一表中匹配的所有行。因此，如果左表中有与右表中不匹配的行，这些行将被包括在内。如果右表中的行与左表中的行不匹配，那么这些行将被包括在内。

```
SELECT Customers.CustomerName, Orders.OrderID
FROM Customers
FULL OUTER JOIN Orders
ON Customers.CustomerID=Orders.CustomerID
ORDER BY Customers.CustomerName 
```

### 以下命令的结果是什么？

```
 DROP VIEW view_name 
```

这将是一个错误，因为我们不能在视图上执行 DML 操作。

### 我们可以在使用 ALTER 命令后执行回滚吗？

不会，因为 ALTER 是一个 DDL 命令，而 Oracle server 会在执行 DDL 语句时执行自动提交。

### 在列级别实施规则的唯一约束是什么？

NOT NULL 是在列级别起作用的唯一约束。

### SQL 中有哪些伪列？举几个例子？

伪列是返回系统生成值的函数。之所以这样说，是因为伪列是 Oracle 分配的值，与 Oracle 数据库列在同一上下文中使用，但不存储在磁盘上。

```
 ROWNUM, ROWID, USER, CURRVAL, NEXTVAL etc. 
```

### 使用密码 kmd26pt 创建用户 my723acct。使用 PO8 提供的用户*数据和临时数据表空间，在 user* data 中为该用户提供 10M 的存储空间，在 temporary_data 中提供 5M 的存储空间。

```
 CREATE USER my723acct IDENTIFIED BY kmd26pt
    DEFAULT TABLESPACE user_data
    TEMPORARY TABLESPACE temporary_data
    QUOTA 10M on user_data QUOTA 5M on temporary_data 
```

### 创建角色 role *表*和 _ 视图。

```
 CREATE ROLE role_tables_and_views 
```

### 授予前一个问题的角色连接数据库的权限和创建表和视图的权限。

连接到数据库的权限是创建会话创建表的权限是创建表创建视图的权限是创建视图

```
 GRANT Create session, create table, create view TO role_tables_and_views 
```

### 将问题中的前一个角色授予用户 anny 和 rita

```
 GRANT role_tables_and_views TO anny, rita 
```

### 使用密码 kmd26pt 创建用户 my723acct。使用 PO8 提供的用户*数据和临时数据表空间，在 user* data 中为该用户提供 10M 的存储空间，在 temporary_data 中提供 5M 的存储空间。

```
 CREATE USER my723acct IDENTIFIED BY kmd26pt
    DEFAULT TABLESPACE user_data
    TEMPORARY TABLESPACE temporary_data
    QUOTA 10M on user_data QUOTA 5M on temporary_data 
```

### 创建角色 role *表*和 _ 视图。

```
 CREATE ROLE role_tables_and_views 
```

### 授予前一个问题的角色连接数据库的权限和创建表和视图的权限。

连接到数据库的权限是创建会话创建表的权限是创建表创建视图的权限是创建视图

```
 GRANT Create session, create table, create view TO role_tables_and_views 
```

### 将问题中的前一个角色授予用户 anny 和 rita。

```
 GRANT role_tables_and_views TO anny, rita 
```

### 编写一个命令，将用户 rita 的密码从 abcd 更改为 dfgh。

```
 ALTER USER rita IDENTIFIED BY dfgh 
```

用户 rita 和 anny 对 SCOTT 创建的表 INVENTORY 没有 SELECT 权限。编写一个命令，允许 SCOTT 授予用户对这些表的 SELECT 权限。

```
 GRANT select ON inventory TO rita, anny 
```

用户 rita 已经被转移，不再需要通过角色表和 _ 视图授予她的权限。编写一个命令，将她从之前给定的特权中删除，但她仍然可以连接到数据库。

```
 REVOKE select ON scott.inventory FROM rita
    REVOKE create table, create view FROM rita 
```

被转移的用户 rita 现在正转到另一家公司。因为她创建的对象不再有用，所以编写一个命令来删除这个用户和她的所有对象。

这里 CASCADE 选项是删除数据库中用户的所有对象所必需的。

```
 DROP USER rita CASCADE

### User rita has been transferred and no longer needs the privilege that was granted to her through the role role_tables_and_views. Write a command to remove her from her previous given priviliges except that she still could connect to the database.
``` sql    
    REVOKE select ON scott.inventory FROM rita
    REVOKE create table, create view FROM rita 
```

被转移的用户 rita 现在正转到另一家公司。因为她创建的对象不再有用，所以编写一个命令来删除这个用户和她的所有对象。

这里 CASCADE 选项是删除数据库中用户的所有对象所必需的。

```
 DROP USER rita CASCADE 
```

### 编写 SQL 查询，从表中查找第 n 份最高工资。

```
 SELECT TOP 1 Salary
   FROM (
      SELECT DISTINCT TOP N Salary
      FROM Employee
      ORDER BY Salary DESC
      )
    ORDER BY Salary ASC 
```