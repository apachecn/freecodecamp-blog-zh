# Python。split()–在 Python 中拆分字符串

> 原文：<https://www.freecodecamp.org/news/how-to-split-a-string-in-python/>

在本文中，您将学习如何在 Python 中拆分字符串。

首先，我将向您介绍`.split()`方法的语法。之后，您将看到如何使用带参数和不带参数的`.split()`方法，同时使用代码示例。

以下是我们将要介绍的内容:

1.  [`.split()`方法语法](#syntax)
    1.  [没有任何参数的`.split()`方法是如何工作的？](#no-arguments)
    2.  [`.split()`方法如何与`separator`参数一起工作？](#separator)
    3.  [`.split()`方法如何与`maxsplit`参数一起工作？](#maxsplit)

## Python 中的`.split()`方法是什么？`.split()`方法语法分解

您使用`.split()`方法将一个字符串拆分成一个列表。

`.split()`方法的一般语法如下所示:

```
string.split(separator, maxsplit) 
```

让我们来分解一下:

*   `string`是要拆分的字符串。这是您调用`.split()`方法的字符串。
*   `.split()`方法接受两个参数。
*   第一个*可选的*参数是`separator`，它指定使用哪种分隔符来分割字符串。如果没有提供这个参数，缺省值是任何空格，这意味着只要`.split()`遇到任何空格，字符串就会被分割。
*   第二个*可选的*参数是`maxsplit`，它指定了`.split()`方法应该执行的最大拆分次数。如果没有提供这个参数，缺省值是`-1`，这意味着对拆分次数没有限制，并且`.split()`应该在遇到`separator`时拆分字符串。

`.split()`方法返回一个新的子字符串列表，原始字符串没有任何修改。

### 没有任何参数的`.split()`方法是如何工作的？

下面是如何使用不带任何参数的`.split()`方法将一个字符串拆分成一个列表:

```
coding_journey = "I am learning to code for free with freeCodecamp!"

# split string into a list and save result into a new variable
coding_journey_split = coding_journey.split()

print(coding_journey)
print(coding_journey_split)

# check the data type of coding_journey_split by using the type() function
print(type(coding_journey_split))

# output
# I am learning to code for free with freeCodecamp!
# ['I', 'am', 'learning', 'to', 'code', 'for', 'free', 'with', 'freeCodecamp!']
# <class 'list'> 
```

输出显示组成字符串的每个单词现在都是一个列表项，并且保留了原始字符串。

当您没有传递`.split()`方法接受的两个参数中的任何一个时，那么默认情况下，它将在每次遇到空白时分割字符串，直到字符串结束。

当您不向`.split()`方法传递任何参数，并且它遇到连续的空格而不是一个空格时，会发生什么？

```
coding_journey = "I love   coding"

coding_journey_split = coding_journey.split()

print(coding_journey_split)

# output
# ['I', 'love', 'coding'] 
```

在上面的例子中，我在单词`love`和单词`coding`之间添加了连续的空格。在这种情况下，`.split()`方法将任何连续的空格视为一个空格。

### `.split()`方法如何与`separator`参数一起工作？

正如您之前看到的，当没有`separator`参数时，它的默认值是空白。也就是说，你可以设置一个不同的`separator`。

每当遇到您指定的字符时,`separator`将断开并分割字符串，并将返回一个子字符串列表。

例如，每当`.split()`方法遇到一个点`.`时，您可以使字符串分裂:

```
fave_website = "www.freecodecamp.org"

fave_website_split = fave_website.split(".")

print(fave_website_split)

# output
# ['www', 'freecodecamp', 'org'] 
```

在上面的例子中，每当`.split()`遇到`.`时，字符串就会分开

请记住，我没有指定一个点后跟一个空格。这是行不通的，因为字符串不包含后跟空格的点:

```
fave_website = "www.freecodecamp.org"

fave_website_split = fave_website.split(". ")

print(fave_website_split)

# output
# ['www.freecodecamp.org'] 
```

现在，让我们重温上一节的最后一个例子。

当没有`separator`参数时，连续的空白被视为单个空白。

但是，当您指定单个空格作为`separator`时，字符串每次遇到单个空格字符时都会拆分:

```
coding_journey = "I love   coding"

coding_journey_split = coding_journey.split(" ")

print(coding_journey_split)

# output
# ['I', 'love', '', '', 'coding'] 
```

在上面的例子中，每次`.split()`遇到一个空格字符，它就将单词拆分，并添加空白作为列表项。

### `.split()`方法如何与`maxsplit`参数一起工作？

当没有`maxsplit`参数时，对于何时应该停止分割没有指定的限制。

在上一节的第一个例子中，`.split()`在每次遇到`separator`时都会分割字符串，直到到达字符串的末尾。

但是，您可以指定希望拆分何时结束。

例如，您可以指定分割在遇到一个点后结束:

```
fave_website = "www.freecodecamp.org"

fave_website_split = fave_website.split(".", 1)

print(fave_website_split)

# output
# ['www', 'freecodecamp.org'] 
```

在上面的例子中，我将`maxsplit`设置为`1`，创建了一个包含两个列表项的列表。

我指定了列表在遇到一个点时应该被分割。一旦遇到一个点，操作就会结束，字符串的其余部分就会成为一个单独的列表项。

## 结论

现在你知道了！您现在知道了如何使用`.split()`方法在 Python 中拆分字符串。

我希望这篇教程对你有所帮助。

要了解更多关于 Python 编程语言的知识，请查看 freeCodeCamp 的 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢您的阅读，祝您编码愉快！