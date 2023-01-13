# 如何在 Python 中使用 PostgreSQL

> 原文：<https://www.freecodecamp.org/news/postgresql-in-python/>

如今有许多不同类型的数据库在使用。我们有集中式数据库、商业数据库、云数据库、分布式数据库、最终用户数据库、NoSQL 数据库、关系数据库等等。

本文将重点介绍一个关系数据库的例子( **PostgreSQL** )以及如何从中查询数据。关系数据库的其他例子包括 MySQL、MariaDB 和 SQLite。

在本教程中，您将学习如何使用 Python 安装、连接并最终查询 PostgreSQL 数据库。

首先，让我们通过学习更多关于 PostgreSQL 的知识来开始学习。

## PostgreSQL 是什么？

最著名的开源关系数据库之一是 **PostgreSQL** 。全世界各种规模的开发人员和企业都在使用它。

从全球受欢迎程度来看，PostgreSQL[被 DB-Engines 排名第四](https://db-engines.com/en/ranking)，受欢迎程度越来越大。鉴于许多 web 和移动应用程序以及分析工具都使用 PostgreSQL 数据库，这并不奇怪。

PostgreSQL 也有一个强大的生态系统，有大量的附加组件和扩展可供选择，可以很好地与主数据库配合工作。由于这些原因，无论您是想创建自己的定制数据库解决方案，还是需要一个事务性或分析性数据库，PostgreSQL 都是一个极好的选择。

现在您已经知道了 PostgreSQL 是什么，让我们讨论如何使用 Python 连接到数据库。

## 入门指南

我们必须使用数据库连接器库从 Python 脚本连接到 PostgreSQL 数据库实例。在 Python 中，我们可以从多种选择中挑选，但是 **[Psycopg2](https://www.psycopg.org/docs/)** 是最著名和使用最广泛的一个。

还有完全用 Python 构建的备选库，比如 **[pg8000](https://github.com/tlocke/pg8000)** 和 **[py-postgresql](https://github.com/python-postgres/fe)** ，但这里我们用 Psycopg2。

### 什么是 Psycopg2？

Psycopg2 库使用 C 编程语言作为对 [**libpq**](https://www.postgresql.org/docs/current/libpq.html) PostgreSQL 库的包装，以支持 Python DB API 2.0 标准。Psycopg2 的 C 实现使它变得非常快速和有效。

使用 SQL 查询，我们可以利用 Psycopg2 从数据库中获取一行或多行。有了这个库，我们还可以使用各种单次或批量插入方法将数据插入数据库。

该库类似于 SQL(结构化查询语言),它执行查询语言可以执行的所有任务和操作。它对 Unicode 和 Python 3 都是友好的，还具有线程安全性(多个线程共享同一个连接)。

它被设计成运行高度多线程的程序，这些程序频繁地产生和删除大量光标，并同时进行大量的**插入**或**更新**。Psycopg2 的特性包括客户端和服务器端游标、异步通信和通知。

## 如何安装 Psycopg2

我们必须首先安装 Psycopg2 才能使用它。我们可以使用`pip`通过终端或命令提示符安装它。

```
#installation

pip install psycopg2
pip3 install psycopg2
```

如果我们还决定在虚拟环境中安装连接器库，您可以使用以下代码:

```
virtualenv env && source env/bin/activate
pip install psycopg2-binary
```

Psycopg2 库及其所有依赖项将通过这段代码片段安装到我们的 Python 虚拟环境中。

我们已经安装了连接器，所以让我们开始输入一些查询。

## 如何使用 Python 查询 PostgreSQL

首先，您需要创建一个新文件，并随意命名。然后在 IDE 中打开它，开始编写代码。

首先要做的是导入库(这很重要)。我们将利用两个 Psycogp2 对象:

*   **连接对象**:到 PostgreSQL 数据库实例的连接由连接对象管理。它封装了一个使用函数`connect()`创建的数据库会话。
*   **光标对象**:光标对象使得 Python 脚本可以在数据库会话中运行 PostgreSQL 命令。连接生成游标，然后`cursor()`方法将它们永久地绑定到连接上。所有命令都在连接封闭的数据库会话框架内执行。

```
import psycopg2

conn = psycopg2.connect(database="db_name",
                        host="db_host",
                        user="db_user",
                        password="db_pass",
                        port="db_port")
```

为了能够连接到数据库，我们必须指定这些参数。让我们快速浏览一下这些论点。

*   **数据库:**我们希望访问或连接的数据库的名称。注意，我们只能用一个连接对象连接到一个数据库。
*   **主机:**这很可能是指数据库服务器的 IP 地址或 URL。
*   **用户**:顾名思义，这是指 PostgreSQL 用户的名字。
*   **密码**:这是匹配 PostgreSQL 用户的密码。
*   **port**:PostgreSQL 服务器在本地主机上的端口号——通常是 **5432** 。

如果我们的数据库凭证输入正确，我们将收到一个活动的数据库连接对象，我们可以用它来构建一个游标对象。我们可以继续运行任何数据库查询，并在游标对象的帮助下检索数据。

```
cursor = conn.cursor()
```

cursor object

让我们编写一个简单的查询:

```
cursor.execute("SELECT * FROM DB_table WHERE id = 1")
```

我们应用`execute()`函数并提供一个查询字符串作为它的参数。然后，将使用我们输入的查询来查询数据库。

在我们成功地做到这一点之后，为了能够使用 **Pyscopg2** 从数据库中检索数据，我们必须使用这些函数中的任何一个:`fetchone()` `fetchall()`或`fetchmany()`。

### 如何使用`fetchone()`:

运行 SQL 查询后，该函数将只返回第一行。这是从数据库中获取数据的最简单的方法。

```
#code
print(cursor.fetchone())

#output
(1, 'A-CLASS', '2018', 'Subcompact executive hatchback')
```

fetchone() example

`fetchone()`函数以**元组**的形式返回一行，信息按照查询提供的列指定的顺序排列。

在构造查询字符串时，为了区分元组中的哪些数据属于哪个数据，精确地提供列顺序是至关重要的。

### 如何使用`fetchall()`:

`fetchall()`函数的工作方式与`fetchone()`相同，只是它返回的不是一行而是所有行。因此，如果我们想要 20-200 行或更多，我们使用 Psycopg2 `fetchall()`。

```
#code
print(cursor.fetchall())

#output
[(1, 'A-CLASS', '2018', 'Subcompact executive hatchback'),
 (2, 'C-CLASS', '2021', 'D-segment/compact executive sedan'),
 (3, 'CLA', '2019', 'Subcompact executive fastback sedan'),
 (4, 'CLS', '2018', 'E-segment/executive fastback sedan'),
 (5, 'E-CLASS', '2017', 'E-segment/executive sedan'),
 (6, 'EQE', '2022', 'All-electric E-segment fastback'),
 (7, 'EQS', '2021', 'All-electric full-size luxury liftback'),
 (8, 'S-CLASS', '2020', 'F-segment/full-size luxury sedan.'),
 (9, 'G-CLASS', '2018', 'Mid-size luxury SUV, known as the G-Wagen'),
 (10, 'GLE', '2019', 'Mid-size luxury crossover SUV')]
[...]
```

fetchall() example

### 如何使用`fetchmany()`:

`fetchmany()`函数允许我们从数据库中获取大量记录，并为我们获取的精确行数提供额外的控制。

```
#code
print(cursor.fetchmany(size=3))

#output
[(1, 'A-CLASS', '2018', 'Subcompact executive hatchback'),
 (2, 'C-CLASS', '2021', 'D-segment/compact executive sedan'),
 (3, 'CLA', '2019', 'Subcompact executive fastback sedan')] 
```

fetchmany() example

因为我们将参数设置为 3，所以只收到了三行。

当我们查询完数据库后，我们需要关闭与`conn.close()`的连接。

## 结论

很简单，对吧？我们能够从一个 Python 脚本中执行所有这些任务，而且效果非常好。

我希望这篇文章对您有所帮助，并且您现在可以使用 Python 处理 PostgreSQL 了。

要了解更多信息，请查看 Psycopg2 [文档](https://www.psycopg.org/docs/)以了解更多信息。