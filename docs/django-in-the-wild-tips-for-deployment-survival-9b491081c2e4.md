# 野外的姜戈:生存部署技巧

> 原文：<https://www.freecodecamp.org/news/django-in-the-wild-tips-for-deployment-survival-9b491081c2e4/>

阿里·阿拉维

# 野外的姜戈:生存部署技巧

![D6-FKRsVGvi72K1n0wh6g2AgzxjIkQ0ypLL2](img/d4ce050e74cff96c86dfa316bf4a9ddd.png)

Photo by [Thanh Tran](https://unsplash.com/@coffee_wanderer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在现实世界中部署 Django web 应用程序之前，您需要确保您的项目已经准备好投入生产。最好早点开始实现它们。它为你和你的团队节省了大量的时间和头痛。以下是我一路走来学到的几点:

1.  使用 pipenv(或 requirements.txt + venv)。提交 Pipefile 和 Pipefile.lock(或 requirements.txt)。给你的 venv 命名。
2.  有一个快速入门脚本。
3.  编写测试。使用 [Django 测试框架](https://docs.djangoproject.com/en/2.0/topics/testing/overview/) 和[假设](https://github.com/HypothesisWorks/hypothesis/tree/master/hypothesis-python)。
4.  使用 [environ](https://github.com/joke2k/django-environ) 和 [direnv](https://direnv.net/) 来管理您的设置并自动加载您的环境变量。
5.  确保所有开发人员提交他们的迁移。不时的挤压迁徙。如有必要，重置它们。为更顺利的迁移设计您的项目。阅读关于迁移的信息。
6.  使用持续集成。保护你的主分支。
7.  查看 Django 的官方部署清单。
8.  不要管理自己的服务器，但如果必须，使用适当的目录结构，使用 Supervisord、Gunicorn 和 NGINX。

这个列表是在我完成我们的第一个 Django 应用程序时自然增长的，并不详尽。但是我认为这些是你需要知道的最重要的几点。

请阅读每一点的讨论。

## 以正确的方式管理您的依赖关系和虚拟环境

您和您的团队应该就管理依赖性和虚拟环境的方法达成一致。我建议要么使用 [pipenv](https://github.com/pypa/pipenv) ，这是管理虚拟环境和依赖关系的新方法，要么使用创建 venv 并用`requirements.txt`文件跟踪依赖关系的老方法。

使用`requirements.txt`方法容易出现人为错误，因为开发人员往往会忘记更新包列表。这不是 **pipenv，**的问题，因为它会自动更新 **Pipefile。**pipenv 的缺点是它存在的时间还不够长。尽管 Python 软件基金会正式推荐了它，但在某些机器上运行它可能会有问题。就我个人而言，我仍然使用老方法，但是我将在我的下一个项目中使用 **pipenv** 。

### 使用 venv 和 requirements.txt

如果你使用的是 Python ≥ 3.6(你应该是)，你可以简单的用`python -m venv ENV`创建一个。确保您命名了您的虚拟环境(而不是使用`.`)。有时，您需要删除您的虚拟环境并重新创建它。这样更容易。另外，你应该把`ENV`目录添加到你的。`gitignore`文件(我更喜欢 ****ENV**** 名称而不是 ****venv**** ， ****)。env**** ，…因为它突出了当我 ****ls**** 时的项目文件夹)。

为了管理依赖关系，每个开发人员在安装新的包时都会运行`pip freeze > requirements.txt` ,并将它添加并提交给 repo。每当他们从远程存储库中提取数据时，他们都会使用`pip install -r requirements.txt`。

### 使用 pipenv

如果使用 **pipenv** ，你只需要添加 [Pipfile 和 Pipfile.lock](https://github.com/pypa/pipenv/issues/598) 到你的 repo 中。

## 拥有快速入门脚本

这有助于确保您的开发人员尽可能少花时间在与他们的工作不直接相关的事情上。

这不仅节省了时间和金钱，还确保了它们都工作在相似的环境中(例如，相同版本的 Python 和 pip)。

因此，尽可能多地尝试自动化设置任务。

此外，拥有一个单步构建脚本正是 Joel Test 的第二步所要做的。

下面是我使用的一个小脚本，它为我的开发人员节省了相当多的击键次数:

```
#!/bin/bash
python3.6 -m venv ENV
source ENV/bin/activate
pip install --upgrade pip
pip install -r requirements.txt 
source .envrc
python ./manage.py migrate
python ./manage.py loaddata example-django/fixtures/quickstart.json
python ./manage.py runserver
```

## 编写测试

每个人都知道编写测试是一种很好的实践。但是许多人忽视了这一点，他们认为这是为了更快的发展。不要。在编写用于生产的软件时，测试是绝对必要的，尤其是当你在团队中工作时。您可以知道最新的更新没有破坏什么的唯一方法是编写良好的测试。此外，您绝对需要对产品的持续集成和交付进行测试。

Django 有一个不错的测试框架。你也可以使用基于属性的测试框架，比如[假设](https://github.com/HypothesisWorks/hypothesis/tree/master/hypothesis-python)，它可以帮助你为你的代码编写更短的、数学上更严格的测试。在许多情况下，编写基于属性的测试更快。在实践中，您可能最终会使用这两种框架来编写全面的、易于读写的测试。

## 使用环境变量进行设置

您的 **settings.py** 文件是您存储所有重要项目设置的地方:您的数据库 URL、媒体和静态文件夹的路径等等。这些在您的开发机器和生产服务器上有不同的值。解决这个问题的最好方法是使用环境变量。第一步是使用[环境](https://github.com/joke2k/django-environ) **:** 更新您的[设置. py](https://gist.github.com/alialavia/da1c82a9f5194257d1de868decec933c) 以读取环境变量

```
import environ
import os
root = environ.Path(__file__) - 2 # two folders back (/a/b/ - 2 = /)
env = environ.Env(DEBUG=(bool, False),) # set default values and casting
GOOGLE_ANALYTICS_ID=env('GOOGLE_ANALYTICS_ID')

SITE_DOMAIN = env('SITE_DOMAIN') 
SITE_ROOT = root()

DEBUG = env('DEBUG') # False if not in os.environ

DATABASES = {
    'default': env.db(), # Raises ImproperlyConfigured exception if DATABASE_URL not in os.environ
}

public_root = root.path('./public/')

MEDIA_ROOT = public_root('media')
MEDIA_URL = '/media/'

STATIC_ROOT = public_root('static')
STATIC_URL = '/static/'

AWS_ACCESS_KEY_ID = env('AWS_ACCESS_KEY_ID')
AWS_SECRET_ACCESS_KEY = env('AWS_SECRET_ACCESS_KEY')

..
```

为了避免手动加载 envvars，在开发机器上设置 [direnv](https://direnv.net/) ，并将 envvars 存储在项目目录中的`.envrc`文件中。现在，无论何时进入项目文件夹，环境变量都会自动加载。将`.envrc`添加到您的存储库中(如果所有开发人员使用相同的设置)，并确保每当`.envrc`文件发生变化时，您都运行`direnv allow`。

不要在你的生产服务器上使用`direnv`。而是创建一个名为 ****.server.envrc**** 的文件，将其添加到`.gitignore`中，并将生产设置放在那里。现在，创建一个脚本`runinenv.sh`，自动从`.server.envrc`获取环境变量，激活虚拟环境，并运行提供的命令。您将在下一节看到如何使用它。这里是`runinenv.sh`应该看起来的样子([链接到 GitHub](https://gist.github.com/alialavia/7a29ea149870e151b4a27d53993d5d17) )。

```
#!/bin/bash
WORKING_DIR=/home/myuser/example.com/example-django
cd ${WORKING_DIR}
source .server.envrc
source ENV/bin/activate
exec $@
```

## 正确处理您的迁移

Django 迁移很棒，但是使用它们，尤其是在团队中，远非一帆风顺。

首先，您应该确保所有开发人员都提交了迁移文件。是的，您可能(将)最终会有冲突，尤其是当您与一个大型团队一起工作时，但是这比有一个不一致的模式要好。

一般来说，处理迁移并不容易。你需要知道你在做什么，你需要遵循一些最佳实践来确保工作流程的顺畅。

我想到的一件事是，如果您不时地压缩迁移(例如，每周一次)，通常会有所帮助。这有助于减少文件的数量和依赖图的大小，从而导致更快的构建时间，并且通常更少(或更容易处理)冲突。

有时重置迁移并重新进行迁移更容易，有时您需要手动修复冲突的迁移或将它们合并在一起。一般来说，处理迁移是一个值得单独张贴的主题，并且有一些关于这个主题的好读物:

*   [Django 迁移和如何管理冲突](https://www.algotech.solutions/blog/python/django-migrations-and-how-to-manage-conflicts/)
*   [如何重置迁移](https://simpleisbetterthancomplex.com/tutorial/2016/07/26/how-to-reset-migrations.html)

此外，项目的迁移如何结束取决于项目架构、模型等等。当您的代码库增长时，当您有多个应用程序时，或者当您在模型之间有复杂的关系时，这尤其重要。

我强烈推荐你[阅读这篇关于扩展 Django 应用](https://blog.doordash.com/tips-for-building-high-quality-django-apps-at-scale-a5a25917b2b5)的文章，这篇文章很好地涵盖了这个主题。它还有一个小而有用的迁移脚本。

## 使用持续集成

CI 背后的思想很简单:新代码一推出就运行测试。

使用与您的版本控制平台集成良好的解决方案。我最后用了 [CircleCI](https://circleci.com/) 。当您与多个开发人员一起工作时，CI 尤其有用，因为您可以确保他们的代码在发送拉请求之前通过所有测试。同样，正如你所看到的，拥有覆盖良好的测试是非常重要的。此外，确保您的主分支受到保护。

## 部署清单

Django 的官方网站提供了一份[便捷的部署清单](https://docs.djangoproject.com/en/2.0/howto/deployment/checklist/)。它帮助您确保项目的安全性和性能。确保你遵循这些指导方针。

## 如果您必须管理自己的服务器…

有很多很好的理由不管理自己的服务器。Docker 为您提供了更多的可移植性和安全性，而 AWS Lambda 等无服务器架构可以用更少的钱为您提供更多的好处。

但是有些情况下，你需要对你的服务器进行更多的控制(如果你需要更多的灵活性，如果你的服务不能与容器化的应用程序一起工作——比如安全监控代理，等等)。

### 使用适当的目录结构

首先要做的是使用适当的文件夹结构。如果您想在服务器上提供的只是 Django 应用程序，那么您可以简单地克隆您的存储库，并将其用作您的主目录。但这种情况很少发生:通常你还需要一些静态页面(主页、联系人等)。它们应该与您的 Django 代码库分开。

一个好的方法是创建一个父存储库，它将项目的不同部分作为子模块。您的 django 开发人员在 Django 资源库上工作，您的设计人员在 homepage 资源库上工作，……您将他们全部集成到一个资源库中:

```
example.com/
   example-django/
   homepage/
```

### 使用 Supervisord、NGINX 和 Gunicorn

当然，`manage runserver`有效，但只是为了快速测试。对于任何严重的问题，您都需要使用合适的应用服务器。Gunicorn 就是要走的路。

请记住，任何应用服务器都是一个长期运行的过程。您需要确保它保持运行，在服务器出现故障后自动重启，正确记录错误，等等。为此，我们使用`[supervisord](http://supervisord.org/).`

Supervisord 需要一个配置文件，在这个文件中我们告诉我们希望我们的进程如何运行。它不仅限于我们的应用服务器。如果我们有其他长期运行的流程(例如芹菜)，我们应该在`/etc/supervisor/supervisord.conf`中定义它们。这里有一个例子([也在 GitHub](https://gist.github.com/alialavia/c3b5ec16823a1278833c66cfd3a638ca) 上):

```
[supervisord]
nodaemon=true
logfile=supervisord.log

[supervisorctl]

[inet_http_server]
port = 127.0.0.1:9001

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

[program:web-1]
command=/home/myuser/example.com/example-django/runinenv.sh gunicorn example.wsgi --workers 3 --reload --log-level debug --log-file gunicorn.log --bind=0.0.0.0:8000
autostart=true
autorestart=true
stopsignal=QUIT
stdout_logfile=/var/log/example-django/web-1.log
stderr_logfile=/var/log/example-django/web-1.error.log
user=myuser
directory=/home/myuser/example.com/example-django

[program:celery-1]
command=/home/myuser/example.com/example-django/runinenv.sh celery worker --app=example --loglevel=info
autostart=true
autorestart=true
stopsignal=QUIT
stdout_logfile=/var/log/example-django/celery-1.log
stderr_logfile=/var/log/example-django/celery-1.error.log
user=myuser
directory=/home/myuser/example.com/example-django

[program:beat-1]
command=/home/myuser/example.com/example-django/runinenv.sh celery beat --app=example --loglevel=info
autostart=true
autorestart=true
stopsignal=QUIT
stdout_logfile=/var/log/example-django/beat-1.log
stderr_logfile=/var/log/example-django/beat-1.error.log
user=myuser
directory=/home/myuser/example.com/example-django

[group:example-django]
programs=web-1,celery-1,beat-1
```

注意我们在这里如何使用`runinenv.sh`(第 14、24 和 34 行)。另外，请注意第 14 行，我们告诉 ****gunicorn**** 派遣 3 名工人。该数量取决于您的服务器拥有的核心数量。[工作线程的推荐数量为:2 *核心数量+ 1。](http://docs.gunicorn.org/en/stable/design.html#how-many-workers)

您还需要一个反向代理将您的应用服务器连接到外部世界。用 [NGINX](https://www.nginx.com/) 就行了，因为它有广泛的用户基础，而且非常容易配置([你也可以在 GitHub](https://gist.github.com/alialavia/63805a95cb821c19c553d0c21a8da938) 上找到这段代码):

```
server {
    server_name www.example.com;
    access_log  /var/log/nginx/example.com.log;
    error_log    /var/log/nginx/example.com.error.log debug;
    root  /home/myuser/example.com/homepage;
    sendfile on;

# if the uri is not found, look for index.html, else pass everthing to gunicorn
    location / {
 index index.html;
 try_files $uri $uri/
     @gunicorn;
    }

# Django media
    location /media  {
        alias /home/myuser/example.com/example-django/public/media;      # your Django project's media files
    }

# Django static files
    location /static {
        alias /home/myuser/example.com/example-django/public/static;   # your Django project's static files
    }

location @gunicorn {

proxy_set_header Host $host;
        proxy_set_header X-Forwarded-Proto $scheme;
 #proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
 proxy_redirect off;

proxy_pass http://0.0.0.0:8000;
    }

client_max_body_size 100M;

listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/www.example.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/www.example.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}

server {
    server_name example.com;
    listen 443 ssl;
    ssl_certificate /etc/letsencrypt/live/www.example.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/www.example.com/privkey.pem; # managed by Certbot
    return 301 https://www.example.com$request_uri;

}

server {
    if ($host = www.example.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

if ($host = example.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

listen 80 default_server;
    listen [::]:80 default_server;
    server_name example.com www.example.com;
    return 301 https://$server_name$request_uri;
}
```

将配置文件存储在`/etc/nginx/sites-available`中，并在`/etc/nginx/sites-enabled`中创建一个指向它的符号链接。

我希望这篇文章对你有所帮助。请让我知道你对它的看法，如果你愿意的话，给我看看❤的作品。