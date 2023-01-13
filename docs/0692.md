# Python。split()–在 Python 中拆分字符串

> 原文：<https://www.freecodecamp.org/news/python-split-string/>

要不要用 Python 把一个字符串变成一个字符串数组？一种方法是使用 Python 的内置`.split()`方法。

以下是如何在 Python 命令行中实现这一点的示例:

```
>>> string1 = "test your might"
>>> string1.split(" ");
# Output: ['test', 'your', 'might']
```

您可以从命令行打开 Python REPL。Python 内置于 Linux、Mac 和 Windows 中。我写了一个指南，告诉你如何从 Mac 终端打开最新版本的 Python。

请注意，上例中的“，”参数实际上是可选的。看看这个:

```
>>> string1 = "test your might"
>>> string1.split();
# Output: ['test', 'your', 'might']

>>> string2 = "test,your,might"
>>> s.split();
# Output: ['test', 'your', 'might']
```

Python `.split()`方法足够智能，可以推断出分隔符应该是什么。在`string1`中，我使用了一个空格。在`string2`中，我使用了逗号。在这两种情况下，它的工作。

## 如何使用 Python？用特定分隔符拆分()

在实践中，您会想要传递一个`separator`作为参数。让我告诉你怎么做:

```
>>> s = "test your might"
>>> s.split(" ");
# Output: ['test', 'your', 'might']

>>> s2 = "test,your,might"
>>> s.split(",");
# Output: ['test', 'your', 'might']
```

输出是一样的，但是更干净。下面是一个更复杂的字符串，指定分隔符会产生更大的不同:

```
>>> string3 = "excellent, test your might, fight, mortal kombat"
>>> string3.split(",");
# Output: ['excellent', ' test your might', ' fight', ' mortal kombat']

>>> string3.split(" ");
# Output: ['excellent,', 'test', 'your', 'might,', 'fight,', 'mortal', 'kombat']
```

如您所见，指定一个分隔符是更安全的做法。

另请注意，在生成的数组中，某些字符串可能包含前导空格和尾随空格。只是一些需要注意的事情。😉

## 如何在 Python 中将一个字符串拆分成多个字符串？

您可以根据需要将字符串拆分成任意多个部分。这完全取决于你想在哪个字符上分割字符串。

但是如果你想确保一个字符串不被分割成超过一定数量的部分，你会想在你的函数调用中使用 pass`maxsplit`参数。

## 在 Python 中，如何将一个字符串分成 3 部分？

如果你想给你的字符串被分割成的部分的数量设定一个上限，你可以使用`maxsplit`参数来指定，就像这样:

```
string3 = "excellent, test your might, fight, mortal kombat"

print(string.split(" ", 3))

# Output: ['excellent,', 'test', 'your', 'might, fight, mortal kombat']
# maxsplit=3 means that string will be split into at most three parts
```

如您所见，`split`函数只是在第三个空格后停止分割字符串，因此结果数组中总共有 4 个字符串。

我希望这能对你有所帮助。感谢您的阅读，祝您编码愉快。如果你想了解更多，请查看 [freeCodeCamp 的核心课程](https://www.freecodecamp.org/learn)。