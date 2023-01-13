# 类型错误:无法解包不可迭代的 nonetype 对象-如何在 Python 中修复

> 原文：<https://www.freecodecamp.org/news/typeerror-cannot-unpack-non-iterable-nonetype-object-how-to-fix-in-python/>

当您使用 Python 中的列表、集合和元组等可迭代对象时，您可能希望将这些对象中的项分配给单独的变量。这是一个被称为拆包的过程。

在对可迭代对象中的项进行解包的过程中，您可能会得到一个错误消息:“类型错误:无法解包不可迭代的非类型对象”。

这个错误主要发生在你试图将一个类型为`None`的对象赋给一组独立变量的时候。这可能听起来有点混乱，但是一旦我们看到一些例子，就会清楚得多。

在此之前，我们先来说说在错误信息中看到的一些关键术语。我们将讨论以下术语:类型错误、解包和非类型。

## Python 中什么是 TypeError？

当在操作中使用不兼容的数据类型时，Python 中会出现 TypeError。

TypeError 的一个例子是在操作中使用了一个`None`数据类型和一个 iterable 对象。

## Python 中的解包是什么？

要解释拆包，你得明白打包是什么意思。

当您用 Python 创建一个包含项目的列表时，您已经将这些项目“打包”到一个数据结构中。这里有一个例子:

```
names = ["John", "Jane", "Doe"]
```

在上面的代码中，我们将“John”、“Jane”和“Doe”打包到一个名为`names`的列表中。

为了解开这些项目，我们必须将每个项目分配给一个单独的变量。方法如下:

```
names = ["John", "Jane", "Doe"]

a, b, c = names

print(a,b,c)
# John Jane Doe
```

既然我们已经创建了`names`列表，我们可以通过创建新的变量并将它们分配给列表:`a, b, c = names`来轻松地解包列表。

因此，`a`将获得列表中的第一项，`b`将获得第二项，`c`将获得第三项。那就是:

`a` =【约翰】
`b` =【简】
`c` =【多伊】

## Python 中的 NoneType 是什么？

Python 中的 NoneType 是一种数据类型，简单的表示一个对象没有值/有值`None`。

您可以将`None`的值赋给一个变量，但是也有返回`None`的方法。

我们将处理 Python 中的`sort()`方法，因为它通常与“类型错误:无法解包不可迭代的非类型对象”错误相关联。这是因为该方法返回值`None`。

接下来，我们将看到一个引发“TypeError:无法解包不可迭代的 NoneType 对象”错误的示例。

## “TypeError:无法解包不可迭代的 NoneType 对象”错误示例

在这一节中，您将理解为什么我们在解包一个列表之前错误地使用了`sort()`方法而得到一个错误。

```
names = ["John", "Jane", "Doe"]

names = names.sort()

a, b, c = names

print(a,b,c)
# TypeError: cannot unpack non-iterable NoneType object
```

在上面的例子中，我们试图使用`sort()`方法对`names`列表进行升序排序。

之后，我们继续打开列表。但是当我们打印出新的变量时，我们得到了一个错误。

这将我们带到错误消息中的最后一个重要术语:`non-iterable`。排序后，`names`列表变成了一个`None`对象，而不是一个列表(一个可迭代对象)。

出现这个错误是因为我们将`names.sort()`赋给了`names`。由于`names.sort()`返回`None`，我们已经覆盖并赋值`None`给一个曾经是列表的变量。那就是:

`names` = `names.sort()`
但是`names.sort()` = `None`
所以`names` = `None`

所以错误消息试图告诉你在一个`None`对象中没有任何东西可以解包。

这很容易解决。我们将在下一节中介绍这一点。

## 如何修复 Python 中的“类型错误:无法解包不可迭代的非类型对象”错误

出现此错误是因为我们试图解包一个`None`对象。最简单的方法是不要将`names.sort()`指定为列表的新值。

在 Python 中，您可以对变量集合使用`sort()`方法，而无需将操作的结果重新分配给正在排序的集合。

以下是解决该问题的方法:

```
names = ["John", "Jane", "Doe"]

names.sort()

a, b, c = names

print(a,b,c)
Doe Jane John
```

现在一切都很好。列表已被排序并解压缩。

我们只改变了`names.sort()`，而不是使用`names` = `names.sort()`。

现在，当打开列表时，`a, b, c`将按升序被分配到`names`中的项目。那就是:

`a` =“母鹿”
`b` =“简”
`c` =“约翰”

## 摘要

在本文中，我们讨论了 Python 中的“类型错误:无法解包不可迭代的非类型对象”错误。

我们解释了错误消息中的关键术语:类型错误、解包、非类型和不可迭代。

然后我们看到了一些例子。第一个例子展示了错误地使用`sort()`是如何引起错误的，而第二个例子展示了如何修复错误。

尽管修复“TypeError:无法解包不可迭代的 NoneType 对象”的错误很容易，但是理解错误消息中的重要术语很重要。这不仅有助于解决这个特定的错误，而且有助于您理解和解决包含类似术语的错误。

编码快乐！