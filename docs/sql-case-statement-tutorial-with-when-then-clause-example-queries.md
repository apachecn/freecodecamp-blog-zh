# SQL Case 语句教程–带有 When-Then 子句的示例查询

> 原文：<https://www.freecodecamp.org/news/sql-case-statement-tutorial-with-when-then-clause-example-queries/>

## 什么是 SQL Case 语句？

case 语句基本上是 SQL 版本的条件逻辑。它可以以与 JavaScript 等编程语言中的`if`语句相同的方式使用，但其结构略有不同。

## 抽样资料

想象你在教一门文学课。为了达到课程要求，你的学生必须写一篇论文。

您已经创建了下表来跟踪哪些学生提交了他们的论文以及他们的分数。如果他们还没有提交论文，他们的分数会列为`NULL`。

| 学生 id | 名字 | 已提交 _ 论文 | 等级 |
| --- | --- | --- | --- |
| one | 约翰 | 真实的 | Eighty-six |
| Two | 上述的 | 真实的 | Ninety |
| three | 艾丽莎 | 错误的 | 空 |
| four | 鲨鱼 | 真实的 | sixty-eight |
| five | 埃莉诺 | 真实的 | Ninety-five |
| six | 秋子 | 错误的 | 空 |
| seven | 香精油 | 真实的 | Seventy-six |
| eight | 贾马尔（男子名） | 真实的 | eighty-five |
| nine | 李颖芝 | 真实的 | Eighty-eight |
| Ten | 温和的 | 错误的 | 空 |

## 如何用 SQL 编写 Case 语句

也许你想给你的学生一个关于他们作业状态的信息。要获得状态，您可以只选择`submitted_essay`列，但是只显示`TRUE`或`FALSE`的消息不是特别易读。

相反，您可以使用一个`CASE`语句，并根据`submitted_essay`是真还是假打印出不同的消息。

`CASE`语句的基本结构是`CASE WHEN... THEN... END`。`CASE WHEN`、`THEN`、`END`均为必填项。`ELSE`和`AS`是可选的。`CASE`语句必须放在`SELECT`子句中。

```
SELECT name,
	CASE WHEN submitted_essay IS TRUE THEN 'essay submitted!'
	ELSE 'finish that essay!' END  AS status
FROM students;
```

在上面的例子中，我们选择我们学生的名字，然后根据`submitted_essay`的真假在`status`列显示不同的消息。结果表如下所示:

| 名字 | 状态 |
| --- | --- |
| 秋子 | 完成那篇文章！ |
| 温和的 | 完成那篇文章！ |
| 艾丽莎 | 完成那篇文章！ |
| 上述的 | 征文提交！ |
| 埃莉诺 | 征文提交！ |
| 香精油 | 征文提交！ |
| 鲨鱼 | 征文提交！ |
| 李颖芝 | 征文提交！ |
| 约翰 | 征文提交！ |
| 贾马尔（男子名） | 征文提交！ |

现在，假设您想要包含更多的信息。如果学生提交了论文，你想对他们的成绩进行评论，如果他们还没有提交，你想告诉他们完成论文。这就是多条`WHEN/THEN`语句有用的地方。

```
SELECT name, essay_grade,
CASE WHEN essay_grade >= 80 THEN 'great job'
	 WHEN essay_grade < 80 THEN 'try harder'
	 ELSE 'finish that essay!' END  AS teacher_comment
FROM students;
```

在上面的代码示例中，我们显示了学生的姓名和论文分数，以及根据分数不同而不同的注释。

在第一个`WHEN/THEN`语句之后，您可以根据需要添加任意多的其他`WHEN/THEN`语句，以及一个`ELSE`语句来捕捉其他可能的情况。这类似于 JavaScript 中的`if... else if... else`风格逻辑(或者 Python 中的`if... elif... else`，等等)。

请注意，在这种情况下，`ELSE`旨在捕获等级为`NULL`(意味着尚未提交的论文)的论文，但是在其他情况下，您可以使用`IS NULL`来检查选择的值是否为空。

别忘了用`END`来结束你的案例陈述！下面，您可以看到这个查询的结果。

| 名字 | 论文 _ 成绩 | 老师 _ 评论 |
| --- | --- | --- |
| 秋子 | 空 | 完成那篇文章！ |
| 温和的 | 空 | 完成那篇文章！ |
| 艾丽莎 | 空 | 完成那篇文章！ |
| 上述的 | Ninety | 伟大的工作 |
| 埃莉诺 | Ninety-five | 伟大的工作 |
| 香精油 | Seventy-six | 更加努力 |
| 鲨鱼 | sixty-eight | 更加努力 |
| 李颖芝 | Eighty-eight | 伟大的工作 |
| 约翰 | Eighty-six | 伟大的工作 |
| 贾马尔（男子名） | eighty-five | 伟大的工作 |

## 结论

Case 语句是一种清晰、简洁的方式来理解 SQL 中的查询，并且易于学习和理解。查询愉快！