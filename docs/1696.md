# Python 函数示例–如何用参数声明和调用

> 原文：<https://www.freecodecamp.org/news/python-function-examples-how-to-declare-and-invoke-with-parameters-2/>

Python 有一堆有用的内置函数，可以用来做各种事情。每个人都执行特定的任务。

但是您知道 Python 也允许您定义自己的函数吗？

本文将向您展示如何创建和调用您自己的 Python 函数。它还将概述如何向函数传递输入参数和自变量。

## 什么是函数？

函数是执行特定任务的独立代码块。

函数在编程中很有用，因为它们消除了程序中不必要的和过多的代码复制和粘贴。

如果在不同的地方经常需要某个动作，这是一个很好的迹象，表明你可以为它编写一个函数。函数应该是可重用的。

函数还有助于组织您的代码。

如果你需要做一些改变，你只需要更新特定的功能。这使您不必通过复制和粘贴来搜索分散在程序中不同位置的相同代码的不同部分。

这符合软件开发中的 DRY(不要重复自己)原则。

函数内部的代码只有在函数被调用时才会运行。

函数可以接受参数和默认值，一旦代码运行，可能会也可能不会将值返回给调用者。

## 如何在 Python 中定义函数

在 Python 中创建函数的一般语法如下所示:

```
def function_name(parameters):
    function body 
```

让我们来分析一下这里发生了什么:

*   `def`是一个关键字，告诉 Python 一个新的函数正在被定义。
*   接下来是您选择的有效函数名。有效名称以字母或下划线开头，但可以包含数字。单词是小写的，用下划线隔开。要知道函数名不能是 Python 保留的关键字，这一点很重要。
*   然后我们有一组左右括号，`()`。在它们内部，可以有零个、一个或多个*可选的*逗号分隔的参数以及它们的*可选的*默认值。这些被传递给函数。
*   接下来是冒号`:`，它结束了函数的定义行。
*   然后是一个新行，后面跟着一级缩进(你可以用键盘上的 4 个空格或 1 个制表符来代替)。缩进很重要，因为它让 Python 知道什么代码属于函数。
*   然后我们有了函数体。下面是要执行的代码——函数被调用时要采取的动作的内容。
*   最后，在函数体中有一个*可选的* return 语句，在函数退出时将一个值传回给调用者。

请记住，如果您在尝试定义新函数时忘记了括号`()`或冒号`:`，Python 会用一个`SyntaxError`通知您。

## 如何在 Python 中定义和调用一个基本函数

下面是一个基本函数的例子，它没有 return 语句，也没有任何参数。

它只是在被调用时打印`hello world`。

```
def hello_world_func():
    print("hello world") 
```

一旦你定义了一个函数，代码就不会自己运行。

为了执行函数内部的代码，你需要进行一个*函数调用*或者一个*函数调用*。

然后，您可以根据需要多次调用该函数。

要调用一个函数，您需要这样做:

```
function_name(arguments) 
```

下面是代码的细目分类:

*   键入函数名。
*   函数名后面必须有括号。如果有任何必需的参数，它们必须在括号中传递。如果函数不接受任何参数，你仍然需要括号。

要调用上面示例中的函数，该函数不接受任何参数，请执行以下操作:

```
hello_world_func()

#Output
#hello world 
```

## 如何定义和调用带参数的函数

到目前为止，您已经看到了一些简单的功能，除了将一些内容打印到控制台之外，它们实际上并没有做多少事情。

如果你想传递一些额外的数据给函数呢？

我们在这里使用了像*参数*和*参数*这样的术语。

他们的定义到底是什么？

参数是一个命名的占位符，它将信息传递给函数。

它们充当在函数定义行中本地定义的变量。

```
def hello_to_you(name):
   print("Hello " + name) 
```

在上面的例子中，有一个参数`name`。

我们可以使用`f-string formatting`来代替——它的工作方式与前面的例子相同:

```
def hello_to_you(name):
    print(f"Hello {name}") 
```

括号内可以有一个参数列表，所有参数用逗号分隔。

```
def name_and_age(name,age):
    print(f"I am {name} and I am {age} years old!") 
```

这里，函数中的参数是`name`和`age`。

当函数被调用时，参数被传入。

像参数一样，实参也是传递给函数的信息。

特别是，它们是与函数定义中的参数相对应的实际值。

调用前面示例中的函数并传入一个参数，如下所示:

```
def hello_to_you(name):
    print(f"Hello {name}")

hello_to_you("Timmy")
#Output
# Hello Timmy 
```

该函数可以多次调用，每次传入不同的值。

```
def hello_to_you(name):
    print(f"Hello {name}")

hello_to_you("Timmy")
hello_to_you("Kristy")
hello_to_you("Jackie")
hello_to_you("Liv")

#Output:
#"Hello Timmy"
#"Hello Kristy"
#"Hello Jackie"
#"Hello Liv" 
```

到目前为止提出的论点被称为**位置论点**。

所有的位置参数都需要。

### 立场论据的数量很重要

调用函数时，需要传递正确数量的参数，否则会出错。

对于位置参数，传递给函数调用的参数数量必须与函数定义中的参数数量完全相同。

你不能再遗漏或增加任何东西了。

假设您有一个接受两个参数的函数:

```
def hello_to_you(first_name,last_name):
    print(f"Hello, {first_name} {last_name}") 
```

如果调用函数时只传入一个参数，就像这样，将会出现错误:

```
def hello_to_you(first_name,last_name):
    print(f"Hello, {first_name} {last_name}")

hello_to_you("Timmy") 
```

输出:

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: hello_to_you() missing 1 required positional argument: 'last_name' 
```

如果调用该函数时传入了三个参数，将再次出现错误:

```
def hello_to_you(first_name,last_name):
    print(f"Hello, {first_name} {last_name}")

hello_to_you("Timmy","Jones",34) 
```

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: hello_to_you() takes 2 positional arguments but 3 were given 
```

如果我们传入 *no* 参数，也会有一个错误。

```
def hello_to_you(first_name,last_name):
    print(f"Hello, {first_name} {last_name}")

hello_to_you() 
```

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: hello_to_you() missing 2 required positional arguments: 'first_name' and 'last_name' 
```

相反，我们需要两个参数，因为函数定义中列出了两个参数。

```
def hello_to_you(first_name,last_name):
    print(f"Hello, {first_name} {last_name}")

hello_to_you("Timmy","Jones")
#Output:
# Hello,Timmy Jones 
```

### 立场论点的顺序很重要

除了包含参数的正确数量*和*之外，重要的是要注意参数传递的*顺序*。

参数的传递顺序需要与函数定义中声明的参数的顺序完全相同。

它的工作原理类似于变量赋值。

第一个参数是函数定义中第一个参数的值。第二个参数是函数的数据定义中第二个参数的值，依此类推。

```
def hello_to_you(first_name,last_name):
    print(f"Hello, {first_name} {last_name}")

hello_to_you("Timmy","Jones")
#Output
#Hello,Timmy Jones 
#you can visualise it like:
#first_name = "Timmy"
#last_name = "Jones" 
```

如果顺序不正确，输出可能没有多大意义，也不会是您想要的样子。

```
def fave_language(name,language):
    print(f"Hello, I am {name} and my favorite programming language is {language}")

fave_language("Python","Dionysia")
#output:
#Hello, I am Python and my favorite programming language is Dionysia
#it is like you did
#name="Python"
#language = "Dionysia" 
```

### 如何在 Python 函数中使用关键字参数

到目前为止，你们已经看到了一种论点，位置论点。

使用位置参数，调用函数时只使用函数调用中传递的值。在这里，每个值直接对应于函数定义中每个参数的数量、顺序和位置。

然而，Python 中的函数可以接受另一种类型的参数，即**关键字参数**。

在这种情况下，不只是在函数调用中传递值，而是指定参数名，然后以`key = value`的形式指定要赋予它的值。

每个键匹配函数定义中的每个参数。

显式地调用参数的名称和它们所取的值有助于更加清楚地了解你所传递的内容，并避免任何可能的混淆。

```
def fave_language(name,language):
    print(f"Hello, I am {name} and my favorite programming language is {language}")

fave_language(name="Dionysia",language="Python")
#output:
#Hello, I am Dionysia and my favorite programming language is Python 
```

如上所述，关键字参数可以按照特定的顺序排列。但是它们比位置论点更灵活，因为论点的顺序不再重要。

所以你可以这样做，不会有错误:

```
def fave_language(name,language):
    print(f"Hello, I am {name} and my favorite programming language is {language}")

fave_language(language="Python",name="Dionysia")
#output:
#Hello, I am Dionysia and my favorite programming language is Python 
```

但是你仍然需要给出正确的参数个数。

您可以在函数调用中同时使用位置参数和关键字参数，如下例所示，其中有一个位置参数和一个关键字参数:

```
def fave_language(name,language):
    print(f"Hello, I am {name} and my favorite programming language is {language}")

fave_language("dionysia",language="Python")
#output:
#Hello, I am dionysia and my favorite programming language is Python 
```

在这种情况下，顺序再次变得重要。

位置参数总是在最前面，所有关键字参数都应该跟在位置参数后面。否则会出现一个错误:

```
def fave_language(name,language):
    print(f"Hello, I am {name} and my favorite programming language is {language}")

fave_language(language="Python","dionysia") 
```

### 如何在 Python 中定义带有默认值的参数

函数参数也可以有默认值。它们被称为*默认或可选参数*。

要使函数参数具有默认值，必须在函数定义中为参数分配默认值。

您可以使用`key=value`表单来实现，其中`value`将是该参数的默认值。

```
def fave_language(language="python"):
    print(f"My favorite  programming language is {language}!")

fave_language()
#output
#My favorite  programming language is python! 
```

如您所见，默认参数是*，而不是函数调用中所需的*，如果调用中没有提供另一个值，`language`的值将总是默认为 Python。

但是，如果在函数调用中提供另一个值，默认值很容易被覆盖:

```
def fave_language(language="python"):
    print(f"My favorite  programming language is {language}!")

fave_language("Java")
#prints "My favorite  programming language is Java!" to the console 
```

可以有多个默认值传递给该函数。

调用该函数时，可以不提供、提供一个、一些或所有默认参数，并且顺序无关紧要。

```
def personal_details(name="Jimmy",age=28,city="Berlin"):
    print(f"I am {name},{age} years old and live in {city}") 

#We can do:

personal_details()
#output:
#I am Jimmy,28 years old and live in Berlin

personal_details(age=30)
#I am Jimmy,30 years old and live in Berlin

personal_details(city="Athens",name="Ben",age=24)
##I am Ben,24 years old and live in Athens 
```

在函数调用中，默认参数可以与非默认参数结合使用。

下面是一个接受两个参数的函数:一个非默认的位置参数(`name`)和一个可选的默认参数(`language`)。

```
def fave_language(name,language="Python"):
    print(f"My name is {name} and my favorite  programming language is {language}!")

fave_language("Dionysia")
#output:
#"My name is Dionysia and my favorite  programming language is Python!" 
```

默认参数`langyage="Python"`是*可选的*。但是位置参数`name`总是必需的。如果没有传入另一个`language`，该值将始终默认为 Python。

这里要提到的另一件事是，当缺省值和非缺省值一起使用时，它们在函数定义中的定义顺序很重要。

所有的位置参数都在前面，后面是默认参数。

这是行不通的:

```
def fave_language(language="Python",name):
    print(f"My name is {name} and my favorite  programming language is {language}!")

fave_language("Dionysia") 
```

输出:

```
 File "<stdin>", line 1
SyntaxError: non-default argument follows default argument 
```

## 结论

在本文中，您学习了如何在 Python 编程语言中声明函数并使用参数调用它们。

这是关于如何创建简单函数以及如何通过参数向它们传递数据的介绍。我们还讨论了不同类型的参数，如*位置参数*、*关键字*和*默认参数*。

概括一下:

*   *位置*参数的顺序和数量很重要。
*   有了*关键字*参数，顺序并不重要。不过，数字确实很重要，因为每个关键字参数都对应于函数定义中的每个参数。
*   *默认*参数完全是可选的。你可以全部传入，部分传入，或者一个都不传入。

如果你有兴趣更深入地了解 Python 编程语言，freeCodeCamp 有一个[免费的 Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

您将从这门语言的基础开始，然后学习更高级的概念，如数据结构和关系数据库。最后，您将构建 5 个项目来实践您所学的内容。

感谢阅读，快乐学习。