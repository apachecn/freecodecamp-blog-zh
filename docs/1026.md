# Python 列表长度——如何在 Python 中获得列表的大小

> 原文：<https://www.freecodecamp.org/news/python-list-length-how-to-get-the-size-of-a-list-in-python/>

在 Python 中，使用列表来存储各种类型的数据，如字符串和数字。

列表可以用方括号来标识，各个值用逗号分隔。

要获得 Python 中列表的长度，可以使用内置的`len()`函数。

除了`len()`函数，你还可以使用 for 循环和`length_hint()`函数来获得一个列表的长度。

在这篇文章中，我将向你展示如何用三种不同的方法获得一个列表的长度。

## 如何在 Python 中用 For 循环获得列表的长度

您可以使用 Python 的本机 for 循环来获取列表的长度，因为就像元组和字典一样，列表是可迭代的。

这种方法通常被称为天真的方法。

下面的例子展示了如何使用简单的方法在 Python 中获得一个列表的长度

```
demoList = ["Python", 1, "JavaScript", True, "HTML", "CSS", 22]

# Initializing counter variable
counter = 0

for item in demoList:
    # Incrementing counter variable to get each item in the list
    counter = counter + 1

    # Printing the result to the console by converting counter to string in order to get the number
print("The length of the list using the naive method is: " + str(counter))
# Output: The length of the list using the naive method is: 7 
```

## 如何用`len()`函数得到列表的长度

使用`len()`函数是获取 iterable 长度的最常见方法。

这比使用 for 循环更简单。

使用`len()`方法的语法是`len(listName)`。

下面的代码片段展示了如何使用`len()`函数来获取列表的长度:

```
demoList = ["Python", 1, "JavaScript", True, "HTML", "CSS", 22]

sizeOfDemoList = len(demoList)

print("The length of the list using the len() method is: " + str(sizeOfDemoList))
# Output: The length of the list using the len() method is: 7 
```

## 如何用`length_hint()`函数得到列表的长度

`length_hint()`方法是一种不太为人所知的获取列表和其他可重复项长度的方法。

`length_hint()`是在操作员模块中定义的，需要从那里导入才能使用。

使用`length_hint()`方法的语法是`length_hint(listName)`。

下面的例子展示了如何使用`length_hint()`方法从操作符 import length_hint 中获取列表的长度:

```
demoList = ["Python", 1, "JavaScript", True, "HTML", "CSS", 22]

sizeOfDemoList = length_hint(demoList)
print("The length of the list using the length_hint() method is: " + str(sizeOfDemoList))
# The length of the list using the length_hint() method is: 7 
```

## 最后的想法

本文向您展示了如何用 3 种不同的方法获得列表的大小:for 循环、`len()`函数和来自操作符模块的`length_hint()`函数。

您可能想知道在这三种方法中使用哪一种。

我建议你使用`len()`，因为与 for 循环和`length_hint()`相比，你不需要做太多就可以使用它。

另外，`len()`似乎比 for 循环和`length_hint()`都要快。

如果你觉得这篇文章有帮助，请分享它，让其他需要的人也能看到。