# 如何管理多个 Python 版本和虚拟环境

> 原文：<https://www.freecodecamp.org/news/manage-multiple-python-versions-and-virtual-environments-venv-pyenv-pyvenv-a29fb00c296f/>

【2019 年 1 月补充:如果你在升级到 macOS Mojave 后回到这个博客，请参见[这个 github 问题](https://github.com/pyenv/pyenv/issues/1219#issue-363576794)以获得常见 pyenv“zlib 不可用”问题的解决方案。

在我们开始之前，让我们简要回顾一下标题中使用的术语:

*   **多个 Python 版本**:同一台机器上不同的 Python 安装，比如 2.7 和 3.4。
*   [**虚拟环境**](https://packaging.python.org/tutorials/installing-packages/#creating-virtual-environments) :隔离的独立环境，可以安装特定版本的 Python 和任何特定于项目的包，而不会影响任何其他项目。

在这里，我们将看看三种不同的工具来处理这些问题，以及何时您可能需要每一种工具。让我们来探索以下使用案例:

*   `venv` / `pyvenv`
*   `pyenv`
*   `pyenv-virtualenv`

如果你正在使用 Python 的**单一版本**，比如说版本 **3.3+** ，并且想要管理**不同的虚拟环境，**那么`venv`就是你所需要的。

如果你想在 **3.3+** ，**使用**多个版本**的 Python，不管有没有虚拟环境**，那么继续阅读关于`pyenv`的内容。

如果你也想使用 **Python 2** ，那么`pyenv-virtualenv`是一个可以考虑的工具。

### 文夫

从 Python 3.3+开始，包含了`venv`包。它是创建轻量级虚拟环境的理想选择。

在 Python 3.6 之前，一个名为`pyvenv`的脚本也被包含在`venv`的包装器中，但是这个已经被[弃用了](https://docs.python.org/3/library/venv.html)。它将在 Python 3.8 中被完全移除。使用`venv`时可获得完全相同的功能，任何现有文档都应更新。任何感兴趣的人都可以阅读`pyvenv` 贬值背后的原因。

`venv`用于通过终端命令创建新环境:

```
$ python3 -m venv directory-name-to-create
```

激活方式:

```
$ source name-given/bin/activate
```

只需简单地:

```
$ deactivate
```

如果您需要在停用环境后将其完全删除，您可以运行:

```
$ rm -r name-given
```

默认情况下，它创建的环境将是您正在使用的 Python 的当前版本。如果您正在编写文档，并且希望读者使用的 Python 版本更加安全，您可以在命令中指定主版本号和次版本号，如下所示:

```
$ python3.6 -m venv example-three-six
```

如果读取器使用的版本不是 3.6，则该命令将不会成功，并将在其错误消息中指出。但是，任何补丁版本(例如 3.6.4)都可以。

当环境处于活动状态时，任何包都可以像往常一样通过`[pip](https://pip.pypa.io/en/stable/installing/#installation)`安装到环境中。默认情况下，新创建的环境将**而不是**包含机器上已经安装的任何包。因为`pip`本身不一定安装在机器上。建议先将`pip`升级到最新版本，使用`pip install --upgrade pip`。

项目通常会有一个指定其依赖关系的`requirements.txt`文件。这允许快捷命令`pip install -r requirements.txt`命令将所有包快速安装到新创建的虚拟环境中。它们只会存在于虚拟环境中。它在被停用时将不可用，但在被重新激活时将持续存在。

如果您不需要使用 Python 本身的其他版本，那么这就是创建隔离的、特定于项目的虚拟环境所需的全部内容。

### [pyenv](https://github.com/pyenv/pyenv)

如果你希望在一台机器上使用多个版本的 Python，那么`pyenv`是一个常用的安装和切换版本的工具。这不要与前面提到的 depreciated `pyvenv`脚本混淆。它不与 Python 捆绑在一起，必须单独安装。

`[pyenv](https://github.com/pyenv/pyenv)` [文档](https://github.com/pyenv/pyenv)包含了对[如何工作](https://github.com/pyenv/pyenv#how-it-works)的精彩描述，所以这里我们将简单地看一下如何使用它。

首先，我们需要安装它。如果使用 Mac OS X，我们可以使用自制软件，否则考虑其他安装选项。

```
$ brew update
$ brew install pyenv
```

接下来，在 shell 脚本的底部添加以下内容，以允许`pyenv`自动为您更改版本:

```
eval "$(pyenv init -)"
```

为此，通过`$ ~/.zshrc`、`$ ~/.bashrc`或`$ ~/.bash_profile`在使用 shell 脚本中打开您的[，复制并粘贴上面的行。](https://askubuntu.com/questions/590899/how-to-check-which-shell-am-i-using)

运行`pyenv versions`将显示当前安装了哪些 Python 版本，当前正在使用的版本旁边有一个`*`。`pyenv version`直接说明了这一点，`python --version`可以用来验证这一点。

要安装额外的版本，比如说`3.4.0`，只需使用`pyenv install 3.4.0`。

`pyenv`在四个地方查找以决定使用哪个版本的 Python，按优先顺序:

1.  `PYENV_VERSION`环境变量(如果指定的话)。您可以使用`[pyenv shell](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md#pyenv-shell)`命令在当前的 shell 会话中设置这个环境变量。
2.  当前目录中特定于应用程序的`.python-version`文件(如果存在)。你可以用`[pyenv local](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md#pyenv-local)`命令修改当前目录的`.python-version`文件。
3.  通过搜索每个父目录找到的第一个`.python-version`文件(如果有的话),直到到达文件系统的根目录。
4.  全局版本文件。您可以使用`[pyenv global](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md#pyenv-global)`命令修改该文件。如果全局版本文件不存在，pyenv 假定您想要使用“系统”Python。(换句话说，如果 pyenv 不在您的`PATH`中，无论什么版本都会运行。)

当建立一个使用 Python 3.6.4 的新项目时，那么`pyenv local 3.6.4`将在其根目录下运行。这既设置了版本，又创建了一个`.python-version`文件，以便其他贡献者的机器可以获取它。

`pyenv`命令的[完整描述需要添加书签。](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md)

#### pyenv 和 venv

使用 Python 3.3+时，我们现在知道如何安装不同版本的 Python 并在不同版本之间切换，以及如何创建新的虚拟环境。

举个例子，假设我们正在建立一个使用 Python 3.4 的项目。

首先，我们可以使用`pyenv local 3.4.0`设置我们的本地版本。

如果我们随后运行`python3 -m venv example-project`，一个新的虚拟环境将在`example-project`下建立，使用我们本地启用的 Python 3.4.0。

我们使用`source example-project/bin/activate`激活并开始工作。

接下来我们可以*可选地*记录一个合作者应该使用的`python3.4 -m venv <name>`。这意味着即使合作者没有使用 pyenv，如果他们的 Python 版本不是我们想要的主版本和次版本(3 和 4 ),那么`python3.4`命令也会出错。

或者，我们可以选择简单地指定要使用 3.4.0，并指示`python3 -m venv <name>`。如果我们相信任何大于 3.4 的版本 g 都是可以接受的，那么我们也可以选择使用`python3`而不是`python3.4`，就好像合作者使用的是 3.6，否则他们也会收到一个错误。这是一个项目的具体决定。

### pyenv-virtualenv

`pyenv`可用于安装 Python 2 和 3 版本。然而，正如我们所见，`venv`仅限于 than 3.3 以上的版本。

`pyenv-virtualenv`是一个与`pyenv`集成的创建虚拟环境的工具，适用于所有版本的 Python。仍然建议尽可能使用官方的 Python `venv`。但是，举例来说，如果你正在基于`2.7.13`创建一个虚拟环境，那么这是对`pyenv`的赞美。

如果你已经在使用的话，它也能很好地与 [Anaconda 和 Miniconda](https://conda.io/docs/glossary.html#anaconda-glossary)环境一起工作。一个叫做`virtualenv`的工具也存在。这里不涉及，但是最后有链接。

安装`pyenv`后，接下来可以使用自制软件([或替代软件](https://github.com/pyenv/pyenv-virtualenv))进行安装，如下所示:

```
$ brew install pyenv-virtualenv
```

接下来，在您的`.zshrc`、`.bashrc`或`.bash_profile`(取决于您使用的 shell)中，向底部添加以下内容:

```
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

这允许`pyenv`在移动目录时自动激活和停用环境。

要创建新的虚拟环境，请使用:

```
$ pyenv virtualenv <version> <name-to-give-it>

// for example

$ pyenv virtualenv 2.7.10 my-virtual-env-2.7.10
```

现有环境可以列出:

```
$ pyenv virtualenvs
```

激活/停用:

```
$ pyenv activate <name>
$ pyenv deactivate
```

在写入时，当使用`activate`时，将显示警告`prompt changing will be removed from future release`。这是[所期望的](https://github.com/pyenv/pyenv-virtualenv/issues/135#issuecomment-386154344)，并且仅指在你的外壳中显示的`(env-name)`，而不是`activate`命令本身的使用。

安装要求工作如`venv`所述。与在`venv`中不需要`rm -r`命令来移除环境不同，存在`[uninstall](https://github.com/pyenv/pyenv-virtualenv#delete-existing-virtualenv)` [命令](https://github.com/pyenv/pyenv-virtualenv#delete-existing-virtualenv)。

### 最后的想法

在这三个工具之间，我们能够在任何项目上进行协作，无论 Python 的版本或所需的依赖关系如何。我们还知道如何记录安装说明，供其他人在我们从事的任何项目中使用。

我们还可以看到使用哪一套背后的推理，因为不是所有的开发人员都需要这三个。

希望这是有帮助的，并且结合下面链接的文档是一个有用的参考。

感谢阅读！？

### 我探索的其他事情:

*   [用 jest.mock()](https://medium.com/codeclan/mocking-es-and-commonjs-modules-with-jest-mock-37bbb552da43) 模仿 ES 和 CommonJS 模块
*   [亚马逊弹性容器服务入门指南](https://www.freecodecamp.org/news/amazon-ecs-terms-and-architecture-807d8c4960fd/)

### 资源

*   [Python 虚拟环境:初级读本](https://realpython.com/python-virtual-environments-a-primer/)
*   [贬值`pyvenv`](https://bugs.python.org/issue25154)
*   [Python `venv`文档](https://docs.python.org/3/library/venv.html)
*   `[venv](https://www.reddit.com/r/learnpython/comments/4hsudz/pyvenv_vs_virtualenv/)` [vs `virtualenv`](https://www.reddit.com/r/learnpython/comments/4hsudz/pyvenv_vs_virtualenv/)
*   [venv，pyvenv，pyenv，virtualenv，virtualenvwrapper，pipenv 等有什么区别？](https://stackoverflow.com/questions/41573587/what-is-the-difference-between-venv-pyvenv-pyenv-virtualenv-virtualenvwrappe)
*   [我需要安装`pip`吗？](https://pip.pypa.io/en/stable/installing/#installation)