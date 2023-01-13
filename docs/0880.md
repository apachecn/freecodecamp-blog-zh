# 如何创建第一个 Python 包并上传到 PyPI

> 原文：<https://www.freecodecamp.org/news/how-to-create-and-upload-your-first-python-package-to-pypi/>

几周前，我想学习如何构建我的第一个 Python 包，我试图找到从哪里开始。

嗯，我很困惑，也有点紧张，试图找到一个简单易行的教程来开始。出于这个原因，我决定写这篇教程，记录我如何构建我的第一个 Python 包。

## Python 中的包是什么？

在我们开始之前，我们应该知道包在 Python 中的意思。

Python 包是一个目录，其中包含一系列模块，并带有一个名为`__init__.py`的依赖文件。这个文件可以完全为空，您可以使用它将磁盘上的目录标记为 Python 包。

下面显示了一个软件包目录的示例:

```
package
	__init__.py
	module_a.py
	module_b.py
	module_c.py 
```

An example of a package directory

`__init__.py`是一个依赖文件，帮助 Python 在我们的包目录中寻找可用的模块。如果我们删除这个文件，Python 将无法导入我们的模块。

## Python 中的包与模块

您现在应该明白 Python 包创建了一个包含几个模块的结构化目录，但是模块呢？让我们确保理解包和模块之间的区别:

一个**模块**总是对应一个单独的 Python 文件 *turtle.py* 。它包含类、函数和常量等逻辑。

一个**包**基本上是一个包含许多模块或子包的模块。

## 程序包结构

但是，包不仅仅包含模块。它们也包括顶级脚本、文档和测试。以下示例显示了如何构建基本 Python 包:

```
package_name/
	docs/
	scripts/
	src/
		package_a
			__init__.py
			module_a.py
		package_b
			__init__.py
			module_b.py
	tests/
    	__init__.py
		test_module_a.py
		test_module_b.py
	LICENSE.txt
	CHANGES.txt
	MANIFEST.in
	README.txt
	pyproject.toml
	setup.py
	setup.cfg 
```

An example of a package structure

让我们了解一下上面树中每个文件的用途:

*   **包名**:代表主包。
*   **docs** :包括如何使用软件包的文档文件。
*   **脚本/** :你的顶层脚本。
*   src :你的代码去哪里。它包含包、模块、子包等等。
*   **测试**:您可以在其中放置单元测试。
*   **LICENSE.txt** :包含许可证的文本(例如 MIT)。
*   **CHANGES.txt** :报告每个版本的变更。
*   **MANIFEST.in** :您可以在这里放置关于您想要包含哪些额外文件(非代码文件)的指令。
*   **README.txt** :包含包描述(markdown 格式)。
*   **pyproject.toml** :注册你的构建工具。
*   **setup.py** :包含您的构建工具的构建脚本。
*   **setup.cfg** :你的构建工具的配置文件。

注意，有两种方法可以将我们的测试文件包含在我们的主包中。我们可以像上面一样将它放在顶层，或者像下面这样将它放在包内:

```
package_name/
      __init__.py
      module_a.py
      module_b.py
      test/
          __init__.py
          test_module_a.py
          test_module_b.py 
```

An example of self-contained tests

在我看来，我认为将测试保持在最高水平会有很大帮助，尤其是当我们的测试需要读写其他外部文件的时候。

## 应该用 setup.cfg 还是 setup.py？

**setup.py** 和 **setup.cfg** 是 PyPI、setuptools、pip 和标准 python 库中的默认打包工具。

在这里，它们代表 setuptools 的配置和构建脚本。它们都告诉 setuptools 如何构建和安装软件包。

提到的文件包含了版本、包和要包含的文件等信息，以及任何需求。

下面展示了一个使用一些`setup()`参数的`setup.py`的例子。你可以在这里找到更多的论据:

```
import setuptools

with open("README.md", "r", encoding = "utf-8") as fh:
    long_description = fh.read()

setuptools.setup(
    name = "package-name",
    version = "0.0.1",
    author = "author",
    author_email = "author@example.com",
    description = "short package description",
    long_description = long_description,
    long_description_content_type = "text/markdown",
    url = "package URL",
    project_urls = {
        "Bug Tracker": "package issues URL",
    },
    classifiers = [
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
    package_dir = {"": "src"},
    packages = setuptools.find_packages(where="src"),
    python_requires = ">=3.6"
) 
```

setup.py

与`setup.py`相比，`setup.cfg`是以不同的格式编写的，并且基本上包含两个基本键，`command`和`options`。

`command`键代表 **distutils** 命令中的一个，而`options`键定义了该命令可以支持的选项。

```
[command]
option = value 
```

setup.cfg

下面展示了一个使用一些元数据和选项的`setup.cfg`的例子。你可以在这里找到各种元数据和选项:

```
[metadata]
name = package-name
version = 0.0.1
author = name of the author
author_email = author@example.com
description = short package description
long_description = file: README.md
long_description_content_type = text/markdown
url = package url
project_urls =
    Bug Tracker = package issues url
classifiers =
    Programming Language :: Python :: 3
    License :: OSI Approved :: MIT License
    Operating System :: OS Independent

[options]
package_dir =
    = src
packages = find:
python_requires = >=3.6

[options.packages.find]
where = src 
```

setup.cfg

`setup.py`和`setup.cfg`都是 setuptools 特有的。同样，`setup.cfg`也可以安全的移动到`pyproject.toml`。

这里的想法是，也许有一天我们会想切换到其他构建系统，如 **flit** 或**poems**。在这种情况下，我们需要做的就是将`pyproject.toml`中的构建系统条目(例如 setuptools)更改为类似 flit 或诗歌的内容，而不是从头开始。

[在这里](https://packaging.python.org/en/latest/key_projects/#build)您可以找到关于构建和分发 Python 包的其他工具的信息。

无论我们选择哪个配置文件，我们都被“锁定”在维护那个特定的配置文件，pyproject.toml、setup.cfg 或 setup.py。

根据 [Python 打包用户指南](https://packaging.python.org/en/latest/)的说法，`setup.cfg`是首选，因为它是静态的、干净的、更容易阅读的，并且避免了编码错误。

## 如何构建您的第一个 Python 包

现在，是时候开始构建一个简单的 Python 包了。我们将使用 setuptools 作为构建系统，并使用`setup.cfg`和`pyproject.toml`来配置我们的项目。

### 设置包文件

对于这个简单的包，我们需要通过添加需要的依赖文件来构建我们的目录，以使包为分发做好准备。这就是我们包装的结构:

```
basicpkg/
	src/
		divide
			__init__.py
			divide_by_three.py
		multiply
			__init__.py
			multiply_by_three.py
	tests/
		__init__.py
		test_divide_by_three.py
		test_multiply_by_three.py
	LICENSE.txt
	README.txt
	pyproject.toml
	setup.cfg 
```

Our package structure

我们的主包由两个包组成:第一个包将数字除以 3，另一个包将数字乘以 3。

此外，我们忽略了一些文件，如`CONTEXT.txt`、`MANIFEST.in`和`docs/`目录，以保持简单。但是我们需要`test/`目录来包含我们的单元测试来测试包的行为。

### 给我们的模块添加一些逻辑

一如既往，我们将保持`__init__.py`为空。然后，我们需要在模块中放置一些逻辑来执行我们的操作。

对于 divide 包，我们把下面的内容加入到`divide_by_three.py`中，将任意一个数除以三:

```
def divide_by_three(num):
	return num / 3 
```

divide_by_three.py

同样的逻辑也适用于乘法包内的`multiply_by_three.py`。但是，这一次是将任何数字乘以三:

```
def multiply_by_three(num):
	return num * 3 
```

multiply_by_three.py

随意添加更多的包和模块来执行其他类型的操作。例如，您可以添加包来完成加法和减法任务。

### 测试我们的模块

练习将自动化测试添加到我们创建的任何程序中是很好的。我们将使用`unittest`来测试我们的模块和包的行为。

在`test/`目录中，让我们将下面的代码添加到`test_divide_by_three.py`中:

```
import unittest
from divide.by_three import divide_by_three 

class TestDivideByThree(unittest.TestCase):

	def test_divide_by_three(self):
		self.assertEqual(divide_by_three(12), 4)

unittest.main() 
```

test_divide_by_three.py

我们从`unittest`导入测试用例来执行我们的自动化测试。然后，我们从位于 divide 包内的`by_three`模块中导入了我们的除法方法`divide_by_three()`。

如果我们删除这里的`__init__.py`，Python 将不再能够找到我们的模块。

`.assertEqual()`用于检查上述两个值是否相等(除以 3(12)和 4)。`unittest.main()`被实例化来运行我们所有的测试。

同样的逻辑也适用于`test_multiply_by_three.py`:

```
import unittest
from multiply.by_three import multiply_by_three

class TestMultiplyByThree(unittest.TestCase):

	def test_multiply_by_three(self):
		self.assertEqual(multiply_by_three(3), 9)

unittest.main() 
```

test_multiply_by_three.py

要运行测试，请在终端/命令中键入以下内容:

对于 Linux:

```
python3 tests/test_divide_by_three.py 
```

对于 Windows:

```
py tests/test_divide_by_three.py 
```

用同样的方法测试乘法模块。如果我们的测试成功运行，您应该得到以下内容:

```
.
----------------------------------------------------------------------
Ran 1 test in 0.000s

OK 
```

如果你添加额外的包和模块，试着给它们添加一些`unittest`方法。这对你来说是一个很好的挑战。

### 使用 setup.cfg 配置元数据

接下来，我们需要为 setuptools 添加一个配置文件。如前所述，这个配置文件将告诉 setuptools 如何构建和安装我们的包。

因此，我们需要向我们的`setup.cfg`添加以下元数据和选项。然后，不要忘记选择一个不同的名字，因为我已经上传了这个包到 [TestPyPi](https://test.pypi.org/project/basicpkg/) 。此外，更改其他信息，如作者、电子邮件和项目 URL，以分发包含您的信息的包。

```
[metadata]
name = basicpkg
version = 1.0.0
author = your name
author_email = your email
description = A simple Python package
long_description = file: README.md, LICENSE.txt
long_description_content_type = text/markdown
url = https://gitlab.com/codasteroid/basicpkg
project_urls =
    Bug Tracker = https://gitlab.com/codasteroid/basicpkg/-/issues
    repository = https://gitlab.com/codasteroid/basicpkg
classifiers =
    Programming Language :: Python :: 3
    License :: OSI Approved :: MIT License
    Operating System :: OS Independent

[options]
package_dir =
    = src
packages = find:
python_requires = >=3.6

[options.packages.find]
where = src 
```

setup.cfg

你应该保持选项类别中的所有默认设置。package_dir 定位您的包、模块和所有 Python 源文件所在的根包。

在 packages 键中，我们可以像这样手动列出我们的包`[divide, multiply]`。但是如果我们想要得到所有的包，我们可以使用`find:`并指定我们需要在哪里找到这些包，方法是使用`[options.packages.find]`并把`where`键分配给根包的名称。

请务必在您的配置文件中包含`classifiers`键，以添加一些重要信息，如 Python 的版本和我们的包适用的操作系统。你可以在这里找到分类器[的完整列表。](https://pypi.org/classifiers/)

### 创建 pyproject.toml

我们将使用 setuptools 作为构建系统。为了告诉`pip`或其他构建工具我们的构建系统，我们需要两个变量，如下所示。

我们使用`build-system.require`来包含构建我们的包所需的内容，而`build-system.build-back-end`定义了将执行构建的对象。

那么，让我们在`pyproject.toml`中输入以下内容:

```
[build-system]
requires = ['setuptools>=42']
build-backend = 'setuptools.build_meta' 
```

pyproject.toml

注意，例如，如果您决定将构建系统更改为 flit 或 poem，您可以安全地将`setup.cfg`中的所有配置设置移动到`pyproject.toml`。这会节省你的时间。

### 创建 README.md

创造一个好的`README.md`很重要。让我们为我们的包添加一个描述，并包括一些关于如何安装它的说明:

```
# `basicpkg`

The `basicpkg` is a simple testing example to understand the basics of developing your first Python package. 
```

README.md

我们还可以像这样添加如何使用我们的包:

```
from multiply.by_three import multiply_by_three
from divide.by_three import divide_by_three

multiply_by_three(9)
divide_by_three(21) 
```

README.md

请随意添加任何可以帮助其他开发人员理解您的包的用途的信息，以及一些关于如何安装和正确使用它的说明。

请注意，我们的配置文件将加载`README.md`，并且将在我们分发我们的包时包含在内。

### 添加许可证

在你的项目中添加一个许可让用户知道他们如何使用你的代码是非常重要的。让我们为我们的 Python 包选择一个 MIT 许可证，并将以下内容添加到`LICENSE.txt`:

```
MIT License

Copyright (c) [year] [fullname]

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE. 
```

LICENSE.txt

不要忘记将[year]替换为当前年份，将[full name]替换为您的姓名或版权所有者的姓名。

### 生成分发档案

让我们的包为分发做好准备还有一步:为我们的 Python 包生成分发档案。为此，首先我们需要更新我们的 PyPA 构建，然后生成源代码和构建的档案。

在 terminal/cmd 中，从`pyproject.toml`文件所在的同一目录运行以下命令:

对于 Linux:

```
python3 -m pip install --upgrade build
python3 -m build 
```

对于 Windows:

```
py -m pip install --upgrade build
py -m build 
```

一旦上述过程完成，就会生成一个名为`dist/`的新目录，其中包含两个文件。`.tag.tz`文件是源归档文件，`.whl*`文件是构建的归档文件。这些文件代表了我们的 Python 包的分发档案，它将被上传到 Python 包索引，并由`pip`在下面的部分中安装。

## 如何用 Python 上传包

Python 包索引是我们应该上传项目的地方。因为我们的 Python 包还在测试中，我们可能会添加其他功能来试验它，所以我们应该使用一个单独的 PyPI 实例 TestPyPI。这个实例是专门为测试和实验而实现的。然后，一旦您的包准备好并符合您的期望，您就可以将它作为一个真正的包上传到 PyPI。

让我们按照下面的说明让我们的 TestPyPI 准备好上传我们的包:

1.  转到 [TestPyPI](https://test.pypi.org/) 并创建一个帐户。
2.  验证您的电子邮件地址，以便您可以上传包。
3.  更新您的个人资料设置(添加您的图片等)。
4.  转到 [api-tokens](https://test.pypi.org/manage/account/#api-tokens) 并创建您的 api 令牌以安全上传您的包。
5.  在同一页面上，将范围设置为“整个帐户”。
6.  将您的令牌复制并保存在磁盘上的安全位置。

接下来，我们需要上传我们的发行版档案。为此，我们必须使用上传工具来上传我们的包。官方的 PyPI 上传工具是 **twine** ，所以让我们安装 twine 并上传我们在`dist/`目录下的发行版档案。

在 terminal/cmd 中，从`pyproject.toml`文件所在的同一目录运行以下命令:

对于 Linux:

```
python3 -m pip install --upgrade twine
python3 -m twine upload --repository testpypi dist/* 
```

对于 Windows:

```
py -m pip install --upgrade twine
py -m twine upload --repository testpypi dist/* 
```

然后，输入`__token__`。作为用户名，您保存的令牌(包括 pypi 前缀)作为密码。按回车键上传分配。

## 如何安装上传的 Python 包

现在，是时候安装我们的软件包了。您可以创建一个新的虚拟环境，并使用`pip`从 TestPyPI 安装它:

对于 Linux:

```
python3 -m venv env
source env/bin/activate
(env) python3 -m pip install --index-url https://test.pypi.org/simple/ --no-deps basicpkg 
```

对于 Windows:

```
py -m venv env
.\env\Scripts\activate
(env) py -m pip install --index-url https://test.pypi.org/simple/ --no-deps basicpkg 
```

在验证我们的软件包是否正常工作之前，请确保您的虚拟环境已激活。

在终端/命令中，对于 Linux 用户运行`python3`，或者对于 Windows 用户运行`py`。然后，键入以下代码以确保乘除包按预期工作:

```
from multiply.by_three import multiply_by_three
from divide.by_three import divide_by_three

multiply_by_three(9)
divide_by_three(21)

# Output: 27
# Output: 7 
```

请记住，我们需要从我们的模块中导入我们需要执行所需操作的方法，就像我们上面所做的那样。

万岁！我们的软件包如预期的那样工作。

因此，一旦您测试和试验了您的包，请按照下面的说明将您的包上传到真正的 PyPI:

1.  转到 [PyPI](https://pypi.org/) 并创建一个帐户。
2.  在终端/命令行运行`twine upload dist/*`。
3.  输入您在实际 PyPI 上注册的帐户凭证。
4.  然后，运行`pip install [package_name]`来安装您的软件包。

恭喜你！您的软件包是从真正的 PyPI 安装的。

## 下一步是什么？

如果你有一个简单的想法，并用它来构建你的第一个真正的 Python 包，那就太好了。在这篇博文中，我只关注了入门所需的基础知识，但是在包装领域还有很多东西需要学习。

希望我开发 Python 包的第一次经历能帮助你了解你需要构建什么。如果您有任何问题，请随时联系我，并在 [LinkedIn](https://www.linkedin.com/in/rochdi-khalid/) 上联系我。此外，你可以在 YouTube 上订阅我的[频道](https://www.youtube.com/channel/UCF8iZXSsjgc8kE8hITp5rdQ)，在那里我分享我学习和用代码构建的视频。

下期帖子再见，继续前进！

### 参考

以下是一些帮助我开发第一个 Python 包的参考资料:

*   [打包 Python 项目](https://packaging.python.org/en/latest/tutorials/packaging-projects/)
*   [配置元数据](https://packaging.python.org/en/latest/tutorials/packaging-projects/#configuring-metadata)
*   [PEP 621–在 pyproject.toml 中存储项目元数据](https://peps.python.org/pep-0621/#example)
*   [词汇表- Python 打包用户指南](https://packaging.python.org/en/latest/glossary/#term-Source-Archive)