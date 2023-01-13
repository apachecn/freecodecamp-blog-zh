# 举例说明 SQL 主键约束

> 原文：<https://www.freecodecamp.org/news/the-sql-primary-key-constraint-explained/>

主键是唯一标识表中每一行的一列或一组列。

它被称为“约束”，因为它导致系统限制这些列中允许的数据。在这种情况下…

*   包含数据(非空)
*   与表中的所有其他行不同。
*   每个表只能有一个主键

主键主要用于维护每行的数据完整性。

它还允许系统和应用程序确保正确地读取、更新和连接数据。

### 创建表的示例

下面是一个 create table 命令，它也将使用两个字段创建一个主键。

```
CREATE TABLE priKeyExample(
rcdKey_id_a INT NOT NULL,
rcdKeySeq_id INT NOT NULL,
someData varchar(256) NOT NULL,
PRIMARY KEY(rcdKey_id_a,rcdKeySeq_id)); 
```

### alter table 示例

必须先删除现有的

```
DROP INDEX `primary` ON priKeyExample; 
```

现在我们将添加新的。

```
ALTER TABLE priKeyExample 
ADD CONSTRAINT myPriKey PRIMARY KEY(rcdKey_id_a,rcdKeySeq_id); 
```

与所有这些 SQL 内容一样，它们比本入门指南中的内容要多得多。

我希望这至少能给你足够的时间开始。

请参阅数据库管理器的手册，并亲自尝试不同的选项。