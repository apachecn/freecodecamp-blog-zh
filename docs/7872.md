# 如何创建第一台可用于生产的安全服务器

> 原文：<https://www.freecodecamp.org/news/how-to-create-your-first-safe-server-ready-for-production-f1cc60eec69a/>

弗拉维奥·h·弗雷塔斯

# 如何创建第一台可用于生产的安全服务器

在本教程中，我将介绍一些构建您自己的第一个安全服务器的最佳实践。我将列出您需要采取的步骤，以拥有一个功能齐全的服务器，您可以在您的应用程序的**生产**中使用它。

拥有一个安全的服务器不仅仅依赖于遵循一些步骤。这是对新资源的不断探索和永无止境的改进。但是本文可以作为构建您自己的基础设施的第 0 步。

我将使用 Amazon EC2 来运行这些测试，但我也使用过 Amazon LightSail、Digital Ocean、Vultr 和其他一些工具。在所有情况下，它们的配置都是一样的，所以您可以使用您喜欢的提供程序。

![oc74KOmG7JQiUS3NJ4fHjNM8S6BoKZLeyExz](img/22e10dac9b03591309a26f45fdb4b520.png)

Let's make it safe!

所以我们走吧:

### 创建公共和私有 SSH 密钥

在开始之前，让我们创建一对密钥，一些主机在安装服务器时会要求提供这对密钥。如果您决定在用 Amazon 启动机器实例时创建一对密钥，那么可以省略这一步和下一步。

使用 ssh-keygen 工具创建一个 SSH 密钥对。

```
$ ssh-keygen -t rsa -b 4096
```

完成这一步后，您将拥有以下文件:id_rsa 和 id_rsa.pub(私钥和公钥)。**永远不要**分享你的私人密钥。

关于创建密钥的更详细的文档可以在[这里](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/)找到。

### 在 Amazon 上导入公钥:

我们将导入刚刚在 Amazon 平台上创建的公钥。

1.  访问[亚马逊管理控制台](https://us-west-2.console.aws.amazon.com/console/home)
2.  点击 AWS 服务>计算> EC2
3.  点击左侧菜单网络和安全>密钥对
4.  点击“导入密钥对”并上传您的公钥(id_rsa.pub)

### **创建您的机器**

我会在亚马逊 EC2 上安装一个 Ubuntu 版本。您可以在此[链接](https://aws.amazon.com/getting-started/tutorials/launch-a-virtual-machine)找到完整的设置。步骤如下(但只是为了简单起见，跟随这个[链接](https://aws.amazon.com/getting-started/tutorials/launch-a-virtual-machine)以获得更多解释):

1.  访问[亚马逊管理控制台](https://us-west-2.console.aws.amazon.com/console/home)
2.  点击 AWS 服务>计算> EC2
3.  选择启动实例
4.  选择其中一幅图像。我选择了 Ubuntu 服务器 16.04 LTS (HVM)，固态硬盘卷类型(但根据您的需求选择)
5.  选择实例(同样，根据您的需要)。单击查看并启动
6.  打开一个新标签，在 Amazon 上导入创建的公钥。
7.  这里要求我们“选择一个现有的密钥对或创建一个新的密钥对”。我选择了“选择现有的密钥对”。选择您在上一步中上传的文件。
8.  单击“启动实例”。
9.  单击您刚刚创建的实例的链接。

注意:下面的一些步骤可以在亚马逊的初始屏幕上配置。但是因为我想创建一个可以用于其他主机的通用教程，所以我选择了默认配置。

### 连接到新服务器

使用 ssh 访问机器。

在您的终端上键入:

```
$ ssh <USER>@<IP-ADDRESS> -p 22 -i <PATH-TO-PRIVATE-KEY>
```

*   <user>:Linux 系统上的用户。对于亚马逊使用 ubuntu，对于其他人，根</user>
*   <ip-address>:您创建的机器的 IP 地址。它是实例的“描述”选项卡上的“公共 DNS (IPv4)”字段。</ip-address>
*   <path-to-private-key>:您之前在项目上生成的私钥的完整路径(例如/Users/flavio/)。ssh/id_rsa)。</path-to-private-key>
*   -i <path-to-private-key>:如果您将密钥添加到您的 SSH 代理中，那么可以省略这一部分。</path-to-private-key>

### 授予您的新用户访问权限

创建一个名为“向导”的新用户帐户

```
$ sudo adduser wizard
```

给予“巫师”对须藤的许可。打开文件

```
$ sudo nano /etc/sudoers.d/wizard
```

并设置内容:

```
wizard ALL=(ALL) NOPASSWD:ALL
```

创建以下目录:

```
$ mkdir /home/wizard/.ssh# create authorized_keys file and copy your public key here$ nano /home/wizard/.ssh/authorized_keys$ chown wizard /home/wizard/.ssh$ chown wizard /home/wizard/.ssh/authorized_keys
```

复制公钥的内容(公钥的路径)并粘贴到/home/wizard/目录下的远程实例上。ssh/authorized _ key。设置权限:

```
$ chmod 700 /home/wizard/.ssh$ chmod 600 /home/wizard/.ssh/authorized_keys
```

### 保护系统

更新所有当前安装的软件包。

```
$ sudo apt-get update$ sudo apt-get upgrade
```

将 SSH 端口从 22 更改为 2201。配置防火墙(ufw，不复杂的防火墙，它真的不复杂)来允许它。打开文件/etc/ssh/sshd_config

```
$ sudo nano /etc/ssh/sshd_config
```

并更改以下数据:

```
Port 2201PermitRootLogin noPasswordAuthentication no
```

```
# add this to avoid problem with multiple sshd processesClientAliveInterval 600ClientAliveCountMax 3
```

重新启动 ssh 服务:

```
$ sudo service ssh restart
```

将简单防火墙(UFW)配置为仅允许 SSH(端口 2201)、HTTP(端口 80)和 NTP(端口 123)的传入连接。

```
# close all incoming ports$ sudo ufw default deny incoming# open all outgoing ports$ sudo ufw default allow outgoing# open ssh port$ sudo ufw allow 2201/tcp# open http port$ sudo ufw allow 80/tcp# open ntp port : to sync the clock of your machine$ sudo ufw allow 123/udp# turn on firewall$ sudo ufw enable
```

### 配置您的服务器时钟

将本地时区配置为 UTC:

```
$ sudo dpkg-reconfigure tzdata
```

选择“以上都不是”选项，然后选择 UTC。

### 断开连接并将您的密钥添加到您的 SSH 代理中

断开与服务器的连接，并在计算机上执行以下操作。要断开连接:

```
$ exit
```

### 在 Amazon 上添加访问端口权限

亚马逊上需要这一步。我们还将设置希望在 Amazon 上使用的 ssh 端口。

1.  访问[亚马逊管理控制台](https://us-west-2.console.aws.amazon.com/console/home)
2.  点击 AWS 服务>计算> EC2
3.  单击左侧菜单网络和安全>安全组
4.  选择附加到您的实例的那个
5.  点击操作>编辑入站规则
6.  点击“添加规则”并设置:类型:自定义 TCP，端口范围:2201，来源:0.0.0.0/0 和描述:SSH

### 使用新凭据连接

现在，您可以在机器上连接新端口上的用户。

```
$ ssh wizard@<IP-ADDRESS> -p 2201 -i <PATH-TO-PRIVATE-KEY>
```

您已经准备好运行应用程序的服务器。很快我将写另一篇教程，介绍如何安装一个环境来运行使用 pm2 的 Meteor 应用程序。我还将讨论配置 SSL、反向代理、负载平衡器和 Nginx。但是本文展示了如何创建一个通用的更安全的服务器，您可以运行任何您需要的服务器。

如果你喜欢这篇文章，一定要喜欢它，给我很多掌声——它对作者来说意味着整个世界？如果你想阅读更多关于文化、技术和创业的文章，请关注我。

Flávio H. de Freitas 是一名企业家、工程师、技术爱好者、梦想家和旅行家。先后在**【巴西】**【硅谷】和欧洲**担任 **CTO** 。**

**本·怀特在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片**