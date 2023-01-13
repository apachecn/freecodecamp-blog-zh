# SQL 视图解释-如何在 SQL 和 MySQL 中创建视图

> 原文：<https://www.freecodecamp.org/news/sql-create-view-mysql/>

### SQL 中的视图是什么？

视图是一个数据库对象，它显示一个或多个表中的数据。视图的使用方式与表类似，但它们不包含任何数据。它们只是“指向”存在于别处的数据(例如，表或视图)。

### 我们为什么喜欢他们？

*   视图是限制显示数据的一种方式。例如，人力资源部门的数据经过过滤，只显示敏感信息。在这种情况下，敏感信息可能是社会保险号、员工性别、工资率、家庭住址等。
*   跨越多个表的复杂数据可以合并到一个“视图”中这可以让您的业务分析师和程序员的生活更加轻松。

### 重要的安全提示

*   视图由系统管理。当相关表中的数据发生更改、添加或更新时，系统会更新视图。我们希望仅在需要管理系统资源的使用时使用它们。
*   在 MySQL 中，创建视图后对表设计的更改(即新的或删除的列)不会在视图本身中更新。必须更新或重新创建视图。
*   视图是四种标准数据库对象类型之一。其他的是表、存储过程和函数。
*   通常可以像对待表一样对待视图，但是当视图包含多个表时，更新会受到限制或者不可用。
*   还有许多关于视图的其他细节超出了本入门指南的范围。花时间阅读您的数据库管理器手册，享受这个强大的 SQL 对象带来的乐趣。

### Create View 语句(MySQL)的语法

```
CREATE
    [OR REPLACE]
    [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
    [DEFINER = { user | CURRENT_USER }]
    [SQL SECURITY { DEFINER | INVOKER }]
    VIEW view_name [(column_list)]
    AS select_statement
	[WITH [CASCADED | LOCAL] CHECK OPTION] 
```

*本指南将涵盖陈述的这一部分……*

```
CREATE
    VIEW view_name [(column_list)]
    AS select_statement 
```

### 从学生表创建视图的示例

注意事项:

*   视图的名称末尾有一个“v”。建议视图名称表明它是一个视图，以使程序员和数据库管理员的工作更容易。您的 IT 部门应该有自己的对象命名规则。
*   视图中的列受 SELECT 限制，数据行受 WHERE 子句限制。
*   视图名称两边需要有“`”字符，因为名称中有“-”。如果没有它们，MySQL 会报告一个错误。

```
create view `programming-students-v` as
select FullName, programOfStudy 
from student 
where programOfStudy = 'Programming';

select * from `programming-students-v`; 
```

### 使用视图合并多个表中的数据的示例

数据库中添加了一个学生人口统计表来演示这种用法。该视图将组合这些表。

注意事项:

*   要“连接”表，表必须有共同的字段(通常是主键)，这些字段唯一地标识每一行。在这种情况下，它是学生证。(在 [SQL Joins](https://guide.freecodecamp.org/sql/sql-joins/index.md) 指南中有更多相关信息。)
*   注意每个表的“别名”(s 代表学生，sc 代表学生联系人)。这是一个缩短表名的工具，可以更容易地识别正在使用的表。这比重复输入长表名要容易得多。在本例中，这是必需的，因为 studentID 在两个表中是相同的列名，并且系统将显示“不明确的列名错误”,而不指定使用哪个表。