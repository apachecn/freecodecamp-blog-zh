# SQL 运算符教程–按位、比较、算术和逻辑运算符查询示例

> 原文：<https://www.freecodecamp.org/news/sql-operators-tutorial/>

从本质上讲，互联网及其所有应用都只是数据。

每一封电子邮件、每一条推文、每一张自拍照、每一笔银行交易等等，都只是存放在某个数据库中的数据。

为了使数据有用，我们必须能够检索它。然而，仅仅检索数据是不够的——数据*和*必须是有用的，并且与我们的情况相关。

在数据库级别，我们通过编写一个 **[SQL 查询](https://en.wikipedia.org/wiki/SQL)从数据库中请求特定的信息。**这个 SQL 查询指定了我们想要接收的数据*和*我们想要接收的格式。

在本文中，我们将研究过滤 SQL 查询的所有最常见的方法。

以下是我们将要介绍的内容:

*   [设置您的数据库](#setting-up-your-database)
*   [创建用户](#creating-users)
*   [插入用户](#inserting-users)
*   [用`WHERE`](#filtering-data-with-where) 过滤数据
*   [逻辑运算符(`AND` / `OR` / `NOT` )](#logical-operators-and-or-not-)
*   [比较运算符(`<`、`>`、`<=`、`>=` )](#comparison-operators-)
*   [算术运算符(`+`、`-`、`*`、`/`、`%` )](#arithmetic-operators-)
*   [存在算子(`IN` / `NOT IN` )](#existence-operators-in-not-in-)
*   [部分匹配使用`LIKE`](#partial-matching-using-like)
*   [处理缺失数据(`NULL` )](#dealing-with-missing-data-null-)
*   [使用`IS NULL`和`IS NOT NULL`和](#using-is-null-and-is-not-null)
*   [带有日期和时间的比较运算符](#comparison-operators-with-dates-and-times)
*   [存在使用`EXISTS`/`NOT EXISTS`/](#existence-using-exists-not-exists)
*   [按位运算符](#bitwise-operators)
*   [结论](#conclusion)

## 设置您的数据库

为了过滤我们的数据，我们首先必须有一些。

对于这些示例，我们将使用 PostgreSQL，但是这里显示的查询和概念将很容易转换到任何其他现代数据库系统(如 MySQL、SQL Server 等)。).

要使用我们的 PostgreSQL 数据库，我们可以使用 [`psql`](https://www.postgresql.org/docs/current/app-psql.html) —交互式 PostgreSQL 命令行程序。如果您有另一个喜欢使用的数据库客户机，那也很好！

首先，让我们创建数据库。PostgreSQL 已经安装了，我们可以在我们的终端运行`psql`命令`createdb <database-name>`来创建一个新的数据库。我叫我的`fcc`:

```
$ createdb fcc 
```

接下来，让我们使用命令`psql`启动交互控制台，并使用`\c <database-name>`连接到我们刚刚创建的数据库:

```
$ psql
psql (11.5)
Type "help" for help.

john=# \c fcc
You are now connected to database "fcc" as user "john".
fcc=# 
```

## 创建用户

现在我们有了一个数据库，让我们创建一个数据库表来为我们虚构的系统中的一个潜在用户建模。

我们称这个表为`users`，表中的每一行都代表我们的一个用户。

这个`users`表将包含我们期望用来描述用户的列——比如姓名、电子邮件和年龄。

在我们的`psql`会话中，让我们创建`users`表:

```
CREATE TABLE users(
  id SERIAL PRIMARY KEY,
  first_name TEXT NOT NULL,
  last_name TEXT NOT NULL,
  email TEXT NOT NULL,
  age INTEGER NOT NULL
);
```

输出显示`CREATE TABLE`，这意味着 are 表创建成功。

> **注意:**我已经清理了这些例子中的`psql`输出，以便于阅读，所以如果这里显示的输出与您在终端中看到的不完全一样，也不用担心。

让我们看看用户表的内容:

```
SELECT * FROM users;

 id | first_name | last_name | email | age
----+------------+-----------+-------+-----
(0 rows) 
```

我们还没有在表中插入任何数据，所以我们只看到空的表结构。

如果您不熟悉 SQL 查询，我们刚刚运行的查询`SELECT * FROM users`，是您可以编写的最简单的查询之一。

关键字`SELECT`指定要返回的列(`*`表示“所有列”)，关键字`FROM`指定要从哪个表中选择(在本例中为`users`)。

所以`SELECT * FROM users`实际上意味着*从`users`表中返回所有行和所有列。*

如果我们想要从`users`表中返回特定的列，我们可以用我们想要返回的列替换`SELECT *`—例如`SELECT id, name FROM users`。

## 插入用户

空表没什么意思，所以让我们在表中插入一些数据，这样我们就可以练习对它进行查询:

```
INSERT INTO users(first_name, last_name, email, age) VALUES
('John', 'Smith', 'johnsmith@gmail.com', 25),
('Jane', 'Doe', 'janedoe@Gmail.com', 28),
('Xavier', 'Wills', 'xavier@wills.io', 35),
('Bev', 'Scott', 'bev@bevscott.com', 16),
('Bree', 'Jensen', 'bjensen@corp.net', 42),
('John', 'Jacobs', 'jjacobs@corp.net', 56),
('Rick', 'Fuller', 'fullman@hotmail.com', 16);
```

如果我们在我们的`psql`会话中运行 insert 语句，我们会看到输出`INSERT 0 7`。这意味着我们已经成功地在表中插入了 7 个新行。

如果我们再次运行我们的`SELECT * FROM users`查询，我们现在将看到数据:

```
SELECT * FROM users;

id | first_name | last_name |        email        | age
----+------------+-----------+---------------------+-----
  1 | John       | Jacobs    | jjacobs@corp.net    |  56
  2 | Rick       | Fuller    | fullman@hotmail.com |  16
  3 | Bree       | Jensen    | bjensen@corp.net    |  42
  4 | Bev        | Scott     | bev@bevscott.com    |  16
  5 | Xavier     | Wills     | xavier@wills.io     |  35
  6 | Jane       | Doe       | janedoe@Gmail.com   |  28
  7 | John       | Smith     | johnsmith@gmail.com |  25
(7 rows) 
```

## 用`WHERE`过滤数据

到目前为止，我们已经返回了表中的所有行。这是查询的默认行为。为了返回一组更具选择性的行，我们需要使用一个`WHERE`子句**过滤这些行。**

使用`WHERE`子句过滤我们的行有很多种方法。我们可以使用的最简单的操作符是等式操作符:`=`。

假设我们想要查找名字为“John”的用户:

```
SELECT *
FROM users
WHERE first_name = 'John';

 id | first_name | last_name |        email        | age
----+------------+-----------+---------------------+-----
  1 | John       | Jacobs    | jjacobs@corp.net    |  56
  7 | John       | Smith     | johnsmith@gmail.com |  25
(2 rows) 
```

这里，我们将关键字`WHERE`附加到查询中，后跟一个等式语句:`first_name = 'John'`。

我们的数据库首先查看关键字`FROM`来决定获取什么数据。因此，数据库将读取这个查询，查看`FROM users`，并从磁盘中获取`users`表的所有行。

一旦从`users`表中检索到所有的行，它就对每一行运行`WHERE`子句，并且只返回`first_name`列值等于“约翰”的行

在我们的数据中，有两行与那个名字匹配。

如果我们想在系统中找到一个特定的“John ”,我们可以基于一个我们知道是唯一的列进行查询——比如我们的`id`列。

为了具体找到“John Jacobs”行，我们可以通过他的 ID 进行查询:

```
SELECT *
FROM users
WHERE id = 1;

 id | first_name | last_name |      email       | age
----+------------+-----------+------------------+-----
  1 | John       | Jacobs    | jjacobs@corp.net |  56
(1 row) 
```

这里只有一条记录符合`id = 1`的条件，所以我们只得到一行。

### 逻辑运算符(`AND` / `OR` / `NOT`)

除了等式运算符，我们还可以进行其他筛选。我们也可以使用大多数编程语言中都有的布尔逻辑运算符: *and，or，* and *not* 。

在许多编程语言中，*和*和*或*由`&&`和`||`表示。在 SQL 中，它们仅仅是`AND`和`OR`。

让我们尝试查找名为“John Smith”的人的记录，而不是按 ID 进行查询为此，我们可以在我们的`WHERE`子句中使用一个`AND`来查找名字和姓氏条件:

```
SELECT *
FROM users
WHERE first_name = 'John'
  AND last_name = 'Smith';

 id | first_name | last_name |        email        | age
----+------------+-----------+---------------------+-----
  7 | John       | Smith     | johnsmith@gmail.com |  25
(1 row) 
```

要查找名字为“John”*或*姓氏为“Doe”的人:

```
SELECT *
FROM users
WHERE first_name = 'John'
  OR last_name = 'Doe';

 id | first_name | last_name |        email        | age
----+------------+-----------+---------------------+-----
  1 | John       | Jacobs    | jjacobs@corp.net    |  56
  6 | Jane       | Doe       | janedoe@Gmail.com   |  28
  7 | John       | Smith     | johnsmith@gmail.com |  25
(3 rows) 
```

在这里，我们的结果包含了约翰和简·T2 以及无名氏。

这些`AND`和`OR`条件也可以链接在一起。假设我们想找一个名字完全是“约翰·史密斯”、“T2”或“T3”的人，他的姓是“多伊”:

```
SELECT *
FROM users
WHERE
(
  first_name = 'John'
  AND last_name = 'Smith'
)
OR last_name = 'Doe';

 id | first_name  | last_name |        email        | age
----+------------+-----------+---------------------+-----
  6 | Jane       | Doe       | janedoe@Gmail.com   |  28
  7 | John       | Smith     | johnsmith@gmail.com |  25
(2 rows) 
```

如果我们想颠倒这个条件，找到名为“John Smith”的*而不是*的用户，并且*而不是*的姓氏是“Doe ”,我们可以添加`NOT`运算符:

```
SELECT *
FROM users
WHERE NOT
(
  (
    first_name = 'John'
    AND last_name = 'Smith'
  )
  OR last_name = 'Doe'
);

 id | first_name | last_name |        email        | age
----+------------+-----------+---------------------+-----
  4 | Bev        | Scott     | bev@bevscott.com    |  16
  5 | Bree       | Jensen    | bjensen@corp.net    |  42
  6 | John       | Jacobs    | jjacobs@corp.net    |  56
  7 | Rick       | Fuller    | fullman@hotmail.com |  16
  3 | Xavier     | Wills     | xavier@wills.io     |  35
(5 rows)
```

> **注意:**每个人都有自己喜欢的格式化查询的个人风格——做任何对你有意义的事情！

### 比较运算符(`<`、`>`、`<=`、`>=`)

与其他编程语言类似，SQL 也有比较运算符:`<`、`>`、`<=`、`>=`。

让我们针对用户的`age`列练习使用这些操作符。

假设我们想找到 18 岁或以上的用户:

```
SELECT * FROM users WHERE age >= 18;

 id | first_name | last_name |        email        | age
----+------------+-----------+---------------------+-----
  1 | John       | Jacobs    | jjacobs@corp.net    |  56
  3 | Bree       | Jensen    | bjensen@corp.net    |  42
  5 | Xavier     | Wills     | xavier@wills.io     |  35
  6 | Jane       | Doe       | janedoe@Gmail.com   |  28
  7 | John       | Smith     | johnsmith@gmail.com |  25
(5 rows) 
```

年龄大于 25 岁但小于或等于 35 岁的用户怎么办？

```
SELECT * FROM users WHERE age > 25 AND age <= 35;

 id | first_name | last_name |       email       | age
----+------------+-----------+-------------------+-----
  5 | Xavier     | Wills     | xavier@wills.io   |  35
  6 | Jane       | Doe       | janedoe@Gmail.com |  28
(2 rows) 
```

### 算术运算符(`+`、`-`、`*`、`/`、`%`)

我们还可以对数据进行数学计算。

我们的`users`表有一个`age`列，如果我们想找到每个人年龄的一半*呢？*

```
SELECT
  *,
  age / 2 AS half_of_their_age
FROM users;

 id | first_name | last_name |        email        | age | half_of_their_age
----+------------+-----------+---------------------+-----+-------------------
  1 | John       | Jacobs    | jjacobs@corp.net    |  56 |                28
  2 | Rick       | Fuller    | fullman@hotmail.com |  16 |                 8
  3 | Bree       | Jensen    | bjensen@corp.net    |  42 |                21
  4 | Bev        | Scott     | bev@bevscott.com    |  16 |                 8
  5 | Xavier     | Wills     | xavier@wills.io     |  35 |                17
  6 | Jane       | Doe       | janedoe@Gmail.com   |  28 |                14
  7 | John       | Smith     | johnsmith@gmail.com |  25 |                12
(7 rows) 
```

这里我们选择了所有的表列(使用`SELECT *`)，并且我们还选择了一个新的聚合计算:`age / 2`。我们还使用关键字`AS`给这个值起了一个描述性的名字(`half_of_their_age`)和一个别名。

我们还可以通过使用模数或余数运算符(`%`)找到谁的年龄是一个*偶数*:

```
SELECT * FROM users WHERE (age % 2) = 0;

 id | first_name | last_name |        email        | age
----+------------+-----------+---------------------+-----
  1 | John       | Jacobs    | jjacobs@corp.net    |  56
  2 | Rick       | Fuller    | fullman@hotmail.com |  16
  3 | Bree       | Jensen    | bjensen@corp.net    |  42
  4 | Bev        | Scott     | bev@bevscott.com    |  16
  6 | Jane       | Doe       | janedoe@Gmail.com   |  28
(5 rows) 
```

我们可以通过使用`!=`或`<>`将我们的`=`条件改为“不等于”来找到谁的年龄是一个*奇数*:

```
SELECT * FROM users WHERE (age % 2) <> 0;

 id | first_name | last_name |        email        | age
----+------------+-----------+---------------------+-----
  5 | Xavier     | Wills     | xavier@wills.io     |  35
  7 | John       | Smith     | johnsmith@gmail.com |  25
(2 rows) 
```

### 存在运算符(`IN` / `NOT IN`)

如果我们想检查一个列值是否存在于一个值列表中，我们可以使用`IN`或`NOT IN`:

```
SELECT *
FROM users
WHERE first_name IN ('John', 'Jane', 'Rick');

 id | first_name | last_name |        email        | age
----+------------+-----------+---------------------+-----
  1 | John       | Smith     | johnsmith@gmail.com |  25
  2 | Jane       | Doe       | janedoe@Gmail.com   |  28
  6 | John       | Jacobs    | jjacobs@corp.net    |  56
  7 | Rick       | Fuller    | fullman@hotmail.com |  16
(4 rows) 
```

类似地，我们可以使用`NOT IN`来否定这个条件:

```
SELECT *
FROM users
WHERE first_name NOT IN ('John', 'Jane', 'Rick');

 id | first_name | last_name |      email       | age
----+------------+-----------+------------------+-----
  3 | Xavier     | Wills     | xavier@wills.io  |  35
  4 | Bev        | Scott     | bev@bevscott.com |  16
  5 | Bree       | Jensen    | bjensen@corp.net |  42
(3 rows) 
```

### 使用`LIKE`进行部分匹配

有时，我们可能希望基于部分搜索来搜索行。

比方说，我们想找到所有使用 Gmail 地址注册我们应用程序的用户。我们可以使用关键字`LIKE`对一列进行部分匹配。我们还可以使用`%`在匹配字符串中指定一个通配符(或“匹配任何东西”)。

要查找电子邮件以`gmail.com`结尾的用户:

```
SELECT *
FROM users
WHERE email LIKE '%gmail.com';

 id | first_name | last_name |        email        | age
----+------------+-----------+---------------------+-----
  1 | John       | Smith     | johnsmith@gmail.com |  25
(1 row) 
```

字符串`%gmail.com`表示“匹配以`gmail.com`结尾的任何内容。”

如果我们回头看看我们的用户数据，我们会注意到实际上有两个用户有一个`gmail.com`地址:

```
('John', 'Smith', 'johnsmith@gmail.com', 25),
('Jane', 'Doe', 'janedoe@Gmail.com', 28), 
```

然而，Jane 的电子邮件地址中有一个大写的“G”。或者前一个查询没有选择这个记录，因为它将*与小写字母“g”的`gmail.com`完全匹配*

为了进行不区分大小写的匹配，我们只需要用`LIKE`代替`ILIKE`:

```
SELECT *
FROM users
WHERE email ILIKE '%gmail.com';

 id | first_name | last_name |        email        | age
----+------------+-----------+---------------------+-----
  1 | John       | Smith     | johnsmith@gmail.com |  25
  2 | Jane       | Doe       | janedoe@Gmail.com   |  28
(2 rows) 
```

字符串开头的通配符符号`%`表示任何以“gmail.com”结尾的内容都将被返回。那可能是`bob.jones+12345@gmail.com`或`asdflkasdflkj@gmail.com`——只要它以`gmail.com`结尾。

我们还可以添加任意数量的通配符(`%`)。

例如，搜索词`%j%o%`将返回任何遵循以下模式的电子邮件:`<anything>`后接`j`，再接`<anything>`，再接`o`，再接`<anything>`:

```
SELECT * FROM users WHERE email ILIKE '%j%o%';

 id | first_name | last_name |        email        | age
----+------------+-----------+---------------------+-----
  1 | John       | Smith     | johnsmith@gmail.com |  25
  2 | Jane       | Doe       | janedoe@Gmail.com   |  28
  5 | Bree       | Jensen    | bjensen@corp.net    |  42
  6 | John       | Jacobs    | jjacobs@corp.net    |  56
(4 rows) 
```

## 处理缺失数据(`NULL`)

接下来，让我们看看如何处理带有缺失数据的列的行。

为此，让我们向我们的`users`表添加另一列:`first_paid_at`。

这个新列将是一个`TIMESTAMP`(类似于其他语言中的`datetime`)，它将表示用户为我们的应用程序支付费用的第一个日期和时间。也许我们想在使用我们的应用程序的周年纪念日给他们寄一张精美的卡片或一些鲜花？

我们可以使用`DROP TABLE users;`删除`users`表并重新创建它，但是这也会删除表中的所有数据。

要更改一个表而不删除它并丢失数据，我们可以使用`ALTER TABLE`:

```
ALTER TABLE users ADD COLUMN first_paid_at TIMESTAMP; 
```

该命令返回结果`ALTER TABLE`，因此我们的`ALTER`查询成功了。

如果我们现在查询我们的`users`表，我们会注意到这个新列中没有任何数据:

```
SELECT * FROM users;

 id | first_name | last_name |        email        | age | first_paid_at
----+------------+-----------+---------------------+-----+---------------
  1 | John       | Smith     | johnsmith@gmail.com |  25 |
  2 | Jane       | Doe       | janedoe@Gmail.com   |  28 |
  3 | Xavier     | Wills     | xavier@wills.io     |  35 |
  4 | Bev        | Scott     | bev@bevscott.com    |  16 |
  5 | Bree       | Jensen    | bjensen@corp.net    |  42 |
  6 | John       | Jacobs    | jjacobs@corp.net    |  56 |
  7 | Rick       | Fuller    | fullman@hotmail.com |  16 |
(7 rows) 
```

我们的`first_paid_at`列是空的，我们的`psql`查询的结果显示它是一个空列。从技术上来说，这个列不是空的——它包含一个特殊值，`psql`选择不在它的输出中显示这个值:`NULL`。

[`NULL`](https://en.wikipedia.org/wiki/Null_(SQL)) 是数据库中的特殊值。它是一种价值的缺失，它的行为并不像我们预期的那样。

为了说明这一点，让我们看看下面简单的`SELECT`语句:

```
SELECT
  1 = 1,
  1 = 2;

 ?column? | ?column?
----------+----------
 t        | f
(1 row) 
```

这里我们简单选择了`1 = 1`和`1 = 2`。正如我们所料，这两个语句的结果是`t`和`f`(或`TRUE`和`FALSE`)。`1`等于`1`，`1`不等于`2`。

现在让我们对`NULL`做同样的尝试:

```
SELECT 1 = NULL;

 ?column?
----------

(1 row) 
```

我们可能期望这个值是`FALSE`，但是返回值实际上是`NULL`。

为了更好地形象化这些`NULL`,让我们使用`\pset`选项来设置`psql`如何显示`NULL`值:

```
fcc=# \pset null 'NULL'
Null display is "NULL". 
```

现在，如果我们再次运行该查询，我们将看到我们期望的`NULL`输出:

```
SELECT 1 = NULL;

 ?column?
----------
 NULL
(1 row) 
```

所以`1`不等于`NULL`，那么`NULL = NULL`呢？

```
SELECT NULL = NULL;

 ?column?
----------
 NULL
(1 row) 
```

说来也怪，`NULL`不等于`NULL`。

将`NULL`视为未知值会有所帮助。未知值等于`1`吗？嗯，我们不知道——这是未知的。一个未知数等于一个未知数吗？还是那句话，未知。这样`NULL`就更有意义了。

### 使用`IS NULL`和`IS NOT NULL`

我们不能使用带有`NULL`的等号运算符，但是可以使用专门为它设计的两个运算符:`IS NULL`和`IS NOT NULL`。

```
SELECT
  NULL IS NULL,
  NULL IS NOT NULL;

 ?column? | ?column?
----------+----------
 t        | f
(1 row) 
```

这些值如预期的那样出来:`NULL IS NULL`为真，`NULL IS NOT NULL`为假。

这一切都很好，也很奇怪，但我们如何利用这一点呢？

首先，让我们在`first_paid_at`栏中获取一些数据:

```
UPDATE users SET first_paid_at = NOW() WHERE id = 1;
UPDATE 1

UPDATE users SET first_paid_at = (NOW() - INTERVAL '1 month') WHERE id = 2;
UPDATE 1

UPDATE users SET first_paid_at = (NOW() - INTERVAL '1 year') WHERE id = 3;
UPDATE 1 
```

在上面的那些`UPDATE`语句中，我们设置了三个不同的用户`first_paid_at`列:用户 ID 1 为当前时间(`NOW()`)，用户 ID 2 为一个月前，用户 ID 3 为一年前。

首先，让我们找出付费用户和未付费用户:

```
SELECT *
FROM users
WHERE first_paid_at IS NULL;

 id | first_name | last_name |        email        | age | first_paid_at
----+------------+-----------+---------------------+-----+---------------
  4 | Bev        | Scott     | bev@bevscott.com    |  16 | NULL
  5 | Bree       | Jensen    | bjensen@corp.net    |  42 | NULL
  6 | John       | Jacobs    | jjacobs@corp.net    |  56 | NULL
  7 | Rick       | Fuller    | fullman@hotmail.com |  16 | NULL
(4 rows)

SELECT *
FROM users
WHERE first_paid_at IS NOT NULL;

 id | first_name | last_name |        email        | age |       first_paid_at
----+------------+-----------+---------------------+-----+----------------------------
  1 | John       | Smith     | johnsmith@gmail.com |  25 | 2020-08-11 20:49:17.230517
  2 | Jane       | Doe       | janedoe@Gmail.com   |  28 | 2020-07-11 20:49:17.233124
  3 | Xavier     | Wills     | xavier@wills.io     |  35 | 2019-08-11 20:49:17.23488
(3 rows) 
```

### 带有日期和时间的比较运算符

现在我们有了一些数据，让我们对这个新的`TIMESTAMP`字段使用相同的比较操作符。

让我们试着找到在过去一周内第一次向我们付费的用户。为此，我们可以使用关键字`INTERVAL`将当前时间`NOW()`减去一周:

```
SELECT *
FROM users
WHERE first_paid_at > (NOW() - INTERVAL '1 week');

 id | first_name | last_name |        email        | age |       first_paid_at
----+------------+-----------+---------------------+-----+----------------------------
  1 | John       | Smith     | johnsmith@gmail.com |  25 | 2020-08-11 20:49:17.230517
(1 row) 
```

我们也可以使用不同的时间间隔，例如三个月前:

```
SELECT *
FROM users
WHERE first_paid_at < (NOW() - INTERVAL '3 months');

 id | first_name | last_name |      email      | age |       first_paid_at
----+------------+-----------+-----------------+-----+---------------------------
  3 | Xavier     | Wills     | xavier@wills.io |  35 | 2019-08-11 20:49:17.23488
(1 row) 
```

让我们试着找到一至六个月前第一次向我们付费的用户。

我们可以使用`AND`再次组合我们的条件，但是不要使用小于的*和大于*的*操作符，让我们使用`BETWEEN`关键字:*

```
SELECT *
FROM users
WHERE first_paid_at BETWEEN (NOW() - INTERVAL '6 month')
  AND (NOW() - INTERVAL '1 month');

 id | first_name | last_name |       email       | age |       first_paid_at
----+------------+-----------+-------------------+-----+----------------------------
  2 | Jane       | Doe       | janedoe@Gmail.com |  28 | 2020-07-11 20:49:17.233124
(1 row) 
```

### 存在使用`EXISTS` / `NOT EXISTS`

另一种检查存在的方法是使用`EXISTS`和`NOT EXISTS`。

这些运算符通过检查条件是否存在来筛选出行。这种情况通常是对另一个表的查询。

为了进行设置，让我们创建一个名为`posts`的新表。该表将包含用户可以在我们的系统中发布的帖子。

```
CREATE TABLE posts(
  id SERIAL PRIMARY KEY,
  body TEXT NOT NULL,
  user_id INTEGER REFERENCES users NOT NULL
); 
```

这是一张简单的桌子。它只包含一个 ID、一个存储文章文本的字段(`body`)和一个对写文章的用户的引用(`user_id`)。

让我们在这个新表中插入一些数据:

```
INSERT INTO posts(body, user_id) VALUES
('Here is post 1', 1),
('Here is post 2', 1),
('Here is post 3', 2),
('Here is post 4', 3); 
```

在我们插入到`posts`表中的数据中，用户 ID 1 有两篇文章，用户 ID 2 有一篇文章，用户 ID 3 也有一篇文章。

要找到有帖子的用户，我们可以使用`EXISTS`。

`EXISTS`关键字采用子查询。如果该子查询返回了*任何东西*(甚至是只有值`NULL`的行)，数据库将在结果集中包含该行。

[来自于`EXISTS`的 PostgreSQL 文档](https://www.postgresql.org/docs/current/functions-subquery.html#FUNCTIONS-SUBQUERY-EXISTS):

> EXISTS 的参数是一个任意的 SELECT 语句或子查询。评估子查询以确定它是否返回任何行。如果它至少返回一行，EXISTS 的结果为“true”；如果子查询不返回任何行，EXISTS 的结果为“false”。

`EXISTS`只是从子查询中寻找某一行的*存在性*——这与其中的内容无关。

下面是一个使用`EXISTS`发帖的用户示例:

```
SELECT *
FROM users
WHERE EXISTS (
  SELECT 1
  FROM posts
  WHERE posts.user_id = users.id
);

 id | first_name | last_name |        email        | age |       first_paid_at
----+------------+-----------+---------------------+-----+----------------------------
  1 | John       | Smith     | johnsmith@gmail.com |  25 | 2020-08-11 20:49:17.230517
  2 | Jane       | Doe       | janedoe@Gmail.com   |  28 | 2020-07-11 20:49:17.233124
  3 | Xavier     | Wills     | xavier@wills.io     |  35 | 2019-08-11 20:49:17.23488
(3 rows) 
```

正如我们所料，我们得到了用户 1、2 和 3。

我们的`EXISTS`子查询正在检查帖子的`user_id`与`users`表上的`id`列相匹配的`posts`记录。我们在我们的`SELECT`中返回了`1`,因为我们可以在这里返回任何东西——数据库只是想看到一些东西确实被返回了。

类似地，我们可以通过将`EXISTS`更改为`NOT EXISTS`来找到没有任何帖子的用户:

```
SELECT *
FROM users
WHERE NOT EXISTS (
  SELECT 1
  FROM posts
  WHERE posts.user_id = users.id
);

 id | first_name | last_name |        email        | age | first_paid_at
----+------------+-----------+---------------------+-----+---------------
  4 | Bev        | Scott     | bev@bevscott.com    |  16 | NULL
  5 | Bree       | Jensen    | bjensen@corp.net    |  42 | NULL
  6 | John       | Jacobs    | jjacobs@corp.net    |  56 | NULL
  7 | Rick       | Fuller    | fullman@hotmail.com |  16 | NULL
(4 rows) 
```

最后，我们也可以重新编写这个查询，使用`IN`或`NOT IN`来代替`EXISTS`或`NOT EXISTS`，就像这样:

```
SELECT *
FROM users
WHERE users.id IN (
  SELECT user_id
  FROM posts
); 
```

这在技术上是可行的，但是一般来说，如果你正在测试另一个记录的*存在*，那么使用`EXISTS`通常会*更有性能。`IN`和`NOT IN`操作符通常更适用于根据静态列表检查值，就像我们之前做的那样:*

```
SELECT *
FROM users
WHERE first_name IN ('John', 'Jane', 'Rick'); 
```

### 按位运算符

尽管在实践中不经常使用按位运算符，但为了完整起见，让我们看一个简单的例子。

如果我们想(出于某种原因)以二进制形式查看用户的年龄，并翻转这些位，我们可以使用各种位运算符。

作为一个例子，我们来看看按位“与”运算符:`&`。

```
SELECT age::bit(8) & '11111111' FROM users;

 ?column?
----------
 00010000
 00101010
 00111000
 00010000
 00011001
 00011100
 00100011
(7 rows)
```

为了执行按位计算，我们首先必须将我们的`age`列从整数转换为二进制——在本例中，我们使用`::bit(8)`将其转换为八位二进制字符串。

接下来我们可以用另一个二进制字符串`11111111`以二进制格式“与”我们时代的结果。由于二进制码`AND`只有在两位都是 1 的情况下才返回 1，所以这个全是 1 的字符串保持了输出的趣味性。

几乎所有其他按位运算符都使用相同的格式:

```
SELECT age::bit(8) | '11111111' FROM users;    -- bitwise OR
SELECT age::bit(8) # '11111111' FROM users;    -- bitwise XOR
SELECT age::bit(8) << '00000001' FROM users;   -- bitwise shift left
SELECT age::bit(8) >> '00000001' FROM users;   -- bitwise shift right
```

按位“非”运算符(`~`)略有不同，它应用于单个术语，类似于常规的`NOT`运算符:

```
SELECT ~age::bit(8) FROM users;

 ?column?
----------
 11101111
 11010101
 11000111
 11101111
 11100110
 11100011
 11011100
(7 rows)
```

最后，最有用的按位运算符:串联。

该运算符的一个常见用途是将文本字符串组合在一起。例如，如果我们想为用户构建一个“全名”的计算属性，我们可以使用串联:

```
SELECT first_name || ' ' || last_name AS name
FROM users;

     name
--------------
 Bev Scott
 Bree Jensen
 John Jacobs
 Rick Fuller
 John Smith
 Jane Doe
 Xavier Wills
(7 rows)
```

这里我们连接(或“组合”)了`first_name`、一个空格(`' '`)和`last_name`属性来构建一个`name`值。

## 结论

这基本上是您需要使用的每个查询过滤操作符的概述！

还有一些操作符我们没有在这里介绍，但是这些操作符要么不经常使用，要么使用方式与上面完全相同——所以它们不会给你带来任何麻烦。

如果你喜欢这篇文章，我在我的博客上也写了类似的东西。

感谢阅读！

约翰