# Python If Else 语句–条件语句解释

> 原文：<https://www.freecodecamp.org/news/python-if-else-statement-conditional-statements-explained/>

在许多情况下，您不希望所有代码都在程序中执行。

相反，您可能希望某些代码仅在满足特定条件时运行，而另一组代码在条件不满足时运行。

这就是条件语句派上用场的地方。

条件语句允许你以简洁紧凑的方式控制程序的逻辑流程。

它们是分支——就像道路上的岔口——修改代码的执行方式和处理决策。

本教程介绍了 Python 编程语言中的`if`、`if..else`和`elif`语句的基础知识，并在过程中使用了示例。

我们开始吧！

## 基本`if`语句的语法

Python 中的一个`if`语句本质上说:

"如果此表达式的计算结果为 True，则运行一次表达式后面的代码。如果不是这样，就不要运行下面的代码块。”

基本`if`语句的一般语法如下所示:

```
if condition:
    execute statement 
```

一条`if`语句包括:

*   `if`关键字，启动`if`语句。
*   然后出现了一个条件。条件可以评估为 True 或 False。条件周围的括号(`()`)是可选的，但是当存在多个条件时，它们有助于提高代码的可读性。
*   冒号`:`将条件与后面的可执行语句分开。
*   新的一行。
*   一级缩进的 **4** 个空格，这是 Python 的约定。缩进的级别与后面的语句体相关联。
*   最后是声明的主体。这是仅当语句评估为 True 时才运行的代码。我们可以在主体中有多个可以执行的行，在这种情况下，我们需要小心它们都有相同的缩进级别。

让我们举下面的例子:

```
a = 1
b = 2

if b > a:
    print(" b is in fact bigger than a") 
```

输出:

```
b is in fact bigger than a 
```

在上面的例子中，我们创建了两个变量，`a`和`b`，并分别给它们赋值`1`和`2`。

print 语句中的短语实际上会被打印到控制台，因为条件`b > a`被评估为 True，所以它后面的代码会运行。如果不是真的，什么都不会发生。没有代码会运行。

如果我们这样做了:

```
a = 1
b = 2

if a > b
    print("a is in fact bigger than b") 
```

不会执行任何代码，也不会将任何内容打印到控制台。

## Python `if..else`语句是如何工作的？

`if`语句仅在满足条件时运行代码。否则什么都不会发生。

如果我们还希望代码在条件不满足时运行，该怎么办？这就是`else`部分出现的地方。

`if..else`语句的语法如下所示:

```
if condition:
    execute statement if condition is True
else:
     execute statement if condition is False 
```

Python 中的一个`if..else`语句的意思是:

当`if`表达式的计算结果为真时，执行它后面的代码。但是如果它评估为假，那么运行遵循`else`语句的代码”

`else`语句写在缩进代码最后一行之后的新行上，它*不能*自己写。一条`else`语句是一条`if`语句的一部分。

它后面的代码也需要用 **4** 空格缩进，以显示它是`else`子句的一部分。

当且仅当`if`语句为假时，跟随`else`语句的代码被执行*。如果您的`if`语句为真，因此代码运行了，那么`else`块中的代码将永远不会运行。*

```
a = 1
b = 2

if a < b:
    print(" b is in fact bigger than a")
else:
    print("a is in fact bigger than b") 
```

这里，`else`语句后面的代码行`print("a is in fact bigger than b")`永远不会运行。在它之前的`if`语句为真，所以只有代码运行。

`else`块在以下情况下运行:

```
a = 1
b = 2

if a > b:
    print(" a is in fact bigger than b")
else:
    print("b is in fact bigger than a") 
```

输出:

```
b is in fact bigger than a 
```

要知道在`if`和`else`之间不能写任何其他代码。如果你这样做，你会得到一个`SyntaxError`:

```
if 1 > 2:
   print("1 is bigger than 2")
print("hello world")
else:
   print("1 is less than 2") 
```

输出:

```
File "<stdin>", line 3
print("hello world")
^
SyntaxError: invalid syntax 
```

## `elif`在 Python 中是如何工作的？

如果我们想有两个以上的选择呢？

不是说:“如果第一个条件为真，就这样做，否则就那样做”，而是说“如果这不是真的，就这样做，如果所有条件都不为真，就这样做”。

`elif`代表 else，如果。

基本语法如下所示:

```
if first_condition:
    execute statement
elif second_condition:
    execute statement
else:
    alternative executable statement if all previous conditions are False 
```

我们可以使用不止一个`elif`语句。这给了我们更多的条件和更多的选择。

例如:

```
x = 1

if x > 10:
    print(" x is greater than 10!")
elif x < 10:
      print("x is less than 10!")
elif x < 20 :
      print("x is less than 20!")
else:
     print("x is equal to 10") 
```

输出:

```
x is less than 10! 
```

在这个例子中，`if`语句测试一个特定的条件，`elif`程序块是两个选项，而`else`程序块是当所有先前的条件都不满足时的最后一个解决方案。

注意你写`elif`陈述的顺序。

在前面的示例中，如果您编写了:

```
x = 1

if x > 10:
    print(" x is greater than 10!")
elif x < 20 :
      print("x is less than 20!")
elif x < 10:
      print("x is less than 10!")
else:
     print("x is equal to 10") 
```

第`x is less than 20!`行将被执行，因为它排在第一位。

`elif`语句使代码更容易编写。随着程序变得越来越复杂，规模越来越大，您可以使用它来代替跟踪`if..else`语句。

如果所有的`elif`语句都没有被考虑并且为假，那么并且只有到那时，作为最后的手段，跟随`else`语句的代码才会运行。

例如，下面是一个运行`else`语句的例子:

```
x = 10

if x > 10:
    print(" x is greater than 10!")
elif x < 10:
      print("x is less than 10!")
elif x > 20 :
      print("x is greater than 20!")
else:
     print("x is equal to 10") 
```

输出

```
x is equal to 10 
```

## 结论

就是这样！

这些是 Python 中`if`、`if..else`和`elif`的基本原则，可以帮助你开始使用条件语句。

从这里开始，语句可以变得更加高级和复杂。

条件语句可以嵌套在其他条件语句中，这取决于您试图解决的问题以及解决方案背后的逻辑。

感谢阅读和快乐编码！