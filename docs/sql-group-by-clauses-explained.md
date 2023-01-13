# SQL 分组教程:计数、求和、平均和 Having 子句说明

> 原文：<https://www.freecodecamp.org/news/sql-group-by-clauses-explained/>

子句是一个强大但有时需要思考的复杂语句。

即使八年后，每次我使用一个`GROUP BY`时，我都必须停下来想想它到底在做什么。

在本文中，我们将了解如何构建一个`GROUP BY`子句，它对您的查询有什么作用，以及您如何使用它来执行聚合和收集关于您的数据的见解。

以下是我们将要介绍的内容:

*   [设置您的数据库](#setting-up-your-database)
*   [设置示例数据(创建销售)](#setting-up-the-data-creating-sales-)
*   [a`GROUP BY`是如何工作的？](#how-does-a-group-by-work)
*   [撰写`GROUP BY`条款](#writing-group-by-clauses)
*   [聚合(`COUNT`、`SUM`、`AVG` )](#aggregations-count-sum-avg-)
*   [与多个小组合作](#working-with-multiple-groups)
*   [使用`GROUP BY`中的功能](#using-functions-in-the-group-by)
*   [用`HAVING`过滤组](#filtering-groups-with-having)
*   [隐式分组聚合](#aggregates-with-implicit-grouping)

## 设置您的数据库

在编写查询之前，我们需要设置数据库。

对于这些示例，我们将使用 PostgreSQL，但是这里显示的查询和概念将很容易转换到任何其他现代数据库系统(如 MySQL、SQL Server 等)。

为了使用我们的 PostgreSQL 数据库，我们可以使用[psql](https://www.postgresql.org/docs/current/app-psql.html)——交互式 PostgreSQL 命令行程序。如果您有另一个喜欢使用的数据库客户机，那也很好。

首先，让我们创建数据库。已经安装了 [PostgreSQL](https://www.postgresql.org/download/) 之后，我们可以在终端上运行命令`createdb <database-name>`来创建一个新的数据库。我叫我的`fcc`:

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

## 设置数据(创建销售)

在我们的示例中，我们将使用一个表来存储不同商店位置的各种产品的销售记录。

我们将这个表称为`sales`，它将是商店销售的简单表示:位置名称、产品名称、价格和销售时间。

如果我们在实际的应用程序中构建这个表，我们将设置其他表的外键(比如`locations`或`products`)。但是为了说明`GROUP BY`概念，我们将只使用简单的`TEXT`列。

让我们创建表并插入一些销售数据:

```
CREATE TABLE sales(
  location TEXT,
  product TEXT,
  price DECIMAL,
  sold_at TIMESTAMP
);

INSERT INTO sales(location, product, price, sold_at) VALUES
('HQ', 'Coffee', 2, NOW()),
('HQ', 'Coffee', 2, NOW() - INTERVAL '1 hour'),
('Downtown', 'Bagel', 3, NOW() - INTERVAL '2 hour'),
('Downtown', 'Coffee', 2, NOW() - INTERVAL '1 day'),
('HQ', 'Bagel', 2, NOW() - INTERVAL '2 day'),
('1st Street', 'Bagel', 3, NOW() - INTERVAL '2 day' - INTERVAL '1 hour'),
('1st Street', 'Coffee', 2, NOW() - INTERVAL '3 day'),
('HQ', 'Bagel', 3, NOW() - INTERVAL '3 day' - INTERVAL '1 hour'); 
```

我们有三个地点:*总部*、*市区*和*第一街。*

我们有两种产品，*咖啡*和*百吉饼*，我们用不同的`sold_at`值插入这些销售额，以表示在不同的日期和时间销售的商品。

今天有销售，昨天有，前天也有。

## 一个`GROUP BY`是如何工作的？

为了说明`GROUP BY`子句是如何工作的，我们先通过一个例子来谈谈。

想象一下，我们有一屋子出生在不同国家的人。

如果我们想知道房间里的人在每个国家的平均身高，我们首先会要求这些人根据他们的出生国分组。

一旦他们被分成不同的组，我们就可以计算出该组的平均身高。

这就是`GROUP BY`子句的工作方式。首先，我们定义如何将行分组，然后我们可以对这些组执行计算或聚合。

### 多个组

我们可以根据需要将数据分成任意多的组或子组。

例如，在要求人们根据他们的出生国家分组后，我们可以告诉每一组国家根据他们眼睛的颜色进一步将*分成*组*。*

通过这样做，我们可以根据他们的出生国家*和*他们眼睛的颜色进行分组。

现在我们可以找到每个小组的平均身高，我们会有一个更具体的结果:每个国家每个眼睛颜色的平均身高。

`GROUP BY`从句常用于这样的情况:你可以用短语 ***表示每个**某物*或 ***表示每个**某物*:

*   每个出生国家的平均身高
*   *每个眼睛和头发颜色组合的总人数*
*   **每个产品的总销售额**

## ***撰写`GROUP BY`条款***

***一个`GROUP BY`子句非常容易编写——我们只需使用关键字`GROUP BY`,然后指定我们想要分组的字段:***

```
***`SELECT ...
FROM sales
GROUP BY location;`***
```

***这个简单的查询按照`location`列对我们的`sales`数据进行分组。***

***我们已经完成了分组——但是我们应该在`SELECT`中放什么呢？***

***显而易见，要选择的是我们的`location`—我们按它分组，所以我们至少希望看到我们分组的名称:***

```
***`SELECT location
FROM sales
GROUP BY location;`*** 
```

***结果是我们的三个位置:***

```
 ***`location
------------
 1st Street
 HQ
 Downtown
(3 rows)`*** 
```

***如果我们查看我们的原始表数据(`SELECT * FROM sales;`)，我们会看到有四行位置为*总部*，两行位置为*市中心*，两行位置为*第一街:****

```
 **`product |  location  | price |          sold_at
---------+------------+-------+----------------------------
 Coffee  | HQ         |     2 | 2020-09-01 09:42:33.085995
 Coffee  | HQ         |     2 | 2020-09-01 08:42:33.085995
 Bagel   | Downtown   |     3 | 2020-09-01 07:42:33.085995
 Coffee  | Downtown   |     2 | 2020-08-31 09:42:33.085995
 Bagel   | HQ         |     2 | 2020-08-30 09:42:33.085995
 Bagel   | 1st Street |     3 | 2020-08-30 08:42:33.085995
 Coffee  | 1st Street |     2 | 2020-08-29 09:42:33.085995
 Bagel   | HQ         |     3 | 2020-08-29 08:42:33.085995
(8 rows)`** 
```

**通过在`location`列上分组，我们的数据库获取这些输入行，并识别它们之间的唯一位置——这些唯一位置作为我们的*组。***

**但是表中的其他列呢？**

**如果我们试图选择一个像`product`这样没有分组的列...**

```
**`SELECT
  location,
  product
FROM sales
GROUP BY location;`** 
```

**...我们遇到了这个错误:**

```
**`ERROR:  column "sales.product" must appear in the GROUP BY clause or be used in an aggregate function`** 
```

**这里的问题是，我们已经采取了*八*行，并压扁或蒸馏他们到*三。***

**我们不能像平常一样只返回其余的列—我们有八行，现在有三行了。**

**我们如何处理剩下的五行数据？这八行数据中的哪一行应该显示在这三个不同的位置行上？**

**这里没有一个明确而确定的答案。**

**为了使用表中的其余数据，我们还必须将这些剩余列中的数据提取出来，分成三个位置组。**

**这意味着我们必须**聚集**或者执行计算来产生某种关于我们剩余数据的汇总信息。**

## **聚合(`COUNT`、`SUM`、`AVG`)**

**一旦我们决定了如何对数据进行分组，我们就可以对剩余的列执行聚合。**

**例如计算每组的行数、对整个组的特定值求和，或者对组内的信息求平均值。**

**首先，让我们找出每个位置的*销售额。***

**因为我们的`sales`表中的每条记录都是一笔销售，所以每个位置的销售数量就是**每个位置组中的行数。****

**为此，我们将使用聚合函数`COUNT()`来计算每个组中的行数:**

```
**`SELECT
  location,
  COUNT(*) AS number_of_sales
FROM sales
GROUP BY location;`** 
```

**我们使用`COUNT(*)`对一个组的所有输入行进行计数。**

**(`COUNT()`也适用于表达式，但其行为略有不同。)**

**下面是数据库执行该查询的方式:**

*   **`FROM sales` —首先，从`sales`表中检索所有记录**
*   **`GROUP BY location` —接下来，确定唯一的`location`组**
*   **`SELECT ...` —最后，选择位置名称和该组中的行数**

**我们还使用`AS number_of_sales`给这个行数取了一个别名，以使输出更具可读性。看起来是这样的:**

```
 **`location  | number_of_sales
------------+-----------------
 1st Street |               2
 HQ         |               4
 Downtown   |               2
(3 rows)`** 
```

**第一街位置*有两个销售，*总部*有四个，*市区*有两个。***

*在这里，我们可以看到如何从八个独立的行中提取剩余的列数据，并将其提取为每个位置的有用汇总信息:销售额。*

### *`SUM`*

*以类似的方式，我们可以对组内的信息求和，而不是计算一个组中的行数，比如从这些位置赚到的钱的总数。*

*为此，我们将使用`SUM()`函数:*

```
*`SELECT
  location,
  SUM(price) AS total_revenue
FROM sales
GROUP BY location;`* 
```

*我们没有计算每组中的行数，而是合计了每笔销售的金额，这显示了每个位置的总收入:*

```
 *`location  | total_revenue
------------+---------------
 1st Street |             5
 HQ         |             9
 Downtown   |             5
(3 rows)`* 
```

### *平均值(`AVG`)*

*找到每个位置的平均销售价格意味着将`SUM()`函数替换为`AVG()`函数:*

```
*`SELECT
  location,
  AVG(price) AS average_revenue_per_sale
FROM sales
GROUP BY location;`* 
```

## *与多个组合作*

*到目前为止，我们只和一组人合作:地点。*

*如果我们想进一步细分这个群体呢？*

*类似于我们开始时的*“出生国家和眼睛颜色”*场景，如果我们想要找到每个地点每个产品的销售数量**会怎么样呢？***

*为此，我们只需将第二个分组条件添加到我们的`GROUP BY`语句中:*

```
*`SELECT ...
FROM sales
GROUP BY location, product;`*
```

*通过在我们的`GROUP BY`中添加第二列，我们进一步将我们的位置组细分为每个产品的位置组*。**

*因为我们现在也按`product`列分组，所以我们现在可以在我们的`SELECT`中返回它！*

*(我将在这些查询中加入一些`ORDER BY`子句，以使输出更容易阅读。)*

```
*`SELECT
  location,
  product
FROM sales
GROUP BY location, product
ORDER BY location, product;`* 
```

*查看我们新分组的结果，我们可以看到我们独特的位置/产品组合:*

```
 *`location  | product
------------+---------
 1st Street | Bagel
 1st Street | Coffee
 Downtown   | Bagel
 Downtown   | Coffee
 HQ         | Bagel
 HQ         | Coffee
(6 rows)`* 
```

*现在我们有了自己的组，我们想如何处理其余的列数据呢？*

*那么，我们可以使用与之前相同的聚合函数来查找每个地点的每个产品的销售数量*:**

```
*`SELECT
  location,
  product,
  COUNT(*) AS number_of_sales
FROM sales
GROUP BY location, product
ORDER BY location, product;`* 
```

```
 *`location  | product | number_of_sales
------------+---------+-----------------
 1st Street | Bagel   |               1
 1st Street | Coffee  |               1
 Downtown   | Bagel   |               1
 Downtown   | Coffee  |               1
 HQ         | Bagel   |               2
 HQ         | Coffee  |               2
(6 rows)`* 
```

> *作为对读者的一个*练习:*找出每个地点每种产品的总收入(总和)。*

## *使用`GROUP BY`中的功能*

*接下来，我们试着找出每天的总销售数**。***

*如果我们遵循与我们的位置相似的模式，并按我们的`sold_at`列分组...*

```
*`SELECT
  sold_at,
  COUNT(*) AS sales_per_day
FROM sales
GROUP BY sold_at
ORDER BY sold_at;`* 
```

*...我们可能希望每个小组都有独特的一天，但是我们看到的是这样的情况:*

```
 *`sold_at           | sales_per_day
----------------------------+---------------
 2020-08-29 08:42:33.085995 |             1
 2020-08-29 09:42:33.085995 |             1
 2020-08-30 08:42:33.085995 |             1
 2020-08-30 09:42:33.085995 |             1
 2020-08-31 09:42:33.085995 |             1
 2020-09-01 07:42:33.085995 |             1
 2020-09-01 08:42:33.085995 |             1
 2020-09-01 09:42:33.085995 |             1
(8 rows)`* 
```

*看起来我们的数据根本没有分组——我们分别得到每一行。*

*但是，我们的数据*实际上是分组的！*问题是每行的`sold_at`是一个唯一的值——所以每行都有自己的组！*

*`GROUP BY`工作正常，但这不是我们想要的输出。*

*罪魁祸首是时间戳唯一的小时/分钟/秒信息。*

*每个时间戳都有小时、分钟或秒的差异，因此它们都被放在各自的组中。*

*我们需要将这些日期和时间值转换成一个日期:*

*   *`2020-09-01 08:42:33.085995`=>= *
*   *`2020-09-01 09:42:33.085995`=>= *

*转换为日期后，同一天的所有时间戳将返回相同的日期值，因此将被放入同一组。*

*为此，我们将把`sold_at`时间戳值转换为一个日期:*

```
*`SELECT
  sold_at::DATE AS date,
  COUNT(*) AS sales_per_day
FROM sales
GROUP BY sold_at::DATE
ORDER BY sold_at::DATE;`* 
```

*在我们的`GROUP BY`子句中，我们使用`::DATE`将时间戳部分截短为“日”这实际上去掉了时间戳的小时/分钟/秒，只返回日期。*

*在我们的`SELECT`中，我们也返回同样的表达式，并给它一个别名来美化输出。*

*出于同样的原因，如果不对其分组或对其执行某种聚合，我们就不能返回`product`，数据库不会让我们只返回`sold_at`—`SELECT`中的所有内容要么在`GROUP BY`中，要么在结果组上进行某种聚合。*

*结果是我们最初想要看到的每天的*销售额:**

```
 *`date    | sales_per_day
------------+---------------
 2020-08-29 |             2
 2020-08-30 |             2
 2020-08-31 |             1
 2020-09-01 |             3
(4 rows)`* 
```

## *用`HAVING`过滤组*

*接下来，让我们看看如何过滤分组后的行。*

*要做到这一点，让我们试着找出有超过一次销售的日子。*

*如果没有分组，我们通常会使用一个`WHERE`子句来过滤我们的行。例如:*

```
*`SELECT *
FROM sales
WHERE product = 'Coffee';`* 
```

*对于我们的组，我们可能想做类似这样的事情来根据行数过滤我们的组...*

```
*`SELECT
  sold_at::DATE AS date,
  COUNT(*) AS sales_per_day
FROM sales
WHERE COUNT(*) > 1      -- filter the groups?
GROUP BY sold_at::DATE;`* 
```

*不幸的是，这不起作用，我们收到这个错误:*

*`ERROR:  aggregate functions are not allowed in WHERE`*

*在`WHERE`子句中不允许使用聚合函数，因为`WHERE`子句在和`GROUP BY`子句之前**被求值——还没有对任何组执行计算。***

*但是，有一种类型的子句允许我们过滤、执行聚合，它在子句之后`GROUP BY`被求值`HAVING`子句。*

***`HAVING`条款就像是你们团队的`WHERE`条款。***

*为了找到有不止一笔销售的日子，我们可以添加一个`HAVING`子句来检查组中的行数:*

```
*`SELECT
  sold_at::DATE AS date,
  COUNT(*) AS sales_per_day
FROM sales
GROUP BY sold_at::DATE
HAVING COUNT(*) > 1;`* 
```

*这个`HAVING`子句过滤掉该组中行数不大于 1 的所有行，我们在结果集中看到:*

```
 *`date    | sales_per_day
------------+---------------
 2020-09-01 |             3
 2020-08-29 |             2
 2020-08-30 |             2
(3 rows)`* 
```

*为了完整起见，下面是 SQL 语句所有部分的执行顺序:*

*   *`FROM` —从`FROM`表中检索所有行*
*   *`JOIN` —执行任何连接*
*   *`WHERE` —过滤行*
*   *`GROUP BY` -表格组*
*   *`HAVING` -过滤组*
*   *`SELECT` -选择要返回的数据*
*   *`ORDER BY` -对输出行进行排序*
*   *`LIMIT` -返回一定数量的行*

## *隐式分组的聚合*

*我们要看的最后一个主题是可以在没有`GROUP BY`的情况下执行的聚合，或者更好地说，它们有一个*隐式* *分组。**

*这些聚合在您希望从表中查找某个特定聚合的情况下非常有用，例如总收入或某列的最大值或最小值。*

*例如，我们可以通过从整个表中选择总和来计算所有地点的总收入:*

```
*`SELECT SUM(price)
FROM sales;`* 
```

```
 *`sum
-----
  19
(1 row)`* 
```

*到目前为止，我们已经在所有地点完成了 19 美元的销售额(*万岁！*)。*

*我们可以查询的另一个有用的东西是某物的第一个或最后一个的*。**

*例如，我们第一次销售的日期是哪一天？*

*为了找到这一点，我们只需使用`MIN()`函数:*

```
*`SELECT MIN(sold_at)::DATE AS first_sale
FROM sales;`* 
```

```
 *`first_sale
------------
 2020-08-29
(1 row)`* 
```

*(要查找最后一次销售的日期，只需将`MIN()`替换为`MAX()`。)*

### *使用`MIN` / `MAX`*

*虽然这些简单的查询作为独立的查询很有用，但它们通常是大型查询的过滤器的一部分。*

*例如，让我们试着找出我们销售的最后一天的总销售额。*

*我们可以这样编写查询:*

```
*`SELECT
  SUM(price)
FROM sales
WHERE sold_at::DATE = '2020-09-01';`* 
```

*这个查询可以工作，但是我们显然已经硬编码了`2020-09-01`的日期。*

**09/01/2020* 可能是我们销售的最后一天，但不会永远是这个日子。我们需要一个动态的解决方案。*

*这可以通过将该查询与子查询中的`MAX()`函数相结合来实现:*

```
*`SELECT
  SUM(price)
FROM sales
WHERE sold_at::DATE = (
  SELECT MAX(sold_at::DATE)
  FROM sales
);`* 
```

*在我们的`WHERE`子句中，我们使用子查询`SELECT MAX(sold_at::DATE) FROM sales`找到表中最大的日期。*

*然后，我们使用这个最大日期作为筛选表的值，并对每笔销售的价格求和。*

### *隐式分组*

*我之所以说这些是隐式分组，是因为如果我们试图选择一个带有非聚合列的聚合值，就像这样...*

```
*`SELECT
  SUM(price),
  location
FROM sales;`* 
```

*...我们会犯常见的错误:*

```
*`ERROR:  column "sales.location" must appear in the GROUP BY clause or be used in an aggregate function`* 
```

## *`GROUP BY`是工具*

*与软件开发中的许多其他主题一样，`GROUP BY`是一个工具。*

*使用`GROUP BY`、聚合函数或其他工具(如`DISTINCT`、`ORDER BY`和`LIMIT`)的组合，有许多方法可以编写和重写这些查询。*

*理解和使用`GROUP BY`需要一点点练习，但是一旦你记下了，你会发现一批全新的问题现在对你来说是可以解决的！*

*如果你喜欢这篇文章，你可以[在 twitter](https://twitter.com/johnmosesman) 上关注我，我在那里谈论数据库的事情以及如何在开发人员的职业生涯中取得成功。*

*感谢阅读！*

*约翰*