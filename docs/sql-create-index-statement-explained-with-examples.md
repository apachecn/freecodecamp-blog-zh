# SQL Create Index 语句举例说明

> 原文：<https://www.freecodecamp.org/news/sql-create-index-statement-explained-with-examples/>

该语句用于在现有表中的列上创建“索引”。

索引要点:

*   它们用于提高数据搜索的效率，在连接表时以特定的顺序显示数据(参见[语句](https://www.freecodecamp.org/news/the-ultimate-guide-to-sql-join-statements/)的`JOIN`最终指南】等等)。
*   索引是一个“系统”对象，这意味着它由数据库管理器使用。
*   这种用法的一部分是当索引使用的数据在相关表中发生变化时，数据库管理器更新索引。请记住这一点，因为随着数据库中索引数量的增加，整体系统性能可能会受到影响。
*   如果您发现 SQL 在给定的一个或多个表上运行缓慢，那么首先要考虑创建一个索引来纠正这个问题。

下面是一个`create index`语句的语法示例。请注意，语法允许一个索引包含多个列:

```
CREATE INDEX index_name
ON table_name (column1, column2, ...);
```

要在学生表的字段`programOfStudy`上创建新索引，请使用以下语句:

下面是创建索引的语句:

```
create index pStudyIndex
on student (programOfStudy);
```

在 MySQL 中，您使用`ALTER TABLE`命令来更改和删除索引。MySQL Workbench 还提供 GUI 工具来管理索引。

但这只是触及表面。查看您选择的数据库管理器的文档，并亲自尝试不同的选项。