# 如何使用 SQL CASE 语句–带示例挑战

> 原文：<https://www.freecodecamp.org/news/sql-case-statement/>

编写具有多个条件的 SQL 可能是一项艰巨的任务，尤其是在需要进行大量检查的情况下。

例如，`if () else if () else {}` check case 表达式处理所有 SQL 条件。如果满足第一个条件，查询将停止执行并返回一个值。如果不满足任何条件，则返回 else 中指定的值。

在本文中，我们将讨论:

1.  什么是 SQL CASE 语句及其工作原理
2.  如何使用 SQL CASE 语句解决一个练习
3.  一些重要术语的含义，如 order by、limit、offset、left join 和 alias。

## SQL CASE 语句解释

在编程中，当您有一组给定的条件时，您最终会使用条件(`switch`或`if else`)来知道当条件满足时执行哪个代码块。

对于 SQL，您可以使用 CASE 语句来实现这一点。使用 CASE 关键字和 WHEN 子句来执行条件语句代码块。使用 THEN 语句返回表达式的结果。如果这些条件都不满足，那么使用最后一个 ELSE 子句返回回退结果。

**SQL CASE 语句的语法如下**:

```
CASE
    WHEN conditional_statement1 THEN result1
    .
    .
    .
    .
    WHEN condition_statementN THEN resultN

    ELSE result
END;
```

SQL case syntax

当您使用 CASE 语句时，如果满足第一个条件，它必须后跟 When 和 result。如果第一个条件不满足，它继续检查其他条件，直到第 n 个(或最终)条件。如果仍然不满足，则执行 ELSE 条件。

此外，在使用 CASE 语句时，ELSE 部分是可选的。在不使用它的情况下，查询结果返回 NULL。

## SQL 挑战

在本节中，我们将通过一个真实场景的案例研究来帮助您学习如何使用 case 语句解决 SQL 挑战。

这个挑战是我在 Coderbyte 上遇到的，这是一个练习编码挑战的平台。这有点棘手，我将分解本文中涉及的一步一步的过程。

### 挑战是什么？

挑战包括提出一个 SQL 查询，从一个表中返回工资第三高的雇员。

您需要构建一个查询来查找该雇员并返回该行。您还必须用 company_divisions 表中相应的 DivisionName 替换 DivisionID 列的位置。如果 ID 存在于表中并且不为空，那么您需要用 ManagerName 替换 ManagerID 列。

### 在这个挑战中，SQL CASE 语句解决了什么问题？

在这项挑战中，我们需要案例陈述来帮助实现以下目标:

1.  请确保 MangerID 不为空。
2.  将 company ManagerID 与 company ID 进行匹配，并将名称作为 ManagerName 返回。
3.  确保如果没有返回姓名，则将姓名 Susan Wall 用作默认的经理姓名。

下面是预期的输出:

| 身份证明 | 名字 | 分部名称 | 经理姓名 | 薪水 |
| Two hundred and twenty-two | 标记红色 | 销售 | 苏珊·沃尔 | Eighty-six thousand |

这里是解决这一挑战所需的数据:

#### 表 1:公司

| 身份证明 | 名字 | DivisionID | ManagerID | 薪水 |
| Three hundred and fifty-six | 丹尼尔·史密斯 | One hundred | One hundred and thirty-three | forty thousand |
| One hundred and twenty-two | 阿诺德·萨利 | One hundred and one | 空 | Sixty thousand |
| Four hundred and sixty-seven | 丽莎·罗伯茨 | One hundred | 空 | Eighty thousand |
| One hundred and twelve | 玛丽·戴尔 | One hundred and five | Four hundred and sixty-seven | Sixty-five thousand |
| Seven hundred and seventy-five | 丹尼斯前线 | One hundred and three | 空 | Ninety thousand |
| One hundred and eleven | 拉里·韦斯 | One hundred and four | Thirty-five thousand five hundred and thirty-four | Seventy-five thousand |
| Two hundred and twenty-two | 标记红色 | One hundred and two | One hundred and thirty-three | Eighty-six thousand |
| Five hundred and seventy-seven | 罗伯特·尼日尔 | One hundred and five | Twelve thousand three hundred and fifty-three | Seventy-six thousand |
| One hundred and thirty-three | 苏珊·沃尔 | One hundred and five | Five hundred and seventy-seven | One hundred and ten thousand |

#### 表 2:公司部门

| 身份证明 |  | 分部名称 |
| One hundred |  | 会计 |
| One hundred and one |  | 信息技术 |
| One hundred and two |  | 销售 |
| One hundred and three |  | 营销 |
| One hundred and four |  | 工程 |
| One hundred and five |  | 客户支持 |

## 如何解决 SQL CASE 语句挑战

在本节中，我们将了解解决这一挑战的逐步过程。

### 第一步:拿到第三高的薪水

首先，您需要构造一个查询来返回第三高的薪水。您将通过从 company 表中选择并按薪水排序来实现这一点(因为我们对薪水第三高的记录感兴趣)。

你可以这样做:

```
SELECT *

FROM company

ORDER BY salary DESC limit 1 offset 2;
```

Select third highest salary

如预期的那样，该查询返回工资第三高的雇员行。

| 身份证明 | 名字 | DivisionID | ManagerID | 薪水 |
| Two hundred and twenty-two | 标记红色 | One hundred and two | One hundred and thirty-three | Eighty-six thousand |

那么这个查询中发生了什么呢？

`SELECT`:使用带有星号(*)(也称为通配符)的 SELECT 命令从 **company** 表中检索所有列。

`ORDER BY`:ORDER BY 命令按升序或降序对列进行排序。SQL 默认按升序排序( **ASC** )，但是我们将按降序排序 salary 列( **DESC** )。这是因为我们需要从最高到最低的 desc 工资，即 11 万- 4 万。

`limit`:limit 命令根据限值的设定值限制返回的记录数。因为我们只对一行感兴趣，所以我们将查询中的限制设置为 1。这确保了每次执行该查询时，我们将获得单个记录的返回值。

`offset`:在这里使用 offset 子句可以帮助您指定在从查询中实际返回行之前要跳过的行数。Offset 让我们跳过两个收入最高的行(Susan Wall 和 Dennis Front)，返回收入第三高的行(Mark Red)。

### 步骤 2:用 DivisionName 替换 DivisionID

现在，您需要修改查询，只选择您需要的列——ID、Name、ManagerID、DivisionName 和 Salary。然后，您需要用表 **company_divisions** 中相应的 DivisionName 替换 DivisionID 列。

你可以这样做:

```
SELECT c.ID, c.Name, c.ManagerID, c.salary, cd.DivisionName

FROM company as c

LEFT JOIN company_divisions as cd ON c.DivisionId = cd.id

ORDER BY salary DESC limit 1 offset 2;
```

Select columns and join with company division

以下是输出结果:

| 身份证明 | 名字 | 分部名称 | ManagerID | 薪水 |
| Two hundred and twenty-two | 标记红色 | 销售 | One hundred and thirty-three | Eighty-six thousand |

让我们讨论一下上面的查询中发生了什么:

`LEFT JOIN` : 由于记录是从左侧(公司)返回的，我们将使用`company_division.id and company.DivisionID`在右侧(公司 _ 分部)使用左连接来匹配它们。

如果找到匹配的记录，即公司的 id 也存在于 company division 中，那么 DivisionName 列将填充左连接中的实际值，在我们的示例中为(Sales)。如果没有记录，则不返回任何内容。

`as`(别名):使用的别名是表的临时名称。因此，我们可以将公司的别名定义为 c.name，而不是 company.name。

### 第 3 步:用 ManagerName 替换 ManagerID

我们将基于步骤 2 的查询结果。我们将使用我们所学的 CASE 语句来添加 ManagerId 不为 null 时的条件，并检查 ManagerId 是否也存在。

我们要做的第一件事是检查公司。ManagerID 不为 null，请确保表中存在该 ID。我们将在这里应用 CASE 语句。

```
CASE WHEN c.ManagerID IS NOT NULL 

AND c.ManagerID = c.ID 
```

First Step Case Statement

CASE 语句的第二部分是用 ManagerName 替换 ManagerID 列。然后我们需要像这样使用我们之前学过的 Then 块:

```
CASE WHEN c.ManagerID IS NOT NULL 

AND c.ManagerID = c.ID

THEN Name ELSE 'Susan Wall' END AS 'ManagerName'
```

Complete Case Statement for challenge

最后，我们现在可以将 CASE 块包含到步骤 2 中已经存在的代码片段中。这看起来有点像这样:

```
SELECT c.ID, c.Name, c.salary, cd.DivisionName

CASE WHEN c.ManagerID IS NOT NULL 

AND c.ManagerID = c.ID

THEN Name ELSE 'Susan Wall' END AS 'ManagerName'

FROM company as c

LEFT JOIN company_divisions as cd ON c.DivisionId = cd.id

ORDER BY salary DESC limit 1 offset 2;
```

Sql case with select and join

第 3 步的结果是预期的输出-工资第三高的员工。

| 身份证明 | 名字 | 部名 | 经理姓名 | 薪水 |
| Two hundred and twenty-two | 标记红色 | 销售 | 苏珊·沃尔 | Eighty-six thousand |

## **结束**

在本文中，我希望您了解了 SQL 中的 CASE 语句，以及如何使用 CASE 处理现实世界中的问题。

您还学习了其他 SQL 命令，如 SELECT、ORDER BY、LIMIT、OFFSET、LEFT JOIN 和 ALIAS。