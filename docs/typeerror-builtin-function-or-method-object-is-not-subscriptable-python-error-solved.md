# type Error:builtin _ function _ or _ method 对象不是可订阅的 Python 错误[已解决]

> 原文：<https://www.freecodecamp.org/news/typeerror-builtin-function-or-method-object-is-not-subscriptable-python-error-solved/>

顾名思义，错误`TypeError: builtin_function_or_method object is not subscriptable`是一种“类型错误”,当您试图以错误的方式调用内置函数时就会发生。

当一个“类型错误”发生时，程序告诉你你混淆了类型。这意味着，例如，你可以将一个字符串和一个整数连接起来。

在本文中，我将向您展示为什么会出现 type error:builtin _ function _ or _ method 对象不可订阅，以及如何修复它。

## 为什么 type error:builtin _ function _ or _ method 对象不可订阅

Python 的每一个内置函数如`print()`、`append()`、`sorted()`、`max()`等都必须用圆括号或圆括号(`()`)来调用。

如果你试图使用方括号，Python 不会把它当作函数调用。相反，Python 会认为你试图从一个列表或字符串中访问某些东西，然后抛出错误。

例如，下面的代码抛出了错误，因为我试图在`print()`函数前面用方括号打印变量的值:

如果您用方括号将想要打印的内容括起来，即使该项目是可迭代的，您仍然会得到错误:

```
gadgets = ["Mouse", "Monitor", "Laptop"]
print[gadgets[0]]

# Output: Traceback (most recent call last):
#   File "built_in_obj_not_subable.py", line 2, in <module>
#     print[gadgets[0]]
# TypeError: 'builtin_function_or_method' object is not subscriptable 
```

这个问题不是`print()`函数所特有的。如果您试图用方括号调用任何其他内置函数，也会得到错误。

在下面的例子中，我试图用方括号调用`max()`,得到的错误是:

```
numbers = [5, 7, 24, 6, 90]
max_num = max(numbers)
print[max_num]

# Traceback (most recent call last):
#   File "built_in_obj_not_subable.py", line 11, in <module>
#     print[max_num]
# TypeError: 'builtin_function_or_method' object is not subscriptable 
```

## 如何修复类型错误:builtin_function_or_method 对象不是可订阅的错误

要修复这个错误，您需要做的就是确保使用括号来调用函数。

如果要从字符串、列表或元组等可迭代数据中访问某个项，只需使用方括号:

```
gadgets = ["Mouse", "Monitor", "Laptop"]
print(gadgets[0])

# Output: Mouse 
```

```
numbers = [5, 7, 24, 6, 90]
max_num = max(numbers)
print(max_num)

# Output: 90 
```

## 包扎

本文向您展示了为什么会出现 type error:builtin _ function _ or _ method 对象不可订阅以及如何修复它。

请记住，您只需要使用方括号(`[]`)来访问可迭代数据中的项目，而不应该使用它来调用函数。

如果你得到了这个错误，你应该在你的代码中寻找任何一点，在那里你用方括号调用一个内置函数，并用圆括号替换它。

感谢阅读。