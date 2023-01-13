# SQL Right Join 命令:示例语法

> 原文：<https://www.freecodecamp.org/news/sql-right-join-command-example-syntax/>

在本指南中，我们将讨论 SQL 右连接。

### 右连接

RIGHT JOIN 关键字返回右表(表 2)中的所有记录，以及左表(表 1)中的匹配记录。当没有匹配时，结果从左侧为空。

```
SELECT *
FROM table1
RIGHT JOIN table2
ON table1.column_name = table2.column_name; 
```

### 供参考的完整表格列表

食物或左桌数据

```
+---------+--------------+-----------+------------+
| ITEM_ID | ITEM_NAME    | ITEM_UNIT | COMPANY_ID |
+---------+--------------+-----------+------------+
| 1       | Chex Mix     | Pcs       | 16         |
| 6       | Cheez-It     | Pcs       | 15         |
| 2       | BN Biscuit   | Pcs       | 15         |
| 3       | Mighty Munch | Pcs       | 17         |
| 4       | Pot Rice     | Pcs       | 15         |
| 5       | Jaffa Cakes  | Pcs       | 18         |
| 7       | Salt n Shake | Pcs       |            |
+---------+--------------+-----------+------------+

company or RIGHT table data
``` text
+------------+---------------+--------------+
| COMPANY_ID | COMPANY_NAME  | COMPANY_CITY |
+------------+---------------+--------------+
| 18         | Order All     | Boston       |
| 15         | Jack Hill Ltd | London       |
| 16         | Akas Foods    | Delhi        |
| 17         | Foodies.      | London       |
| 19         | sip-n-Bite.   | New York     |
+------------+---------------+--------------+ 
```

要从 company 表中获取 company name，从 foods 表中获取 company ID、item name 列，可以使用以下 SQL 语句:

```
SELECT company.company_id,company.company_name,
company.company_city,foods.company_id,foods.item_name
FROM   company
RIGHT JOIN foods
ON company.company_id = foods.company_id; 
```

输出

```
COMPANY_ID COMPANY_NAME              COMPANY_CITY              COMPANY_ID ITEM_NAME
---------- ------------------------- ------------------------- ---------- --------------
18         Order All                 Boston                    18         Jaffa Cakes
15         Jack Hill Ltd             London                    15         Pot Rice
15         Jack Hill Ltd             London                    15         BN Biscuit
15         Jack Hill Ltd             London                    15         Cheez-It
16         Akas Foods                Delhi                     16         Chex Mix
17         Foodies.                  London                    17         Mighty Munch
NULL       NULL                      NULL                      NULL       Salt n Shake 
```