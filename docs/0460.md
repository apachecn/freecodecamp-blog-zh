# 列表索引超出范围–Python 错误[已解决]

> 原文：<https://www.freecodecamp.org/news/list-index-out-of-range-python-error-solved/>

在本文中，我们将讨论 Python 中的`IndexError: list index out of range`错误。

在本文的每一节中，我将强调错误的可能原因以及如何修复它。

由于以下原因，您可能会得到`IndexError: list index out of range`错误:

*   试图访问列表中不存在的索引。
*   在循环中使用无效索引。
*   使用`range()`函数时，指定超出列表中索引的范围。

在修复错误之前，让我们讨论一下 Python 列表中的索引是如何工作的。如果您已经知道索引是如何工作的，您可以跳过下一节。

## Python 列表中的索引是如何工作的？

Python 列表中的每一项都可以使用其索引号进行评估。列表中的第一项的索引为零。

考虑下面的列表:

```
languages = ['Python', 'JavaScript', 'Java']

print(languages[1])
# JavaScript
```

在上面的例子中，我们有一个名为`languages`的列表。该列表有三个项目——“Python”、“JavaScript”和“Java”。

为了访问第二项，我们使用了它的索引:`languages[1]`。这个打印出来的`JavaScript`。

一些初学者可能会误解这一点。他们可能会假设，既然索引是 1，那么它应该是第一项。

为了便于理解，下面根据索引对列表中的项目进行了分类:

Python (item 1) = >索引 0
JavaScript (item 2) = >索引 1
Java (item 3) = >索引 2

从上面可以看到，第一项的索引为 0(因为 Python 是“零索引的”)。要访问列表中的项目，可以利用它们的索引。

## 如果试图使用 Python 列表中超出范围的索引会发生什么？

如果您试图使用超出范围的索引来访问列表中的项目，您将得到`IndexError: list index out of range`错误。

这里有一个例子:

```
languages = ['Python', 'JavaScript', 'Java']

print(languages[3])
# IndexError: list index out of range
```

在上面的例子中，我们试图使用索引来访问第四个条目:`languages[3]`。我们得到了`IndexError: list index out of range`错误，因为列表没有第四项——它只有三项。

简单的解决方法是在试图访问列表中的项目时，总是使用列表中存在的索引。

## 如何修复 Python 循环中的`IndexError: list index out of range`错误

循环是有条件的。因此，在满足某个条件之前，它们会一直运行。

在下面的例子中，我们将尝试使用一个`while`循环打印列表中的所有项目。

```
languages = ['Python', 'JavaScript', 'Java']

i = 0

while i <= len(languages):
    print(languages[i])
    i += 1

# IndexError: list index out of range
```

上面的代码返回了`IndexError: list index out of range`错误。让我们分解代码来理解为什么会发生这种情况。

首先，我们初始化一个变量`i`，并赋予它一个值 0: `i = 0`。

然后我们给出了一个`while`循环的条件(这是导致错误的原因):`while i <= len(languages)`。

根据给定的条件，我们说，“只要`i`小于的**或**等于**列表的**长度**，这个循环就应该继续运行”。**

`len()`函数返回列表的长度。在我们的例子中，将返回 3。所以条件会是这样:`while i <= 3`。当`i`等于 3 时，循环将停止。

让我们假装是 Python 编译器。下面是循环运行时发生的情况。

列表如下:`languages = ['Python', 'JavaScript', 'Java']`。它有三个索引— 0、1 和 2。

当`i`为 0 = > Python

当`i`为 1 = >时

当`i`是 2 = > Java

当`i`为 3 时= >列表中未找到索引。`IndexError: list index out of range`抛出错误。

所以当`i`等于 3 时抛出错误，因为列表中没有索引为 3 的项。

要解决这个问题，我们可以通过删除等号来修改循环的条件。这将在循环到达最后一个索引时停止循环。

方法如下:

```
languages = ['Python', 'JavaScript', 'Java']

i = 0

while i < len(languages):
    print(languages[i])
    i += 1

    # Python
    # JavaScript
    # Java 
```

现在的情况是这样的:`while i < 3`。

循环将在 2 处停止，因为条件不允许它等于`len()`函数返回的值。

## 在 Python 中使用`range()`函数时，如何修复中的`IndexError: list index out of range`错误

默认情况下，`range()`函数返回从零开始的指定数字的“范围”。

下面是一个正在使用的`range()`函数的例子:

```
for num in range(5):
  print(num)
    # 0
    # 1
    # 2
    # 3
    # 4
```

正如你在上面的例子中看到的，`range(5)`返回 0，1，2，3，4。

您可以使用带有循环的`range()`函数来打印列表中的项目。

第一个例子将展示一个抛出`IndexError: list index out of range`错误的代码块。在指出错误发生的原因后，我们将修复它。

```
languages = ['Python', 'JavaScript', 'Java']

for language in range(4):
  print(languages[language])
    # Python
    # JavaScript
    # Java
    # Traceback (most recent call last):
    #   File "<string>", line 5, in <module>
    # IndexError: list index out of range
```

上面的例子打印了列表中的所有项目以及`IndexError: list index out of range`错误。

我们得到这个错误是因为`range(4)`返回 0，1，2，3。我们的列表没有值为 3 的索引。

要解决这个问题，您可以修改`range()`函数中的参数。更好的解决方案是使用列表的长度作为`range()`函数的参数。

那就是:

```
languages = ['Python', 'JavaScript', 'Java']

for language in range(len(languages)):
  print(languages[language])
    # Python
    # JavaScript
    # Java
```

上面的代码运行时没有任何错误，因为`len()`函数返回 3。与`range(3)`一起使用将返回 0，1，2，它与列表中的项目数相匹配。

## 摘要

在本文中，我们讨论了 Python 中的`IndexError: list index out of range`错误。

当我们试图使用列表中不存在的索引来访问列表中的项目时，通常会发生此错误。

我们看到了一些例子，展示了在使用循环、`len()`函数和`range()`函数时，我们是如何得到错误的。

我们还看到了如何修复每种情况下的`IndexError: list index out of range`错误。

编码快乐！