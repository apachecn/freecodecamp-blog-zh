# 一个有用的 Python 技巧的 A-Z

> 原文：<https://www.freecodecamp.org/news/an-a-z-of-useful-python-tricks-b467524ee747/>

Python 是世界上最流行、最受欢迎的编程语言之一。这有许多原因:

*   这很容易学
*   超级百搭
*   它有大量的模块和库

作为一名数据科学家，我每天都使用 Python，这是我工作中不可或缺的一部分。一路上，我学到了一些有用的技巧和窍门。

在这里，我以 A-Z 格式分享了其中的一些。

这些“窍门”大多是我在日常工作中用过或偶然发现的。有些是我在浏览 [Python 标准库文档](https://docs.python.org/3/library/index.html)时发现的。我通过搜索 [PyPi](https://pypi.org/search/) 找到了另外几个。

然而，值得表扬的是——我在 awesome-python.com 发现了四五个。这是数百个有趣的 Python 工具和模块的精选列表。值得浏览寻找灵感！

#### 全部或任何

Python 是如此受欢迎的语言的众多原因之一是因为它具有可读性和表现力。

经常有人开玩笑说 Python 是'[可执行伪代码](https://www.artima.com/intv/tippingP.html)'。但是当你能写出这样的代码时，就很难反驳了:

```
x = [True, True, False]
if any(x):
    print("At least one True")
if all(x):
    print("Not one False")
if any(x) and not all(x):    
    print("At least one True and one False")
```

#### bashplotlib

您想在控制台中绘制图表吗？

```
$ pip install bashplotlib
```

您可以在控制台中看到图表。

#### 收集

Python 有一些很好的默认数据类型，但有时它们的行为并不完全符合您的期望。

幸运的是，Python 标准库提供了集合模块。这个方便的附加组件为您提供了更多的数据类型。

```
from collections import OrderedDict, Counter

# Remembers the order the keys are added!
x = OrderedDict(a=1, b=2, c=3)

# Counts the frequency of each character
y = Counter("Hello World!") 
```

#### 目录

有没有想过如何查看 Python 对象的内部，看看它有什么属性？你当然有。

从命令行:

```
>>> dir()
>>> dir("Hello World")
>>> dir(dir)
```

当以交互方式运行 Python 时，这是一个非常有用的特性，对于动态浏览您正在使用的对象和模块来说也是如此。

点击阅读更多[。](https://docs.python.org/3/library/functions.html#dir)

#### 绘文字

是的，[真的](https://pypi.org/project/emoji/)。

```
$ pip install emoji
```

不要假装你不会尝试它…

```
from emoji import emojize
print(emojize(":thumbs_up:"))
```

？

#### 从 __ 未来 _ _ 进口

Python 流行的一个后果是总有新版本在开发中。新版本意味着新特性——除非您的版本已经过时。

然而，不要害怕。 [__future__ 模块](https://docs.python.org/2/library/__future__.html)允许您从 Python 的未来版本中导入功能。这简直就像时间旅行，或者魔法，或者别的什么。

```
from __future__ import print_function
print("Hello World!")
```

为什么不试试导入花括号呢？

#### 地质公园

地理对于程序员来说可能是一个具有挑战性的领域(哈，一语双关！).但是[geo py 模块](https://geopy.readthedocs.io/en/latest/)让它变得异常简单。

```
$ pip install geopy
```

它通过抽象一系列不同地理编码服务的 API 来工作。它使你能够获得一个地方的完整的街道地址，纬度，经度，甚至海拔。

还有一个有用的远程课程。它以你最喜欢的测量单位计算两个位置之间的距离。

```
from geopy import GoogleV3

place = "221b Baker Street, London"
location = GoogleV3().geocode(place)
print(location.address)
print(location.location)
```

#### 怎么办

被困在一个编码问题上，却不记得之前看到的解决方案？需要检查 StackOverflow，但不想离开终端？

然后你需要[这个有用的命令行工具](https://github.com/gleitz/howdoi)。

```
$ pip install howdoi
```

你有什么问题就问它，它会尽最大努力回答你。

```
$ howdoi vertical align css
$ howdoi for loop in java
$ howdoi undo commits in git
```

但是要注意，它从 StackOverflow 的顶级答案中抓取代码。它可能不总是提供最有用的信息…

```
$ howdoi exit vim
```

#### 检查

Python 的 [inspect 模块](https://docs.python.org/3/library/inspect.html)对于理解幕后发生的事情非常有用。你甚至可以在它自身上调用它的方法！

下面的代码示例使用`inspect.getsource()`打印自己的源代码。它还使用`inspect.getmodule()`来打印定义它的模块。

最后一行代码打印出自己的行号。

```
import inspect

print(inspect.getsource(inspect.getsource))
print(inspect.getmodule(inspect.getmodule))
print(inspect.currentframe().f_lineno)
```

当然，除了这些琐碎的用途之外，inspect 模块对于理解您的代码在做什么也很有用。您还可以用它来编写自文档化的代码。

#### 绝地

Jedi 库是一个自动补全和代码分析库。它使编写代码更快、更有效率。

除非你正在开发自己的 IDE，否则你可能会对使用 Jedi 作为编辑器插件最感兴趣。幸运的是，已经有可用的负载了！

然而，你可能已经在用绝地了。IPython 项目利用 Jedi 的代码自动完成功能。

#### **kwargs

在学习任何语言时，都有许多里程碑。用 Python，理解神秘的`**kwargs`语法大概算一个吧。

dictionary 对象前面的双星号允许您将该字典的内容作为函数的[命名参数传递。](https://docs.python.org/3/tutorial/controlflow.html#keyword-arguments)

字典的键是参数名，值是传递给函数的值。你甚至不需要叫它`kwargs`！

```
dictionary = {"a": 1, "b": 2}

def someFunction(a, b):
    print(a + b)
    return

# these do the same thing:
someFunction(**dictionary)
someFunction(a=1, b=2)
```

当您想要编写能够处理没有预先定义的命名参数的函数时，这很有用。

#### 列出理解

关于 Python 编程，我最喜欢的事情之一是它的[列表理解](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions)。

这些表达式使得编写非常简洁的代码变得容易，读起来几乎像自然语言。

你可以在这里阅读更多关于如何使用它们的信息。

```
numbers = [1,2,3,4,5,6,7]
evens = [x for x in numbers if x % 2 is 0]
odds = [y for y in numbers if y not in evens]

cities = ['London', 'Dublin', 'Oslo']

def visit(city):
    print("Welcome to "+city)
for city in cities:
    visit(city)
```

#### 地图

Python 通过许多内置特性支持函数式编程。其中最有用的是`map()`函数——尤其是与[λ函数](https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions)结合使用时。

```
x = [1, 2, 3]
y = map(lambda x : x + 1 , x)
# prints out [2,3,4]print(list(y))
```

在上面的例子中，`map()`对`x`中的每个元素应用一个简单的 lambda 函数。它返回一个 map 对象，这个对象可以被转换成一些 iterable 对象，比如 list 或 tuple。

#### 报纸 3k

如果你还没有看过，那就准备好被 [Python 的报纸模块](https://pypi.org/project/newspaper3k/)震撼吧。

它让你从一系列领先的国际出版物中检索新闻文章和相关的元数据。您可以检索图像、文本和作者姓名。

它甚至有一些内置的自然语言处理功能。

因此，如果你正考虑在你的下一个项目中使用 BeautifulSoup 或其他一些 DIY 网络抓取库，省下你自己的时间和精力，用`$ pip install newspaper3k`代替。

#### 运算符重载

Python 支持[运算符重载](https://docs.python.org/3/reference/datamodel.html#special-method-names)，这是让你听起来像一个合法的计算机科学家的术语之一。

这其实是一个简单的概念。想知道为什么 Python 允许使用`+`操作符来添加数字和连接字符串吗？这是操作符过载在起作用。

您可以以自己的特定方式定义使用 Python 的标准运算符符号的对象。这使您可以在与您正在处理的对象相关的上下文中使用它们。

```
class Thing:
    def __init__(self, value):
        self.__value = value
    def __gt__(self, other):
        return self.__value > other.__value
    def __lt__(self, other):
        return self.__value < other.__value

something = Thing(100)
nothing = Thing(0)

# True
something > nothing

# False
something < nothing

# Error
something + nothing
```

#### 点点点

Python 的默认`print`函数完成了它的工作。但是尝试打印出任何大的、嵌套的对象，结果是相当难看的。

这里是[标准库的漂亮打印模块](https://docs.python.org/3/library/pprint.html)介入的地方。这就以一种易于阅读的格式打印出复杂的结构化对象。

任何使用非平凡数据结构的 Python 开发人员的必备工具。

```
import requests
import pprint

url = 'https://randomuser.me/api/?results=1'
users = requests.get(url).json()
pprint.pprint(users)
```

#### 长队

Python 支持多线程，标准库的队列模块促进了这一点。

该模块允许您实现队列数据结构。这些数据结构允许您根据特定的规则添加和检索条目。

先进先出(或 FIFO)队列允许您按照对象添加的顺序检索对象。后进先出(LIFO)队列允许您首先访问最近添加的对象。

最后，优先级队列允许您根据对象的排序顺序来检索对象。

这里有一个如何在 Python 中使用队列进行多线程编程的例子。

#### __repr__

当在 Python 中定义一个类或一个对象时，提供一种将该对象表示为字符串的“官方”方式是很有用的。例如:

```
>>> file = open('file.txt', 'r')
>>> print(file)
<open file 'file.txt', mode 'r' at 0x10d30aaf0>
```

这使得调试代码更加容易。将其添加到您的类定义中，如下所示:

```
class someClass:
    def __repr__(self):
        return "<some description here>"

someInstance = someClass()

# prints <some description here>
print(someInstance)
```

#### 嘘

Python 是一种很棒的脚本语言。有时，使用标准操作系统和子进程库可能有点令人头疼。

sh 库提供了一个很好的选择。

它让您可以像调用普通函数一样调用任何程序——这对于自动化工作流和任务非常有用，所有这些都是在 Python 中完成的。

```
import sh
sh.pwd()
sh.mkdir('new_folder')
sh.touch('new_file.txt')
sh.whoami()
sh.echo('This is great!')
```

#### 类型提示

Python 是一种动态类型的语言。当你定义变量、函数、类等时，你不需要指定数据类型。

这允许快速的开发时间。然而，没有什么比由简单的输入问题引起的运行时错误更令人烦恼的了。

[从 Python 3.5](https://docs.python.org/3/library/typing.html) 开始，您可以选择在定义函数时提供类型提示。

```
def addTwo(x : Int) -> Int:    return x + 2
```

您也可以定义类型别名:

```
from typing import List
```

```
Vector = List[float]Matrix = List[Vector]
```

```
def addMatrix(a : Matrix, b : Matrix) -> Matrix:
    result = []
    for i,row in enumerate(a):
        result_row =[]
        for j, col in enumerate(row):
            result_row += [a[i][j] + b[i][j]]
        result += [result_row]
    return result

x = [[1.0, 0.0], [0.0, 1.0]]
y = [[2.0, 1.0], [0.0, -2.0]]
z = addMatrix(x, y)
```

虽然不是强制性的，但是类型注释可以使您的代码更容易理解。

它们还允许您使用类型检查工具在运行前捕捉那些零散的类型错误。如果你从事大型复杂的项目，这可能是值得的！

#### uuid

生成通用唯一 id(或“uuid”)的一种快速简单的方法是通过 [Python 标准库的 uuid 模块](https://docs.python.org/3/library/uuid.html)。

```
import uuid

user_id = uuid.uuid4()
print(user_id)
```

这就产生了一个随机的 128 位数字，几乎可以肯定它是唯一的。

事实上，有超过 2 种可能的 UUIDs 可以生成。那是 50 多亿英镑。

在给定的集合中找到重复项的概率极低。即使有一万亿个 UUIDs，重复存在的可能性也远远小于十亿分之一。

对于两行代码来说已经很不错了。

#### 虚拟环境

这可能是我最喜欢的 Python 了。

您有可能同时从事多个 Python 项目。不幸的是，有时两个项目会依赖同一个依赖项的不同版本。您在系统上安装了哪个？

幸运的是，Python 的[对虚拟环境的支持](https://docs.python.org/3/tutorial/venv.html)让你拥有了两个世界的精华。从命令行:

```
python -m venv my-project
source my-project/bin/activate
pip install all-the-modules 
```

现在，您可以在同一台机器上运行 Python 的独立版本和安装。分类了！

#### 维基百科（开放式百科全书）

维基百科有一个很棒的 API，允许用户有计划地访问无与伦比的完全免费的知识和信息。

维基百科模块让访问这个 API 变得非常方便。

```
import wikipedia

result = wikipedia.page('freeCodeCamp')
print(result.summary)

for link in result.links:
    print(link)
```

与真实站点一样，该模块提供了对多种语言、页面消歧、随机页面检索的支持，甚至还有一个`donate()`方法。

#### xkcd

幽默是 Python 语言的一个重要特征——毕竟，它是以英国喜剧小品节目[蒙蒂·Python 的《飞行马戏团](https://en.wikipedia.org/wiki/Monty_Python%27s_Flying_Circus)命名的。Python 的许多官方文档引用了该节目最著名的草图。

然而，幽默感并不局限于医生。试着运行下面的行:

```
import antigravity
```

永不改变，巨蟒。永远不变。

#### 亚姆

YAML 代表“ [YAML 不是标记语言](http://yaml.org/)”。它是一种数据格式化语言，是 JSON 的超集。

与 JSON 不同，它可以存储更复杂的对象并引用自己的元素。您还可以编写注释，这使它特别适合编写配置文件。

[PyYAML 模块](https://pyyaml.org/wiki/PyYAMLDocumentation)让你可以在 Python 中使用 YAML。安装方式:

```
$ pip install pyyaml
```

然后导入到项目中:

```
import yaml
```

PyYAML 允许您存储任何数据类型的 Python 对象，以及任何用户定义的类的实例。

#### 活力

给你最后一个技巧，真的很酷。曾经需要用两个列表组成一个字典吗？

```
keys = ['a', 'b', 'c']
vals = [1, 2, 3]
zipped = dict(zip(keys, vals))
```

`zip()`内置函数接受许多可迭代的对象，并返回一个元组列表。每个元组按照位置索引对输入对象的元素进行分组。

你也可以通过调用对象上的`*zip()`来“解压”对象。

#### 感谢阅读！

这就是 Python 技巧的 A-Z——希望你已经找到了对你的下一个项目有用的东西。

Python 是一种非常多样化且开发良好的语言，所以肯定会有很多我还没来得及介绍的特性。

请在下面留言，分享你最喜欢的 Python 技巧！