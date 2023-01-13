# 如何使用 Python 调试器(pdb)调试 Python 代码

> 原文：<https://www.freecodecamp.org/news/debugging-in-python-using-pdb/>

调试工具是任何编程语言的核心。

作为一名开发人员，除非你知道如何使用这些工具，否则很难取得进展并写出干净的代码。

本文将帮助您熟悉这样一个工具:[Python 调试器(pdb)](https://docs.python.org/3/library/pdb.html#:~:text=The%20module%20pdb%20defines%20an,context%20of%20any%20stack%20frame)

注意，这是一个调试教程。我假设您至少熟悉一种编程语言，并且对编写测试用例有所了解。

# 如何入门`pdb`

调用`pdb`有两种方式:

## 1.对外调用`pdb`

要在终端上调用`pdb`，您可以在执行您的`.py`文件时调用它。

```
python -m pdb <test-file-name>.py 
```

如果你使用[诗歌](https://python-poetry.org/)和 [pytest](https://docs.pytest.org/en/7.1.x/) 最后你可以使用`--pdb`标志调用`pdb`。

```
poetry run python <path/to_your/test_file.py> --pdb 
```

要用 Docker、`poetry`和`pytest`调用`pdb`，可以使用以下语法:

```
COMPOSE_PROJECT_NAME=<test_docker_image_name> docker-compose run --rm workers poetry run pytest <path/to_your/test_file.py>::<name_of_the_test_function> --pdb 
```

您将总是在您的测试文件的名称后面添加`--pdb`标志。这将在测试中断时打开`pdb`控制台。但是记住`--pdb`是一面`pytest`旗帜。

## 2.用`pdb`添加断点

在测试中可能会有假阳性的情况。您的测试用例可能通过了，但是您没有得到您期望的数据。

如果您想读取原始数据库查询，该怎么办？在这种情况下，您可以从 Python 函数内部调用`pdb`。

要进入`pdb`调试器，您需要在函数内部调用`import pdb; pdb.set_trace()`。

让我们通过一个嵌套函数示例来理解这一点:

```
# file1.py

from . import function3

def function1():
    // logic for function1
    function3() 
```

```
# file2.py

def function2():
    // logic for function2
    // some database query 
```

```
# file3.py

from . import function2

def function3():
    // logic for function3
    function2()
    // logic for function3 continues 
```

在上面的代码中，一个函数调用另一个函数。

您想在`function2`中添加一个断点，以了解函数中实际发生了什么。

您可以使用以下语句添加断点:

`import pdb; pdb.set_trace()`

```
# file2.py

1\. def function2():
2\.     // logic for function2
3\.     // line 1
4\.     // line 2
5\.    
6\.     import pdb; pdb.set_trace();
7\.    
8\.     // some database query
9\.     // line 3
10\.    // line 4 
```

当你的代码中断时，打开它的控制台。大概是这样的:

```
(Pdb) 
```

当 Python 解释器执行`line2`时，会读取断点并打开`pdb`控制台。我们使用`pdb`命令来导航代码。我们将在下一节学习这些命令。

# 常见的`pdb`命令

`pdb`是一个交互式命令行调试器。除非你熟悉它的命令，否则你无法利用它的全部潜力。

像其他控制台日志一样，`pdb`会告诉你代码在哪一行中断。

### [打印](https://docs.python.org/3/library/pdb.html#pdbcommand-p)命令

假设您有一个带有`assert`语句的测试用例。大概是这样的:

```
# test.py

def test1():
    ...
    result = function1()
    assert result.json = {'status_code':1, 'status': 'saved', 'description':'data saved'} 
```

您将使用`p`命令将一个值打印到控制台。

```
(Pdb) p result.json
{'status_code':1, 'status': 'saved', 'description':'data saved'} 
```

这将打印变量保存的值。

### [上升](https://docs.python.org/3/library/pdb.html#pdbcommand-up)命令

up 命令将堆栈上移一帧。

在嵌套函数调用的情况下，它会将您向上移动到调用您的函数的函数中。

让我们举个例子:

```
# test.py

def function1():
    print("invoking function1")
    import pdb;pdb.set_trace()
    print("function1 invoked")

def function2():
    print("invoking function2")
    function1()
    print("function2 invoked")

def function3():
    print("inside function3")
    function2()
    print("function3 invoked")

# starting the call with function2()

function3() 
```

在`pdb`中，我们将这样称呼它:

```
$ python -m pdb test.py

> test.py(1)<module>()
-> def function1():
(Pdb) n
> test.py(7)<module>()
-> def function2():
(Pdb) n
> test.py(13)<module>()
-> def function3():
(Pdb) n
> test.py(20)<module>()
-> function3()
(Pdb) n
inside function3
invoking function2
invoking function1
> test.py(4)function1()
-> print("function1 invoked")
(Pdb) n
function1 invoked
--Return--
> test.py(4)function1()->None
-> print("function1 invoked")
(Pdb) u
> test.py(9)function2()
-> function1()
(Pdb) l
  4         print("function1 invoked")
  5
  6
  7     def function2():
  8         print("invoking function2")
  9  ->     function1()
 10         print("function2 invoked")
 11
 12
 13     def function3():
 14         print("inside function3")
(Pdb) u
> test.py(15)function3()
-> function2()
(Pdb) l
 10         print("function2 invoked")
 11
 12
 13     def function3():
 14         print("inside function3")
 15  ->     function2()
 16         print("function3 invoked")
 17
 18     # starting the call with function2()
 19
 20     function3()
(Pdb) u
> test.py(20)<module>()
-> function3()
(Pdb) l
 15         function2()
 16         print("function3 invoked")
 17
 18     # starting the call with function2()
 19
 20  -> function3()
[EOF]
(Pdb) u
> <string>(1)<module>() 
```

这里我们从调用`function3()`开始。遇到`import pdb`时执行停止。

`pdb`打开控制台，等待输入。我们为 up 键入`u`，它返回调用函数:`function2()`。在下一个`u`命令时，它返回`function3`(调用`function2`的函数)。

我们正在使用`l`命令。它是 list 命令。它准确地列出了当前执行行的位置。

### 第[步](https://docs.python.org/3/library/pdb.html#pdbcommand-step)命令

为了理解`step`命令，让我们继续前面的例子。

```
# test.py

1\. def test1():
2\.    ...
3\.    result = function1()
4\.    assert result.json.status_code == 1
5\.    assert result.json.status == 'saved'
6\.    assert result.json.description == 'data saved'
7. 
```

```
# function_file.py

def function1():
    foo = ['bar']
    ... 
```

您怀疑从`function1()`返回的结果不正确。你的代码在第 6 行。你如何进入第三行？

您将首先使用`up`命令，最后使用`s`命令进入该功能。

```
(Pdb) u
assert result.json.status == 'saved'
(Pdb) u
assert result.json.status_code == 1
(Pdb) p assert result.json.status_code
1
(Pdb) u
result = function1()
(Pdb) s
(Pdb) n
foo = ['bar'] 
```

当您进入`function1()`时，`pdb`控制台将开始打印该函数的语句。

# 结论

是一个强大的调试器。本教程旨在让你熟悉`pdb`的基础知识。

我推荐阅读[它的文档](https://docs.python.org/3/library/pdb.html)来探索全部潜力。