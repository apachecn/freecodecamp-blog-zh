# 如何用 Dokku 建立自己的 Heroku

> 原文：<https://www.freecodecamp.org/news/how-to-build-your-on-heroku-with-dokku/>

Heroku 是开发者广泛使用的知名 PaaS。作为一个有趣而有用的项目，你可以很容易地用 Dokku 制作你自己的 Heroku 风格的 PaaS。

![dokku-logo-with-name5-1](img/a0a8909476370666fc559c88e3ea24da.png)

## 什么是 Heroku？

Heroku 是一家成立于 2007 年的平台即服务(PaaS)公司。该平台运行在 AWS 上，其短暂的存储系统称为“Dyno”。

Heroku 是开发人员使用最多的 PaaS 之一，这是有原因的——它易于使用，有良好的文档记录，并且支持多种编程语言。

但是，如果您可以部署自己的类似 Heroku 的平台，包括 CI/CD 管道、数据库连接、HTTPS 连接，以及一个简单易用的应用程序的更多功能，会怎么样呢？

这就是多库所提供的，甚至更多。让我们来看看。

## 什么是 PaaS？

平台即服务(PaaS)是一种软件架构风格，它为部署和管理应用程序代码提供了一个易于使用的抽象层。

这使您可以专注于编写业务逻辑，而不是担心平台本身。

PaaS 提供商通常提供自己的数据库服务以及其他相关服务，这可以大大简化常见的开发任务。

PaaS 的巨大优势是应用程序开发人员不需要执行任何系统管理工作。相反，您可以将代码和配置设置上传到中央服务器平台。

然后，该服务负责部署代码、根据需要扩展代码、备份数据、处理托管和正常运行时间等问题。

## 什么是 Dokku？

Dokku 是一个托管平台即服务，使开发人员能够轻松部署他们的应用程序。

从他们的网站:

> 您见过的最小的 PaaS 实现

Dokku 基于 Docker，并使用 Heroku 的构建包来编译和打包您的应用程序。

Dokku 最好的一点是它非常轻量级，可以安装在单个服务器或虚拟机上。

它包括使用 Docker 容器的可伸缩托管、使用 Git 的持续部署以及其他流行的 DevOps 工具。

Dokku 还提供了多种功能，比如支持多种语言、自定义域、自动化部署等等。

您可以轻松地将 Postgres 数据库甚至文件存储连接到您的应用程序。

您可以在 https://dokku.com/查看更多信息，或者在 https://dokku.com/docs/getting-started/installation/的[查看文档。](https://dokku.com/docs/getting-started/installation/)

你也可以在这里对[开源 GitHub 项目表现出一些喜爱。](https://github.com/dokku/dokku)

## 如何安装 Dokku

为了安装 Dokku，你需要一个 Linux VPS 和一个域名。

没有域名你也可以安装和使用 Dokku，但是使用域名就简单多了。我推荐云 VPS，因为它更容易访问和配置。

连接域时，可以将单个域或通配符与服务器的 IP 相关联。

我将使用一个安装了 Ubuntu 20.04 的 VPS。

我们首先确保我们的系统使用这些命令是最新的:

```
# Update the linux installation
$ sudo apt update
$ sudo apt upgrade -y
```

然后我们可以下载并运行 Dokku 的安装脚本:

```
# Install Dokku with the install script
$ wget https://raw.githubusercontent.com/dokku/dokku/v0.26.8/bootstrap.sh;
$ sudo DOKKU_TAG=v0.26.8 bash bootstrap.sh

--> Ensuring we have the proper dependencies
--> Note: Installing dokku for the first time will result in removal of
    files in the nginx 'sites-enabled' directory. Please manually
    restore any files that may be removed after the installation and
    web setup is complete.

    Installation will continue in 10 seconds.

    [...........]

    --> Running post-install dependency installation

 ! Setup a user's ssh key for deployment by passing in the public ssh key as shown:

     echo 'CONTENTS_OF_ID_RSA_PUB_FILE' | dokku ssh-keys:add admin
```

安装脚本将安装 Docker 和所有必要的依赖项以及 Dokku 本身，如上面的代码所示。

安装完成后，我们需要分配 SSH 密钥来访问并配置我们的域名。

如果您已经使用 SSH 设置了对 VPS 的访问(您应该这样做),那么您已经有了必要的密钥——您只需要将它们添加到 Dokku:

```
# Assign SSH key to Dokku
$ cat ~/.ssh/authorized_keys | dokku ssh-keys:add admin

SHA256:6O1TLVOUkWV+zmTWXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

如果服务器中还没有 SSH 密钥，那么您需要生成一个密钥对:

```
# Generate SSH key
$ ssh-keygen

Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa
Your public key has been saved in /root/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:7T6BbRCVWjGtcSUXXXXXXXXXXXXXXXXXXXXXXXXXXXXX root@freeDokku
The key's randomart image is:
+---[RSA 3072]----+
[.................]
|     . oS*.o . . |
[.................]
+----[SHA256]-----+ 
```

然后可以添加到 Dokku:

```
# Add SSH key to Dokku
$ dokku ssh-keys:add admin /root/.ssh/id_rsa.pub

SHA256:7T6BbRCVWjGtcSUXXXXXXXXXXXXXXXXXXXXXXXX 
```

下一步，也是最后一步，是为您的 Dokku 安装分配域。我们使用命令来完成:

```
# Set installation global domain
$ dokku domains:set-global domain.com

-----> Set domain.com
```

请确保将“domain.com”替换为您自己的域，并且您的域名 DNS 指向服务器的 IP 地址。

这就是安装和设置 Dokku 所需要做的全部工作。真的就这么简单。

您现在可以开始添加应用程序了。

让我们在下一节中通过添加一个标准的 Django 应用程序来看一个例子。

## 如何在 Dokku 中创建您的应用程序

为了创建和部署我们的第一个应用程序，我们需要在 Dokku 上做一些准备工作。

要在 Dokku 上部署应用程序，请执行以下步骤:

*   在 Dokku 上创建应用程序，这意味着给它一个名称。
*   创建关联数据库(或其他插件，如果需要的话)。这将创建并提供一个数据库，使用添加到应用程序中的自动 DATABASE_URL，以便于部署。
*   将必要的代码推送到 Dokku 的应用内部 GitHub 端点。这也可以包括必要的发布步骤(比如运行 Django 迁移)。

在代码被推送后，Dokku 将生成任何必要的 Docker 容器，并将使用任何关联的数据库(或其他插件)运行我们的应用程序。

现在，我们已经介绍了必要的步骤，让我们在实践中进行一遍。

让我们从创建应用程序开始。对于本教程，我将创建一个非常简单的 Django 网站，其中包含所有测试 Dokku 所需的逻辑。

我们用这个命令在 Dokku 上创建一个应用程序(在我们安装 Dokku 的服务器上):

```
# Creating our application on Dokku
$ dokku apps:create djangotutorial

-----> Creating djangotutorial...
```

默认情况下，创建应用程序时不会创建数据存储(或数据库)。

数据存储由一系列插件处理。你可以在这里查看所有可用的插件。

对于我们的应用程序，我们将创建一个 Postgres 数据存储。由于默认情况下没有安装插件，我们首先需要安装 Postgres 插件:

```
# install the postgres plugin
# plugin installation requires root, hence the user change
sudo dokku plugin:install https://github.com/dokku/dokku-postgres.git
```

然后我们可以创建 Postgres 数据存储:

```
# Create a Postgres datastore
$ dokku postgres:create djangotutorial_datastore

       Waiting for container to be ready
       Creating container database
       Securing connection to database
=====> Postgres container created: djangotutorial_datastore
=====> djangotutorial_datastore postgres service information
       Config dir:          /var/lib/dokku/services/postgres/djangotutorial_datastore/data
       Config options:
       Data dir:            /var/lib/dokku/services/postgres/djangotutorial_datastore/data
       Dsn:                 postgres://postgres:ea706cc108c805d5124d134d934024c5@dokku-postgres-djangotutorial-datastore:5432/djangotutorial_datastore
       Exposed ports:       -
       Id:                  782a04fe6bbd25958752c17c304358fd5ec1f3c54d6d53175b6481b3b957d94b
       Internal ip:         172.17.0.5
       Links:               -
       Service root:        /var/lib/dokku/services/postgres/djangotutorial_datastore
       Status:              running
       Version:             postgres:14.1 
```

我们可以检查数据存储区的 Docker 容器是否已经启动并运行:

```
# Check running containers
$ docker ps

CONTAINER ID   IMAGE                      COMMAND                  CREATED              STATUS              PORTS      NAMES
782a04fe6bbd   postgres:14.1              "docker-entrypoint.s…"   About a minute ago   Up About a minute   5432/tcp   dokku.postgres.djangotutorial_datastore 
```

既然我们已经启动并运行了数据存储，我们需要将它与我们的应用程序相关联:

```
# Associate datastore with the application
$ dokku postgres:link djangotutorial_datastore djangotutorial

-----> Setting config vars
       DATABASE_URL:  postgres://postgres:ea706cc108c805d5124d134d934024c5@dokku-postgres-djangotutorial-datastore:5432/djangotutorial_datastore
-----> Restarting app djangotutorial
 !     App image (dokku/djangotutorial:latest) not found 
```

您可以看到一个 DATABASE_URL 被自动创建并与应用程序相关联。

上面的例子提到我们的应用程序映像没有找到，因为我们还没有向它推送任何代码。

我们可以检查应用程序的环境变量，以确认我们的 DATABASE_URL 是否存在:

```
# Checking an application environment variables
$ dokku config:show djangotutorial

=====> djangotutorial env vars
DATABASE_URL:  postgres://postgres:ea706cc108c805d5124d134d934024c5@dokku-postgres-djangotutorial-datastore:5432/djangotutorial_datastore 
```

现在，我们已经在 Dokku 端完成了所有必要的配置，以支持应用程序的部署。

接下来，我们将为我们的应用程序创建代码，并将其部署到 Dokku 以实现自动化的 CI/CD 管道。

## 如何在 PyCharm 上创建应用程序代码

在部署应用程序之前，我们需要将它的源代码推送到 Dokku。

在本教程中，我们将创建一个非常简单的 Django 应用程序，展示 Postgres 数据库的使用。

我们将使用 PyCharm 作为 IDE 来创建和管理我们的项目。

我们在 PyCharm 中创建了一个新项目——我们称之为“DjangoTutorial ”:

![PyCharm-NewProject](img/de86de08d98b3cf31a6adb5c2bfb033d.png)

Creating a new project on PyCharm - Screenshot by author

我个人更喜欢在已经存在虚拟环境的情况下创建新项目，这让生活变得容易得多。

如果您使用默认的 main.py 文件创建了项目(就像我一样，因为我总是忘记去掉复选标记)，现在您可以安全地删除它了。我们不会使用它。

当然，第一步是安装 Django，这样我们就可以构建我们的应用程序。我们使用 pip 进行安装:

```
$ pip install django

Collecting django
  Downloading Django-4.0.2-py3-none-any.whl (8.0 MB)
     |████████████████████████████████| 8.0 MB 6.4 MB/s
Collecting sqlparse>=0.2.2
  Using cached sqlparse-0.4.2-py3-none-any.whl (42 kB)
Collecting tzdata
  Using cached tzdata-2021.5-py2.py3-none-any.whl (339 kB)
Collecting asgiref<4,>=3.4.1
  Downloading asgiref-3.5.0-py3-none-any.whl (22 kB)
Installing collected packages: tzdata, sqlparse, asgiref, django
Successfully installed asgiref-3.5.0 django-4.0.2 sqlparse-0.4.2 tzdata-2021.5
```

然后，我们用以下内容创建 Django 项目:

```
$ django-admin startproject DjangoTutorial . 
```

请注意“.”在命令的最后。我喜欢用它在当前目录下创建项目，而不是创建一个额外的子目录。

在 PyCharm 中，您现在应该有这样的项目结构:

![PyCharm-Project](img/3efb2f76b18be731108ebd9ff1246e85.png)

PyCharm folder structure for our Django application - screenshot by the author

我们可以用标准的 Django 运行来运行我们的项目:

```
$ python manage.py runserver   

Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 18 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.
February 02, 2022 - 16:49:27
Django version 4.0.2, using settings 'DjangoTutorial.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CTRL-BREAK. 
```

我们还没有应用我们的迁移，所以我们将在讨论本地和 Dokku 访问的数据库配置之后再做。

导航到链接 [http://127.0.0.1:8000/](http://127.0.0.1:8000/) ，我们现在可以访问我们的标准 Django 欢迎页面:

![Django](img/53d292351f84fdfebd2c9c82d87e895e.png)

Django welcome page - screenshot by the author

我们已经安装并运行了 Django，所以现在我们可以开始构建项目的其余部分了。

像大多数项目一样，我们需要将数据存储在数据库中(或者使用 Dokku 命名的数据库)。

我们还希望能够在开发机器上本地调试和运行我们的应用程序(在这个 SQLite 中使用本地数据库),并使用 Postgres 数据库通过 Dokku 在云上运行它。

这意味着我们需要更改 settings.py 中的一些配置，以便能够支持这两种用例，而不需要每次都更改任何标志或配置。

我们从安装包 dj-database-url 开始，其中包含:

```
# Install packages for the database url
$ pip install dj-database-url
$ pip install psycopg2

# We also install this package to be able to use environment variables
$ pip install python-decouple
```

这个包使我们能够拥有一个 Django 数据库连接字典，通过简单地指定一个数据库 URL 就可以用所有数据填充它。

随着软件包的安装，让我们更新 settings.py 上的配置:

```
# We need to add this import at the beginning to use environment variables
import dj_database_url
from decouple import config
from django.conf.global_settings import DATABASES

.....

# Let's also updated the allowed host so we can use it later on
ALLOWED_HOSTS = config('ALLOWED_HOSTS').split(',')

.....

# We replace the default database configuration from Django with this one
# Database
# https://docs.djangoproject.com/en/4.0/ref/settings/#databases

# DATABASE URL
DATABASES['default'] = dj_database_url.parse(config('DATABASE_URL'),conn_max_age=600)
```

settings.py

我们还需要创造。项目根目录下的“env”文件:

```
#HOST SETTINGS
ALLOWED_HOSTS = 127.0.0.1

#DATABASE SETTINGS
DATABASE_URL='sqlite:///db.sqlite3'
```

.env

如您所见，通过这一更改，我们可以从本地使用数据库 URL。env '文件，然后在 Dokku 上，它将自动使用已经定义的 DATABASE_URL，这是在我们将数据存储链接到 Dokku 上的应用程序时创建的。

我们现在可以创建我们的第一个(也是本教程的唯一一个)web 页面，一个简单的计数器，用于存储和读取数据库中的值。

让我们创建一个单独的应用程序来包含我们的逻辑:

```
$ python manage.py startapp counter 
```

我们现在应该在项目中有一个名为“counter”的新文件夹。让我们通过打开“models.py”文件来添加一个新模型:

```
from django.db import models

class Counter(models.Model):
    count = models.IntegerField(default=0)

    def __str__(self):
        return self.count
```

counter/models.py

我们现在可以添加一个新的 URL 来加载我们的计数器页面。为此，我们将名为“urls.py”的新文件添加到我们的“counter”文件夹中:

```
from django.urls import path
from . import views

urlpatterns = [
    path('counter/', views.counter, name='counter'),
]
```

counter/urls.py

我们现在有了模型和 URL 来加载我们的测试页面。我们现在只需要视图和 HTML 模板来呈现页面。

让我们通过编辑“views.py”文件来创建视图:

```
from django.shortcuts import render
from .models import Counter

def counter(request):
    counter_value = Counter.objects.last()

    if counter_value is None:
        counter_value = Counter(count=0)
        counter_value.save()

    if request.method == 'POST':
        counter_value.count += 1
        counter_value.save()

    return render(request, 'counter.html', {'counter': counter_value.count}) 
```

counter/views.py

现在我们可以创建 HTML 模板来在页面上显示计数器值。我们在新的“templates”文件夹中创建一个名为“counter.html”的新文件:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Counter</title>
</head>
<body>
  <form method="post">
      {%csrf_token%}
    <h4>Counter value is: {{ counter }}</h4>
    <input type="submit" name="submit" value="Increase Counter">
  </form>
</body>
</html>
```

counter/templates/counter.html

最后一步是将我们新创建的应用程序添加到“settings.py”文件中，以便 Django 能够识别它:

```
.....

INSTALLED_APPS = [
    'counter',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

.....
```

settings.py

以及我们的主 URL 文件的 URL:

```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('', include('counter.urls')),
    path('admin/', admin.site.urls),
]
```

urls.py

有了所有必要的代码和 HTML，我们现在可以创建并运行我们的迁移，在数据库中创建我们的新模型。我们首先在本地服务器上运行:

```
# Create and run migrations
$ python manage.py makemigrations
$ python manage.py migrate

Operations to perform:
  Apply all migrations: admin, auth, contenttypes, counter, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying counter.0001_initial... OK
  Applying sessions.0001_initial... OK 
```

如您所见，我们不仅为我们的新应用程序应用了迁移，还为其他 Django 应用程序运行了初始迁移，因为这是我们第一次运行迁移。

我们可以再次在本地运行我们的服务器，我们应该能够访问 URL[http://127 . 0 . 0 . 1:8000/counter/](http://127.0.0.1:8000/counter/)并递增计数器:

![CounterPage_Local](img/72647a36fea5f0e8caa82de3c30e353b.png)

Running our counter application - GIF by the author

如您所见，重新加载页面保留了我们的计数器值，这意味着该值与我们的模型一起存储在数据库中。

## 如何将我们的应用程序部署到 Dokku

我们现在有了一个非常简单的应用程序，它运行在数据库集成中，用来存储我们的计数器值。

我们准备将它部署到云中，这样我们就可以在那里进行测试，并确保我们的数据库也可以在云中工作。

在我们使用 Git push 将代码部署到 Dokku 之前，我们需要做一些准备工作:

*   安装我们的网络服务器(gunicorn)
*   创建我们的需求文件(针对我们的包)
*   创建我们的 Procfile(用于我们的部署命令)

让我们从安装要在云中使用的 web 服务器开始:

```
# Install our web server
$ pip install gunicorn
```

有了我们的包，我们现在可以用以下内容创建我们的需求文件:

```
# Create requirements file
$ pip freeze > requirements.txt
```

现在我们需要创建“Procfile”。Dokku 使用该文件来确定在部署时和部署后运行哪些命令。

因此，让我们在根目录下创建一个名为“Procfile”的新文件，其内容如下:

```
web: gunicorn DjangoTutorial.wsgi
release: python manage.py migrate
```

Procfile

我们为 Dokku 创建了两个命令来运行:

*   release——该命令在 Dokku 中部署我们的应用程序时执行。我们用它来迁移我们的数据库。
*   web——这个命令允许 Dokku 知道运行哪个 web 服务器来访问应用程序。

最后，为了确保当我们的代码被部署到 Dokku 时，我们可以收集任何静态文件，我们需要在根目录上创建一个名为“static”的新目录。在里面，我们创建一个名为'的空文件。Git keep’(这将允许我们稍后将目录添加到 Git 存储库中)。

我们还需要将静态文件的路径添加到我们的“settings.py”文件中:

```
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/4.0/howto/static-files/

STATIC_URL = 'static/'
STATIC_ROOT = BASE_DIR / "static"
```

settings.py

现在所有的文件和逻辑都准备好了，我们可以用一个标准的 Git push 部署到 Dokku。让我们检查一下当前的文件结构:

![PyCharm-FolderStrcuture-1](img/d5d538b0448b3b5d12f592a2ef8bc274.png)

PyCharm folder structure - screenshot by the author

为了能够将我们的代码推送到 Dokku，我们需要将我们的项目添加到 Git 存储库中。

因为我们不想将文件夹结构中的所有文件都推到 Dokku git 存储库中，所以我们创建了一个。“gitignore”来排除某些文件和目录。我使用这个优秀要点的内容来填充文件:

[python pycharm gitignorepython pycharm gitignore. GitHub Gist: instantly share code, notes, and snippets.![favicon](img/0973ea8ce7121c320f68413e2a2f23ab.png)262588213843476Gist![gist-og-image](img/b5d0f1167a86fdbbb8945644fc4c54f6.png)](https://gist.github.com/MOOOWOOO/3cf91616c9f3bbc3d1339adfc707b08a)

我们现在可以在本地初始化并提交我们的代码到 Git 存储库:

```
# Initialize repository
$ git init -b main

# Add and commit our files
$ git add . && git commit -m "initial commit"

[main (root-commit) e77a16a] initial commit
 20 files changed, 438 insertions(+)       
 create mode 100644 .gitignore
 create mode 100644 DjangoTutorial/__init__.py
 create mode 100644 counter/tests.py
 create mode 100644 counter/urls.py
 create mode 100644 counter/views.py
 create mode 100644 db.sqlite3
 create mode 100644 manage.py
 create mode 100644 requirements.txt 
```

提交了我们的存储库之后，我们现在可以将它推送到一个远程存储库，也就是我们的应用程序的 Dokku Git 存储库:

```
# Adding our remote repository (replace domain.com with your domain name)
$ git remote add dokku dokku@domain.com:djangotutorial

# Time to push our code to the remote repository
$ git push dokku main

Enumerating objects: 34, done.
Counting objects: 100% (34/34), done.
Delta compression using up to 8 threads
Compressing objects: 100% (31/31), done.
Writing objects: 100% (34/34), 11.41 KiB | 402.00 KiB/s, done.
Total 34 (delta 7), reused 0 (delta 0)
-----> Set main to DOKKU_DEPLOY_BRANCH.
-----> Cleaning up...
-----> Building djangotutorial from herokuish
-----> Adding BUILD_ENV to build environment...
       BUILD_ENV added successfully
-----> Python app detected
-----> No Python version was specified. Using the buildpack default: python-3.9.9
       To use a different version, see: https://devcenter.heroku.com/articles/python-runtimes
-----> No change in requirements detected, installing from cache
-----> Installing python-3.9.9
-----> Installing pip 21.3.1, setuptools 57.5.0 and wheel 0.37.0
-----> Installing SQLite3
-----> Installing requirements with pip
       Collecting asgiref==3.5.0
       Downloading asgiref-3.5.0-py3-none-any.whl (22 kB)
       Collecting dj-database-url==0.5.0
       Downloading dj_database_url-0.5.0-py2.py3-none-any.whl (5.5 kB)
       Collecting Django==4.0.2
       Downloading Django-4.0.2-py3-none-any.whl (8.0 MB)
       Collecting gunicorn==20.1.0
       Downloading gunicorn-20.1.0-py3-none-any.whl (79 kB)
       Collecting psycopg2==2.9.3
       Downloading psycopg2-2.9.3.tar.gz (380 kB)
       Preparing metadata (setup.py): started
       Preparing metadata (setup.py): finished with status 'done'
       Collecting python-decouple==3.5
       Downloading python_decouple-3.5-py3-none-any.whl (9.6 kB)
       Collecting sqlparse==0.4.2
       Downloading sqlparse-0.4.2-py3-none-any.whl (42 kB)
       Collecting tzdata==2021.5
       Downloading tzdata-2021.5-py2.py3-none-any.whl (339 kB)
       Building wheels for collected packages: psycopg2
       Building wheel for psycopg2 (setup.py): started
       Building wheel for psycopg2 (setup.py): finished with status 'done'
       Created wheel for psycopg2: filename=psycopg2-2.9.3-cp39-cp39-linux_x86_64.whl size=579484 sha256=9d6a2810a5d766738526d6f411e5e9ce514cce882b6c80a47a13c02dc7529e02
       Stored in directory: /tmp/pip-ephem-wheel-cache-8k0chg5g/wheels/b3/a1/6e/5a0e26314b15eb96a36263b80529ce0d64382540ac7b9544a9
       Successfully built psycopg2
       Installing collected packages: sqlparse, asgiref, tzdata, python-decouple, psycopg2, gunicorn, Django, dj-database-url
       Successfully installed Django-4.0.2 asgiref-3.5.0 dj-database-url-0.5.0 gunicorn-20.1.0 psycopg2-2.9.3 python-decouple-3.5 sqlparse-0.4.2 tzdata-2021.5
-----> $ python manage.py collectstatic --noinput
       128 static files copied to '/tmp/build/static'.

-----> Discovering process types
       Procfile declares types -> release, web
-----> Releasing djangotutorial...
-----> Checking for predeploy task
       No predeploy task found, skipping
-----> Checking for release task
-----> Executing release task from Procfile: python manage.py migrate
=====> Start of djangotutorial release task (a602cab30) output
       Operations to perform:
         Apply all migrations: admin, auth, contenttypes, counter, sessions
       Running migrations:
         Applying contenttypes.0001_initial... OK
         Applying auth.0001_initial... OK
         Applying admin.0001_initial... OK
         Applying admin.0002_logentry_remove_auto_add... OK
         Applying admin.0003_logentry_add_action_flag_choices... OK
         Applying contenttypes.0002_remove_content_type_name... OK
         Applying auth.0002_alter_permission_name_max_length... OK
         Applying auth.0003_alter_user_email_max_length... OK
         Applying auth.0004_alter_user_username_opts... OK
         Applying auth.0005_alter_user_last_login_null... OK
         Applying auth.0006_require_contenttypes_0002... OK
         Applying auth.0007_alter_validators_add_error_messages... OK
         Applying auth.0008_alter_user_username_max_length... OK
         Applying auth.0009_alter_user_last_name_max_length... OK
         Applying auth.0010_alter_group_name_max_length... OK
         Applying auth.0011_update_proxy_permissions... OK
         Applying auth.0012_alter_user_first_name_max_length... OK
         Applying counter.0001_initial... OK
         Applying sessions.0001_initial... OK
=====> End of djangotutorial release task (a602cab30) output
-----> App Procfile file found
=====> Processing deployment checks
       No CHECKS file found. Simple container checks will be performed.
       For more efficient zero downtime deployments, create a CHECKS file. See https://dokku.com/docs/deployment/zero-downtime-deploys/ for examples
-----> Deploying djangotutorial via the docker-local scheduler...
-----> Deploying web (count=1)
       Attempting pre-flight checks (web.1)
       Waiting for 10 seconds (web.1)
       Default container check successful (web.1)
-----> Deploying release (count=0)
-----> Running post-deploy
-----> Creating new app virtual host file...
-----> Configuring djangotutorial.domain.com...(using built-in template)
-----> Creating http nginx.conf
       Reloading nginx
-----> Renaming containers
       Renaming container djangotutorial.web.1.upcoming-7101 (f8d229ebd8bc) to djangotutorial.web.1
-----> Checking for postdeploy task
       No postdeploy task found, skipping
-----> Updated schedule file
=====> Application deployed:
       http://djangotutorial.domain.com

To domain.com:djangotutorial
 * [new branch]      main -> main 
```

我们刚刚将我们的应用程序部署到 Dokku。

刚刚发生了什么？多库为我们做了很多工作:

*   安装的 Python
*   安装需求
*   收集静态文件
*   执行迁移
*   最后启动 gunicorn 服务器来部署我们的应用程序

如果您有权限错误，那么您的私钥应该在您的本地开发环境中注册。如果推送时出现`permission denied`错误，可以注册私钥如下:`ssh-add -k ~/<your private key>`。

访问应用程序时，您还可能会看到一个关于 ALLOWED_HOSTS 的错误。在这种情况下，您只需在 Dokku 服务器上运行以下命令，将环境变量设置为正确的值:

```
# Set ALLOWED_HOSTS environment variable (make sure to use your domain name)
$ dokku config:set djangotutorial ALLOWED_HOSTS=djangotutorial.domain.com
```

我们现在可以通过上面的 URL 访问和测试我们的应用程序:

![PageCounter_Server](img/81d1a1afcaee051b1ef6193f4dc9cfb5.png)

Running our counter application on Dokku - GIF by the author

恭喜，您刚刚在 Dokku 上部署了您的应用程序。

## 如何用“让我们加密”添加 SSL

我们可以做的最后一个配置是通过安装一个让我们加密 SSL 证书来为我们的应用程序添加 SSL 安全性。

我们可以在 Dokku 上用 Let's Encrypt 插件很容易地做到这一点:

```
# Install the Let's Encrypt plugin
sudo dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git

# Configure the plugin (make sure to replace to your email)
dokku config:set --global DOKKU_LETSENCRYPT_EMAIL=email@domain.com

# set a custom domain that you own for your application
dokku domains:set djangotutorial djangotutorial.your.domain.com

# Enable Let's Encrypt
dokku letsencrypt:enable djangotutorial

# Enable Let's Encrypt auto-renewal
dokku letsencrypt:cron-job --add
```

现在我们有了一个更安全的应用程序。毕竟我们的专柜很重要。

## 结论

使用 PaaS 使开发人员在构建 web 应用程序时更加轻松。

你可以像 Heroku 一样使用托管的 PaaS，还有很多其他的，所以选择是存在的。

但是有一些主要的缺点:

*   价格托管解决方案可能在数据库存储或文件存储等方面有限制
*   您无法控制部署 PaaS 的主机。AWS 最近的例子表明，即使是最大的主机也不是没有问题的。

您可以通过自托管 PaaS 来解决这些问题。

这允许在定价方面有更多的控制。你可以使用托管服务提供商，如[数字海洋](https://www.digitalocean.com/)、[海兹纳](https://hetzner.cloud/)，以及其他拥有非常便宜的 VPS 并能与 Dokku 完美合作的提供商。

没有数据库限制。你可能有的唯一限制是内存和磁盘空间，但是你可以用比在 Heroku 获得一个新数据库更低的价格升级你的 VPS。

正如我们所见，Dokku 易于安装。创建和部署应用程序分为三个步骤:

*   在 Dokku 上创建应用程序
*   在 Dokku 上创建一个数据存储(如果需要，像 Postgres 一样)并链接到应用程序
*   用 Git 将代码部署到 Dokku

此外，您可能需要配置一些环境变量和 SSL 证书，但仅此而已。

Dokku 确实是最小的 PaaS 实现。

Django 应用程序的完整源代码可从以下网址获得:

[GitHub - nunombispo/DjangoTutorial: Example application for freeCodeCamp article.Example application for freeCodeCamp article. Contribute to nunombispo/DjangoTutorial development by creating an account on GitHub.![favicon](img/0973ea8ce7121c320f68413e2a2f23ab.png)nunombispoGitHub![DjangoTutorial](img/89cda0a4e86f368250d4e06f39e63419.png)](https://github.com/nunombispo/DjangoTutorial)

在推特上关注我:[https://twitter.com/DevAsService](https://twitter.com/DevAsService)

查看我的网站:[https://developer-service.io/](https://developer-service.io/)

或者查看我的博客:[https://blog.developer-service.io/](https://blog.developer-service.io/)