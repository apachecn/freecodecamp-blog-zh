# Python 中的缩进及示例

> 原文：<https://www.freecodecamp.org/news/indentation-in-python/>

在 Python 中编码时，不要混合使用制表符和空格，这通常是一个好习惯。这样做可能会导致`TabError`，你的程序将会崩溃。编写代码时保持一致——选择使用制表符或空格缩进，并在整个程序中遵循您选择的约定。

### 代码块和缩进

Python 最与众不同的特性之一是使用缩进来标记代码块。考虑我们简单的密码检查程序中的 if 语句:

```
if pwd == 'apple':
    print('Logging on ...')
else:
    print('Incorrect password.')

print('All done!')
```

这几行打印('登录…')和打印('密码不正确。')是两个独立的代码块。这些语句只有一行长，但是 Python 允许您编写由任意数量的语句组成的代码块。

要在 Python 中表示一个代码块，必须将代码块的每一行缩进相同的量。我们的示例 if-statement 中的两个代码块都缩进了四个空格，这是 Python 的典型缩进量。

在大多数其他编程语言中，缩进只是用来帮助代码看起来漂亮。但是在 Python 中，它是指示一个语句属于哪个代码块所必需的。例如，最终的印刷(“全部完成！”)不是缩进的，因此不是 else 块的一部分。

熟悉其他语言的程序员常常对缩进很重要的想法感到恼火:许多程序员喜欢随心所欲地格式化他们的代码。然而，Python 缩进规则非常简单，大多数程序员已经使用缩进来使他们的代码可读。Python 只是将这一思想向前推进了一步，赋予了缩进意义。

### If/elif 语句

if/elif 语句是具有多个条件的广义 if 语句。它被用来做复杂的决定。例如，假设一家航空公司有以下“儿童”票价:2 岁或 2 岁以下的儿童免费乘坐飞机，2 岁以上但 13 岁以下的儿童支付折扣儿童票价，13 岁以上的人支付普通成人票价。以下程序决定了乘客应支付的费用:

```
# airfare.py
age = int(input('How old are you? '))
if age <= 2:
    print(' free')
elif 2 < age < 13:
    print(' child fare)
else:
    print('adult fare')
```

Python 从用户那里获得年龄后，它进入 if/elif 语句，并按照给出的顺序逐个检查每个条件。因此，它首先检查年龄是否小于 2，如果是，则表明飞行是自由的，并跳出 elif-条件。如果年龄不小于 2，则它检查下一个 elif 条件，以查看年龄是否在 2 和 13 之间。如果是这样，它打印适当的消息并跳出 if/elif 语句。如果 If 条件和 elif 条件都不为真，那么它执行 else 块中的代码。

### 条件表达式

Python 多了一个逻辑操作符，有些程序员喜欢(有些不喜欢！).它本质上是可以直接在表达式中使用的 if 语句的简写符号。考虑以下代码:

```
food = input("What's your favorite food? ")
reply = 'yuck' if food == 'lamb' else 'yum'
```

第二行中=右侧的表达式称为条件表达式，其计算结果为“yuck”或“yum”。它相当于以下内容:

```
food = input("What's your favorite food? ")
if food == 'lamb':
   reply = 'yuck'
else:
   reply = 'yum'
```

条件表达式通常比相应的 if/else 语句要短，尽管没有那么灵活或易读。一般来说，当它们使您的代码更简单时，您应该使用它们。

[Python 文档-缩进](https://docs.python.org/3/reference/lexical_analysis.html#indentation)