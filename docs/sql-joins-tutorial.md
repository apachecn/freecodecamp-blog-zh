# SQL 联接教程:交叉联接、完全外联接、内联接、左联接和右联接。

> 原文：<https://www.freecodecamp.org/news/sql-joins-tutorial/>

SQL 连接允许我们的关系数据库管理系统成为关系型的。

连接允许我们将分离的数据库表重新构建成支持应用程序的关系。

在本文中，我们将研究 SQL 中每种不同的连接类型以及如何使用它们。

以下是我们将要介绍的内容:

*   什么是联接？
*   [设置您的数据库](#setting-up-your-database)
*   `[CROSS JOIN](#cross-join)`
*   [设置我们的示例数据(导演和电影)](#creating-directors-and-movies)
*   `[FULL OUTER JOIN](#full-outer-join)`
*   `[INNER JOIN](#inner-join)`
*   [`LEFT JOIN`/`RIGHT JOIN`](#left-join-right-join)
*   [使用`LEFT JOIN`过滤](#filtering-using-left-join)
*   [多重连接](#multiple-joins)
*   [带附加条件的连接](#joins-with-extra-conditions)
*   [用连接编写查询的现实](#the-reality-about-writing-queries-with-joins)

(*剧透提示*:我们将涵盖五种不同的类型——但你真的只需要知道其中的两种！)

## 什么是联接？

**连接**是将两行组合成一行的操作。

这些行通常来自两个不同的表，但也不是必须如此。

在我们研究如何编写连接本身之前，让我们先看看连接的结果是什么样的。

让我们以一个存储用户及其地址信息的系统为例。

存储用户信息的表中的行可能如下所示:

```
 id |     name     |        email        | age
----+--------------+---------------------+-----
  1 | John Smith   | johnsmith@gmail.com |  25
  2 | Jane Doe     | janedoe@Gmail.com   |  28
  3 | Xavier Wills | xavier@wills.io     |  3
...
(7 rows)
```

存储地址信息的表中的行可能如下所示:

```
 id |      street       |     city      | state | user_id
----+-------------------+---------------+-------+---------
  1 | 1234 Main Street  | Oklahoma City | OK    |       1
  2 | 4444 Broadway Ave | Oklahoma City | OK    |       2
  3 | 5678 Party Ln     | Tulsa         | OK    |       3
(3 rows)
```

我们可以编写单独的查询来检索用户信息和地址信息——但是理想情况下，我们可以编写*一个查询*,并在同一个结果集中接收所有用户及其地址。

这正是加入让我们做到的！

我们很快就会看到如何编写这些连接，但是如果我们将用户信息连接到地址信息，我们可能会得到如下结果:

```
 id |     name     |        email        | age | id |      street       |     city      | state | user_id
----+--------------+---------------------+-----+----+-------------------+---------------+-------+---------
  1 | John Smith   | johnsmith@gmail.com |  25 |  1 | 1234 Main Street  | Oklahoma City | OK    |       1
  2 | Jane Doe     | janedoe@Gmail.com   |  28 |  2 | 4444 Broadway Ave | Oklahoma City | OK    |       2
  3 | Xavier Wills | xavier@wills.io     |  35 |  3 | 5678 Party Ln     | Tulsa         | OK    |       3
(3 rows) 
```

在这里，我们在一个漂亮的结果集中看到了所有用户及其地址。

除了生成组合结果集，连接的另一个重要用途是将额外的信息拉入我们的查询中，以便我们进行过滤。

例如，如果我们想给居住在俄克拉荷马城的所有用户发送一些物理邮件，我们可以使用这个连接在一起的结果集，并基于`city`列进行过滤。

现在我们知道了连接的目的——让我们开始写一些吧！

## 设置您的数据库

在编写查询之前，我们需要设置数据库。

对于这些示例，我们将使用 PostgreSQL，但是这里显示的查询和概念将很容易转换到任何其他现代数据库系统(如 MySQL、SQL Server 等)。).

要使用我们的 PostgreSQL 数据库，我们可以使用[`psql`](https://www.postgresql.org/docs/current/app-psql.html)—交互式 PostgreSQL 命令行程序。如果您有另一个喜欢使用的数据库客户机，那也很好。

首先，让我们创建数据库。PostgreSQL 已经安装了，我们可以在终端运行命令`createdb <database-name>`来创建一个新的数据库。我叫我的`fcc`:

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

> **注意:**我已经清理了这些例子中的`psql`输出，以便于阅读，所以如果这里显示的输出与您在终端中看到的不完全一样，也不用担心。

我鼓励你跟随这些例子，自己运行这些查询。通过阅读这些例子，你会学到更多，记住更多。

现在开始连接！

## `CROSS JOIN`

我们能做的最简单的连接是`CROSS JOIN`或*“笛卡尔乘积”*

该联接从一个表中取出每一行，并将其与另一个表中的每一行联接起来。

如果我们有两个列表——一个包含`1, 2, 3`,另一个包含`A, B, C`——这两个列表的笛卡尔积应该是这样的:

```
1A, 1B, 1C
2A, 2B, 2C
3A, 3B, 3C 
```

第一个列表中的每个值都与第二个列表中的每个值成对出现。

让我们把这个例子写成一个 SQL 查询。

首先，让我们创建两个非常简单的表，并在其中插入一些数据:

```
CREATE TABLE letters(
  letter TEXT
);

INSERT INTO letters(letter) VALUES ('A'), ('B'), ('C');

CREATE TABLE numbers(
  number TEXT
);

INSERT INTO numbers(number) VALUES (1), (2), (3); 
```

我们的两个表，`letters`和`numbers`，只有一列:一个简单的文本字段。

现在让我们用一个`CROSS JOIN`把它们连在一起:

```
SELECT *
FROM letters
CROSS JOIN numbers; 
```

```
 letter | number
--------+--------
 A      | 1
 A      | 2
 A      | 3
 B      | 1
 B      | 2
 B      | 3
 C      | 1
 C      | 2
 C      | 3
(9 rows) 
```

这是我们能做的最简单的连接类型——但是即使在这个简单的例子中，我们也能看到连接在起作用:两个独立的行(一个来自`letters`一个来自`numbers`)被*连接到*一起形成一行。

虽然这种类型的连接经常被当作纯粹的学术例子来讨论，但它至少有一个很好的用例:覆盖日期范围。

### `CROSS JOIN`带日期范围

`CROSS JOIN`的一个很好的用例是从表中取出每一行，并将其应用于日期范围内的每一天。

例如，假设您正在构建一个跟踪日常任务的应用程序，比如刷牙、吃早餐或洗澡。

如果您想为过去一周的每项任务和每一天生成一条记录*，那么您可以针对一个日期范围使用`CROSS JOIN`。*

要设定这个日期范围，我们可以使用`[generate_series](https://www.postgresql.org/docs/current/functions-srf.html)`函数:

```
SELECT generate_series(
  (CURRENT_DATE - INTERVAL '5 day'),
  CURRENT_DATE,
  INTERVAL '1 day'
)::DATE AS day; 
```

`generate_series`函数有三个参数。

第一个参数是起始值。在这个例子中，我们使用`CURRENT_DATE - INTERVAL '5 day'`。这将返回当前日期减去五天，即“五天前”

第二个参数是当前日期(`CURRENT_DATE`)。

第三个参数是“步长间隔”，即我们希望每次值增加多少。由于这些是日常任务，我们将使用一天的间隔(`INTERVAL '1 day'`)。

将所有这些放在一起，这会生成一系列日期，从五天前开始，到今天结束，一次一天。

最后，我们通过使用`::DATE`将这些值的输出转换为一个日期来移除时间部分，并且我们使用`AS day`来别名该列以使输出更好一些。

该查询的输出是过去五天加上今天:

```
 day
------------
 2020-08-19
 2020-08-20
 2020-08-21
 2020-08-22
 2020-08-23
 2020-08-24
(6 rows) 
```

回到我们的每日任务示例，让我们创建一个简单的表来保存我们想要完成的任务，并插入几个任务:

```
CREATE TABLE tasks(
  name TEXT
);

INSERT INTO tasks(name) VALUES
('Brush teeth'),
('Eat breakfast'),
('Shower'),
('Get dressed'); 
```

我们的`tasks`表只有一列`name`，我们在这个表中插入了四个任务。

现在让我们用查询来生成日期:

```
SELECT
  tasks.name,
  dates.day
FROM tasks
CROSS JOIN
(
  SELECT generate_series(
    (CURRENT_DATE - INTERVAL '5 day'),
    CURRENT_DATE,
    INTERVAL '1 day'
  )::DATE	AS day
) AS dates 
```

(因为我们的日期生成查询不是一个实际的表，所以我们只是将其编写为一个子查询。)

从这个查询中，我们返回任务名称和日期，结果集如下所示:

```
 name      |    day
---------------+------------
 Brush teeth   | 2020-08-19
 Brush teeth   | 2020-08-20
 Brush teeth   | 2020-08-21
 Brush teeth   | 2020-08-22
 Brush teeth   | 2020-08-23
 Brush teeth   | 2020-08-24
 Eat breakfast | 2020-08-19
 Eat breakfast | 2020-08-20
 Eat breakfast | 2020-08-21
 Eat breakfast | 2020-08-22
 ...
 (24 rows) 
```

正如我们所期望的，在我们的日期范围内，每天的每个任务都有一行。

`CROSS JOIN`是我们能做的最简单的连接，但是为了看接下来的几个类型，我们需要一个更真实的表设置。

## 创作导演和电影

为了说明下面的连接类型，我们将使用*电影*和*电影导演的例子。*

在这种情况下，一部电影有一个导演，但一部电影并不需要有一个导演——想象一下一部新电影宣布上映，但导演的人选尚未确定。

我们的`directors`表将存储每个导演的名字，而`movies`表将存储电影的名字以及对电影导演的引用(如果有的话)。

让我们创建这两个表，并在其中插入一些数据:

```
CREATE TABLE directors(
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL
);

INSERT INTO directors(name) VALUES
('John Smith'),
('Jane Doe'),
('Xavier Wills')
('Bev Scott'),
('Bree Jensen');

CREATE TABLE movies(
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  director_id INTEGER REFERENCES directors 
);

INSERT INTO movies(name, director_id) VALUES
('Movie 1', 1),
('Movie 2', 1),
('Movie 3', 2),
('Movie 4', NULL),
('Movie 5', NULL); 
```

我们有五个导演，五部电影，其中三部电影都有指定的导演。导演 ID 1 有两部电影，导演 ID 2 有一部。

## `FULL OUTER JOIN`

现在我们有了一些数据，让我们看看`FULL OUTER JOIN`。

一个`FULL OUTER JOIN`和一个`CROSS JOIN`有一些相似之处，但是它有一些关键的不同。

第一个区别是`FULL OUTER JOIN`需要一个**连接条件。**

联接条件指定两个表之间的行如何相互关联，以及根据什么标准将它们联接在一起。

在我们的示例中，我们的`movies`表通过`director_id`列引用了控制器，该列与`directors`表的`id`列相匹配。这是我们将用作连接条件的两列。

下面是我们如何编写两个表之间的连接:

```
SELECT *
FROM movies
FULL OUTER JOIN directors
  ON directors.id = movies.director_id; 
```

注意我们指定的将电影与其导演匹配的连接条件:`ON movies.director_id = directors.id`。

我们的结果集看起来像某种奇怪的笛卡尔乘积:

```
 id  |  name   | director_id |  id  |     name
------+---------+-------------+------+--------------
    1 | Movie 1 |           1 |    1 | John Smith
    2 | Movie 2 |           1 |    1 | John Smith
    3 | Movie 3 |           2 |    2 | Jane Doe
    4 | Movie 4 |        NULL | NULL | NULL
    5 | Movie 5 |        NULL | NULL | NULL
 NULL | NULL    |        NULL |    5 | Bree Jensen
 NULL | NULL    |        NULL |    4 | Bev Scott
 NULL | NULL    |        NULL |    3 | Xavier Wills
(8 rows) 
```

我们看到的第一行是电影有导演的行，我们的连接条件被评估为 true。

然而，在这些行之后，我们看到来自每个表的剩余的每一行*——但是具有另一个表不匹配的`NULL`值。*

> **注意:**如果你不熟悉`NULL`的值，[请看我在这里的解释](https://www.freecodecamp.org/news/sql-operators-tutorial/#dealing-with-missing-data-null-)在这个 SQL 操作符教程里。

我们在这里还看到了`CROSS JOIN`和`FULL OUTER JOIN`之间的另一个区别。一个`FULL OUTER JOIN`从每个表中返回一个不同的行——不像`CROSS JOIN`有多个。

## `INNER JOIN`

下一个连接类型`INNER JOIN`，是最常用的连接类型之一。

内部连接**只返回连接条件为真的行。**

在我们的例子中，我们的`movies`和`directors`表之间的内部连接将只返回电影被指定了导演的记录。

语法基本上和以前一样:

```
SELECT *
FROM movies
INNER JOIN directors
  ON directors.id = movies.director_id; 
```

我们的结果显示了三部有导演的电影:

```
 id |  name   | director_id | id |    name
----+---------+-------------+----+------------
  1 | Movie 1 |           1 |  1 | John Smith
  2 | Movie 2 |           1 |  1 | John Smith
  3 | Movie 3 |           2 |  2 | Jane Doe
(3 rows) 
```

因为内部连接只包括符合连接条件的行，所以连接中两个表的*顺序无关紧要。*

如果我们颠倒查询中表的顺序，我们会得到相同的结果:

```
SELECT *
FROM directors
INNER JOIN movies
  ON movies.director_id = directors.id; 
```

```
 id |    name    | id |  name   | director_id
----+------------+----+---------+-------------
  1 | John Smith |  1 | Movie 1 |           1
  1 | John Smith |  2 | Movie 2 |           1
  2 | Jane Doe   |  3 | Movie 3 |           2
(3 rows) 
```

因为我们在这个查询中首先列出了`directors`表，并且我们选择了所有列(`SELECT *`)，所以我们首先看到了`directors`列的数据，然后是来自`movies`的列——但是结果数据是相同的。

这是内部连接的一个有用的属性，但是它并不适用于所有的连接类型——比如我们的下一个类型。

## `LEFT JOIN` / `RIGHT JOIN`

接下来的两个连接类型使用了一个修饰符(`LEFT`或`RIGHT`)，它影响结果集中包含的表数据。

> **注:**`LEFT JOIN`和`RIGHT JOIN`也可称为`LEFT OUTER JOIN`和`RIGHT OUTER JOIN`。

这些连接用在查询中，我们希望返回某个特定表的所有数据，*如果它存在的话*，以及相关表的数据。

如果关联的数据不存在，我们仍然可以得到所有“主”表的数据。

这是对关于特定事物的信息和奖励信息(如果该奖励信息存在)的查询。

这用一个例子就简单理解了。让我们找到所有的电影和他们的导演，但我们不关心他们是否有导演——这是一个奖金:

```
SELECT *
FROM movies
LEFT JOIN directors
  ON directors.id = movies.director_id; 
```

该查询遵循与之前相同的模式——我们只是将连接指定为一个`LEFT JOIN`。

在本例中，`movies`表是“左”表。

如果我们将查询写在一行上，会更容易看到:

```
... FROM movies LEFT JOIN directors ... 
```

左连接返回“左”表中的所有记录。

左连接从“右”表**中返回任何符合连接条件的行。**

“右”表中与连接条件不匹配的行被返回为`NULL`。

```
 id |  name   | director_id |  id  |    name
----+---------+-------------+------+------------
  1 | Movie 1 |           1 |    1 | John Smith
  2 | Movie 2 |           1 |    1 | John Smith
  3 | Movie 3 |           2 |    2 | Jane Doe
  4 | Movie 4 |        NULL | NULL | NULL
  5 | Movie 5 |        NULL | NULL | NULL
(5 rows) 
```

查看该结果集，我们可以看到为什么这种类型的连接对于所有这些查询以及某些类型的查询是有用的。

### `RIGHT JOIN`

`RIGHT JOIN`的工作方式与`LEFT JOIN`完全一样——除了两个表的规则是相反的。

在右连接中，返回“右”表中的所有行。基于连接条件有条件地返回“左”表。

让我们使用与上面相同的查询，但是用`LEFT JOIN`代替`RIGHT JOIN`:

```
SELECT *
FROM movies
RIGHT JOIN directors
  ON directors.id = movies.director_id; 
```

```
 id  |  name   | director_id | id |     name
------+---------+-------------+----+--------------
    1 | Movie 1 |           1 |  1 | John Smith
    2 | Movie 2 |           1 |  1 | John Smith
    3 | Movie 3 |           2 |  2 | Jane Doe
 NULL | NULL    |        NULL |  5 | Bree Jensen
 NULL | NULL    |        NULL |  4 | Bev Scott
 NULL | NULL    |        NULL |  3 | Xavier Wills
(6 rows) 
```

我们的结果集现在返回每一个`directors`行，如果存在的话，还返回`movies`数据。

我们所做的只是切换我们认为是“主”表的表——我们希望看到所有数据的表，不管它的相关数据是否存在。

### `LEFT JOIN` / `RIGHT JOIN`在生产应用中

在生产应用程序中，我只使用过`LEFT JOIN`而从不使用`RIGHT JOIN`。

我这样做是因为，在我看来，`LEFT JOIN`使查询更容易阅读和理解。

当我编写查询时，我喜欢从一个“基础”结果集开始，比如说所有的电影，然后从这个基础中引入(或减去)一组事物。

因为我喜欢从基础开始，`LEFT JOIN`符合这种思路。我想要我的基表(“左”表)中的所有行，并且有条件地想要“右”表中的行。

实际上，我想我甚至从未在生产应用程序中见过`RIGHT JOIN`。一个`RIGHT JOIN`没有错——我只是觉得它让查询更难理解。

### 重写`RIGHT JOIN`

如果我们想翻转上面的场景，转而返回所有导演并有条件地返回他们的电影，我们可以很容易地将`RIGHT JOIN`重写为`LEFT JOIN`。

我们需要做的就是颠倒查询中表的顺序，将`RIGHT`改为`LEFT`:

```
SELECT *
FROM directors
LEFT JOIN movies
  ON movies.director_id = directors.id; 
```

> **注意:**我喜欢将被连接的表(上例中的“右”表`movies`)放在连接条件(`ON movies.director_id = ...`)的最前面，但这只是我个人的偏好。

## 使用`LEFT JOIN`过滤

使用`LEFT JOIN`(或`RIGHT JOIN`)有两种情况。

我们已经讨论过的第一个用例:返回一个表中的所有行，并有条件地返回另一个表中的所有行。

第二个用例是返回第一个表**中的行，而第二个表中的数据不存在。**

场景应该是这样的:找到不属于某部电影的导演。

为此，我们将从一个`LEFT JOIN`开始，我们的`directors`表将是主表或“左”表:

```
SELECT *
FROM directors
LEFT JOIN movies
  ON movies.director_id = directors.id; 
```

对于不属于电影的导演，`movies`表中的列是`NULL`:

```
 id |     name     |  id  |  name   | director_id
----+--------------+------+---------+-------------
  1 | John Smith   |    1 | Movie 1 |           1
  1 | John Smith   |    2 | Movie 2 |           1
  2 | Jane Doe     |    3 | Movie 3 |           2
  5 | Bree Jensen  | NULL | NULL    |        NULL
  4 | Bev Scott    | NULL | NULL    |        NULL
  3 | Xavier Wills | NULL | NULL    |        NULL
(6 rows) 
```

在我们的示例中，导演 ID 3、4 和 5 不属于电影。

为了将我们的结果集过滤到这些行，我们可以添加一个`WHERE`子句，只返回电影数据为`NULL`的行:

```
SELECT *
FROM directors
LEFT JOIN movies
  ON movies.director_id = directors.id
WHERE movies.id IS NULL; 
```

```
 id |     name     |  id  | name | director_id
----+--------------+------+------+-------------
  5 | Bree Jensen  | NULL | NULL |        NULL
  4 | Bev Scott    | NULL | NULL |        NULL
  3 | Xavier Wills | NULL | NULL |        NULL
(3 rows) 
```

还有我们三个没拍电影的导演！

通常使用表的`id`列来过滤(`WHERE movies.id IS NULL`)，但是来自`movies`表的所有列都是`NULL`——所以它们中的任何一个都可以工作。

(因为我们知道`movies`表中的所有列都将是`NULL`，所以在上面的查询中，我们可以只写`SELECT directors.*`而不是`SELECT *`来返回所有主管的信息。)

### 使用`LEFT JOIN`查找匹配项

在我们之前的查询中，我们发现*不属于*电影的导演。

使用我们相同的结构，我们可以通过改变我们的`WHERE`条件来查找电影数据是*而不是* `NULL`的行，从而找到*属于*的导演:

```
SELECT *
FROM directors
LEFT JOIN movies
  ON movies.director_id = directors.id
WHERE movies.id IS NOT NULL; 
```

```
 id |    name    | id |  name   | director_id
----+------------+----+---------+-------------
  1 | John Smith |  1 | Movie 1 |           1
  1 | John Smith |  2 | Movie 2 |           1
  2 | Jane Doe   |  3 | Movie 3 |           2
(3 rows) 
```

这看起来很方便，但是我们实际上只是重新实现了`INNER JOIN`！

## 多重连接

我们已经看到了如何将两个表连接在一起，但是如何在一行中进行多个连接呢？

这实际上很简单，但是为了说明这一点，我们需要第三表:`tickets`。

此表将代表一部电影的售出门票:

```
CREATE TABLE tickets(
  id SERIAL PRIMARY KEY,
  movie_id INTEGER REFERENCES movies NOT NULL
);

INSERT INTO tickets(movie_id) VALUES (1), (1), (3); 
```

`tickets`表只有一个`id`和一个对电影的引用:`movie_id`。

我们还插入了两张为电影 ID 1 售出的门票，以及一张为电影 ID 3 售出的门票。

现在，让我们将`directors`连接到`movies`——然后将`movies`连接到`tickets`！

```
SELECT *
FROM directors
INNER JOIN movies
  ON movies.director_id = directors.id
INNER JOIN tickets
  ON tickets.movie_id = movies.id; 
```

因为这些是内部连接，所以我们编写连接的顺序并不重要。我们可以从`tickets`开始，然后在`movies`加入，然后在`directors`加入。

这又一次归结到你试图查询什么，以及什么使查询最容易理解。

在我们的结果集中，我们会注意到我们进一步缩小了返回的行:

```
 id |    name    | id |  name   | director_id | id | movie_id
----+------------+----+---------+-------------+----+----------
  1 | John Smith |  1 | Movie 1 |           1 |  1 |        1
  1 | John Smith |  1 | Movie 1 |           1 |  2 |        1
  2 | Jane Doe   |  3 | Movie 3 |           2 |  3 |        3
(3 rows) 
```

这是有意义的，因为我们已经添加了另一个`INNER JOIN`。实际上，这为我们的查询添加了另一个*和*条件。

我们的查询实际上是:*“返回所有属于也有票房收入的电影**的导演。”***

相反，如果我们想找到属于电影*的导演，而这些电影可能还没有售票*，我们可以用最后一个`INNER JOIN`代替一个`LEFT JOIN`:

```
SELECT *
FROM directors
JOIN movies
  ON movies.director_id = directors.id
LEFT JOIN tickets
  ON tickets.movie_id = movies.id; 
```

我们可以看到`Movie 2`现在回到了结果集中:

```
 id |    name    | id |  name   | director_id |  id  | movie_id
----+------------+----+---------+-------------+------+----------
  1 | John Smith |  1 | Movie 1 |           1 |    1 |        1
  1 | John Smith |  1 | Movie 1 |           1 |    2 |        1
  2 | Jane Doe   |  3 | Movie 3 |           2 |    3 |        3
  1 | John Smith |  2 | Movie 2 |           1 | NULL |     NULL
(4 rows) 
```

这部电影没有任何门票销售，因此它之前由于`INNER JOIN`而被排除在结果集之外。

我将把这个问题留给读者来做*练习，但是你如何找到那些属于**的电影却没有任何票房收入的导演呢？***

### 加入执行顺序

最后，我们并不真正关心连接的执行顺序。

SQL 和其他现代编程语言的一个关键区别是，SQL 是一种**声明性**语言。

这意味着我们指定了我们想要的结果，但是我们没有指定执行细节——这些细节留给了数据库查询规划器。我们指定我们想要的连接和它们的条件，查询计划器处理剩下的事情。

但是，实际上，数据库并不是同时将三个表连接在一起。相反，它可能会将前两个表连接在一起成为一个中间结果，然后将该中间结果集连接到第三个表。

(**注:**这是一个有些简化的解释。)

因此，当我们在查询中处理多个连接时，我们可以把它们看作是两个表之间的一系列连接——尽管其中一个表可能会变得很大。

## 带有附加条件的联接

我们要讨论的最后一个主题是带有额外条件的连接。

类似于一个`WHERE`子句，我们可以向我们的连接条件添加任意多的条件。

例如，如果我们想找到导演是*而不是* *并被命名为* *【约翰·史密斯】*的电影，我们可以用`AND`将额外的条件添加到我们的连接中:

```
SELECT *
FROM movies
INNER JOIN directors
  ON directors.id = movies.director_id
  AND directors.name <> 'John Smith';
```

我们可以在这个连接条件的`WHERE`子句中使用任何操作符。

如果我们将条件放在一个`WHERE`子句中，我们也会从这个查询中得到相同的结果:

```
SELECT *
FROM movies
INNER JOIN directors
  ON directors.id = movies.director_id
WHERE directors.name <> 'John Smith';
```

这里有一些细微的差别，但是对于本文来说，结果集是相同的。

(如果您不熟悉过滤 SQL 查询的所有方法，请在这里查看前面提到的文章。)

## 用连接编写查询的现实

实际上，我发现自己只以三种不同的方式使用联接:

#### `INNER JOIN`

第一个用例是两个表**之间存在**关系的记录。这由`INNER JOIN`完成。

这些情况类似于寻找“*有导演的电影”*或*“有帖子的用户”。*

#### `LEFT JOIN`

第二个用例是一个表中的记录——如果存在关系，则是另一个表中的记录。这由`LEFT JOIN`完成。

这些情况类似于*“有导演的电影，如果他们有”*或者*“有帖子的用户，如果他们有一些帖子。”*

#### `LEFT JOIN`排除

第三个最常见的用例是我们对 a `LEFT JOIN`的第二个用例:在一个表中查找与第二个表中的****没有关系的记录。****

****这些情况类似于*“没有导演的电影”*或者*“没有帖子的用户”*****

### ****两种非常有用的连接类型****

****我不认为我曾经在生产应用程序中使用过`FULL OUTER JOIN`或`RIGHT JOIN`。用例出现得不够频繁，或者查询可以用一种更清晰的方式编写(在`RIGHT JOIN`的例子中)。****

****我偶尔会使用`CROSS JOIN`来处理跨日期范围传播记录之类的事情(就像我们一开始看到的那样)，但是这种场景也不经常出现。****

****好消息。对于您将遇到的 99.9%的用例，实际上只需要理解两种类型的连接:`INNER JOIN`和`LEFT JOIN`！****

****如果你喜欢这篇文章，你可以[在 twitter](https://twitter.com/johnmosesman) 上关注我，在那里我谈论数据库的事情和所有其他与开发相关的话题。****

****感谢阅读！****

****约翰****

******附言**阅读到最后的额外提示:大多数数据库系统会让你只写`JOIN`来代替`INNER JOIN`——这会节省你一点额外的打字时间。:)****