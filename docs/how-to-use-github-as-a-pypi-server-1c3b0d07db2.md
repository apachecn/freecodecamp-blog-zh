# 如何使用 GitHub 作为 PyPi 服务器

> 原文：<https://www.freecodecamp.org/news/how-to-use-github-as-a-pypi-server-1c3b0d07db2/>

我在寻找一个托管的私有 PyPi Python 包服务器，它使用团队已经拥有的凭证(比如 GitHub)。

我不想创建本地服务器。对我们来说，这将使我们无法使用基于云的构建服务器，这是另一个可能出错的移动部分。细粒度的安全性和速度也存在潜在的问题。(我们有一个全球团队，所以通过 [CDN](https://www.webopedia.com/TERM/C/CDN.html) 提供内容会很有帮助。)

我不想强迫团队创建另一个提供商的帐户。他们已经有了 Active Directory 和 GitHub 账户。这对他们来说是一种烦恼，对我来说是一种治理负担。

遗憾的是，我找不到这样的服务。 [GemFury](https://gemfury.com/) 很优秀，但是不支持 GitHub 授权(在团队/组织层面)，而[packar](https://www.packagr.app)根本不支持 GitHub 授权。 [MyGet](https://docs.myget.org/) 也很优秀，它确实允许我使用 GitHub 授权，但是不托管 Python 包。Azure DevOps 有一些看起来很有前景的东西，但目前还处于私人测试阶段。

令人高兴的是，使用 GitHub、GitLab 和 BitBucket 等云 Git 库，这是可能的。

### Pip 可以从 Git 安装软件包

我在 GitHub 上托管了一个 Python 包( [python_world](https://github.com/ceddlyburge/python_world) )，你可以用下面的命令安装它(在运行这个命令并在你的计算机上安装我的代码之前，确保你信任我)。

`pip install git+https://github.com/ceddlyburge/python_world#egg=python_world`

Pip 提供了从头安装、从分支安装、从标记安装或从提交安装的选项。我通常标记每个版本，并从这些标记安装。参见 [pip 安装文件了解全部细节](https://pip.pypa.io/en/stable/reference/pip_install/#git)。

这个存储库是公共的，但是只要你有权限，它与私有回购是一样的。没有什么特别的魔力(这是一个普通的 Python 包), [Setup.py](https://github.com/ceddlyburge/python_world/blob/master/setup.py) 照常完成大部分工作。

如果你是创建 Python 包的新手，那么[打包 Python 项目教程](https://packaging.python.org/tutorials/packaging-projects/)值得一读。

### Setuptools 也可以从 Git 安装依赖项

大多数人都是这样创建 Python 包的。

我在 GitHub [python_hello](https://github.com/ceddlyburge/python_hello) 上托管过另一个包，这个包依赖于 [python_world](https://github.com/ceddlyburge/python_world) 。(我相信你能看出这是怎么回事。)

setup.py 中的相关位如下。`install_requires`指定`python_world`是必需的依赖项，并告诉 Setuptools 在哪里可以找到它。

```
install_requires=[
	'python_world@git+https://github.com/ceddlyburge/python_world#egg=python_world-0.0.1',
]
```

您可以使用下面的命令安装这个软件包。它还会下载相关的`python_world`包。

`pip install git+https://github.com/ceddlyburge/python_hello#egg=python_hello`

这链接到了`python_world`的一个特定版本，这是一个遗憾，因为这意味着 pip 不能进行任何依赖管理(例如，如果多个东西都依赖于它，就制定出一个可接受的版本)。但是，在本文结束时，我们将不再需要特定的链接。

### Python 环境

正如每个在没有环境的情况下使用过 Python 的人都知道的那样，环境节省了很多挫折和浪费的时间。所以我们需要支持他们。

我创建了一个 repo ( [use-hello-world](https://github.com/ceddlyburge/python_use_hello_world) )，将`python_hello`定义为 [Virtualenv](https://virtualenv.pypa.io/) 的 [requirements.txt](https://github.com/ceddlyburge/python_use_hello_world/blob/master/requirements.txt) 中的依赖项，将 [environment.yml](https://github.com/ceddlyburge/python_use_hello_world/blob/master/environment.yml) 定义为 [Conda](https://www.anaconda.com) 中的依赖项。

如果您下载了 repo，您可以使用下面的命令将依赖项安装到 virtualenv 中。

`pip install -r requirements.txt`

如果您正在使用 conda，您可以使用以下命令:

`conda env create -n use-hello-world`

### pii 索引

到目前为止，我们能够从我们的私有 Git 库安装包。反过来，这些包可以定义对其他私有存储库的依赖。仍然看不到 PyPi 服务器。

我们可以就此打住。然而，定义依赖关系的语法有点神秘。团队很难发现哪些包是可用的，我们链接到依赖包的特定版本，而不是让 pip 管理它。

为了解决这个问题，我们可以设置一个符合 [Pep 503](https://www.python.org/dev/peps/pep-0503) 的 PyPi 指数。这个规范非常简单，我刚刚手工创建了索引。如果这变得太麻烦，我可以从 GitHub API 生成它。

我使用 GitHub 页面创建了这个 [PyPi 索引](https://ceddlyburge.github.io/python-package-server/)。GitLab 和 BitBucket 有等价的东西。你可以看到[的源代码](https://github.com/ceddlyburge/python-package-server/)非常简单。GitHub Pages 站点总是公开的(并且您的索引中可能没有敏感信息)。然而，如果你需要他们是私人的，你可以使用像 [PrivateHub](https://www.privatehub.cloud/) 这样的服务。

需要注意的一点是规范的[名称规范化](https://www.python.org/dev/peps/pep-0503/#normalized-names)。这要求`python_hello`包信息出现在`python-hello/index.html`(注意从下划线到破折号的变化)。

现在我们有了一个 PyPi 服务器，我们可以使用下面的命令安装包。

`pip install python_hello --extra-index-url [https://ceddlyburge.github.io/python-package-server/](https://ceddlyburge.github.io/python-package-server/)`

为了让您看到这一点，我创建了另一个 repo([use _ hello _ world _ from _ server](https://github.com/ceddlyburge/python_use_hello_world_from_server))，它使用这个 PyPi 索引而不是直接的 GitHub 链接来定义`python_hello`依赖关系。如果您尝试使用 Conda，则需要版本> 4.4。

此时，我们可以返回并删除 python_hello 的 setup.py 中的 [install_requires 中的直接 Git 链接(因为 Setuptools 将能够从我们的服务器中找到它)。](https://github.com/ceddlyburge/python_hello_world/blob/master/setup.py)

### 结论

使用云托管的 Git 提供者作为 PyPi 服务器是一个可行的选择。如果您已经在使用一个，这意味着您可以重用您已经拥有的凭据和权限。它将与云构建服务器一起工作，并可能通过 CDN 提供，因此将在全球范围内快速推广。与托管服务器相比，它需要更多的设置知识，但可能与在本地托管自己的服务器相同或更少。

### 提示和技巧

在本地提供索引有助于解决问题(如名称规范化)。很容易看到发出了什么请求。为此，您可以使用内置的 python HTTP 服务器(`python -m Http.Server -8000`)。这让我发现`pip search`使用了`post`请求，所以不能与 GitHub 页面一起工作。

您可以运行`python setup.py -install`在本地检查您的 pip 包，然后将它们推送到 Git。