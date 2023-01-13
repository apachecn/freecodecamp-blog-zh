# 如何在 Ubuntu 20.04 上设置 Python 虚拟环境

> 原文：<https://www.freecodecamp.org/news/how-to-set-up-python-virtual-environment-on-ubuntu-20-04/>

我最近给自己买了一台“新”笔记本电脑——联想 x270(耶)！我再次需要建立一个 Python 虚拟环境。所以我当然谷歌了一个解决方案，只是为了找到我之前写的关于同一主题的文章。

因此，在本文中，我将根据我新获得的知识更新说明。

让我告诉你，这比以前更容易，因为我们将只做两件事:

*   安装 virtualenvwrapper
*   编辑。bashrc 文件

## 先决条件

在本文中，我将向您展示如何使用 pip 3(Python 3 的 pip)设置 virtualenvwrapper。我们不打算使用 Python 2，因为[它不再被支持](https://www.python.org/doc/sunset-python-2/)。

要完成本教程，你需要一台安装了 Ubuntu 20.04 的电脑和一个互联网连接。此外，终端和 Vim 编辑器的一些知识将是有用的。

## 设置虚拟环境

现在，右键单击并选择“在终端中打开”选项，在主目录中打开您的终端。您也可以同时按下键盘上的 CTRL、ALT 和 T 键来自动打开终端应用程序。

您首先需要创建一个特殊的目录来保存您的所有虚拟环境。因此，继续创建一个名为 virtualenv 的新隐藏目录:

```
mkdir .virtualenv
```

## pip3

现在您应该为 Python3 安装 pip:

```
sudo apt install python3-pip
```

确认 pip3 安装:

```
pip3 -V
```

## virtualenvwrapper(虚拟包装器)

virtualenvwrapper 是 virtualenv 的一组扩展。它提供了 mkvirtualenv、lssitepackages 等命令，尤其是用于在不同 virtualenv 环境之间切换的 workon。

通过 pip3 安装 virtualenvwrapper:

```
pip3 install virtualenvwrapper
```

## bashrc 文件

我们将修改您的。bashrc 文件，通过添加一行来调整每个新的虚拟环境以使用 Python 3。我们将虚拟环境指向我们在上面创建的目录(。virtualenv ),我们还将指向 virtualenv 和 virtualenvwrapper 的位置。

现在打开。使用 Vim 编辑器的 bashrc 文件:

```
vim .bashrc
```

如果你以前还没有使用过 Vim 或者你的电脑上没有安装它，你现在应该安装它。它是使用最广泛的 Linux 编辑器之一，理由很充分。

```
sudo apt install vim
```

安装完 Vim 后，打开。bashrc 文件通过在终端中键入`vim .bashrc` 命令。导航到的底部。bashrc 文件，按字母 ***i*** 进入 Vim 中的插入模式，并添加这几行:

```
#Virtualenvwrapper settings:
export WORKON_HOME=$HOME/.virtualenvs
VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
. /usr/local/bin/virtualenvwrapper.sh 
```

完成后，按下 ***esc*** 键，然后键入 **: *wq*** 并按回车键。这个命令将保存文件并退出 Vim。

现在您需要重新加载 bashrc 脚本。有两种方法可以做到这一点——关闭并重新打开您的终端，或者在终端中执行此命令:

```
source ~/.bashrc
```

要在 Python3 中创建一个虚拟环境并立即激活它，请在终端中使用以下命令:

```
mkvirtualenv name_of_your_env
```

要停用环境，请使用停用命令。

要列出所有可用的虚拟环境，请在您的终端中使用命令 *workon* 或*lsvirtualenv*(lsvirtualenv 将显示与 workon 相同的结果，但方式更好):

```
workon
```

```
lsvirtualenv
```

要激活一个特定的环境，请使用您的环境的 workon +名称:

```
workon name_of_your_env
```

有几个有用命令您可能需要在某一天使用:

*Rmvirtualenv* 将删除位于您的中的特定虚拟环境。虚拟目录。

```
rmvirtualenv name_of_your_env
```

*Cpvirtualenv* 会将现有的虚拟环境复制到一个新的虚拟环境中并激活它。

```
cpvirtualenv old_virtual_env new_virtual_env
```

干得好！现在，您已经创建了第一个独立的 Python 3 环境。

感谢您的阅读！

在我的 [freeCodeCamp 个人资料](https://www.freecodecamp.org/news/author/goran/)、 [Medium 个人资料](https://medium.com/@goranaviani)上查看更多类似的文章，以及我在 [GitHub 页面](https://github.com/GoranAviani)上创建的其他有趣的东西。