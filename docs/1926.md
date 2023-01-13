# SQL 聚合函数——为初学者提供示例数据查询

> 原文：<https://www.freecodecamp.org/news/sql-aggregate-functions-with-example-data-queries-for-beginners/>

## 什么是聚合函数？

如果您使用过电子表格，那么 SQL 聚合函数将会非常熟悉。

你在 Google Sheets 或者 Excel 中用过`SUM`吗？SQL 中也存在`SUM`函数，它被称为*聚合函数。*

聚合函数跨数据库行执行特定的任务。例如，假设你组织了一次年度筹款活动。你有一个捐赠者的数据库以及他们每年捐赠的金额。

您可以使用`COUNT`函数来确定收到了多少捐款，或者使用`SUM`函数来确定您今年总共捐了多少。

为了便于说明，我们将使用下面的小数据集。

| 名字 | 电子邮件 | 捐赠 _2020 | 捐赠 _2021 |
| --- | --- | --- | --- |
| 安德鲁·琼斯 | [ajons @ someemail . com](mailto:ajones@someemail.com) | four hundred | Five hundred |
| 玛丽亚·罗德里格斯 | [maria77@someemail.com](mailto:maria77@someemail.com) | One thousand | Three hundred and fifty |
| 格里·福特 | 空 | Twenty-five | Twenty-five |
| Isabella Munn | [isamun91@someemail.com](mailto:isamun91@someemail.com) | Two hundred and fifty | 空 |
| 詹妮弗·沃德 | [jjw1972@someemail.com](mailto:jjw1972@someemail.com) | Two thousand | Two thousand three hundred |
| 罗文·帕克 | 空 | Five thousand | Four thousand |

在本文中，我们将介绍以下聚合函数:`COUNT`、`SUM`、`MIN/MAX`和`AVG`。

## 计数功能

`COUNT`函数返回行数。最简单的形式是，`COUNT`计算表中的总行数。

要从我们的提供者表中获取该值，您可以运行下面的查询:`SELECT COUNT(*) FROM donors`。这将返回捐赠者的总数，在本例中是 6。当然，在这个例子中，你可以只计算行数，但是让我们想象有更多的行。

但是，您可能只想对某些行进行计数。在我们的捐赠者数据库示例中，假设您想要计算列出了电子邮件的捐赠者的数量。

如果运行查询`SELECT COUNT(email) FROM donors`，将得到 4，这是 email 列中具有非空值的捐赠者总数。

请记住，除非使用别名，否则返回的列将简单地标记为“count”。如果您想要一个更具描述性的名称，使用`AS`运行一个查询来创建一个别名，例如`SELECT COUNT(email) FROM donors AS email_count`。

## 求和函数

SUM 是一个非常方便的聚合函数，可以用来将不同行的数值相加。

在我们的捐赠数据库中，您可以通过运行查询`SELECT SUM(donation_2021) FROM donors`，使用`SUM`将所有 2021 笔捐赠加起来。注意，`SUM`忽略了`NULL`的值，所以这个查询的结果将是 7175。

请记住，`SUM`像其他聚合函数一样，跨行工作，而不是跨列工作。

所以在我们的例子中，你可以用它把 2021 年以来每个人的捐款加在一起(由`donation_2021`列表示)，但不能把一个人从`donation_2020`列和`donation_2021`列的所有捐款加在一起。

## 最小值和最大值函数

正如您可能猜到的，您可以使用 MIN 和 MAX 来查找数据库中特定列的最小值和最大值。

在我们的示例数据中，假设您想要查找 2021 年的最低和最高捐赠金额。您可以通过运行查询:`SELECT MIN(donation_2021) AS "Minimum donation 2021", MAX(donation_2021) AS "Maximum donation 2021" FROM donors`来做到这一点。

请注意，在本例中，我们使用引号将别名分配给返回的列。如果您的别名中没有空格，则不需要引号，但是我们在这里使用引号，所以我们可以使用空格。

一个有趣的旁注:您可以在非数值上使用`MIN`和`MAX`。`MIN`会找到最小的数字，最接近 A 的字母，或者最早的日期。`MAX`会找到最大的数字，最接近 Z 的字母，或者最晚的日期。非常得心应手！

## AVG 函数

最后，`AVG`函数对特定列的数值进行平均。与`SUM`一样，它忽略`NULL`值。

要获得 2020 年以来所有捐赠的平均值，可以运行以下查询:`SELECT AVG(donation_2020) FROM donors`。该查询的结果将是 1435。

## 包扎

如您所见，聚合函数是分析 SQL 中数据的简单而有用的工具。`AVG`、`MIN/MAX`、`SUM`和`COUNT`是您 SQL 技能的重要补充。