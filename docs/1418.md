# python map()–列表函数及示例

> 原文：<https://www.freecodecamp.org/news/python-map-explained-with-examples/>

Python 提供了许多函数式编程工具，尽管它主要是一种面向对象的编程语言。其中最值得注意的是 map()函数。

在本文中，我们将探索什么是`map()`函数以及如何在您的代码中使用它。

# Python 中的 map()函数

`map()`函数(Python 中的内置函数)用于将函数应用于 *iterable* (类似于 Python 列表或字典)中的每一项。它返回一个新的 iterable(一个 *map 对象)*，您可以在代码的其他部分使用它。

一般的语法是:

```
map(function, iterable, [iterable1, iterable2, ...])
```

让我们看一个例子:假设你有一个数字列表，你想用第一个列表中数字的*立方*创建一个新列表。传统的方法是将*用于*循环:

```
org_list = [1, 2, 3, 4, 5]
fin_list = []

for num in org_list:
    fin_list.append(num**3)

print(fin_list) # [1, 8, 27, 64, 125]
```

这是完全正确的，但是让我们看看使用`map()`函数如何简化您的代码:

```
org_list = [1, 2, 3, 4, 5]

# define a function that returns the cube of `num`
def cube(num):
    return num**3

fin_list = list(map(cube, org_list))
print(fin_list) # [1, 8, 27, 64, 125]
```

我不知道你怎么想，但我发现这是一个更清晰的逻辑。

> 如果您想知道幕后发生了什么，`map()`函数实际上遍历了 iterable 的每个元素(在我们的例子中是`org_list`)，并对其应用了 cube 函数。它最终返回了一个新的 iterable ( `fin_list`)结果。

## 如何在 Python 中使用 Lambda 表达式

我们可以使用一个 *lambda* 表达式来代替编写一个单独的函数来计算一个数的立方。你应该这样做:

```
fin_list = list(map(lambda x:x**3, org_list))
print(fin_list) # [1, 8, 27, 64, 125]
```

干净多了，你同意吗？

## 如何使用 Python 中的内置函数

还可以传入内置的 Python 函数。例如，如果你有一个字符串列表，你可以很容易地用列表中每个字符串的长度创建一个新列表。

```
org_list = ["Hello", "world", "freecodecamp"]
fin_list = list(map(len, org_list))
print(fin_list) # [5, 5, 12]
```

## 如何在 Python 中使用具有多个可迭代项的函数

到目前为止，我们已经进入了只接受一个参数的`map()`函数(回想一下`cube(num)`)。但是如果你的函数接受多个参数呢？一个这样的例子是接受两个参数的`pow(x, y)`函数(它返回 x^y).的结果

要应用一个有多个参数的函数，只需在第一个名字后面传入另一个可迭代的名字。

```
base = [1, 2, 3, 4]
power = [1, 2, 3, 4]

result = list(map(pow, base, power))
print(result) # [1, 4, 27, 256]
```

# 包扎

在本文中，您已经学习了如何使用 Python 中的`map()`函数。您还看到了它如何显著地减少代码的大小，使代码更具可读性和无 bug 性。

现在你应该可以轻松地使用内置函数、lambda 表达式，甚至你自己的自定义函数来使用`map()`!

请务必在 Twitter 上[关注我，了解未来文章的更新。祝你愉快！](http://twitter.com/jasmcaus)