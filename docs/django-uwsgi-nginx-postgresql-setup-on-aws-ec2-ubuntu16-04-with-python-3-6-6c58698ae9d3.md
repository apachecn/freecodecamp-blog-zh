# 如何用 Python 3.6 在 AWS EC2 上创建运行 uWSGI、NGINX 和 PostgreSQL 的 Django 服务器

> 原文：<https://www.freecodecamp.org/news/django-uwsgi-nginx-postgresql-setup-on-aws-ec2-ubuntu16-04-with-python-3-6-6c58698ae9d3/>

作者:苏米特·库马尔

# 如何用 Python 3.6 在 AWS EC2 上创建运行 uWSGI、NGINX 和 PostgreSQL 的 Django 服务器

![1*RoxYjB7zefsqzjUMLLaprQ](img/ad83840e324fd9821d18532f003657a1.png)

对于新开发人员来说，每次为一个新项目启动并运行一台服务器可能非常耗时或困难。所以我想我应该写一个分步指南来简化部署过程。

如果你没有心情阅读，你可以按照描述复制粘贴每个步骤(替换值)，让你的服务器启动并运行？

#### 先决条件:

1.  Amazon Linux EC2 实例使用相关的密钥对启动并运行( *ssh 访问它*)。
2.  对于此实例，端口 22，80 必须打开。
3.  您想要部署的 Django 应用程序。
4.  数据库设置被配置为使用 PostgreSQL。
5.  *requirements.txt* 出现在您的应用程序中，有要安装的依赖项列表。
6.  Django 应用程序的 Git 存储库。

### SSH &更新 ubuntu 实例

您需要 ssh 到您的 EC2 实例，所以确保您为您的实例打开了**端口 22** ，然后进行更新/升级。

```
ssh -i path-to-your-key.pem ubuntu@your-aws-instance-public-ip

sudo apt-get update && sudo apt-get upgrade -y
```

### 在 AWS EC2 (ubuntu 16.04)上安装 Python3.6.x

我们将从官方网站下载 **tar.xz** 文件，然后手动安装。在此之前，我们需要安装一些必需的依赖项。

#### 构建和安装依赖项

```
sudo apt install build-essential checkinstall

sudo apt install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev
```

#### 下载并手动安装所需的 Python 版本

```
cd /opt && sudo wget https://www.python.org/ftp/python/3.6.6/Python-3.6.6.tar.xz

sudo tar -xvf Python-3.6.6.tar.xz

cd Python-3.6.6/

sudo ./configure

sudo make && sudo make install
```

#### 正在删除下载的文件

```
sudo rm -rf Python-3.6.6.tar.xz
```

#### 检查 Python 版本

```
python3 -V
> Python 3.6.6
```

### 为我们的应用程序设置 Ubuntu 用户

我同意 Django 本身是非常安全的框架。但是 web 应用程序仍然容易受到攻击。作为具有有限权限的系统用户运行您的应用程序是一个很好的实践，它对服务器上的资源具有有限的访问权限。因此，在本节中，我们将向 EC2 实例添加一个新的用户和权限组。

#### 添加 ubuntu 系统组“group name”[在我的情况下为 webapps，并为该组分配一个用户“username”[在我的情况下为 bunny

```
sudo groupadd --system webapps
sudo useradd --system --gid webapps --shell /bin/bash --home /webapps/project_name bunny
```

注意:我假设“**项目名称**”是您可能在“**姜戈-管理开始项目<纳**我>”期间使用的名称

#### 创建一个目录来存储应用程序

在/webapps/project_name/中创建一个目录来存储应用程序。将该目录的所有者更改为应用程序用户 bunny:

```
sudo mkdir -p /webapps/project_name/

sudo chown bunny /webapps/project_name/
```

#### 允许其他组用户有限地访问应用程序目录

```
sudo chown -R bunny:users /webapps/project_name

sudo chmod -R g+w /webapps/project_name
```

#### 现在您可以切换到您的用户

```
sudo su - bunny

// your console will switch to something like this
bunny@ip-172-31-5-231:~$
```

要切换回 **sudo** 用户，只需做`**ctrl+d**`就会杀死用户终端。

### 安装和配置 postgresql

#### 安装 PostgreSQL 并创建数据库

```
sudo apt install postgresql postgresql-contrib

sudo su - postgres

postgres@ip-172-31-5-231:~$ psql

postgres=# CREATE DATABASE database_name;
```

#### 在 **psql** 终端中更改 postgres 的默认密码

```
postgres=# \password
```

### 通过虚拟环境中的 Git 在 EC2 实例上部署 Django 应用程序

使用虚拟环境部署您的应用程序允许您的应用程序及其需求分开处理。保持你的应用程序独立是一个好习惯。

当您在一个实例上部署多个 Django 应用程序以保持它们和它们的依赖项相互隔离时，使用环境概念是很方便的。

我们将在我们的系统用户(*兔子*)目录中创建一个[虚拟环境](https://docs.python.org/3.6/library/venv.html)。在此之前，我们将安装 git 作为一个 *sudo* 用户。

#### 安装 Git 并从 git repo 中提取代码

```
sudo apt-get install git

sudo su - bunny

// change to your repo https or ssh link
bunny@ip-172-31-5-231:~$ git remote add origin 

git@github.com:<user>/<user-repo>.git

bunny@ip-172-31-5-231:~$ git pull origin <branch_name>
```

请注意，我们在这里没有克隆完整的回购协议。相反，我们手动设置我们的 git 链接，并且只提取我们想要部署到这个实例的分支。对于您的开发、测试或生产就绪的 web 应用程序，您可能有不同的实例，对应于 git 上的每个分支。

#### 在当前目录下使用 Python3.6 创建虚拟环境

```
bunny@ip-172-31-5-231:~$ python3.6 -m venv .
bunny@ip-172-31-5-231:~$ source bin/activate
(project_name)bunny@ip-172-31-5-231:~$ pip install -r requirements.txt
```

至此，我们已经成功地建立了我们的项目。现在我们需要运行一些**manage . p*y*命令。这将要求我们位于 manage.py 所在的目录中，或者每次我们需要给它一个路径时:**

```
(project_name)bunny@ip-172-31-5-231:~$ python <path-to->manage.py migrate

(project_name)bunny@ip-172-31-5-231:~$ python <path-to->manage.py createsuperuser

(project_name)bunny@ip-172-31-5-231:~$ python <path-to->manage.py collectstatic
```

注意:`collectstatic`命令要求正确设置静态配置。不过，我们在这里不讨论这个问题，因为它不在本教程的范围之内。

```
(project_name)bunny@ip-172-31-5-231:~$ python <path-to->manage.py runserver 0.0.0.0:8000
```

这将在端口`8000`上启动开发服务器。**假设端口 8000 也为您的实例打开，您可以在浏览器中访问服务器的域名或 IP 地址，后跟`8000`。**

```
http://your_server_domain_or_public_IP:8000
```

```
http://your_server_domain_or_public_IP:8000/admin
```

**注意:不要忘记在你的设置中添加你的域名或 IP 到 ALLOWED _ HOST . py**

### 设置 uWSGI 应用程序服务器

既然我们已经设置好项目并准备好了，我们可以配置 uWSGI 来将我们的应用程序提供给 web，而不是 Django 提供的轻量级开发服务器。

如果你想在屏幕上运行 runserver 命令，那就放弃吧。Django 的开发服务器非常轻量级，非常不安全，并且不可伸缩。

您可以在 virtualenv 中或全局安装 uWSGI，并对其进行相应的配置。

在本教程中，我们将在 *virtualenv* 中安装 uWSGI。在安装 uWSGI 之前，我们需要软件所依赖的 Python 开发文件。

#### 安装 uWSGI 及其依赖项

```
sudo apt-get install python3-dev
```

```
sudo su - bunny
```

```
bunny@ip-172-31-5-231:~$ source bin/activate
```

```
(project_name)bunny@ip-172-31-5-231:~$ pip install uwsgi
```

让我们使用 uWSGI 运行服务器。这个命令做的事情和 *manage.py runserver* 做的事情一样。您需要相应地替换值，以便成功地使用该命令进行测试。

```
(project_name)bunny@ip-172-31-5-231:~$ uwsgi --http :8000 --home <path-to-virtualenv> --chdir <path-to-manage.py-dir> -w <project-name>.wsgi
```

#### 正在创建 uWSGI 配置文件

从命令行运行 uWSGI 只对测试有用。对于实际部署，我们将创建一个 ***。*ini*文件在我们系统用户目录的某个地方。这个文件将包含处理大量请求负载的所有配置，并可以进行相应的调整。***

*在本教程的后面，我们将在 NGINX 后面运行 uWSGI。NGINX 与 uWSGI 高度兼容，并内置了与 uWSGI 交互的支持。*

#### *在您的系统用户目录中创建一个目录 **conf** ，您将在其中存储 **uwsgi.ini***

```
*`(project_name)bunny@ip-172-31-5-231:~$ mkdir conf`*
```

```
*`(project_name)bunny@ip-172-31-5-231:~$ cd conf`*
```

```
*`(project_name)bunny@ip-172-31-5-231:~$ nano uwsgi.ini`*
```

*从要点中复制下面的代码并保存它，我认为注释对每个选项都足够说明问题了。*

***注意:`updateMe`应该是你的项目名称。它与您在上面创建系统用户目录时给出的名称相同，因此要相应地更新。***

```
*`[uwsgi]

# telling user to execute file
uid = bunny

# telling group to execute file
gid = webapps

# name of project you during "django-admin startproject <name>"
project_name = updateMe

# building base path to where project directory is present [In my case this dir is also where my virtual env is]
base_dir = /webapps/%(project_name)

# set PYTHONHOME/virtualenv or setting where my virtual enviroment is
virtualenv = %(base_dir)

# changig current directory to project directory where manage.py is present
chdir = %(base_dir)/src/

# loading wsgi module
module =  %(project_name).wsgi:application

# enabling master process with n numer of child process
master = true
processes = 4

# enabling multithreading and assigning threads per process
# enable-threads  = true
# threads = 2

# Enable post buffering past N bytes. save to disk all HTTP bodies larger than the limit $
post-buffering = 204800

# Serialize accept() usage (if possibie).
thunder-lock = True

# Bind to the specified socket using default uwsgi protocol.
uwsgi-socket = %(base_dir)/run/uwsgi.sock

# set the UNIX sockets’ permissions to access
chmod-socket = 666

# Set internal sockets timeout in seconds.
socket-timeout = 300

# Set the maximum time (in seconds) a worker can take to reload/shutdown.
reload-mercy = 8

# Reload a worker if its address space usage is higher than the specified value (in megabytes).
reload-on-as = 512

# respawn processes taking more than 50 seconds
harakiri = 50

# respawn processes after serving 5000 requests
max-requests = 5000

# clear environment on exit
vacuum = true

# When enabled (set to True), only uWSGI internal messages and errors are logged.
disable-logging = True

# path to where uwsgi logs will be saved
logto = %(base_dir)/log/uwsgi.log

# maximum size of log file 20MB
log-maxsize = 20971520

# Set logfile name after rotation.
log-backupname = %(base_dir)/log/old-uwsgi.log

# Reload uWSGI if the specified file or directory is modified/touched.
touch-reload = %(base_dir)/src/

# Set the number of cores (CPUs) to allocate to each worker process.
# cpu-affinity = 1

# Reload workers after this many seconds. Disabled by default.
max-worker-lifetime = 300`*
```

*我试图用清晰的解释让一切变得简单。交叉检查路径、目录名和其他需要替换的输入。*

*我们需要创建日志文件，并运行将创建套接字文件的目录，我们刚刚在 uwsgi.ini 中提到过:*

```
*`(project_name)bunny@ip-172-31-5-231:~$ mkdir log`*
```

```
*`(project_name)bunny@ip-172-31-5-231:~$ mkdir run`*
```

```
*`(project_name)bunny@ip-172-31-5-231:~$ touch log/uwsgi.log`*
```

*请确保更改这两个目录的权限，以便每个组或用户都可以在这些目录中写入或执行文件:*

```
*`$ sudo chmod 777 /webapps/updateMe/run`*
```

```
*`$ sudo chmod 777 /webapps/updateMe/log`*
```

*现在让我们尝试使用我们刚刚创建的 **uwsgi.ini** 来运行服务器。*

```
*`(project_name)bunny@ip-172-31-5-231:~$ uwsgi --ini /webapps/updateMe/conf/uwsgi.ini`*
```

*如果到目前为止一切都设置正确，那么它应该正在运行。如果没有，那么您需要返回去检查您遗漏了什么(比如路径/项目名称等)。*

*要检查任何 uswgi 日志，您可以 **cat** 或 **tail** uwsgi.log:*

```
*`(project_name)bunny@ip-172-31-5-231:~$ tail log/uwsgi.log`*
```

#### *为 uwsgi 创建系统单元文件*

*此时，如果一切正常，您甚至可以在[屏幕](http://manpages.ubuntu.com/manpages/bionic/en/man1/screen.1.html)中运行该命令并将其分离——但同样，这根本不是一个好的做法。相反，我们将创建一个系统服务，并让 **systemd** (Ubuntu 的服务管理器)来处理它。*

#### *切换回 sudo 用户*

```
*`$ sudo nano /etc/systemd/system/uwsgi.service`*
```

*并从下面的要点复制粘贴代码。不要忘记更新和交叉检查适合您的应用程序的名称/路径:*

```
*`[Unit]
Description=uWSGI instance to serve updateMe project
After=network.target

[Service]
User=bunny
Group=webapps
WorkingDirectory=/webapps/project_name/src
Environment="PATH=/webapps/project_name/bin"
ExecStart=/webapps/project_name/bin/uwsgi --ini /webapps/project_name/conf/uwsgi.ini
Restart=always
KillSignal=SIGQUIT
Type=notify
NotifyAccess=all

[Install]
WantedBy=multi-user.target`*
```

*保存并关闭上述文件后，您可以运行以下命令:*

***重新加载 systemctl 守护进程以重新加载 systemd 管理器配置并重新创建整个依赖关系树***

```
*`$ sudo systemctl daemon-reload`*
```

***使 uwsgi 服务在系统重启时启动***

```
*`$ sudo systemctl enable uwsgi`*
```

***启动 uwsgi 服务***

```
*`$ sudo service uwsgi start`*
```

***重启 uwsgi 服务***

```
*`$ sudo service uwsgi restart`*
```

***检查 uwsgi 服务状态***

```
*`$ sudo service uwsgi status`*
```

*如果一切顺利，在这里深呼吸。我们刚刚完成了本教程最令人兴奋的部分，所以你应该感到自豪。*

*接下来我们将设置 NGINX，然后我们就完成了！我知道这需要一点时间，但是请相信我——一旦完成，你将会和我一样，在出版这篇教程后感到高兴。*

### *在 EC2 上为 uWSGI 设置 NGINX*

*NGINX 是一个轻量级服务器，我们将使用它作为反向代理。*

*我们可以让 uWSGI 直接在 80 端口上运行，但是 NGINX 有更多的[好处](https://serverfault.com/questions/590819/why-do-i-need-nginx-when-i-have-uwsgi/590833#590833)，这使得它更可取。【NGINX 本身也包含了对 uWSGI 的[支持](https://uwsgi-docs.readthedocs.io/en/latest/Nginx.html)。*

#### *说够了，让我们在我们的实例上安装 NGINX*

```
*`$ sudo apt-get install nginx`*
```

*现在当你进入**http://your-public-IP-or-address**时，你会看到一个 Nginx 欢迎页面。这是因为 NGINX 根据其默认配置监听端口 80。*

*NGINX 有两个目录，**站点可用**和**站点启用，**需要我们注意。 **sites-available** 存储该特定实例上所有可用站点的所有配置文件。**启用站点的**存储每个启用站点到可用站点目录的符号链接。*

*默认情况下，只有一个名为 default 的配置文件具有 NGINX 的基本设置。您可以修改它或创建一个新的。在我们的例子中，我将删除它:*

```
*`$ sudo rm -rf /etc/nginx/sites-available/default`*
```

```
*`$ sudo rm -rf /etc/nginx/sites-enabled/default`*
```

*让我们创建我们的 **nginx-uwsgi.conf** 文件，将浏览器请求连接到我们在站点上运行的 uwsgi 服务器:*

```
*`$ sudo nano /etc/nginx/sites-available/nginx-uwsgi.conf`*
```

*并从下面的要点中复制以下代码:*

```
*`upstream updateMe_dev {
    server unix:/webapps/updateMe/run/uwsgi.sock;
}

server {
    listen 80;
    server_name your-IP-or-address-here;
    charset utf-8;

    client_max_body_size 128M;

    location /static {
    # exact path to where your static files are located on server 
    # [mostly you won't need this, as you will be using some storage service for same]
        alias /webapps/updateMe/static_local;
    }

    location /media {
    # exact path to where your media files are located on server 
    # [mostly you won't need this, as you will be using some storage service for same]
        alias /webapps/updateMe/media_local;
    }

    location / {
        include uwsgi_params;
        uwsgi_pass updateMe_dev;
        uwsgi_read_timeout 300s;
        uwsgi_send_timeout 300s;
    }

    access_log /webapps/updateMe/log/dev-nginx-access.log;
    error_log /webapps/updateMe/log/dev-nginx-error.log;
}`*
```

#### *为其创建到启用站点的目录的符号链接*

```
*`$ sudo ln -s /etc/nginx/sites-available/nginx-uwsgi.conf /etc/nginx/sites-enabled/nginx-uwsgi.conf`*
```

*就这样，我们就快到了，就要结束了…*

#### *Reload systemctl daemon*

```
*`$ sudo systemctl daemon-reload`*
```

#### *系统重启时启用 nginx 服务*

```
*`$ sudo systemctl enable nginx`*
```

#### *启动 Nginx 服务*

```
*`$ sudo service nginx start`*
```

*测试 Nginx。作为结果的一部分，它应该返回 OK，Successful。*

```
*`$ sudo nginx -t`*
```

*如果 NGINX 失败，您可以在我们在它的配置文件中指定的路径上检查它最后的错误日志或访问日志。*

```
*`$ tail -f /webapps/updateMe/log/nginx-error.log`*
```

```
*`$ tail -f /webapps/updateMe/log/nginx-access.log`*
```

#### *重新启动 Nginx 服务*

```
*`$ sudo service nginx restart`*
```

#### *检查 Nginx 服务状态*

```
*`$ sudo service nginx status`*
```

*您现在应该可以在[**http://your-public-IP-or-address**](http://your-public-ip-or-address)访问您的应用程序*

*这个冗长的教程到此结束。我希望你从中得到了你所期望的。谢谢你对我的容忍。*

*PS: uWSGI + NGINX + Django 高度可定制，可满足任何大规模需求。也就是说，核心优化仍然在应用程序级别。如何编码和使用 Django ORM 或原始 SQL 查询等。会进一步帮助你。*