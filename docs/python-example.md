# Python 示例–数据结构、算法、语法示例代码

> 原文：<https://www.freecodecamp.org/news/python-example/>

Python 是一种通用编程语言，它是动态类型化的、可解释的，并且以其简单的可读性和优秀的设计原则而闻名。

freeCodeCamp 有一个最受欢迎的 Python 课程。完全免费。你可以在 YouTube 上看这个视频。

![maxresdefault](img/78d520922041325377ae0c5fc97abff9.png)

# Python 数据结构示例

关于浮点数以及它们在 Python 中如何工作的一些一般信息，可以在这里找到。

几乎所有 Python 的实现都遵循 IEEE 754 规范:二进制浮点运算的标准。更多信息可在 IEEE 网站上找到。

可以使用[浮点文字](https://docs.python.org/3/reference/lexical_analysis.html#floating-point-literals)创建浮点对象:

```
>>> 3.14
3.14
>>> 314\.    # Trailing zero(s) not required.
314.0
>>> .314    # Leading zero(s) not required.
0.314
>>> 3e0
3.0
>>> 3E0     # 'e' or 'E' can be used.
3.0
>>> 3e1     # Positive value after e moves the decimal to the right.
30.0
>>> 3e-1    # Negative value after e moves the decimal to the left.
0.3
>>> 3.14e+2 # '+' not required but can be used for exponent part.
314.0
```

数字文字不包含符号，但是可以通过在文字前加上一元`-`(减)运算符(没有空格)来创建负浮点对象:

```
>>> -3.141592653589793
-3.141592653589793
>>> type(-3.141592653589793)
<class 'float'>
```

同样，正 float 对象可以以一元的`+`(加号)操作符为前缀，在字面量之前没有空格。通常`+`被省略:

```
>>> +3.141592653589793
3.141592653589793
```

请注意，前导零和尾随零对浮点文字有效。

同样，正 float 对象可以以一元的`+`(加号)操作符为前缀，在字面量之前没有空格。通常`+`被省略:

```
>>> +3.141592653589793
3.141592653589793
```

请注意，前导零和尾随零对浮点文字有效。

```
>>> 0.0
0.0
>>> 00.00
0.0
>>> 00100.00100
100.001
>>> 001e0010      # Same as 1e10
10000000000.0
```

[`float`构造函数](https://docs.python.org/3/library/functions.html#float)是创建`float`对象的另一种方式。

在可能的情况下，最好使用浮点文字创建`float`对象:

```
>>> a = 3.14         # Prefer floating point literal when possible.
>>> type(a)
<class 'float'>
>>> b = int(3.14)    # Works but unnecessary.
>>> type(b)
<class 'float'>
```

但是，float 构造函数允许从其他数字类型创建 float 对象:

```
>>> a = 4
>>> type(a)
<class 'int'>
>>> print(a)
4
>>> b = float(4)
>>> type(b)
<class 'float'>
>>> print(b)
4.0
>>> float(400000000000000000000000000000000)
4e+32
>>> float(.00000000000000000000000000000004)
4e-32
>>> float(True)
1.0
>>> float(False)
0.0
```

`float`构造函数还将从表示数字文字的字符串中生成`float`对象:

```
>>> float('1')
1.0
>>> float('.1')
0.1
>>> float('3.')
3.0
>>> float('1e-3')
0.001
>>> float('3.14')
3.14
>>> float('-.15e-2')
-0.0015
```

`float`构造函数也可以用来表示`NaN`(不是数字)、负数`infinity`和`infinity`(注意这些字符串不区分大小写):

```
>>> float('nan')
nan
>>> float('inf')
inf
>>> float('-inf')
-inf
>>> float('infinity')
inf
>>> float('-infinity')
-inf
```

### 复数

复数有一个实部和一个虚部，每个都用一个浮点数表示。

一个复数的虚部可以用一个虚文字来创建，这导致一个复数的实部为`0.0`:

```
>>> a = 3.5j
>>> type(a)
<class 'complex'>
>>> print(a)
3.5j
>>> a.real
0.0
>>> a.imag
3.5
```

不存在创建具有非零实部和虚部的复数的文字。要创建非零实部复数，请向浮点数添加一个虚数:

```
>>> a = 1.1 + 3.5j
>>> type(a)
<class 'complex'>
>>> print(a)
(1.1+3.5j)
>>> a.real
1.1
>>> a.imag
3.5
```

或者使用[复杂构造函数](https://docs.python.org/3/library/functions.html#complex)。

```
class complex([real[, imag]])
```

用于调用复杂构造函数的参数可以是数字(包括`complex`)类型，用于以下任一参数:

```
>>> complex(1, 1)
(1+1j)
>>> complex(1j, 1j)
(-1+1j)
>>> complex(1.1, 3.5)
(1.1+3.5j)
>>> complex(1.1)
(1.1+0j)
>>> complex(0, 3.5)
3.5j
```

A `string`也可以作为自变量。如果将字符串用作参数，则不允许有第二个参数

```
>>> complex("1.1+3.5j")
(1.1+3.5j)
```

# python 胸部示例

`bool()`是 Python 3 中的内置函数。该函数返回一个布尔值，即 True 或 False。它需要一个参数，`x`。

## **自变量**

它需要一个参数。使用标准的[真值测试程序](https://docs.python.org/3/library/stdtypes.html#truth)转换`x`。

## **返回值**

如果`x`为假或省略，则返回`False`；否则它返回`True`。

## **代码示例**

```
print(bool(4 > 2)) # Returns True as 4 is greater than 2
print(bool(4 < 2)) # Returns False as 4 is not less than 2
print(bool(4 == 4)) # Returns True as 4 is equal to 4
print(bool(4 != 4)) # Returns False as 4 is equal to 4 so inequality doesn't holds
print(bool(4)) # Returns True as 4 is a non-zero value
print(bool(-4)) # Returns True as -4 is a non-zero value
print(bool(0)) # Returns False as it is a zero value
print(bool('dskl')) # Returns True as the string is a non-zero value
print(bool([1, 2, 3])) # Returns True as the list is a non-zero value
print(bool((2,3,4))) # Returns True as tuple is a non-zero value
print(bool([])) # Returns False as list is empty and equal to 0 according to truth value testing
```

# Python 布尔运算符示例

[`and`](https://docs.python.org/3/reference/expressions.html#and)[`or`](https://docs.python.org/3/reference/expressions.html#or)[`not`](https://docs.python.org/3/reference/expressions.html#not)

[Python 文档-布尔运算](https://docs.python.org/3/library/stdtypes.html#boolean-operations-and-or-not)

这些是布尔运算，按优先级升序排列:

OperationResultNotes x 或 y 如果 x 为假，则 y，else x (1) x 和 y 如果 x 为假，则 x，else y (2) not x 如果 x 为假，则 True，else False (3)。

****备注:****

1.  这是一个短路操作符，所以如果第一个参数为假，它只计算第二个参数。
2.  这是一个短路操作符，所以只有当第一个参数为真时，它才计算第二个参数。
3.  not 的优先级比非布尔运算符低，所以 not a == b 解释为 not (a == b)，a == not b 是语法错误。

## **例子:**

### **:**

```
>>> not True
False
>>> not False
True
```

### **:**

```
>>> True and False    # Short-circuited at first argument.
False
>>> False and True    # Second argument is evaluated.
False
>>> True and True     # Second argument is evaluated.
True
```

### **:**

```
>>> True or False    # Short-circuited at first argument.
True
>>> False or True    # Second argument is evaluated.
True
>>> False or False   # Second argument is evaluated.
False
```

# Python 常量示例

三个常用的内置常数:

*   `True`:*布尔*类型的真值。对`True`的赋值产生一个*语法错误*。
*   `False`:*布尔*类型的假值。对`False`的赋值产生一个*语法错误*。
*   `None`:字体 *NoneType* 的唯一值。None 通常用于表示缺少值，例如当默认参数没有传递给函数时。对`None`的赋值产生一个*语法错误*。

其他内置常数:

*   `NotImplemented`:应由二进制特殊方法返回的特殊值，如`__eg__()`、`__add__()`、`__rsub__()`等。)来指示该操作对于另一个类型没有实现。
*   `Ellipsis`:特殊值，通常与用户定义的容器数据类型的扩展切片语法结合使用。
*   `__debug__`:如果 Python 没有使用-o 选项启动，则为 True。

站点模块添加的常量。site 模块(在启动时自动导入，除非给出了-S 命令行选项)向内置名称空间添加了几个常量。它们对于交互式解释器外壳很有用，不应该在程序中使用。

这些对象在打印时会打印一条类似“使用 quit()或 Ctrl-D(即 EOF)退出”的消息，并且在被调用时会使用指定的退出代码引发 SystemExit:

*   退出(代码=无)
*   退出(代码=无)

这些对象在打印时会打印一条类似“键入许可证()以查看完整的许可证文本”的消息，并在被调用时以类似寻呼机的方式显示相应的文本(一次显示一个屏幕):

*   版权
*   许可证
*   信用

# 调用 Python 函数示例

函数定义语句不执行函数。执行(调用)一个函数是通过使用该函数的名称，后跟括起所需参数(如果有的话)的括号来完成的。

```
>>> def say_hello():
...     print('Hello')
...
>>> say_hello()
Hello
```

函数的执行引入了一个新的符号表，用于函数的局部变量。更准确地说，函数中的所有变量赋值都将值存储在局部符号表中。

另一方面，变量引用首先在局部符号表中查找，然后在封闭函数的局部符号表中查找，然后在全局符号表中查找，最后在内置名称表中查找。因此，全局变量不能在函数中直接赋值(除非在全局语句中命名)，尽管它们可能被引用。

```
>>> a = 1
>>> b = 10
>>> def fn():
...     print(a)    # local a is not assigned, no enclosing function, global a referenced.
...     b = 20      # local b is assigned in the local symbol table for the function.
...     print(b)    # local b is referenced.
...
>>> fn()
1
20
>>> b               # global b is not changed by the function call.
10
```

函数调用的实际参数(自变量)在被调用函数被调用时被引入到被调用函数的局部符号表中。通过这种方式，使用 call by value 传递参数(其中值始终是对象引用，而不是对象的值)。当一个函数调用另一个函数时，会为该调用创建一个新的局部符号表。

```
>>> def greet(s):
...     s = "Hello " + s    # s in local symbol table is reassigned.
...     print(s)
...
>>> person = "Bob"
>>> greet(person)
Hello Bob
>>> person                  # person used to call remains bound to original object, 'Bob'.
'Bob'
```

用于调用函数的参数不能被函数重新分配，但是引用可变对象的参数的值可以改变:

```
>>> def fn(arg):
...     arg.append(1)
...
>>> a = [1, 2, 3]
>>> fn(a)
>>> a
[1, 2, 3, 1]
```

# Python 类示例

类提供了一种将数据和功能捆绑在一起的方法。创建一个新类会创建一个新类型的对象，允许创建该类型的新实例。每个类实例都可以附加属性来维护其状态。类实例也可以有修改其状态的方法(由其类定义)。

与其他编程语言相比，Python 的类机制以最少的新语法和语义添加了类。它混合了 C++中的类机制。

Python 类提供了面向对象编程的所有标准特性:类继承机制允许多个基类，派生类可以覆盖其基类的任何方法，方法可以调用同名基类的方法。

对象可以包含任意数量和种类的数据。与模块一样，类也具有 Python 的动态特性:它们是在运行时创建的，创建后可以进一步修改。

#### **类定义语法:**

最简单的类定义形式如下:

```
class ClassName:
    <statement-1>
        ...
        ...
        ...
    <statement-N>
```

#### **类对象:**

类对象支持两种操作:属性引用和实例化。

属性引用使用 Python 中用于所有属性引用的标准语法:`obj.name`。有效的属性名是创建类对象时类的命名空间中的所有名称。所以，如果类的定义是这样的:

```
class MyClass:
    """ A simple example class """
    i = 12345

    def f(self):
        return 'hello world'
```

那么`MyClass.i`和`MyClass.f`就是有效的属性引用，分别返回一个整数和一个函数对象。类属性也可以被赋值，所以你可以通过赋值来改变`MyClass.i`的值。`__doc__`也是一个有效属性，返回属于类:`"A simple example class"`的 docstring。

类实例化使用函数符号。假设类对象是一个无参数的函数，它返回该类的一个新实例。例如(假设上面的类):

```
x = MyClass()
```

创建类的新实例，并将此对象赋给局部变量 x。

实例化操作(“调用”一个类对象)创建一个空对象。许多类喜欢用定制到特定初始状态的实例来创建对象。因此一个类可以定义一个名为 ****init**** ()的特殊方法，如下所示:

```
def __init__(self):
    self.data = []
```

当一个类定义了一个`__init__()`方法时，类实例化自动为新创建的类实例调用`__init__()`。因此，在本例中，可以通过以下方式获得一个新的初始化实例:

```
x = MyClass()
```

当然，`__init__()`方法可能有更大灵活性的理由。在这种情况下，给予类实例化操作符的参数被传递给`__init__()`。举个例子，

```
class Complex:
    def __init__(self, realpart, imagpart):
        self.r = realpart
        self.i = imagpart
              ...

x = Complex(3.0, -4.5)
>>> x.r, x.i
(3.0, -4.5)
```

# Python 代码块和缩进示例

在 Python 中编码时，不要混合使用制表符和空格，这通常是一个好习惯。这样做可能会导致`TabError`，你的程序将会崩溃。编写代码时保持一致——选择使用制表符或空格缩进，并在整个程序中遵循您选择的约定。

#### **代码块和缩进**

Python 最与众不同的特性之一是使用缩进来标记代码块。考虑我们简单的密码检查程序中的 if 语句:

```
if pwd == 'apple':
    print('Logging on ...')
else:
    print('Incorrect password.')

print('All done!')
```

这几行打印('登录…')和打印('密码不正确。')是两个独立的代码块。这些恰好只有一行，但是 Python 允许您编写由任意数量的语句组成的代码块。

要在 Python 中表示一个代码块，必须将代码块的每一行缩进相同的量。我们的示例 if-statement 中的两个代码块都缩进了四个空格，这是 Python 的典型缩进量。

在大多数其他编程语言中，缩进只是用来帮助代码看起来漂亮。但是在 Python 中，它是指示一个语句属于哪个代码块所必需的。例如，最终的印刷(“全部完成！”)不是缩进的，因此不是 else 块的一部分。

熟悉其他语言的程序员常常对缩进很重要的想法感到恼火:许多程序员喜欢随心所欲地格式化他们的代码。然而，Python 缩进规则非常简单，大多数程序员已经使用缩进来使他们的代码可读。Python 只是将这一思想向前推进了一步，赋予了缩进意义。

#### **If/elif-语句**

if/elif 语句是具有多个条件的广义 if 语句。它被用来做复杂的决定。例如，假设一家航空公司有以下“儿童”票价:2 岁或 2 岁以下的儿童免费乘坐飞机，2 岁以上但 13 岁以下的儿童支付折扣儿童票价，13 岁以上的人支付普通成人票价。以下程序决定了乘客应支付的费用:

```
# airfare.py
age = int(input('How old are you? '))
if age <= 2:
    print(' free')
elif 2 < age < 13:
    print(' child fare)
else:
    print('adult fare')
```

Python 从用户那里获得年龄后，它进入 if/elif 语句，并按照给出的顺序逐个检查每个条件。

因此，它首先检查年龄是否小于 2，如果是，则表明飞行是自由的，并跳出 elif-条件。如果年龄不小于 2，则它检查下一个 elif 条件，以查看年龄是否在 2 和 13 之间。如果是这样，它打印适当的消息并跳出 if/elif 语句。如果 If 条件和 elif 条件都不为真，那么它执行 else 块中的代码。

#### **条件表达式**

Python 多了一个逻辑操作符，有些程序员喜欢(有些不喜欢！).它本质上是可以直接在表达式中使用的 if 语句的简写符号。考虑以下代码:

```
food = input("What's your favorite food? ")
reply = 'yuck' if food == 'lamb' else 'yum'
```

第二行中=右侧的表达式称为条件表达式，其计算结果为“yuck”或“yum”。它相当于以下内容:

```
food = input("What's your favorite food? ")
if food == 'lamb':
   reply = 'yuck'
else:
   reply = 'yum'
```

条件表达式通常比相应的 if/else 语句要短，尽管没有那么灵活或易读。一般来说，当它们使您的代码更简单时，您应该使用它们。

## Python 比较运算符示例

Python 中有八种比较操作。它们都具有相同的优先级(高于布尔运算的优先级)。比较可以任意链接；例如，`x < y <= z`等价于`x < y and y <= z`，除了`y`只被求值一次(但是在两种情况下当`x < y`被发现为假时`z`根本不被求值)。

下面总结了比较操作:

| 操作 | 意义 |
| --- | --- |
| `<` | 严格小于 |
| `<=` | 小于或等于 |
| `>` | 严格大于 |
| `>=` | 大于或等于 |
| `==` | 等于 |
| `!=` | 不等于 |
| `is` | 对象身份 |
| `is not` | 否定的对象标识 |

不同类型的对象，除了不同的数字类型，从来不会相等。此外，一些类型(例如，函数对象)只支持比较的退化概念，即任何两个该类型的对象都不相等。`<`、`<=`、`>`和`>=`运算符在将一个复数与另一个内置的数值类型进行比较时，当对象属于不同的类型而无法进行比较时，或者在没有定义排序的其他情况下，会引发`TypeError`异常。

除非类定义了`__eq__()`方法，否则一个类的不同实例通常被认为是不相等的。

一个类的实例不能相对于同一个类的其他实例或其他类型的对象进行排序，除非该类定义了足够多的方法`__lt__()`、`__le__()`、`__gt__()`和`__ge__()`(通常，`__lt__()`和`__eq__()`就足够了，如果你想要比较操作符的常规含义的话)。

不能自定义`is`和`is not`运算符的行为；此外，它们可以应用于任何两个对象，永远不会引发异常。

我们也可以将`<`和`>`操作符链接在一起。例如，`3 < 4 < 5`会返回`True`，而`3 < 4 > 5`不会。我们还可以链接等式运算符。例如，`3 == 3 < 5`将返回`True`，而`3 == 5 < 5`不会。

### **相等比较-"是" vs "=="**

在 Python 中，有两个比较运算符，允许我们检查两个对象是否相等。`is`操作员和`==`操作员。但是，它们之间有一个关键的区别！

“is”和“==”之间的主要区别可以总结为:

*   `is`用来比较 ****身份****
*   `==`用来比较 ****的平等****

## **例子**

首先，用 Python 创建一个列表。

```
myListA = [1,2,3]
```

接下来，创建该列表的副本。

```
myListB = myListA
```

如果我们使用' == '操作符或' is '操作符，两者都会产生一个 ****真**** 输出。

```
>>> myListA == myListB # both lists contains similar elements
True
>>> myListB is myListA # myListB contains the same elements
True
```

这是因为 myListA 和 myListB 都指向同一个列表变量，这是我在 Python 程序开始时定义的。两份名单完全一样，无论是身份还是内容。

但是，如果我现在创建一个新列表呢？

```
myListC = [1,2,3]
```

执行`==`操作符仍然显示两个列表在内容上是相同的。

```
>>> myListA == myListC
True
```

然而，执行`is`操作符现在将产生一个`False`输出。这是因为 myListA 和 myListC 是两个不同的变量，尽管包含相同的数据。即使他们看起来一样，他们是**T3**T5【不同】。

```
>>> myListA is myListC
False # both lists have different reference
```

总而言之:

*   如果两个变量指向同一个引用，一个`is`表达式输出`True`
*   如果两个变量包含相同的数据，一个`==`表达式输出`True`

# Python 字典示例

python 中的字典(又名“dict”)是一种内置的数据类型，可用于存储 ****`key-value`**** 对。这允许你把一个 ****`dict`**** 当作一个*数据库*来存储和组织数据。

字典的特别之处在于它们的实现方式。类似哈希表的结构使得检查存在性变得容易——这意味着我们可以容易地确定特定的键是否存在于字典中，而不需要检查每个元素。Python 解释器可以直接找到 location 键并检查该键是否在那里。

字典可以使用几乎任何任意的数据类型作为关键字，比如字符串、整数等等。然而，不可散列的值，即包含列表、字典或其他可变类型的值(通过值而不是通过对象标识进行比较)不能用作键。用于键的数字类型遵循数字比较的常规规则:如果两个数字比较相等(例如`1`和`1.0`)，那么它们可以互换使用来索引同一个字典条目。(但是请注意，由于计算机将浮点数存储为近似值，因此将它们用作字典键通常是不明智的。)

字典的一个最重要的要求是键 ****必须**** 是唯一的。

要创建一个空字典，只需使用一对大括号:

```
 >>> teams = {}
    >>> type(teams)
    >>> <class 'dict'>
```

要创建包含一些初始值的非空字典，请放置一个以逗号分隔的键值对列表:

```
 >>> teams = {'barcelona': 1875, 'chelsea': 1910}
    >>> teams
    {'barcelona': 1875, 'chelsea': 1910}
```

向现有字典添加键值对很容易:

```
 >>> teams['santos'] = 1787
    >>> teams
    {'chelsea': 1910, 'barcelona': 1875, 'santos': 1787} # Notice the order - Dictionaries are unordered !
    >>> # extracting value - Just provide the key
    ...
    >>> teams['barcelona']
    1875
```

****`del`**** 操作符用于从字典中删除一个键值对。在已经使用的键再次用于存储值的情况下，与该键关联的旧值会完全丢失。此外，请记住，使用不存在的键提取值是错误的。

```
 >>> del teams['santos']
    >>> teams
    {'chelsea': 1910, 'barcelona': 1875}
    >>> teams['chelsea'] = 2017 # overwriting    
    >>> teams
    {'chelsea': 2017, 'barcelona': 1875}
```

****`in`**** 关键字可以用来检查关键字是否存在于字典中:

```
 >>> 'sanots' in teams
    False    
    >>> 'barcelona' in teams
    True
    >>> 'chelsea' not in teams
    False
```

****`keys`**** 是一个内置的*方法*，可以用来获取给定字典的键。要将字典中的关键字提取为列表:

```
 >>> club_names = list(teams.keys())    
    >>> club_names
    ['chelsea', 'barcelona']
```

还有另一种创建字典的方法是使用 ****`dict()`**** 的方法:

```
 >>> players = dict( [('messi','argentina'), ('ronaldo','portugal'), ('kaka','brazil')] ) # sequence of key-value pair is passed  
    >>> players
    {'ronaldo': 'portugal', 'kaka': 'brazil', 'messi': 'argentina'}
    >>> 
    >>> # If keys are simple strings, it's quite easier to specify pairs using keyword arguments
    ...
    >>> dict( totti = 38, zidane = 43 )
    {'zidane': 43, 'totti': 38}
```

Dict comprehensions 也可以用于从任意键和值表达式创建字典:

```
 >>> {x: x**2 for x in (2, 4, 6)}
    {2: 4, 4: 16, 6: 36}
```

****在字典中循环****
简单地循环字典中的键，而不是键和值:

```
 >>> d = {'x': 1, 'y': 2, 'z': 3} 
    >>> for key in d:
    ...     print(key) # do something
    ...
    x
    y
    z
```

要循环访问键和值，可以使用下面的:
For Python 2.x:

```
 >>> for key, item in d.iteritems():
    ...     print items
    ...
    1
    2
    3
```

对于 Python 3.x 使用 ****`items()`**** :

```
 >>> for key, item in d.items():
    ...     print(key, items)
    ...
    x 1
    y 2
    z 3
```

# Python 对象示例

在 Python 中，一切都是一个*对象*。

*对象*代表属性的逻辑分组。属性是数据和/或函数。当在 Python 中创建一个对象时，它是用一个*身份*、*类型*和*值*创建的。

在其他语言中，*原语*是没有*属性*(属性)的*值*。比如 javascript 中的`undefined`、`null`、`boolean`、`string`、`number`、`symbol`(ECMAScript 2015 新增)都是原语。

在 Python 中，没有原语。`None`、*布尔*、*字符串*、*数字*，甚至*函数*无论如何创建，都是*对象*。

我们可以使用一些内置函数来演示这一点:

*   [T2`id`](https://docs.python.org/3/library/functions.html#id)
*   [T2`type`](https://docs.python.org/3/library/functions.html#type)
*   [T2`dir`](https://docs.python.org/3/library/functions.html#dir)
*   [T2`issubclass`](https://docs.python.org/3/library/functions.html#issubclass)

内置常数`None`、`True`、`False`是*对象*:

我们在这里测试`None`对象。

```
>>> id(None)
4550218168
>>> type(None)
<class 'NoneType'>
>>> dir(None)
[__bool__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
>>> issubclass(type(None), object)
True
```

接下来，我们来考察一下`True`。

```
>>> id(True)
4550117616
>>> type(True)
<class 'bool'>
>>> dir(True)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
>>> issubclass(type(True), object)
True
```

没有理由漏掉`False`！

```
>>> id(False)
4550117584
>>> type(False)
<class 'bool'>
>>> dir(False)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
>>> issubclass(type(False), object)
True
```

*字符串*，即使是由字符串文字创建的，也是*对象*。

```
>>> id("Hello campers!")
4570186864
>>> type('Hello campers!')
<class 'str'>
>>> dir("Hello campers!")
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
>>> issubclass(type('Hello campers!'), object)
True
```

与*号相同。*

```
>>> id(42)
4550495728
>>> type(42)
<class 'int'>
>>> dir(42)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
>>> issubclass(type(42), object)
True
```

## **函数也是对象**

在 Python 中，函数是第一类对象。

Python 中的*函数*也是*对象*，用*身份*、*类型*和*值*创建。它们也可以传递给其他*函数*:

```
>>> id(dir)
4568035688
>>> type(dir)
<class 'builtin_function_or_method'>
>>> dir(dir)
['__call__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__name__', '__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__self__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__text_signature__']
>>> issubclass(type(dir), object)
True
```

也可以将函数绑定到一个名称，并使用该名称调用绑定的函数:

```
>>> a = dir
>>> print(a)
<built-in function dir>
>>> a(a)
['__call__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__name__', '__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__self__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__text_signature__']
```

## 名称绑定和别名功能

在 Python 中定义函数时，该函数名会被引入到当前符号表中。然后，函数名的值的类型被解释器识别为用户定义的函数:

```
>>> something = 1
>>> type(something)
<type 'int'>

>>> def something():
...     pass
...
>>> type(something)
<type 'function'>

>>> something = []
>>> type(something)
<type 'list'>
```

然后可以将函数值赋给另一个名称。在它被重新分配后，它仍然可以作为一个函数使用。使用此方法重命名您的函数:

```
>>> def something(n):
...     print(n)
...
>>> type(something)
<type 'function'>

>>> s = something
>>> s(100)
100
```

# Python 元组

元组是 Python 对象的序列。元组是不可变的，这意味着它们在创建后不能被修改，这与列表不同。

****创作:****

使用一对圆括号`()`创建一个空的`tuple`:

```
 >>> empty_tuple = ()
    >>> print(empty_tuple)
    ()
    >>> type(empty_tuple)
    <class 'tuple'>
    >>> len(empty_tuple)
    0
```

通过用逗号分隔元素来创建带有元素的`tuple`(圆括号`()`是可选的，但有例外):

```
 >>> tuple_1 = 1, 2, 3       # Create tuple without round brackets.
    >>> print(tuple_1)
    (1, 2, 3)
    >>> type(tuple_1)
    <class 'tuple'>
    >>> len(tuple_1)
    3
    >>> tuple_2 = (1, 2, 3)     # Create tuple with round brackets.
    >>> print(tuple_2)
    (1, 2, 3)
    >>> tuple_3 = 1, 2, 3,      # Trailing comma is optional.
    >>> print(tuple_3)
    (1, 2, 3)
    >>> tuple_4 = (1, 2, 3,)    # Trailing comma in round brackets is also optional.
    >>> print(tuple_4)
    (1, 2, 3)
```

具有单个元素的`tuple`必须有尾随逗号(有或没有圆括号):

```
>>> not_tuple = (2)    # No trailing comma makes this not a tuple.
>>> print(not_tuple)
2
>>> type(not_tuple)
<class 'int'>
>>> a_tuple = (2,)     # Single element tuple. Requires trailing comma.
>>> print(a_tuple)
(2,)
>>> type(a_tuple)
<class 'tuple'>
>>> len(a_tuple)
1
>>> also_tuple = 2,    # Round brackets omitted. Requires trailing comma.
>>> print(also_tuple)
(2,)
>>> type(also_tuple)
<class 'tuple'>
```

在不明确的情况下需要圆括号(如果元组是更大表达式的一部分):

注意，实际上是逗号构成了元组，而不是括号。括号是可选的，除了空元组的情况，或者当需要它们来避免语法歧义时。

例如，`f(a, b, c)`是一个带有三个参数的函数调用，而`f((a, b, c))`是一个带有一个 3 元组作为唯一参数的函数调用。

```
 >>> print(1,2,3,4,)          # Calls print with 4 arguments: 1, 2, 3, and 4
    1 2 3 4
    >>> print((1,2,3,4,))        # Calls print with 1 argument: (1, 2, 3, 4,)
    (1, 2, 3, 4)
    >>> 1, 2, 3 == (1, 2, 3)     # Equivalent to 1, 2, (3 == (1, 2, 3))
    (1, 2, False)
    >>> (1, 2, 3) == (1, 2, 3)   # Use surrounding round brackets when ambiguous.
    True
```

也可以用`tuple`构造函数创建一个`tuple`:

```
 >>> empty_tuple = tuple()
    >>> print(empty_tuple)
    ()
    >>> tuple_from_list = tuple([1,2,3,4])
    >>> print(tuple_from_list)
    (1, 2, 3, 4)
    >>> tuple_from_string = tuple("Hello campers!")
    >>> print(tuple_from_string)
    ('H', 'e', 'l', 'l', 'o', ' ', 'c', 'a', 'm', 'p', 'e', 'r', 's', '!')
    >>> a_tuple = 1, 2, 3
    >>> b_tuple = tuple(a_tuple)    # If the constructor is called with a tuple for
    the iterable,
    >>> a_tuple is b_tuple          # the tuple argument is returned.
    True
```

****访问元素一`tuple` :****

`tuples`的元素的访问和索引方式与`lists`相同。

```
>>> my_tuple = 1, 2, 9, 16, 25
>>> print(my_tuple)
(1, 2, 9, 16, 25)
```

*零索引*

```
 >>> my_tuple[0]
    1
    >>> my_tuple[1]
    2
    >>> my_tuple[2]
    9
```

*环绕分度*

```
 >>> my_tuple[-1]
    25
    >>> my_tuple[-2]
    16
```

****装箱拆箱:****

语句`t = 12345, 54321, 'hello!'`是元组打包的一个例子:值`12345`、`54321`和`'hello!'`一起打包在一个元组中。相反的操作也是可能的:

```
 >>> x, y, z = t
```

这被恰当地称为序列解包，适用于右侧的任何序列。序列解包要求等号左边的变量和序列中的元素一样多。注意，多重赋值实际上只是元组打包和序列解包的组合。

```
 >>> t = 1, 2, 3    # Tuple packing.
    >>> print(t)
    (1, 2, 3)
    >>> a, b, c = t    # Sequence unpacking.
    >>> print(a)
    1
    >>> print(b)
    2
    >>> print(c)
    3
    >>> d, e, f = 4, 5, 6    # Multiple assignment combines packing and unpacking.
    >>> print(d)
    4
    >>> print(e)
    5
    >>> print(f)
    6
    >>> a, b = 1, 2, 3       # Multiple assignment requires each variable (right)
    have a matching element (left).
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ValueError: too many values to unpack (expected 2)
```

****不可变:****

`tuples`是不可变的容器，保证它们包含的**对象不会改变。它不 ****而**** 保证它们包含的对象不会改变:**

```
 `>>> a_list = []
    >>> a_tuple = (a_list,)    # A tuple (immutable) with a list (mutable) element.
    >>> print(a_tuple)
    ([],)

    >>> a_list.append("Hello campers!")
    >>> print(a_tuple)         # Element of the immutable is mutated.
    (['Hello campers!'],)`
```

******用途:******

**函数只能返回一个值，然而，一个异构的`tuple`可以用来从一个函数返回多个值。一个例子是内置的`enumerate`函数，它返回一个异构`tuples`的 iterable:**

```
 `>>> greeting = ["Hello", "campers!"]
    >>> enumerator = enumerate(greeting)
    >>> enumerator.next()
    >>> enumerator.__next__()
    (0, 'Hello')
    >>> enumerator.__next__()
    (1, 'campers!')`
```

# **Python For 循环语句示例**

**Python 利用 for 循环来迭代元素列表。这与 C 或 Java 不同，C 或 Java 使用 for 循环逐步更改值，并使用该值访问数组之类的东西。**

**For 循环遍历基于集合的数据结构，如列表、元组和字典。**

**基本语法是:**

```
`for value in list_of_values:
  # use value inside this block`
```

**一般来说，您可以使用任何东西作为迭代器值，其中 iterable 的条目可以被赋值给。例如，您可以从元组列表中解包元组:**

```
`list_of_tuples = [(1,2), (3,4)]

for a, b in list_of_tuples:
  print("a:", a, "b:", b)`
```

**另一方面，你可以循环任何可迭代的东西。您可以调用函数或使用列表文字。**

```
`for person in load_persons():
  print("The name is:", person.name)`
```

```
`for character in ["P", "y", "t", "h", "o", "n"]:
  print("Give me a '{}'!".format(character))`
```

**使用 For 循环的一些方式:**

******迭代范围()函数******

```
`for i in range(10):
    print(i)`
```

**range 不是一个函数，它实际上是一个不可变的序列类型。输出将包含从下限(即 0)到上限(即 10)的结果，但不包括 10。默认情况下，下限或起始索引设置为零。输出:**

```
`>
0
1
2
3
4
5
6
7
8
9
>`
```

**此外，可以通过添加第二个和第三个参数来指定序列的下限，甚至序列的步长。**

```
`for i in range(4,10,2): #From 4 to 9 using a step of two
    print(i)`
```

**输出:**

```
`>
4
6
8
>`
```

******xrange()函数******

**在很大程度上，xrange 和 range 在功能上是完全相同的。它们都提供了一种生成整数列表的方法，供您随意使用。唯一的区别是 range 返回一个 Python 列表对象，而 xrange 返回一个 xrange 对象。这意味着 xrange 并不像 range 那样在运行时生成静态列表。它用一种叫做让步的特殊技术创造出你需要的价值。这种技术用于一种称为发生器的对象。**

**再补充一点。在 Python 3.x 中，xrange 函数不再存在。range 函数现在做 Python 2.x 中 xrange 做的事情**

******迭代列表或元组中的值******

```
`A = ["hello", 1, 65, "thank you", [2, 3]]
for value in A:
    print(value)`
```

**输出:**

```
`>
hello
1
65
thank you
[2, 3]
>`
```

```
**`fruits_to_colors = {"apple": "#ff0000",
                    "lemon": "#ffff00",
                    "orange": "#ffa500"}

for key in fruits_to_colors:
    print(key, fruits_to_colors[key])`**
```

****输出:****

```
**`>
apple #ff0000
lemon #ffff00
orange #ffa500
>`**
```

********用 zip()函数**** 在一个循环中迭代两个相同大小的列表****

```
`A = ["a", "b", "c"]
B = ["a", "d", "e"]

for a, b in zip(A, B):
  print a, b, a == b` 
```

**输出:**

```
`>
a a True
b d False
c e False
>`
```

******遍历一个列表，用 enumerate()函数**** 得到对应的索引**

```
`A = ["this", "is", "something", "fun"]

for index,word in enumerate(A):
    print(index, word)`
```

**输出:**

```
`>
0 this
1 is
2 something
3 fun
>`
```

**一个常见的用例是迭代字典:**

```
`for name, phonenumber in contacts.items():
  print(name, "is reachable under", phonenumber)`
```

**如果您绝对需要访问您的迭代的当前索引，请使用 ****而不是****`range(len(iterable))`！这是一种极其糟糕的做法，会让你遭到资深 Python 开发者的嘲笑。请改用内置函数`enumerate()`:**

```
`for index, item in enumerate(shopping_basket):
  print("Item", index, "is a", item)`
```

******for/else 语句**** Pyhton 允许在 for 循环中使用 else，当循环体中的所有条件都不满足时，就会执行 else 情况。要使用 else，我们必须使用`break`语句，这样我们就可以在满足条件的情况下跳出循环。如果我们不越狱，那么 else 部分将被执行。**

```
`week_days = ['Monday','Tuesday','Wednesday','Thursday','Friday']
today = 'Saturday'
for day in week_days:
  if day == today:
    print('today is a week day')
    break
else:
  print('today is not a week day')`
```

**在上面的例子中，输出将是`today is not a week day`,因为循环中的 break 永远不会被执行。**

******使用内联循环函数**** 遍历列表**

**我们也可以使用 python 进行内联迭代。例如，如果我们需要将列表中的所有单词大写，我们可以简单地执行以下操作:**

```
`A = ["this", "is", "awesome", "shinning", "star"]

UPPERCASE = [word.upper() for word in A]
print (UPPERCASE)`
```

**输出:**

```
`>
['THIS', 'IS', 'AWESOME', 'SHINNING', 'STAR']`
```

# **Python 函数示例**

**函数允许你定义一个可重用的代码块，它可以在你的程序中多次执行。**

**函数允许你为复杂的问题创建更多的模块和[干](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)的解决方案。**

**虽然 Python 已经提供了许多内置函数，比如`print()`和`len()`，但是您也可以定义自己的函数在项目中使用。**

**在代码中使用函数的一个很大的好处是，它减少了项目中代码的总行数。**

### ****语法****

**在 Python 中，函数定义具有以下特性:**

1.  **关键词`def`**
2.  **函数名**
3.  **括号'()'，并在括号内输入参数，尽管输入参数是可选的。**
4.  **冒号':'**
5.  **一些要执行的代码块**
6.  **返回语句(可选)**

```
`# a function with no parameters or returned values
def sayHello():
  print("Hello!")

sayHello()  # calls the function, 'Hello!' is printed to the console

# a function with a parameter
def helloWithName(name):
  print("Hello " + name + "!")

helloWithName("Ada")  # calls the function, 'Hello Ada!' is printed to the console

# a function with multiple parameters with a return statement
def multiply(val1, val2):
  return val1 * val2

multiply(3, 5)  # prints 15 to the console`
```

**函数是代码块，只需调用函数就可以重用。这使得简单、优雅的代码重用成为可能，而无需显式地重写代码部分。这使得代码更具可读性，更易于调试，并减少了键入错误。**

**Python 中的函数是使用`def`关键字创建的，后跟括号内的函数名和函数参数。**

**函数总是返回值。函数使用关键字`return`来返回值。如果不想返回任何值，就返回默认值`None`。**

**函数名用于调用函数，在括号内传递所需的参数。**

```
`# this is a basic sum function
def sum(a, b):
  return a + b

result = sum(1, 2)
# result = 3`
```

**您可以为参数定义默认值，如果没有给定，Python 会将该参数的值解释为默认值。**

```
`def sum(a, b=3):
  return a + b

result = sum(1)
# result = 4`
```

**您可以使用参数的名称，按照您想要的顺序传递参数。**

```
`result = sum(b=2, a=2)
# result = 4`
```

**但是，不可能在非关键字参数之前传递关键字参数。**

```
`result = sum(3, b=2)
#result = 5
result2 = sum(b=2, 3)
#Will raise SyntaxError`
```

**函数也是对象，所以你可以把它们赋给一个变量，然后像函数一样使用这个变量。**

```
`s = sum
result = s(1, 2)
# result = 3`
```

### ****注释****

**如果函数定义包含参数，则在调用函数时必须提供相同数量的参数。**

```
`print(multiply(3))  # TypeError: multiply() takes exactly 2 arguments (0 given)

print(multiply('a', 5))  # 'aaaaa' printed to the console

print(multiply('a', 'b'))  # TypeError: Python can't multiply two strings`
```

**函数将运行的代码块包括函数内缩进的所有语句。**

```
`def myFunc():
print('this will print')
print('so will this')

x = 7
# the assignment of x is not a part of the function since it is not indented`
```

**函数中定义的变量只存在于该函数的范围内。**

```
`def double(num):
x = num * 2
return x

print(x)  # error - x is not defined
print(double(4))  # prints 8`
```

**Python 只在调用函数时解释函数块，而不是在定义函数时解释。因此，即使函数定义块包含某种错误，python 解释器也只会在调用函数时指出来。**

# **Python 生成器示例**

**生成器是一种特殊类型的函数，允许您在不结束函数的情况下返回值。它通过使用`yield`关键字来实现这一点。类似于`return`,`yield`表达式将返回一个值给调用者。两者的关键区别在于`yield`将暂停函数，允许将来返回更多的值。**

**生成器是可迭代的，因此它们可以干净地用于 for 循环或任何迭代。**

```
`def my_generator():
    yield 'hello'
    yield 'world'
    yield '!'

for item in my_generator():
    print(item)

# output:
# hello
# world
# !`
```

**像其他迭代器一样，生成器可以传递给`next`函数来检索下一项。当生成器没有更多的值要生成时，就会产生一个`StopIteration`错误。**

```
`g = my_generator()
print(next(g))
# 'hello'
print(next(g))
# 'world'
print(next(g))
# '!'
print(next(g))
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# StopIteration`
```

**当您需要创建大量的值，但不需要同时将它们保存在内存中时，生成器特别有用。例如，如果您需要打印前一百万个斐波那契数，您通常会返回一个包含一百万个值的列表，并迭代该列表以打印每个值。然而，使用生成器，您可以一次返回一个值:**

```
`def fib(n):
    a = 1
    b = 1
    for i in range(n):
        yield a
        a, b = b, a + b

for x in fib(1000000):
    print(x)`
```

# **Python 迭代器示例**

**Python 支持容器迭代的概念。这是使用两种不同的方法实现的；这些用于允许用户定义的类支持迭代。**

**[Python 文档-迭代器类型](https://docs.python.org/3/library/stdtypes.html#iterator-types)**

**迭代是以编程方式将一个步骤重复给定次数的过程。程序员可以利用迭代对数据集合中的每一项执行相同的操作，例如打印出列表中的每一项。**

*   **对象可以实现一个返回迭代器对象的`__iter__()`方法来支持迭代。**

**迭代器对象必须实现:**

*   **`__iter__()`:返回迭代器对象。**
*   **`__next__()`:返回容器的下一个对象，迭代器 *object = 'abc '。 ****iter**** () print(迭代器*对象)print(id(迭代器*对象))print(id(迭代器*对象。 ****iter**** ())) #返回迭代器本身。打印(迭代器*对象。 ****下一个**** ()) #返回第一个对象并推进迭代器。打印(迭代器*对象。 ****下一个**** ()) #返回第二个对象并推进迭代器。打印(迭代器*对象。 ****下一个**** ()) #返回第三个对象并推进迭代器。打印(迭代器*对象。 ****下一个**** ()) #引发 StopIteration 异常。**

**输出:**

```
`<str_iterator object at 0x102e196a0>
4343305888
4343305888
a
b
c
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-1-d466eea8c1b0> in <module>()
      6 print(iterator_object.__next__())     # Returns 2nd object and advances iterator.
      7 print(iterator_object.__next__())     # Returns 3rd object and advances iterator.
----> 8 print(iterator_object.__next__())     # Raises StopIteration Exception.

StopIteration:`
```

# ****Python 示例中的三元运算符****

**Python 中的三元运算，通常也称为条件表达式，允许程序员执行评估并根据给定条件的真值返回值。**

**三元运算符不同于标准的`if`、`else`、`elif`结构，因为它不是控制流结构，其行为更像其他运算符，如 Python 语言中的`==`或`!=`。**

### ****例子****

**在这个例子中，如果变量`val`是偶数，则返回字符串`Even`，否则返回字符串`Odd`。然后将返回的字符串赋给`is_even`变量并打印到控制台。**

#### ****输入****

```
`for val in range(1, 11):
    is_even = "Even" if val % 2 == 0 else "Odd"
    print(val, is_even, sep=' = ')`
```

#### ****输出****

```
`1 = Odd
2 = Even
3 = Odd
4 = Even
5 = Odd
6 = Even
7 = Odd
8 = Even
9 = Odd
10 = Even`
```

# **Python While 循环语句示例**

**Python 与其他流行语言一样利用了`while`循环。`while`循环评估一个条件，如果条件为真，则执行一段代码。代码块重复执行，直到条件变为假。**

**基本语法是:**

```
`counter = 0
while counter < 10:
   # Execute the block of code here as
   # long as counter is less than 10`
```

**下面显示了一个示例:**

```
`days = 0    
week = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
while days < 7:
   print("Today is " + week[days])
   days += 1`
```

**输出:**

```
`Today is Monday
Today is Tuesday
Today is Wednesday
Today is Thursday
Today is Friday
Today is Saturday
Today is Sunday`
```

**以上代码的逐行解释:**

1.  **变量“天数”被设置为值 0。**
2.  **变量 week 被分配给包含一周中所有日期的列表。**
3.  **当循环开始时**
4.  **代码块将被执行，直到条件返回“真”。**
5.  **条件是“天数< 7”，大致意思是运行 while 循环，直到变量天数小于 7**
6.  **因此，当 days=7 时，while 循环停止执行。**
7.  **days 变量在每次迭代中都会更新。**
8.  **当 while 循环第一次运行时，控制台上会显示一行“今天是星期一”,变量 days 等于 1。**
9.  **因为变量 days 等于小于 7 的 1，所以再次执行 while 循环。**
10.  **当控制台打印出“今天是星期天”时，变量 days 现在等于 7，while 循环停止执行。**

## **Python 中的 f 字符串**

**在 Python 版中，实现了一种格式化字符串的新方法。这种新方法被称为文字字符串插值(虽然通常被称为 f 字符串)。**

**f-string 的使用允许程序员以干净简洁的方式动态地将变量插入到字符串中。除了在字符串中插入变量之外，这个特性还为程序员提供了计算表达式、连接集合内容，甚至在 f 字符串中调用函数的能力。**

**为了在 f 字符串中执行这些动态行为，我们将它们放在字符串中的花括号内，并在字符串的开头加上小写的 f(在左引号之前)。**

### ****例题****

**在运行时将变量动态插入字符串:**

```
`name = 'Jon Snow'
greeting = f'Hello! {name}'
print(greeting)`
```

**计算字符串中的表达式:**

```
`val1 = 2
val2 = 3
expr = f'The sum of {val1} + {val2} is {val1 + val2}'
print(expr)`
```

**调用函数并在字符串中插入输出:**

```
`def sum(*args):
    result = 0
    for arg in args:
        result += arg
    return result

func = f'The sum of 3 + 5 is {sum(3, 5)}'
print(func)`
```

**将集合的内容连接到字符串中:**

```
`fruits = ['Apple', 'Banana', 'Pear']

list_str = f'List of fruits: {", ".join(fruits)}'
print(list_str)`
```

# **如何安装 Python 3**

**可以从这个官方[链接](https://www.python.org/downloads/)下载 Python。基于你的操作系统(Windows 或 Linux 或 OSX)，你可能想按照这些指令[安装 Python 3。](http://docs.python-guide.org/en/latest/starting/installation/)**

## ****使用虚拟环境****

**用沙箱保护你的 Python 安装并把它与你的 T2 系统 Python 分开总是一个好主意。*系统 Python* 是 Python 解释器的路径，随操作系统一起安装的其他模块会用到它。**

**直接用*系统 Python* 安装 Python Web-frames 或库**不安全。相反，在开发 Python 应用程序时，您可以使用 [Virtualenv](https://virtualenv.readthedocs.org/en/latest/) 来创建和派生一个单独的 Python 进程。****

### ****虚拟包装****

****[Virtualenvwrapper 模块](https://virtualenvwrapper.readthedocs.org/en/latest/)使您可以轻松地在一台机器上管理和运行多个 Python 沙盒环境，而不会破坏任何用 Python 编写并由您的机器使用的模块或服务。****

****当然，大多数云托管的开发环境，如 [Nitrous](https://www.nitrous.io/) 或 [Cloud9](https://c9.io/) 也预装了这些，并为您获取编码做好了准备！激活 Python 3 环境后，您可以从仪表板中快速选择一个框并开始编码。****

****在 [Cloud9](https://c9.io/) 中，创建新的开发环境时需要选择 Django 框。****

****下面是几个 shell 命令示例。如果您希望复制粘贴，请注意,`$`符号是终端提示符的简写，它不是命令的一部分。我的终端提示符看起来像这样:****

```
**`alayek:~/workspace (master) # Python 示例–数据结构、算法、语法示例代码

> 原文：<https://www.freecodecamp.org/news/python-example/>

Python 是一种通用编程语言，它是动态类型化的、可解释的，并且以其简单的可读性和优秀的设计原则而闻名。

freeCodeCamp 有一个最受欢迎的 Python 课程。完全免费。你可以在 YouTube 上看这个视频。

![maxresdefault](img/78d520922041325377ae0c5fc97abff9.png)

# Python 数据结构示例

关于浮点数以及它们在 Python 中如何工作的一些一般信息，可以在这里找到。

几乎所有 Python 的实现都遵循 IEEE 754 规范:二进制浮点运算的标准。更多信息可在 IEEE 网站上找到。

可以使用[浮点文字](https://docs.python.org/3/reference/lexical_analysis.html#floating-point-literals)创建浮点对象:

```
>>> 3.14
3.14
>>> 314\.    # Trailing zero(s) not required.
314.0
>>> .314    # Leading zero(s) not required.
0.314
>>> 3e0
3.0
>>> 3E0     # 'e' or 'E' can be used.
3.0
>>> 3e1     # Positive value after e moves the decimal to the right.
30.0
>>> 3e-1    # Negative value after e moves the decimal to the left.
0.3
>>> 3.14e+2 # '+' not required but can be used for exponent part.
314.0
```

数字文字不包含符号，但是可以通过在文字前加上一元`-`(减)运算符(没有空格)来创建负浮点对象:

```
>>> -3.141592653589793
-3.141592653589793
>>> type(-3.141592653589793)
<class 'float'>
```

同样，正 float 对象可以以一元的`+`(加号)操作符为前缀，在字面量之前没有空格。通常`+`被省略:

```
>>> +3.141592653589793
3.141592653589793
```

请注意，前导零和尾随零对浮点文字有效。

同样，正 float 对象可以以一元的`+`(加号)操作符为前缀，在字面量之前没有空格。通常`+`被省略:

```
>>> +3.141592653589793
3.141592653589793
```

请注意，前导零和尾随零对浮点文字有效。

```
>>> 0.0
0.0
>>> 00.00
0.0
>>> 00100.00100
100.001
>>> 001e0010      # Same as 1e10
10000000000.0
```

[`float`构造函数](https://docs.python.org/3/library/functions.html#float)是创建`float`对象的另一种方式。

在可能的情况下，最好使用浮点文字创建`float`对象:

```
>>> a = 3.14         # Prefer floating point literal when possible.
>>> type(a)
<class 'float'>
>>> b = int(3.14)    # Works but unnecessary.
>>> type(b)
<class 'float'>
```

但是，float 构造函数允许从其他数字类型创建 float 对象:

```
>>> a = 4
>>> type(a)
<class 'int'>
>>> print(a)
4
>>> b = float(4)
>>> type(b)
<class 'float'>
>>> print(b)
4.0
>>> float(400000000000000000000000000000000)
4e+32
>>> float(.00000000000000000000000000000004)
4e-32
>>> float(True)
1.0
>>> float(False)
0.0
```

`float`构造函数还将从表示数字文字的字符串中生成`float`对象:

```
>>> float('1')
1.0
>>> float('.1')
0.1
>>> float('3.')
3.0
>>> float('1e-3')
0.001
>>> float('3.14')
3.14
>>> float('-.15e-2')
-0.0015
```

`float`构造函数也可以用来表示`NaN`(不是数字)、负数`infinity`和`infinity`(注意这些字符串不区分大小写):

```
>>> float('nan')
nan
>>> float('inf')
inf
>>> float('-inf')
-inf
>>> float('infinity')
inf
>>> float('-infinity')
-inf
```

### 复数

复数有一个实部和一个虚部，每个都用一个浮点数表示。

一个复数的虚部可以用一个虚文字来创建，这导致一个复数的实部为`0.0`:

```
>>> a = 3.5j
>>> type(a)
<class 'complex'>
>>> print(a)
3.5j
>>> a.real
0.0
>>> a.imag
3.5
```

不存在创建具有非零实部和虚部的复数的文字。要创建非零实部复数，请向浮点数添加一个虚数:

```
>>> a = 1.1 + 3.5j
>>> type(a)
<class 'complex'>
>>> print(a)
(1.1+3.5j)
>>> a.real
1.1
>>> a.imag
3.5
```

或者使用[复杂构造函数](https://docs.python.org/3/library/functions.html#complex)。

```
class complex([real[, imag]])
```

用于调用复杂构造函数的参数可以是数字(包括`complex`)类型，用于以下任一参数:

```
>>> complex(1, 1)
(1+1j)
>>> complex(1j, 1j)
(-1+1j)
>>> complex(1.1, 3.5)
(1.1+3.5j)
>>> complex(1.1)
(1.1+0j)
>>> complex(0, 3.5)
3.5j
```

A `string`也可以作为自变量。如果将字符串用作参数，则不允许有第二个参数

```
>>> complex("1.1+3.5j")
(1.1+3.5j)
```

# python 胸部示例

`bool()`是 Python 3 中的内置函数。该函数返回一个布尔值，即 True 或 False。它需要一个参数，`x`。

## **自变量**

它需要一个参数。使用标准的[真值测试程序](https://docs.python.org/3/library/stdtypes.html#truth)转换`x`。

## **返回值**

如果`x`为假或省略，则返回`False`；否则它返回`True`。

## **代码示例**

```
print(bool(4 > 2)) # Returns True as 4 is greater than 2
print(bool(4 < 2)) # Returns False as 4 is not less than 2
print(bool(4 == 4)) # Returns True as 4 is equal to 4
print(bool(4 != 4)) # Returns False as 4 is equal to 4 so inequality doesn't holds
print(bool(4)) # Returns True as 4 is a non-zero value
print(bool(-4)) # Returns True as -4 is a non-zero value
print(bool(0)) # Returns False as it is a zero value
print(bool('dskl')) # Returns True as the string is a non-zero value
print(bool([1, 2, 3])) # Returns True as the list is a non-zero value
print(bool((2,3,4))) # Returns True as tuple is a non-zero value
print(bool([])) # Returns False as list is empty and equal to 0 according to truth value testing
```

# Python 布尔运算符示例

[`and`](https://docs.python.org/3/reference/expressions.html#and)[`or`](https://docs.python.org/3/reference/expressions.html#or)[`not`](https://docs.python.org/3/reference/expressions.html#not)

[Python 文档-布尔运算](https://docs.python.org/3/library/stdtypes.html#boolean-operations-and-or-not)

这些是布尔运算，按优先级升序排列:

OperationResultNotes x 或 y 如果 x 为假，则 y，else x (1) x 和 y 如果 x 为假，则 x，else y (2) not x 如果 x 为假，则 True，else False (3)。

****备注:****

1.  这是一个短路操作符，所以如果第一个参数为假，它只计算第二个参数。
2.  这是一个短路操作符，所以只有当第一个参数为真时，它才计算第二个参数。
3.  not 的优先级比非布尔运算符低，所以 not a == b 解释为 not (a == b)，a == not b 是语法错误。

## **例子:**

### **:**

```
>>> not True
False
>>> not False
True
```

### **:**

```
>>> True and False    # Short-circuited at first argument.
False
>>> False and True    # Second argument is evaluated.
False
>>> True and True     # Second argument is evaluated.
True
```

### **:**

```
>>> True or False    # Short-circuited at first argument.
True
>>> False or True    # Second argument is evaluated.
True
>>> False or False   # Second argument is evaluated.
False
```

# Python 常量示例

三个常用的内置常数:

*   `True`:*布尔*类型的真值。对`True`的赋值产生一个*语法错误*。
*   `False`:*布尔*类型的假值。对`False`的赋值产生一个*语法错误*。
*   `None`:字体 *NoneType* 的唯一值。None 通常用于表示缺少值，例如当默认参数没有传递给函数时。对`None`的赋值产生一个*语法错误*。

其他内置常数:

*   `NotImplemented`:应由二进制特殊方法返回的特殊值，如`__eg__()`、`__add__()`、`__rsub__()`等。)来指示该操作对于另一个类型没有实现。
*   `Ellipsis`:特殊值，通常与用户定义的容器数据类型的扩展切片语法结合使用。
*   `__debug__`:如果 Python 没有使用-o 选项启动，则为 True。

站点模块添加的常量。site 模块(在启动时自动导入，除非给出了-S 命令行选项)向内置名称空间添加了几个常量。它们对于交互式解释器外壳很有用，不应该在程序中使用。

这些对象在打印时会打印一条类似“使用 quit()或 Ctrl-D(即 EOF)退出”的消息，并且在被调用时会使用指定的退出代码引发 SystemExit:

*   退出(代码=无)
*   退出(代码=无)

这些对象在打印时会打印一条类似“键入许可证()以查看完整的许可证文本”的消息，并在被调用时以类似寻呼机的方式显示相应的文本(一次显示一个屏幕):

*   版权
*   许可证
*   信用

# 调用 Python 函数示例

函数定义语句不执行函数。执行(调用)一个函数是通过使用该函数的名称，后跟括起所需参数(如果有的话)的括号来完成的。

```
>>> def say_hello():
...     print('Hello')
...
>>> say_hello()
Hello
```

函数的执行引入了一个新的符号表，用于函数的局部变量。更准确地说，函数中的所有变量赋值都将值存储在局部符号表中。

另一方面，变量引用首先在局部符号表中查找，然后在封闭函数的局部符号表中查找，然后在全局符号表中查找，最后在内置名称表中查找。因此，全局变量不能在函数中直接赋值(除非在全局语句中命名)，尽管它们可能被引用。

```
>>> a = 1
>>> b = 10
>>> def fn():
...     print(a)    # local a is not assigned, no enclosing function, global a referenced.
...     b = 20      # local b is assigned in the local symbol table for the function.
...     print(b)    # local b is referenced.
...
>>> fn()
1
20
>>> b               # global b is not changed by the function call.
10
```

函数调用的实际参数(自变量)在被调用函数被调用时被引入到被调用函数的局部符号表中。通过这种方式，使用 call by value 传递参数(其中值始终是对象引用，而不是对象的值)。当一个函数调用另一个函数时，会为该调用创建一个新的局部符号表。

```
>>> def greet(s):
...     s = "Hello " + s    # s in local symbol table is reassigned.
...     print(s)
...
>>> person = "Bob"
>>> greet(person)
Hello Bob
>>> person                  # person used to call remains bound to original object, 'Bob'.
'Bob'
```

用于调用函数的参数不能被函数重新分配，但是引用可变对象的参数的值可以改变:

```
>>> def fn(arg):
...     arg.append(1)
...
>>> a = [1, 2, 3]
>>> fn(a)
>>> a
[1, 2, 3, 1]
```

# Python 类示例

类提供了一种将数据和功能捆绑在一起的方法。创建一个新类会创建一个新类型的对象，允许创建该类型的新实例。每个类实例都可以附加属性来维护其状态。类实例也可以有修改其状态的方法(由其类定义)。

与其他编程语言相比，Python 的类机制以最少的新语法和语义添加了类。它混合了 C++中的类机制。

Python 类提供了面向对象编程的所有标准特性:类继承机制允许多个基类，派生类可以覆盖其基类的任何方法，方法可以调用同名基类的方法。

对象可以包含任意数量和种类的数据。与模块一样，类也具有 Python 的动态特性:它们是在运行时创建的，创建后可以进一步修改。

#### **类定义语法:**

最简单的类定义形式如下:

```
class ClassName:
    <statement-1>
        ...
        ...
        ...
    <statement-N>
```

#### **类对象:**

类对象支持两种操作:属性引用和实例化。

属性引用使用 Python 中用于所有属性引用的标准语法:`obj.name`。有效的属性名是创建类对象时类的命名空间中的所有名称。所以，如果类的定义是这样的:

```
class MyClass:
    """ A simple example class """
    i = 12345

    def f(self):
        return 'hello world'
```

那么`MyClass.i`和`MyClass.f`就是有效的属性引用，分别返回一个整数和一个函数对象。类属性也可以被赋值，所以你可以通过赋值来改变`MyClass.i`的值。`__doc__`也是一个有效属性，返回属于类:`"A simple example class"`的 docstring。

类实例化使用函数符号。假设类对象是一个无参数的函数，它返回该类的一个新实例。例如(假设上面的类):

```
x = MyClass()
```

创建类的新实例，并将此对象赋给局部变量 x。

实例化操作(“调用”一个类对象)创建一个空对象。许多类喜欢用定制到特定初始状态的实例来创建对象。因此一个类可以定义一个名为 ****init**** ()的特殊方法，如下所示:

```
def __init__(self):
    self.data = []
```

当一个类定义了一个`__init__()`方法时，类实例化自动为新创建的类实例调用`__init__()`。因此，在本例中，可以通过以下方式获得一个新的初始化实例:

```
x = MyClass()
```

当然，`__init__()`方法可能有更大灵活性的理由。在这种情况下，给予类实例化操作符的参数被传递给`__init__()`。举个例子，

```
class Complex:
    def __init__(self, realpart, imagpart):
        self.r = realpart
        self.i = imagpart
              ...

x = Complex(3.0, -4.5)
>>> x.r, x.i
(3.0, -4.5)
```

# Python 代码块和缩进示例

在 Python 中编码时，不要混合使用制表符和空格，这通常是一个好习惯。这样做可能会导致`TabError`，你的程序将会崩溃。编写代码时保持一致——选择使用制表符或空格缩进，并在整个程序中遵循您选择的约定。

#### **代码块和缩进**

Python 最与众不同的特性之一是使用缩进来标记代码块。考虑我们简单的密码检查程序中的 if 语句:

```
if pwd == 'apple':
    print('Logging on ...')
else:
    print('Incorrect password.')

print('All done!')
```

这几行打印('登录…')和打印('密码不正确。')是两个独立的代码块。这些恰好只有一行，但是 Python 允许您编写由任意数量的语句组成的代码块。

要在 Python 中表示一个代码块，必须将代码块的每一行缩进相同的量。我们的示例 if-statement 中的两个代码块都缩进了四个空格，这是 Python 的典型缩进量。

在大多数其他编程语言中，缩进只是用来帮助代码看起来漂亮。但是在 Python 中，它是指示一个语句属于哪个代码块所必需的。例如，最终的印刷(“全部完成！”)不是缩进的，因此不是 else 块的一部分。

熟悉其他语言的程序员常常对缩进很重要的想法感到恼火:许多程序员喜欢随心所欲地格式化他们的代码。然而，Python 缩进规则非常简单，大多数程序员已经使用缩进来使他们的代码可读。Python 只是将这一思想向前推进了一步，赋予了缩进意义。

#### **If/elif-语句**

if/elif 语句是具有多个条件的广义 if 语句。它被用来做复杂的决定。例如，假设一家航空公司有以下“儿童”票价:2 岁或 2 岁以下的儿童免费乘坐飞机，2 岁以上但 13 岁以下的儿童支付折扣儿童票价，13 岁以上的人支付普通成人票价。以下程序决定了乘客应支付的费用:

```
# airfare.py
age = int(input('How old are you? '))
if age <= 2:
    print(' free')
elif 2 < age < 13:
    print(' child fare)
else:
    print('adult fare')
```

Python 从用户那里获得年龄后，它进入 if/elif 语句，并按照给出的顺序逐个检查每个条件。

因此，它首先检查年龄是否小于 2，如果是，则表明飞行是自由的，并跳出 elif-条件。如果年龄不小于 2，则它检查下一个 elif 条件，以查看年龄是否在 2 和 13 之间。如果是这样，它打印适当的消息并跳出 if/elif 语句。如果 If 条件和 elif 条件都不为真，那么它执行 else 块中的代码。

#### **条件表达式**

Python 多了一个逻辑操作符，有些程序员喜欢(有些不喜欢！).它本质上是可以直接在表达式中使用的 if 语句的简写符号。考虑以下代码:

```
food = input("What's your favorite food? ")
reply = 'yuck' if food == 'lamb' else 'yum'
```

第二行中=右侧的表达式称为条件表达式，其计算结果为“yuck”或“yum”。它相当于以下内容:

```
food = input("What's your favorite food? ")
if food == 'lamb':
   reply = 'yuck'
else:
   reply = 'yum'
```

条件表达式通常比相应的 if/else 语句要短，尽管没有那么灵活或易读。一般来说，当它们使您的代码更简单时，您应该使用它们。

## Python 比较运算符示例

Python 中有八种比较操作。它们都具有相同的优先级(高于布尔运算的优先级)。比较可以任意链接；例如，`x < y <= z`等价于`x < y and y <= z`，除了`y`只被求值一次(但是在两种情况下当`x < y`被发现为假时`z`根本不被求值)。

下面总结了比较操作:

| 操作 | 意义 |
| --- | --- |
| `<` | 严格小于 |
| `<=` | 小于或等于 |
| `>` | 严格大于 |
| `>=` | 大于或等于 |
| `==` | 等于 |
| `!=` | 不等于 |
| `is` | 对象身份 |
| `is not` | 否定的对象标识 |

不同类型的对象，除了不同的数字类型，从来不会相等。此外，一些类型(例如，函数对象)只支持比较的退化概念，即任何两个该类型的对象都不相等。`<`、`<=`、`>`和`>=`运算符在将一个复数与另一个内置的数值类型进行比较时，当对象属于不同的类型而无法进行比较时，或者在没有定义排序的其他情况下，会引发`TypeError`异常。

除非类定义了`__eq__()`方法，否则一个类的不同实例通常被认为是不相等的。

一个类的实例不能相对于同一个类的其他实例或其他类型的对象进行排序，除非该类定义了足够多的方法`__lt__()`、`__le__()`、`__gt__()`和`__ge__()`(通常，`__lt__()`和`__eq__()`就足够了，如果你想要比较操作符的常规含义的话)。

不能自定义`is`和`is not`运算符的行为；此外，它们可以应用于任何两个对象，永远不会引发异常。

我们也可以将`<`和`>`操作符链接在一起。例如，`3 < 4 < 5`会返回`True`，而`3 < 4 > 5`不会。我们还可以链接等式运算符。例如，`3 == 3 < 5`将返回`True`，而`3 == 5 < 5`不会。

### **相等比较-"是" vs "=="**

在 Python 中，有两个比较运算符，允许我们检查两个对象是否相等。`is`操作员和`==`操作员。但是，它们之间有一个关键的区别！

“is”和“==”之间的主要区别可以总结为:

*   `is`用来比较 ****身份****
*   `==`用来比较 ****的平等****

## **例子**

首先，用 Python 创建一个列表。

```
myListA = [1,2,3]
```

接下来，创建该列表的副本。

```
myListB = myListA
```

如果我们使用' == '操作符或' is '操作符，两者都会产生一个 ****真**** 输出。

```
>>> myListA == myListB # both lists contains similar elements
True
>>> myListB is myListA # myListB contains the same elements
True
```

这是因为 myListA 和 myListB 都指向同一个列表变量，这是我在 Python 程序开始时定义的。两份名单完全一样，无论是身份还是内容。

但是，如果我现在创建一个新列表呢？

```
myListC = [1,2,3]
```

执行`==`操作符仍然显示两个列表在内容上是相同的。

```
>>> myListA == myListC
True
```

然而，执行`is`操作符现在将产生一个`False`输出。这是因为 myListA 和 myListC 是两个不同的变量，尽管包含相同的数据。即使他们看起来一样，他们是**T3**T5【不同】。

```
>>> myListA is myListC
False # both lists have different reference
```

总而言之:

*   如果两个变量指向同一个引用，一个`is`表达式输出`True`
*   如果两个变量包含相同的数据，一个`==`表达式输出`True`

# Python 字典示例

python 中的字典(又名“dict”)是一种内置的数据类型，可用于存储 ****`key-value`**** 对。这允许你把一个 ****`dict`**** 当作一个*数据库*来存储和组织数据。

字典的特别之处在于它们的实现方式。类似哈希表的结构使得检查存在性变得容易——这意味着我们可以容易地确定特定的键是否存在于字典中，而不需要检查每个元素。Python 解释器可以直接找到 location 键并检查该键是否在那里。

字典可以使用几乎任何任意的数据类型作为关键字，比如字符串、整数等等。然而，不可散列的值，即包含列表、字典或其他可变类型的值(通过值而不是通过对象标识进行比较)不能用作键。用于键的数字类型遵循数字比较的常规规则:如果两个数字比较相等(例如`1`和`1.0`)，那么它们可以互换使用来索引同一个字典条目。(但是请注意，由于计算机将浮点数存储为近似值，因此将它们用作字典键通常是不明智的。)

字典的一个最重要的要求是键 ****必须**** 是唯一的。

要创建一个空字典，只需使用一对大括号:

```
 >>> teams = {}
    >>> type(teams)
    >>> <class 'dict'>
```

要创建包含一些初始值的非空字典，请放置一个以逗号分隔的键值对列表:

```
 >>> teams = {'barcelona': 1875, 'chelsea': 1910}
    >>> teams
    {'barcelona': 1875, 'chelsea': 1910}
```

向现有字典添加键值对很容易:

```
 >>> teams['santos'] = 1787
    >>> teams
    {'chelsea': 1910, 'barcelona': 1875, 'santos': 1787} # Notice the order - Dictionaries are unordered !
    >>> # extracting value - Just provide the key
    ...
    >>> teams['barcelona']
    1875
```

****`del`**** 操作符用于从字典中删除一个键值对。在已经使用的键再次用于存储值的情况下，与该键关联的旧值会完全丢失。此外，请记住，使用不存在的键提取值是错误的。

```
 >>> del teams['santos']
    >>> teams
    {'chelsea': 1910, 'barcelona': 1875}
    >>> teams['chelsea'] = 2017 # overwriting    
    >>> teams
    {'chelsea': 2017, 'barcelona': 1875}
```

****`in`**** 关键字可以用来检查关键字是否存在于字典中:

```
 >>> 'sanots' in teams
    False    
    >>> 'barcelona' in teams
    True
    >>> 'chelsea' not in teams
    False
```

****`keys`**** 是一个内置的*方法*，可以用来获取给定字典的键。要将字典中的关键字提取为列表:

```
 >>> club_names = list(teams.keys())    
    >>> club_names
    ['chelsea', 'barcelona']
```

还有另一种创建字典的方法是使用 ****`dict()`**** 的方法:

```
 >>> players = dict( [('messi','argentina'), ('ronaldo','portugal'), ('kaka','brazil')] ) # sequence of key-value pair is passed  
    >>> players
    {'ronaldo': 'portugal', 'kaka': 'brazil', 'messi': 'argentina'}
    >>> 
    >>> # If keys are simple strings, it's quite easier to specify pairs using keyword arguments
    ...
    >>> dict( totti = 38, zidane = 43 )
    {'zidane': 43, 'totti': 38}
```

Dict comprehensions 也可以用于从任意键和值表达式创建字典:

```
 >>> {x: x**2 for x in (2, 4, 6)}
    {2: 4, 4: 16, 6: 36}
```

****在字典中循环****
简单地循环字典中的键，而不是键和值:

```
 >>> d = {'x': 1, 'y': 2, 'z': 3} 
    >>> for key in d:
    ...     print(key) # do something
    ...
    x
    y
    z
```

要循环访问键和值，可以使用下面的:
For Python 2.x:

```
 >>> for key, item in d.iteritems():
    ...     print items
    ...
    1
    2
    3
```

对于 Python 3.x 使用 ****`items()`**** :

```
 >>> for key, item in d.items():
    ...     print(key, items)
    ...
    x 1
    y 2
    z 3
```

# Python 对象示例

在 Python 中，一切都是一个*对象*。

*对象*代表属性的逻辑分组。属性是数据和/或函数。当在 Python 中创建一个对象时，它是用一个*身份*、*类型*和*值*创建的。

在其他语言中，*原语*是没有*属性*(属性)的*值*。比如 javascript 中的`undefined`、`null`、`boolean`、`string`、`number`、`symbol`(ECMAScript 2015 新增)都是原语。

在 Python 中，没有原语。`None`、*布尔*、*字符串*、*数字*，甚至*函数*无论如何创建，都是*对象*。

我们可以使用一些内置函数来演示这一点:

*   [T2`id`](https://docs.python.org/3/library/functions.html#id)
*   [T2`type`](https://docs.python.org/3/library/functions.html#type)
*   [T2`dir`](https://docs.python.org/3/library/functions.html#dir)
*   [T2`issubclass`](https://docs.python.org/3/library/functions.html#issubclass)

内置常数`None`、`True`、`False`是*对象*:

我们在这里测试`None`对象。

```
>>> id(None)
4550218168
>>> type(None)
<class 'NoneType'>
>>> dir(None)
[__bool__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
>>> issubclass(type(None), object)
True
```

接下来，我们来考察一下`True`。

```
>>> id(True)
4550117616
>>> type(True)
<class 'bool'>
>>> dir(True)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
>>> issubclass(type(True), object)
True
```

没有理由漏掉`False`！

```
>>> id(False)
4550117584
>>> type(False)
<class 'bool'>
>>> dir(False)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
>>> issubclass(type(False), object)
True
```

*字符串*，即使是由字符串文字创建的，也是*对象*。

```
>>> id("Hello campers!")
4570186864
>>> type('Hello campers!')
<class 'str'>
>>> dir("Hello campers!")
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
>>> issubclass(type('Hello campers!'), object)
True
```

与*号相同。*

```
>>> id(42)
4550495728
>>> type(42)
<class 'int'>
>>> dir(42)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
>>> issubclass(type(42), object)
True
```

## **函数也是对象**

在 Python 中，函数是第一类对象。

Python 中的*函数*也是*对象*，用*身份*、*类型*和*值*创建。它们也可以传递给其他*函数*:

```
>>> id(dir)
4568035688
>>> type(dir)
<class 'builtin_function_or_method'>
>>> dir(dir)
['__call__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__name__', '__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__self__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__text_signature__']
>>> issubclass(type(dir), object)
True
```

也可以将函数绑定到一个名称，并使用该名称调用绑定的函数:

```
>>> a = dir
>>> print(a)
<built-in function dir>
>>> a(a)
['__call__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__name__', '__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__self__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__text_signature__']
```

## 名称绑定和别名功能

在 Python 中定义函数时，该函数名会被引入到当前符号表中。然后，函数名的值的类型被解释器识别为用户定义的函数:

```
>>> something = 1
>>> type(something)
<type 'int'>

>>> def something():
...     pass
...
>>> type(something)
<type 'function'>

>>> something = []
>>> type(something)
<type 'list'>
```

然后可以将函数值赋给另一个名称。在它被重新分配后，它仍然可以作为一个函数使用。使用此方法重命名您的函数:

```
>>> def something(n):
...     print(n)
...
>>> type(something)
<type 'function'>

>>> s = something
>>> s(100)
100
```

# Python 元组

元组是 Python 对象的序列。元组是不可变的，这意味着它们在创建后不能被修改，这与列表不同。

****创作:****

使用一对圆括号`()`创建一个空的`tuple`:

```
 >>> empty_tuple = ()
    >>> print(empty_tuple)
    ()
    >>> type(empty_tuple)
    <class 'tuple'>
    >>> len(empty_tuple)
    0
```

通过用逗号分隔元素来创建带有元素的`tuple`(圆括号`()`是可选的，但有例外):

```
 >>> tuple_1 = 1, 2, 3       # Create tuple without round brackets.
    >>> print(tuple_1)
    (1, 2, 3)
    >>> type(tuple_1)
    <class 'tuple'>
    >>> len(tuple_1)
    3
    >>> tuple_2 = (1, 2, 3)     # Create tuple with round brackets.
    >>> print(tuple_2)
    (1, 2, 3)
    >>> tuple_3 = 1, 2, 3,      # Trailing comma is optional.
    >>> print(tuple_3)
    (1, 2, 3)
    >>> tuple_4 = (1, 2, 3,)    # Trailing comma in round brackets is also optional.
    >>> print(tuple_4)
    (1, 2, 3)
```

具有单个元素的`tuple`必须有尾随逗号(有或没有圆括号):

```
>>> not_tuple = (2)    # No trailing comma makes this not a tuple.
>>> print(not_tuple)
2
>>> type(not_tuple)
<class 'int'>
>>> a_tuple = (2,)     # Single element tuple. Requires trailing comma.
>>> print(a_tuple)
(2,)
>>> type(a_tuple)
<class 'tuple'>
>>> len(a_tuple)
1
>>> also_tuple = 2,    # Round brackets omitted. Requires trailing comma.
>>> print(also_tuple)
(2,)
>>> type(also_tuple)
<class 'tuple'>
```

在不明确的情况下需要圆括号(如果元组是更大表达式的一部分):

注意，实际上是逗号构成了元组，而不是括号。括号是可选的，除了空元组的情况，或者当需要它们来避免语法歧义时。

例如，`f(a, b, c)`是一个带有三个参数的函数调用，而`f((a, b, c))`是一个带有一个 3 元组作为唯一参数的函数调用。

```
 >>> print(1,2,3,4,)          # Calls print with 4 arguments: 1, 2, 3, and 4
    1 2 3 4
    >>> print((1,2,3,4,))        # Calls print with 1 argument: (1, 2, 3, 4,)
    (1, 2, 3, 4)
    >>> 1, 2, 3 == (1, 2, 3)     # Equivalent to 1, 2, (3 == (1, 2, 3))
    (1, 2, False)
    >>> (1, 2, 3) == (1, 2, 3)   # Use surrounding round brackets when ambiguous.
    True
```

也可以用`tuple`构造函数创建一个`tuple`:

```
 >>> empty_tuple = tuple()
    >>> print(empty_tuple)
    ()
    >>> tuple_from_list = tuple([1,2,3,4])
    >>> print(tuple_from_list)
    (1, 2, 3, 4)
    >>> tuple_from_string = tuple("Hello campers!")
    >>> print(tuple_from_string)
    ('H', 'e', 'l', 'l', 'o', ' ', 'c', 'a', 'm', 'p', 'e', 'r', 's', '!')
    >>> a_tuple = 1, 2, 3
    >>> b_tuple = tuple(a_tuple)    # If the constructor is called with a tuple for
    the iterable,
    >>> a_tuple is b_tuple          # the tuple argument is returned.
    True
```

****访问元素一`tuple` :****

`tuples`的元素的访问和索引方式与`lists`相同。

```
>>> my_tuple = 1, 2, 9, 16, 25
>>> print(my_tuple)
(1, 2, 9, 16, 25)
```

*零索引*

```
 >>> my_tuple[0]
    1
    >>> my_tuple[1]
    2
    >>> my_tuple[2]
    9
```

*环绕分度*

```
 >>> my_tuple[-1]
    25
    >>> my_tuple[-2]
    16
```

****装箱拆箱:****

语句`t = 12345, 54321, 'hello!'`是元组打包的一个例子:值`12345`、`54321`和`'hello!'`一起打包在一个元组中。相反的操作也是可能的:

```
 >>> x, y, z = t
```

这被恰当地称为序列解包，适用于右侧的任何序列。序列解包要求等号左边的变量和序列中的元素一样多。注意，多重赋值实际上只是元组打包和序列解包的组合。

```
 >>> t = 1, 2, 3    # Tuple packing.
    >>> print(t)
    (1, 2, 3)
    >>> a, b, c = t    # Sequence unpacking.
    >>> print(a)
    1
    >>> print(b)
    2
    >>> print(c)
    3
    >>> d, e, f = 4, 5, 6    # Multiple assignment combines packing and unpacking.
    >>> print(d)
    4
    >>> print(e)
    5
    >>> print(f)
    6
    >>> a, b = 1, 2, 3       # Multiple assignment requires each variable (right)
    have a matching element (left).
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ValueError: too many values to unpack (expected 2)
```

****不可变:****

`tuples`是不可变的容器，保证它们包含的**对象不会改变。它不 ****而**** 保证它们包含的对象不会改变:**

```
 `>>> a_list = []
    >>> a_tuple = (a_list,)    # A tuple (immutable) with a list (mutable) element.
    >>> print(a_tuple)
    ([],)

    >>> a_list.append("Hello campers!")
    >>> print(a_tuple)         # Element of the immutable is mutated.
    (['Hello campers!'],)`
```

******用途:******

**函数只能返回一个值，然而，一个异构的`tuple`可以用来从一个函数返回多个值。一个例子是内置的`enumerate`函数，它返回一个异构`tuples`的 iterable:**

```
 `>>> greeting = ["Hello", "campers!"]
    >>> enumerator = enumerate(greeting)
    >>> enumerator.next()
    >>> enumerator.__next__()
    (0, 'Hello')
    >>> enumerator.__next__()
    (1, 'campers!')`
```

# **Python For 循环语句示例**

**Python 利用 for 循环来迭代元素列表。这与 C 或 Java 不同，C 或 Java 使用 for 循环逐步更改值，并使用该值访问数组之类的东西。**

**For 循环遍历基于集合的数据结构，如列表、元组和字典。**

**基本语法是:**

```
`for value in list_of_values:
  # use value inside this block`
```

**一般来说，您可以使用任何东西作为迭代器值，其中 iterable 的条目可以被赋值给。例如，您可以从元组列表中解包元组:**

```
`list_of_tuples = [(1,2), (3,4)]

for a, b in list_of_tuples:
  print("a:", a, "b:", b)`
```

**另一方面，你可以循环任何可迭代的东西。您可以调用函数或使用列表文字。**

```
`for person in load_persons():
  print("The name is:", person.name)`
```

```
`for character in ["P", "y", "t", "h", "o", "n"]:
  print("Give me a '{}'!".format(character))`
```

**使用 For 循环的一些方式:**

******迭代范围()函数******

```
`for i in range(10):
    print(i)`
```

**range 不是一个函数，它实际上是一个不可变的序列类型。输出将包含从下限(即 0)到上限(即 10)的结果，但不包括 10。默认情况下，下限或起始索引设置为零。输出:**

```
`>
0
1
2
3
4
5
6
7
8
9
>`
```

**此外，可以通过添加第二个和第三个参数来指定序列的下限，甚至序列的步长。**

```
`for i in range(4,10,2): #From 4 to 9 using a step of two
    print(i)`
```

**输出:**

```
`>
4
6
8
>`
```

******xrange()函数******

**在很大程度上，xrange 和 range 在功能上是完全相同的。它们都提供了一种生成整数列表的方法，供您随意使用。唯一的区别是 range 返回一个 Python 列表对象，而 xrange 返回一个 xrange 对象。这意味着 xrange 并不像 range 那样在运行时生成静态列表。它用一种叫做让步的特殊技术创造出你需要的价值。这种技术用于一种称为发生器的对象。**

**再补充一点。在 Python 3.x 中，xrange 函数不再存在。range 函数现在做 Python 2.x 中 xrange 做的事情**

******迭代列表或元组中的值******

```
`A = ["hello", 1, 65, "thank you", [2, 3]]
for value in A:
    print(value)`
```

**输出:**

```
`>
hello
1
65
thank you
[2, 3]
>`
```

```
**`fruits_to_colors = {"apple": "#ff0000",
                    "lemon": "#ffff00",
                    "orange": "#ffa500"}

for key in fruits_to_colors:
    print(key, fruits_to_colors[key])`**
```

****输出:****

```
**`>
apple #ff0000
lemon #ffff00
orange #ffa500
>`**
```

********用 zip()函数**** 在一个循环中迭代两个相同大小的列表****

```
`A = ["a", "b", "c"]
B = ["a", "d", "e"]

for a, b in zip(A, B):
  print a, b, a == b` 
```

**输出:**

```
`>
a a True
b d False
c e False
>`
```

******遍历一个列表，用 enumerate()函数**** 得到对应的索引**

```
`A = ["this", "is", "something", "fun"]

for index,word in enumerate(A):
    print(index, word)`
```

**输出:**

```
`>
0 this
1 is
2 something
3 fun
>`
```

**一个常见的用例是迭代字典:**

```
`for name, phonenumber in contacts.items():
  print(name, "is reachable under", phonenumber)`
```

**如果您绝对需要访问您的迭代的当前索引，请使用 ****而不是****`range(len(iterable))`！这是一种极其糟糕的做法，会让你遭到资深 Python 开发者的嘲笑。请改用内置函数`enumerate()`:**

```
`for index, item in enumerate(shopping_basket):
  print("Item", index, "is a", item)`
```

******for/else 语句**** Pyhton 允许在 for 循环中使用 else，当循环体中的所有条件都不满足时，就会执行 else 情况。要使用 else，我们必须使用`break`语句，这样我们就可以在满足条件的情况下跳出循环。如果我们不越狱，那么 else 部分将被执行。**

```
`week_days = ['Monday','Tuesday','Wednesday','Thursday','Friday']
today = 'Saturday'
for day in week_days:
  if day == today:
    print('today is a week day')
    break
else:
  print('today is not a week day')`
```

**在上面的例子中，输出将是`today is not a week day`,因为循环中的 break 永远不会被执行。**

******使用内联循环函数**** 遍历列表**

**我们也可以使用 python 进行内联迭代。例如，如果我们需要将列表中的所有单词大写，我们可以简单地执行以下操作:**

```
`A = ["this", "is", "awesome", "shinning", "star"]

UPPERCASE = [word.upper() for word in A]
print (UPPERCASE)`
```

**输出:**

```
`>
['THIS', 'IS', 'AWESOME', 'SHINNING', 'STAR']`
```

# **Python 函数示例**

**函数允许你定义一个可重用的代码块，它可以在你的程序中多次执行。**

**函数允许你为复杂的问题创建更多的模块和[干](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)的解决方案。**

**虽然 Python 已经提供了许多内置函数，比如`print()`和`len()`，但是您也可以定义自己的函数在项目中使用。**

**在代码中使用函数的一个很大的好处是，它减少了项目中代码的总行数。**

### ****语法****

**在 Python 中，函数定义具有以下特性:**

1.  **关键词`def`**
2.  **函数名**
3.  **括号'()'，并在括号内输入参数，尽管输入参数是可选的。**
4.  **冒号':'**
5.  **一些要执行的代码块**
6.  **返回语句(可选)**

```
`# a function with no parameters or returned values
def sayHello():
  print("Hello!")

sayHello()  # calls the function, 'Hello!' is printed to the console

# a function with a parameter
def helloWithName(name):
  print("Hello " + name + "!")

helloWithName("Ada")  # calls the function, 'Hello Ada!' is printed to the console

# a function with multiple parameters with a return statement
def multiply(val1, val2):
  return val1 * val2

multiply(3, 5)  # prints 15 to the console`
```

**函数是代码块，只需调用函数就可以重用。这使得简单、优雅的代码重用成为可能，而无需显式地重写代码部分。这使得代码更具可读性，更易于调试，并减少了键入错误。**

**Python 中的函数是使用`def`关键字创建的，后跟括号内的函数名和函数参数。**

**函数总是返回值。函数使用关键字`return`来返回值。如果不想返回任何值，就返回默认值`None`。**

**函数名用于调用函数，在括号内传递所需的参数。**

```
`# this is a basic sum function
def sum(a, b):
  return a + b

result = sum(1, 2)
# result = 3`
```

**您可以为参数定义默认值，如果没有给定，Python 会将该参数的值解释为默认值。**

```
`def sum(a, b=3):
  return a + b

result = sum(1)
# result = 4`
```

**您可以使用参数的名称，按照您想要的顺序传递参数。**

```
`result = sum(b=2, a=2)
# result = 4`
```

**但是，不可能在非关键字参数之前传递关键字参数。**

```
`result = sum(3, b=2)
#result = 5
result2 = sum(b=2, 3)
#Will raise SyntaxError`
```

**函数也是对象，所以你可以把它们赋给一个变量，然后像函数一样使用这个变量。**

```
`s = sum
result = s(1, 2)
# result = 3`
```

### ****注释****

**如果函数定义包含参数，则在调用函数时必须提供相同数量的参数。**

```
`print(multiply(3))  # TypeError: multiply() takes exactly 2 arguments (0 given)

print(multiply('a', 5))  # 'aaaaa' printed to the console

print(multiply('a', 'b'))  # TypeError: Python can't multiply two strings`
```

**函数将运行的代码块包括函数内缩进的所有语句。**

```
`def myFunc():
print('this will print')
print('so will this')

x = 7
# the assignment of x is not a part of the function since it is not indented`
```

**函数中定义的变量只存在于该函数的范围内。**

```
`def double(num):
x = num * 2
return x

print(x)  # error - x is not defined
print(double(4))  # prints 8`
```

**Python 只在调用函数时解释函数块，而不是在定义函数时解释。因此，即使函数定义块包含某种错误，python 解释器也只会在调用函数时指出来。**

# **Python 生成器示例**

**生成器是一种特殊类型的函数，允许您在不结束函数的情况下返回值。它通过使用`yield`关键字来实现这一点。类似于`return`,`yield`表达式将返回一个值给调用者。两者的关键区别在于`yield`将暂停函数，允许将来返回更多的值。**

**生成器是可迭代的，因此它们可以干净地用于 for 循环或任何迭代。**

```
`def my_generator():
    yield 'hello'
    yield 'world'
    yield '!'

for item in my_generator():
    print(item)

# output:
# hello
# world
# !`
```

**像其他迭代器一样，生成器可以传递给`next`函数来检索下一项。当生成器没有更多的值要生成时，就会产生一个`StopIteration`错误。**

```
`g = my_generator()
print(next(g))
# 'hello'
print(next(g))
# 'world'
print(next(g))
# '!'
print(next(g))
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# StopIteration`
```

**当您需要创建大量的值，但不需要同时将它们保存在内存中时，生成器特别有用。例如，如果您需要打印前一百万个斐波那契数，您通常会返回一个包含一百万个值的列表，并迭代该列表以打印每个值。然而，使用生成器，您可以一次返回一个值:**

```
`def fib(n):
    a = 1
    b = 1
    for i in range(n):
        yield a
        a, b = b, a + b

for x in fib(1000000):
    print(x)`
```

# **Python 迭代器示例**

**Python 支持容器迭代的概念。这是使用两种不同的方法实现的；这些用于允许用户定义的类支持迭代。**

**[Python 文档-迭代器类型](https://docs.python.org/3/library/stdtypes.html#iterator-types)**

**迭代是以编程方式将一个步骤重复给定次数的过程。程序员可以利用迭代对数据集合中的每一项执行相同的操作，例如打印出列表中的每一项。**

*   **对象可以实现一个返回迭代器对象的`__iter__()`方法来支持迭代。**

**迭代器对象必须实现:**

*   **`__iter__()`:返回迭代器对象。**
*   **`__next__()`:返回容器的下一个对象，迭代器 *object = 'abc '。 ****iter**** () print(迭代器*对象)print(id(迭代器*对象))print(id(迭代器*对象。 ****iter**** ())) #返回迭代器本身。打印(迭代器*对象。 ****下一个**** ()) #返回第一个对象并推进迭代器。打印(迭代器*对象。 ****下一个**** ()) #返回第二个对象并推进迭代器。打印(迭代器*对象。 ****下一个**** ()) #返回第三个对象并推进迭代器。打印(迭代器*对象。 ****下一个**** ()) #引发 StopIteration 异常。**

**输出:**

```
`<str_iterator object at 0x102e196a0>
4343305888
4343305888
a
b
c
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-1-d466eea8c1b0> in <module>()
      6 print(iterator_object.__next__())     # Returns 2nd object and advances iterator.
      7 print(iterator_object.__next__())     # Returns 3rd object and advances iterator.
----> 8 print(iterator_object.__next__())     # Raises StopIteration Exception.

StopIteration:`
```

# ****Python 示例中的三元运算符****

**Python 中的三元运算，通常也称为条件表达式，允许程序员执行评估并根据给定条件的真值返回值。**

**三元运算符不同于标准的`if`、`else`、`elif`结构，因为它不是控制流结构，其行为更像其他运算符，如 Python 语言中的`==`或`!=`。**

### ****例子****

**在这个例子中，如果变量`val`是偶数，则返回字符串`Even`，否则返回字符串`Odd`。然后将返回的字符串赋给`is_even`变量并打印到控制台。**

#### ****输入****

```
`for val in range(1, 11):
    is_even = "Even" if val % 2 == 0 else "Odd"
    print(val, is_even, sep=' = ')`
```

#### ****输出****

```
`1 = Odd
2 = Even
3 = Odd
4 = Even
5 = Odd
6 = Even
7 = Odd
8 = Even
9 = Odd
10 = Even`
```

# **Python While 循环语句示例**

**Python 与其他流行语言一样利用了`while`循环。`while`循环评估一个条件，如果条件为真，则执行一段代码。代码块重复执行，直到条件变为假。**

**基本语法是:**

```
`counter = 0
while counter < 10:
   # Execute the block of code here as
   # long as counter is less than 10`
```

**下面显示了一个示例:**

```
`days = 0    
week = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
while days < 7:
   print("Today is " + week[days])
   days += 1`
```

**输出:**

```
`Today is Monday
Today is Tuesday
Today is Wednesday
Today is Thursday
Today is Friday
Today is Saturday
Today is Sunday`
```

**以上代码的逐行解释:**

1.  **变量“天数”被设置为值 0。**
2.  **变量 week 被分配给包含一周中所有日期的列表。**
3.  **当循环开始时**
4.  **代码块将被执行，直到条件返回“真”。**
5.  **条件是“天数< 7”，大致意思是运行 while 循环，直到变量天数小于 7**
6.  **因此，当 days=7 时，while 循环停止执行。**
7.  **days 变量在每次迭代中都会更新。**
8.  **当 while 循环第一次运行时，控制台上会显示一行“今天是星期一”,变量 days 等于 1。**
9.  **因为变量 days 等于小于 7 的 1，所以再次执行 while 循环。**
10.  **当控制台打印出“今天是星期天”时，变量 days 现在等于 7，while 循环停止执行。**

## **Python 中的 f 字符串**

**在 Python 版中，实现了一种格式化字符串的新方法。这种新方法被称为文字字符串插值(虽然通常被称为 f 字符串)。**

**f-string 的使用允许程序员以干净简洁的方式动态地将变量插入到字符串中。除了在字符串中插入变量之外，这个特性还为程序员提供了计算表达式、连接集合内容，甚至在 f 字符串中调用函数的能力。**

**为了在 f 字符串中执行这些动态行为，我们将它们放在字符串中的花括号内，并在字符串的开头加上小写的 f(在左引号之前)。**

### ****例题****

**在运行时将变量动态插入字符串:**

```
`name = 'Jon Snow'
greeting = f'Hello! {name}'
print(greeting)`
```

**计算字符串中的表达式:**

```
`val1 = 2
val2 = 3
expr = f'The sum of {val1} + {val2} is {val1 + val2}'
print(expr)`
```

**调用函数并在字符串中插入输出:**

```
`def sum(*args):
    result = 0
    for arg in args:
        result += arg
    return result

func = f'The sum of 3 + 5 is {sum(3, 5)}'
print(func)`
```

**将集合的内容连接到字符串中:**

```
`fruits = ['Apple', 'Banana', 'Pear']

list_str = f'List of fruits: {", ".join(fruits)}'
print(list_str)`
```

# **如何安装 Python 3**

**可以从这个官方[链接](https://www.python.org/downloads/)下载 Python。基于你的操作系统(Windows 或 Linux 或 OSX)，你可能想按照这些指令[安装 Python 3。](http://docs.python-guide.org/en/latest/starting/installation/)**

## ****使用虚拟环境****

**用沙箱保护你的 Python 安装并把它与你的 T2 系统 Python 分开总是一个好主意。*系统 Python* 是 Python 解释器的路径，随操作系统一起安装的其他模块会用到它。**

**直接用*系统 Python* 安装 Python Web-frames 或库**不安全。相反，在开发 Python 应用程序时，您可以使用 [Virtualenv](https://virtualenv.readthedocs.org/en/latest/) 来创建和派生一个单独的 Python 进程。****

### ****虚拟包装****

****[Virtualenvwrapper 模块](https://virtualenvwrapper.readthedocs.org/en/latest/)使您可以轻松地在一台机器上管理和运行多个 Python 沙盒环境，而不会破坏任何用 Python 编写并由您的机器使用的模块或服务。****

****当然，大多数云托管的开发环境，如 [Nitrous](https://www.nitrous.io/) 或 [Cloud9](https://c9.io/) 也预装了这些，并为您获取编码做好了准备！激活 Python 3 环境后，您可以从仪表板中快速选择一个框并开始编码。****

****在 [Cloud9](https://c9.io/) 中，创建新的开发环境时需要选择 Django 框。****

****下面是几个 shell 命令示例。如果您希望复制粘贴，请注意,`$`符号是终端提示符的简写，它不是命令的一部分。我的终端提示符看起来像这样:****

**
```

****并且，一个`ls`看起来像****

```
**`alayek:~/workspace (master) $ ls`**
```

****但是，在这个文档中写同样的内容时，我会把它写成****

```
**`$ ls`**
```

****回到我们的讨论，您可以通过在您的云终端上运行，在 Cloud9 上创建一个包含 Python 3 解释器的沙箱:****

```
**`$ mkvirtualenv py3 --python=/usr/bin/python3`**
```

****您只需在为项目创建新盒子后运行一次。一旦执行，这个命令将创建一个新的沙盒 virtualenv 供您使用，命名为`py3`。****

****要查看可用的虚拟环境，您可以使用****

```
**`$ workon`**
```

****要激活`py3`，您可以使用带有环境名称的`workon`命令:****

```
**`$ workon py3`**
```

****以上所有三个终端命令也可以在本地 Linux 机器或 OSX 机器上运行。这些是[虚拟包装器](https://virtualenvwrapper.readthedocs.org/en/latest/#introduction)命令；所以如果你打算使用它们，确保你已经安装了这个模块并添加到了`PATH`变量中。****

****如果你在虚拟环境中；通过检查您的终端提示符，您可以很容易地发现这一点。环境名称将清楚地显示在您的终端提示符中。****

****例如，当我在`py3`环境中时，我会看到这是我的终端提示符:****

```
**`(py3)alayek:~/workspace (master) # Python 示例–数据结构、算法、语法示例代码

> 原文：<https://www.freecodecamp.org/news/python-example/>

Python 是一种通用编程语言，它是动态类型化的、可解释的，并且以其简单的可读性和优秀的设计原则而闻名。

freeCodeCamp 有一个最受欢迎的 Python 课程。完全免费。你可以在 YouTube 上看这个视频。

![maxresdefault](img/78d520922041325377ae0c5fc97abff9.png)

# Python 数据结构示例

关于浮点数以及它们在 Python 中如何工作的一些一般信息，可以在这里找到。

几乎所有 Python 的实现都遵循 IEEE 754 规范:二进制浮点运算的标准。更多信息可在 IEEE 网站上找到。

可以使用[浮点文字](https://docs.python.org/3/reference/lexical_analysis.html#floating-point-literals)创建浮点对象:

```
>>> 3.14
3.14
>>> 314\.    # Trailing zero(s) not required.
314.0
>>> .314    # Leading zero(s) not required.
0.314
>>> 3e0
3.0
>>> 3E0     # 'e' or 'E' can be used.
3.0
>>> 3e1     # Positive value after e moves the decimal to the right.
30.0
>>> 3e-1    # Negative value after e moves the decimal to the left.
0.3
>>> 3.14e+2 # '+' not required but can be used for exponent part.
314.0
```

数字文字不包含符号，但是可以通过在文字前加上一元`-`(减)运算符(没有空格)来创建负浮点对象:

```
>>> -3.141592653589793
-3.141592653589793
>>> type(-3.141592653589793)
<class 'float'>
```

同样，正 float 对象可以以一元的`+`(加号)操作符为前缀，在字面量之前没有空格。通常`+`被省略:

```
>>> +3.141592653589793
3.141592653589793
```

请注意，前导零和尾随零对浮点文字有效。

同样，正 float 对象可以以一元的`+`(加号)操作符为前缀，在字面量之前没有空格。通常`+`被省略:

```
>>> +3.141592653589793
3.141592653589793
```

请注意，前导零和尾随零对浮点文字有效。

```
>>> 0.0
0.0
>>> 00.00
0.0
>>> 00100.00100
100.001
>>> 001e0010      # Same as 1e10
10000000000.0
```

[`float`构造函数](https://docs.python.org/3/library/functions.html#float)是创建`float`对象的另一种方式。

在可能的情况下，最好使用浮点文字创建`float`对象:

```
>>> a = 3.14         # Prefer floating point literal when possible.
>>> type(a)
<class 'float'>
>>> b = int(3.14)    # Works but unnecessary.
>>> type(b)
<class 'float'>
```

但是，float 构造函数允许从其他数字类型创建 float 对象:

```
>>> a = 4
>>> type(a)
<class 'int'>
>>> print(a)
4
>>> b = float(4)
>>> type(b)
<class 'float'>
>>> print(b)
4.0
>>> float(400000000000000000000000000000000)
4e+32
>>> float(.00000000000000000000000000000004)
4e-32
>>> float(True)
1.0
>>> float(False)
0.0
```

`float`构造函数还将从表示数字文字的字符串中生成`float`对象:

```
>>> float('1')
1.0
>>> float('.1')
0.1
>>> float('3.')
3.0
>>> float('1e-3')
0.001
>>> float('3.14')
3.14
>>> float('-.15e-2')
-0.0015
```

`float`构造函数也可以用来表示`NaN`(不是数字)、负数`infinity`和`infinity`(注意这些字符串不区分大小写):

```
>>> float('nan')
nan
>>> float('inf')
inf
>>> float('-inf')
-inf
>>> float('infinity')
inf
>>> float('-infinity')
-inf
```

### 复数

复数有一个实部和一个虚部，每个都用一个浮点数表示。

一个复数的虚部可以用一个虚文字来创建，这导致一个复数的实部为`0.0`:

```
>>> a = 3.5j
>>> type(a)
<class 'complex'>
>>> print(a)
3.5j
>>> a.real
0.0
>>> a.imag
3.5
```

不存在创建具有非零实部和虚部的复数的文字。要创建非零实部复数，请向浮点数添加一个虚数:

```
>>> a = 1.1 + 3.5j
>>> type(a)
<class 'complex'>
>>> print(a)
(1.1+3.5j)
>>> a.real
1.1
>>> a.imag
3.5
```

或者使用[复杂构造函数](https://docs.python.org/3/library/functions.html#complex)。

```
class complex([real[, imag]])
```

用于调用复杂构造函数的参数可以是数字(包括`complex`)类型，用于以下任一参数:

```
>>> complex(1, 1)
(1+1j)
>>> complex(1j, 1j)
(-1+1j)
>>> complex(1.1, 3.5)
(1.1+3.5j)
>>> complex(1.1)
(1.1+0j)
>>> complex(0, 3.5)
3.5j
```

A `string`也可以作为自变量。如果将字符串用作参数，则不允许有第二个参数

```
>>> complex("1.1+3.5j")
(1.1+3.5j)
```

# python 胸部示例

`bool()`是 Python 3 中的内置函数。该函数返回一个布尔值，即 True 或 False。它需要一个参数，`x`。

## **自变量**

它需要一个参数。使用标准的[真值测试程序](https://docs.python.org/3/library/stdtypes.html#truth)转换`x`。

## **返回值**

如果`x`为假或省略，则返回`False`；否则它返回`True`。

## **代码示例**

```
print(bool(4 > 2)) # Returns True as 4 is greater than 2
print(bool(4 < 2)) # Returns False as 4 is not less than 2
print(bool(4 == 4)) # Returns True as 4 is equal to 4
print(bool(4 != 4)) # Returns False as 4 is equal to 4 so inequality doesn't holds
print(bool(4)) # Returns True as 4 is a non-zero value
print(bool(-4)) # Returns True as -4 is a non-zero value
print(bool(0)) # Returns False as it is a zero value
print(bool('dskl')) # Returns True as the string is a non-zero value
print(bool([1, 2, 3])) # Returns True as the list is a non-zero value
print(bool((2,3,4))) # Returns True as tuple is a non-zero value
print(bool([])) # Returns False as list is empty and equal to 0 according to truth value testing
```

# Python 布尔运算符示例

[`and`](https://docs.python.org/3/reference/expressions.html#and)[`or`](https://docs.python.org/3/reference/expressions.html#or)[`not`](https://docs.python.org/3/reference/expressions.html#not)

[Python 文档-布尔运算](https://docs.python.org/3/library/stdtypes.html#boolean-operations-and-or-not)

这些是布尔运算，按优先级升序排列:

OperationResultNotes x 或 y 如果 x 为假，则 y，else x (1) x 和 y 如果 x 为假，则 x，else y (2) not x 如果 x 为假，则 True，else False (3)。

****备注:****

1.  这是一个短路操作符，所以如果第一个参数为假，它只计算第二个参数。
2.  这是一个短路操作符，所以只有当第一个参数为真时，它才计算第二个参数。
3.  not 的优先级比非布尔运算符低，所以 not a == b 解释为 not (a == b)，a == not b 是语法错误。

## **例子:**

### **:**

```
>>> not True
False
>>> not False
True
```

### **:**

```
>>> True and False    # Short-circuited at first argument.
False
>>> False and True    # Second argument is evaluated.
False
>>> True and True     # Second argument is evaluated.
True
```

### **:**

```
>>> True or False    # Short-circuited at first argument.
True
>>> False or True    # Second argument is evaluated.
True
>>> False or False   # Second argument is evaluated.
False
```

# Python 常量示例

三个常用的内置常数:

*   `True`:*布尔*类型的真值。对`True`的赋值产生一个*语法错误*。
*   `False`:*布尔*类型的假值。对`False`的赋值产生一个*语法错误*。
*   `None`:字体 *NoneType* 的唯一值。None 通常用于表示缺少值，例如当默认参数没有传递给函数时。对`None`的赋值产生一个*语法错误*。

其他内置常数:

*   `NotImplemented`:应由二进制特殊方法返回的特殊值，如`__eg__()`、`__add__()`、`__rsub__()`等。)来指示该操作对于另一个类型没有实现。
*   `Ellipsis`:特殊值，通常与用户定义的容器数据类型的扩展切片语法结合使用。
*   `__debug__`:如果 Python 没有使用-o 选项启动，则为 True。

站点模块添加的常量。site 模块(在启动时自动导入，除非给出了-S 命令行选项)向内置名称空间添加了几个常量。它们对于交互式解释器外壳很有用，不应该在程序中使用。

这些对象在打印时会打印一条类似“使用 quit()或 Ctrl-D(即 EOF)退出”的消息，并且在被调用时会使用指定的退出代码引发 SystemExit:

*   退出(代码=无)
*   退出(代码=无)

这些对象在打印时会打印一条类似“键入许可证()以查看完整的许可证文本”的消息，并在被调用时以类似寻呼机的方式显示相应的文本(一次显示一个屏幕):

*   版权
*   许可证
*   信用

# 调用 Python 函数示例

函数定义语句不执行函数。执行(调用)一个函数是通过使用该函数的名称，后跟括起所需参数(如果有的话)的括号来完成的。

```
>>> def say_hello():
...     print('Hello')
...
>>> say_hello()
Hello
```

函数的执行引入了一个新的符号表，用于函数的局部变量。更准确地说，函数中的所有变量赋值都将值存储在局部符号表中。

另一方面，变量引用首先在局部符号表中查找，然后在封闭函数的局部符号表中查找，然后在全局符号表中查找，最后在内置名称表中查找。因此，全局变量不能在函数中直接赋值(除非在全局语句中命名)，尽管它们可能被引用。

```
>>> a = 1
>>> b = 10
>>> def fn():
...     print(a)    # local a is not assigned, no enclosing function, global a referenced.
...     b = 20      # local b is assigned in the local symbol table for the function.
...     print(b)    # local b is referenced.
...
>>> fn()
1
20
>>> b               # global b is not changed by the function call.
10
```

函数调用的实际参数(自变量)在被调用函数被调用时被引入到被调用函数的局部符号表中。通过这种方式，使用 call by value 传递参数(其中值始终是对象引用，而不是对象的值)。当一个函数调用另一个函数时，会为该调用创建一个新的局部符号表。

```
>>> def greet(s):
...     s = "Hello " + s    # s in local symbol table is reassigned.
...     print(s)
...
>>> person = "Bob"
>>> greet(person)
Hello Bob
>>> person                  # person used to call remains bound to original object, 'Bob'.
'Bob'
```

用于调用函数的参数不能被函数重新分配，但是引用可变对象的参数的值可以改变:

```
>>> def fn(arg):
...     arg.append(1)
...
>>> a = [1, 2, 3]
>>> fn(a)
>>> a
[1, 2, 3, 1]
```

# Python 类示例

类提供了一种将数据和功能捆绑在一起的方法。创建一个新类会创建一个新类型的对象，允许创建该类型的新实例。每个类实例都可以附加属性来维护其状态。类实例也可以有修改其状态的方法(由其类定义)。

与其他编程语言相比，Python 的类机制以最少的新语法和语义添加了类。它混合了 C++中的类机制。

Python 类提供了面向对象编程的所有标准特性:类继承机制允许多个基类，派生类可以覆盖其基类的任何方法，方法可以调用同名基类的方法。

对象可以包含任意数量和种类的数据。与模块一样，类也具有 Python 的动态特性:它们是在运行时创建的，创建后可以进一步修改。

#### **类定义语法:**

最简单的类定义形式如下:

```
class ClassName:
    <statement-1>
        ...
        ...
        ...
    <statement-N>
```

#### **类对象:**

类对象支持两种操作:属性引用和实例化。

属性引用使用 Python 中用于所有属性引用的标准语法:`obj.name`。有效的属性名是创建类对象时类的命名空间中的所有名称。所以，如果类的定义是这样的:

```
class MyClass:
    """ A simple example class """
    i = 12345

    def f(self):
        return 'hello world'
```

那么`MyClass.i`和`MyClass.f`就是有效的属性引用，分别返回一个整数和一个函数对象。类属性也可以被赋值，所以你可以通过赋值来改变`MyClass.i`的值。`__doc__`也是一个有效属性，返回属于类:`"A simple example class"`的 docstring。

类实例化使用函数符号。假设类对象是一个无参数的函数，它返回该类的一个新实例。例如(假设上面的类):

```
x = MyClass()
```

创建类的新实例，并将此对象赋给局部变量 x。

实例化操作(“调用”一个类对象)创建一个空对象。许多类喜欢用定制到特定初始状态的实例来创建对象。因此一个类可以定义一个名为 ****init**** ()的特殊方法，如下所示:

```
def __init__(self):
    self.data = []
```

当一个类定义了一个`__init__()`方法时，类实例化自动为新创建的类实例调用`__init__()`。因此，在本例中，可以通过以下方式获得一个新的初始化实例:

```
x = MyClass()
```

当然，`__init__()`方法可能有更大灵活性的理由。在这种情况下，给予类实例化操作符的参数被传递给`__init__()`。举个例子，

```
class Complex:
    def __init__(self, realpart, imagpart):
        self.r = realpart
        self.i = imagpart
              ...

x = Complex(3.0, -4.5)
>>> x.r, x.i
(3.0, -4.5)
```

# Python 代码块和缩进示例

在 Python 中编码时，不要混合使用制表符和空格，这通常是一个好习惯。这样做可能会导致`TabError`，你的程序将会崩溃。编写代码时保持一致——选择使用制表符或空格缩进，并在整个程序中遵循您选择的约定。

#### **代码块和缩进**

Python 最与众不同的特性之一是使用缩进来标记代码块。考虑我们简单的密码检查程序中的 if 语句:

```
if pwd == 'apple':
    print('Logging on ...')
else:
    print('Incorrect password.')

print('All done!')
```

这几行打印('登录…')和打印('密码不正确。')是两个独立的代码块。这些恰好只有一行，但是 Python 允许您编写由任意数量的语句组成的代码块。

要在 Python 中表示一个代码块，必须将代码块的每一行缩进相同的量。我们的示例 if-statement 中的两个代码块都缩进了四个空格，这是 Python 的典型缩进量。

在大多数其他编程语言中，缩进只是用来帮助代码看起来漂亮。但是在 Python 中，它是指示一个语句属于哪个代码块所必需的。例如，最终的印刷(“全部完成！”)不是缩进的，因此不是 else 块的一部分。

熟悉其他语言的程序员常常对缩进很重要的想法感到恼火:许多程序员喜欢随心所欲地格式化他们的代码。然而，Python 缩进规则非常简单，大多数程序员已经使用缩进来使他们的代码可读。Python 只是将这一思想向前推进了一步，赋予了缩进意义。

#### **If/elif-语句**

if/elif 语句是具有多个条件的广义 if 语句。它被用来做复杂的决定。例如，假设一家航空公司有以下“儿童”票价:2 岁或 2 岁以下的儿童免费乘坐飞机，2 岁以上但 13 岁以下的儿童支付折扣儿童票价，13 岁以上的人支付普通成人票价。以下程序决定了乘客应支付的费用:

```
# airfare.py
age = int(input('How old are you? '))
if age <= 2:
    print(' free')
elif 2 < age < 13:
    print(' child fare)
else:
    print('adult fare')
```

Python 从用户那里获得年龄后，它进入 if/elif 语句，并按照给出的顺序逐个检查每个条件。

因此，它首先检查年龄是否小于 2，如果是，则表明飞行是自由的，并跳出 elif-条件。如果年龄不小于 2，则它检查下一个 elif 条件，以查看年龄是否在 2 和 13 之间。如果是这样，它打印适当的消息并跳出 if/elif 语句。如果 If 条件和 elif 条件都不为真，那么它执行 else 块中的代码。

#### **条件表达式**

Python 多了一个逻辑操作符，有些程序员喜欢(有些不喜欢！).它本质上是可以直接在表达式中使用的 if 语句的简写符号。考虑以下代码:

```
food = input("What's your favorite food? ")
reply = 'yuck' if food == 'lamb' else 'yum'
```

第二行中=右侧的表达式称为条件表达式，其计算结果为“yuck”或“yum”。它相当于以下内容:

```
food = input("What's your favorite food? ")
if food == 'lamb':
   reply = 'yuck'
else:
   reply = 'yum'
```

条件表达式通常比相应的 if/else 语句要短，尽管没有那么灵活或易读。一般来说，当它们使您的代码更简单时，您应该使用它们。

## Python 比较运算符示例

Python 中有八种比较操作。它们都具有相同的优先级(高于布尔运算的优先级)。比较可以任意链接；例如，`x < y <= z`等价于`x < y and y <= z`，除了`y`只被求值一次(但是在两种情况下当`x < y`被发现为假时`z`根本不被求值)。

下面总结了比较操作:

| 操作 | 意义 |
| --- | --- |
| `<` | 严格小于 |
| `<=` | 小于或等于 |
| `>` | 严格大于 |
| `>=` | 大于或等于 |
| `==` | 等于 |
| `!=` | 不等于 |
| `is` | 对象身份 |
| `is not` | 否定的对象标识 |

不同类型的对象，除了不同的数字类型，从来不会相等。此外，一些类型(例如，函数对象)只支持比较的退化概念，即任何两个该类型的对象都不相等。`<`、`<=`、`>`和`>=`运算符在将一个复数与另一个内置的数值类型进行比较时，当对象属于不同的类型而无法进行比较时，或者在没有定义排序的其他情况下，会引发`TypeError`异常。

除非类定义了`__eq__()`方法，否则一个类的不同实例通常被认为是不相等的。

一个类的实例不能相对于同一个类的其他实例或其他类型的对象进行排序，除非该类定义了足够多的方法`__lt__()`、`__le__()`、`__gt__()`和`__ge__()`(通常，`__lt__()`和`__eq__()`就足够了，如果你想要比较操作符的常规含义的话)。

不能自定义`is`和`is not`运算符的行为；此外，它们可以应用于任何两个对象，永远不会引发异常。

我们也可以将`<`和`>`操作符链接在一起。例如，`3 < 4 < 5`会返回`True`，而`3 < 4 > 5`不会。我们还可以链接等式运算符。例如，`3 == 3 < 5`将返回`True`，而`3 == 5 < 5`不会。

### **相等比较-"是" vs "=="**

在 Python 中，有两个比较运算符，允许我们检查两个对象是否相等。`is`操作员和`==`操作员。但是，它们之间有一个关键的区别！

“is”和“==”之间的主要区别可以总结为:

*   `is`用来比较 ****身份****
*   `==`用来比较 ****的平等****

## **例子**

首先，用 Python 创建一个列表。

```
myListA = [1,2,3]
```

接下来，创建该列表的副本。

```
myListB = myListA
```

如果我们使用' == '操作符或' is '操作符，两者都会产生一个 ****真**** 输出。

```
>>> myListA == myListB # both lists contains similar elements
True
>>> myListB is myListA # myListB contains the same elements
True
```

这是因为 myListA 和 myListB 都指向同一个列表变量，这是我在 Python 程序开始时定义的。两份名单完全一样，无论是身份还是内容。

但是，如果我现在创建一个新列表呢？

```
myListC = [1,2,3]
```

执行`==`操作符仍然显示两个列表在内容上是相同的。

```
>>> myListA == myListC
True
```

然而，执行`is`操作符现在将产生一个`False`输出。这是因为 myListA 和 myListC 是两个不同的变量，尽管包含相同的数据。即使他们看起来一样，他们是**T3**T5【不同】。

```
>>> myListA is myListC
False # both lists have different reference
```

总而言之:

*   如果两个变量指向同一个引用，一个`is`表达式输出`True`
*   如果两个变量包含相同的数据，一个`==`表达式输出`True`

# Python 字典示例

python 中的字典(又名“dict”)是一种内置的数据类型，可用于存储 ****`key-value`**** 对。这允许你把一个 ****`dict`**** 当作一个*数据库*来存储和组织数据。

字典的特别之处在于它们的实现方式。类似哈希表的结构使得检查存在性变得容易——这意味着我们可以容易地确定特定的键是否存在于字典中，而不需要检查每个元素。Python 解释器可以直接找到 location 键并检查该键是否在那里。

字典可以使用几乎任何任意的数据类型作为关键字，比如字符串、整数等等。然而，不可散列的值，即包含列表、字典或其他可变类型的值(通过值而不是通过对象标识进行比较)不能用作键。用于键的数字类型遵循数字比较的常规规则:如果两个数字比较相等(例如`1`和`1.0`)，那么它们可以互换使用来索引同一个字典条目。(但是请注意，由于计算机将浮点数存储为近似值，因此将它们用作字典键通常是不明智的。)

字典的一个最重要的要求是键 ****必须**** 是唯一的。

要创建一个空字典，只需使用一对大括号:

```
 >>> teams = {}
    >>> type(teams)
    >>> <class 'dict'>
```

要创建包含一些初始值的非空字典，请放置一个以逗号分隔的键值对列表:

```
 >>> teams = {'barcelona': 1875, 'chelsea': 1910}
    >>> teams
    {'barcelona': 1875, 'chelsea': 1910}
```

向现有字典添加键值对很容易:

```
 >>> teams['santos'] = 1787
    >>> teams
    {'chelsea': 1910, 'barcelona': 1875, 'santos': 1787} # Notice the order - Dictionaries are unordered !
    >>> # extracting value - Just provide the key
    ...
    >>> teams['barcelona']
    1875
```

****`del`**** 操作符用于从字典中删除一个键值对。在已经使用的键再次用于存储值的情况下，与该键关联的旧值会完全丢失。此外，请记住，使用不存在的键提取值是错误的。

```
 >>> del teams['santos']
    >>> teams
    {'chelsea': 1910, 'barcelona': 1875}
    >>> teams['chelsea'] = 2017 # overwriting    
    >>> teams
    {'chelsea': 2017, 'barcelona': 1875}
```

****`in`**** 关键字可以用来检查关键字是否存在于字典中:

```
 >>> 'sanots' in teams
    False    
    >>> 'barcelona' in teams
    True
    >>> 'chelsea' not in teams
    False
```

****`keys`**** 是一个内置的*方法*，可以用来获取给定字典的键。要将字典中的关键字提取为列表:

```
 >>> club_names = list(teams.keys())    
    >>> club_names
    ['chelsea', 'barcelona']
```

还有另一种创建字典的方法是使用 ****`dict()`**** 的方法:

```
 >>> players = dict( [('messi','argentina'), ('ronaldo','portugal'), ('kaka','brazil')] ) # sequence of key-value pair is passed  
    >>> players
    {'ronaldo': 'portugal', 'kaka': 'brazil', 'messi': 'argentina'}
    >>> 
    >>> # If keys are simple strings, it's quite easier to specify pairs using keyword arguments
    ...
    >>> dict( totti = 38, zidane = 43 )
    {'zidane': 43, 'totti': 38}
```

Dict comprehensions 也可以用于从任意键和值表达式创建字典:

```
 >>> {x: x**2 for x in (2, 4, 6)}
    {2: 4, 4: 16, 6: 36}
```

****在字典中循环****
简单地循环字典中的键，而不是键和值:

```
 >>> d = {'x': 1, 'y': 2, 'z': 3} 
    >>> for key in d:
    ...     print(key) # do something
    ...
    x
    y
    z
```

要循环访问键和值，可以使用下面的:
For Python 2.x:

```
 >>> for key, item in d.iteritems():
    ...     print items
    ...
    1
    2
    3
```

对于 Python 3.x 使用 ****`items()`**** :

```
 >>> for key, item in d.items():
    ...     print(key, items)
    ...
    x 1
    y 2
    z 3
```

# Python 对象示例

在 Python 中，一切都是一个*对象*。

*对象*代表属性的逻辑分组。属性是数据和/或函数。当在 Python 中创建一个对象时，它是用一个*身份*、*类型*和*值*创建的。

在其他语言中，*原语*是没有*属性*(属性)的*值*。比如 javascript 中的`undefined`、`null`、`boolean`、`string`、`number`、`symbol`(ECMAScript 2015 新增)都是原语。

在 Python 中，没有原语。`None`、*布尔*、*字符串*、*数字*，甚至*函数*无论如何创建，都是*对象*。

我们可以使用一些内置函数来演示这一点:

*   [T2`id`](https://docs.python.org/3/library/functions.html#id)
*   [T2`type`](https://docs.python.org/3/library/functions.html#type)
*   [T2`dir`](https://docs.python.org/3/library/functions.html#dir)
*   [T2`issubclass`](https://docs.python.org/3/library/functions.html#issubclass)

内置常数`None`、`True`、`False`是*对象*:

我们在这里测试`None`对象。

```
>>> id(None)
4550218168
>>> type(None)
<class 'NoneType'>
>>> dir(None)
[__bool__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
>>> issubclass(type(None), object)
True
```

接下来，我们来考察一下`True`。

```
>>> id(True)
4550117616
>>> type(True)
<class 'bool'>
>>> dir(True)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
>>> issubclass(type(True), object)
True
```

没有理由漏掉`False`！

```
>>> id(False)
4550117584
>>> type(False)
<class 'bool'>
>>> dir(False)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
>>> issubclass(type(False), object)
True
```

*字符串*，即使是由字符串文字创建的，也是*对象*。

```
>>> id("Hello campers!")
4570186864
>>> type('Hello campers!')
<class 'str'>
>>> dir("Hello campers!")
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
>>> issubclass(type('Hello campers!'), object)
True
```

与*号相同。*

```
>>> id(42)
4550495728
>>> type(42)
<class 'int'>
>>> dir(42)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
>>> issubclass(type(42), object)
True
```

## **函数也是对象**

在 Python 中，函数是第一类对象。

Python 中的*函数*也是*对象*，用*身份*、*类型*和*值*创建。它们也可以传递给其他*函数*:

```
>>> id(dir)
4568035688
>>> type(dir)
<class 'builtin_function_or_method'>
>>> dir(dir)
['__call__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__name__', '__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__self__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__text_signature__']
>>> issubclass(type(dir), object)
True
```

也可以将函数绑定到一个名称，并使用该名称调用绑定的函数:

```
>>> a = dir
>>> print(a)
<built-in function dir>
>>> a(a)
['__call__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__name__', '__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__self__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__text_signature__']
```

## 名称绑定和别名功能

在 Python 中定义函数时，该函数名会被引入到当前符号表中。然后，函数名的值的类型被解释器识别为用户定义的函数:

```
>>> something = 1
>>> type(something)
<type 'int'>

>>> def something():
...     pass
...
>>> type(something)
<type 'function'>

>>> something = []
>>> type(something)
<type 'list'>
```

然后可以将函数值赋给另一个名称。在它被重新分配后，它仍然可以作为一个函数使用。使用此方法重命名您的函数:

```
>>> def something(n):
...     print(n)
...
>>> type(something)
<type 'function'>

>>> s = something
>>> s(100)
100
```

# Python 元组

元组是 Python 对象的序列。元组是不可变的，这意味着它们在创建后不能被修改，这与列表不同。

****创作:****

使用一对圆括号`()`创建一个空的`tuple`:

```
 >>> empty_tuple = ()
    >>> print(empty_tuple)
    ()
    >>> type(empty_tuple)
    <class 'tuple'>
    >>> len(empty_tuple)
    0
```

通过用逗号分隔元素来创建带有元素的`tuple`(圆括号`()`是可选的，但有例外):

```
 >>> tuple_1 = 1, 2, 3       # Create tuple without round brackets.
    >>> print(tuple_1)
    (1, 2, 3)
    >>> type(tuple_1)
    <class 'tuple'>
    >>> len(tuple_1)
    3
    >>> tuple_2 = (1, 2, 3)     # Create tuple with round brackets.
    >>> print(tuple_2)
    (1, 2, 3)
    >>> tuple_3 = 1, 2, 3,      # Trailing comma is optional.
    >>> print(tuple_3)
    (1, 2, 3)
    >>> tuple_4 = (1, 2, 3,)    # Trailing comma in round brackets is also optional.
    >>> print(tuple_4)
    (1, 2, 3)
```

具有单个元素的`tuple`必须有尾随逗号(有或没有圆括号):

```
>>> not_tuple = (2)    # No trailing comma makes this not a tuple.
>>> print(not_tuple)
2
>>> type(not_tuple)
<class 'int'>
>>> a_tuple = (2,)     # Single element tuple. Requires trailing comma.
>>> print(a_tuple)
(2,)
>>> type(a_tuple)
<class 'tuple'>
>>> len(a_tuple)
1
>>> also_tuple = 2,    # Round brackets omitted. Requires trailing comma.
>>> print(also_tuple)
(2,)
>>> type(also_tuple)
<class 'tuple'>
```

在不明确的情况下需要圆括号(如果元组是更大表达式的一部分):

注意，实际上是逗号构成了元组，而不是括号。括号是可选的，除了空元组的情况，或者当需要它们来避免语法歧义时。

例如，`f(a, b, c)`是一个带有三个参数的函数调用，而`f((a, b, c))`是一个带有一个 3 元组作为唯一参数的函数调用。

```
 >>> print(1,2,3,4,)          # Calls print with 4 arguments: 1, 2, 3, and 4
    1 2 3 4
    >>> print((1,2,3,4,))        # Calls print with 1 argument: (1, 2, 3, 4,)
    (1, 2, 3, 4)
    >>> 1, 2, 3 == (1, 2, 3)     # Equivalent to 1, 2, (3 == (1, 2, 3))
    (1, 2, False)
    >>> (1, 2, 3) == (1, 2, 3)   # Use surrounding round brackets when ambiguous.
    True
```

也可以用`tuple`构造函数创建一个`tuple`:

```
 >>> empty_tuple = tuple()
    >>> print(empty_tuple)
    ()
    >>> tuple_from_list = tuple([1,2,3,4])
    >>> print(tuple_from_list)
    (1, 2, 3, 4)
    >>> tuple_from_string = tuple("Hello campers!")
    >>> print(tuple_from_string)
    ('H', 'e', 'l', 'l', 'o', ' ', 'c', 'a', 'm', 'p', 'e', 'r', 's', '!')
    >>> a_tuple = 1, 2, 3
    >>> b_tuple = tuple(a_tuple)    # If the constructor is called with a tuple for
    the iterable,
    >>> a_tuple is b_tuple          # the tuple argument is returned.
    True
```

****访问元素一`tuple` :****

`tuples`的元素的访问和索引方式与`lists`相同。

```
>>> my_tuple = 1, 2, 9, 16, 25
>>> print(my_tuple)
(1, 2, 9, 16, 25)
```

*零索引*

```
 >>> my_tuple[0]
    1
    >>> my_tuple[1]
    2
    >>> my_tuple[2]
    9
```

*环绕分度*

```
 >>> my_tuple[-1]
    25
    >>> my_tuple[-2]
    16
```

****装箱拆箱:****

语句`t = 12345, 54321, 'hello!'`是元组打包的一个例子:值`12345`、`54321`和`'hello!'`一起打包在一个元组中。相反的操作也是可能的:

```
 >>> x, y, z = t
```

这被恰当地称为序列解包，适用于右侧的任何序列。序列解包要求等号左边的变量和序列中的元素一样多。注意，多重赋值实际上只是元组打包和序列解包的组合。

```
 >>> t = 1, 2, 3    # Tuple packing.
    >>> print(t)
    (1, 2, 3)
    >>> a, b, c = t    # Sequence unpacking.
    >>> print(a)
    1
    >>> print(b)
    2
    >>> print(c)
    3
    >>> d, e, f = 4, 5, 6    # Multiple assignment combines packing and unpacking.
    >>> print(d)
    4
    >>> print(e)
    5
    >>> print(f)
    6
    >>> a, b = 1, 2, 3       # Multiple assignment requires each variable (right)
    have a matching element (left).
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ValueError: too many values to unpack (expected 2)
```

****不可变:****

`tuples`是不可变的容器，保证它们包含的**对象不会改变。它不 ****而**** 保证它们包含的对象不会改变:**

```
 `>>> a_list = []
    >>> a_tuple = (a_list,)    # A tuple (immutable) with a list (mutable) element.
    >>> print(a_tuple)
    ([],)

    >>> a_list.append("Hello campers!")
    >>> print(a_tuple)         # Element of the immutable is mutated.
    (['Hello campers!'],)`
```

******用途:******

**函数只能返回一个值，然而，一个异构的`tuple`可以用来从一个函数返回多个值。一个例子是内置的`enumerate`函数，它返回一个异构`tuples`的 iterable:**

```
 `>>> greeting = ["Hello", "campers!"]
    >>> enumerator = enumerate(greeting)
    >>> enumerator.next()
    >>> enumerator.__next__()
    (0, 'Hello')
    >>> enumerator.__next__()
    (1, 'campers!')`
```

# **Python For 循环语句示例**

**Python 利用 for 循环来迭代元素列表。这与 C 或 Java 不同，C 或 Java 使用 for 循环逐步更改值，并使用该值访问数组之类的东西。**

**For 循环遍历基于集合的数据结构，如列表、元组和字典。**

**基本语法是:**

```
`for value in list_of_values:
  # use value inside this block`
```

**一般来说，您可以使用任何东西作为迭代器值，其中 iterable 的条目可以被赋值给。例如，您可以从元组列表中解包元组:**

```
`list_of_tuples = [(1,2), (3,4)]

for a, b in list_of_tuples:
  print("a:", a, "b:", b)`
```

**另一方面，你可以循环任何可迭代的东西。您可以调用函数或使用列表文字。**

```
`for person in load_persons():
  print("The name is:", person.name)`
```

```
`for character in ["P", "y", "t", "h", "o", "n"]:
  print("Give me a '{}'!".format(character))`
```

**使用 For 循环的一些方式:**

******迭代范围()函数******

```
`for i in range(10):
    print(i)`
```

**range 不是一个函数，它实际上是一个不可变的序列类型。输出将包含从下限(即 0)到上限(即 10)的结果，但不包括 10。默认情况下，下限或起始索引设置为零。输出:**

```
`>
0
1
2
3
4
5
6
7
8
9
>`
```

**此外，可以通过添加第二个和第三个参数来指定序列的下限，甚至序列的步长。**

```
`for i in range(4,10,2): #From 4 to 9 using a step of two
    print(i)`
```

**输出:**

```
`>
4
6
8
>`
```

******xrange()函数******

**在很大程度上，xrange 和 range 在功能上是完全相同的。它们都提供了一种生成整数列表的方法，供您随意使用。唯一的区别是 range 返回一个 Python 列表对象，而 xrange 返回一个 xrange 对象。这意味着 xrange 并不像 range 那样在运行时生成静态列表。它用一种叫做让步的特殊技术创造出你需要的价值。这种技术用于一种称为发生器的对象。**

**再补充一点。在 Python 3.x 中，xrange 函数不再存在。range 函数现在做 Python 2.x 中 xrange 做的事情**

******迭代列表或元组中的值******

```
`A = ["hello", 1, 65, "thank you", [2, 3]]
for value in A:
    print(value)`
```

**输出:**

```
`>
hello
1
65
thank you
[2, 3]
>`
```

```
**`fruits_to_colors = {"apple": "#ff0000",
                    "lemon": "#ffff00",
                    "orange": "#ffa500"}

for key in fruits_to_colors:
    print(key, fruits_to_colors[key])`**
```

****输出:****

```
**`>
apple #ff0000
lemon #ffff00
orange #ffa500
>`**
```

********用 zip()函数**** 在一个循环中迭代两个相同大小的列表****

```
`A = ["a", "b", "c"]
B = ["a", "d", "e"]

for a, b in zip(A, B):
  print a, b, a == b` 
```

**输出:**

```
`>
a a True
b d False
c e False
>`
```

******遍历一个列表，用 enumerate()函数**** 得到对应的索引**

```
`A = ["this", "is", "something", "fun"]

for index,word in enumerate(A):
    print(index, word)`
```

**输出:**

```
`>
0 this
1 is
2 something
3 fun
>`
```

**一个常见的用例是迭代字典:**

```
`for name, phonenumber in contacts.items():
  print(name, "is reachable under", phonenumber)`
```

**如果您绝对需要访问您的迭代的当前索引，请使用 ****而不是****`range(len(iterable))`！这是一种极其糟糕的做法，会让你遭到资深 Python 开发者的嘲笑。请改用内置函数`enumerate()`:**

```
`for index, item in enumerate(shopping_basket):
  print("Item", index, "is a", item)`
```

******for/else 语句**** Pyhton 允许在 for 循环中使用 else，当循环体中的所有条件都不满足时，就会执行 else 情况。要使用 else，我们必须使用`break`语句，这样我们就可以在满足条件的情况下跳出循环。如果我们不越狱，那么 else 部分将被执行。**

```
`week_days = ['Monday','Tuesday','Wednesday','Thursday','Friday']
today = 'Saturday'
for day in week_days:
  if day == today:
    print('today is a week day')
    break
else:
  print('today is not a week day')`
```

**在上面的例子中，输出将是`today is not a week day`,因为循环中的 break 永远不会被执行。**

******使用内联循环函数**** 遍历列表**

**我们也可以使用 python 进行内联迭代。例如，如果我们需要将列表中的所有单词大写，我们可以简单地执行以下操作:**

```
`A = ["this", "is", "awesome", "shinning", "star"]

UPPERCASE = [word.upper() for word in A]
print (UPPERCASE)`
```

**输出:**

```
`>
['THIS', 'IS', 'AWESOME', 'SHINNING', 'STAR']`
```

# **Python 函数示例**

**函数允许你定义一个可重用的代码块，它可以在你的程序中多次执行。**

**函数允许你为复杂的问题创建更多的模块和[干](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)的解决方案。**

**虽然 Python 已经提供了许多内置函数，比如`print()`和`len()`，但是您也可以定义自己的函数在项目中使用。**

**在代码中使用函数的一个很大的好处是，它减少了项目中代码的总行数。**

### ****语法****

**在 Python 中，函数定义具有以下特性:**

1.  **关键词`def`**
2.  **函数名**
3.  **括号'()'，并在括号内输入参数，尽管输入参数是可选的。**
4.  **冒号':'**
5.  **一些要执行的代码块**
6.  **返回语句(可选)**

```
`# a function with no parameters or returned values
def sayHello():
  print("Hello!")

sayHello()  # calls the function, 'Hello!' is printed to the console

# a function with a parameter
def helloWithName(name):
  print("Hello " + name + "!")

helloWithName("Ada")  # calls the function, 'Hello Ada!' is printed to the console

# a function with multiple parameters with a return statement
def multiply(val1, val2):
  return val1 * val2

multiply(3, 5)  # prints 15 to the console`
```

**函数是代码块，只需调用函数就可以重用。这使得简单、优雅的代码重用成为可能，而无需显式地重写代码部分。这使得代码更具可读性，更易于调试，并减少了键入错误。**

**Python 中的函数是使用`def`关键字创建的，后跟括号内的函数名和函数参数。**

**函数总是返回值。函数使用关键字`return`来返回值。如果不想返回任何值，就返回默认值`None`。**

**函数名用于调用函数，在括号内传递所需的参数。**

```
`# this is a basic sum function
def sum(a, b):
  return a + b

result = sum(1, 2)
# result = 3`
```

**您可以为参数定义默认值，如果没有给定，Python 会将该参数的值解释为默认值。**

```
`def sum(a, b=3):
  return a + b

result = sum(1)
# result = 4`
```

**您可以使用参数的名称，按照您想要的顺序传递参数。**

```
`result = sum(b=2, a=2)
# result = 4`
```

**但是，不可能在非关键字参数之前传递关键字参数。**

```
`result = sum(3, b=2)
#result = 5
result2 = sum(b=2, 3)
#Will raise SyntaxError`
```

**函数也是对象，所以你可以把它们赋给一个变量，然后像函数一样使用这个变量。**

```
`s = sum
result = s(1, 2)
# result = 3`
```

### ****注释****

**如果函数定义包含参数，则在调用函数时必须提供相同数量的参数。**

```
`print(multiply(3))  # TypeError: multiply() takes exactly 2 arguments (0 given)

print(multiply('a', 5))  # 'aaaaa' printed to the console

print(multiply('a', 'b'))  # TypeError: Python can't multiply two strings`
```

**函数将运行的代码块包括函数内缩进的所有语句。**

```
`def myFunc():
print('this will print')
print('so will this')

x = 7
# the assignment of x is not a part of the function since it is not indented`
```

**函数中定义的变量只存在于该函数的范围内。**

```
`def double(num):
x = num * 2
return x

print(x)  # error - x is not defined
print(double(4))  # prints 8`
```

**Python 只在调用函数时解释函数块，而不是在定义函数时解释。因此，即使函数定义块包含某种错误，python 解释器也只会在调用函数时指出来。**

# **Python 生成器示例**

**生成器是一种特殊类型的函数，允许您在不结束函数的情况下返回值。它通过使用`yield`关键字来实现这一点。类似于`return`,`yield`表达式将返回一个值给调用者。两者的关键区别在于`yield`将暂停函数，允许将来返回更多的值。**

**生成器是可迭代的，因此它们可以干净地用于 for 循环或任何迭代。**

```
`def my_generator():
    yield 'hello'
    yield 'world'
    yield '!'

for item in my_generator():
    print(item)

# output:
# hello
# world
# !`
```

**像其他迭代器一样，生成器可以传递给`next`函数来检索下一项。当生成器没有更多的值要生成时，就会产生一个`StopIteration`错误。**

```
`g = my_generator()
print(next(g))
# 'hello'
print(next(g))
# 'world'
print(next(g))
# '!'
print(next(g))
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# StopIteration`
```

**当您需要创建大量的值，但不需要同时将它们保存在内存中时，生成器特别有用。例如，如果您需要打印前一百万个斐波那契数，您通常会返回一个包含一百万个值的列表，并迭代该列表以打印每个值。然而，使用生成器，您可以一次返回一个值:**

```
`def fib(n):
    a = 1
    b = 1
    for i in range(n):
        yield a
        a, b = b, a + b

for x in fib(1000000):
    print(x)`
```

# **Python 迭代器示例**

**Python 支持容器迭代的概念。这是使用两种不同的方法实现的；这些用于允许用户定义的类支持迭代。**

**[Python 文档-迭代器类型](https://docs.python.org/3/library/stdtypes.html#iterator-types)**

**迭代是以编程方式将一个步骤重复给定次数的过程。程序员可以利用迭代对数据集合中的每一项执行相同的操作，例如打印出列表中的每一项。**

*   **对象可以实现一个返回迭代器对象的`__iter__()`方法来支持迭代。**

**迭代器对象必须实现:**

*   **`__iter__()`:返回迭代器对象。**
*   **`__next__()`:返回容器的下一个对象，迭代器 *object = 'abc '。 ****iter**** () print(迭代器*对象)print(id(迭代器*对象))print(id(迭代器*对象。 ****iter**** ())) #返回迭代器本身。打印(迭代器*对象。 ****下一个**** ()) #返回第一个对象并推进迭代器。打印(迭代器*对象。 ****下一个**** ()) #返回第二个对象并推进迭代器。打印(迭代器*对象。 ****下一个**** ()) #返回第三个对象并推进迭代器。打印(迭代器*对象。 ****下一个**** ()) #引发 StopIteration 异常。**

**输出:**

```
`<str_iterator object at 0x102e196a0>
4343305888
4343305888
a
b
c
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-1-d466eea8c1b0> in <module>()
      6 print(iterator_object.__next__())     # Returns 2nd object and advances iterator.
      7 print(iterator_object.__next__())     # Returns 3rd object and advances iterator.
----> 8 print(iterator_object.__next__())     # Raises StopIteration Exception.

StopIteration:`
```

# ****Python 示例中的三元运算符****

**Python 中的三元运算，通常也称为条件表达式，允许程序员执行评估并根据给定条件的真值返回值。**

**三元运算符不同于标准的`if`、`else`、`elif`结构，因为它不是控制流结构，其行为更像其他运算符，如 Python 语言中的`==`或`!=`。**

### ****例子****

**在这个例子中，如果变量`val`是偶数，则返回字符串`Even`，否则返回字符串`Odd`。然后将返回的字符串赋给`is_even`变量并打印到控制台。**

#### ****输入****

```
`for val in range(1, 11):
    is_even = "Even" if val % 2 == 0 else "Odd"
    print(val, is_even, sep=' = ')`
```

#### ****输出****

```
`1 = Odd
2 = Even
3 = Odd
4 = Even
5 = Odd
6 = Even
7 = Odd
8 = Even
9 = Odd
10 = Even`
```

# **Python While 循环语句示例**

**Python 与其他流行语言一样利用了`while`循环。`while`循环评估一个条件，如果条件为真，则执行一段代码。代码块重复执行，直到条件变为假。**

**基本语法是:**

```
`counter = 0
while counter < 10:
   # Execute the block of code here as
   # long as counter is less than 10`
```

**下面显示了一个示例:**

```
`days = 0    
week = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
while days < 7:
   print("Today is " + week[days])
   days += 1`
```

**输出:**

```
`Today is Monday
Today is Tuesday
Today is Wednesday
Today is Thursday
Today is Friday
Today is Saturday
Today is Sunday`
```

**以上代码的逐行解释:**

1.  **变量“天数”被设置为值 0。**
2.  **变量 week 被分配给包含一周中所有日期的列表。**
3.  **当循环开始时**
4.  **代码块将被执行，直到条件返回“真”。**
5.  **条件是“天数< 7”，大致意思是运行 while 循环，直到变量天数小于 7**
6.  **因此，当 days=7 时，while 循环停止执行。**
7.  **days 变量在每次迭代中都会更新。**
8.  **当 while 循环第一次运行时，控制台上会显示一行“今天是星期一”,变量 days 等于 1。**
9.  **因为变量 days 等于小于 7 的 1，所以再次执行 while 循环。**
10.  **当控制台打印出“今天是星期天”时，变量 days 现在等于 7，while 循环停止执行。**

## **Python 中的 f 字符串**

**在 Python 版中，实现了一种格式化字符串的新方法。这种新方法被称为文字字符串插值(虽然通常被称为 f 字符串)。**

**f-string 的使用允许程序员以干净简洁的方式动态地将变量插入到字符串中。除了在字符串中插入变量之外，这个特性还为程序员提供了计算表达式、连接集合内容，甚至在 f 字符串中调用函数的能力。**

**为了在 f 字符串中执行这些动态行为，我们将它们放在字符串中的花括号内，并在字符串的开头加上小写的 f(在左引号之前)。**

### ****例题****

**在运行时将变量动态插入字符串:**

```
`name = 'Jon Snow'
greeting = f'Hello! {name}'
print(greeting)`
```

**计算字符串中的表达式:**

```
`val1 = 2
val2 = 3
expr = f'The sum of {val1} + {val2} is {val1 + val2}'
print(expr)`
```

**调用函数并在字符串中插入输出:**

```
`def sum(*args):
    result = 0
    for arg in args:
        result += arg
    return result

func = f'The sum of 3 + 5 is {sum(3, 5)}'
print(func)`
```

**将集合的内容连接到字符串中:**

```
`fruits = ['Apple', 'Banana', 'Pear']

list_str = f'List of fruits: {", ".join(fruits)}'
print(list_str)`
```

# **如何安装 Python 3**

**可以从这个官方[链接](https://www.python.org/downloads/)下载 Python。基于你的操作系统(Windows 或 Linux 或 OSX)，你可能想按照这些指令[安装 Python 3。](http://docs.python-guide.org/en/latest/starting/installation/)**

## ****使用虚拟环境****

**用沙箱保护你的 Python 安装并把它与你的 T2 系统 Python 分开总是一个好主意。*系统 Python* 是 Python 解释器的路径，随操作系统一起安装的其他模块会用到它。**

**直接用*系统 Python* 安装 Python Web-frames 或库**不安全。相反，在开发 Python 应用程序时，您可以使用 [Virtualenv](https://virtualenv.readthedocs.org/en/latest/) 来创建和派生一个单独的 Python 进程。****

### ****虚拟包装****

****[Virtualenvwrapper 模块](https://virtualenvwrapper.readthedocs.org/en/latest/)使您可以轻松地在一台机器上管理和运行多个 Python 沙盒环境，而不会破坏任何用 Python 编写并由您的机器使用的模块或服务。****

****当然，大多数云托管的开发环境，如 [Nitrous](https://www.nitrous.io/) 或 [Cloud9](https://c9.io/) 也预装了这些，并为您获取编码做好了准备！激活 Python 3 环境后，您可以从仪表板中快速选择一个框并开始编码。****

****在 [Cloud9](https://c9.io/) 中，创建新的开发环境时需要选择 Django 框。****

****下面是几个 shell 命令示例。如果您希望复制粘贴，请注意,`$`符号是终端提示符的简写，它不是命令的一部分。我的终端提示符看起来像这样:****

```
**`alayek:~/workspace (master) # Python 示例–数据结构、算法、语法示例代码

> 原文：<https://www.freecodecamp.org/news/python-example/>

Python 是一种通用编程语言，它是动态类型化的、可解释的，并且以其简单的可读性和优秀的设计原则而闻名。

freeCodeCamp 有一个最受欢迎的 Python 课程。完全免费。你可以在 YouTube 上看这个视频。

![maxresdefault](img/78d520922041325377ae0c5fc97abff9.png)

# Python 数据结构示例

关于浮点数以及它们在 Python 中如何工作的一些一般信息，可以在这里找到。

几乎所有 Python 的实现都遵循 IEEE 754 规范:二进制浮点运算的标准。更多信息可在 IEEE 网站上找到。

可以使用[浮点文字](https://docs.python.org/3/reference/lexical_analysis.html#floating-point-literals)创建浮点对象:

```
>>> 3.14
3.14
>>> 314\.    # Trailing zero(s) not required.
314.0
>>> .314    # Leading zero(s) not required.
0.314
>>> 3e0
3.0
>>> 3E0     # 'e' or 'E' can be used.
3.0
>>> 3e1     # Positive value after e moves the decimal to the right.
30.0
>>> 3e-1    # Negative value after e moves the decimal to the left.
0.3
>>> 3.14e+2 # '+' not required but can be used for exponent part.
314.0
```

数字文字不包含符号，但是可以通过在文字前加上一元`-`(减)运算符(没有空格)来创建负浮点对象:

```
>>> -3.141592653589793
-3.141592653589793
>>> type(-3.141592653589793)
<class 'float'>
```

同样，正 float 对象可以以一元的`+`(加号)操作符为前缀，在字面量之前没有空格。通常`+`被省略:

```
>>> +3.141592653589793
3.141592653589793
```

请注意，前导零和尾随零对浮点文字有效。

同样，正 float 对象可以以一元的`+`(加号)操作符为前缀，在字面量之前没有空格。通常`+`被省略:

```
>>> +3.141592653589793
3.141592653589793
```

请注意，前导零和尾随零对浮点文字有效。

```
>>> 0.0
0.0
>>> 00.00
0.0
>>> 00100.00100
100.001
>>> 001e0010      # Same as 1e10
10000000000.0
```

[`float`构造函数](https://docs.python.org/3/library/functions.html#float)是创建`float`对象的另一种方式。

在可能的情况下，最好使用浮点文字创建`float`对象:

```
>>> a = 3.14         # Prefer floating point literal when possible.
>>> type(a)
<class 'float'>
>>> b = int(3.14)    # Works but unnecessary.
>>> type(b)
<class 'float'>
```

但是，float 构造函数允许从其他数字类型创建 float 对象:

```
>>> a = 4
>>> type(a)
<class 'int'>
>>> print(a)
4
>>> b = float(4)
>>> type(b)
<class 'float'>
>>> print(b)
4.0
>>> float(400000000000000000000000000000000)
4e+32
>>> float(.00000000000000000000000000000004)
4e-32
>>> float(True)
1.0
>>> float(False)
0.0
```

`float`构造函数还将从表示数字文字的字符串中生成`float`对象:

```
>>> float('1')
1.0
>>> float('.1')
0.1
>>> float('3.')
3.0
>>> float('1e-3')
0.001
>>> float('3.14')
3.14
>>> float('-.15e-2')
-0.0015
```

`float`构造函数也可以用来表示`NaN`(不是数字)、负数`infinity`和`infinity`(注意这些字符串不区分大小写):

```
>>> float('nan')
nan
>>> float('inf')
inf
>>> float('-inf')
-inf
>>> float('infinity')
inf
>>> float('-infinity')
-inf
```

### 复数

复数有一个实部和一个虚部，每个都用一个浮点数表示。

一个复数的虚部可以用一个虚文字来创建，这导致一个复数的实部为`0.0`:

```
>>> a = 3.5j
>>> type(a)
<class 'complex'>
>>> print(a)
3.5j
>>> a.real
0.0
>>> a.imag
3.5
```

不存在创建具有非零实部和虚部的复数的文字。要创建非零实部复数，请向浮点数添加一个虚数:

```
>>> a = 1.1 + 3.5j
>>> type(a)
<class 'complex'>
>>> print(a)
(1.1+3.5j)
>>> a.real
1.1
>>> a.imag
3.5
```

或者使用[复杂构造函数](https://docs.python.org/3/library/functions.html#complex)。

```
class complex([real[, imag]])
```

用于调用复杂构造函数的参数可以是数字(包括`complex`)类型，用于以下任一参数:

```
>>> complex(1, 1)
(1+1j)
>>> complex(1j, 1j)
(-1+1j)
>>> complex(1.1, 3.5)
(1.1+3.5j)
>>> complex(1.1)
(1.1+0j)
>>> complex(0, 3.5)
3.5j
```

A `string`也可以作为自变量。如果将字符串用作参数，则不允许有第二个参数

```
>>> complex("1.1+3.5j")
(1.1+3.5j)
```

# python 胸部示例

`bool()`是 Python 3 中的内置函数。该函数返回一个布尔值，即 True 或 False。它需要一个参数，`x`。

## **自变量**

它需要一个参数。使用标准的[真值测试程序](https://docs.python.org/3/library/stdtypes.html#truth)转换`x`。

## **返回值**

如果`x`为假或省略，则返回`False`；否则它返回`True`。

## **代码示例**

```
print(bool(4 > 2)) # Returns True as 4 is greater than 2
print(bool(4 < 2)) # Returns False as 4 is not less than 2
print(bool(4 == 4)) # Returns True as 4 is equal to 4
print(bool(4 != 4)) # Returns False as 4 is equal to 4 so inequality doesn't holds
print(bool(4)) # Returns True as 4 is a non-zero value
print(bool(-4)) # Returns True as -4 is a non-zero value
print(bool(0)) # Returns False as it is a zero value
print(bool('dskl')) # Returns True as the string is a non-zero value
print(bool([1, 2, 3])) # Returns True as the list is a non-zero value
print(bool((2,3,4))) # Returns True as tuple is a non-zero value
print(bool([])) # Returns False as list is empty and equal to 0 according to truth value testing
```

# Python 布尔运算符示例

[`and`](https://docs.python.org/3/reference/expressions.html#and)[`or`](https://docs.python.org/3/reference/expressions.html#or)[`not`](https://docs.python.org/3/reference/expressions.html#not)

[Python 文档-布尔运算](https://docs.python.org/3/library/stdtypes.html#boolean-operations-and-or-not)

这些是布尔运算，按优先级升序排列:

OperationResultNotes x 或 y 如果 x 为假，则 y，else x (1) x 和 y 如果 x 为假，则 x，else y (2) not x 如果 x 为假，则 True，else False (3)。

****备注:****

1.  这是一个短路操作符，所以如果第一个参数为假，它只计算第二个参数。
2.  这是一个短路操作符，所以只有当第一个参数为真时，它才计算第二个参数。
3.  not 的优先级比非布尔运算符低，所以 not a == b 解释为 not (a == b)，a == not b 是语法错误。

## **例子:**

### **:**

```
>>> not True
False
>>> not False
True
```

### **:**

```
>>> True and False    # Short-circuited at first argument.
False
>>> False and True    # Second argument is evaluated.
False
>>> True and True     # Second argument is evaluated.
True
```

### **:**

```
>>> True or False    # Short-circuited at first argument.
True
>>> False or True    # Second argument is evaluated.
True
>>> False or False   # Second argument is evaluated.
False
```

# Python 常量示例

三个常用的内置常数:

*   `True`:*布尔*类型的真值。对`True`的赋值产生一个*语法错误*。
*   `False`:*布尔*类型的假值。对`False`的赋值产生一个*语法错误*。
*   `None`:字体 *NoneType* 的唯一值。None 通常用于表示缺少值，例如当默认参数没有传递给函数时。对`None`的赋值产生一个*语法错误*。

其他内置常数:

*   `NotImplemented`:应由二进制特殊方法返回的特殊值，如`__eg__()`、`__add__()`、`__rsub__()`等。)来指示该操作对于另一个类型没有实现。
*   `Ellipsis`:特殊值，通常与用户定义的容器数据类型的扩展切片语法结合使用。
*   `__debug__`:如果 Python 没有使用-o 选项启动，则为 True。

站点模块添加的常量。site 模块(在启动时自动导入，除非给出了-S 命令行选项)向内置名称空间添加了几个常量。它们对于交互式解释器外壳很有用，不应该在程序中使用。

这些对象在打印时会打印一条类似“使用 quit()或 Ctrl-D(即 EOF)退出”的消息，并且在被调用时会使用指定的退出代码引发 SystemExit:

*   退出(代码=无)
*   退出(代码=无)

这些对象在打印时会打印一条类似“键入许可证()以查看完整的许可证文本”的消息，并在被调用时以类似寻呼机的方式显示相应的文本(一次显示一个屏幕):

*   版权
*   许可证
*   信用

# 调用 Python 函数示例

函数定义语句不执行函数。执行(调用)一个函数是通过使用该函数的名称，后跟括起所需参数(如果有的话)的括号来完成的。

```
>>> def say_hello():
...     print('Hello')
...
>>> say_hello()
Hello
```

函数的执行引入了一个新的符号表，用于函数的局部变量。更准确地说，函数中的所有变量赋值都将值存储在局部符号表中。

另一方面，变量引用首先在局部符号表中查找，然后在封闭函数的局部符号表中查找，然后在全局符号表中查找，最后在内置名称表中查找。因此，全局变量不能在函数中直接赋值(除非在全局语句中命名)，尽管它们可能被引用。

```
>>> a = 1
>>> b = 10
>>> def fn():
...     print(a)    # local a is not assigned, no enclosing function, global a referenced.
...     b = 20      # local b is assigned in the local symbol table for the function.
...     print(b)    # local b is referenced.
...
>>> fn()
1
20
>>> b               # global b is not changed by the function call.
10
```

函数调用的实际参数(自变量)在被调用函数被调用时被引入到被调用函数的局部符号表中。通过这种方式，使用 call by value 传递参数(其中值始终是对象引用，而不是对象的值)。当一个函数调用另一个函数时，会为该调用创建一个新的局部符号表。

```
>>> def greet(s):
...     s = "Hello " + s    # s in local symbol table is reassigned.
...     print(s)
...
>>> person = "Bob"
>>> greet(person)
Hello Bob
>>> person                  # person used to call remains bound to original object, 'Bob'.
'Bob'
```

用于调用函数的参数不能被函数重新分配，但是引用可变对象的参数的值可以改变:

```
>>> def fn(arg):
...     arg.append(1)
...
>>> a = [1, 2, 3]
>>> fn(a)
>>> a
[1, 2, 3, 1]
```

# Python 类示例

类提供了一种将数据和功能捆绑在一起的方法。创建一个新类会创建一个新类型的对象，允许创建该类型的新实例。每个类实例都可以附加属性来维护其状态。类实例也可以有修改其状态的方法(由其类定义)。

与其他编程语言相比，Python 的类机制以最少的新语法和语义添加了类。它混合了 C++中的类机制。

Python 类提供了面向对象编程的所有标准特性:类继承机制允许多个基类，派生类可以覆盖其基类的任何方法，方法可以调用同名基类的方法。

对象可以包含任意数量和种类的数据。与模块一样，类也具有 Python 的动态特性:它们是在运行时创建的，创建后可以进一步修改。

#### **类定义语法:**

最简单的类定义形式如下:

```
class ClassName:
    <statement-1>
        ...
        ...
        ...
    <statement-N>
```

#### **类对象:**

类对象支持两种操作:属性引用和实例化。

属性引用使用 Python 中用于所有属性引用的标准语法:`obj.name`。有效的属性名是创建类对象时类的命名空间中的所有名称。所以，如果类的定义是这样的:

```
class MyClass:
    """ A simple example class """
    i = 12345

    def f(self):
        return 'hello world'
```

那么`MyClass.i`和`MyClass.f`就是有效的属性引用，分别返回一个整数和一个函数对象。类属性也可以被赋值，所以你可以通过赋值来改变`MyClass.i`的值。`__doc__`也是一个有效属性，返回属于类:`"A simple example class"`的 docstring。

类实例化使用函数符号。假设类对象是一个无参数的函数，它返回该类的一个新实例。例如(假设上面的类):

```
x = MyClass()
```

创建类的新实例，并将此对象赋给局部变量 x。

实例化操作(“调用”一个类对象)创建一个空对象。许多类喜欢用定制到特定初始状态的实例来创建对象。因此一个类可以定义一个名为 ****init**** ()的特殊方法，如下所示:

```
def __init__(self):
    self.data = []
```

当一个类定义了一个`__init__()`方法时，类实例化自动为新创建的类实例调用`__init__()`。因此，在本例中，可以通过以下方式获得一个新的初始化实例:

```
x = MyClass()
```

当然，`__init__()`方法可能有更大灵活性的理由。在这种情况下，给予类实例化操作符的参数被传递给`__init__()`。举个例子，

```
class Complex:
    def __init__(self, realpart, imagpart):
        self.r = realpart
        self.i = imagpart
              ...

x = Complex(3.0, -4.5)
>>> x.r, x.i
(3.0, -4.5)
```

# Python 代码块和缩进示例

在 Python 中编码时，不要混合使用制表符和空格，这通常是一个好习惯。这样做可能会导致`TabError`，你的程序将会崩溃。编写代码时保持一致——选择使用制表符或空格缩进，并在整个程序中遵循您选择的约定。

#### **代码块和缩进**

Python 最与众不同的特性之一是使用缩进来标记代码块。考虑我们简单的密码检查程序中的 if 语句:

```
if pwd == 'apple':
    print('Logging on ...')
else:
    print('Incorrect password.')

print('All done!')
```

这几行打印('登录…')和打印('密码不正确。')是两个独立的代码块。这些恰好只有一行，但是 Python 允许您编写由任意数量的语句组成的代码块。

要在 Python 中表示一个代码块，必须将代码块的每一行缩进相同的量。我们的示例 if-statement 中的两个代码块都缩进了四个空格，这是 Python 的典型缩进量。

在大多数其他编程语言中，缩进只是用来帮助代码看起来漂亮。但是在 Python 中，它是指示一个语句属于哪个代码块所必需的。例如，最终的印刷(“全部完成！”)不是缩进的，因此不是 else 块的一部分。

熟悉其他语言的程序员常常对缩进很重要的想法感到恼火:许多程序员喜欢随心所欲地格式化他们的代码。然而，Python 缩进规则非常简单，大多数程序员已经使用缩进来使他们的代码可读。Python 只是将这一思想向前推进了一步，赋予了缩进意义。

#### **If/elif-语句**

if/elif 语句是具有多个条件的广义 if 语句。它被用来做复杂的决定。例如，假设一家航空公司有以下“儿童”票价:2 岁或 2 岁以下的儿童免费乘坐飞机，2 岁以上但 13 岁以下的儿童支付折扣儿童票价，13 岁以上的人支付普通成人票价。以下程序决定了乘客应支付的费用:

```
# airfare.py
age = int(input('How old are you? '))
if age <= 2:
    print(' free')
elif 2 < age < 13:
    print(' child fare)
else:
    print('adult fare')
```

Python 从用户那里获得年龄后，它进入 if/elif 语句，并按照给出的顺序逐个检查每个条件。

因此，它首先检查年龄是否小于 2，如果是，则表明飞行是自由的，并跳出 elif-条件。如果年龄不小于 2，则它检查下一个 elif 条件，以查看年龄是否在 2 和 13 之间。如果是这样，它打印适当的消息并跳出 if/elif 语句。如果 If 条件和 elif 条件都不为真，那么它执行 else 块中的代码。

#### **条件表达式**

Python 多了一个逻辑操作符，有些程序员喜欢(有些不喜欢！).它本质上是可以直接在表达式中使用的 if 语句的简写符号。考虑以下代码:

```
food = input("What's your favorite food? ")
reply = 'yuck' if food == 'lamb' else 'yum'
```

第二行中=右侧的表达式称为条件表达式，其计算结果为“yuck”或“yum”。它相当于以下内容:

```
food = input("What's your favorite food? ")
if food == 'lamb':
   reply = 'yuck'
else:
   reply = 'yum'
```

条件表达式通常比相应的 if/else 语句要短，尽管没有那么灵活或易读。一般来说，当它们使您的代码更简单时，您应该使用它们。

## Python 比较运算符示例

Python 中有八种比较操作。它们都具有相同的优先级(高于布尔运算的优先级)。比较可以任意链接；例如，`x < y <= z`等价于`x < y and y <= z`，除了`y`只被求值一次(但是在两种情况下当`x < y`被发现为假时`z`根本不被求值)。

下面总结了比较操作:

| 操作 | 意义 |
| --- | --- |
| `<` | 严格小于 |
| `<=` | 小于或等于 |
| `>` | 严格大于 |
| `>=` | 大于或等于 |
| `==` | 等于 |
| `!=` | 不等于 |
| `is` | 对象身份 |
| `is not` | 否定的对象标识 |

不同类型的对象，除了不同的数字类型，从来不会相等。此外，一些类型(例如，函数对象)只支持比较的退化概念，即任何两个该类型的对象都不相等。`<`、`<=`、`>`和`>=`运算符在将一个复数与另一个内置的数值类型进行比较时，当对象属于不同的类型而无法进行比较时，或者在没有定义排序的其他情况下，会引发`TypeError`异常。

除非类定义了`__eq__()`方法，否则一个类的不同实例通常被认为是不相等的。

一个类的实例不能相对于同一个类的其他实例或其他类型的对象进行排序，除非该类定义了足够多的方法`__lt__()`、`__le__()`、`__gt__()`和`__ge__()`(通常，`__lt__()`和`__eq__()`就足够了，如果你想要比较操作符的常规含义的话)。

不能自定义`is`和`is not`运算符的行为；此外，它们可以应用于任何两个对象，永远不会引发异常。

我们也可以将`<`和`>`操作符链接在一起。例如，`3 < 4 < 5`会返回`True`，而`3 < 4 > 5`不会。我们还可以链接等式运算符。例如，`3 == 3 < 5`将返回`True`，而`3 == 5 < 5`不会。

### **相等比较-"是" vs "=="**

在 Python 中，有两个比较运算符，允许我们检查两个对象是否相等。`is`操作员和`==`操作员。但是，它们之间有一个关键的区别！

“is”和“==”之间的主要区别可以总结为:

*   `is`用来比较 ****身份****
*   `==`用来比较 ****的平等****

## **例子**

首先，用 Python 创建一个列表。

```
myListA = [1,2,3]
```

接下来，创建该列表的副本。

```
myListB = myListA
```

如果我们使用' == '操作符或' is '操作符，两者都会产生一个 ****真**** 输出。

```
>>> myListA == myListB # both lists contains similar elements
True
>>> myListB is myListA # myListB contains the same elements
True
```

这是因为 myListA 和 myListB 都指向同一个列表变量，这是我在 Python 程序开始时定义的。两份名单完全一样，无论是身份还是内容。

但是，如果我现在创建一个新列表呢？

```
myListC = [1,2,3]
```

执行`==`操作符仍然显示两个列表在内容上是相同的。

```
>>> myListA == myListC
True
```

然而，执行`is`操作符现在将产生一个`False`输出。这是因为 myListA 和 myListC 是两个不同的变量，尽管包含相同的数据。即使他们看起来一样，他们是**T3**T5【不同】。

```
>>> myListA is myListC
False # both lists have different reference
```

总而言之:

*   如果两个变量指向同一个引用，一个`is`表达式输出`True`
*   如果两个变量包含相同的数据，一个`==`表达式输出`True`

# Python 字典示例

python 中的字典(又名“dict”)是一种内置的数据类型，可用于存储 ****`key-value`**** 对。这允许你把一个 ****`dict`**** 当作一个*数据库*来存储和组织数据。

字典的特别之处在于它们的实现方式。类似哈希表的结构使得检查存在性变得容易——这意味着我们可以容易地确定特定的键是否存在于字典中，而不需要检查每个元素。Python 解释器可以直接找到 location 键并检查该键是否在那里。

字典可以使用几乎任何任意的数据类型作为关键字，比如字符串、整数等等。然而，不可散列的值，即包含列表、字典或其他可变类型的值(通过值而不是通过对象标识进行比较)不能用作键。用于键的数字类型遵循数字比较的常规规则:如果两个数字比较相等(例如`1`和`1.0`)，那么它们可以互换使用来索引同一个字典条目。(但是请注意，由于计算机将浮点数存储为近似值，因此将它们用作字典键通常是不明智的。)

字典的一个最重要的要求是键 ****必须**** 是唯一的。

要创建一个空字典，只需使用一对大括号:

```
 >>> teams = {}
    >>> type(teams)
    >>> <class 'dict'>
```

要创建包含一些初始值的非空字典，请放置一个以逗号分隔的键值对列表:

```
 >>> teams = {'barcelona': 1875, 'chelsea': 1910}
    >>> teams
    {'barcelona': 1875, 'chelsea': 1910}
```

向现有字典添加键值对很容易:

```
 >>> teams['santos'] = 1787
    >>> teams
    {'chelsea': 1910, 'barcelona': 1875, 'santos': 1787} # Notice the order - Dictionaries are unordered !
    >>> # extracting value - Just provide the key
    ...
    >>> teams['barcelona']
    1875
```

****`del`**** 操作符用于从字典中删除一个键值对。在已经使用的键再次用于存储值的情况下，与该键关联的旧值会完全丢失。此外，请记住，使用不存在的键提取值是错误的。

```
 >>> del teams['santos']
    >>> teams
    {'chelsea': 1910, 'barcelona': 1875}
    >>> teams['chelsea'] = 2017 # overwriting    
    >>> teams
    {'chelsea': 2017, 'barcelona': 1875}
```

****`in`**** 关键字可以用来检查关键字是否存在于字典中:

```
 >>> 'sanots' in teams
    False    
    >>> 'barcelona' in teams
    True
    >>> 'chelsea' not in teams
    False
```

****`keys`**** 是一个内置的*方法*，可以用来获取给定字典的键。要将字典中的关键字提取为列表:

```
 >>> club_names = list(teams.keys())    
    >>> club_names
    ['chelsea', 'barcelona']
```

还有另一种创建字典的方法是使用 ****`dict()`**** 的方法:

```
 >>> players = dict( [('messi','argentina'), ('ronaldo','portugal'), ('kaka','brazil')] ) # sequence of key-value pair is passed  
    >>> players
    {'ronaldo': 'portugal', 'kaka': 'brazil', 'messi': 'argentina'}
    >>> 
    >>> # If keys are simple strings, it's quite easier to specify pairs using keyword arguments
    ...
    >>> dict( totti = 38, zidane = 43 )
    {'zidane': 43, 'totti': 38}
```

Dict comprehensions 也可以用于从任意键和值表达式创建字典:

```
 >>> {x: x**2 for x in (2, 4, 6)}
    {2: 4, 4: 16, 6: 36}
```

****在字典中循环****
简单地循环字典中的键，而不是键和值:

```
 >>> d = {'x': 1, 'y': 2, 'z': 3} 
    >>> for key in d:
    ...     print(key) # do something
    ...
    x
    y
    z
```

要循环访问键和值，可以使用下面的:
For Python 2.x:

```
 >>> for key, item in d.iteritems():
    ...     print items
    ...
    1
    2
    3
```

对于 Python 3.x 使用 ****`items()`**** :

```
 >>> for key, item in d.items():
    ...     print(key, items)
    ...
    x 1
    y 2
    z 3
```

# Python 对象示例

在 Python 中，一切都是一个*对象*。

*对象*代表属性的逻辑分组。属性是数据和/或函数。当在 Python 中创建一个对象时，它是用一个*身份*、*类型*和*值*创建的。

在其他语言中，*原语*是没有*属性*(属性)的*值*。比如 javascript 中的`undefined`、`null`、`boolean`、`string`、`number`、`symbol`(ECMAScript 2015 新增)都是原语。

在 Python 中，没有原语。`None`、*布尔*、*字符串*、*数字*，甚至*函数*无论如何创建，都是*对象*。

我们可以使用一些内置函数来演示这一点:

*   [T2`id`](https://docs.python.org/3/library/functions.html#id)
*   [T2`type`](https://docs.python.org/3/library/functions.html#type)
*   [T2`dir`](https://docs.python.org/3/library/functions.html#dir)
*   [T2`issubclass`](https://docs.python.org/3/library/functions.html#issubclass)

内置常数`None`、`True`、`False`是*对象*:

我们在这里测试`None`对象。

```
>>> id(None)
4550218168
>>> type(None)
<class 'NoneType'>
>>> dir(None)
[__bool__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
>>> issubclass(type(None), object)
True
```

接下来，我们来考察一下`True`。

```
>>> id(True)
4550117616
>>> type(True)
<class 'bool'>
>>> dir(True)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
>>> issubclass(type(True), object)
True
```

没有理由漏掉`False`！

```
>>> id(False)
4550117584
>>> type(False)
<class 'bool'>
>>> dir(False)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
>>> issubclass(type(False), object)
True
```

*字符串*，即使是由字符串文字创建的，也是*对象*。

```
>>> id("Hello campers!")
4570186864
>>> type('Hello campers!')
<class 'str'>
>>> dir("Hello campers!")
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
>>> issubclass(type('Hello campers!'), object)
True
```

与*号相同。*

```
>>> id(42)
4550495728
>>> type(42)
<class 'int'>
>>> dir(42)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
>>> issubclass(type(42), object)
True
```

## **函数也是对象**

在 Python 中，函数是第一类对象。

Python 中的*函数*也是*对象*，用*身份*、*类型*和*值*创建。它们也可以传递给其他*函数*:

```
>>> id(dir)
4568035688
>>> type(dir)
<class 'builtin_function_or_method'>
>>> dir(dir)
['__call__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__name__', '__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__self__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__text_signature__']
>>> issubclass(type(dir), object)
True
```

也可以将函数绑定到一个名称，并使用该名称调用绑定的函数:

```
>>> a = dir
>>> print(a)
<built-in function dir>
>>> a(a)
['__call__', '__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__le__', '__lt__', '__module__', '__name__', '__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__self__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__text_signature__']
```

## 名称绑定和别名功能

在 Python 中定义函数时，该函数名会被引入到当前符号表中。然后，函数名的值的类型被解释器识别为用户定义的函数:

```
>>> something = 1
>>> type(something)
<type 'int'>

>>> def something():
...     pass
...
>>> type(something)
<type 'function'>

>>> something = []
>>> type(something)
<type 'list'>
```

然后可以将函数值赋给另一个名称。在它被重新分配后，它仍然可以作为一个函数使用。使用此方法重命名您的函数:

```
>>> def something(n):
...     print(n)
...
>>> type(something)
<type 'function'>

>>> s = something
>>> s(100)
100
```

# Python 元组

元组是 Python 对象的序列。元组是不可变的，这意味着它们在创建后不能被修改，这与列表不同。

****创作:****

使用一对圆括号`()`创建一个空的`tuple`:

```
 >>> empty_tuple = ()
    >>> print(empty_tuple)
    ()
    >>> type(empty_tuple)
    <class 'tuple'>
    >>> len(empty_tuple)
    0
```

通过用逗号分隔元素来创建带有元素的`tuple`(圆括号`()`是可选的，但有例外):

```
 >>> tuple_1 = 1, 2, 3       # Create tuple without round brackets.
    >>> print(tuple_1)
    (1, 2, 3)
    >>> type(tuple_1)
    <class 'tuple'>
    >>> len(tuple_1)
    3
    >>> tuple_2 = (1, 2, 3)     # Create tuple with round brackets.
    >>> print(tuple_2)
    (1, 2, 3)
    >>> tuple_3 = 1, 2, 3,      # Trailing comma is optional.
    >>> print(tuple_3)
    (1, 2, 3)
    >>> tuple_4 = (1, 2, 3,)    # Trailing comma in round brackets is also optional.
    >>> print(tuple_4)
    (1, 2, 3)
```

具有单个元素的`tuple`必须有尾随逗号(有或没有圆括号):

```
>>> not_tuple = (2)    # No trailing comma makes this not a tuple.
>>> print(not_tuple)
2
>>> type(not_tuple)
<class 'int'>
>>> a_tuple = (2,)     # Single element tuple. Requires trailing comma.
>>> print(a_tuple)
(2,)
>>> type(a_tuple)
<class 'tuple'>
>>> len(a_tuple)
1
>>> also_tuple = 2,    # Round brackets omitted. Requires trailing comma.
>>> print(also_tuple)
(2,)
>>> type(also_tuple)
<class 'tuple'>
```

在不明确的情况下需要圆括号(如果元组是更大表达式的一部分):

注意，实际上是逗号构成了元组，而不是括号。括号是可选的，除了空元组的情况，或者当需要它们来避免语法歧义时。

例如，`f(a, b, c)`是一个带有三个参数的函数调用，而`f((a, b, c))`是一个带有一个 3 元组作为唯一参数的函数调用。

```
 >>> print(1,2,3,4,)          # Calls print with 4 arguments: 1, 2, 3, and 4
    1 2 3 4
    >>> print((1,2,3,4,))        # Calls print with 1 argument: (1, 2, 3, 4,)
    (1, 2, 3, 4)
    >>> 1, 2, 3 == (1, 2, 3)     # Equivalent to 1, 2, (3 == (1, 2, 3))
    (1, 2, False)
    >>> (1, 2, 3) == (1, 2, 3)   # Use surrounding round brackets when ambiguous.
    True
```

也可以用`tuple`构造函数创建一个`tuple`:

```
 >>> empty_tuple = tuple()
    >>> print(empty_tuple)
    ()
    >>> tuple_from_list = tuple([1,2,3,4])
    >>> print(tuple_from_list)
    (1, 2, 3, 4)
    >>> tuple_from_string = tuple("Hello campers!")
    >>> print(tuple_from_string)
    ('H', 'e', 'l', 'l', 'o', ' ', 'c', 'a', 'm', 'p', 'e', 'r', 's', '!')
    >>> a_tuple = 1, 2, 3
    >>> b_tuple = tuple(a_tuple)    # If the constructor is called with a tuple for
    the iterable,
    >>> a_tuple is b_tuple          # the tuple argument is returned.
    True
```

****访问元素一`tuple` :****

`tuples`的元素的访问和索引方式与`lists`相同。

```
>>> my_tuple = 1, 2, 9, 16, 25
>>> print(my_tuple)
(1, 2, 9, 16, 25)
```

*零索引*

```
 >>> my_tuple[0]
    1
    >>> my_tuple[1]
    2
    >>> my_tuple[2]
    9
```

*环绕分度*

```
 >>> my_tuple[-1]
    25
    >>> my_tuple[-2]
    16
```

****装箱拆箱:****

语句`t = 12345, 54321, 'hello!'`是元组打包的一个例子:值`12345`、`54321`和`'hello!'`一起打包在一个元组中。相反的操作也是可能的:

```
 >>> x, y, z = t
```

这被恰当地称为序列解包，适用于右侧的任何序列。序列解包要求等号左边的变量和序列中的元素一样多。注意，多重赋值实际上只是元组打包和序列解包的组合。

```
 >>> t = 1, 2, 3    # Tuple packing.
    >>> print(t)
    (1, 2, 3)
    >>> a, b, c = t    # Sequence unpacking.
    >>> print(a)
    1
    >>> print(b)
    2
    >>> print(c)
    3
    >>> d, e, f = 4, 5, 6    # Multiple assignment combines packing and unpacking.
    >>> print(d)
    4
    >>> print(e)
    5
    >>> print(f)
    6
    >>> a, b = 1, 2, 3       # Multiple assignment requires each variable (right)
    have a matching element (left).
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    ValueError: too many values to unpack (expected 2)
```

****不可变:****

`tuples`是不可变的容器，保证它们包含的**对象不会改变。它不 ****而**** 保证它们包含的对象不会改变:**

```
 `>>> a_list = []
    >>> a_tuple = (a_list,)    # A tuple (immutable) with a list (mutable) element.
    >>> print(a_tuple)
    ([],)

    >>> a_list.append("Hello campers!")
    >>> print(a_tuple)         # Element of the immutable is mutated.
    (['Hello campers!'],)`
```

******用途:******

**函数只能返回一个值，然而，一个异构的`tuple`可以用来从一个函数返回多个值。一个例子是内置的`enumerate`函数，它返回一个异构`tuples`的 iterable:**

```
 `>>> greeting = ["Hello", "campers!"]
    >>> enumerator = enumerate(greeting)
    >>> enumerator.next()
    >>> enumerator.__next__()
    (0, 'Hello')
    >>> enumerator.__next__()
    (1, 'campers!')`
```

# **Python For 循环语句示例**

**Python 利用 for 循环来迭代元素列表。这与 C 或 Java 不同，C 或 Java 使用 for 循环逐步更改值，并使用该值访问数组之类的东西。**

**For 循环遍历基于集合的数据结构，如列表、元组和字典。**

**基本语法是:**

```
`for value in list_of_values:
  # use value inside this block`
```

**一般来说，您可以使用任何东西作为迭代器值，其中 iterable 的条目可以被赋值给。例如，您可以从元组列表中解包元组:**

```
`list_of_tuples = [(1,2), (3,4)]

for a, b in list_of_tuples:
  print("a:", a, "b:", b)`
```

**另一方面，你可以循环任何可迭代的东西。您可以调用函数或使用列表文字。**

```
`for person in load_persons():
  print("The name is:", person.name)`
```

```
`for character in ["P", "y", "t", "h", "o", "n"]:
  print("Give me a '{}'!".format(character))`
```

**使用 For 循环的一些方式:**

******迭代范围()函数******

```
`for i in range(10):
    print(i)`
```

**range 不是一个函数，它实际上是一个不可变的序列类型。输出将包含从下限(即 0)到上限(即 10)的结果，但不包括 10。默认情况下，下限或起始索引设置为零。输出:**

```
`>
0
1
2
3
4
5
6
7
8
9
>`
```

**此外，可以通过添加第二个和第三个参数来指定序列的下限，甚至序列的步长。**

```
`for i in range(4,10,2): #From 4 to 9 using a step of two
    print(i)`
```

**输出:**

```
`>
4
6
8
>`
```

******xrange()函数******

**在很大程度上，xrange 和 range 在功能上是完全相同的。它们都提供了一种生成整数列表的方法，供您随意使用。唯一的区别是 range 返回一个 Python 列表对象，而 xrange 返回一个 xrange 对象。这意味着 xrange 并不像 range 那样在运行时生成静态列表。它用一种叫做让步的特殊技术创造出你需要的价值。这种技术用于一种称为发生器的对象。**

**再补充一点。在 Python 3.x 中，xrange 函数不再存在。range 函数现在做 Python 2.x 中 xrange 做的事情**

******迭代列表或元组中的值******

```
`A = ["hello", 1, 65, "thank you", [2, 3]]
for value in A:
    print(value)`
```

**输出:**

```
`>
hello
1
65
thank you
[2, 3]
>`
```

```
**`fruits_to_colors = {"apple": "#ff0000",
                    "lemon": "#ffff00",
                    "orange": "#ffa500"}

for key in fruits_to_colors:
    print(key, fruits_to_colors[key])`**
```

****输出:****

```
**`>
apple #ff0000
lemon #ffff00
orange #ffa500
>`**
```

********用 zip()函数**** 在一个循环中迭代两个相同大小的列表****

```
`A = ["a", "b", "c"]
B = ["a", "d", "e"]

for a, b in zip(A, B):
  print a, b, a == b` 
```

**输出:**

```
`>
a a True
b d False
c e False
>`
```

******遍历一个列表，用 enumerate()函数**** 得到对应的索引**

```
`A = ["this", "is", "something", "fun"]

for index,word in enumerate(A):
    print(index, word)`
```

**输出:**

```
`>
0 this
1 is
2 something
3 fun
>`
```

**一个常见的用例是迭代字典:**

```
`for name, phonenumber in contacts.items():
  print(name, "is reachable under", phonenumber)`
```

**如果您绝对需要访问您的迭代的当前索引，请使用 ****而不是****`range(len(iterable))`！这是一种极其糟糕的做法，会让你遭到资深 Python 开发者的嘲笑。请改用内置函数`enumerate()`:**

```
`for index, item in enumerate(shopping_basket):
  print("Item", index, "is a", item)`
```

******for/else 语句**** Pyhton 允许在 for 循环中使用 else，当循环体中的所有条件都不满足时，就会执行 else 情况。要使用 else，我们必须使用`break`语句，这样我们就可以在满足条件的情况下跳出循环。如果我们不越狱，那么 else 部分将被执行。**

```
`week_days = ['Monday','Tuesday','Wednesday','Thursday','Friday']
today = 'Saturday'
for day in week_days:
  if day == today:
    print('today is a week day')
    break
else:
  print('today is not a week day')`
```

**在上面的例子中，输出将是`today is not a week day`,因为循环中的 break 永远不会被执行。**

******使用内联循环函数**** 遍历列表**

**我们也可以使用 python 进行内联迭代。例如，如果我们需要将列表中的所有单词大写，我们可以简单地执行以下操作:**

```
`A = ["this", "is", "awesome", "shinning", "star"]

UPPERCASE = [word.upper() for word in A]
print (UPPERCASE)`
```

**输出:**

```
`>
['THIS', 'IS', 'AWESOME', 'SHINNING', 'STAR']`
```

# **Python 函数示例**

**函数允许你定义一个可重用的代码块，它可以在你的程序中多次执行。**

**函数允许你为复杂的问题创建更多的模块和[干](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)的解决方案。**

**虽然 Python 已经提供了许多内置函数，比如`print()`和`len()`，但是您也可以定义自己的函数在项目中使用。**

**在代码中使用函数的一个很大的好处是，它减少了项目中代码的总行数。**

### ****语法****

**在 Python 中，函数定义具有以下特性:**

1.  **关键词`def`**
2.  **函数名**
3.  **括号'()'，并在括号内输入参数，尽管输入参数是可选的。**
4.  **冒号':'**
5.  **一些要执行的代码块**
6.  **返回语句(可选)**

```
`# a function with no parameters or returned values
def sayHello():
  print("Hello!")

sayHello()  # calls the function, 'Hello!' is printed to the console

# a function with a parameter
def helloWithName(name):
  print("Hello " + name + "!")

helloWithName("Ada")  # calls the function, 'Hello Ada!' is printed to the console

# a function with multiple parameters with a return statement
def multiply(val1, val2):
  return val1 * val2

multiply(3, 5)  # prints 15 to the console`
```

**函数是代码块，只需调用函数就可以重用。这使得简单、优雅的代码重用成为可能，而无需显式地重写代码部分。这使得代码更具可读性，更易于调试，并减少了键入错误。**

**Python 中的函数是使用`def`关键字创建的，后跟括号内的函数名和函数参数。**

**函数总是返回值。函数使用关键字`return`来返回值。如果不想返回任何值，就返回默认值`None`。**

**函数名用于调用函数，在括号内传递所需的参数。**

```
`# this is a basic sum function
def sum(a, b):
  return a + b

result = sum(1, 2)
# result = 3`
```

**您可以为参数定义默认值，如果没有给定，Python 会将该参数的值解释为默认值。**

```
`def sum(a, b=3):
  return a + b

result = sum(1)
# result = 4`
```

**您可以使用参数的名称，按照您想要的顺序传递参数。**

```
`result = sum(b=2, a=2)
# result = 4`
```

**但是，不可能在非关键字参数之前传递关键字参数。**

```
`result = sum(3, b=2)
#result = 5
result2 = sum(b=2, 3)
#Will raise SyntaxError`
```

**函数也是对象，所以你可以把它们赋给一个变量，然后像函数一样使用这个变量。**

```
`s = sum
result = s(1, 2)
# result = 3`
```

### ****注释****

**如果函数定义包含参数，则在调用函数时必须提供相同数量的参数。**

```
`print(multiply(3))  # TypeError: multiply() takes exactly 2 arguments (0 given)

print(multiply('a', 5))  # 'aaaaa' printed to the console

print(multiply('a', 'b'))  # TypeError: Python can't multiply two strings`
```

**函数将运行的代码块包括函数内缩进的所有语句。**

```
`def myFunc():
print('this will print')
print('so will this')

x = 7
# the assignment of x is not a part of the function since it is not indented`
```

**函数中定义的变量只存在于该函数的范围内。**

```
`def double(num):
x = num * 2
return x

print(x)  # error - x is not defined
print(double(4))  # prints 8`
```

**Python 只在调用函数时解释函数块，而不是在定义函数时解释。因此，即使函数定义块包含某种错误，python 解释器也只会在调用函数时指出来。**

# **Python 生成器示例**

**生成器是一种特殊类型的函数，允许您在不结束函数的情况下返回值。它通过使用`yield`关键字来实现这一点。类似于`return`,`yield`表达式将返回一个值给调用者。两者的关键区别在于`yield`将暂停函数，允许将来返回更多的值。**

**生成器是可迭代的，因此它们可以干净地用于 for 循环或任何迭代。**

```
`def my_generator():
    yield 'hello'
    yield 'world'
    yield '!'

for item in my_generator():
    print(item)

# output:
# hello
# world
# !`
```

**像其他迭代器一样，生成器可以传递给`next`函数来检索下一项。当生成器没有更多的值要生成时，就会产生一个`StopIteration`错误。**

```
`g = my_generator()
print(next(g))
# 'hello'
print(next(g))
# 'world'
print(next(g))
# '!'
print(next(g))
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# StopIteration`
```

**当您需要创建大量的值，但不需要同时将它们保存在内存中时，生成器特别有用。例如，如果您需要打印前一百万个斐波那契数，您通常会返回一个包含一百万个值的列表，并迭代该列表以打印每个值。然而，使用生成器，您可以一次返回一个值:**

```
`def fib(n):
    a = 1
    b = 1
    for i in range(n):
        yield a
        a, b = b, a + b

for x in fib(1000000):
    print(x)`
```

# **Python 迭代器示例**

**Python 支持容器迭代的概念。这是使用两种不同的方法实现的；这些用于允许用户定义的类支持迭代。**

**[Python 文档-迭代器类型](https://docs.python.org/3/library/stdtypes.html#iterator-types)**

**迭代是以编程方式将一个步骤重复给定次数的过程。程序员可以利用迭代对数据集合中的每一项执行相同的操作，例如打印出列表中的每一项。**

*   **对象可以实现一个返回迭代器对象的`__iter__()`方法来支持迭代。**

**迭代器对象必须实现:**

*   **`__iter__()`:返回迭代器对象。**
*   **`__next__()`:返回容器的下一个对象，迭代器 *object = 'abc '。 ****iter**** () print(迭代器*对象)print(id(迭代器*对象))print(id(迭代器*对象。 ****iter**** ())) #返回迭代器本身。打印(迭代器*对象。 ****下一个**** ()) #返回第一个对象并推进迭代器。打印(迭代器*对象。 ****下一个**** ()) #返回第二个对象并推进迭代器。打印(迭代器*对象。 ****下一个**** ()) #返回第三个对象并推进迭代器。打印(迭代器*对象。 ****下一个**** ()) #引发 StopIteration 异常。**

**输出:**

```
`<str_iterator object at 0x102e196a0>
4343305888
4343305888
a
b
c
---------------------------------------------------------------------------
StopIteration                             Traceback (most recent call last)
<ipython-input-1-d466eea8c1b0> in <module>()
      6 print(iterator_object.__next__())     # Returns 2nd object and advances iterator.
      7 print(iterator_object.__next__())     # Returns 3rd object and advances iterator.
----> 8 print(iterator_object.__next__())     # Raises StopIteration Exception.

StopIteration:`
```

# ****Python 示例中的三元运算符****

**Python 中的三元运算，通常也称为条件表达式，允许程序员执行评估并根据给定条件的真值返回值。**

**三元运算符不同于标准的`if`、`else`、`elif`结构，因为它不是控制流结构，其行为更像其他运算符，如 Python 语言中的`==`或`!=`。**

### ****例子****

**在这个例子中，如果变量`val`是偶数，则返回字符串`Even`，否则返回字符串`Odd`。然后将返回的字符串赋给`is_even`变量并打印到控制台。**

#### ****输入****

```
`for val in range(1, 11):
    is_even = "Even" if val % 2 == 0 else "Odd"
    print(val, is_even, sep=' = ')`
```

#### ****输出****

```
`1 = Odd
2 = Even
3 = Odd
4 = Even
5 = Odd
6 = Even
7 = Odd
8 = Even
9 = Odd
10 = Even`
```

# **Python While 循环语句示例**

**Python 与其他流行语言一样利用了`while`循环。`while`循环评估一个条件，如果条件为真，则执行一段代码。代码块重复执行，直到条件变为假。**

**基本语法是:**

```
`counter = 0
while counter < 10:
   # Execute the block of code here as
   # long as counter is less than 10`
```

**下面显示了一个示例:**

```
`days = 0    
week = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
while days < 7:
   print("Today is " + week[days])
   days += 1`
```

**输出:**

```
`Today is Monday
Today is Tuesday
Today is Wednesday
Today is Thursday
Today is Friday
Today is Saturday
Today is Sunday`
```

**以上代码的逐行解释:**

1.  **变量“天数”被设置为值 0。**
2.  **变量 week 被分配给包含一周中所有日期的列表。**
3.  **当循环开始时**
4.  **代码块将被执行，直到条件返回“真”。**
5.  **条件是“天数< 7”，大致意思是运行 while 循环，直到变量天数小于 7**
6.  **因此，当 days=7 时，while 循环停止执行。**
7.  **days 变量在每次迭代中都会更新。**
8.  **当 while 循环第一次运行时，控制台上会显示一行“今天是星期一”,变量 days 等于 1。**
9.  **因为变量 days 等于小于 7 的 1，所以再次执行 while 循环。**
10.  **当控制台打印出“今天是星期天”时，变量 days 现在等于 7，while 循环停止执行。**

## **Python 中的 f 字符串**

**在 Python 版中，实现了一种格式化字符串的新方法。这种新方法被称为文字字符串插值(虽然通常被称为 f 字符串)。**

**f-string 的使用允许程序员以干净简洁的方式动态地将变量插入到字符串中。除了在字符串中插入变量之外，这个特性还为程序员提供了计算表达式、连接集合内容，甚至在 f 字符串中调用函数的能力。**

**为了在 f 字符串中执行这些动态行为，我们将它们放在字符串中的花括号内，并在字符串的开头加上小写的 f(在左引号之前)。**

### ****例题****

**在运行时将变量动态插入字符串:**

```
`name = 'Jon Snow'
greeting = f'Hello! {name}'
print(greeting)`
```

**计算字符串中的表达式:**

```
`val1 = 2
val2 = 3
expr = f'The sum of {val1} + {val2} is {val1 + val2}'
print(expr)`
```

**调用函数并在字符串中插入输出:**

```
`def sum(*args):
    result = 0
    for arg in args:
        result += arg
    return result

func = f'The sum of 3 + 5 is {sum(3, 5)}'
print(func)`
```

**将集合的内容连接到字符串中:**

```
`fruits = ['Apple', 'Banana', 'Pear']

list_str = f'List of fruits: {", ".join(fruits)}'
print(list_str)`
```

# **如何安装 Python 3**

**可以从这个官方[链接](https://www.python.org/downloads/)下载 Python。基于你的操作系统(Windows 或 Linux 或 OSX)，你可能想按照这些指令[安装 Python 3。](http://docs.python-guide.org/en/latest/starting/installation/)**

## ****使用虚拟环境****

**用沙箱保护你的 Python 安装并把它与你的 T2 系统 Python 分开总是一个好主意。*系统 Python* 是 Python 解释器的路径，随操作系统一起安装的其他模块会用到它。**

**直接用*系统 Python* 安装 Python Web-frames 或库**不安全。相反，在开发 Python 应用程序时，您可以使用 [Virtualenv](https://virtualenv.readthedocs.org/en/latest/) 来创建和派生一个单独的 Python 进程。****

### ****虚拟包装****

****[Virtualenvwrapper 模块](https://virtualenvwrapper.readthedocs.org/en/latest/)使您可以轻松地在一台机器上管理和运行多个 Python 沙盒环境，而不会破坏任何用 Python 编写并由您的机器使用的模块或服务。****

****当然，大多数云托管的开发环境，如 [Nitrous](https://www.nitrous.io/) 或 [Cloud9](https://c9.io/) 也预装了这些，并为您获取编码做好了准备！激活 Python 3 环境后，您可以从仪表板中快速选择一个框并开始编码。****

****在 [Cloud9](https://c9.io/) 中，创建新的开发环境时需要选择 Django 框。****

****下面是几个 shell 命令示例。如果您希望复制粘贴，请注意,`$`符号是终端提示符的简写，它不是命令的一部分。我的终端提示符看起来像这样:****

**
```

****并且，一个`ls`看起来像****

```
**`alayek:~/workspace (master) $ ls`**
```

****但是，在这个文档中写同样的内容时，我会把它写成****

```
**`$ ls`**
```

****回到我们的讨论，您可以通过在您的云终端上运行，在 Cloud9 上创建一个包含 Python 3 解释器的沙箱:****

```
**`$ mkvirtualenv py3 --python=/usr/bin/python3`**
```

****您只需在为项目创建新盒子后运行一次。一旦执行，这个命令将创建一个新的沙盒 virtualenv 供您使用，命名为`py3`。****

****要查看可用的虚拟环境，您可以使用****

```
**`$ workon`**
```

****要激活`py3`，您可以使用带有环境名称的`workon`命令:****

```
**`$ workon py3`**
```

****以上所有三个终端命令也可以在本地 Linux 机器或 OSX 机器上运行。这些是[虚拟包装器](https://virtualenvwrapper.readthedocs.org/en/latest/#introduction)命令；所以如果你打算使用它们，确保你已经安装了这个模块并添加到了`PATH`变量中。****

****如果你在虚拟环境中；通过检查您的终端提示符，您可以很容易地发现这一点。环境名称将清楚地显示在您的终端提示符中。****

****例如，当我在`py3`环境中时，我会看到这是我的终端提示符:****

**
```

****注意括号里的`(py3)`！如果由于某种原因，你没有看到这一点，即使你是在一个虚拟的环境；你可以尝试做这里提到的事情之一。****

****要退出虚拟环境或停用虚拟环境，请使用以下命令:****

```
**`$ deactivate`**
```

****同样，这只适用于 virtualenvwrapper 模块。****

### ******Pipenv******

****使用 virtualenvwrapper 的替代方法是 [Pipenv](https://docs.pipenv.org/) 。它自动为您的项目创建虚拟环境，并维护一个包含依赖关系的`Pipfile`。使用 Pipenv 意味着你不再需要单独使用 pip 和 virtualenv，或者管理你自己的`requirements.txt`文件。对于熟悉 JavaScript 的人来说，Pipenv 类似于使用`npm`这样的打包工具。****

****要开始使用 Pipenv，您可以遵循这个非常详细的[指南](https://docs.pipenv.org/install.html#installing-pipenv)。Pipenv 使得[指定您希望为每个项目使用哪个版本的 Python](https://docs.pipenv.org/basics.html#specifying-versions-of-python) ，[从现有的`requirements.txt`文件导入](https://docs.pipenv.org/basics.html#importing-from-requirements-txt)并[绘制](https://docs.pipenv.org/#pipenv-graph)您的依赖关系变得容易。****