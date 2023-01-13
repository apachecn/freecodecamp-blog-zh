# Python f-String 教程–用代码示例解释 Python 中的字符串格式

> 原文：<https://www.freecodecamp.org/news/python-f-strings-tutorial-how-to-use-f-strings-for-string-formatting/>

当你在 Python 中格式化字符串时，你可能习惯于使用`format()`方法。

但是在 Python 3.6 和更高版本中，你可以使用 *f 字符串*来代替。f 字符串，也称为*格式的字符串文字，*有更简洁的语法，在字符串格式化中非常有用。

在本教程中，您将学习 Python 中的 f 字符串，以及使用它们格式化字符串的几种不同方式。

## Python 中的 f 串是什么？

Python 中的字符串通常用双引号(`""`)或单引号(`''`)括起来。要创建 f 弦，你只需要在你的弦的左引号前加一个`f`或`F`。

> 例如，`"This"`是一个字符串，而`f"This"`是一个 f 字符串。

## 如何使用 Python f 字符串打印变量

当使用 f 字符串显示变量时，您只需要在一组花括号`{}`内指定变量的名称。并且在运行时，所有的变量名都将被它们各自的值所替换。

> 如果字符串中有多个变量，就需要将每个变量名用一对花括号括起来。

语法如下所示:

```
f"This is an f-string {var_name} and {var_name}."
```

这里有一个例子。

你有两个变量，`language`和`school`，在 f-String 里面用花括号括起来。

```
language = "Python"
school = "freeCodeCamp"
print(f"I'm learning {language} from {school}.")
```

让我们来看看输出:

```
#Output
I'm learning Python from freeCodeCamp.
```

注意变量`language`和`school`是如何被分别替换为`Python`和`freeCodeCamp`的。

## 如何使用 Python f 字符串计算表达式

因为 f 字符串是在运行时计算的，所以您也可以动态地计算有效的 Python 表达式。

在下面的例子中，`num1`和`num2`是两个变量。为了计算他们的乘积，你可以在一组花括号中插入表达式`num1 * num2`。

```
num1 = 83
num2 = 9
print(f"The product of {num1} and {num2} is {num1 * num2}.")
```

注意在输出中`num1 * num2`是如何被`num1`和`num2`的乘积所取代的。

```
#Output
The product of 83 and 9 is 747.
```

我希望你现在能够看到这个模式。

在任何 f 字符串中，`{var_name}`、`{expression}`作为变量和表达式的占位符，在运行时被相应的值替换。

请阅读下一部分，了解更多关于 f 弦的知识。

## 如何在 Python f 字符串中使用条件

我们先来回顾一下 Python 的`if..else`语句。通用语法如下所示:

```
if condition:
  # do this if condition is True <true_block>
else:
  # do this if condition is False <false_block>
```

这里，`condition`是检查真值的表达式。

*   如果`condition`的计算结果为`True`，则执行 If 块中的语句(`<true_block>`)。
*   如果`condition`评估为`False`，则执行 else 块(`<false_block>`)中的语句。

有一个更简洁的一行代码相当于上面的`if..else`块。语法如下所示:

```
<true_block> if <condition> else <false_block>
```

> 在上面的语法中，`<true block>`是当`condition`为`True`时所做的事情，`<false_block>`是当条件为`False`时要执行的语句。

如果你以前没有见过，这个语法可能看起来有点不同。如果让事情变得简单一点，你可以这样理解:“*做这个* `if` `condition`就是`True`；`else`、*做这个*”。

在 Python 中，这通常被称为*三元*运算符，因为它在某种意义上接受 3 个操作数——真块、*条件*测试和*假块*。

让我们举一个使用三元运算符的简单例子。

给定一个数字`num`，你想检查它是否是偶数。你知道一个数即使能被 2 整除也是偶数。让我们用这个来写我们的表达式，如下所示:

```
num = 87;
print(f"Is num even? {True if num%2==0 else False}")
```

在上面的代码片段中，

*   `num%2==0`是条件。
*   如果条件是`True`，你只需返回`True`表示`num`确实是偶数，否则返回`False`。

```
#Output
Is num even? False
```

上例中，`num`是 87，是奇数。因此，f 字符串中的条件语句被替换为`False`。

## 如何用 Python f 字符串调用方法

到目前为止，您只看到了如何打印变量值、计算表达式以及在 f 字符串中使用条件。是时候升级了。

让我们举以下例子:

```
author = "jane smith"
print(f"This is a book by {author}.")
```

上面的代码打印出`This is a book by jane smith.`

如果改为打印出`This is a book by Jane Smith.`不是更好吗？是的，在 Python 中，字符串方法返回经过必要修改的修改过的字符串。

> Python 中的`title()`方法返回一个按照标题大小写格式化的新字符串——这是名称通常的格式化方式(`First_name Last_name`)。

要以标题大小写格式打印出作者姓名，您可以执行以下操作:

*   对字符串`author`使用`title()`方法，
*   将返回的字符串存储在另一个变量中，并
*   使用 f 字符串打印它，如下所示:

```
author = "jane smith"
a_name = author.title()
print(f"This is a book by {a_name}.")

#Output
This is a book by Jane Smith.
```

然而，你可以用 f 弦一步完成。您只需要在 f 字符串的花括号内的字符串`author`上调用`title()`方法。

```
author = "jane smith"
print(f"This is a book by {author.title()}.")
```

当 f 字符串在运行时被解析时，

*   在字符串`author`上调用`title()`方法，并且
*   输出以标题大小写格式返回的字符串。

您可以在下面显示的输出中验证这一点。

```
#Output
This is a book by Jane Smith. 
```

您可以在花括号内的任何有效 Python 对象上进行方法调用，它们会工作得很好。

## 如何在 Python f-Strings 内部调用函数

除了调用 Python 对象上的方法，还可以调用 f 字符串内部的函数。它的工作原理和你之前看到的非常相似。

就像变量名被替换为值，表达式被替换为计算结果，函数调用被替换为函数的返回值。

让我们以下面所示的函数`choice()`为例:

```
def choice(c):
  if c%2 ==0:
    return "Learn Python!"
  else:
    return "Learn JavaScript!"
```

如果使用偶数作为参数调用上面的函数，它将返回`"Learn Python!"`。并且当函数调用中的参数是奇数时，它返回`"Learn JavaScript!"`。

在下面的例子中，您有一个 f 字符串，它调用了花括号内的 choice 函数。

```
print(f"Hello Python, tell me what I should learn. {choice(3)}")
```

由于参数是奇数(`3`)，Python 建议您学习 JavaScript，如下所示:

```
#Output
Hello Python, tell me what I should learn. Learn JavaScript!
```

如果你用偶数调用函数`choice()`，你会看到 Python 告诉你改为学习 Python。🙂

```
print(f"Hello Python, tell me what I should learn. {choice(10)}")
```

```
#Output
Hello Python, tell me what I should learn. Learn Python!
```

我们的教程到此圆满结束！

## 结论

在本教程中，您已经学习了如何使用 f 弦来:

*   打印变量值，
*   评估表达式，
*   调用其他 Python 对象上的方法，以及
*   调用 Python 函数。

### 相关职位

这里有一篇杰西卡写的[帖子](https://www.freecodecamp.org/news/python-string-format-python-s-print-format-example/)，解释了使用`format()`方法的字符串格式化。