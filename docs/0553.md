# Python 中的 key Error–如何修复字典错误

> 原文：<https://www.freecodecamp.org/news/keyerror-in-python-how-to-fix-dictionary-error/>

在 Python 中使用字典时，当您试图访问 Python 字典中不存在的项时，会引发 KeyError。

这里有一个叫做`student`的 Python 字典:

```
student = {
  "name": "John",
  "course": "Python",
}
```

在上面的字典中，您可以通过引用名称“John”的键–`name`来访问它。方法如下:

```
print(student["name"])
# John
```

但是当您试图访问一个不存在的键时，就会引发一个 KeyError。那就是:

```
student = {
  "name": "John",
  "course": "Python",
}

print(student["age"])
# ...KeyError: 'age'
```

当你是编写/测试代码的人时，这个问题很容易解决——你可以检查拼写错误，或者使用字典中已知的键。

但是在需要用户输入从字典中检索特定条目的程序中，用户可能不知道字典中存在的所有条目。

在本文中，您将看到如何修复 Python 字典中的 KeyError。

我们将讨论在执行程序之前检查字典中是否存在某个条目的方法，以及在找不到该条目时该怎么做。

## 如何修复 Python 中的字典 KeyError

我们将讨论的在 Python 中修复 KeyError 异常的两种方法是:

*   `in`关键字。
*   `try except`区块。

让我们开始吧。

### 如何使用`in`关键字修复 Python 中的 KeyError

我们可以使用`in`关键字来检查字典中是否存在某个条目。

使用一个`if...else`语句，如果项目存在，我们就返回它，或者向用户返回一条消息，通知他们找不到项目。

这里有一个例子:

```
student = {
  "name": "John",
  "course": "Python",
  "age": 20
}

getStudentInfo = input("What info about the student do you want? ")

if getStudentInfo in student:
    print(f"The value for your request is {student[getStudentInfo]}")
else:
	print(f"There is no parameter with the '{getStudentInfo}' key. Try inputing name, course, or age.")
```

让我们试着通过分解来理解上面的代码。

我们首先创建了一个名为`student`的字典，它有三个条目/键——`name`、`course`和`age`:

```
student = {
  "name": "John",
  "course": "Python",
  "age": 20
} 
```

接下来，我们创建了一个名为`getStudentInfo` : `getStudentInfo = input("What info about the student do you want? ")`的`input()`函数。我们将使用来自`input()`函数的值作为从字典中获取条目的键。

然后我们创建了一个`if...else`语句来检查来自`input()`函数的值是否匹配字典中的任何键:

```
if getStudentInfo in student:
    print(f"The value for your request is {student[getStudentInfo]}")
else:
	print(f"There is no parameter with the '{getStudentInfo}' key. Try inputing name, course, or age.")
```

根据上面的`if...else`语句，如果来自`input()`函数的值作为字典中的一项存在，`print(f"The value for your request is {student[getStudentInfo]}")`将运行。`student[getStudentInfo]`表示`student`字典，从`input()`函数获得的值作为关键字。

如果来自`input()`函数的值不存在，那么`print(f"There is no parameter with the '{getStudentInfo}' key. Try inputing name, course, or age.")`将运行，告诉用户他们的输入是错误的，并建议他们可以使用的可能的键。

继续运行代码-输入正确和错误的密钥。这将有助于验证上述解释。

### 如何使用`try except`关键字修复 Python 中的 KeyError

在`try except`模块中，`try`模块检查错误，而`except`模块处理任何发现的错误。

让我们看一个例子。

```
student = {
  "name": "John",
  "course": "Python",
  "age": 20
}

getStudentInfo = input("What info about the student do you want? ")

try:
    print(f"The value for your request is {student[getStudentInfo]}")
except KeyError:
    print(f"There is no parameter with the '{getStudentInfo}' key. Try inputing name, course, or age.")
```

就像我们在上一节中所做的一样，我们创建了字典和一个`input()`函数。

我们还为从`input()`函数得到的任何结果创建了不同的消息。

如果没有错误，只执行`try`块中的代码——这将返回用户输入的键值。

如果发现错误，程序将退回到`except`块，告诉用户该键不存在，同时建议可能使用的键。

## 摘要

在本文中，我们讨论了 Python 中的 KeyError。当我们试图访问一个不存在于 Python 字典中的条目时，就会出现这个错误。

我们看到了两种可以用来解决这个问题的方法。

我们首先看到了如何在执行代码之前使用`in`关键字来检查一个项目是否存在。

最后，我们使用`try except`块创建了两个代码块——如果项目存在,`try`块成功运行，而如果项目不存在,`except`块运行。

编码快乐！