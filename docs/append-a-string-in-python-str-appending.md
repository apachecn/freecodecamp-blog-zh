# 在 Python 中追加字符串–字符串追加

> 原文：<https://www.freecodecamp.org/news/append-a-string-in-python-str-appending/>

在本文中，您将学习在 Python 中追加字符串的不同方法。

谈到追加字符串时，另一个常用术语是串联。所以你会经常看到这些术语——追加和连接——互换使用。

无论哪种方式，追加或连接字符串都意味着将一个字符串的值添加或连接到另一个字符串。

让我们通过简单的代码示例来看看实现这一点的不同方法。

## 如何使用`+`操作符在 Python 中追加字符串

您可以使用`+`操作符追加两个或更多的字符串。这里有一个例子:

```
first_name = "John"
last_name = "Doe"

print(first_name + last_name)
# JohnDoe
```

在上面的例子中，我们创建了两个字符串变量—`first_name`和`last_name`。它们的值分别为“John”和“Doe”。

为了添加这些变量，我们使用了`+`操作符:`first_name + last_name`。

您会注意到在输出中，我们将两个变量没有任何间隔地连接在一起:`JohnDoe`。

可以在`first_name`值后面加一个空格:“John”。或者在`last_name`值之前:“Doe”。那就是:

```
first_name = "John "
last_name = "Doe"

print(first_name + last_name)
# John Doe
```

您也可以在追加字符串时使用引号来添加间距。方法如下:

```
first_name = "John "
last_name = "Doe"

print(first_name + "" + last_name)
# John Doe
```

## 如何使用`join()`方法在 Python 中追加字符串

在 Python 中追加字符串的另一种方法是使用`join()`方法。

`join()`方法将一个 iterable 对象——列表、元组、字符串、集合、字典——作为它的参数。下面是语法的样子:

```
string.join(iterable_object)
```

下面的例子展示了 use 如何使用`join()`方法追加字符串:

```
first_name = "John"
last_name = "Doe"

print("".join([first_name, last_name]))
# JohnDoe
```

这里，我们将两个字符串变量作为参数传递给了`join()`方法。

您还会注意到变量被嵌套在方括号`[]`中，使其成为一个字符串列表:`[first_name, last_name]`。这是因为该方法只接受一个必须是 iterable 对象的参数。

关于`join()`方法的一个奇怪的事情是在句点/点之前的引号。

您可以使用这些引号来说明 iterable 对象值中的各项之间出现的内容。我举个例子来演示一下。

```
first_name = "John"
last_name = "Doe"

print("#".join([first_name, last_name]))
# John#Doe
```

在上面的例子中，我在引号中添加了`#`符号:`"#".join([first_name, last_name])`。这个`#`加在我们的琴弦之间:`John#Doe`。

在上一节中，我们不得不使用不同的方法来增加字符串之间的间距。您可以通过在`join()`方法之前的引号中添加一个空格来轻松实现这一点:

```
first_name = "John"
last_name = "Doe"

print(" ".join([first_name, last_name]))
# John Doe
```

## 如何使用 String `format()`方法在 Python 中追加字符串

下面是字符串`format()`方法的语法:

```
{}.format(value)
```

基本上，字符串格式方法采用上面语法中的`value`参数，并将其插入到花括号中。结果值将是一个字符串。

这里有一个例子:

```
first_name = "John"
last_name = "Doe"

print("{} {}".format(first_name, last_name))
# John Doe
```

因为我们在示例中提供了两个花括号和两个参数(`first_name`和`last_name`)，所以 string `format()`方法将字符串插入到它们各自的花括号中。

您可以在引号中找到花括号的地方添加更多字符串。这不会改变 string `format()`方法的操作——字符串仍然会被插入到花括号中。那就是:

```
first_name = "John"
last_name = "Doe"

print("My name is {} {}".format(first_name, last_name))
# My name is John Doe
```

## 如何使用 f 字符串在 Python 中追加字符串

这种方法很容易理解。f 字符串是在 Python 中引入的，以使字符串格式化和插值更容易。但是也可以用它来追加字符串。

要使用 f 字符串，您只需写一个 f 后跟引号:`f""`。然后，您可以在引号之间插入字符串和变量名。所有变量名必须嵌套在花括号中。

这里有一个例子:

```
first_name = "John"
last_name = "Doe"

print(f"{first_name} {last_name}")
# John Doe
```

## 摘要

在本文中，我们讨论了在 Python 中添加字符串的不同方法。

将一个字符串附加到另一个字符串意味着将它们连接在一起。

正如本文所讨论的，通过代码示例，您可以使用`+`操作符、`join()`方法、`string()`格式方法和 f 字符串在 Python 中追加字符串。

编码快乐！