# python strip()-如何修剪字符串或线条

> 原文：<https://www.freecodecamp.org/news/python-strip-how-to-trim-a-string-or-line/>

在本文中，您将学习如何使用`.strip()`方法在 Python 中修剪字符串。

您还将看到如何使用`.lstrip()`和`.rstrip()`方法，它们是`.strip()`的对应方法。

我们开始吧！

## 如何在 Python 中修剪字符串

Python 有**三个**内置方法，用于修剪字符串中的前导和尾随空格和字符。

*   `.strip()`
*   `.lstrip()`
*   `.rstrip()`

每个方法都返回一个新的修整过的字符串。

### 如何在 Python 中移除字符串中的前导和尾随空格

当`.strip()`方法没有参数时，它从字符串中删除任何前导和/或尾随空白。

因此，如果在单词或短语的开头和/或结尾有空格，默认情况下，`.strip()`会单独删除它。

下面的变量`greeting`存储了字符串“Hello”。该字符串的左右两边都有空格。

```
greeting = "     Hello!  "

print(greeting,"How are you?")

#output
#     Hello!   How are you? 
```

要删除它们，您可以使用`.strip()`方法，就像这样:

```
greeting = "     Hello!  "

stripped_greeting = greeting.strip()

print(stripped_greeting,"How are you?")

#output
#Hello! How are you? 
```

您也可以这样使用`.strip()`方法:

```
greeting = "     Hello!  "

print(greeting.strip(),"How are you?")

#output
#Hello! How are you? 
```

### 如何在 Python 中移除字符串中的前导和尾随字符

`.strip()`方法将*可选的*字符作为参数传递。

作为参数添加的字符指定了要从字符串的开头和结尾删除的字符。

下面是这种情况的一般语法:

```
str.strip(char) 
```

您指定的字符用引号括起来。

例如，假设您有以下字符串:

```
greeting = "Hello World?" 
```

你想去掉“H”和“？”，它们分别位于字符串的开头和结尾。

要删除它们，需要将这两个字符作为参数传递给`strip()`。

```
greeting = "Hello World?"

stripped_greeting = greeting.strip("H?")

print(stripped_greeting)

#output
#ello World 
```

请注意，当您想要从“World”中删除“W”时会发生什么，因为“W”位于字符串的中间，而不是开头或结尾，并且您将它作为参数包括在内:

```
greeting = "Hello World?"

stripped_greeting = greeting.strip("HW?")

print(stripped_greeting)
#ello World 
```

它不会被删除！只有在所述字符串的**开始**和**结束**的字符被删除。

话虽如此，看看下一个例子。

假设您想要删除字符串的前两个和后两个字符:

```
phrase = "Hello world?"

stripped_phrase = phrase.strip("Hed?")

print(stripped_phrase)

#output
#llo worl 
```

前两个字符(“他”)和后两个(“d？”)已被移除。

另一件要注意的事情是，参数不仅仅删除指定字符的第一个实例。

例如，假设您有一个字符串，开头有几个句点，结尾有几个感叹号:

```
phrase = ".....Python !!!" 
```

当您指定参数`.`和`!`时，两者的所有实例都将被删除:

```
phrase = ".....Python !!!"

stripped_phrase = phrase.strip(".!")

print(stripped_phrase)

#output
#Python 
```

### 如何在 Python 中只移除字符串中的前导空格和字符

要仅删除*的*前导空格和字符，请使用`.lstrip()`。

当您只想删除字符串开头的空白和字符时，这很有帮助。

这方面的一个例子是从域名中删除`www.`。

```
domain_name = "www.freecodecamp.org www."

stripped_domain = domain_name.lstrip("w.")

print(stripped_domain)

#output
#freecodecamp.org www. 
```

在这个例子中，我在字符串的开头和结尾都使用了`w`和`.`字符来展示`.lstrip()`是如何工作的。

如果我使用了`.strip(w.)`,我会得到如下输出:

```
freecodecamp.org 
```

同样的道理也适用于删除空白。

让我们看一个上一节的例子:

```
greeting = "     Hello!  "

stripped_greeting = greeting.lstrip()

print(stripped_greeting,"How are you?" )

#output
#Hello!   How are you? 
```

输出中只删除了字符串开头的空格。

### 如何在 Python 中仅删除字符串中的尾随空格和字符

要仅删除*后面的空白和字符*，使用`.rstrip()`方法。

假设您只想删除字符串末尾的所有标点符号。

您将执行以下操作:

```
enthusiastic_greeting = "!!! Hello !!!!"

less_enthusiastic_greeting = enthusiastic_greeting.rstrip("!")

print(less_enthusiastic_greeting)

#output
#!!! Hello 
```

空白也是如此。

再次以前面的例子为例，这一次将只从输出的末尾删除空白:

```
greeting = "     Hello!  "

stripped_greeting = greeting.rstrip()

print(stripped_greeting,"How are you?")

#output
#     Hello! How are you? 
```

## 结论

现在你知道了！现在，您已经了解了如何在 Python 中修剪字符串的基础知识。

总而言之:

*   使用`.strip()`方法删除字符串开头**和结尾**的空白和字符。
*   使用`.lstrip()`方法只删除从字符串的**开始的**中的空白和字符。
*   使用`.rstrip()`方法只移除字符串的**端**的空白和字符。

如果你想了解更多关于 Python 的知识，可以查看 freeCodeCamp 的 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。你将开始以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢阅读和快乐编码！