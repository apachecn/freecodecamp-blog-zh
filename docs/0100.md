# Bash If 语句–Linux Shell If-Else 语法示例

> 原文：<https://www.freecodecamp.org/news/bash-if-statement-linux-shell-if-else-syntax-example/>

编码时，您可能需要根据特定条件做出决策。条件是评估为布尔表达式(`true`或`false`)的表达式。

基于特定条件帮助执行不同代码分支的语句称为条件语句。

`if...else`是最常用的条件语句之一。和其他编程语言一样，Bash 脚本也支持`if...else`语句。我们将在这篇博文中详细研究这一点。

## `if`语句的语法

您可以以多种方式使用`if`语句。`if`语句的一般结构如下:

*   仅使用`if`语句:`if...then...fi`
*   将 and `if`与`else`语句一起使用:`if...then...else...fi`语句
*   使用带有`if` : `if..elif..else..fi`的多个`else`语句

## 如何使用`if`语句

当您使用单个`if`语句时，语法如下:

```
if [ condition ]
then
	statement
fi
```

> 请注意，空格是语法的一部分，不应删除。

让我们看一个例子，在这个例子中，我们比较两个数字，看第一个数字是否是较小的那个。

```
#! /bin/sh

a=5
b=30

if [ $a -lt $b ]
then
        echo "a is less than b"
fi
```

如果运行上面的代码片段，条件`if [ $a -lt $b ]`的计算结果为`True`，If 语句中的语句将执行

**输出:**

```
a is less than b
```

## 如何使用`if .. else`语句

当您正在使用一个`if`语句并且想要添加另一个条件时，语法如下:

```
if [ condition ]
then
	statement
else
	do this by default
fi
```

让我们看一个例子，我们想知道第一个数字是大于还是小于第二个数字。这里，`if [ $a -lt $b ]`的计算结果为 false，这将导致代码的`else`部分运行。

```
#! /bin/sh

a=99
b=45

if [ $a -lt $b ]
then
        echo "a is less than b"
else
        echo "a is greater than b"
fi
```

**输出:**

```
a is greater than b
```

## 如何使用`if..elif..else`语句

假设您想要添加进一步的条件和比较，以使代码动态化。在这种情况下，语法如下所示:

```
if [ condition ]
then
	statement
elif [ condition ] 
then
	statement 
else
	do this by default
fi
```

为了创建有意义的比较，我们也可以使用 AND `-a`和 OR `-o`。

在本例中，我们将使用以下条件来确定三角形的类型:

*   每条边的长度都不同的三角形。
*   `Isosceles`:两条边相等的三角形。
*   所有边都相等的三角形。

```
read a
read b
read c

if [ $a == $b -a $b == $c -a $a == $c ]
then
echo EQUILATERAL

elif [ $a == $b -o $b == $c -o $a == $c ]
then
echo ISOSCELES
else
echo SCALENE

fi
```

在上面的例子中，脚本会要求用户输入三角形的三条边。接下来，它将比较边并决定三角形的类型。

```
3
4
5
SCALENE
```

## 结论

您可以基于类似`if..else`的条件轻松地分支您的代码，并使代码更加动态。在本教程中，您学习了`if...else`的语法以及一些例子。

我希望这篇教程对你有所帮助。

你从这个教程中学到的最喜欢的东西是什么？在 [Twitter](https://twitter.com/hira_zaira) 上告诉我！

你可以在这里阅读我的其他帖子[。](https://www.freecodecamp.org/news/author/zaira/)