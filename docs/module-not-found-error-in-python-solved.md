# ModuleNotFoundError:没有名为 Python 的模块错误[已修复]

> 原文：<https://www.freecodecamp.org/news/module-not-found-error-in-python-solved/>

当您尝试在 Python 文件中导入一个模块时，Python 会尝试用几种方法来解析这个模块。有时，Python 会在之后抛出 **ModuleNotFoundError** 。这个错误在 Python 中是什么意思？

顾名思义，当你试图访问或使用一个找不到的模块时，就会出现这个错误。如题，找不到“名为 **Python** 的模块”。

*Python* 这里可以是任何模块。当我试图导入一个找不到的`numpys`模块时，出现了一个错误:

```
import numpys as np 
```

下面是错误的样子:

![image-341](img/6ef547f70594a2a215180cc624ef596e.png)

以下是找不到模块的几个原因:

*   您的计算机上没有安装您尝试导入的模块
*   您错误地拼写了一个模块(仍然链接到上一点，拼写错误的模块没有安装)...例如，在导入过程中将`numpy`拼写为`numpys`
*   您对模块使用了不正确的大小写(它仍然链接回第一个点)...例如，在导入过程中将`numpy`拼写为`NumPy`将会抛出模块未找到错误，因为两个模块“不相同”
*   您正在使用错误的路径导入模块

## 如何修复 Python 中的 ModuleNotFoundError

正如我在上一节中提到的，模块可能找不到有几个原因。以下是一些解决方法。

### 1.确保安装了导入的模块

就拿`numpy`来说吧。您可以在名为“test.py”的文件中的代码中使用该模块，如下所示:

```
import numpy as np

arr = np.array([1, 2, 3])

print(arr) 
```

如果您尝试用`python test.py`运行这段代码，并得到以下错误:

```
ModuleNotFoundError: No module named "numpy" 
```

那么很有可能您的设备上没有安装`numpy`模块。您可以像这样安装模块:

```
python -m pip install numpy 
```

安装后，前面的代码将正确工作，您将在终端中打印结果:

```
[1, 2, 3] 
```

### 2.确保模块拼写正确

在某些情况下，您可能已经安装了所需的模块，但是试图使用它仍然会引发 ModuleNotFound 错误。在这种情况下，可能是你拼写错误。以这段代码为例:

```
import nompy as np

arr = np.array([1, 2, 3])

print(arr) 
```

这里，您已经安装了`numpy`,但是运行上面的代码会抛出这个错误:

```
ModuleNotFoundError: No module named "nompy" 
```

这个错误是由于将`numpy`模块拼错为`nompy`(字母 **o** 而不是 **u** )造成的。您可以通过正确拼写模块来修复此错误。

### 3.确保模块在正确的外壳中

类似于模块未找到错误的拼写错误问题，也可能是模块拼写正确，但大小写错误。这里有一个例子:

```
import Numpy as np

arr = np.array([1, 2, 3])

print(arr) 
```

对于这个代码，您已经安装了`numpy`,但是运行上面的代码会抛出这个错误:

```
ModuleNotFoundError: No module named 'Numpy' 
```

由于大小写不同，`numpy`和`Numpy`是不同的模块。您可以通过用正确的大小写拼写模块来修复此错误。

### 4.确保你使用正确的路径

在 Python 中，你可以使用**绝对**或**相对**路径从其他文件导入模块。对于这个例子，我将关注绝对路径。

当你试图从错误的路径访问一个模块时，你也会得到这里没有的模块。这里有一个例子:

假设您有一个名为 **test** 的项目文件夹。在里面，你有两个文件夹 **demoA** 和 **demoB** 。

**demoA** 有一个`__init__.py`文件(显示它是一个 Python 包)和一个`test1.py`模块。

**demoA** 也有一个`__init__.py`文件和一个`test2.py`模块。

结构如下:

```
└── test
    ├── demoA
        ├── __init__.py
    │   ├── test1.py
    └── demoB
        ├── __init__.py
        ├── test2.py 
```

以下是`test1.py`的内容:

```
def hello():
  print("hello") 
```

假设您想在`test2.py`中使用这个声明的`hello`函数。以下代码将引发“找不到模块”错误:

```
import demoA.test as test1

test1.hello() 
```

此代码将引发以下错误:

```
ModuleNotFoundError: No module named 'demoA.test' 
```

原因是我们使用了错误的路径来访问`test1`模块。正确的路径应该是`demoA.test1`。当您纠正这种情况时，代码会起作用:

```
import demoA.test1 as test1

test1.hello()
# hello 
```

## 包扎

为了解析导入的模块，Python 会检查内置库、安装的模块和当前项目中的模块。如果它不能解析那个模块，它抛出 **ModuleNotFoundError** 。

有时你没有安装那个模块，所以你必须安装它。有时是拼错了模块，或者是用错误的大小写命名，或者是错误的路径。在本文中，我展示了四种可能的方法来修复您遇到的错误。

希望你从中吸取了教训:)