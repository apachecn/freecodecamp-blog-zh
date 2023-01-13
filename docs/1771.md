# SQL 中的 alter Table–如何在 SQL 中添加列或重命名列

> 原文：<https://www.freecodecamp.org/news/alter-table-in-sql-how-to-add-a-column-or-rename-a-column-in-sql/>

您已经创建了数据库和表，完成所有这些工作后，您注意到需要添加或重命名一个列。你可以使用`ALTER TABLE`语句来实现。

请记住，当你这样做的时候，你需要非常小心。如果您的表有很多行，可能会导致数据库的性能问题。

注意:如果这里给出的语法不起作用，请查阅您正在使用的 SQL 实现的文档。大多数东西都是一样的，但是有一些不同。

# 如何用`ALTER TABLE`添加新列

要添加新列，首先需要用`ALTER TABLE table_name`选择表，然后用`ADD column_name datatype`写入新列的名称及其数据类型。总的来说，代码如下所示:

```
ALTER TABLE table_name
ADD column_name datatype; 
```

## 使用`ALTER TABLE`添加新列的示例

我们有一个用户数据库如下:

| 身份证明（identification） | 名字 | 年龄 | 状态 | 电子邮件 |
| --- | --- | --- | --- | --- |
| one | 保罗 | Twenty-four | 密歇根 | [paul@example.com](mailto:paul@example.com) |
| Two | 懦夫 | Twenty-two | 新泽西州 | [molly@example.com](mailto:molly@example.com) |
| three | 罗伯特 | Nineteen | 纽约 | [robert@example.com](mailto:robert@example.com) |

我们已经到了需要存储用户的身份文档号的地步，所以我们需要为此添加一个新列。

要向我们的`users`表添加一个新列，我们需要用`ALTER TABLE users`选择表，然后用`ADD id_number TEXT`指定新列的名称及其数据类型。总的来说，就像这样:

```
ALTER TABLE users
ADD id_number TEXT;
```

带有新列的表如下所示:

| 身份证明（identification） | 名字 | 年龄 | 状态 | 电子邮件 | isnumber |
| --- | --- | --- | --- | --- | --- |
| one | 保罗 | Twenty-four | 密歇根 | [paul@example.com](mailto:paul@example.com) | 空 |
| Two | 懦夫 | Twenty-two | 新泽西州 | [molly@example.com](mailto:molly@example.com) | 空 |
| three | 罗伯特 | Nineteen | 纽约 | [robert@example.com](mailto:robert@example.com) | 空 |

一旦提供了信息，您将需要使用[和`UPDATE`语句](https://www.freecodecamp.org/news/sql-update-statement-update-query-in-sql/)为已经存在的用户添加缺失的信息。

### 如何用默认值而不是 NULL 创建新列

您还可以使用关键字`default`后跟要使用的值来创建具有默认值的列。然后，用户将看到默认值，而不是用 NULL 填充缺少的值。

假设我们即将开始有国际用户，我们想添加一个`country`列。我们现有的所有用户都来自美国，所以我们可以使用它作为默认值。

```
ALTER TABLE users
ADD country TEXT default "United States";
```

该表将如下所示:

| 身份证明（identification） | 名字 | 年龄 | 状态 | 电子邮件 | isnumber | 国家 |
| --- | --- | --- | --- | --- | --- | --- |
| one | 保罗 | Twenty-four | 密歇根 | [paul@example.com](mailto:paul@example.com) | 空 | 美国 |
| Two | 懦夫 | Twenty-two | 新泽西州 | [molly@example.com](mailto:molly@example.com) | 空 | 美国 |
| three | 罗伯特 | Nineteen | 纽约 | [robert@example.com](mailto:robert@example.com) | 空 | 美国 |

### 向表中添加新列时要小心

如果您的表已经有了很多行——比如您已经有了很多用户，或者已经存储了很多数据——添加一个新列可能会占用大量资源。所以一定要小心操作。

# 如何用`ALTER TABLE`重命名列

你可以用下面的代码重命名一个列。你用`ALTER TABLE table_name`选择表，然后用`RENAME COLUMN old_name TO new_name`写下要重命名的列和重命名为什么。

```
ALTER TABLE table_name
RENAME COLUMN old_name TO new_name;
```

## 如何重命名列的示例

让我们看一下上一个示例中使用的同一个表:

| 身份证明（identification） | 名字 | 年龄 | 状态 | 电子邮件 | isnumber | 国家 |
| --- | --- | --- | --- | --- | --- | --- |
| one | 保罗 | Twenty-four | 密歇根 | [paul@example.com](mailto:paul@example.com) | 空 | 美国 |
| Two | 懦夫 | Twenty-two | 新泽西州 | [molly@example.com](mailto:molly@example.com) | 空 | 美国 |
| three | 罗伯特 | Nineteen | 纽约 | [robert@example.com](mailto:robert@example.com) | 空 | 美国 |

为了避免混淆`id`和`id_number`列，让我们将第一列重命名为`user_id`。

我们将首先用`ALTER TABLE users`选择表，然后声明列名，这样它就会变成我们想用`RENAME COLUMN id TO user_id`改变的样子。

```
ALTER TABLE users
RENAME COLUMN id TO user_id;
```

\

使用查询后，该表将如下所示:

| 用户标识 | 名字 | 年龄 | 状态 | 电子邮件 | isnumber | 国家 |
| --- | --- | --- | --- | --- | --- | --- |
| one | 保罗 | Twenty-four | 密歇根 | [paul@example.com](mailto:paul@example.com) | 空 | 美国 |
| Two | 懦夫 | Twenty-two | 新泽西州 | [molly@example.com](mailto:molly@example.com) | 空 | 美国 |
| three | 罗伯特 | Nineteen | 纽约 | [robert@example.com](mailto:robert@example.com) | 空 | 美国 |

### 重命名表中的列时要小心

当您使用`ALTER TABLE`重命名列时，您有破坏数据库依赖性的风险。

如果您使用数据库重构工具来更改列名，而不是使用`ALTER TABLE`，它将管理所有的依赖项，并用新的列名更新它们。

如果您有一个小数据库，您可能不需要担心，但记住这一点很重要。

# 结论

在本文中，您已经学习了如何使用`ALTER TABLE`在表中添加列和重命名列。

请记住，这两种操作都有自己的风险，了解这一点很重要。正如有人所说，*权力越大，责任越大——*和`ALTER TABLE`是一种权力，所以要小心使用！