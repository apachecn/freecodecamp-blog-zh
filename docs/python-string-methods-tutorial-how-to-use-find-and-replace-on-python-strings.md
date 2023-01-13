# Python 字符串方法教程–如何对 Python 字符串使用 find()和 replace()

> 原文：<https://www.freecodecamp.org/news/python-string-methods-tutorial-how-to-use-find-and-replace-on-python-strings/>

在 Python 中处理字符串时，您可能需要在字符串中搜索模式，甚至用另一个子字符串替换部分字符串。

Python 有有用的字符串方法`find()`和`replace()`来帮助我们执行这些字符串处理任务。

在本教程中，我们将通过示例代码了解这两个字符串方法。

## Python 字符串的不变性

Python 中的字符串是*不可变的*。因此，我们不能在处修改字符串*。*

> 字符串是遵循零索引的 Python 可迭代对象。长度为`n`的字符串的有效索引是`0,1,2,..,(n-1)`。
> 
> 我们也可以使用*负索引*，其中最后一个元素在索引`-1`处，倒数第二个元素在索引`-2`处，依此类推。

例如，我们有保存字符串`"writer"`的`my_string`。让我们通过将最后一个字符设置为`"s"`来尝试将它更改为`"writes"`。

```
my_string = "writer" 
```

让我们试着给字母`"s"`分配最后一个字符，如下所示。通过负索引，我们知道`my_string`中最后一个字符的索引是`-1`。

```
my_string[-1]="s"

# Output
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-1-670491032ba6> in <module>()
      1 my_string = "writer"
----> 2 my_string[-1]="s"

TypeError: 'str' object does not support item assignment
```

如上面的代码片段所示，我们得到一个`TypeError`。这是因为字符串对象是不可变的 T2，我们试图执行的赋值是无效的。

但是，我们可能经常需要修改字符串。字符串方法帮助我们轻松地做到这一点。

> 字符串方法对现有的字符串*进行操作，而*返回新的*修改后的字符串。现有的字符串是*而不是*修改的。*

让我们转到下一节来学习`find()`和`replace()`方法。

## 如何使用 find()在 Python 字符串中搜索模式

您可以使用 Python 的`find()`方法在字符串中搜索模式。一般语法如下所示。

```
<this_string>.find(<this_pattern>)
```

我们想要搜索的输入字符串由占位符`this_string`表示。占位符`this_pattern`给出了我们想要搜索的模式。

现在让我们来解析上面的语法。

*   `find()`方法在`this_string`中搜索`this_pattern`的出现。
*   如果`this_pattern`存在，则返回`this_pattern`的第一个出现的*的起始索引。*
*   如果`this_string`中没有出现`this_pattern`，则返回`-1`。

我们来看一些例子。

### Python find()方法示例

我们来举个例子字符串`"I enjoy coding in Python!"`。

```
my_string = "I enjoy coding in Python!"
my_string.find("Python")

# Output
18
```

在上面的例子中，我们尝试在`"my_string"`中搜索`"Python"`。`find()`方法返回了`18`，模式`"Python"`的起始索引。

为了验证返回的索引是否正确，我们可以检查`my_string[18]=="P"`的值是否等于`True`。

现在，我们将尝试搜索我们的字符串中不存在的子字符串。

```
my_string.find("JavaScript")

# Output
-1
```

在这个例子中，我们试图搜索不在我们的字符串中的`"JavaScript"`。如前所述，`find()`方法已经返回了`-1`。

## 如何使用 find()在 Python 子字符串中搜索模式

我们还可以使用`find()`方法在某个字符串的*子串*或*片段*中搜索模式，而不是整个字符串。

在这种情况下，`find()`方法调用采用以下形式:

```
<this_string>.find(<this_pattern>, <start_index>,<end_index>)
```

这与前面讨论的语法非常相似。

> 唯一的区别是对模式的搜索不是在整个字符串上。它只超过了由`start_index:end_index`指定的字符串的*片段*。

## 如何使用 index()在 Python 字符串中搜索模式

要在字符串中搜索一个模式的出现，我们不妨使用`index()`方法。方法调用采用如下所示的语法。

```
<this_string>.index(<this_pattern>)
```

`index()`方法的工作原理与`find()`方法非常相似。

*   如果`this_string`中存在`this_pattern`，`index()`方法返回`this_pattern`第一个出现的*的起始索引。*
*   但是，如果在`this_string`中找不到`this_pattern`，它会引发一个`ValueError`。

举例时间。

### Python index()方法示例

让我们使用上一个例子中的字符串`my_string = "I enjoy coding in Python!"`。

```
my_string.index("Python")

# Output
18 
```

在这种情况下，输出与`find()`方法的输出相同。

如果我们搜索一个不在我们的字符串中的子串，`index()`方法会引发一个`ValueError`。这显示在下面的代码片段中。

```
my_string.index("JavaScript")

# Output
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-4-377f7c374e16> in <module>()
----> 1 my_string.index("JavaScript")

ValueError: substring not found
```

在下一节中，我们将学习如何查找和替换字符串中的模式。

## 如何使用 replace()查找和替换 Python 字符串中的模式

如果您需要在一个字符串中搜索一个模式，并用另一个模式替换它，您可以使用`replace()`方法。通用语法如下所示:

```
<this_string>.replace(<this>,<with_this>)
```

我们现在将解析这个语法。

*   `replace()`方法在`this_string`中搜索`this`模式。
*   如果`this`模式存在，它返回一个新字符串，其中`this`模式的所有出现的*被替换为由`with_this`参数指定的模式。*
*   如果在`this_string`中没有找到`this`模式，返回的字符串与`this_string`相同。

> 如果您只想替换一定数量的事件，而不是模式的所有事件，该怎么办？

在这种情况下，您可以在方法调用中添加一个*可选的*参数，指定您想要替换的模式的出现次数。

```
<this_string>.replace(<this>,<with_this>, n_occurrences)
```

在上面显示的语法中，`n_occurrences`确保在返回的字符串中只有第一个`n`出现的模式被替换。

让我们看一些例子来理解`replace()`函数是如何工作的。

### Python replace()方法示例

现在让我们将`my_string`重新定义如下:

```
I enjoy coding in C++.
C++ is easy to learn.
I've been coding in C++ for 2 years now.:)
```

这显示在下面的代码片段中:

```
my_string = "I enjoy coding in C++.\nC++ is easy to learn.\nI've been coding in C++ for 2 years now.:)" 
```

现在让我们用`"Python"`替换所有出现的`"C++"`，就像这样:

```
my_string.replace("C++","Python")

# Output
'I enjoy coding in Python.\nPython is easy to learn.\nI've been coding in Python for 2 years now.:)'
```

当`replace()`方法返回一个字符串时，我们会在输出中看到`\n`字符。为了让`\n`字符引入换行符，我们可以打印出如下所示的字符串:

```
print(my_string.replace("C++","Python"))

# Output
I enjoy coding in Python.
Python is easy to learn.
I've been coding in Python for 2 years now.:)
```

在上面的例子中，我们看到所有出现的`"C++"`都被替换为`"Python"`。

现在，我们也将使用额外的`n_occurrences`参数。

以下代码返回一个字符串，其中前两次出现的`"C++"`被替换为`"Python"`。

```
print(my_string.replace("C++","Python",2))

# Output
I enjoy coding in Python.
Python is easy to learn.
I've been coding in C++ for 2 years now.:)
```

以下代码返回一个字符串，其中只有第一次出现的`"C++"`被替换为`"Python"`:

```
print(my_string.replace("C++","Python",1))

# Output
I enjoy coding in Python.
C++ is easy to learn.
I've been coding in C++ for 2 years now.:)
```

现在，我们尝试替换`my_string`中不存在的子串`"JavaScript"`。因此，返回的字符串与`my_string`相同。

```
print(my_string.replace("JavaScript","Python"))

# Output
I enjoy coding in C++.
C++ is easy to learn.
I've been coding in C++ for 2 years now.:)
```

## 结论

在本教程中，我们学习了以下内容:

*   如何使用`find()`和`index()`方法在 Python 中搜索字符串，以及
*   如何使用`replace()`方法找到并替换模式或子字符串

希望你喜欢这个教程。

很快在另一篇文章中再见。直到那时，快乐学习！