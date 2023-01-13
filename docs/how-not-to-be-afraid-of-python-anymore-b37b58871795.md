# 如何不再害怕 Python

> 原文：<https://www.freecodecamp.org/news/how-not-to-be-afraid-of-python-anymore-b37b58871795/>

#### 深入研究语言参考文档

在我开始编码的头一两年，我认为学习语言就是学习语法。所以，我就做了这么多。

不用说，我没有成为一个伟大的开发者。我被卡住了。然后，在一个晴朗的日子里，它突然灵光一现。我意识到我做错了。学习语法应该是我最不关心的事情。重要的是语言的其他方面。这到底是什么？请继续阅读。

本文分为三个主要部分:数据模型、执行模型和词法分析。

与如何学习 Python 相比，本文更深入地探讨了 Pythonland 的工作原理。你会在网上找到许多如何学习的资源。

我在网上没有找到的是 Python 中常见“陷阱”的单一来源。解释语言如何工作的来源。本文试图解决这个问题。我想我做得不够，有太多的东西了！

这里的一切都来自官方文档。我把它浓缩了——浓缩到重要的地方，重新排序，并增加了我的例子。所有链接都指向文档。

事不宜迟，我们开始吧。

### 数据模型

#### 对象、值和类型

[对象](https://docs.python.org/3.7/reference/datamodel.html#objects-values-and-types)是 Python 对数据的抽象。

每个物体都有它唯一的固定`identity`，一个固定`type`和一个`value`。

‘固定’意味着`Object`的`identity`和`type`永远不能改变。

`value`可能会改变。值可以改变的对象称为**可变的**，值不能改变的对象称为**不可变的**。

可变性由`type`决定:

*   数字、字符串和元组是不可变的
*   列表和字典是可变的

对象的身份可以通过`is`操作符进行比较。

`id()`返回`identity`

`type()`返回`type`

> 注意:当可变对象的值改变时，包含可变对象的引用的不可变容器对象的值可以改变。然而，容器仍然被认为是不可变的，因为它包含的对象集合不能被改变。所以，严格来说，不变性并不等同于拥有一个不变的值。

我头两次读到这张纸条时，觉得头昏脑胀。

简单翻译:不变性不等同于不变的价值。在下面的例子中，`tuple`是`immutable`，而它的`value`保持变化(随着`list`的变化)。

示例:

```
>>> t = ("a", [1]) # a tuple of string and list
>>> id(t)
4372661064
>>> t
('a', [1])
>>> type(t)
<class 'tuple'>
>>> t[1]
[1]
>>> t[1].append(2)
>>> t
('a', [1, 2])
>>> id(t)
4372661064
>>> type(t)
<class 'tuple'>
```

元组是不可变的，即使它包含一个可变对象，一个列表。

与字符串相比，改变现有数组会改变对象(因为字符串是不可变的)。

```
>>> x = "abc"
>>> id(x)
4371437472
>>> x += "d"
>>> x
'abcd'
>>> id(x)
4373053712
```

在这里，名称`x`被绑定到另一个 string 类型的对象。这也改变了它的 id。

不可变的原始对象保持不变。下面将更详细地解释绑定，这会让事情变得更清楚。

#### 内置类型

Python 自带了几个[内置类型](https://docs.python.org/3.7/reference/datamodel.html#the-standard-type-hierarchy):

#### 没有人

该类型由单个对象表示，因此是单个值。唯一的对象有`type = NoneType`

```
>>> type(None)
<class 'NoneType'>
```

#### 数字

这是用于表示数字的抽象基类的集合。它们不能被实例化，`int`，`float`继承自`numbers.Number`。

它们是由数值和算术运算创建的。正如我们所见，返回的对象是不可变的。以下列举的例子将使这一点更加清楚:

```
>>> a = 3 + 4
>>> type(a)
<class 'int'>
>>> isinstance(a, numbers.Number)
True
>>> isinstance(a, numbers.Integral)
True
>>> isinstance(3.14 + 2j, numbers.Real)
False
>>> isinstance(3.14 + 2j, numbers.Complex)
True
```

#### 顺序

这些表示由非负整数索引的有限有序集。就像其他语言中的数组一样。

`len()`返回序列的长度。当长度为`n`时，索引集包含来自`0...n-1`的元素。然后通过`seq[i-1]`选择第 I 个元素。

对于一个序列`l`，您可以使用 slicing: `l[i:j]`选择索引之间的元素。

有两种类型的序列:可变的和不可变的。

*   不可变序列包括:字符串、元组和字节。
*   可变序列包括:列表和字节数组

#### 设置

这些表示唯一的、不可变的对象的无序的、有限的集合。它们不能被索引，但是可以被迭代。`len()`仍然返回集合中的项目数。

集合有两种类型:可变的和不可变的。

*   可变集合由`set()`创建。
*   不可变集合是由`frozenset()`创建的。

#### 绘图

#### 词典

这些表示由几乎任意值索引的有限对象集。键不能是可变对象。这包括列表、其他字典和其他通过值而不是通过对象标识进行比较的对象。

这意味着`frozenset`也可以是字典键！

#### **模块**

模块对象是 Python 中的基本组织单位。命名空间被实现为一个字典。属性引用是在这个字典中的查找。

对于模块`m`，字典是只读的，由`m.__dict__`访问。

这是一本普通的字典，所以你可以给它添加关键字！

这里有一个例子，关于 Python 的[禅:](https://www.python.org/dev/peps/pep-0020/)

我们将自定义函数`figure()`添加到模块`this`中。

```
>>> import this as t
>>> t.__dict__
{'__name__': 'this', '__doc__': None, '__package__': '',
.....
.....
's': "Gur Mra bs Clguba, ol Gvz Crgref\n\nOrnhgvshy vf orggre guna
vqrn.\nAnzrfcnprf ner bar ubaxvat terng vqrn -- yrg'f qb zber bs gubfr!",
'd': {'A': 'N', 'B': 'O', 'C': 'P', 'D': 'Q', 'E': 'R', 'F': 'S', 
'u': 'h', 'v': 'i', 'w': 'j', 'x': 'k', 'y': 'l', 'z': 'm'},
'c': 97,
'i': 25
}
>>> def figure():
...   print("Can you figure out the Zen of Python?")
... 
>>> t.fig = figure
>>> t.fig()
Can you figure out the Zen of Python?
>>> t.__dict__
{'__name__': 'this', '__doc__': None, '__package__': '',
.....
.....
's': "Gur Mra bs Clguba, ol Gvz Crgref\n\nOrnhgvshy vf orggre guna
vqrn.\nAnzrfcnprf ner bar ubaxvat terng vqrn -- yrg'f qb zber bs gubfr!",
'd': {'A': 'N', 'B': 'O', 'C': 'P', 'D': 'Q', 'E': 'R', 'F': 'S', 
'u': 'h', 'v': 'i', 'w': 'j', 'x': 'k', 'y': 'l', 'z': 'm'},
'c': 97,
'i': 25
'fig': <function figure at 0x109872620>
}
>>> print("".join([t.d.get(c, c) for c in t.s]))
The Zen of Python, by Tim Peters
Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

也不是很有用，但是值得一提。

#### 运算符重载

Python 允许[操作符重载](https://docs.python.org/3.7/reference/datamodel.html#special-method-names)。

类有特殊的函数名——它们可以实现使用 Python 定义的操作符的方法。这包括切片、算术运算和下标。

例如，`__getitem__()`指下标。因此，`x[i]`相当于`type(x).__getitem__(x,i)`。

因此，要在类`someClass`上使用操作符`[]`:您需要在`someClass`中定义`__getitem__()`。

```
>>> class operatorTest(object):
...     vals = [1,2,3,4]
...     def __getitem__(self, i):
...         return self.vals[i]
... 
>>> x = operatorTest()
>>> x[2]
3
>>> x.__getitem__(2)
3
>>> type(x)
<class '__main__.OperatorTest'>
>>> type(x).__getitem__(x,2)
3
>>> OperatorTest.__getitem__(x,2)
3
```

困惑为什么都是等价的？这是下一部分的内容——我们将讨论类和函数的定义。

同样，当对你的类的对象调用`str()`方法时，`__str__()` [函数](https://docs.python.org/3.7/reference/datamodel.html#object.__str__)决定输出。

对于比较操作，特殊函数名为:

*   `object.__lt__(self, other)`为`<`(“小于”)
*   `object.__le__(self, other)`为`≤`(“小于或等于”)
*   `object.__eq__(self, other)`为`==`(“等于”)
*   `object.__ne__(self, other)`为`!=`(“不等于”)
*   `object.__gt__(self, other)`为`>`(“大于”)
*   `object.__ge__(self, other)`为`≥`(“大于等于”)

例如，`x < y`被称为`x.__lt__` (y)

还有[算术运算的特殊函数](https://docs.python.org/3.7/reference/datamodel.html#emulating-numeric-types)，像`object.__add__(self, other)`。

作为一个例子，`x+y`被称为`x.__add__(y)`

另一个有趣的[功能](https://docs.python.org/3.7/reference/datamodel.html#object.__iter__)是`__iter__()`。

当你需要一个容器的迭代器时，你可以调用这个方法。它返回一个新的迭代器对象,可以遍历容器中的所有对象。

对于映射，它应该迭代容器的键。

迭代器对象本身支持两种方法:

*   `iterator.__iter__()`:返回对象本身。

这使得`iterators`和`containers`等价。

这允许迭代器和容器都在`for`和`in`语句中使用。

*   `iterator.__next__()`:返回容器中的下一个项目。如果没有进一步的项目，则引发`StopIteration`异常。

```
class IterableObject(object):    # The iterator object class
     vals = []
     it = 0
     def __init__(self, val):
         self.vals = val
         it = 0

     def __iter__(self):
         return self

     def __next__(self):
         if self.it < len(self.vals):
             index = self.it
             self.it += 1
             return self.vals[index]
         raise StopIteration

 class IterableClass(object):    # The container class
       vals = [1,2,3,4]

       def __iter__(self):
         return iterableObject(self.vals)
>>> iter_object_example = IterableObject([1,2,3])
>>> for val in iter_object_example:
...   print(val)
... 
1
2
3
>>> iter_container_example = IterableClass()
>>> for val in iter_container_example:
...  print(val)
... 
1
2
3
4
```

很酷的东西，对吧？Javascript 中也有直接的对等物。

[上下文管理器](https://docs.python.org/3.7/reference/datamodel.html#with-statement-context-managers)也是通过操作符重载实现的。

`with open(filename, 'r') as f`

`open(filename, 'r')`是一个上下文管理器对象，它实现

`object.__enter__(self)`和

`object.__exit__(self, exc_type, exc_value, traceback)`
当误差为`None`时，以上三个参数均为空。

```
class MyContextManager(object):
    def __init__(self, some_stuff):
        self.object_to_manage = some_stuff
    def __enter__(self):
        print("Entering context management")
        return self.object_to_manage # can do some transforms too

    def __exit__(self, exc_type, exc_value, traceback):
        if exc_type is None:
            print("Successfully exited")
            # Other stuff to close
>>> with MyContextManager("file") as f:
...     print(f)
... 
Entering context management
file
Successfully exited
```

这没用——但能让人明白这一点。这让它有用吗？

![vJ7yWCli55L8Sn1i-cnzBNyjXJm8quKKcPRq](img/d7c317461ea6a9b8665a9d8e76093f90.png)

[Philosoraptor](https://www.google.co.uk/search?q=Philosoraptor)

### 执行模型

块是在执行帧中作为一个单元执行的一段代码。

块的例子包括:

*   模块，它们是顶级块
*   功能体
*   类别定义
*   但不包括`for`循环和其他控制结构

还记得在 Python 中一切都是一个`object`吗？

嗯，你已经把`names`绑定到这些`objects`上了。这些`names`就是你认为的变量。

```
>>> x
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'x' is not defined
```

名称绑定或赋值发生在块中。

名称绑定示例—这些都很直观:

*   函数的参数被绑定到函数中定义的名称
*   导入语句绑定模块名
*   类和函数定义将名称绑定到类/函数对象
*   上下文管理器:`with ... as f` : f 是绑定到`...`对象的名称

绑定到块的名称是该块的本地名称。这意味着全局变量只是绑定到模块的名字。

块中使用的未定义变量是自由变量。

范围定义名称在块中的可见性。变量的范围包括定义它的块，以及定义块中包含的所有块。

还记得 for 循环不是块吗？这就是为什么在循环中定义的迭代变量在循环之后是可访问的，不像在 C++和 JavaScript 中。

```
>>> for i in range(5):
...   x = 2*i
...   print(x, i)
... 
0 0
2 1
4 2
6 3
8 4
>>> print(x, i)    # outside the loop! x was defined inside.
8 4
```

当在块中使用名称时，使用最近的封闭范围来解析它。

> 注意:如果一个名字绑定操作发生在一个代码块中的任何地方，那么这个名字在这个代码块中的所有使用都被认为是对当前代码块的引用。如果在绑定之前在块中使用名称，这可能会导致错误。

例如:

```
>>> name = "outer_scope"
>>> def foo():
...     name = "inner_function" if name == "outer_scope" \
                else "not_inner_function"
... 
>>> foo()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in foo
UnboundLocalError: local variable 'name' referenced before assignment
```

这是一个很棒的回溯，现在应该说得通了。

我们有顶级块，模块，其中还有另一个块，函数。函数内部的每个绑定都将该函数作为其顶级作用域！

因此，当您将名称`name`绑定到对象`"inner_function"`时:在绑定之前，您将检查它的值。规则说绑定前不能引用。`UnboundLocalError`的确切原因。

![3vBeauEpskRngPsPSWn2ww0KsgA3mNCGGzjl](img/bddcd3739a1e690be5481af38e82d0f5.png)

Not this kind of **Execution Model**? [Source](https://myanimelist.net/featured/1195/Top_25_Badass_Anime_Warrior_Girls)

### 词汇分析

Python 允许你使用[行连接](https://docs.python.org/3.7/reference/lexical_analysis.html#explicit-line-joining)。若要显式延续行，请使用反斜杠。

行连接后不允许注释。

```
if a < 10 and b < 10 \ # Comment results in SyntaxError
and c < 10: # Comment okay
    return True
else:
    return False
```

隐式地，当元素在大括号内时，行连接自行发生。此处允许评论。

```
month_names = ['Januari', 'Februari', 'Maart',      # These are the
               'April',   'Mei',      'Juni',       # Dutch names
               'Juli',    'Augustus', 'September',  # for the months
               'Oktober', 'November', 'December']   # of the year
```

#### 刻痕

[缩进](https://docs.python.org/3.7/reference/lexical_analysis.html#indentation)中的空格/制表符的数量并不重要，只要它对于应该缩进的内容来说是增加的。第一行不应该缩进。

四个空格规则是由 [PEP 8: Style Guide](https://www.python.org/dev/peps/pep-0008/#string-quotes) 定义的约定。遵循它是很好的实践。

```
# Compute the list of all permutations of l.
def perm(l):
        # Comment indentation is ignored
    if len(l) <= 1:
                  return [l]
    r = []
    for i in range(len(l)):
             s = l[:i] + l[i+1:]     # Indentation level chosen
             p = perm(s)             # Must be same level as above
             for x in p:
              r.append(l[i:i+1] + x) # One space okay
    return r
```

还有一些保留的标识符。

*   `_`用于导入:以`_`开头的函数/变量不被导入。
*   对于系统定义的名字，由实现定义:我们已经看到了一些。(`__str__()`、`__iter__()`、`__add__()`)

Python 还提供了[隐式字符串文字连接](https://docs.python.org/3.7/reference/lexical_analysis.html#string-literal-concatenation)

```
>>> def name():
...   return "Neil" "Kakkar"
...
>>> name()
'Neil Kakkar'
```

#### 格式字符串

[字符串格式化](https://docs.python.org/3.7/reference/lexical_analysis.html#formatted-string-literals)是 Python 中一个有用的工具。

字符串中可以有`{ expr }`，其中`expr`是一个表达式。表达式“评估”被替换。

可以指定转换，以便在格式化之前转换结果。

`!r`呼叫`repr()`，`!s`呼叫`str()`，`!a`呼叫`ascii()`

```
>>> name = "Fred"
>>> f"He said his name is {name!r}."
"He said his name is 'Fred'."
>>> f"He said his name is {repr(name)}."  # repr() is equiv. to !r
"He said his name is 'Fred'."
>>> width = 10
>>> precision = 4
>>> value = decimal.Decimal("12.34567")
>>> f"result: {value:{width}.{precision}}"  # nested fields
'result:      12.35'
# This is same as "{decf:10.4f}".format(decf=float(value))
>>> today = datetime(year=2017, month=1, day=27)
>>> f"{today:%B %d, %Y}"  # using date format specifier
'January 27, 2017'
>>> number = 1024
>>> f"{number:#0x}"  # using hexadecimal format specifier
'0x400'
```

使用`[str.format()](https://docs.python.org/3.7/library/stdtypes.html#str.format)`是一种更简洁的语法

### 摘要

至此，我们已经介绍了 Python 的主要支柱。对象数据模型、执行模型及其作用域和块以及字符串上的一些位。了解所有这些会让你领先于那些只知道语法的开发人员。这个数字比你想象的要高。

本系列的其他故事:

*   [如何不再害怕 Vim](https://medium.freecodecamp.org/how-not-to-be-afraid-of-vim-anymore-ec0b7264b0ae)
*   [如何不再害怕 GIT](https://medium.freecodecamp.org/how-not-to-be-afraid-of-git-anymore-fe1da7415286)

喜欢这个吗？不要再错过任何帖子了——订阅我的邮件列表吧！