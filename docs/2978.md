# SQL Create Table 语句-示例语法

> 原文：<https://www.freecodecamp.org/news/sql-create-table-statement-with-example-syntax/>

SQL 是最可靠和最简单的查询语言之一。它提供了清晰的语法，阅读起来很容易，而不会抽象出太多功能的含义。

如果您想了解该语言的一些历史以及一些有趣的事实，请查看我的 [SQL Update 语句文章](https://www.freecodecamp.org/news/sql-update-statement-example-queries-for-updating-table-values/)的介绍部分。

在本文中，我们将介绍在 SQL 中创建表的重要部分。我更喜欢 SQL 的“风格”是 SQL Server，但是关于创建表的信息在所有 SQL 变体中相当普遍。

如果您从未使用过 SQL 或者不知道什么是表，不要害怕！简单地说(广义地说)，表是一个数据库对象，它保存或包含数据库该部分中的所有数据。它将这些数据存储在命名的列和编号的行中，如果您曾经使用过任何电子表格程序，这并不陌生。每行代表一个完整的数据库记录。

如果数据是盒子形式的，那么表格就是我们存放这些盒子的仓库货架的一部分。

![nana-smirnova-IEiAmhXehwE-unsplash](img/f3df561a5ee744377b618b3cb43dc264.png)

Photo by [Nana Smirnova](https://unsplash.com/@nananadolgo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/warehouse?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我大大简化了解释，还有更多关于 SQL 表的内容，但这超出了本文的范围。如果你渴望得到关于表格的更深入的解释，我鼓励你深入阅读微软数据库设计文档。

在我们学习如何创建表之前，我们必须了解这些列和行可以存储什么类型的数据。

## 数据类型

SQL 表可以保存文本、数字、文本和数字的组合，以及图像和链接。

当创建我们的表时，我们指定它的行和列将保存的数据类型。以下是数据的总体分类:

*   近似数字
*   用线串
*   日期和时间
*   Unicode 字符串
*   精确数字
*   其他的

我将在下面列出一些更常用的数据类型，但是如果您想了解所有数据类型的更多信息，我邀请您查看这篇来自微软的关于每种类型的[详尽文章。](https://docs.microsoft.com/en-us/sql/t-sql/data-types/data-types-transact-sql?view=sql-server-ver15)

以下是我的经验中比较常用的数据类型，排名不分先后:

*   char(size) - *固定长度的*字符串，可以包含字母、数字、特殊字符
*   varchar(size) - *可变*长度字符串，可以包含字母，数字，&等特殊字符
*   布尔零(或等于 0 的值)为假，非零为真
*   int( *size 可选* ) -长度最多 10 个字符的数字，接受负数&正数
*   bigint( *size 可选* ) -一个最长 19 个字符的数字，接受负数&正数
*   float(size，d) -一个数字，总的数字大小由 size 表示，小数点后的字符数由 *d* 表示
*   日期日期格式为 *YYYY-MM-DD*
*   日期时间-日期时间，格式为*YYY-月-日 hh:mm:ss*
*   时间-时间格式为 *hh:mm:ss*

好了，现在我们知道了行和列可以包含什么类型的数据，让我们进入有趣的部分吧！

## 创建表格

![nikhil-mitra-Q_6BS8IN0J8-unsplash](img/6a9d1fdead5d32728252df3e826e0107.png)

Photo by [Nikhil Mitra](https://unsplash.com/@nikhilmitra?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/create?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在我们开始之前，重要的是要注意，我将独立于任何程序提供我所有的例子。

但是，如果您想开始编写查询，但又不确定从哪里开始，那么可以看看 SQL Server Management Studio。这是一个免费的、强大的程序，在社区中得到广泛使用和支持。

或者，有几个选项，包括 [DB Fiddle](https://www.db-fiddle.com/) ，允许您在浏览器中构建模式和编写查询。

让我们从创建一个基本表的简单语句开始:

`CREATE TABLE table_name ( column1_name datatype, column2_name datatype, column3_name datatype, column4_name datatype, column5_name datatype,)`

我们可以在`datatype`之后添加其他参数来扩充列:

*   `NOT NULL` -传递该参数将确保该列不能保存`NULL`值
*   `UNIQUE` -传递该参数将防止该列多次保存相同的值
*   `UNIQUE KEY` -传递该参数会将该列指定为唯一标识符。它本质上是前面两个参数的组合。

现在，我们将创建一个表(名为 doggo_info，必须符合数据库的标识符标准)来保存关于 Woof Woof Retreat 的居民的信息，这是我刚刚想到的一个虚构的狗狗日托所:)

`CREATE TABLE doggo_info ( ID int UNIQUE KEY, Name varchar(50) NOT NULL, Color varchar(50), Breed varchar(50), Age int, Weight int, Height int, Fav_Food varchar(100), Fav_Toy varchar(100), Dislikes varchar(500), Allergies varchar(500) NOT NULL )`

这是我们刚刚创建的全新表格:

| 名字 | 颜色 | 类型 | 年龄 | 重量 | 高度 | 最喜欢的食物 | Fav_Toy | 厌恶 | 厌恶 |
|  |  |  |  |  |  |  |  |  |  |

您会注意到我们的表完全是空的，这是因为我们还没有向其中添加任何数据。这样做超出了本文的范围，但是我希望您注意这个小细节。

### 从现有表格创建表格

也可以基于现有的表创建一个新表。

这非常简单，不需要太多的语法。我们需要选择要“复制”的表和列:

`CREATE TABLE new_table_name AS SELECT column1, column2, column3, column4 (use * to select all columns to be added to the new_table) FROM current_table_name WHERE conditions_exist`

因此，为了方便起见，我在我们的`doggo_info`表中添加了一些数据，现在看起来像下面的例子:

|  |
| 名字 | 颜色 | 类型 | 年龄 | 重量 | 高度 | 最喜欢的食物 | Fav_Toy | 厌恶 | 厌恶 |
| 雏菊 | 红色 | 标准腊肠 | one | Fourteen | six | 鲑鱼味粗粮 | 挤压球 | 飞过院子的鸟 | 猫，洗澡，清洁 |
| 主要的 | 黑色/棕褐色 | 罗特韦尔犬 | three | Forty-one | Seventeen | 几乎任何东西 | 绳索拖船 | 远离沙发 | 倾听，表现，而不是在任何事情上流口水 |
| 三明治 | 淡蜂蜜 | 金毛猎犬 | nine | Forty-six | Nineteen | 牛肉味粗粮 | 她的床 | 粗暴的小狗 | 未知 |

现在，我们可以通过运行下面的查询，基于我们的`doggo_info`表中的数据创建另一个表:

`CREATE TABLE puppies_only AS SELECT * FROM doggo_info WHERE Age < 4`

我们希望创建一个新表，包含来自`doggo_info`表的所有列，但是只在`Age`小于 4 的地方。运行该查询后，我们的新表将如下所示:

|  |
| 名字 | 颜色 | 类型 | 年龄 | 重量 | 高度 | 最喜欢的食物 | Fav_Toy | 厌恶 | 厌恶 |
| 雏菊 | 红色 | 标准腊肠 | one | Fourteen | six | 鲑鱼味粗粮 | 挤压球 | 飞过院子的鸟 | 猫，洗澡，清洁 |
| 主要的 | 黑色/棕褐色 | 罗特韦尔犬 | three | Forty-one | Seventeen | 几乎任何东西 | 绳索拖船 | 远离沙发 | 倾听，表现，而不是在任何事情上流口水 |

我希望你能看到这句话的力量有多大。通过查询中的几行代码，我们实际上已经将数据从一个表复制到另一个表中，但只复制了我们需要的行。

这不仅是您开发人员工具箱中的一个方便工具——当您需要在表之间移动数据时，它将为您节省大量时间。

## 包扎

现在您已经知道了如何在 SQL 中创建(或复制)一个表，无论您面临什么情况，您都可以开始用数据填充列和行来存储了！

`CREATE TABLE`语句非常有用和强大。你就可以开始好好利用它了。

如果你觉得这篇文章很有帮助，可以看看我的博客,我经常在那里发布关于 web 开发、生活和学习的文章。

既然你在那里，为什么不订阅我的时事通讯呢？你可以在博客主页的右上方这样做。我喜欢不时地为开发人员发送有趣的文章(我的和其他人的)、资源和工具。

如果你对这篇文章有任何疑问，或者只是一般地让我知道——来在 [Twitter](https://twitter.com/jj_goose) 或者我的任何其他社交媒体账户上打招呼，你可以在时事通讯下面找到，在我的博客主页上注册，或者在我在 fCC 的个人资料上注册:)

祝你愉快！朋友，快乐学习，快乐编码！