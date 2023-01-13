# 如何使用 Sphinx 工具记录您的 Django 项目

> 原文：<https://www.freecodecamp.org/news/sphinx-for-django-documentation-2454e924b3bc/>

我最近参观了一家公司，在那里我和它的一名员工进行了愉快的交谈。我们讨论了技术和编程。然后我们谈到了项目文档的主题。具体来说，它是如何自动反应的，但 Django 不是。这让我觉得我应该为我的 Django 项目做一些自动文档。

我找不到任何关于它是如何完成的相关文档，所以它比我最初计划的时间要长一点。不是因为这很难，而是因为我发现狮身人面像官方文档和其他资源已经过时或晦涩难懂。

所以今天我做了一个简单但全面的教程，解释了如何使用 Ubuntu 中的 Sphinx 文档工具制作 Django 文档。

#### **安装斯芬克斯**

首先，您应该进入 Django 项目的虚拟环境。

使用 pip 3(Python 3 的 pip)安装 Sphinx 非常简单:

```
pip3 install sphinx
```

#### 创建文档目录

一旦安装了 Sphinx，您将需要创建文档根文件夹。该文件夹将保存您的文档和您需要的其他文件(图像、关于页面等)。

在项目主文件夹中创建文档根文件夹，并将其命名为/docs。

要启动 Sphinx，请在/docs 文件夹中运行以下命令:

```
sphinx-quickstart
```

你现在有很多选择。在大多数情况下，您可以简单地重新键入默认选项，但有一些选项您需要注意。

我是这样回答的:

```
Welcome to the Sphinx 1.7.5 quickstart utility.

Please enter values for the following settings (just press Enter to
accept a default value, if one is given in brackets).

Selected root path: .

You have two options for placing the build directory for Sphinx output.
Either, you use a directory “_build” within the root path, or you separate
“source” and “build” directories within the root path.

> Separate source and build directories (y/n) [n]: n

Inside the root directory, two more directories will be created; “_templates”
for custom HTML templates and “_static” for custom stylesheets and other static
files. You can enter another prefix (such as “.”) to replace the underscore.

> Name prefix for templates and static dir [_]: _

The project name will occur in several places in the built documentation.
> Project name: Your_project_name
> Author name(s): Goran Aviani
> Project release []: 1.0

If the documents are to be written in a language other than English,
you can select a language here by its language code. Sphinx will then
translate text that it generates into that language.

For a list of supported codes, see
http://sphinx-doc.org/config.html#confval-language.

> Project language [en]: en

The file name suffix for source files. Commonly, this is either “.txt”
or “.rst”. Only files with this suffix are considered documents.

> Source file suffix [.rst]: .rst

One document is special in that it is considered the top node of the
“contents tree”, that is, it is the root of the hierarchical structure
of the documents. Normally, this is “index”, but if your “index”
document is a custom template, you can also set this to another filename.

> Name of your master document (without suffix) [index]: index

Sphinx can also add configuration for epub output:

> Do you want to use the epub builder (y/n) [n]: n

Indicate which of the following Sphinx extensions should be enabled:

> autodoc: automatically insert docstrings from modules (y/n) [n]: y
> doctest: automatically test code snippets in doctest blocks (y/n) [n]: y
> intersphinx: link between Sphinx documentation of different projects (y/n) [n]: n
> todo: write “todo” entries that can be shown or hidden on build (y/n) [n]: y
> coverage: checks for documentation coverage (y/n) [n]: y
> imgmath: include math, rendered as PNG or SVG images (y/n) [n]: y
> mathjax: include math, rendered in the browser by MathJax (y/n) [n]: n
> ifconfig: conditional inclusion of content based on config values (y/n) [n]: n
> viewcode: include links to the source code of documented Python objects (y/n) [n]: n
> githubpages: create .nojekyll file to publish the document on GitHub pages (y/n) [n]: n
A Makefile and a Windows command file can be generated for you so that you
only have to run e.g. `make html’ instead of invoking sphinx-build
directly.
> Create Makefile? (y/n) [y]: y
> Create Windows command file? (y/n) [y]: y

Creating file ./conf.py.
Creating file ./index.rst.
Creating file ./Makefile.
Creating file ./make.bat.

Finished: An initial directory structure has been created.

You should now populate your master file ./index.rst and create other documentation
source files. Use the Makefile to build the docs, like so:

make builder

where “builder” is one of the supported builders, e.g. html, latex or linkcheck.
```

#### 姜戈连接

在您的项目文件夹中，找到/docs/conf.py，并在其中靠近文件顶部的某个位置找到“#import os”。就在它下面，写下这个:

```
import os
import sys
import django
sys.path.insert(0, os.path.abspath('..'))
os.environ['DJANGO_SETTINGS_MODULE'] = 'Your_project_name.settings'
django.setup()
```

**现在有两种方法可以继续:**

1.  您可以使用 *sphinx-apidoc* 来生成完全自动的文档，或者
2.  您可以构建自己的模块，这些模块将显示在文档中。

在本教程中，我将向你展示如何做到这两种方式。

#### 1.狮身人面像

这是一种更简单的方法，您只需导航到/docs 文件夹并执行:

```
sphinx-apidoc -o . ..
```

现在您需要通过运行以下命令来构建您的文档:

```
make html
```

导航到*Your _ project _ folder/docs/_ build/html*并打开【index.html 。这是您文档的索引页。

#### 2.构建您自己的模块

这是稍微复杂一点的方法，但是会给你更多的自由来组织你的文档。

现在，您将需要制作关于您的视图、模块等的文档。但首先让我给你看一个简单的例子，以便你理解这部分的逻辑:

进入/docs 文件夹，创建一个名为“/modules”的新文件夹。在其中创建一个名为 all-about-me.rst 的文件:

```
mkdir modulesf
touch modules/all-about-me.rst
```

在 all-about-me.rst 中写下这样的内容:

```
############
All about me
############

I’m Goran Aviani, a Django developer.
```

现在，您已经创建了一些要在文档中显示的内容，但是您仍然没有一个实际的位置来显示它。返回到/docs 文件夹，打开 index.rst，执行下面的代码

```
.. toctree::
   :maxdepth: 2
   :caption: Contents:
```

添加这个:

```
modules/all-about-me.rst
```

使上面的代码和添加的行之间有一个空行:

```
.. toctree::
   :maxdepth: 2
   :caption: Contents:

   modules/all-about-me.rst
```

现在您需要构建您的文档。将位置更改为包含 Makefile 的文件夹(即/docs 文件夹)。然后在终端中运行:

```
make html
```

您可以在中找到您的文档

> 您的 _ project _ folder/docs/_ build/html 并打开 index.html

您可以对 Django 视图进行同样的操作:

在/modules 文件夹中，创建 views.rst 文件。

```
touch views.rst
```

在 views.rst 文件中编写以下内容:

```
Views
======

.. automodule:: yourapp.views
   :members:
   :undoc-members:
```

在 index.rst 中，在 modules/all-about-me.rst 下添加以下内容:

```
modules/views.rst
```

现在，您再次需要通过在/docs 文件夹中运行“make html”来构建您的文档:

```
make html
```

明白了吗？首先创建。rst 文件，然后将它放在 index.rst 中，这样它就可以显示在 index.rst 页面上。

您可以为您模型做同样的东西。进入/modules 文件夹并创建 models.rst 文件。

```
touch models.rst
```

您可以在 models.rst 文件中添加如下内容:

```
Models
=======

.. automodule:: yourapp.models
   :members:
   :undoc-members:
```

在 index.rst 内模块/视图下粘贴:

```
modules/models.rst
```

在/docs 文件夹中运行:

```
make html
```

#### 文档测试

您可以通过在/docs 文件夹中运行以下代码来测试您的文档:

```
make linkcheck
```

瞧啊。你完了！

这是我第一次公开教程，喜欢就给我几个掌声吧:)

感谢您的阅读！在我的 freeCodeCamp 个人资料上查看更多类似的文章:[https://www.freecodecamp.org/news/author/goran/](https://www.freecodecamp.org/news/author/goran/)和我在 GitHub 页面上创建的其他有趣的东西:[https://github.com/GoranAviani](https://github.com/GoranAviani)