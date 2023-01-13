# 用示例语法解释的 SQL Group By 语句

> 原文：<https://www.freecodecamp.org/news/the-sql-group-by-statement-explained-with-example-syntax/>

GROUP BY 为我们提供了一种组合行和聚合数据的方法。

使用的数据来自我们在其中一些指南中使用的活动捐款数据。

以下 SQL 语句回答了这个问题:“哪些候选人在 2016 年获得了最大的总捐款，但只有那些超过 2000 万美元的候选人？”

按照降序(DESC)对该数据集进行排序，将总贡献最大的候选项放在列表的顶部。

```
SELECT Candidate, Election_year, sum(Total_$), count(*)
FROM combined_party_data
WHERE Election_year = 2016
GROUP BY Candidate, Election_year -- this tells the DBMS to summarize by these two columns
HAVING sum(Total_$) > 20000000  -- limits the rows presented from the summary of money ($20 Million USD)
ORDER BY sum(Total_$) DESC; -- orders the presented rows with the largest ones first. 
```

```
+--------------------------------------------------+---------------+-------------------+----------+
| Candidate                                        | Election_year | sum(Total_$)      | count(*) |
+--------------------------------------------------+---------------+-------------------+----------+
| CLINTON, HILLARY RODHAM & KAINE, TIMOTHY M (TIM) |          2016 | 568135094.4400003 |      126 |
| TRUMP, DONALD J & PENCE, MICHAEL R (MIKE)        |          2016 | 366853142.7899999 |      114 |
| SANDERS, BERNARD (BERNIE)                        |          2016 |      258562022.17 |      122 |
| CRUZ, RAFAEL EDWARD (TED)                        |          2016 | 93430700.29000005 |      104 |
| CARSON, BENJAMIN S (BEN)                         |          2016 | 62202411.12999996 |       93 |
| RUBIO, MARCO ANTONIO                             |          2016 |        44384313.9 |      106 |
| BUSH, JOHN ELLIS (JEB)                           |          2016 |       34606731.78 |       97 |
+--------------------------------------------------+---------------+-------------------+----------+
7 rows in set (0.01 sec) 
```

与所有这些 SQL 内容一样，它们比本入门指南中的内容要多得多。

我希望这至少能给你足够的时间开始。

请参阅数据库管理器的手册，并亲自尝试不同的选项。