# python find()–如何在字符串中搜索子字符串

> 原文：<https://www.freecodecamp.org/news/python-find-how-to-search-for-a-substring-in-a-string/>

当您使用 Python 程序时，您可能需要在另一个字符串中搜索和定位特定的字符串。

这就是 Python 的内置字符串方法派上用场的地方。

在本文中，您将学习如何使用 Python 内置的`find()` string 方法来帮助您搜索字符串中的子字符串。

以下是我们将要介绍的内容:

1.  [`find()`方法的语法](#syntax)
    1.  [如何使用没有开始和结束参数的`find()`示例](#no-params)
    2.  [如何使用带有开始和结束参数的`find()`示例](#params)
    3.  [未找到子串示例](#not-found)
    4.  [`find()`方法区分大小写吗？](#case-sensitive)
2.  [`find()` vs `in`关键词](#find-vs-in)
3.  [`find()` vs `index()`](#find-vs-index)

## `find()`方法-语法概述

字符串方法内置于 Python 的标准库中。

它接受一个子串作为输入，并查找它的索引——也就是子串在调用该方法的字符串中的位置。

`find()`方法的一般语法如下所示:

```
string_object.find("substring", start_index_number, end_index_number) 
```

让我们来分解一下:

*   `string_object`是您正在处理的原始字符串，也是您将对其调用`find()`方法的字符串。这可能是你想搜索的任何词。
*   `find()`方法有三个参数——一个是必需的，两个是可选的。
*   `"substring"`是第一个*需要的*参数。这是您试图在`string_object`中找到的子串。确保包含引号。
*   `start_index_number`是第二个参数，它是*可选的*。它指定起始索引和搜索开始的位置。默认值为`0`。
*   `end_index_number`是第三个参数，也是*可选的*。它指定结束索引和搜索将停止的位置。默认值是字符串的长度。
*   `start_index_number`和`end_index_number`都指定了搜索的范围，它们将搜索范围缩小到特定的部分。

`find()`方法的返回值是一个整数值。

如果子字符串存在于字符串中，`find()`将返回给定字符串中指定子字符串的第一个**匹配项的索引或字符位置。**

如果您正在搜索的子字符串是**而不是**，那么`find()`将返回`-1`。它不会抛出异常。

### 如何使用没有开始和结束参数的`find()`示例

下面的例子说明了如何使用`find()`方法，该方法使用了唯一需要的参数——您想要搜索的子字符串。

您可以选择一个单词并进行搜索来查找特定字母的索引号:

```
fave_phrase = "Hello world!"

# find the index of the letter 'w'
search_fave_phrase = fave_phrase.find("w")

print(search_fave_phrase)

#output

# 6 
```

我创建了一个名为`fave_phrase`的变量，并存储了字符串`Hello world!`。

我对包含字符串的变量调用了`find()`方法，并在`Hello world!`中搜索字母“w”。

我将操作的结果存储在一个名为`search_fave_phrase`的变量中，然后将其内容打印到控制台。

返回值是`w`的索引，在本例中是整数`6`。

请记住，通常编程和计算机科学中的索引总是从`0`和**开始，而不是从**和`1`开始。

### 如何将`find()`与开始和结束参数一起使用示例

在`find()`方法中使用 start 和 end 参数可以限制搜索范围。

例如，如果您想找到字母“w”的索引，并从位置`3`开始搜索，而不是更早，您可以执行以下操作:

```
fave_phrase = "Hello world!"

# find the index of the letter 'w' starting from position 3
search_fave_phrase = fave_phrase.find("w",3)

print(search_fave_phrase)

#output

# 6 
```

因为搜索从位置 3 开始，所以返回值将是从该位置开始包含“w”的字符串的第一个实例。

您还可以使用 end 参数进一步缩小搜索范围，使搜索更加具体:

```
fave_phrase = "Hello world!"

# find the index of the letter 'w' between the positions 3 and 8
search_fave_phrase = fave_phrase.find("w",3,8)

print(search_fave_phrase)

#output

# 6 
```

### 找不到子串示例

如前所述，如果用`find()`指定的子串不在字符串中，那么输出将是`-1`而不是异常。

```
fave_phrase = "Hello world!"

# search for the index of the letter 'a' in "Hello world"
search_fave_phrase = fave_phrase.find("a")

print(search_fave_phrase)

# -1 
```

### `find()`方法区分大小写吗？

如果你搜索一个不同大小写的字母会发生什么？

```
fave_phrase = "Hello world!"

#search for the index of the letter 'W' capitalized
search_fave_phrase = fave_phrase.find("W")

print(search_fave_phrase)

#output

# -1 
```

在前面的例子中，我在短语“Hello world！”中搜索字母`w`的索引而`find()`方法返回了它的位置。

在这种情况下，搜索字母`W`大写会返回`-1`，这意味着该字母不在字符串中。

因此，当使用`find()`方法搜索子串时，记住搜索将区分大小写。

## `find()`方法和`in`关键字——有什么区别？

使用`in`关键字首先检查子字符串是否出现在字符串中。

`in`关键字的一般语法如下:

```
substring in string 
```

`in`关键字返回一个布尔值——该值可以是`True`或`False`。

```
>>> "w" in "Hello world!"
True 
```

当子串出现在字符串中时,`in`操作符返回`True`。

如果子串不存在，它返回`False`:

```
>>> "a" in "Hello world!"
False 
```

在使用`find()`方法之前，使用`in`关键字是很有帮助的第一步。

您首先检查一个字符串是否包含子串，然后可以使用`find()`找到子串的位置。这样，您就可以确定子字符串是存在的。

因此，使用`find()`来查找子串在字符串中的索引位置，而不是查看子串是否出现在字符串中。

## `find()`方法与`index()`方法——有何不同？

类似于`find()`方法，`index()`方法是一种字符串方法，用于查找字符串内子字符串的索引。

所以，这两种方法的工作原理是一样的。

这两种方法的区别在于，当子串不在字符串中时，`index()`方法会引发异常，而`find()`方法会返回`-1`值。

```
fave_phrase = "Hello world!"

# search for the index of the letter 'a' in 'Hello world!'
search_fave_phrase = fave_phrase.index("a")

print(search_fave_phrase)

#output

# Traceback (most recent call last):
#  File "/Users/dionysialemonaki/python_article/demopython.py", line 4, in <module>
#    search_fave_phrase = fave_phrase.index("a")
# ValueError: substring not found 
```

上面的例子显示当子串不存在时,`index()`抛出一个`ValueError`。

当你不想在程序中捕捉和处理任何异常时，你可以使用`find()`而不是`index()`。

## 结论

现在你知道了！您现在知道了如何使用`find()`方法在字符串中搜索子字符串。

我希望这篇教程对你有所帮助。

要了解更多关于 Python 编程语言的知识，请查看 freeCodeCamp 的 Python 认证。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您对所学概念的理解。

感谢您的阅读，祝您编码愉快！

编码快乐！