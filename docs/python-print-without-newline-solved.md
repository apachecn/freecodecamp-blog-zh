# 没有换行符的 Python 打印[已解决]

> 原文：<https://www.freecodecamp.org/news/python-print-without-newline-solved/>

如果你熟悉 Python，你可能已经注意到内置函数`print()`不需要在每一行的末尾有一个明确的`\n`字符。

这意味着以下代码片段的输出

```
print('freeCodeCamp Curriculum')
print('freeCodeCamp News')
print('freeCodeCamp YouTube Channel')
```

将会是:

```
freeCodeCamp Curriculum
freeCodeCamp News
freeCodeCamp YouTube Channel
```

这是因为`print()`函数将`\n`字符添加到每一行的末尾。虽然大多数情况下这是一种便利，但有时您可能希望打印出不带`\n`字符的行。

在本文中，您将学习如何更改每个打印语句的结尾。您还将学习许多其他方法来修改内置函数的默认行为。

## 如何在 Python 中不用换行符打印

根据`print()`函数的[官方文档](https://docs.python.org/3/library/functions.html#print)，函数签名如下:

```
print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
```

如您所见，该函数总共有五个参数。第一个是您想要在终端上打印的对象。然后是分隔符(您将在后面的小节中了解)，以及`end`参数。

`end`参数的缺省值是`\n`，这意味着在终端上打印的每一行后面都会附加一个换行符。

要更改此行为，只需按如下方式覆盖此参数:

```
print('freeCodeCamp Curriculum', end=', ')
print('freeCodeCamp News', end=', ')
print('freeCodeCamp YouTube Channel')
```

因为我已经将`end`的值改为逗号，所以输出将是`freeCodeCamp Curriculum, freeCodeCamp News, freeCodeCamp YouTube Channel`而不是三个单独的行。

可以使用任何字符作为`end`的值。如果你不想在行尾有任何东西，只需使用一个空字符串。

## 如何在 Python 中使用分隔符打印

还记得函数签名中的`sep`参数吗？在本节中，您将了解它的用法。

您可能知道也可能不知道`print()`函数可以一次打印多个对象，如下所示:

```
print('freeCodeCamp Curriculum', 'freeCodeCamp News', 'freeCodeCamp YouTube Channel')
```

该代码的输出将是:

```
freeCodeCamp Curriculum, freeCodeCamp News, freeCodeCamp YouTube Channel
```

如您所见，该函数将三个字符串打印在一行中，用逗号分隔。如果您想使用破折号等其他东西作为分隔符，可以按如下方式操作:

```
print('freeCodeCamp Curriculum', 'freeCodeCamp News', 'freeCodeCamp YouTube Channel', sep=' - ')
```

该代码的输出将是:

```
freeCodeCamp Curriculum - freeCodeCamp News - freeCodeCamp YouTube Channel
```

我希望你已经明白了。像`end`参数一样，您可以使用或多或少的任何有效字符串作为`sep`参数的值。

## 如何在 Python 中打印到文件

除了将东西打印到终端，您还可以使用`print()`功能将东西直接打印到文件中。

`print()`函数采用另一个参数`file`，默认为`sys.stdout`或标准输出。它是默认的文件描述符，进程可以在其中写入输出。

您可以覆盖它，改为写入文件，如下所示:

```
with open('output.txt', 'w') as f:
    print('freeCodeCamp', file=f)
```

首先，打开一个名为`output.txt`的文件作为`f`，并将其作为`file`参数的值进行传递。如果您运行代码，将会创建一个新文件，该文件将是您的输出。

## 结论

`print()`函数是 Python 中最常用的函数之一。因此，很好地理解这个内置函数的不同用法可以提高您的工作效率。

函数中还有另一个参数`flush`。它是一个布尔值，将其设置为`True`将在每次执行时刷新输出。

我也有一个个人博客，在那里我写一些随机的技术东西，所以如果你对类似的东西感兴趣，请查看 https://farhan.dev 。如果你有任何问题或对任何事情感到困惑——或者只是想联系——我可以在 [Twitter](https://twitter.com/frhnhsin) 和 [LinkedIn](https://www.linkedin.com/in/farhanhasin/) 上找到我。