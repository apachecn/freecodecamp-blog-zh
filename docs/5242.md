# 五分钟学会 Python 中的类型转换

> 原文：<https://www.freecodecamp.org/news/learn-typecasting-in-python-in-five-minutes-90d42c439743/>

by PALAKOLLU SRI MANIKANTA

# 五分钟学会 Python 中的类型转换

#### 以非常简洁的方式介绍 Python 中的类型转换和类型转换

![pH646PiGhvTTFSW0l649xmzU-3d08zvH-dbS](img/d12d79cb8069c85dc64b69f831823725.png)

Photo by [Suzanne D. Williams](https://unsplash.com/@scw1217?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

### 铸字

将一种数据类型转换成另一种数据类型的过程称为**类型转换**或**类型强制**或**类型转换**。

我将在本文中关注的主题是:

1.  隐式类型转换
2.  显式类型转换
3.  优势
4.  不足之处

### 隐式类型转换

当类型转换由解释器自动执行而没有程序员的干预时，该类型的转换被称为**隐式类型转换**。

#### **示例程序:**

```
myInt = 143     # Integer value.myFloat = 1.43  # Float value.
```

```
myResult = myInt + myFloat   # Sum result
```

```
print("datatype of myInt:",type(myInt))print("datatype of myFloat:",type(myFloat))
```

```
print("Value of myResult:",myResult)print("datatype of myResult:",type(myResult))
```

#### **输出:**

上述程序的输出将是:

```
datatype of myInt: <class 'int'>datatype of myFloat: <class 'float'>Value of myResult: 144.43datatype of myResult: <class 'float'>
```

在上面的程序中，

*   我们添加两个变量 myInt 和 myFloat，将值存储在 myResult 中。
*   我们将分别查看所有三个对象的数据类型。
*   在输出中，我们可以看到 myInt 的数据类型是一个`integer`，myFloat 的数据类型是一个`float`。
*   此外，我们可以看到 myFloat 具有`float`数据类型，因为 Python 将较小的数据类型转换为较大的数据类型，以避免数据丢失。

这种类型的转换称为**隐式类型转换**(或)**向上转换** *。*

### 显式类型转换

在显式类型转换中，用户将对象的数据类型转换为所需的数据类型。我们使用预定义的内置函数，如:

1.  int()
2.  浮动()
3.  复杂的
4.  布尔()
5.  str()

显式类型转换的语法是:

```
(required_datatype)(expression)
```

这种类型的转换称为**显式** **类型转换**(或)**下投** *。*

#### **Int 转换**

我们可以使用这个函数将其他类型的值转换为 int。

例如:

```
>>> int(123.654)123
```

```
>>>int(False)0
```

```
>>> int("10")10
```

```
>>> int("10.5")ValueError: invalid literal for int() with base 10: '10.5'
```

```
>>> int("ten")ValueError: invalid literal for int() with base 10: 'ten'
```

```
>>> int("0B1111")ValueError: invalid literal for int() with base 10: '0B1111'
```

```
>>> int(10+3j)TypeError: can't convert complex to int
```

#### **注:**

1.  不能将复杂数据类型转换为整型
2.  如果要将字符串类型转换为 int 类型，字符串文字必须包含以 10 为基数的值

#### 浮点转换

该函数用于将**任意数据类型转换为浮点数。**

例如:

```
>>> float(10) 10.0
```

```
>>> float(True)1.0
```

```
>>> float(False)0.0
```

```
>>> float("10")10.0
```

```
>>> float("10.5")10.5
```

```
>>> float("ten")ValueError: could not convert string to float: 'ten'
```

```
>>> float(10+5j)TypeError: can't convert complex to float
```

```
>>> float("0B1111")ValueError: could not convert string to float: '0B1111'
```

#### 注意:

1.  您可以将复杂类型转换为浮点类型值。
2.  如果要将字符串类型转换为浮点类型，字符串文字必须包含基数为 10 的值。

#### 复杂转换

此函数用于**将实数转换为复数(实数、虚数)。**

#### **表格 1:复合体(x)**

您可以使用此函数将单个值转换为具有实部 x 和虚部 0 的复数。

例如:

```
>>> complex(10)10+0j
```

```
>>> complex(10.5)10.5+0j
```

```
>>> complex(True)1+0j
```

```
>>> complex(False)0+0j
```

```
>>> complex("10")10+0j
```

```
>>> complex("10.5")10.5+0j
```

```
>>> complex("ten")ValueError: complex() arg is a malformed string
```

#### **形式 2:复数(x，y)**

如果你想把 X 和 Y 转换成复数，那么 X 是实部，Y 是虚部。

例如:

```
>>> complex(10,-2)10-2j
```

```
>>> complex(True, False)1+0j
```

#### 布尔转换

该函数用于将任何数据类型轻松转换为布尔数据类型。它是 Python 中最灵活的数据类型。

例如:

```
>>> bool(0)False
```

```
>>> bool(1)True
```

```
>>> bool(10)True
```

```
>>> bool(0.13332)True
```

```
>>> bool(0.0)False
```

```
>>> bool(10+6j)True
```

```
>>> bool(0+15j)True
```

```
>>> bool(0+0j)False
```

```
>>> bool("Apple")True
```

```
>>> bool("")False
```

> 注意:在 bool 函数的帮助下，您可以将任何类型的数据类型转换为 boolean，输出将是-对于所有值，它将产生 True，除了 0，0+0j 和空字符串。

#### 字符串转换

该函数用于**将任何类型转换为字符串类型。**

例如:

```
>>> str(10)'10'
```

```
>>> str(10.5)'10.5'
```

```
>>> str(True)'True'
```

```
>>> str(False)'False'
```

```
>>> str(10+5j)'10+5j'
```

```
>>> str(False)'False'
```

#### 示例程序:

```
integer_number = 123  # Intstring_number = "456" # String
```

```
print("Data type of integer_number:",type(integer_number))print("Data type of num_str before Type Casting:",type(num_str))
```

```
string_number = int(string_number)print("Data type of string_number after Type Casting:",type(string_number))
```

```
number_sum = integer_number + string_number
```

```
print("Sum of integer_number and num_str:",number_sum)print("Data type of the sum:",type(number_sum))
```

#### **输出:**

当我们运行上面的程序时，输出将是:

```
Data type of integer_number: <class 'int'>Data type of num_str before Type Casting: <class 'str'>Data type of string_number after Type Casting: <class 'int'>Sum of integer_number and num_str: 579Data type of the sum: <class 'int'>
```

在上面的程序中，

*   我们添加了 string_number 和 integer_number 变量。
*   我们使用`int()`函数执行加法，将 string_number 从 string(高位)转换为 integer(低位)类型。
*   将 string_number 转换为整数值后，Python 添加了这两个变量。
*   我们得到的 number_sum 值和数据类型是一个整数。

### 类型转换的优点

1.  使用起来更方便

### 类型转换的缺点

1.  更复杂的类型系统
2.  意外强制转换导致的错误来源

我几乎涵盖了在 Python3 中执行任何类型的类型转换操作所需的所有内容。

希望这有助于你快速简单地了解 Python 类型转换。

如果你喜欢这篇文章，请点击拍手并给我留下你的宝贵反馈。