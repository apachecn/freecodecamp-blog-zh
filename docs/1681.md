# SQL Left Join–示例 Join 语句语法

> 原文：<https://www.freecodecamp.org/news/sql-left-join-example-join-statement-syntax/>

在关系数据库中，表之间的关系通常是这样的，即它们的信息在整个数据库中只需写入一次。然后，当您需要分析数据时，您需要组合来自那些相关表的信息。

要在 SQL 中做到这一点，可以使用`JOIN`语句。`LEFT JOIN`语句是可用的各种`JOIN`语句中的[之一。当您使用它来连接两个表时，它会保留第一个表(左边的表)的所有行，即使第二个表上没有相应的匹配。](https://www.freecodecamp.org/news/sql-join-types-inner-join-vs-outer-join-example/)

您可以在一个`SELECT`查询中使用`JOIN`来连接两个表`table_1`和`table_2`，如下所示:

```
SELECT columns
FROM table_1
LEFT OUTER JOIN table_2
ON relation;
```

```
SELECT columns
FROM table_1
LEFT JOIN table_2
ON relation;
```

首先，编写哪些列将出现在连接的表中。通过在列名前面加上表名，可以指定该列属于哪个表。如果一些列与`SELECT <columns>`同名(如`table_1.column_1`和`table_2.column_1`),这是必要的。

然后，您将把第一个表的名称写成`FROM table_1`。

之后，您可以将第二个表的名称写成`LEFT OUTER JOIN table_2`或`LEFT JOIN table_2`(省略`OUTER`关键字)。

最后，你应该写下用来匹配行的关系，例如`ON table_1.column_A = table_2.column_B`。通常，这种关系是通过 id 建立的，但是它可以与任何列建立关系。

# SQL 左连接示例

假设您有一个图书数据库，其中有两个表，一个包含图书，另一个包含作者。为了避免重复每本书的所有作者信息，该信息在它自己的表中，并且这些书只有`author_name`列。

| 图书 id | 标题 | 作者姓名 | 公共年份 |
| --- | --- | --- | --- |
| one | 一个，没人，十万 | 路易吉·皮兰德娄 | One thousand nine hundred and twenty-six |
| Two | 一半的子爵 | 伊塔洛·卡尔维诺 | One thousand nine hundred and fifty-two |
| three | 蒙巴萨姆老虎 | 埃米利奥·萨尔加里 | One thousand nine hundred |
| four | 在猫头鹰日 | 莱昂纳多·西亚卡西亚 | One thousand nine hundred and sixty-one |
| five | 每一个人 | 莱昂纳多·西亚卡西亚 | One thousand nine hundred and sixty-six |
| six | Il fu Mattia Pascial | 路易吉·皮兰德娄 | One thousand nine hundred and four |
| seven | 我的意思是，我的意思是，我的意思是，我的意思是，我的意思是，我的意思是，我的意思是，我的意思是，我的意思是，我的意思是，我的意思是 | 约翰·韦尔加 | One thousand eight hundred and eighty-one |

| 作者 id | 名字 | 出生年份 | 出生地 | 琐事 |
| --- | --- | --- | --- | --- |
| one | 路易吉·皮兰德娄 | One thousand eight hundred and sixty-seven | -真不错 | 1934 年诺贝尔文学奖 |
| Two | 约翰·韦尔加 | One thousand eight hundred and forty | 维兹尼 | 从 1920 年到 1922 年是意大利王国的参议员 |
| three | 伊塔洛·斯韦沃 | One thousand eight hundred and sixty-one | 的里雅斯特 | 真名是阿伦·赫克托·施密茨 |
| four | 塞萨尔·帕维斯 | One thousand nine hundred and eight | 圣斯特凡诺贝尔博 | 空 |
| five | 朱塞佩·托马西·迪·兰佩杜萨 | One thousand eight hundred and ninety-six | 巴勒莫 | 从 1934 年到 1957 年是兰佩杜萨的王子 |

我们可以根据作者的名字连接这两个表。使用`books`表作为左表，您可以编写以下代码来连接它们:

```
SELECT books.title AS book_title, books.publ_year, books.author_name, authors.year_of_birth, authors.place_of_birth
   FROM books
   LEFT JOIN authors
   ON books.author_name = authors.name
;
```

我们来分解一下。

在第一行中，您选择在最终表格中显示哪些列。它也是决定在使用`AS`的结果表中某些列是否有不同名称的地方，就像使用`books.title AS book_title`一样。

第二行`FROM books`表示要考虑的第一个表，也称为左表。

然后第三行，`LEFT JOIN authors`，表示要考虑哪个表。

`ON books.author_name = authors.name`表示使用行`books.author_name`和`authors.name`来匹配表格。

在这个查询之后，您将得到如下的表，其中没有从 authors 表中获得信息的行只显示`NULL`。

| 图书 _ 标题 | 公共年份 | 作者姓名 | 出生年份 | 出生地 |
| --- | --- | --- | --- | --- |
| 一个，没人，十万 | One thousand nine hundred and twenty-six | 路易吉·皮兰德娄 | One thousand eight hundred and sixty-seven | -真不错 |
| 一半的子爵 | One thousand nine hundred and fifty-two | 伊塔洛·卡尔维诺 | 空 | 空 |
| 蒙巴萨姆老虎 | One thousand nine hundred | 埃米利奥·萨尔加里 | 空 | 空 |
| 在猫头鹰日 | One thousand nine hundred and sixty-one | 莱昂纳多·西亚卡西亚 | 空 | 空 |
| 每一个人 | One thousand nine hundred and sixty-six | 莱昂纳多·西亚卡西亚 | 空 | 空 |
| 伊尔富·马蒂亚·帕斯卡尔 | One thousand nine hundred and four | 路易吉·皮兰德娄 | One thousand eight hundred and sixty-seven | -真不错 |
| 我的意思是，我的意思是，我的意思是，我的意思是，我的意思是，我的意思是，我的意思是，我的意思是，我的意思是，我的意思是，我的意思是 | One thousand eight hundred and eighty-one | 约翰·韦尔加 | One thousand eight hundred and forty | 维兹尼 |

注意，不在`books`表中的作者不在这个连接表中。这是因为，正如我之前所说的，只保留了左边表中不相关的行(在本例中是`books`)，而不是右边/第二个表中的那些行。

## 更复杂的左连接示例

让我们看看另一种方法，您可以将`JOIN`与其他 SQL 特性一起使用来进行一些数据分析。

您可能想要查看每个作者有多少本书出现在数据库中。您可以使用下面的查询来做到这一点:

```
SELECT authors.name AS author_name,
    SUM(
      CASE
        WHEN books.title LIKE '%'
          THEN 1
        ELSE 0
      END
    ) as number_of_books
  FROM authors
  LEFT JOIN books
  ON books.author_name = authors.name
  GROUP BY authors.name
  ORDER BY number_of_books DESC
; 
```

#### 代码分解

第 1 行:使用`SELECT`在结果表中列出您想要的列。

第 2 行: [`SUM`是一个聚合函数](https://www.freecodecamp.org/news/sql-group-by-clauses-explained/#aggregations-count-sum-avg-)，与 GROUP BY 一起使用。然后对组合在一起的行的值求和。

第 3-7 行:使用 [CASE 语句](https://www.freecodecamp.org/news/case-statement-in-sql-example-query/)根据条件得到不同的结果。在这种情况下，如果一行包含书名，则该行计为 1，否则计为 0。这里我们使用`LIKE`来检查单元格是否包含任何字符(在这篇关于包含字符串的[文章中了解更多)。](https://www.freecodecamp.org/news/sql-contains-string-sql-regex-example-query)

第 8 行:这给为 SUM 创建的列命名为`number_of_books`。

第 9 行:本例中左边/第一张表是`authors`。

第 10 行:本例中的右/第二个表是`books`。

第 11 行:这使用作者姓名连接了两个表。

第 12 行:行是按照作者姓名分组的[——在该列中具有相同值的所有行将由一个单独的行表示。](https://www.freecodecamp.org/news/sql-group-by-clauses-explained/)

第 13 行:我们使用 [order by](https://www.freecodecamp.org/news/sql-order-by-statement-example-sytax/) 按照书籍数量降序排列。

该查询将为您提供下表。注意，您在这里只能看到出现在`authors`表中的作者。在`books`表中提到的作者在`authors`表中没有条目，这里不列出。这是因为没有保留`books`表中不相关的行。

| 作者姓名 | 书籍数量 |
| --- | --- |
| 路易吉·皮兰德娄 | Two |
| 约翰·韦尔加 | one |
| 塞萨尔·帕维斯 | Zero |
| 朱塞佩·托马西·迪·兰佩杜萨 | Zero |
| 伊塔洛·斯韦沃 | Zero |

如果`authors`表被更新为包含`books`表中提到的所有作者，如下所示:

| 作者 id | 名字 | 出生年份 | 出生地 | 琐事 |
| --- | --- | --- | --- | --- |
| one | 路易吉·皮兰德娄 | One thousand eight hundred and sixty-seven | -真不错 | 1934 年诺贝尔文学奖 |
| Two | 约翰·韦尔加 | One thousand eight hundred and forty | 维兹尼 | 从 1920 年到 1922 年是意大利王国的参议员 |
| three | 伊塔洛·斯韦沃 | One thousand eight hundred and sixty-one | 的里雅斯特 | 真名是阿伦·赫克托·施密茨 |
| four | 塞萨尔·帕维斯 | One thousand nine hundred and eight | 圣斯特凡诺贝尔博 | 空 |
| five | 朱塞佩·托马西·迪·兰佩杜萨 | One thousand eight hundred and ninety-six | 巴勒莫 | 从 1934 年到 1957 年是兰佩杜萨的王子 |
| six | 伊塔洛·卡尔维诺 | One thousand nine hundred and twenty-three | 圣地亚哥-德-拉斯维加斯 | 空 |
| seven | 埃米利奥·萨尔加里 | One thousand eight hundred and sixty-two | 维罗纳 | 空 |
| eight | 莱昂纳多·西亚卡西亚 | One thousand nine hundred and twenty-one | 拉麦托 | 空 |

那么上面查询中的表实际上会给出所有作者的图书数量。

| 作者姓名 | 书籍数量 |
| --- | --- |
| 莱昂纳多·西亚卡西亚 | Two |
| 路易吉·皮兰德娄 | Two |
| 埃米利奥·萨尔加里 | one |
| 约翰·韦尔加 | one |
| 约翰·韦尔加 | one |
| 塞萨尔·帕维斯 | Zero |
| 朱塞佩·托马西·迪·兰佩杜萨 | Zero |
| 伊塔洛·斯韦沃 | Zero |

# 结论

在关系数据库中，数据应该只写一次，所以我们经常会得到多个彼此相关的表。当我们需要分析数据和连接来自不同表格的信息时,`LEFT JOIN`是一个非常有用的助手。享受使用这个强大的工具查询您的数据库。