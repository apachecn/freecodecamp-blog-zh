# 举例说明 SQL Select Into 语句

> 原文：<https://www.freecodecamp.org/news/sql-select-into-statement-explained-with-examples/>

## SQL Select Into 语句做什么？

`SELECT INTO`语句是一个查询，允许您创建一个*新的*表，并用一个`SELECT statement`的结果集填充它。

要将数据添加到现有表中，请参见 [INSERT INTO](https://guide.freecodecamp.org/sql/sql-select-into-statement/guides/src/pages/sql/sql-insert-into-select-statement/index.md) 语句。

`SELECT INTO`可用于将多个表格或视图中的数据合并到一个新表格中。1 原始表格不受影响。

一般语法是:

```
SELECT column-names
  INTO new-table-name
  FROM table-name
 WHERE EXISTS 
      (SELECT column-name
         FROM table-name
        WHERE condition) 
```

本例显示了一个从“供应商”表“复制”到一个名为 SupplierUSA 的新表中的表集，该表包含与列 country 值“USA”相关的表集。

```
SELECT * INTO SupplierUSA
  FROM Supplier
 WHERE Country = 'USA'; 
```

**结果** : 4 行受影响 2

| 身份证明 | 公司名称 | 联系人姓名 | 城市 | 国家 | 电话 |
| --- | --- | --- | --- | --- | --- |
| Two | 新奥尔良凯郡美食 | 谢莉·伯克 | 新奥尔良 | 美利坚合众国 | (100) 555-4822 |
| three | 凯利奶奶的家园 | 雷吉娜·墨菲 | 安娜堡 | 美利坚合众国 | (313) 555-5735 |
| Sixteen | 大脚啤酒厂 | 谢丽尔·塞勒 | 弯曲 | 美利坚合众国 | 空 |
| Nineteen | 新英格兰海鲜罐头厂 | 罗柏商人 | 波士顿 | 美利坚合众国 | (617) 555-3267 |

请参阅数据库管理器的手册，并亲自尝试不同的选项。

延伸阅读:

1.  (Microsoft -使用 SELECT INTO 插入行)[[https://TechNet . Microsoft . com/en-us/library/ms 190750(v = SQL . 105)。aspx](https://technet.microsoft.com/en-us/library/ms190750(v=sql.105).aspx) ]
2.  (dofactory - SQL SELECT INTO 语句)[[http://www.dofactory.com/sql/select-into](http://www.dofactory.com/sql/select-into)]