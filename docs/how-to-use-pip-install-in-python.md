# 如何在 Python 中使用 pip install

> 原文：<https://www.freecodecamp.org/news/how-to-use-pip-install-in-python/>

Python 附带了几个内置模块，但是 Python 社区提供了更多。正是这些模块让 python 变得如此强大！

第三方模块为 Python 增加了更多的功能。所以是时候学习如何安装这些模块了，这样我们就可以在我们的程序中使用它们。

最简单的方法是使用`pip`

```
pip install <module_name>
```

如果你用过`npm`，那么你可以把它想象成 Python 的 *npm* 。

附注:不同之处在于，使用 npm，`npm install`默认情况下将包本地安装到项目中，而`pip install`默认情况下全局安装。

要在本地安装模块，您需要创建并激活所谓的[虚拟环境](http://docs.python-guide.org/en/latest/dev/virtualenvs/)，因此`pip install`会安装到该虚拟环境所在的文件夹中，而不是全局安装(这可能需要管理员权限)。

上次在  wiki 中我们用了`requests`模块作为例子。由于它是第三方模块，我们必须在安装 python 后单独安装它。

安装它就像`pip install requests`一样简单。您甚至可以传递各种参数。你会更经常遇到的是`--upgrade`。您可以通过以下方式升级 python 模块:

```
pip install <module_name> --upgrade
```

例如，将 requests 模块升级到它的最新版本就像`pip install requests --upgrade`一样简单。

在使用`pip`之前，你需要安装它(这很简单)。你可以从[这里](https://bootstrap.pypa.io/get-pip.py)安装

只需点击链接。并将文件保存为`get-pip.py` *请不要忘记扩展名`.py`。*然后运行它。

使用画中画的一个替代方法是尝试 [`easy_install`](https://bootstrap.pypa.io/ez_setup.py) 。

使用`easy_install`也很简单。语法是:

```
easy_install <module_name>
```

不过，`pip`比用`easy_install`更受欢迎。

****注意:**** 在一些同时安装了 Python 2 & Python 3 的系统上，`pip`和`pip3`会做不同的事情。`pip`安装 Python 2 版本的包，`pip3`将安装 Python 3 版本的包。

更多关于 Python 2 和 3 之间区别的信息，请参见[本](https://guide.freecodecamp.org/python/python-2-vs-python-3)指南。您可以通过执行`pip --version`和/或`pip3 --version`来检查`pip`版本:

```
pip3 --version
pip 18.0 from /usr/local/lib/python3.5/dist-packages/pip (python 3.5)
```

我们还可以创建一个 txt 文件，其中包含应该使用 pip 安装的模块列表。例如，我们可以创建文件`requirements.txt`及其内容:

```
Kivy-Garden==0.1.4
macholib==1.5.1
idna==2.6
geoip2nation==0.1.2
docutils>=0.14
Cython
```

在这个文件中，我们还可以为安装设置一个版本。在这之后，通过调用 pip 与:

```
 pip install -r <FILE CONTAINING MODULES>

          OR IN OUR CASE

 pip install -r requirements.txt 
```

它应该安装文件中列出的所有模块。