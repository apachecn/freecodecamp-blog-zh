# 如何在 Python 中合并字典

> 原文：<https://www.freecodecamp.org/news/merge-dictionaries-in-python/>

在 Python 中，字典是用来以`{key:value}`对的形式存储数据的集合。它是有序的和可变的，它不能存储重复的数据。

我们用像这样的花括号来编写字典:

```
my_dict = {
    "id": 1,
    "name": "Ashutosh",
    "books": ["Python", "DSA"]
} 
```

有时，我们需要合并两个或更多的字典来创建一个更大的字典。例如:

```
dict_one = {
    "id": 1,
    "name": "Ashutosh",
    "books": ["Python", "DSA"]
}

dict_two = {
    "college": "NSEC",
    "city": "Kolkata",
    "country": "India"
}

merged_dict = {
    "id": 1,
    "name": "Ashutosh",
    "books": ["Python", "DSA"],
    "college": "NSEC",
    "city": "Kolkata",
    "country": "India"
} 
```

在`merged_dict`中，我们有`dict_one`和`dict_two`的键值对。这是我们希望通过编程实现的。

在 Python 中有多种方法可以做到这一点:

1.  使用 for 循环
2.  使用`dict.update()`方法
3.  使用`**`操作符
4.  使用`|` (Union)运算符(对于 Python 3.9 和更高版本)

让我们一个接一个地探索每一种方法。

## 如何使用 For 循环在 Python 中合并字典

我们可以使用 for 循环合并两个或多个字典，如下所示:

```
>>> dict_one = {
...     "id": 1,
...     "name": "Ashutosh",
... }
>>> dict_two = {
...     "books": ["Python", "DSA"],
...     "college": "NSEC",
... }
>>> dict_three = {
...     "city": "Kolkata",
...     "country": "India"
... }
>>> for key,value in dict_two.items():
...     merged_dict[key] = value
... 
>>> merged_dict
{'id': 1, 'name': 'Ashutosh', 'books': ['Python', 'DSA'], 'college': 'NSEC'}
>>> for key,value in dict_three.items():
...     merged_dict[key] = value
... 
>>> merged_dict
{'id': 1, 'name': 'Ashutosh', 'books': ['Python', 'DSA'], 'college': 'NSEC', 'city': 'Kolkata', 'country': 'India'}
```

但是这种方法的问题是我们需要运行很多循环来合并字典。

那么还有什么选择呢？

## 如何使用`dict.update()`方法在 Python 中合并字典

如果你探究一下`dict`类，里面有各种各样的方法。一个这样的方法是`update()`方法，您可以使用它将一个字典合并到另一个字典中。

```
>>> dict_one = {
...     "id": 1,
...     "name": "Ashutosh",
...     "books": ["Python", "DSA"]
... }
>>> dict_two = {
...     "college": "NSEC",
...     "city": "Kolkata",
...     "country": "India"
... }
>>> dict_one.update(dict_two)
>>> dict_one
{'id': 1, 'name': 'Ashutosh', 'books': ['Python', 'DSA'], 'college': 'NSEC', 'city': 'Kolkata', 'country': 'India'}
```

但是当我们使用`update()`方法时，问题是它修改了其中一个字典。如果我们希望创建第三个字典而不修改任何其他字典，我们不能使用这种方法。

此外，您只能使用此方法一次合并两个词典。如果您希望合并三个字典，您首先需要合并前两个，然后将第三个与修改后的字典合并。

```
>>> dict_one = {
...     "id": 1,
...     "name": "Ashutosh",
... }
>>> dict_two = {
...     "books": ["Python", "DSA"],
...     "college": "NSEC",
... }
>>> dict_three = {
...     "city": "Kolkata",
...     "country": "India"
... }
>>> dict_one.update(dict_two)
>>> dict_one
{'id': 1, 'name': 'Ashutosh', 'books': ['Python', 'DSA'], 'college': 'NSEC'}
>>> dict_one.update(dict_three)
>>> dict_one
{'id': 1, 'name': 'Ashutosh', 'books': ['Python', 'DSA'], 'college': 'NSEC', 'city': 'Kolkata', 'country': 'India'}
```

让我们探索一些其他的选择。

## 如何使用`**`操作符在 Python 中合并字典

您可以使用双星号(**)方法来解包或扩展字典，如下所示:

```
>>> dict_one = {
...     "id": 1,
...     "name": "Ashutosh",
... }
>>> dict_two = {
...     "books": ["Python", "DSA"]
...     "college": "NSEC",
... }
>>> dict_three = {
...     "city": "Kolkata",
...     "country": "India"
... }
>>> merged_dict = {**dict_one, **dict_two, **dict_three} 
>>> merged_dict
{'id': 1, 'name': 'Ashutosh', 'books': ['Python', 'DSA'], 'college': 'NSEC', 'city': 'Kolkata', 'country': 'India'}
```

使用`**`操作符合并字典不会影响任何字典。

## 如何使用`|`操作符在 Python 中合并字典

从 Python 3.9 开始，我们可以使用 Union ( `|`)操作符来合并两个或多个字典。

```
>>> dict_one = {
...     "id": 1,
...     "name": "Ashutosh",
... }
>>> dict_two = {
...     "books": ["Python", "DSA"],
...     "college": "NSEC",
... }
>>> dict_three = {
...     "city": "Kolkata",
...     "country": "India"
... }
>>> merged_dict = dict_one | dict_two | dict_three
>>> merged_dict
{'id': 1, 'name': 'Ashutosh', 'books': ['Python', 'DSA'], 'college': 'NSEC', 'city': 'Kolkata', 'country': 'India'}
```

这是在 Python 中合并字典最方便的方法。

## 结论

我们已经探索了几种不同的合并字典的方法。如果你有 Python 3.9 或者更高版本，你应该使用`|`操作符。但是如果您使用旧版本的 Python，您仍然可以使用上面讨论的其他方法。