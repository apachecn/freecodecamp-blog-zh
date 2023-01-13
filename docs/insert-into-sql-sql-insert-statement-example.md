# 插入到 SQL–SQL Insert 语句示例

> 原文：<https://www.freecodecamp.org/news/insert-into-sql-sql-insert-statement-example/>

如果您在数据库中创建了一个空表，您需要向其中添加记录。在本文中，您将了解如何使用 SQL 中的`INSERT`语句向表中添加记录。

请记住，如果这里给出的语法不起作用，您可以查看文档以了解您正在使用的 SQL 的实现。大多数东西都是一样的，但是有一些不同。

# SQL `INSERT`语句语法

一个`INSERT`语句指定您想要向哪个表中添加记录。您编写命令`INSERT INTO table_name`，然后是关键字`VALUES`，后面是您想要在括号内的每一列中添加的值，用逗号分隔，如下所示:

```
INSERT INTO table_name
VALUES (value1, value2, value3...);
```

这些值将按照在表中定义列的顺序添加到列中。

## 如何为选定的列赋值

比方说，您想只给几列赋值——例如，如果您想避免手动设置`id`,那么它会自动完成。您可以使用下面的语法:

```
INSERT INTO table_name(column1, column2...)
VALUES (value1, value2...);
```

这些值将按照它们在括号中的写入顺序分配给列。

# SQL `INSERT`语句示例

让我们创建一个表，然后我们将使用`INSERT`向其中添加前几条记录。

下面的代码将创建一个名为`users`的表，它有 5 列。我们将有一个作为`PRIMARY KEY`的`id`列(该列将总是具有唯一的值，并允许我们唯一地标识一行)，然后是`name`、`age`、`state`和`email`列。

```
CREATE TABLE users (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT,  age INTEGER, state TEXT, email TEXT);
```

让我们使用我们看到的第一个语法将第一条记录添加到这个表中。

我们将使用下面的查询添加来自`Michigan`的`state`的用户`Paul`，其`id`为`1`，其`age`为`24`，并且其电子邮件地址为`paul@example.com`:

```
INSERT INTO users
VALUES (1, "Paul", 24, "Michigan", "paul@example.com");
```

这将使表格看起来像这样:

| id(主键) | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| one | 保罗 | Twenty-four | 密歇根 | [paul@example.com](mailto:paul@example.com) |

使用这种语法，每一列都必须有一个值，否则它将抛出一个错误并且不起作用。

现在让我们使用上面看到的第二种语法添加一些其他记录。

```
INSERT INTO users (name, state)
VALUES ("Molly", "New Jersey");

INSERT INTO users (name, state, age)
VALUES ("Robert", "New York", 19);
```

在这种情况下，第一个值被分配给首先提到的列，因此`"Molly"`被分配给`name`列，而`"New Jersey"`被分配给`state`列。然后对于其他记录，列`name`被赋予值`"Robert"`，列`state`得到`"New York"`，列`age`被赋予`19`。

没有赋值的列会发生什么情况？类型为`INTEGER PRIMARY KEY AUTOINCREMENT`的列会自动更新，确保每一行都有唯一的值。当没有为其他列指定值时，它们被赋予一个值`NULL`。

现在表格看起来如下。注意,`id`列已经被更新，在每一行中都有唯一的值，即使我们没有明确地给它赋值。没有赋值的其他列的值为`NULL`。

| id(主键) | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| one | 保罗 | Twenty-four | 密歇根 | [paul@example.com](mailto:paul@example.com) |
| Two | 懦夫 | 空 | 新泽西州 | 空 |
| three | 罗伯特 | Nineteen | 纽约 | 空 |

# 结论

第一次在数据库中创建表时，它是空的。本文解释了如何向表中添加记录，这是创建数据库的良好起点。