# python split()–字符串拆分示例

> 原文：<https://www.freecodecamp.org/news/python-split-string-splitting-example/>

Python 中的`split()`方法将字符串中的字符分割成列表中的单独项目。

在本教程中，我们将通过一些例子来帮助你学习如何使用`split()`方法。我们将从语法开始，然后看看如何使用`split()`方法的参数修改返回的列表。

## Python 中`split()`方法的语法

`split()`方法接受两个参数。下面是语法的样子:

```
str.split(separator, maxsplit)
```

上面的参数是`separator`和`maxsplit`。这两个参数都是可选的，但是让我们讨论一下它们的作用。

`separator`指定出现拆分的字符。如果未指定，空格将用作发生拆分的默认字符。在下一节的示例中，您会更好地理解这一点。

`maxsplit`指定最大分割数。默认值为-1，允许连续的拆分次数。该参数也是可选的。

## 如何使用不带参数的`split()`方法

在这一节中，我们将看到一些使用`split()`方法在不传入任何参数的情况下拆分字符串的例子。

```
myString = "Python is a programming language"

print(myString.split())

# ['Python', 'is', 'a', 'programming', 'language']
```

在上面的代码中，我们创建了一个名为`myString`的字符串，该字符串由五个字符组成:“Python 是一种编程语言”。

然后，我们使用点符号对字符串使用了`split()`方法。

当打印到控制台时，字符串中的每个字符都成为列表数据类型中的一个单独项目:`['Python', 'is', 'a', 'programming', 'language']`。

`split()`方法能够分隔每个单词，因为默认情况下，空格表示每个字符的分隔点(参考上一节中的`separator`参数)。

## 如何使用带参数的`split()`方法

在这一节中，我们将通过例子了解如何使用`split()`方法的参数。

```
myString = "Hello World!, if you're reading this, you're awesome"

print(myString.split(", "))

# ['Hello World!', "if you're reading this", "you're awesome"]
```

在上面的例子中，我们传递了一个逗号(，)作为`separator` : `myString.split(", ")`。

因此，不是在每个空格后拆分字符，而是只在出现逗号时才拆分字符:`['Hello World!', "if you're reading this", "you're awesome"]`。这意味着出现在逗号前的字符将被组合在一起。

在下一个例子中，我们将使用第二个参数—`maxsplit`。

```
myString = "Hello World!, if you're reading this, you're awesome"

print(myString.split(", ", 0))

# ["Hello World!, if you're reading this, you're awesome"]
```

我们在上面的代码中添加了一个值为 0 的`maxsplit`。这将控制字符串的拆分方式。0 表示 1，因此字符作为列表中的一项返回:`["Hello World!, if you're reading this, you're awesome"]`

让我们改变数字，看看会发生什么

```
myString = "Hello World!, if you're reading this, you're awesome"

print(myString.split(", ", 1))

# ['Hello World!', "if you're reading this, you're awesome"]
```

现在我们已经将数字改为 1，列表中的字符被分成两项——“Hello World！和“如果你正在读这封信，你太棒了”。

缺省情况下，省略`maxsplit`值会将其设置为-1。这个负值允许`split()`方法将每个字符连续分割成单独的项目，直到不再有字符。如果有一个指定的`separator`，将根据该值进行拆分——否则，将使用空白。

## 结论

在本文中，我们讨论了 Python 中的`split()`方法，该方法拆分字符串中的字符，并将它们作为列表中的项目返回。

我们看到了`split()`方法的语法和默认提供的两个参数——参数`separator`和`maxsplit`。

我们还看到了一些分成两部分的例子。第一部分展示了如何使用不带参数的`split()`方法，而第二部分展示了我们如何使用方法的参数来获得不同的结果。

编码快乐！