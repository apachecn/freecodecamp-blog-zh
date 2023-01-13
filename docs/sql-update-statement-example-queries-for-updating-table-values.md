# SQL Update 语句-更新表值的示例查询

> 原文：<https://www.freecodecamp.org/news/sql-update-statement-example-queries-for-updating-table-values/>

SQL(发音为 Seequel)代表结构化查询语言。是 1974 年首次出现的[强类型](https://en.wikipedia.org/wiki/Strong_and_weak_typing)，静态(运行时前检查类型)查询语言(woah，46 岁！)，但直到 1986 年才首次发布。

你可能会对自己说，这样一个“旧”的工具已经有了它最好的时光，但是你远不是正确的。2019 年，通过 Scale Grid [DeveloperWeek 调查](https://scalegrid.io/blog/2019-database-trends-sql-vs-nosql-top-databases-single-vs-multiple-database-use/)，有 60.5%的受访者使用 SQL，而 [NoSQL](https://en.wikipedia.org/wiki/NoSQL) 只有 39.5 %的受访者使用。

为了清楚起见，SQL 类别被分成几个子类，包括 [MySQL](https://en.wikipedia.org/wiki/MySQL) 、 [PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL) 、 [SQL Server](https://en.wikipedia.org/wiki/Microsoft_SQL_Server) 等等，而 NoSQL 类别被分成包含 [MongoDB](https://en.wikipedia.org/wiki/MongoDB) 、 [Cassandra](https://en.wikipedia.org/wiki/Apache_Cassandra) 等等的子类。

即使在 2017 年，根据 [Stack Overflow 开发者的调查](https://insights.stackoverflow.com/survey/2017)，第二大最受欢迎的语言是 SQL(仅次于 JavaScript)，64，000 名受访者中有 50%的人表示他们仍在以某种形式使用 SQL。

它的流行至少在一定程度上是由于语言的简单性，它是在考虑关系数据的情况下构建的这一事实，还因为它已经证明自己在搜索、连接和过滤数据方面是可靠的。

可以说，SQL 不仅生机勃勃，而且在当今的开发社区中欣欣向荣。

现在我们来看看为什么！

## 有趣的部分

SQL Server 是我在日常工作中使用的首选 SQL 风格，因此下面的示例将符合这些标准。

我发现自己经常做的一件事是更新一个表中的多条记录。现在我可以一次更新一条记录，但是 SQL 让我们能够通过`UPDATE`语句一次更新多条(如果需要的话，成千上万条)记录。

`UPDATE`语句可用于更新数据库中的单个列、一组更大的记录(通过使用条件)和/或整个表。条件可以是布尔值、字符串检查或解析为布尔值(大于、小于等)的数学序列。).

虽然各种风格之间可能略有不同，但一般语法如下:

`**UPDATE** *table-name*
**SET** *column-name = value[, column-name=value]*
[**WHERE** *condition*]`

上面的方括号([])表示对查询的可选添加。

*** * *需要注意的是，如果没有`WHERE`条件，表中的所有记录将在您执行查询时立即更新。*****

## 示例查询

作为我们的数据集，我将使用这个名为 ***Work_Tickets*** 的表:

|  |
| **SalesOrderNum** | **工作票编号** | **客户 _ 代码** | **客户 _ 联系人** | 单位成本 | **话单** | **ParentLineKey** | **订购数量** | **发货数量** |
| 00061356 | 000931 | One thousand two hundred and fifty | sales@wayneindustries.com | Zero | 错误的 | 079777 | Twelve | Zero |
| 00061357 | 000932 | One thousand two hundred and fifty-one | contact@starkindustries.com | Zero | 错误的 | 085695 | One hundred and ninety-six point five | Zero |
| 00061358 | 000933 | One thousand two hundred and fifty-two | animation@acmetoons.com | Zero | 错误的 | 085569 | Seventeen point five | Zero |

#### 没有条件的简单查询

下面是一个非常简单的更新查询，它将所有的`UnitCost`字段更改为数字`131.6152`:

`UPDATE Work_Tickets`
`SET UnitCost = 131.6152` 

注意，这里没有`WHERE`子句，所以表中的每一行都将被更新，我们的数据集现在看起来像这样:

|  |
| **SalesOrderNum** | **工作票编号** | **客户 _ 代码** | **客户 _ 联系人** | 单位成本 | **话单** | **ParentLineKey** | **订购数量** | **发货数量** |
| 00061356 | 000931 | One thousand two hundred and fifty | sales@wayneindustires.com | 131.6152 | 错误的 | 079777 | Twelve | Zero |
| 00061357 | 000932 | One thousand two hundred and fifty-one | contact@starkindustries.com | 131.6152 | 错误的 | 085695 | One hundred and ninety-six point five | Zero |
| 00061358 | 000933 | One thousand two hundred and fifty-two | animation@acmetoons.com | 131.6152 | 错误的 | 085569 | Seventeen point five | Zero |

#### 带有条件的简单查询

下面是一个带有一个条件语句的简单查询:

`UPDATE Work_Tickets
SET Billed = true
WHERE UnitCost <> 0.00`

这个查询将在符合不等于 0 的`UnitCost`的条件的每一行上将`Billed`字段更新为*真*。运行查询后，数据集将如下所示:

|  |
| **SalesOrderNum** | **工作票编号** | **客户 _ 代码** | **客户 _ 联系人** | 单位成本 | **话单** | **ParentLineKey** | **订购数量** | **发货数量** |
| 00061356 | 000931 | One thousand two hundred and fifty | sales@wayneindustires.com | 131.6152 | 真实的 | 079777 | Twelve | Zero |
| 00061357 | 000932 | One thousand two hundred and fifty-one | contact@starkindustries.com | 131.6152 | 真实的 | 085695 | One hundred and ninety-six point five | Zero |
| 00061358 | 000933 | One thousand two hundred and fifty-two | animation@acmetoons.com | 131.6152 | 真实的 | 085569 | Seventeen point five | Zero |

下面是一个查询，我们将`ParentLineKey`改为字符串`000134`，其中`SalesOrderNum`和`WorkTicketNum`都匹配给定的字符串。

`UPDATE Work_Tickets
SET ParentLineKey = 000134
WHERE SalesOrderNum = 00061358 and WorkTicketNumber = 000933`

因此，`ParentLineKey`字段中的 085569 将被替换为`000134`，我们的数据集现在看起来像这样:

|  |
| **SalesOrderNum** | **工作票编号** | **客户 _ 代码** | **客户 _ 联系人** | 单位成本 | **话单** | **ParentLineKey** | **订购数量** | **发货数量** |
| 00061356 | 000931 | One thousand two hundred and fifty | sales@wayneindustires.com | 131.6152 | 真实的 | 079777 | Twelve | Zero |
| 00061357 | 000932 | One thousand two hundred and fifty-one | contact@starkindustries.com | 131.6152 | 真实的 | 085695 | One hundred and ninety-six point five | Zero |
| 00061358 | 000933 | One thousand two hundred and fifty-two | animation@acmetoons.com | 131.6152 | 真实的 | 000134 | Seventeen point five | Zero |

#### 更新多个字段

假设您有一个比我们目前使用的数据集大得多的数据集，并且您有几个字段要更新。

用不同的 update 语句来更新它们将是乏味且令人麻木的。幸运的是，我们还可以用 update 语句一次更新几个字段，只要我们用逗号分隔列名:

`UPDATE Work_Tickets
SET UnitCost = 129.8511, Qty_Ordered = 72, Qty_Shipped = 72
WHERE SalesOrderNum = 00061358`

以下是运行查询后更新字段的结果:

|  |
| **SalesOrderNum** | **工作票编号** | **客户 _ 代码** | **客户 _ 联系人** | 单位成本 | **话单** | **ParentLineKey** | **订购数量** | **发货数量** |
| 00061356 | 000931 | One thousand two hundred and fifty | sales@wayneindustires.com | 131.6152 | 真实的 | 079777 | Twelve | Zero |
| 00061357 | 000932 | One thousand two hundred and fifty-one | contact@starkindustries.com | 131.6152 | 真实的 | 085695 | One hundred and ninety-six point five | Zero |
| 00061358 | 000933 | One thousand two hundred and fifty-two | animation@acmetoons.com | 129.8511 | 真实的 | 000134 | seventy-two | seventy-two |

#### 在子查询中使用 Update

如果您使用的是一个数据源，那么上面的例子是完美的。但是，大多数数据不会存储在单个表中。这就是对多个数据源使用 *UPDATE* 派上用场的地方。

如果我们想从另一个表中引入数据，更新列/表的语法会稍有变化:

`**UPDATE** *table-name*
**SET** *column-name = (SELECT column name(s)
FROM table2-name
WHERE condition(s))*
[**WHERE** *condition*]`

这里是我们将在这个查询中使用的两个表——***Work _ Tickets 表:***

|  |
| **SalesOrderNum** | **工作票编号** | **客户 _ 代码** | **客户 _ 联系人** | 单位成本 | **话单** | **ParentLineKey** | **订购数量** | **发货数量** |
| 00061356 | 000931 | One thousand two hundred and fifty | sales@wayneindustires.com | 131.6152 | 真实的 | 079777 | Twelve | Zero |
| 00061357 | 000932 | One thousand two hundred and fifty-one | contact@starkindustries.com | 131.6152 | 真实的 | 085695 | One hundred and ninety-six point five | Zero |
| 00061358 | 000933 | One thousand two hundred and fifty-two | animation@acmetoons.com | 129.8511 | 真实的 | 000134 | seventy-two | seventy-two |

以及 ***客户信息表:***

|  |
| **名称** | **行业** | **代码** | **地址** | **城市** | **折扣** | **电话号码** | **电子邮件** |
| 韦恩企业 | 国防、武器、航空航天、工程 | 空 | 1631 黑暗骑士道 | 哥谭市 | Nineteen point seven five | Five billion five hundred and fifty-six million six hundred and fourteen thousand | sales@wayneindustires.com |
| 斯塔克工业 | 国防、武器、保护 | One thousand two hundred and fifty-one | 5641 铁博士 | 身份不明的 | Nineteen point seven three | Nine billion nine hundred and ninety-three million one hundred and twenty-six thousand one hundred and fifty-six | contact@starkindustries.com |
| 阿克梅公司 | 喜剧、笑声、动画 | One thousand two hundred and fifty-two | 微笑街 24569 号 | 卡通镇 | Seventeen point five three | Three billion two hundred and sixteen million five hundred and forty-nine thousand eight hundred and seventy-seven | animation@acmetoons.com |

带有[子查询](https://docs.microsoft.com/en-us/sql/relational-databases/performance/subqueries?view=sql-server-ver15)的`UPDATE`语句如下所示:

`UPDATE Customer_Info
SET Code = (SELECT Customer_Code
FROM Work_Tickets
WHERE Work_Tickets.Customer_Contact = Customer_Info.Email)
FROM Work_Tickets
WHERE Code IS NULL`

这个例子将更新 *Customer_Info* 表上的 *Code* 字段，其中电子邮件地址与两个表中的相匹配。这是我们的*客户信息*表现在的样子:

|  |
| **名称** | **行业** | **代码** | **地址** | **城市** | **折扣** | **电话号码** | **电子邮件** |
| 韦恩企业 | 国防、武器、航空航天、工程 | One thousand two hundred and fifty | 1631 黑暗骑士道 | 哥谭市 | Nineteen point seven five | Five billion five hundred and fifty-six million six hundred and fourteen thousand | sales@wayneindustires.com |
| 斯塔克工业 | 国防、武器、保护 | One thousand two hundred and fifty-one | 5641 铁博士 | 身份不明的 | Nineteen point seven three | Nine billion nine hundred and ninety-three million one hundred and twenty-six thousand one hundred and fifty-six | contact@starkindustries.com |
| 阿克梅公司 | 喜剧、笑声、动画 | One thousand two hundred and fifty-two | 微笑街 24569 号 | 卡通镇 | Seventeen point five three | Three billion two hundred and sixteen million five hundred and forty-nine thousand eight hundred and seventy-seven | animation@acmetoons.com |

## 包扎

我希望这篇文章对您理解 SQL 中的 *UPDATE* 语句有所帮助。

您现在已经准备好像冠军一样编写自己的 SQL *UPDATE* 语句了！之后，我希望你能在社交媒体上与我分享！

别忘了看看我的[博客](https://jonathansexton.me/blog)，我经常在那里发布关于 web 开发的文章。

既然你在那里，为什么不订阅我的时事通讯呢？你可以在博客主页的右上方这样做。我喜欢不时地为开发人员发送有趣的文章(我的和其他人的)、资源和工具。

如果你对这篇文章有任何疑问，或者只是一般来说，我的 DMs 是开放的——来在 [Twitter](https://twitter.com/jj_goose) 或我的任何其他社交媒体账户上打招呼，你可以在时事通讯下面找到，在我的博客主页上注册，或者在我在 fCC 的简介上注册:)

祝你有美好的一天和快乐的编码，朋友！