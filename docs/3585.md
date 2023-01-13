# Python 导入语句解释

> 原文：<https://www.freecodecamp.org/news/python-import-statements/>

在学习编程和阅读一些资源时，你可能会遇到“抽象”这个词，它的意思就是尽可能地减少和重用代码。

功能和模块有助于抽象。当你想在一个文件中重复做一些事情时，你可以创建函数。

当您想要重用不同源文件中的一组函数时，就需要模块了。模块也有助于很好地组织程序。

*   使用标准库和其他第三方模块
*   构建计划

## **使用标准库**

例子:你可以在官方 Python 文档中详细阅读所有标准库的方法/函数。

```
import time
for i in range(100):
    time.sleep(1)   # Waits for 1 second and then executes the next command
    print(str(i) + ' seconds have passed')  # prints the number of seconds passed after the program was started
```

![:rocket:](img/914577d44204781cbe2fdc1ab7e55f0b.png ":rocket:")

[运行代码](https://repl.it/CS6C)

```
# To calculate the execution time of a part of program
import time
start = time.time()
# code here
end = time.time()
print('Execution time:' , end-start)
```

![:rocket:](img/914577d44204781cbe2fdc1ab7e55f0b.png ":rocket:")

[运行代码](https://repl.it/CS6C/1)

```
# Using math Module
import math
print(math.sqrt(100))   # prints 10
```

![:rocket:](img/914577d44204781cbe2fdc1ab7e55f0b.png ":rocket:")

[运行代码](https://repl.it/CS6C/2)

## **使用第三方模块**

第三方模块不与 python 捆绑在一起，但是我们必须使用包管理器从外部安装它，比如 [`pip`](https://bootstrap.pypa.io/get-pip.py) 和 [`easy install`](https://bootstrap.pypa.io/ez_setup.py)

```
# To make http requests
import requests
rq = requests.get(target_url)
print(rq.status_code)
```

点击了解更多关于 python 请求模块[的信息](http://docs.python-requests.org/en/master/)

## **构建程序**

我们想做一个关于质数有各种功能的程序。让我们开始吧。我们将在`prime_functions.py`中定义所有功能

```
# prime_functions.py
from math import ceil, sqrt
def isPrime(a):
    if a == 2:
        return True
    elif a % 2 == 0:
        return False
    else:
        for i in range(3,ceil(sqrt(a)) + 1,2):
            if a % i == 0:
                return False
        return True

def print_n_primes(a):
    i = 0
    m = 2
    while True:
        if isPrime(m) ==True:
            print(m)
            i += 1
        m += 1
        if i == a:
            break
```

现在我们想使用我们刚刚在`prime_functions.py`中创建的函数，所以我们创建一个新文件`playground.py`来使用这些函数。

请注意，这个程序太简单了，不能创建两个独立的文件，它只是用来演示的。但是当有大型复杂程序时，制作不同的文件真的很有用。

```
# playground.py
import prime_functions
print(prime_functions.isPrime(29)) # returns True
```

## **分拣进口**

好的做法是将`import`模块分成三组——标准库导入、相关的第三方导入和本地导入。在每个组中，按照模块名称的字母顺序进行排序是明智的。你可以在 PEP8 中找到[更多信息。](https://www.python.org/dev/peps/pep-0008/?#imports)

Python 语言最重要的一点是易读性，按字母顺序排序的模块阅读和搜索起来更快。此外，更容易验证是否导入了某些内容，并避免重复导入。

## 从 X 导入 Y:示例

这里有一个问题示例:

```
>>> from math import ceil, sqrt
>>> # here it would be
>>> sqrt(36)
<<< 6
```

![:rocket:](img/914577d44204781cbe2fdc1ab7e55f0b.png ":rocket:")

[运行代码](https://repl.it/CS5t/1)

或者我们可以用这个来代替:

```
>>> import math
>>> # here it would be
>>> math.sqrt(36)
<<< 6
```

![:rocket:](img/914577d44204781cbe2fdc1ab7e55f0b.png ":rocket:")

[运行代码](https://repl.it/CS5u)

那么我们的代码将看起来像`math.sqrt(x)`而不是`sqrt(x)`。发生这种情况是因为当我们使用`import x`时，名称空间`x`本身被创建以避免名称冲突。你必须以`x.<name>`的身份访问模块中的每一个对象。

但是当我们使用`from x import y`时，我们同意将`y`添加到主全局名称空间中。所以在使用它的时候，我们必须确保程序中没有同名的对象。

****如果一个名为`y`的对象已经存在**** 就不要使用`from x import y`

例如，在`os`模块中有一个方法`open`。但是我们甚至有一个名为`open`的内置函数。所以，这里要避免使用`from os import open`。

我们甚至可以使用`form x import *`，这将把该模块的所有方法和类导入到程序的全局名称空间中。这是一种糟糕的编程实践。请避开它。

一般来说，你应该避免使用`from x import y`,因为它可能会在大型程序中引起问题。例如，您永远不知道一个程序员同事是否想创建一个新函数，而这个新函数恰好是一个现有函数的名称。您也不知道 Python 是否会改变您从中导入函数的库。虽然这些问题在单独的项目中不会经常出现，但如前所述，这是糟糕的编程实践，应该避免。