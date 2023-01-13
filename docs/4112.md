# SQL 命令:带示例的基本语法、语句和查询介绍

> 原文：<https://www.freecodecamp.org/news/sql-commands-syntax-statements-queries/>

本指南提供了 SQL 语句语法的基本、高级描述。

SQL 是一个国际标准(ISO ),但是您会发现不同实现之间有许多差异。本指南以 MySQL 为例。如果您使用许多其他关系数据库管理器(DBMS)中的一个，如果需要的话，您将需要查看该 DBMS 的手册。

### 我们将涵盖的内容

*   Use(设置语句将使用的数据库)
*   Select 和 From 子句
*   Where 子句(和/或，在，之间，像)
*   订购依据(ASC，DESC)
*   分组依据和拥有

### 这个怎么用

这用于选择包含 SQL 语句表的数据库:

```
use fcc_sql_guides_database; -- select the guide sample database 
```

### Select 和 From 子句

Select 部分通常用于确定要在结果中显示哪些数据列。还有一些选项可以用来显示不是表列的数据。

此示例显示了从“student”表中选择的两列，以及两个计算列。计算列的第一列是无意义的数字，另一列是系统日期。

```
 select studentID, FullName, 3+2 as five, now() as currentDate
    from student; 
```

![image-1](img/a231011ce1a1f08a9194a8202079d092.png)

### Where 子句(and / or，IN，Between and LIKE)

WHERE 子句用于限制返回的行数。

在这种情况下，所有这五个都将被使用是一个有点可笑的 Where 子句。

将这个结果与上面的 SQL 语句进行比较，以遵循这个逻辑。

将显示以下行:

*   学生 id 介于 1 和 5 之间(包括 1 和 5)
*   或者 studentID = 8
*   或者在名称中包含“Maxmimo”

以下示例与此类似，但它进一步指定，如果任何学生具有特定的 SAT 分数(1000、1400)，则不会显示他们:

```
 select studentID, FullName, sat_score, recordUpdated
    from student
    where (
		studentID between 1 and 5
		or studentID = 8
        or FullName like '%Maximo%'
		)
		and sat_score NOT in (1000, 1400); 
```

![image-1](img/095fd982d31a88aed57402e7848c1433.png)

### 订购依据(ASC，DESC)

Order By 为我们提供了一种按照 SELECT 部分中的一项或多项对结果集进行排序的方法。这是和上面一样的列表，但是是按照学生的全名排序的。默认的排序顺序是升序(ASC ),但是要以相反的顺序(降序)排序，可以使用 DESC，如下例所示:

```
 select studentID, FullName, sat_score
    from student
    where (studentID between 1 and 5 -- inclusive
		or studentID = 8
        or FullName like '%Maximo%')
		and sat_score NOT in (1000, 1400)
	order by FullName DESC; 
```

![image-1](img/34413384673fa3ba302a2e241e0f5463.png)

### 分组依据和拥有

Group By 为我们提供了一种组合行和聚合数据的方法。Having 子句类似于上面的 Where 子句，只是它作用于分组数据。

这些数据来自我们在一些指南中使用的活动捐款数据。

这条 SQL 语句回答了这样一个问题:“2016 年，哪些候选人收到了最多的捐款(不是 number 美元，而是计数(*))，但只有那些捐款超过 80 笔的候选人？”

按照降序(DESC)对该数据集进行排序，将贡献数量最大的候选项放在列表的顶部。

```
 select Candidate, Election_year, sum(Total_$), count(*)
    from combined_party_data
    where Election_year = 2016
    group by Candidate, Election_year
    having count(*) > 80
    order by count(*) DESC; 
```

![image-1](img/52af1a4e2cbf415c407d1e214d231586.png)

与所有这些 SQL 内容一样，它们比本入门指南中的内容要多得多。我希望这至少能给你足够的时间开始。请参阅数据库管理器的手册，并亲自尝试不同的选项。