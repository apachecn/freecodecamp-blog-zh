# SQL 别名指南——如何使用 as 关键字为表设置别名——语法示例

> 原文：<https://www.freecodecamp.org/news/sql-alias-guide-as-keyword/>

## 使用 AS 来指定有意义或简单的名称

可以使用 AS 为正在选择或已经计算的数据列指定名称。

```
SELECT user_only_num1 AS AgeOfServer, (user_only_num1 - warranty_period) AS NonWarrantyPeriod FROM server_table 
```

这会产生如下输出。

```
+-------------+------------------------+
| AgeOfServer | NonWarrantyPeriod      | 
+-------------+------------------------+
|         36  |                     24 |
|         24  |                     12 | 
|         61  |                     49 |
|         12  |                      0 | 
|          6  |                     -6 |
|          0  |                    -12 | 
|         36  |                     24 |
|         36  |                     24 | 
|         24  |                     12 | 
+-------------+------------------------+ 
```

还可以使用 AS 为表指定一个名称，以便在联接中更容易引用。

```
SELECT ord.product, ord.ord_number, ord.price, cust.cust_name, cust.cust_number FROM customer_table AS cust

JOIN order_table AS ord ON cust.cust_number = ord.cust_number 
```

这会产生如下输出。

```
+-------------+------------+-----------+-----------------+--------------+
| product     | ord_number | price     | cust_name       | cust_number  |
+-------------+------------+-----------+-----------------+--------------+
|     RAM     |   12345    |       124 | John Smith      |  20          |
|     CPU     |   12346    |       212 | Mia X           |  22          |
|     USB     |   12347    |        49 | Elise Beth      |  21          |
|     Cable   |   12348    |         0 | Paul Fort       |  19          |
|     Mouse   |   12349    |        66 | Nats Back       |  15          |
|     Laptop  |   12350    |       612 | Mel S           |  36          |
|     Keyboard|   12351    |        24 | George Z        |  95          |
|     Keyboard|   12352    |        24 | Ally B          |  55          |
|     Air     |   12353    |        12 | Maria Trust     |  11          |
+-------------+------------+-----------+-----------------+--------------+ 
```