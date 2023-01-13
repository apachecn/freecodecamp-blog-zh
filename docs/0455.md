# Python Switch 语句–Switch 案例示例

> 原文：<https://www.freecodecamp.org/news/python-switch-statement-switch-case-example/>

在 3.10 版本之前，Python 从来没有实现 switch 语句在其他编程语言中的功能。

所以，如果你想执行多个条件语句，你必须像这样使用`elif`关键字:

```
age = 120

if age > 90:
    print("You are too old to party, granny.")
elif age < 0:
    print("You're yet to be born")
elif age >= 18:
    print("You are allowed to party")
else: 
    "You're too young to party"

# Output: You are too old to party, granny. 
```

从 3.10 版本开始，Python 实现了一个名为“结构模式匹配”的切换用例特性。您可以用关键字`match`和`case`来实现这个特性。

有些人争论`match`和`case`是否是 Python 中的关键字。这是因为您可以将它们用作变量名和函数名。但这是另一个故事了。

如果您愿意，可以将这两个关键词都称为“软关键词”。

在本文中，我将向您展示如何使用关键字`match`和`case`用 Python 编写 switch 语句。

但在此之前，我必须向您展示 Python 程序员过去是如何模拟 switch 语句的。

## Python 程序员如何模拟 Switch Case

在过去，Pythonistas 有多种方法模拟 switch 语句。

使用函数和关键字`elif`就是其中之一，你可以这样做:

```
def switch(lang):
    if lang == "JavaScript":
        return "You can become a web developer."
    elif lang == "PHP":
        return "You can become a backend developer."
    elif lang == "Python":
        return "You can become a Data Scientist"
    elif lang == "Solidity":
        return "You can become a Blockchain developer."
    elif lang == "Java":
        return "You can become a mobile app developer"

print(switch("JavaScript"))   
print(switch("PHP"))   
print(switch("Java"))  

"""
Output: 
You can become a web developer.
You can become a backend developer.
You can become a mobile app developer
""" 
```

## 如何在 Python 3.10 中用`match`和`case`关键字实现 Switch 语句

要使用结构化模式匹配功能编写 switch 语句，可以使用下面的语法:

```
match term:
    case pattern-1:
         action-1
    case pattern-2:
         action-2
    case pattern-3:
         action-3
    case _:
        action-default 
```

请注意，下划线符号是 Python 中用于定义 switch 语句默认情况的符号。

下面显示了一个用 match case 语法编写的 switch 语句的示例。这是一个程序，它打印了当你学习各种编程语言时你会成为什么样的人:

```
lang = input("What's the programming language you want to learn? ")

match lang:
    case "JavaScript":
        print("You can become a web developer.")

    case "Python":
        print("You can become a Data Scientist")

    case "PHP":
        print("You can become a backend developer")

    case "Solidity":
        print("You can become a Blockchain developer")

    case "Java":
        print("You can become a mobile app developer")
    case _:
        print("The language doesn't matter, what matters is solving problems.") 
```

这比多个`elif`语句和用函数模拟 switch 语句要干净得多。

您可能已经注意到，我没有像在其他编程语言中那样，在每种情况下都添加 break 关键字。这就是 Python 的本地 switch 语句相对于其他语言的优势。break 关键字的功能是在幕后完成的。

## 结论

本文向您展示了如何使用“match”和“case”关键字编写 switch 语句。您还了解了 Python 程序员在 3.10 版之前是如何编写它的。

Python match 和 case 语句的实现是为了提供其他编程语言(如 JavaScript、PHP、C++)中的 switch 语句特性所提供的功能。

感谢您的阅读。