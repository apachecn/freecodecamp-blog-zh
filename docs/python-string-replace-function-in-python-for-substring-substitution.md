# Python 字符串。replace()–Python 中用于子串替换的函数

> 原文：<https://www.freecodecamp.org/news/python-string-replace-function-in-python-for-substring-substitution/>

在本文中，您将看到如何使用 Python 的`.replace()`方法来执行子串替换。

您还将看到如何执行不区分大小写的子字符串替换。

我们开始吧！

## `.replace()` Python 方法是做什么的？

当使用`.replace()` Python 方法时，您可以用一个新字符替换一个特定字符的每个实例。您甚至可以用您指定的新文本行替换整个文本字符串。

方法返回一个字符串的副本。这意味着旧的子字符串保持不变，但会创建一个新的副本——所有旧文本都被新文本替换。

### `.replace()` Python 方法是如何工作的？语法错误

`.replace()`方法的语法如下所示:

```
string.replace(old_text, new_text, count) 
```

让我们来分解一下:

*   `old_text`是`.replace()`接受的第一个必需参数。它是您想要替换的旧字符或文本。用引号将它括起来。
*   `new_text`是`.replace()`接受的第二个必需参数。您要用新的字符或文本替换旧的字符/文本。这个参数也需要用引号括起来。
*   `count`是`.replace()`接受的*可选*第三个参数。默认情况下，`.replace()`将替换**子串的所有**实例。但是，您可以使用`count`来指定您想要替换的出现次数。

## Python `.replace()`方法代码示例

### 如何替换单个字符的所有实例

要更改单个角色的所有实例，您需要执行以下操作:

```
phrase = "I like to learn coding on the go"

# replace all instances of 'o' with 'a'
substituted_phrase = phrase.replace("o", "a" )

print(phrase)
print(substituted_phrase)

#output

#I like to learn coding on the go
#I like ta learn cading an the ga 
```

在上面的例子中，每个包含字符`o`的单词都被替换为字符`a`。

在这个例子中，有四个字符`o`的实例。具体来说，它出现在`to`、`coding`、`on`和`go`这些词中。

如果你只想改变两个单词，比如`to`和`coding`，来包含`a`而不是`o`呢？

### 如何只替换单个字符的一定数量的实例

要仅更改单个字符的两个实例，您可以使用`count`参数并将其设置为两个:

```
phrase = "I like to learn coding on the go"

# replace only the first two instances of 'o' with 'a'
substituted_phrase = phrase.replace("o", "a", 2 )

print(phrase)
print(substituted_phrase)

#output

#I like to learn coding on the go
#I like ta learn cading on the go 
```

如果您只想更改单个字符的第一个实例，您可以将`count`参数设置为 1:

```
phrase = "I like to learn coding on the go"

# replace only the first instance of 'o' with 'a'
substituted_phrase = phrase.replace("o", "a", 1 )

print(phrase)
print(substituted_phrase)

#output

#I like to learn coding on the go
#I like ta learn coding on the go 
```

### 如何替换一个字符串的所有实例

要更改多个字符，过程看起来是相似的。

```
phrase = "The sun is strong today. I don't really like sun."

#replace all instances of the word 'sun' with 'wind'
substituted_phrase = phrase.replace("sun", "wind")

print(phrase)
print(substituted_phrase)

#output

#The sun is strong today. I don't really like sun.
#The wind is strong today. I don't really like wind. 
```

在上面的例子中，单词`sun`被替换为单词`wind`。

### 如何只替换字符串的一定数量的实例

如果您只想将第一个实例`sun`更改为`wind`，您可以使用`count`参数并将其设置为 1。

```
phrase = "The sun is strong today. I don't really like sun."

#replace only the first instance of the word 'sun' with 'wind'
substituted_phrase = phrase.replace("sun", "wind", 1)

print(phrase)
print(substituted_phrase)

#output

#The sun is strong today. I don't really like sun.
#The wind is strong today. I don't really like sun. 
```

## 如何在 Python 中执行不区分大小写的子串替换

让我们看另一个例子。

```
 phrase = "I am learning Ruby. I really enjoy the ruby programming language!"

#replace the text "Ruby" with "Python"
substituted_text = phrase.replace("Ruby", "Python")

print(substituted_text)

#output

#I am learning Python. I really enjoy the ruby programming language! 
```

在这种情况下，我真正想做的是用`Python`替换单词`Ruby`的所有实例。

但是，有一个小写的`r`的单词`ruby`，我也想改变它。

因为第一个字母是小写的，而不是我用`Ruby`指定的大写，所以它保持不变，没有变成`Python`。

`.replace()`方法是*区分大小写的*，因此它执行区分大小写的子串替换。

为了执行*不区分大小写的*子串替换，你必须做一些不同的事情。

您需要使用`re.sub()`函数和`re.IGNORECASE`标志。

要使用`re.sub()`,您需要:

*   通过`import re`使用`re`模块。
*   指定一个正则表达式`pattern`。
*   提及你想要的`replace`模式。
*   提及您想要执行此操作的`string`。
*   可选地，指定`count`参数以使替换更加精确，并指定您想要发生的最大替换次数。
*   `re.IGNORECASE`标志告诉正则表达式执行不区分大小写的匹配。

因此，总的来说，语法如下所示:

```
import re

re.sub(pattern, replace, string, count, flags) 
```

以前面的例子为例:

```
phrase = "I am learning Ruby. I really enjoy the ruby programming language!" 
```

这就是我用`Python`替换`Ruby`和`ruby`的方法:

```
import re

phrase = "I am learning Ruby. I really enjoy the ruby programming language!"

phrase = re.sub("Ruby","Python", phrase, flags=re.IGNORECASE)

print(phrase)

#output

#I am learning Python. I really enjoy the Python programming language! 
```

## 包扎

现在，您已经知道了子串替换的基本知识。希望这个指南对你有所帮助。

要了解更多关于 Python 的知识，请查看 freeCodeCamp 的[带有 Python 认证的科学计算](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢阅读和快乐编码！