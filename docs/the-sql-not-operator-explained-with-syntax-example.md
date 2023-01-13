# SQL Not 运算符用语法示例解释

> 原文：<https://www.freecodecamp.org/news/the-sql-not-operator-explained-with-syntax-example/>

可以在`SELECT`语句的`WHERE`子句中使用`NOT`运算符。当您想要选择一个不为真的条件时，可以使用它。

下面是一个代码示例，它选择所有非男性人员:

```
SELECT Id, Name, DateOfBirth, Gender
FROM Person
WHERE NOT Gender = "M" 
```