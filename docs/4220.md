# SQL 中的检查约束——用 MySQL 和 SQL Server 语法示例解释

> 原文：<https://www.freecodecamp.org/news/check-constraint-sql-server-mysql/>

CHECK 约束用于限制可放置在列中的值范围。

如果在单个列上定义 CHECK 约束，则该列只允许某些值。

如果在表上定义 CHECK 约束，它可以根据行中其他列的值来限制某些列中的值。

### 创建表时的 SQL 检查

当创建“Persons”表时，下面的 SQL 在“Age”列上创建一个检查约束。检查约束确保您不能有任何 18 岁以下的人:

**MySQL:**

```
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    CHECK (Age>=18)
); 
```

**SQL Server / Oracle / MS 访问:**

```
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int CHECK (Age>=18)
); 
```

若要允许命名 CHECK 约束，并在多个列上定义 CHECK 约束，请使用以下 SQL 语法:

**MySQL / SQL Server / Oracle / MS 访问:**

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

### ALTER TABLE 上的 SQL 检查

要在已经创建表的情况下对“Age”列创建检查约束，请使用以下 SQL:

**MySQL / SQL Server / Oracle / MS 访问:**

```
ALTER TABLE Persons
ADD CHECK (Age>=18); 
```

若要允许命名 CHECK 约束，并在多个列上定义 CHECK 约束，请使用以下 SQL 语法:

**MySQL / SQL Server / Oracle / MS 访问:**

```
ALTER TABLE Persons
ADD CONSTRAINT CHK_PersonAge CHECK (Age>=18 AND City='Sandnes'); 
```

### 删除检查约束

要删除 CHECK 约束，请使用以下 SQL:

**SQL Server / Oracle / MS 访问:**

```
ALTER TABLE Persons
DROP CONSTRAINT CHK_PersonAge; 
```

**MySQL:**

```
ALTER TABLE Persons
DROP CHECK CHK_PersonAge; 
```