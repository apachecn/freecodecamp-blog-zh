# SQL 中的 Case 语句–示例查询

> 原文：<https://www.freecodecamp.org/news/case-statement-in-sql-example-query/>

如果您需要基于其他单元格有条件地向一个单元格添加值，您将使用 SQL 的 case 语句。

如果您了解其他语言，SQL 中的 case 语句类似于 If 语句或 switch 语句。它允许您有条件地指定一个值，以便根据满足的条件，在单元格中获得不同的值。

这在数据分析中非常重要，所以在介绍完 Case 语句后，我们将看到几个例子，说明如何用它来简单地分析数据。

# SQL Case 语句语法

语法中有很多东西，但它仍然相当直观:关键字`CASE`表示 case 语句的开始，关键字`END`表示它的结束。

然后，对于一个单一的条件，你可以写关键字`WHEN`，后跟必须满足的条件。之后是关键字`THEN`和该条件的值，比如`WHEN <condition> THEN <stuff>`。

这之后可以是其他的`WHEN` / `THEN`语句。

最后，您可以使用关键字`ELSE`添加一个缺省使用的值，如下所示。

```
CASE
   WHEN condition1 THEN stuff
   WHEN condition2 THEN other stuff
   ...
   ELSE default stuff
END
```

让我们将此付诸实践，以便更好地理解它。

# SQL Case 语句示例

让我们在一个例子中使用`CASE`语句。我们有一张表，上面列有学生名单和他们的考试成绩。我们需要给每个学生打分，可以用 case 语句自动完成。

| 身份证明（identification） | 名字 | 得分 |
| --- | --- | --- |
| one | 西米索拉 | Sixty |
| Two | 伊凡一世 | Eighty |
| three | Metodija | fifty-two |
| four | 卡勒姆 | Ninety-eight |
| five | 发光酶免疫测定 | Eighty-four |
| six | Aparecida | Eighty-two |
| seven | 厄休拉（女子名） | sixty-nine |
| eight | 拉麦士 | seventy-eight |
| nine | 电晕 | Eighty-seven |
| Ten | 图书馆与资讯学教育协会 | Fifty-seven |
| Eleven | 凯兰崔尔 | eighty-nine |
| Twelve | 梅雷尔 | Ninety-nine |
| Thirteen | 查里斯 | Fifty-five |
| Fourteen | 尼提亚 | Eighty-one |
| Fifteen | Elşad | Seventy-one |
| Sixteen | 丽丝 | Ninety |
| Seventeen | 约翰娜（Joanna 的异体）（f.） | Ninety |
| Eighteen | 两栖动物 | Ninety |
| Nineteen | 大介 | Ninety-seven |
| Twenty | 萨克猜 | Sixty-one |
| Twenty-one | 埃尔伯特 | Sixty-three |
| Twenty-two | 凯特琳 | Fifty-one |

我们可以使用`CASE`语句给每个学生打分，我们将把分数添加到一个名为`grade`的新列中。

我们先写`CASE`语句，里面会写每个年级的细分。当`score`为 94 或更高时，该行将具有`A`的值。如果分数为 90 或更高，则它的值为`A-`，依此类推。

```
 CASE
    WHEN score >= 94 THEN "A"
    WHEN score >= 90 THEN "A-"
    WHEN score >= 87 THEN "B+"
    WHEN score >= 83 THEN "B"
    WHEN score >= 80 THEN "B-"
    WHEN score >= 77 THEN "C+"
    WHEN score >= 73 THEN "C"
    WHEN score >= 70 THEN "C-"
    WHEN score >= 67 THEN "D+"
    WHEN score >= 60 THEN "D"
    ELSE "F"
  END
```

在我们编写完`CASE`语句后，我们将把它添加到一个查询中。然后我们将使用关键字`AS`给列命名为`grade`:

```
SELECT *,
  CASE
    WHEN score >= 94 THEN "A"
    WHEN score >= 90 THEN "A-"
    WHEN score >= 87 THEN "B+"
    WHEN score >= 83 THEN "B"
    WHEN score >= 80 THEN "B-"
    WHEN score >= 77 THEN "C+"
    WHEN score >= 73 THEN "C"
    WHEN score >= 70 THEN "C-"
    WHEN score >= 67 THEN "D+"
    WHEN score >= 60 THEN "D"
    ELSE "F"
  END AS grade
FROM students_grades;
```

我们从这个查询中得到的表格如下——现在每个学生都有一个基于他们分数的分数。

| 身份证明（identification） | 名字 | 得分 | 等级 |
| --- | --- | --- | --- |
| one | 西米索拉 | Sixty | D |
| Two | 伊凡一世 | Eighty | B- |
| three | Metodija | fifty-two | F |
| four | 卡勒姆 | Ninety-eight | A |
| five | 发光酶免疫测定 | Eighty-four | B |
| six | Aparecida | Eighty-two | B- |
| seven | 厄休拉（女子名） | sixty-nine | D+ |
| eight | 拉麦士 | seventy-eight | C+ |
| nine | 电晕 | Eighty-seven | B+ |
| Ten | 图书馆与资讯学教育协会 | Fifty-seven | F |
| Eleven | 凯兰崔尔 | eighty-nine | B+ |
| Twelve | 梅雷尔 | Ninety-nine | A |
| Thirteen | 查里斯 | Fifty-five | F |
| Fourteen | 尼提亚 | Eighty-one | B- |
| Fifteen | Elşad | Seventy-one | C- |
| Sixteen | 丽丝 | Ninety | 表示“不” |
| Seventeen | 约翰娜（Joanna 的异体）（f.） | Ninety | 表示“不” |
| Eighteen | 两栖动物 | Ninety | 表示“不” |
| Nineteen | 大介 | Ninety-seven | A |
| Twenty | 萨克猜 | Sixty-one | D |
| Twenty-one | 埃尔伯特 | Sixty-three | D |
| Twenty-two | 凯特琳 | Fifty-one | F |

## 更复杂的 Case 语句示例

除了 case 语句之外，我们还可以根据需要使用其他语句以不同的方式操纵该表。

### 案例陈述示例 1

例如，我们可以使用 [`ORDER BY`](https://www.freecodecamp.org/news/sql-order-by-statement-example-sytax/) 对等级最高的行进行排序。

```
SELECT name,
  CASE
    WHEN score >= 94 THEN "A"
    WHEN score >= 90 THEN "A-"
    WHEN score >= 87 THEN "B+"
    WHEN score >= 83 THEN "B"
    WHEN score >= 80 THEN "B-"
    WHEN score >= 77 THEN "C+"
    WHEN score >= 73 THEN "C"
    WHEN score >= 70 THEN "C-"
    WHEN score >= 67 THEN "D+"
    WHEN score >= 60 THEN "D"
    ELSE "F"
  END AS grade
FROM students_grades
ORDER BY score DESC;
```

我们基于数字`score`而不是`grade`列进行排序，因为字母顺序与基于值的等级顺序不同。我们使用`DESC`关键字以降序呈现它，最高值在顶部。

我们得到的表格如下所示:

| 名字 | 等级 |
| --- | --- |
| 梅雷尔 | A |
| 卡勒姆 | A |
| 大介 | A |
| 丽丝 | 表示“不” |
| 约翰娜（Joanna 的异体）（f.） | 表示“不” |
| 两栖动物 | 表示“不” |
| 凯兰崔尔 | B+ |
| 电晕 | B+ |
| 发光酶免疫测定 | B |
| Aparecida | B- |
| 尼提亚 | B- |
| 伊凡一世 | B- |
| 拉麦士 | C+ |
| Elşad | C- |
| 厄休拉（女子名） | D+ |
| 埃尔伯特 | D |
| 萨克猜 | D |
| 西米索拉 | D |
| 图书馆与资讯学教育协会 | F |
| 查里斯 | F |
| Metodija | F |
| 凯特琳 | F |

### 案例陈述示例 2

让我们对这些数据做一点分析。我们可以用 [`GROUP BY`和`COUNT`](https://www.freecodecamp.org/news/sql-group-by-clauses-explained/) 来统计每个年级收了多少学生。

```
SELECT 
  CASE
    WHEN score >= 94
      THEN "A"
    WHEN score >= 90 THEN "A-"
    WHEN score >= 87 THEN "B+"
    WHEN score >= 83 THEN "B"
    WHEN score >= 80 THEN "B-"
    WHEN score >= 77 THEN "C+"
    WHEN score >= 73 THEN "C"
    WHEN score >= 70 THEN "C-"
    WHEN score >= 67 THEN "D+"
    WHEN score >= 60 THEN "D"
    ELSE "F"
  END AS grade,
  COUNT(*) AS number_of_students
FROM students_grades
GROUP BY grade
ORDER BY score DESC; 
```

我们使用`[ORDER BY](/news/sql-order-by-statement-example-sytax/)`将等级从最高到最低排序，我们使用`score`是因为它是一个数值(因为通过`grade`列排序将使用字母顺序，这与等级的值排序不同)。

| 等级 | 学生人数 |
| --- | --- |
| A | three |
| 表示“不” | three |
| B+ | Two |
| B | one |
| B- | three |
| C+ | one |
| C- | one |
| D+ | one |
| D | three |
| F | four |

### 案例陈述示例 3

让我们对这些数据做一些不同的分析。我们可以用[`GROUP BY``COUNT`](https://www.freecodecamp.org/news/sql-group-by-clauses-explained/)和不同的 case 语句来统计有多少学生通过了考试。然后，我们可以使用`[ORDER BY](/news/sql-order-by-statement-example-sytax/)`按照我们喜欢的顺序排列该列，通过考试的学生人数排在最上面。

```
SELECT 
  CASE
    WHEN score >= 60
      THEN "passed"
    ELSE "failed"
  END AS result,
  COUNT(*) AS number_of_students
FROM students_grades
GROUP BY result
ORDER BY result DESC;
```

我们得到的表格如下所示。这个班表现不算太差，22 名学生中有 18 名及格——但另外 4 名可能需要一些帮助。

| 结果 | 学生人数 |
| --- | --- |
| 通过 | Eighteen |
| 不成功的 | four |

# 结论

当您需要根据特定条件获取值时，case 语句是一个强大的工具。

在本文中，您已经学习了如何使用它，并且看到了一些如何使用它进行数据分析的例子。