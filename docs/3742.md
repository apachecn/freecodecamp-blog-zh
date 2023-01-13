# 如何在本地 Ubuntu Linux 机器或虚拟机上设置 LAMP 服务器

> 原文：<https://www.freecodecamp.org/news/how-to-setup-a-lamp-server-on-a-local-ubuntu-linux-machine-or-vm/>

本简要指南的目的是带您完成在本地 Ubuntu Linux 机器或虚拟机上设置 LAMP (Linux、Apache、MySQL、PHP)服务器的过程。

这将允许您使用 PHP 和 MySQL(使用 phpMyAdmin)进行开发。这是 Wordpress 开发所必需的一个常用栈。

## 安装必要的软件包

您需要为 LAMP 服务器安装以下软件包。您可以通过用空格分隔每个软件包来一次性安装它们，或者一次安装一个，如图所示。

我更喜欢一次下载一个，因为这样更容易看到是否有错误。

进入终端并键入以下内容:

*   `sudo apt-get install apache2`
*   `sudo apt-get install php`
*   `sudo apt-get install php-mysql`
*   `sudo apt-get install mysql-server`

然后应该提示您为 MySQL root 用户设置一个密码。设置密码后继续安装:

*   `sudo apt-get install libapache2-mod-php`
*   `sudo apt-get install php-mcrypt`
*   `sudo apt-get install phpmyadmin`

然后应该会提示您使用哪个服务器。按 enter 键选择 Apache。对于高级服务器设置，选择否。

## 将权限更改为/var/www/html

为了让 LAMP 服务器运行 PHP 脚本和文件，需要将它们保存在/var/www/html 目录中。您可以将这个位置视为您的本地服务器。

为了更改此目录，我们需要更改它的权限。在终端中输入命令:

`sudo chown {your ubuntu username} /var/www/html`

## 创建一个到 phpMyAdmin 的符号链接

默认情况下，phpMyAdmin 安装在/usr/share/目录中。我们需要将它移动到本地服务器目录中。

我们通过`cd /var/www/html`导航到我们想要链接的服务器目录

然后通过输入命令`ln -s /usr/share/phpmyadmin phpmyadmin`创建链接。

## 重启 Apache 并测试

运行以下命令重新启动 Apache，设置所做的更改:

`sudo systemctl restart apache2`

然后，您应该能够使用以下命令在/var/www/html 目录中创建一个 info.php 文件:`touch /var/www/html/info.php`

在文件中键入以下 php 代码:

`<?php phpinfo(); ?>`

然后，打开一个浏览器，输入 localhost/info.php，你应该会看到你刚刚写的 php 文件中的一个页面，它提供了关于 php 的信息。

最后，要访问 phpMyAdmin，请在浏览器中转到 localhost/phpmyadmin。默认的 root 用户名是“root ”,密码是您之前为 MySQL 数据库选择的密码。