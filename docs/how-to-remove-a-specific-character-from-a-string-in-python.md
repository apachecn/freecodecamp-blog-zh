# 如何在 Python 中删除字符串中的特定字符

> 原文：<https://www.freecodecamp.org/news/how-to-remove-a-specific-character-from-a-string-in-python/>

用 Python 编码时，有时可能需要从字符串中删除一个字符。

如果您正在处理用户生成的输入，并且需要清理数据和删除不需要的字符，那么从字符串中删除字符是很方便的。

具体来说，您可能只需要从字符串中移除一个字符的实例，或者甚至移除一个字符的所有匹配项。

Python 提供了许多方法来帮助你做到这一点。

在 Python 中，从字符串中移除字符的两种最常见的方法是:

*   使用`replace()`字符串方法
*   使用`translate()`字符串方法

使用这两种方法中的任何一种时，都可以指定要从字符串中删除的字符。

这两种方法都用您指定的值替换字符。如果您指定了一个空字符，您想要删除的字符将被删除而不会被替换。

使用这些方法时需要注意的是，由于字符串是不可变的，所以原始字符串不会被改变。相反，这两种方法都返回一个新的修改过的字符串，其中不包含您想要删除的字符。

在本文中，您将在代码示例的帮助下学习如何使用这两种方法从字符串中删除一个或多个字符。

以下是我们将要介绍的内容:

1.  [如何使用`replace()`方法从字符串中删除特定字符](#remove-char-replace)
2.  [如何使用`replace()`方法从字符串中删除多个字符](#remove-chars-replace)
    1.  [用方法链接删除多个字符](#chaining)
    2.  [用`for`循环删除多个字符](#for-loop)
    3.  [用正则表达式删除多个字符](#regular-expressions)
3.  [如何使用`translate()`方法从字符串中删除特定字符](#remove-char-translate)
4.  [如何使用`translate()`方法从字符串中删除多个字符](#remove-chars-translate)

让我们开始吧！

## 如何使用`replace()`方法在 Python 中删除字符串中的特定字符

`replace()`方法的一般语法如下所示:

```
string.replace( character, replacement, count) 
```

让我们来分解一下:

*   您在一个`string`上附加了`replace()`方法。
*   `replace()`方法接受三个参数:
    *   `character`是必需的参数，代表要从`string`中删除的特定字符。
    *   `replacement`是必需的参数，代表将取代`character`的新字符串/字符。
    *   `count`是可选参数，表示要从`string`中删除的`character`个事件的最大数量。如果不包括`count`，那么默认情况下，`replace()`方法将删除`character`的所有出现。

`replace()`方法不修改原始字符串。相反，它的返回值是原始字符串的副本，不包含您想要删除的字符。

现在，让我们来看看`replace()`的行动！

假设您有以下字符串，并且您想要删除所有感叹号:

```
my_string = "Hi! I! Love! Python!" 
```

以下是删除所有出现的`!`字符的方法:

```
my_string = "Hi! I! Love! Python!"

my_new_string = my_string.replace("!", "")

print(my_new_string)

# output

# Hi I Love Python 
```

在上面的例子中，我将`replace()`方法附加到了`my_string`中。

然后，我将结果存储在一个名为`my_new_string`的新变量中。

记住，字符串是不可变的。原字符串保持不变- `replace()`返回一个新字符串，不修改原字符串。

我指定我想删除字符`!`,并指出我想用一个空字符替换`!`。

我也没有使用`count`参数，所以`replace()`用一个空参数替换了`!`角色出现的所有事件。

存储在变量`my_string`中的原始字符串有四次出现`!`字符。

如果我只想删除三次出现的字符，而不是全部，我会使用`count`参数并传递一个值`3`来指定我想要替换字符的次数:

```
my_string = "Hi! I! love! Python!"

my_new_string = my_string.replace("!", "", 3)

print(my_new_string)

# output

# Hi I love Python! 
```

## 如何在 Python 中使用`replace()`方法从一个字符串中删除多个字符

有时您可能需要替换字符串中的多个字符。

在接下来的章节中，您将看到使用`replace()`方法实现这一点的三种方式。

### 用方法链接删除多个字符

实现这一点的一种方法是像这样链接`replace()`方法:

```
my_string = "Hi!? I!? love!? Python!?"

my_new_string = my_string.replace("!","").replace("?","")

print(my_new_string)

# output

# Hi I love Python 
```

也就是说，这种去除字符的方式很难阅读。

### 用一个`for`循环删除多个字符

另一种方法是在一个`for`循环中使用`replace()`方法:

```
my_string = "Hi!? I!? love!? Python!?"

replacements = [('!', ''), ('?', '')]

for char, replacement in replacements:
    if char in my_string:
        my_string = my_string.replace(char, replacement)

print(my_string)

# output

# Hi I love Python 
```

我首先创建了一个包含我想要删除的两个字符的字符串，`!`和`?`，并将其存储在变量`my_string`中。

我将我想要替换的字符以及它们的替换存储在一个名为`replacements`的元组列表中——我想用一个空字符串替换`!`，用一个空字符串替换`?`。

然后，我使用了一个`for`循环来遍历元组列表(如果您需要复习一下`for`循环，请阅读一下[的这篇文章](https://www.freecodecamp.org/news/python-for-loop-example-and-tutorial/))。

在`for`循环中，我使用了`in`操作符来检查字符是否在字符串中。如果是的话，我就用`replace()`的方法替换它们。

最后，我重新分配了变量。

### 用正则表达式删除多个字符

还有一种方法是使用正则表达式库`re`和`sub`方法。

您首先需要导入库:

```
import re 
```

然后，指定要删除的一组字符(在本例中是`!`和`?`字符)，以及要替换的字符。在这种情况下，替换是一个空字符:

```
import re

my_string = "Hi!? I!? love!? Python!?"

my_new_string = re.sub('[!?]',"",my_string)

print(my_new_string)

# output

# Hi I love Python 
```

## 如何使用`translate()`方法在 Python 中删除字符串中的特定字符

类似于`replace()`方法，`translate()`从字符串中删除字符。

也就是说，`translate()`方法有点复杂，而且不是最适合初学者的。

当您需要从字符串中删除字符时，`replace()`方法是最直接的解决方案。

当使用`translate()`方法替换字符串中的字符时，需要创建一个字符翻译表，其中`translate()`使用表的内容替换字符。

转换表是一个键值映射的字典，每个键都用一个值替换。

您可以使用`ord()`函数获取字符的 Unicode 值，然后将该值映射到另一个字符。

该方法返回一个新字符串，旧字符串中的每个字符都被映射到翻译表中的一个字符。

返回值是一个新字符串。

让我们来看一个使用前面几节中相同代码的示例:

```
my_string = "Hi! I! love! Python!"

my_new_string = my_string.translate( { ord("!"): None } )

print(my_new_string)

# output

# Hi I love Python 
```

在上面的例子中，我使用了`ord()`函数来返回与我想要替换的字符相关联的 Unicode 值，在本例中是`!`。

然后，我将这个 Unicode 值映射到`None`——表示空的另一个单词——这确保了删除它。具体来说，它用一个`None`值替换了`!`字符的每个实例。

## 如何在 Python 中使用`translate()`方法从一个字符串中删除多个字符

如果您需要使用`translate()`方法替换多个字符，该怎么办？为此，您可以像这样使用迭代器:

```
my_string = "Hi!? I!? love!? Python!?"

my_new_string = my_string.translate( { ord(i): None for i in '!?'} )

print(my_new_string)

# output

# Hi I love Python 
```

在上面的例子中，我用值`None`替换了`!`和`?`字符，使用了一个迭代器遍历我想要删除的字符。

`translate()`方法检查`my_string`中的每个字符是否等于感叹号或问号。如果是，那么它将被替换为`None`。

## 结论

希望本文能帮助您理解如何使用内置的`replace()`和`translate()`字符串方法从 Python 中删除字符串中的字符。

感谢您的阅读，祝您编码愉快！