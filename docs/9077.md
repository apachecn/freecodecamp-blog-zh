# 创建你自己的 GitHub(有点)

> 原文：<https://www.freecodecamp.org/news/create-your-own-github-kinda-9b4581db675c/>

肖申克的夏尔马

# 创建你自己的 GitHub(有点)

![jA95J8tg2JMxx-zLD2ORofiNNj7pMPdmiAB7](img/5eab3c3cf030398e3be4f80030e4a97e.png)

Git Logo by [Jason Long](http://twitter.com/jasonlong)

为了在 Git 中进行任何协作，您需要有一个远程 Git 存储库。让我们逐步介绍如何在 AWS EC2 实例上创建 Git 服务器。

首先，让我们了解一些基础知识。

#### 裸存储库与非裸存储库

没有工作目录的 git 存储库被称为“空”存储库。Git 中的一个“**bare**”**仓库**只包含版本控制信息，没有工作文件(没有树)。它不包含特殊的。git 子目录。相反，它包含。git 子目录直接在主目录本身。

```
Create: git init --bare
```

```
Clone: git clone --bare $URL
```

**非裸存储库**有一个已签出的工作树。git 子目录。

```
Create: git init
```

```
Clone: git clone $URL
```

您应该使用非空存储库在本地工作，使用空存储库作为中央服务器/中心，与其他人共享您的更改。例如，当您在 github.com 上创建一个存储库时，它会被创建为一个空存储库。

裸存储库比非裸存储库小。由于裸存储库没有工作副本，任何推送到它们的更改都不会导致冲突。按照惯例，裸存储库使用带有。git 后缀。

#### 协议

Git 可以使用四种主要协议来传输数据:Local、HTTP、Secure Shell (SSH)和 Git。

1.*本地协议:*最基本的协议，远程存储库可以驻留在任何共享的挂载磁盘上。您可以像这样克隆一个本地存储库:

```
git clone /var/local/repository
```

2.HTTP 协议:它运行在标准的 HTTP/S 端口上。它可以使用用户名/密码等基本认证，而不必设置 SSH 密钥。如果您使用这个，您可以使用相同的 URL 来查看或克隆存储库，就像使用 Github 一样。

3. *SSH 协议:*这是自托管时 Git 最常用的传输协议。这是因为大多数地方已经建立了对服务器的 SSH 访问——如果没有，那么做起来相对简单。

4.Git 协议:Git 协议通常是最快的网络传输协议。这是 Git 附带的一个特殊的守护进程。它监听一个专用端口(9418 ),该端口提供类似于 SSH 协议的服务，但绝对没有身份验证。

对这些协议的深入解释超出了本文的范围。更多细节可以查看[https://Git-SCM . com/book/en/v2/Git-on-The-Server-The-Protocols](https://git-scm.com/book/en/v2/Git-on-the-Server-The-Protocols)。

#### 在 EC2 实例上设置 Git 服务器

现在让我们从您在这里的原因开始——设置一个 Git 服务器。如果您还没有设置一个带有 SSH 访问的 EC2 实例，请按照本指南创建一个。

我在这个练习中使用的是 Ubuntu t2.micro 实例。

a)使用 SSH 登录到 EC2 实例。

```
-> ssh -i ~/.certs/cert.pem ubuntu@54.254.174.183
```

b) Install Git.

```
-> sudo apt-get install git
```

c)创建一个空存储库，作为您的远程 Git 存储库。

```
-> mkdir gitserverexcersise.git-> cd gitserverexcersise.git-> git init --bare
```

这将创建一个空的仓库。此时，您已经有了一个准备克隆的 Git 存储库。但是要从本地系统克隆它，您需要将您的 pem 文件添加到您的 ssh 配置中。

d)打开另一个终端，并将该实例添加到 SSH 配置中。您可以在主目录中找到 SSH 配置文件。ssh 文件夹。如果您没有此文件夹，可以创建一个。并在首选的文本编辑器中编辑或创建配置文件。

```
-> vi .ssh/config
```

为实例添加条目，如

```
 Host gitserver HostName 54.254.174.183 User ubuntu IdentityFile ~/.certs/cert.pem
```

并保存。

e)关闭此终端并打开新终端。现在，您应该能够使用

```
-> ssh gitserver
```

如果您能够登录，那么您可以使用以下命令将您的存储库克隆到您的本地系统:

```
-> git clone gitserver:gitserverexcersise.git
```

恭喜你！您已经成功地设置了一个远程 Git 服务器，现在可以对该服务器进行推和拉操作了。

#### 设置 GitWeb

既然您对项目拥有基本的读/写和只读访问权限，您可能希望设置一个简单的基于 web 的可视化工具。Git 附带了一个名为 GitWeb 的 CGI 脚本，有时会用到它。按照以下步骤安装 GitWeb。

a.登录您的 EC2 实例

```
-> ssh gitserver
```

b)安装 apache2

```
-> sudo apt-get update-> sudo apt-get install apache2
```

c)按照下一步的要求安装“make”

```
-> sudo apt-get install make
```

d)我们将获得 GitWeb 附带的 Git 源代码，并生成定制的 CGI 脚本:

```
-> git clone git://git.kernel.org/pub/scm/git/git.git-> cd git-> make GITWEB_PROJECTROOT=”/home/ubuntu” prefix=/usr gitweb-> sudo cp -Rf gitweb /var/www/
```

GITWEB_PROJECTROOT 是您的 Git 存储库的位置。

e)为 Apache 添加虚拟主机

```
-> cd /etc/apache2/sites-enabled/
```

将 conf (000-default.conf)更新为

```
<VirtualHost *:80>      DocumentRoot /var/www/gitweb      <Directory /var/www/gitweb>            Options +ExecCGI +FollowSymLinks +SymLinksIfOwnerMatch            AllowOverride All            order allow,deny            Allow from all            AddHandler cgi-script cgi            DirectoryIndex gitweb.cgi      </Directory&gt;</VirtualHost>
```

f)加载 mod_cgi 模块

```
-> sudo a2enmod cgi
```

g)重新启动 apache

```
-> sudo service apache2 restart
```

恭喜你！GitWeb 准备好了。在您能够在[http://54.254.174.183](http://54.254.174.183)/(对您来说，它将是您的实例的公共 URL)上访问 GitWeb 之前，您需要做的最后一件事是:允许 TCP 端口 80 为您的实例打开。您可以通过[为您的实例更改您的安全组设置](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/authorizing-access-to-an-instance.html)来做到这一点。

如果所有这些对你来说都太复杂，你可以使用其他替代方案，比如 [GitLab](https://about.gitlab.com/) 。GitLab 是一个数据库支持的 web 应用程序，所以它的安装比其他一些 git 服务器要复杂一些。幸运的是，这个过程有很好的文档记录和支持。

如果你喜欢这篇文章，并且帮助你建立了 Git 服务器，那么就点击这里，让其他人看到它。关注我的其他文章。