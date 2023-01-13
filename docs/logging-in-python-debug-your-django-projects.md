# Python 中的日志——如何使用日志调试 Django 项目

> 原文：<https://www.freecodecamp.org/news/logging-in-python-debug-your-django-projects/>

唯一完美的代码是从未被写过的代码。作为一名开发人员，你肯定会遇到错误，并负责调试它们。

如果你正在用 Python 编码，你可以随时查看它的错误信息来弄清楚发生了什么。但是，如果发生了您不知道是什么破坏了您的代码的错误，该怎么办呢？

背景中可能有奇怪的错误，但你无法识别它。您可以随时关闭并再次打开它——或者更好的是，您可以检查日志。

## 什么是日志记录？

如果出现错误或者你的应用程序工作异常，你的日志文件将会派上用场。您可以遍历它们，找出应用程序到底哪里有问题，以及如何复制这些问题。

通过重现问题，您可以更深入地挖掘并找到错误的合理解决方案。如果没有日志文件，可能需要几个小时才能检测到的东西可能只需要几分钟就能诊断出来。

## Django 的伐木工作方式

幸运的是，Django 支持日志记录，并且大部分艰苦的工作已经由它的开发者完成了。Django 附带了 Python 的内置日志模块来利用系统日志。

Python 日志模块有四个主要部分:

1.  [记录器](#loggers)
2.  [处理程序](#handlers)
3.  [过滤器](#filters)
4.  [格式化程序](#formatters)

在 Django 官方文档中，每个组件都有详细的解释。我不想让你被它的复杂性淹没，所以我会简单解释每一个部分:

### 1.记录器

记录器基本上是日志系统的入口点。这是你作为一名开发人员实际要做的事情。

当记录器收到消息时，会将日志级别与记录器的日志级别进行比较。如果与日志记录器的日志级别相同或超过日志级别，**消息将被发送到处理器进行进一步处理。**日志级别包括:

*   **调试:**低级系统信息
*   **信息:**一般系统信息
*   **警告:**小问题相关信息
*   **错误:**重大问题相关信息
*   **关键:**关键问题相关信息

### 2.经理人

处理程序基本上决定了记录器中的每条消息会发生什么。它的日志级别与记录器相同。但是，我们可以从本质上定义我们想要处理每个日志级别的方式。

例如:**错误**日志级别消息可以实时发送给开发者，而**信息**日志级别可以只存储在系统文件中。

它本质上告诉系统如何处理消息，比如将消息写在屏幕上、文件中或网络套接字中

### 3.过滤

过滤器可以位于**记录器**和**处理器**之间。它可以用来过滤日志记录。

例如:在**关键**消息中，您可以设置一个过滤器，只允许处理特定的源。

### 4.格式化程序

顾名思义，格式化程序描述将要呈现的文本的格式。

现在我们已经介绍了基础知识，让我们用一个实际的例子来更深入地挖掘。[点击此处获取源代码](https://github.com/sa1if3/django-logging-tutorial)。

请注意，本教程假设您已经熟悉 Django 的基础知识。

## 项目设置

首先，用下面的命令在你的项目文件夹`django-logging-tutorial`中创建一个名为 **`venv`** 的虚拟环境，并激活它。

```
mkdir django-logging-tutorial
virtualenv venv
source venv/bin/activate
```

创建一个名为`django_logging_tutorial`的新 Django 项目。请注意，项目文件夹名称带有破折号，而项目名称带有下划线(- vs _)。我们还将快速运行一系列命令来设置我们的项目。

## 如何配置日志文件

让我们首先建立我们项目的`settings.py`文件。注意——注意我在代码中的注释，这会帮助你更好地理解这个过程。

在官方文档的第三个例子[中也提到了这个代码，在我们的大多数项目中，它会很好地发挥作用。我稍微修改了一下，使它更加健壮。](https://docs.djangoproject.com/en/3.2/topics/logging/#examples)

```
 LOGGING = {
    'version': 1,
    # The version number of our log
    'disable_existing_loggers': False,
    # django uses some of its own loggers for internal operations. In case you want to disable them just replace the False above with true.
    # A handler for WARNING. It is basically writing the WARNING messages into a file called WARNING.log
    'handlers': {
        'file': {
            'level': 'WARNING',
            'class': 'logging.FileHandler',
            'filename': BASE_DIR / 'warning.log',
        },
    },
    # A logger for WARNING which has a handler called 'file'. A logger can have multiple handler
    'loggers': {
       # notice the blank '', Usually you would put built in loggers like django or root here based on your needs
        '': {
            'handlers': ['file'], #notice how file variable is called in handler which has been defined above
            'level': 'WARNING',
            'propagate': True,
        },
    },
}
```

如果你读了我上面的评论，你可能已经注意到日志部分是空白的。也就是说任何一个伐木工。

小心使用这种方法，因为我们的大部分工作可以通过内置的 [Django 记录器](https://docs.djangoproject.com/en/3.2/topics/logging/#django-s-logging-extensions)如`django.request`或`django.db.backends`来满足。

此外，为了简单起见，我只使用了一个文件来存储日志。根据您的使用情况，当遇到**关键**或**错误**消息时，您也可以选择丢弃电子邮件。

要了解这方面的更多信息，我鼓励您阅读文档中的[处理程序](https://docs.djangoproject.com/en/3.2/topics/logging/#id4)部分。开始时，这些文档可能会让人感到不知所措，但是你越习惯阅读它们，你就越可能发现其他有趣或更好的方法。

如果您是第一次使用文档，请不要担心。凡事总有第一次。

我已经在评论中解释了大部分代码，但是我们仍然没有触及`propagate`。这是什么？

当`propagate`被设置为**真**时，子进程将把它们所有的日志调用传播给父进程。这意味着我们可以在 logger 树的根(父)处定义一个处理程序，子树(子)中的所有日志调用都指向父树中定义的处理程序。

同样重要的是要注意这里的等级制度是很重要的。我们也可以在我们的项目中将它设置为 **True** ,因为在我们的例子中没有子树，所以这无关紧要。

## 如何在 Python 中触发日志

现在，我们需要创建一些日志消息，以便在 **`settings.py`** 中测试我们的配置。

让我们有一个默认的主页，只显示'**你好 FreeCodeCamp.org 读者:)'【T2]，每次有人访问该页面时，我们在我们的`warning.log`文件中记下一条**警告**消息，作为“主页在 2021-08-29 22:23:33.551543 小时被访问！”**

转到您的应用程序`logging_example`，并在`views.py`中包含以下代码。确保您已经在`setting.py`的`INSTALLED_APPS`中添加了`logging_example`。

```
 from django.http import HttpResponse
import datetime
# import the logging library
import logging
# Get an instance of a logger
logger = logging.getLogger(__name__)

def hello_reader(request):
    logger.warning('Homepage was accessed at '+str(datetime.datetime.now())+' hours!')
    return HttpResponse("<h1>Hello FreeCodeCamp.org Reader :)</h1>") 
```

在项目的`urls.py`中，添加以下代码，以便当我们访问主页时调用正确的函数。

```
from django.contrib import admin
from django.urls import path
from logging_example import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('',views.hello_reader, name="hello_reader")
]
```

## 是时候做些测试了

最后，我们的简单设置完成了。我们现在需要做的就是启动我们的服务器并测试我们的日志。

使用以下命令运行开发服务器:

```
python manage.py runserver
```

现在，进入你的主页 **127.0.0.1:8000** ，在那里你会看到我们编码的信息。现在在创建的路径中检查您的`warning.log`文件。示例输出如下所示:

```
Homepage was accessed at 2021-08-29 22:38:29.922510 hours!
Homepage was accessed at 2021-08-29 22:48:35.088296 hours! 
```

就是这样！现在您知道了如何在 Django 中执行日志记录。如果你有任何问题，就给我留言。我答应帮忙:)

如果你觉得我的文章很有帮助，并且想阅读更多，请到我的博客[Techflow360.com](https://techflow360.com/category/web-development/django/)查看一些 Django 教程。