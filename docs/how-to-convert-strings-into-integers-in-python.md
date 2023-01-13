# 如何在 Python 中将字符串转换成整数

> 原文：<https://www.freecodecamp.org/news/how-to-convert-strings-into-integers-in-python/>

类似于内置的`str()`方法，Python 也提供了方便的`int()`方法，该方法接受一个字符串对象作为参数并返回一个整数。

#### **示例用法:**

```
# Here age is a string object
age = "18"
print(age)

# Converting a string to an integer
int_age = int(age)
print(int_age)
```

**输出:**

```
18
18
```

虽然输出在视觉上是相似的，但是请记住第一行是一个字符串对象，而下一行是一个整数对象。这将在下一个示例中进一步说明:

```
age = "18"
print(age + 2)
```

**输出:**

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: cannot concatenate 'str' and 'int' objects
```

这个错误应该让您明白，在添加内容之前，您需要将`age`对象转换为整数。

```
age = "18"
age_int = int(age)
print(age_int + 2)
```

**输出:**

```
20
```

但是请记住这些特殊情况:

*   作为参数的浮点(带小数部分的整数)将返回向下舍入到最接近的整数的浮点数。比如:`print(int(7.9))`会打印`7`。另一方面，`print(int("7.9"))`会导致错误，因为作为字符串对象的浮点数不能被转换成整数。

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: '7.9'
```

*   作为参数给出的单词将返回相同的错误。例如，`print(int("one"))`将返回:

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: 'one'
```