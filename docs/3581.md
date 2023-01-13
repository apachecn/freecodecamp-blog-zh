# 如何用 Django 创建一个项目

> 原文：<https://www.freecodecamp.org/news/how-to-create-a-project-with-django/>

现在我们知道了如何创建虚拟环境和使用 pip，我们可以开始构建我们的项目了。在本文中，我们将创建我们的第一个 Django 项目，编写测试，并启动我们的开发服务器。

## **创建虚拟环境**

首先，让我们为这个项目创建一个新的虚拟环境。(如果您还没有，请通过在终端中键入`deactivate`来停用之前的 virtualenv)。更多关于虚拟环境以及如何使用它们的信息，请访问。

导航到 Django 项目所在的目录，并在终端中键入以下内容:

```
mkvirtualenv taskplanner --python=/usr/bin/python3
```

如果您的 Python 路径看起来与上面的不同，您可能需要更改它。

命令行 shell 现在应该如下所示，表明您处于虚拟环境中。

```
(taskplanner)<a href='https://sites.google.com/a/chromium.org/chromedriver/downloads' target='_blank' rel='nofollow'>munsterberg@Lenovo ~/workspace] $
```

如果看起来不是这样，只需键入:

```
workon taskplanner
```

我们现在可以安装 Django 了:

```
pip install Django
```

## **创建我们的 Django 项目**

安装 Django 后，我们可以创建我们的项目:

```
django-admin.py startproject taskplanner
```

接下来，通过键入以下命令导航到我们的新项目:

```
cd taskplanner
```

在我们做任何事情之前，让我们使用 virtualenvwrapper 将这个目录设置为我们的工作目录:

```
setvirtualenvproject
```

****Sidenote**** :要获得 virtualenvwrapper 命令的列表，请在终端中键入`virtualenvwrapper`。

现在，当我们在虚拟环境中时，我们可以键入`cdproject`直接导航到我们的工作目录。

您的项目目录应该如下所示:

```
taskplanner // our main working directory
|--- manage.py // similar to the django-admin script, you will see this used a
               // lot throughout our project
|--- taskplanner
    |--- __init__.py // this just tells python to treat this directory as a package
    |--- settings.py // main configuration file for our project
    |--- urls.py // we will use this to configure urls
    |--- wsgi.py // this is used for deploying our project to a production server
```

## **功能测试**

测试驱动开发是软件开发中广泛使用的最佳实践。基本上，我们希望首先编写一个注定会失败的测试，然后编写尽可能少的代码来通过该测试。对于 Django，我们的目标是编写两个功能测试(也称为；集成测试、端到端测试等。)，以及整个开发过程中的单元测试。别担心，测试并不像看起来那么难！

但是首先，我们需要创建一个新的专用于测试的虚拟环境。在终端中打开一个新选项卡，导航到 taskplanner 项目目录并键入:

```
mkvirtualenv taskplanner_test --python=/usr/bin/python3
```

现在，您应该在终端中打开了两个选项卡，一个在(taskplanner)虚拟环境中，另一个在(taskplanner_test)虚拟环境中。

如果您在我们的新测试环境(taskplanner_test)中键入`pip freeze`，您会注意到什么也没有出现。这是因为我们还没有在新环境中安装任何东西。

因此，让我们先将 Django 安装到我们的测试环境中(taskplanner_test):

```
pip install Django
```

为了创建我们的功能测试，我们需要一些东西。首先，我们需要在我们的机器上安装 Firefox web 浏览器。如果你没有火狐，现在就安装吧。

****旁注**** :可以使用 Chrome 进行集成测试，但是要在这里下载驱动[并跟随](https://sites.google.com/a/chromium.org/chromedriver/downloads)[这个栈溢出问题](http://stackoverflow.com/questions/13724778/how-to-run-selenium-webdriver-test-cases-in-chrome)。在运行集成测试时，Firefox 一直比 chrome 有更好的性能，这是一个非常重要的考虑因素，因为与单元测试相比，集成测试非常慢。

这是因为集成测试是测试 ****整个**** 系统，而不是‘单元’(小组件)。在现实世界中，有时最好避免集成测试，因为创建集成测试需要很长的开发时间，运行时间很慢，存在不明确的错误，以及其他您会及时发现的原因。

然而，在开发真实世界的应用程序时，它们仍然值得我们考虑，尽管性能下降，但在可靠性方面非常有用。

接下来，我们需要安装一个名为 [Selenium](http://selenium.googlecode.com/svn/trunk/docs/api/py/index.html) 的包。这个包将为我们提供一个 web 驱动程序，这样我们就可以用我们的测试来控制浏览器。Selenium 通常用于自动化您的浏览器。

```
pip install selenium
```

现在我们已经安装好了，我们需要一个目录来创建我们的测试:

```
mkdir functional_tests
```

在`taskplanner`目录中，您现在应该看到以下内容:

```
taskplanner
|-- functional_tests
|--- manage.py
|--- taskplanner
    ...
```

我们现在需要在我们的`functional_tests`文件夹中创建一些文件。我们将创建一个`__init__.py`文件(这将告诉 python 把`functional_tests`当作一个包)，和一个`test_all_users.py`文件来包含我们的测试。

让我们现在就开始吧:

```
touch functional_tests/__init__.py
touch functional_tests/test_all_users.py
```

****旁注**** : `__init__.py`几乎都是空文件。有关它的用途的更多信息，请参见[这个堆栈溢出答案。](http://stackoverflow.com/questions/448271/what-is-init-py-for)

我们终于可以开始编写我们的第一个功能测试了！功能测试是为了测试我们的 web 应用程序中的功能块。使用 Python 的 TDD 将功能测试描述为“从用户的角度来看，应用程序如何运行”。

因此，让我们在文本编辑器中打开`test_all_users.py`文件。首先，我们想导入 selenium 的 webdriver，为了使这变得更容易，Django 提供了一个叫做 StaticLiveServerTestCase 的东西来进行实时测试。现在让我们将这两者都导入:

```
from selenium import webdriver
from django.contrib.staticfiles.testing import StaticLiveServerTestCase
```

因为我们是从用户的角度进行测试，所以让我们将这些测试命名为 NewVisitorTest。添加以下内容:

```
class NewVisitorTest(StaticLiveServerTestCase):
    def setUp(self):
        self.browser = webdriver.Firefox()
        self.browser.implicitly_wait(2)

    def tearDown(self):
        self.browser.quit()
```

首先，我们创建一个名为 NewVisitorTest 的 StaticLiveServerTestCase 类，它将包含我们希望为新访问者运行的测试。然后，我们有两个方法名为安装和拆卸。当我们运行测试时，setUp 方法被初始化。因此，对于我们运行的每个测试，我们打开 Firefox 并等待 2 秒钟来加载页面。tearDown 在每次测试完成后运行，这个方法在每次测试后为我们关闭浏览器。

现在我们可以编写我们的第一个测试，让 Firefox 自动打开和关闭。现在让我们在 tearDown 方法下面编写我们的测试。

```
 def test_home_title(self):
        self.browser.get('http://localhost:8000')
        self.assertIn('Welcome to Django', self.browser.title)
```

我们的第一次测试，多么令人兴奋！让我们走一遍。我们要创建的每个测试都必须以“test”开头。例如，如果我想为我的 css 创建一个测试，我会调用方法`test_h2_css`。所以在这里，我们将测试命名为`test_home_title`。这是不言自明的，这正是我们在测试中想要的。该方法首先将 Firefox 带到 url `http://localhost:8000`，然后检查“欢迎来到 Django”是否在 html head 标签标题中。

让我们现在运行这个测试，看看会发生什么:

```
python manage.py test functional_tests
```

首先，我们到底在这里输入什么？manage.py 脚本为我们提供了名为“测试”的东西，我们将使用它来运行所有的测试。这里我们在用`__init__.py`文件创建的`functional_tests`包中运行它。

运行此命令后，您应该会在终端中看到如下内容:

```
F
======================================================================
FAIL: test_home_title (functional_tests.test_all_users.NewVisitorTest)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/Users/username/url/to/project/taskplanner/functional_tests/test_all_users.py", line 15, in test_home_title
    self.assertIn('Welcome to Django', self.browser.title)
AssertionError: 'Welcome to Django' not found in 'Problem loading page'

----------------------------------------------------------------------
Ran 1 test in 4.524s

FAILED (failures=1)
```

所以它失败了，但是它给了我们一些方便的建议。首先，AssertionError。“问题加载页面”中找不到“欢迎使用 Django”。这意味着`http://localhost:8000`的标题是“加载页面时出现问题”。如果您导航到该 url，您将看到该网页不可用。

让我们试着运行 Django 服务器来通过测试。切换回虚拟环境中的终端选项卡，运行我们的服务器。

```
python manage.py runserver
```

您应该会看到如下所示的内容:

```
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

March 06, 2016 - 20:53:38
Django version 1.9.4, using settings 'taskplanner.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

不要担心未应用的迁移消息。

现在我们有了一个运行在`http://localhost:8000`上的服务器，让我们再次运行我们的测试。

返回到`taskplanner_test`虚拟环境中的另一个终端选项卡，再次运行以下命令:

```
python manage.py test functional_tests
```

您应该看到以下内容。

```
Creating test database for alias 'default'...
.
----------------------------------------------------------------------
Ran 1 test in 4.033s

OK
Destroying test database for alias 'default'...
```

## **到目前为止我们做了什么**

我们第一次通过测试！

我们在这篇文章中已经讨论了很多。我们创建了我们的第一个项目，为开发和测试目的设置了虚拟环境，编写了我们的第一个功能测试，并遵循测试驱动的开发过程，编写了一个失败的测试，然后让它通过。

## **使用初学者模板**

通过使用 django starter 模板启动项目，您可以节省大量时间。这些项目使用了最佳实践，当您的项目增长时，这些实践将为您省去麻烦。一些比较受欢迎的项目有

*   烹饪刀
*   [黑客马拉松启动者](https://github.com/DrkSephy/django-hackathon-starter)
*   [边缘](https://github.com/arocks/edge)