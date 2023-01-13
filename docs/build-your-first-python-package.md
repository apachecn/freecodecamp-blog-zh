# 如何构建您的第一个 Python 包

> 原文：<https://www.freecodecamp.org/news/build-your-first-python-package/>

几个月前，我决定发布一个用 Python 编写的计算机视觉包 [Caer](http://github.com/jasmcaus/caer) 。我发现这个过程极其痛苦。您可能已经猜到了原因——文档很少(而且令人困惑),缺少好的教程，等等。

所以我决定写这篇文章，希望它能帮助正在为此奋斗的人们。我们将构建一个非常简单的模块，并提供给世界各地的任何人。

本模块的内容遵循一个非常基本的结构。总共有四个 Python 文件，每个文件中都有一个方法。我们现在要保持简单。

```
base-verysimplemodule  --> Base
└── verysimplemodule   --> Actual Module
    ├── extras
    │   ├── multiply.py
    │   ├── divide.py
    ├── add.py
    ├── subtract.py
```

你会注意到我有一个名为`verysimplemodule`的文件夹，其中有两个 Python 文件`add.py`和`subtract.py`。还有一个文件夹叫`extras`(里面有`multiply.py`和`divide.py`)。这个文件夹将构成我们 Python 模块的基础。

### 带出 __init__s

你总能在每个 Python 包中找到的东西是一个`__init__.py`文件。这个文件将告诉 Python 把目录当作模块(或子模块)。

非常简单，它将保存其直接目录中所有 Python 文件中所有方法的名称。

典型的`__init__.py`文件具有以下格式:

```
from file import method 

# 'method' is a function that is present in a file called 'file.py'
```

在 Python 中构建包时，需要在包的每个子目录中添加一个`__init__.py`文件。这些子目录是你的包的*子模块*。

对于我们的例子，我们将添加 __init__。将文件复制到“实际模块”目录`verysimplemodule`，如下所示:

```
from add import add
from subtract import subtract
```

我们将对`extras`文件夹做同样的事情，就像这样:

```
from multiply import multiply
from divide import divide
```

一旦完成，我们就差不多完成了这个过程的一半！

### 如何设置 setup.py

在`base-verysimplemodule`文件夹中(和我们的模块`verysimplemodule`在同一个目录下)，我们需要添加一个`setup.py`文件。如果你打算*构建*正在讨论的实际模块，这个文件是必不可少的。

注意:随意命名`setup.py`文件。这个文件不像我们的`__init__.py`文件那样是名称特定的。

可能的名字选择是`setup_my_very_awesome_python_package.py`和`python_package_setup.py`，但是最好还是坚持使用`setup.py`。

这个`setup.py`文件将包含关于你的包的信息，特别是包的*名*，它的*版本，*平台依赖性等等。

出于我们的目的，我们不需要高级元信息，所以下面的代码应该适合您构建的大多数包:

```
from setuptools import setup, find_packages

VERSION = '0.0.1' 
DESCRIPTION = 'My first Python package'
LONG_DESCRIPTION = 'My first Python package with a slightly longer description'

# Setting up
setup(
       # the name must match the folder name 'verysimplemodule'
        name="verysimplemodule", 
        version=VERSION,
        author="Jason Dsouza",
        author_email="<youremail@email.com>",
        description=DESCRIPTION,
        long_description=LONG_DESCRIPTION,
        packages=find_packages(),
        install_requires=[], # add any additional packages that 
        # needs to be installed along with your package. Eg: 'caer'

        keywords=['python', 'first package'],
        classifiers= [
            "Development Status :: 3 - Alpha",
            "Intended Audience :: Education",
            "Programming Language :: Python :: 2",
            "Programming Language :: Python :: 3",
            "Operating System :: MacOS :: MacOS X",
            "Operating System :: Microsoft :: Windows",
        ]
)
```

完成后，我们接下来要做的就是在与`base-verysimplemodule`相同的目录下运行下面的命令:

```
python setup.py sdist bdist_wheel
```

这将构建 Python 需要的所有必需的包。`sdist`和`bdist_wheel`命令将创建一个源代码发行版和一个轮子，您可以稍后将它们上传到 PyPi。

### 皮皮——我们来了！

PyPi 是官方的 Python 库，所有的 Python 包都存储在这里。你可以把它想象成 Python 包的 *Github。*

为了让全世界的人都可以使用你的 Python 包，你需要有一个 PyPi 的[账户。](https://pypi.org/account/register/)

完成后，我们就可以在 PyPi 上上传我们的包了。还记得我们跑`python setup.py`的时候建的源码分布和轮子吗？这些是实际上传到 PyPi 的内容。

但是在这之前，如果你还没有安装的话，你需要安装`twine`。就像`pip install twine`这么简单。

### 如何将您的包上传到 PyPi

假设您已经安装了`twine`，继续运行:

```
twine upload dist/*
```

这个命令将上传我们运行`python setup.py`时自动生成的`dist`文件夹的内容。您将得到一个提示，要求您输入您的 PyPi 用户名和密码，所以请继续输入。

现在，如果您按照本教程进行操作，您可能会得到一个类似于**存储库已经存在的错误。**

这通常是因为您的软件包的名称与已经存在的软件包的名称冲突。换句话说，改变你的包的名字——别人已经取了那个名字。

就是这样！

要自豪地安装您的模块，启动终端并运行:

```
pip install <package_name> 

# in our case, this is
pip install verysimplemodule
```

观察 Python 如何从先前生成的二进制文件中安装您的包。

打开 Python 交互式 shell 并尝试导入您的包:

```
>> import verysimplemodule as vsm

>> vsm.add(2,5)
7
>> vsm.subtract(5,4)
1
```

访问除法和乘法方法(记得它们在一个名为`extras`的文件夹中吗？)，运行:

```
>> import verysimplemodule as vsm

>> vsm.extras.divide(4,2)
2
>> vsm.extras.multiple(5,3)
15
```

就这么简单。

恭喜你！您刚刚构建了您的第一个 Python 包。尽管非常简单，但现在世界上的任何人都可以下载您的包(当然，只要他们有 Python)。

## 下一步是什么？

### 测试成功了

我们在本教程中使用的软件包是一个极其简单的模块——加减乘除的基本数学运算。将它们直接上传到 PyPi *尤其是*是没有意义的，因为这是你第一次尝试。

幸运的是，我们有 [Test PyPi](http://test.pypi.org/) ，这是一个单独的 PyPi 实例，您可以在其中测试和试验您的软件包(您需要在平台上注册一个单独的帐户)。

您上传以测试 PyPi 所遵循的过程几乎是相同的，只有一些小的变化。

```
# The following command will upload the package to Test PyPi
# You will be asked to provide your Test PyPi credentials

twine upload --repository testpypi dist/*
```

要从测试 PyPi 下载项目:

```
pip install --index-url "https://test.pypi.org/simple/<package_name>"
```

### 高级元信息

我们在`setup.py`文件中使用的元信息非常基本。您可以添加额外的信息，如多个维护者(如果有的话)、作者电子邮件、许可信息和一大堆其他数据。

如果你打算这么做，这篇文章会特别有帮助。

### 看看其他存储库

看看其他存储库是如何构建它们的包的，会对你非常有用。

在构建 [Caer](https://github.com/jasmcaus/caer) 的时候，我会不断地观察 [Numpy](https://github.com/numpy/numpy) 和 [Sonnet](https://github.com/deepmind/sonnet) 如何设置他们的包。如果你打算构建稍微高级一点的包，我建议你看看 [Caer 的](https://github.com/jasmcaus/caer)、 [Numpy 的](https://github.com/numpy/numpy)和 [Tensorflow 的](https://github.com/tensorflow/tensorflow)库。