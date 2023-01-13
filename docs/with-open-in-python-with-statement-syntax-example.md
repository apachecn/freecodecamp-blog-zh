# Python 中的 With Open–With 语句语法示例

> 原文：<https://www.freecodecamp.org/news/with-open-in-python-with-statement-syntax-example/>

Python 编程语言具有各种用于处理文件的函数和语句。`with`语句和`open()`函数是这些语句和函数中的两个。

在本文中，您将学习如何使用`with`语句和`open()`函数来处理 Python 中的文件。

## `Open()`在 Python 中是做什么的？

要在 Python 中处理文件，必须先打开文件。因此,`open()`函数顾名思义——它为您打开一个文件，以便您可以使用该文件。

要使用 open 函数，首先要为它声明一个变量。`open()`函数最多接受 3 个参数——文件名、模式和编码。然后，您可以在打印功能中指定要对文件执行的操作。

```
my_file = open("hello.txt", "r")
print(my_file.read())

# Output : 
# Hello world
# I hope you're doing well today
# This is a text file 
```

这还不是全部。`open()`函数不关闭文件，所以您也必须用`close()`方法关闭文件。

因此，使用 open 函数的正确方法如下所示:

```
my_file = open("hello.txt", "r")
print(my_file.read())
my_file.close()

# Output : 
# Hello world
# I hope you're doing well today
# This is a text file 
```

读取模式是 Python 中默认的文件模式，所以如果不指定模式，上面的代码仍然可以正常工作:

```
my_file = open("hello.txt")
print(my_file.read())
my_file.close()

# Output : 
# Hello world
# I hope you're doing well today
# This is a text file 
```

## Python 中的`With`语句是如何工作的？

`with`语句与`open()`函数一起打开一个文件。

因此，您可以像这样重写我们在`open()`函数示例中使用的代码:

```
with open("hello.txt") as my_file:
    print(my_file.read())

# Output : 
# Hello world
# I hope you're doing well today
# This is a text file 
```

与必须用`close()`方法关闭文件的`open()`不同的是，`with`语句不用你告诉它就为你关闭了文件。

这是因为`with`语句在幕后调用了两个内置方法——`__enter()__`和`__exit()__`。

当您指定的操作完成时，`__exit()__`方法关闭文件。

使用`write()`方法，您也可以像我下面所做的那样写入文件:

```
with open("hello.txt", "w") as my_file:
    my_file.write("Hello world \n")
    my_file.write("I hope you're doing well today \n")
    my_file.write("This is a text file \n")
    my_file.write("Have a nice time \n")

with open("hello.txt") as my_file:
    print(my_file.read())

# Output: 
# Hello world 
# I hope you're doing well today
# This is a text file
# Have a nice time 
```

**您还可以遍历文件并逐行打印文本:
**

```
with open("hello.txt", "w") as my_file:
    my_file.write("Hello world \n")
    my_file.write("I hope you're doing well today \n")
    my_file.write("This is a text file \n")
    my_file.write("Have a nice time \n")

with open("hello.txt") as my_file:
    for line in my_file:
        print(line)

# Output:
# Hello world 

# I hope you're doing well today 

# This is a text file

# Have a nice time 
```

## 结论

您可能想知道应该使用哪种方式来处理组合了`with`和`open()`的文件，以及只有`open()`功能的文件。

我建议你使用`with`和`open()`的组合，因为`with`语句为你关闭了文件，你可以写更少的代码。

继续编码:)