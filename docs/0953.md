# 如何在 Python 中使用*args 和**kwargs

> 原文：<https://www.freecodecamp.org/news/args-and-kwargs-in-python/>

在本文中，我们将讨论 Python 中的 **`*args`** 和`****kwargs**`以及它们的用法和一些例子。

在编写函数时，我们经常需要向函数传递值。这些值被称为**函数参数**。

## 函数参数有问题

让我们在 Python 中定义一个将两个数相加的函数。我们会这样写:

```
def add(x, y):
    return x+y

print(add(2,3))
```

输出:

```
5
```

如果需要加三个数呢？简单来说，我们可以修改函数来接受三个参数，并将它们的和返回为:

```
def add(x, y, z):
    return x+y+z

print(add(2, 3, 5))
```

输出:

```
10
```

这不是很简单吗？是的，它是！

但是如果我们再次被要求只把两个数相加呢？我们修改过的函数会帮助我们得到总和吗？让我们看看:

```
def add(x, y, z):
    return x+y+z

print(add(2, 3))
```

输出:

```
Traceback (most recent call last):
  File "D:\Quarantine\Test\Blog-Codes\args-kwargs\main.py", line 14, in <module>
    print(add(2, 3))
TypeError: add() missing 1 required positional argument: 'z'
```

你看到问题了吗？

当我们有可变数量的参数时，问题就出现了。我们应该不断修改函数来接受参数的确切数目吗？当然不是，我们不会这么做。

所以一定有其他方法。这就是 **`*args`** 和`****kwargs**`的切入点。

当您不确定函数中要传递的参数数量时，可以使用 **`*args`** 和`****kwargs**`作为函数的参数。

## 如何在 Python 中使用*args

**`*args`** 允许我们向 Python 函数传递可变数量的非关键字参数。在函数中，我们应该在参数名前使用星号( **`*`** )来传递可变个数的参数。

```
def add(*args):
    print(args, type(args))

add(2, 3)
```

输出:

```
(2, 3) <class 'tuple'>
```

因此，我们确信这些传递的参数在函数内部形成了一个与参数同名的元组，不包括 **`*`** 。

现在让我们用可变数量的参数重写我们的`**add()**`函数。

```
def add(*numbers):
    total = 0
    for num in numbers:
        total += num
    return total

print(add(2, 3))
print(add(2, 3, 5))
print(add(2, 3, 5, 7))
print(add(2, 3, 5, 7, 9))
```

输出:

```
5
10
17
26
```

请注意，参数的名称不一定是`**args**`**——它可以是任何名称。这种情况下是`**numbers**`。但是用`***args**`作为名字通常是一种标准的方式。**

## **如何在 Python 中使用**kwargs**

****`**kwargs`** 允许我们向 Python 函数传递可变数量的关键字参数。在函数中，我们在参数名前使用双星号( **`**`** )来表示这种类型的参数。**

```
`def total_fruits(**kwargs):
    print(kwargs, type(kwargs))

total_fruits(banana=5, mango=7, apple=8)`
```

**输出:**

```
`{'banana': 5, 'mango': 7, 'apple': 8} <class 'dict'>`
```

**因此，我们看到，在这种情况下，参数作为一个[字典](https://ireadblog.com/posts/127/everything-you-need-to-know-about-python-dictionaries)传递，这些参数在函数内部创建了一个与参数名称相同的字典，不包括 **`**`** 。**

**现在，让我们完成`**total_fruits()**`函数来返回水果的总数。**

```
`def total_fruits(**fruits):
    total = 0
    for amount in fruits.values():
        total += amount
    return total

print(total_fruits(banana=5, mango=7, apple=8))
print(total_fruits(banana=5, mango=7, apple=8, oranges=10))
print(total_fruits(banana=5, mango=7))`
```

**输出:**

```
`20
30
12`
```

**请注意，参数的名称不一定是`**kwargs**`**——同样，它可以是任何名称。这种情况下是`**fruits**`。但是用`****kwargs**`作为名字通常是一种标准的方式。****

## ****结论****

****在本文中，我们学习了 Python 中的两个特殊关键字——**`*args`**和`****kwargs**`。这使得 Python 函数非常灵活，因此它可以分别接受可变数量的参数和关键字参数。****

****感谢阅读！****

****你可以在这里找到这个博客的代码。****