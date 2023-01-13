# 如何在虚拟环境中安装 Flask

> 原文：<https://www.freecodecamp.org/news/how-to-install-flask-in-a-virtual-environment/>

如果你想用烧瓶，你来对地方了！如果你想用 Flask 探索 web 开发，本指南将教你如何安装它。

请记住，Flask 可能并不总是最佳选择——如果您是 Python web 开发的新手，用它构建大型 web 应用程序会变得很困难。或许去 Django 看看也是一个选择。

Flask 是一个微框架，你可以选择你想要的功能，而不是你已经拥有的标准 web 框架的基本准系统功能。

首先确保你已经安装了 Python 3 并且正在虚拟环境中使用它。

此外，请确保您已经不在虚拟环境中。然后创建一个新的虚拟环境，命名为`py3-flask`

```
$ mkvirtualenv py3-flask --python=/usr/bin/python3
```

现在，执行`workon`命令来查看您机器中的虚拟环境列表。这应该在一行中列出`py3-flask`。

此后，激活此环境:

```
$ workon py3-flask
```

您的虚拟环境将被一个 Python 解释器的副本激活，带有 Python 3 属性。你应该跑

```
$ python --version
```

以确保您确实处于 Python 3 环境中。

需要明确的是，如果你已经安装了 Django 或者其他一些框架，它应该是 ****而不是**** 在这个环境中。我们使用虚拟环境来保持不同框架的独立安装。

可以肯定的是，跑

```
pip freeze
```

确保 Django 不在上述命令生成的输出列表中。

现在，让我们安装烧瓶。如果你想了解更多，这里有[官方安装指南](http://flask.pocoo.org/docs/0.10/installation/)。然而，许多开发人员更喜欢用 Flask 安装一些额外的包来获得更多的功能。

要安装 Flask，执行

```
$ pip install flask
```

当您再次运行`pip freeze`时，它应该会在列出的包中显示`Flask`。

像这样运行长命令很麻烦。幸运的是，Python 领域中也有类似于`package.json`的东西——一个依赖项列表，包管理器可以用它来复制环境，方法是从中央存储库中下载合适的版本。

标准是使用`pip freeze`并将输出记录到一个本地文件，该文件可以被源代码控制。

```
$ pip freeze > requirements.txt
```

在安装了那些 Flask 包之后，下面是我的环境中的`requirements.txt`的内容。随着应用程序的增长，您可以添加或删除更多的包。但是现在，只需将以下内容复制并粘贴到与您所在目录相同的文本文件中。

```
Babel==2.2.0
Flask==0.10.1
Flask-Babel==0.9
Flask-Login==0.3.2
Flask-Mail==0.9.1
Flask-OpenID==1.2.5
Flask-SQLAlchemy==2.1
Flask-WTF==0.12
Flask-WhooshAlchemy==0.56
Jinja2==2.8
MarkupSafe==0.23
SQLAlchemy==1.0.12
Tempita==0.5.2
WTForms==2.1
Werkzeug==0.11.4
Whoosh==2.7.2
blinker==1.4
coverage==4.0.3
decorator==4.0.9
defusedxml==0.4.1
flipflop==1.0
guess-language==0.2
itsdangerous==0.24
pbr==1.8.1
python3-openid==3.0.9
pytz==2015.7
six==1.10.0
speaklater==1.3
sqlalchemy-migrate==0.10.0
sqlparse==0.1.18
```

这个包列表取自[这里](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world)。

一旦保存了文件，只需运行

```
$ pip install -r requirements.txt
```

软件包管理器将负责为您安装缺失的软件包！您应该将这个文件提交到您的源代码控制系统中。

上面的命令集假设您有一台 Linux 机器或 Mac OSX 机器。或者你正在使用 cloud9 或 Nitrous 上的云托管的盒子，或者你正在使用一个流浪者盒子。

但是，如果您必须使用 Windows 机器，请考虑使用 Windows Powershell，而不是 Windows CMD。大多数命令都是一样的。如果你需要任何帮助，你可能想看看这个堆栈溢出讨论。