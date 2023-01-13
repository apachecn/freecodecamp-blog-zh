# SQL 更新语句–用 SQL 更新查询

> 原文：<https://www.freecodecamp.org/news/sql-update-statement-update-query-in-sql/>

一旦在数据库中创建了一个表，就很少需要永远保持不变。您可能需要修改其中的记录。

为了帮助您做到这一点，有一个非常有用的语句，名为`UPDATE`，您可以根据需要使用它来更改记录。

注意:如果这里给出的语法不起作用，请查阅您正在使用的 SQL 实现的文档。大多数东西都是一样的，但是有一些不同。

# SQL 更新语法

要使用`UPDATE`方法，首先要确定需要用`UPDATE table_name`更新哪个表。之后，你用`SET`语句写下你想对记录做什么样的改变。最后，您使用 [a `WHERE`子句](https://www.freecodecamp.org/news/sql-where-clause-examples/)来选择要更改的记录。

**使用那个`WHERE`子句**真的很重要，否则你要对整个表格做同样的修改。

```
UPDATE table_name
SET change to make
WHERE clause to select which records to change;
```

# SQL 更新示例

我们有一个名为`users`的表，如下所示:

| id(主键) | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| one | 保罗 | Twenty-four | 密歇根 | [paul@example.com](mailto:paul@example.com) |
| Two | 懦夫 | 空 | 新泽西州 | 空 |
| three | 罗伯特 | Nineteen | 纽约 | 空 |

这个表中有一些不完整的记录。当用户给我们缺失的信息时，我们可以使用`UPDATE`语句添加它。

用户 Robert 缺少电子邮件地址。由`WHERE`子句选择的所有行都将被更新，所以我们需要小心:我们可以使用 name 列选择要更新的记录，但是名称不是惟一的——我们的表中可能有多个 Roberts。

选择要更新的行的最佳方式(确保只更新您想要更新的行)是使用`PRIMARY KEY`列，其中的值总是唯一的。在本例中，这是名为`id`的列。

因此，让我们使用以下查询来更新电子邮件地址:

```
UPDATE users
SET email="robert@example.com"
WHERE id=3;
```

现在，该表将如下所示:

| id(主键) | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| one | 保罗 | Twenty-four | 密歇根 | [paul@example.com](mailto:paul@example.com) |
| Two | 懦夫 | 空 | 新泽西州 | 空 |
| three | 罗伯特 | Nineteen | 纽约 | [robert@example.com](mailto:robert@example.com) |

## 如何同时更新多个列

Molly 在两个不同的列中缺少一个值。我们可以使用单个`UPDATE`语句，用逗号分隔赋值，如下所示:

```
UPDATE users
SET age=22, email="molly@example.com"
WHERE id=2;
```

该表现在将如下所示:

| id(主键) | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| one | 保罗 | Twenty-four | 密歇根 | [paul@example.com](mailto:paul@example.com) |
| Two | 懦夫 | Twenty-two | 新泽西州 | [molly@example.com](mailto:molly@example.com) |
| three | 罗伯特 | Nineteen | 纽约 | [robert@example.com](mailto:robert@example.com) |

# 请确保只更改您想要更改的记录

这是一个安全问题。我们的例子只有几行，但在现实生活中，它可能是拥有数百、数千甚至数百万用户的应用程序或网站的数据库。你也不想给这么多人添麻烦。

所以在发出一个`UPDATE`查询之前，发送一个带有相同`WHERE`子句的`SELECT`查询。如果它返回您想要更新的记录，那么就去做吧。否则需要更改`WHERE`条款。

例如，在为用户 Molly 发送更新之前，我们可以发送一个`SELECT`语句来检查我们使用的子句`WHERE id=2`是否正确:

```
SELECT * FROM users
WHERE id=2;
```

这个查询返回下面的记录，所以您可以使用`UPDATE`查询来完成数据。

| id(主键) | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| Two | 懦夫 | 空 | 新泽西州 | 空 |

# 结论

一旦创建了表并向其中添加了记录，总会有需要更新行的时候。本文解释了如何使用 SQL `UPDATE`语句实现这一点。

感谢阅读！