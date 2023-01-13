# Int 对象不可迭代–Python 错误[已解决]

> 原文：<https://www.freecodecamp.org/news/int-object-is-not-iterable-python-error-solved/>

如果您正在运行 Python 代码，并看到错误“type error:' int ' object is not iterable”，这意味着您正在尝试循环整数或循环无法处理的其他数据类型。

在 Python 中，可迭代数据是列表、元组、集合、字典等等。

此外，这个错误是“TypeError ”,意味着您试图对不合适的数据类型执行操作。例如，将字符串与整数相加。

今天是运行 Python 代码时出现此错误的最后一天。因为在这篇文章中，我将不仅仅向您展示如何修复它，我还将向您展示如何检查`__iter__`魔法方法，这样您就可以看到一个对象是否是可迭代的。

## 如何修复 Int 对象是不可迭代的

如果您尝试遍历一个整数，将会得到以下错误:

```
count = 14

for i in count:
    print(i)
# Output: TypeError: 'int' object is not iterable 
```

解决这个问题的一个方法是将变量传递给`range()`函数。

在 Python 中，range 函数检查传递给它的变量，并返回一系列数字，这些数字从 0 开始，正好在指定数字之前停止。

该循环现在将运行:

```
count = 14

for i in range(count):
    print(i)

# Output: 0
# 1
# 2
# 3
# 4
# 5
# 6
# 7
# 8
# 9
# 10
# 11
# 12
# 13 
```

使用此解决方案的另一个示例在下面的代码片段中:

```
age = int(input("Enter your age: "))

for num in range(age):
    print(num)

# Output: 
# Enter your age: 6
# 0
# 1
# 2
# 3
# 4
# 5 
```

## 如何检查数据或对象是否可迭代

要检查某些特定数据是否是可迭代的，可以使用`dir()`方法。如果你能看到神奇的方法`__iter__`，那么数据就是可迭代的。如果不是这样，那么数据是不可迭代的，你不应该麻烦地遍历它们。

```
perfectNum = 7

print(dir(perfectNum))

# Output:['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__init_subclass__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', 
# '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes'] 
```

在输出中没有找到`__iter__`魔法方法，所以变量`perfectNum`是不可迭代的。

```
jerseyNums = [43, 10, 7, 6, 8]

print(dir(jerseyNums))

# Output: ['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort'] 
```

魔法方法`__iter__`被找到，所以列表`jerseyNums`是可迭代的。

## 结论

在本文中，您了解了“Int Object is Not Iterable”错误以及如何修复它。

您还可以看到，检查一个对象或一些数据是否可迭代是可能的。

如果您在一些数据中检查`__iter__`魔法方法，但没有找到，最好不要尝试遍历数据，因为它们是不可迭代的。

感谢您的阅读。