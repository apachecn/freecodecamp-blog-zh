# SQL LIKE 语句–如何使用通配符查询 SQL

> 原文：<https://www.freecodecamp.org/news/sql-like-statement-how-to-query-sql-with-wildcard/>

您可以在 SQL LIKE 语句中使用`%`和`_`通配符来比较 SQL 表中的值。但是这到底是怎么回事呢？

在本文中，我将通过代码示例向您展示如何使用 SQL LIKE 语句。

## SQL LIKE 语句的基本语法

以下是 SQL Like 语句的基本语法:

```
SELECT FROM table_name
WHERE column LIKE 'string pattern'
```

[在 SQL](https://dev.mysql.com/doc/mysql-tutorial-excerpt/5.7/en/pattern-matching.html) 中，

> 模式匹配使您能够使用`_`匹配任何单个字符，使用`%`匹配任意数量的字符(包括零个字符)

例如，如果我们想在表格中找到所有以字母“T”开头的名字，那么我们可以使用以下语法:

```
WHERE name LIKE 'T%'
```

或者，如果我们想要查找表中包含字母“on”的所有名称，那么我们可以使用以下语法:

```
WHERE name LIKE '%on%'
```

我们可以使用`_`通配符来查找单个字符匹配。例如，这是查找`quantity`类别中所有 2 位数且以“9”结尾的数字的语法:

```
WHERE quantity LIKE '_9'; 
```

为了更好地理解这些通配符如何与 SQL Like 语句一起工作，我们来看一个示例数据表。

## 如何使用 SQL LIKE 语句–以汽车表为例

在这个例子中，我们有一个包含列`id`、`model`、`make`和`price`的`cars`表。

```
id|make|model|price
1|Honda|Civic|21000
2|Ford|Fusion|23000
3|Toyota|Camry|24000
4|Dodge|Challenger|29000
5|Tesla|Model X|104000
6|Chevrolet|Tahoe|49000
```

### 如何在 SQL LIKE 语句中使用`%`通配符

在第一个示例中，我们希望找到所有以字母“C”开头的汽车型号。

```
SELECT * FROM cars
WHERE model LIKE 'C%';
```

这段代码将从`cars`表中返回以下结果:

```
id|make|model|price
1|Honda|Civic|21000
3|Toyota|Camry|24000
4|Dodge|Challenger|29000
```

我们可以看到，在我们的`cars`表的 6 个条目中，有 3 个条目的型号名称以字母“C”开头。

SQL LIKE 语句不区分大小写，这意味着`'C%'`和`'c%'`将返回相同的结果。

我们还可以使用`%`通配符和 SQL LIKE 语句来查找以一个或多个字符结尾的条目。

在本例中，我们希望找到名称以“a”结尾的所有汽车制造商。

```
SELECT * FROM cars
WHERE make LIKE '%a';
```

这段代码将从`cars`表中返回以下结果:

```
id|make|model|price
1|Honda|Civic|21000
3|Toyota|Camry|24000
5|Tesla|Model X|104000
```

我们可以看到，6 家汽车制造商中有 3 家的名称以字母“a”结尾。

### 如何在 SQL LIKE 语句中使用多个`%`通配符

在本例中，我们要查找所有包含数字 9 的汽车价格。

```
SELECT * FROM cars
WHERE price LIKE '%9%';
```

这段代码将从`cars`表中返回以下结果:

```
id|make|model|price
4|Dodge|Challenger|29000
6|Chevrolet|Tahoe|49000
```

我们可以看到 6 个车价中有 2 个包含数字 9。

### 如何在 SQL LIKE 语句中使用`_`通配符

我们可以使用`_`通配符来查找单个字符匹配。

让我们修改一下我们的`cars`表:

```
id|make|model|price
30|Honda|Civic|21000
35|Ford|Fusion|23000
40|Toyota|Camry|24000
45|Dodge|Challenger|29000
50|Tesla|Model X|104000
55|Chevrolet|Tahoe|49000
```

在本例中，我们希望找到所有两位数长度且以数字 0 结尾的 id。

```
SELECT * FROM cars
WHERE id LIKE '_0';
```

这段代码将从`cars`表中返回以下结果:

```
id|make|model|price
30|Honda|Civic|21000
40|Toyota|Camry|24000
50|Tesla|Model X|104000
```

我们可以看到 6 个两位数的汽车 id 中有 3 个是以数字 0 结尾的。

### 如何在 SQL LIKE 语句中使用多个`_`通配符

让我们再次修改我们的`cars`表。

```
id|make|model|price
130|Honda|Civic|21000
135|Ford|Fusion|23000
140|Toyota|Camry|24000
145|Dodge|Challenger|29000
150|Tesla|Model X|104000
155|Chevrolet|Tahoe|49000
```

在本例中，我们希望找到所有三位数且以数字 0 结尾的 id。

```
SELECT * FROM cars
WHERE id LIKE '__0';
```

这段代码将从`cars`表中返回以下结果:

```
id|make|model|price
130|Honda|Civic|21000
140|Toyota|Camry|24000
150|Tesla|Model X|104000
```

我们必须在 SQL LIKE 语句中使用两个下划线`_`来查找所有三位数且以数字 0 结尾的 id。

```
Using two underscores
'__0'

instead of just one 
'_0'
```

### 如何在 SQL LIKE 语句中使用 NOT 运算符

我们可以在 SQL 中使用 NOT 操作符来查找所有与 LIKE 语句中的字符串模式不匹配的结果。

我们可以修改上一个例子，找到不以数字 0 结尾的所有三个数字 id。

```
SELECT * FROM cars
WHERE id NOT LIKE '__0';
```

这段代码将从`cars`表中返回以下结果:

```
id|make|model|price
135|Ford|Fusion|23000
145|Dodge|Challenger|29000
155|Chevrolet|Tahoe|49000
```

### 如何在 SQL LIKE 语句中使用`%`和`_`通配符

在本例中，我们要查找第二个字母是“o”且名称以“a”结尾的所有汽车制造商。

```
SELECT * FROM cars
WHERE make LIKE '_o%a';
```

`_o`是找到第二个字母是“o”的所有汽车制造商。`%a`是查找所有以字母“a”结尾的汽车制造商。

这段代码将从`cars`表中返回以下结果:

```
id|make|model|price
130|Honda|Civic|21000
140|Toyota|Camry|24000
```

## 结论

您可以在 SQL LIKE 语句中使用`%`和`_`通配符来比较 SQL 表中的值。

下面是 SQL Like 语句的基本语法。

```
SELECT FROM table_name
WHERE column LIKE 'string pattern'
```

`%`匹配零个、一个或多个字符，而`_`匹配单个字符。

在本文中，我们通过使用`cars`表示例学习了如何在 SQL LIKE 语句中使用这两种通配符。

我希望您喜欢这篇文章，并祝您的 SQL 之旅好运。