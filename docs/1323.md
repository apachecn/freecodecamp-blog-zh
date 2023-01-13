# Python 中的 Print 语句–如何使用示例语法命令进行打印

> 原文：<https://www.freecodecamp.org/news/print-statement-in-python-how-to-print-with-example-syntax-command/>

作为一名初学程序员，如何打印出信息是你首先要学习的事情之一。

本文介绍了关于用 Python 打印您需要了解的内容，并且我们将在此过程中查看大量代码示例。

我们开始吧！

## 印刷品是用来做什么的？

当您开始 Python 学习之旅时，打印很可能是您将学习的第一件事。

编写“Hello World”程序作为第一行代码是一种传统。您可以使用`print()`函数将这段文本输出到控制台。

打印主要用于向控制台显示信息，无论是显示特定的消息还是计算结果。但是它也用于调试目的。

### Python 2 中的打印与 Python 3 中的打印

在 Python 2 中，为了将内容打印到控制台，您只需使用`print`关键字:

```
print "Hello world"

#output
#Hello world 
```

这被称为打印声明。

在 Python 3 中，print 语句被`print()`函数所取代。

```
print("Hello world")

#output
#Hello world 
```

如果在 Python 3 中没有添加跟随在`print`后面的一组左括号和右括号，那么在运行代码时会出现错误:

```
print "Hello world"

#output
#File "/Users/dionysialemonaki/python_articles/demo.py", line 1
#    print "Hello world"
#   ^^^^^^^^^^^^^^^^^^^
#SyntaxError: Missing parentheses in call to 'print'. Did you mean #print(...)? 
```

所以在 Python 2 中，print 关键字是一个语句，而在 Python 3 中，print 是一个函数。

## `print()`Python 中的语法

下面显示了`print()`函数的完整语法，以及它所采用的参数的默认值。

这是`print()`在引擎盖下的样子:

```
print(*object,sep=' ',end='\n',file=sys.stdout,flush= False) 
```

让我们来分解一下:

*   `*object`可以是零个、一个或多个要打印的数据值，可以是任何数据类型。对象在打印前被转换成字符串。
*   `sep`是一个可选参数，指定如何分离多个对象。默认为`' '`–一个空格。
*   `end`是一个可选参数，用于指定行的结尾。默认情况下，print 调用以换行符结束，其中`\n`是换行符。
*   `file`是一个可选的参数，它是一个具有写方法的对象——它可以将输出写入并附加(添加)到文件中。默认为`sys.stdout`(或系统标准输出)，输出显示在屏幕上。
*   `flush`是一个布尔参数，指定流是被强制刷新还是被缓冲。刷新表示打印调用是否会立即生效。默认值为 False(或缓冲)。

`print()`函数没有返回值。

## 如何用 Python 打印对象

即使您没有向`print()`传递任何参数——也就是说，您没有传递任何要打印的对象——您仍然需要包含一组空括号。

在这种情况下，这只会向控制台输出一个空行。

这在使用 Python REPL 时得到了最好的说明(阅读 Eval 打印循环)。

要启动一个新的会话，在机器上安装 Python 之后，键入`python3`。完成后，键入`exit()`结束会话。

```
>>> print()

>>> 
```

这类似于在文字处理器中写作时敲击键盘上的`Enter`键。就像 Enter 键创建一个新行并将光标移动到一个新行一样，以同样的方式不带参数调用`print()`会显示一个空行。

## 如何在 Python 中打印字符串

通过将字符串文字作为参数传递给`print()`来打印字符串，用单引号或双引号括起来。

输出将是字符串文字，没有引号。

```
print("I am learning Python")

#output
#I am learning Python 
```

如果您有一个想要打印的字符串或短语，您可以将它存储在一个变量中，并将变量名作为参数传递给`print()`。

```
greeting = "Hey there!"

print(greeting)

#output
#Hey there! 
```

根据变量中存储的内容，给变量起一个有意义的名字是一个最佳实践。这将使代码对你自己和与你一起工作的任何人来说更具可读性。

如前面的`print()`语法所示，可以有多个对象作为参数传递(`*object`)。

您可以通过用逗号分隔每个参数**来做到这一点。**

```
print("Hello","there!")

#output
#Hello there! 
```

有两种说法:“你好”和“那里！”。

在下面的例子中，参数是一个字符串和一个变量。

```
full_name = "John Doe"

print("Hey",full_name)

#output
#Hey John Doe 
```

您也可以使用带有加法运算符的**串联**来实现这一点:

```
full_name = "John Doe"

print("Hey " + full_name)

#output
#Hey John Doe 
```

对于连接，您必须考虑空格，否则您将得到以下结果:

```
full_name = "John Doe"

print("Hey" + full_name)

#output
#HeyJohn Doe 
```

请记住，您不能添加带数字的字符串文字。

这将导致一个错误:

```
print("Hey" + 7)

#output
#Traceback (most recent call last):
#  File "/Users/dionysialemonaki/python_articles/demo.py", line 1, in #<module>
#    print("Hey" + 7)
#TypeError: can only concatenate str (not "int") to str 
```

如该错误所示，您只能将一个字符串连接(添加)到一个字符串。这意味着如果你想包含一个数字，你必须通过类型转换把它转换成等价的字符串。

```
#use the str() method to convert an integer to a string

print("Hey" + str(7))

#output
#Hey 7 
```

现代打印物体的方法是使用 **`f-strings`** 。

```
full_name = "John Doe"

print(f"Hey there {full_name}")

#output
#Hey there John Doe 
```

字符串不是唯一可以作为参数传递的对象。

事实上，`print()`接受所有类型的数据，您将在下一节中看到。

### 如何打印 Python 内置数据类型的例子

```
#how to print ints
print(7) #output is 7

#how to print floats
print(7.0) #output is 7.0

#how to print complex
print(1j) #output is 1j

#how to print a list
print([10,20,30]) #output is [10,20,30]

#how to print a tuple
print((10,20,30)) #output is (10,20,30)

#how to print a dictionary
print({"language": "Python", "field": "data science"})
#output is {"language": "Python", "field": "data science"

#how to print a set
print({"autumn","winter","spring","summer"})
#output is {"autumn","winter","spring","summer"}

#how to print a bool
print(True) #output is True
print(False) #output is False 
```

## 如何在`print()`功能中改变对象的分离方式

正如您在`print()`函数的语法中看到的，`sep`决定了一个对象如何与下一个对象分离。

默认情况下，对象由一个空格分隔。

```
print("Hello","World")

#output
#Hello World 
```

要禁用它，您可以显式地将`sep`的值更改为您想要的字符。

例如，对象可以用破折号分隔:

```
print("Hello","World", sep="---")

#output
#Hello---World 
```

或者，您甚至可以通过添加一个空字符串来删除空格:

```
print("Hello","World", sep="")

#output
#HelloWorld 
```

## 如何移除`print()`中的默认换行符

正如您在前面的`print()`函数的语法分析中看到的，关键字参数`end`的默认参数是`'\n'`。

默认情况下，每次打印调用后，都会创建一个新行。

如果您分别调用 print 两次，一次接一次，您会看到第二次调用在第一次调用后立即显示在一个新行上。

```
print("Hello")
print("World")

#output
#Hello
#World 
```

要禁用它，您可以显式地将`end`的值更改为空字符串`""`。

```
print("Hello ", end="")
print("World")

#output
#Hello World 
```

现在两者在同一条线上。

你甚至可以把它改成句号:

```
print("Hello", end=".")
print("World")

#output
#Hello.World 
```

任何非默认值的内容都将禁止创建新行。

## 如何在 Python 中将`print()`输出指向一个文件

您通常希望将输出打印到标准输出，或者命令行标准输出。

但是，有时您可能希望将输出指向一个现有的文件。

假设您有一个文本文件，并想使用`print()`函数添加一些文本。

要在 Python 中打开和写入文件，可以调用`open()`函数。在它里面你包括了文件名，在这个例子中是`output.txt`和`-w`模式，意思是只写。

在这种模式下，每次运行代码时，文件的内容都将被删除，并替换为您添加的任何新文本。

如果您不想丢失任何内容，您可以使用`-a`模式，将文本附加到文件的末尾。

在`print()`函数中，你添加任何你想添加到文件中的文本，并设置`file`参数等于你为你想添加文本的文件创建的占位符名称，在本例中是`f`。

```
with open('output.txt', 'w') as f:
    print('Hello World!', file=f) 
```

## 结论

感谢阅读并坚持到最后！我希望这篇教程对你有所帮助。您现在知道了如何在 Python 中使用`print()`函数的基本知识。

如果你有兴趣学习更多关于 Python 编程语言的知识，可以查看 freeCodeCamp 的 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)来开始学习。

它涵盖了所有基本的编程概念，并逐步发展到更高级的主题。最后，您还将构建五个项目来巩固您的学习。

编码快乐！