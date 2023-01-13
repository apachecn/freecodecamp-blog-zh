# 如何在任何机器上轻松运行任何 Linux 工具

> 原文：<https://www.freecodecamp.org/news/how-to-run-any-binary-of-any-platform-without-messing-up-with-your-workstation-dade18c18801/>

弗拉维奥·德·斯特凡诺

# 如何在任何机器上轻松运行任何 Linux 工具

![HLG57lJCSiincPYvc08Y6C1OQ0vcUSqdGvH9](img/3fca41e3157c7daf415d921098204a91.png)

你遇到过像下面这样的情况吗？

**情况 1** :你在你的 Linux 工作站上，有一个你必须执行的 PHP 代码。但是这段代码只在 PHP 7 下运行，你的工作站只有 PHP 5。

**情景 2** :您正在 MacBook 笔记本电脑上工作，非常需要 Kali Linux 发行版中的 sqlmap 工具。但是您无法访问您的虚拟机。

**情况 3** :你在你的 Windows PC 上，你立刻需要一个 NGINX 服务器来服务你目录中的静态文件。

**情况四**:无论哪个平台，你都要启动你的 Node.js 10 项目。但是您的平台上没有安装 Node.js。

或者，总的来说，你有没有遇到过这样的情况:

**情况 X:** 你在一个平台上，你马上需要一个特定的 Linux 工具，不需要改变你的配置或者安装额外的软件。

所有这些情况都可以用一个你可能已经听说过的工具轻松解决。它不会因为安装额外的软件或编辑长期有效的配置而搞乱你的电脑。

**Docker** 是 OS 级的虚拟化系统。它有可能运行你所想到的任何二进制文件。此外，它可以在一个隔离的系统中运行，所以它不能接触你的文件和你宝贵的工作配置。

你所需要的就是有人已经把你的二进制文件打包了，这样你就可以简单地把它作为映像下载了。已经有一大堆 Docker 制作的图片在等着你了。

Docker 做的确实不止这些。它是开发人员和系统管理员使用容器开发、部署和运行应用程序的平台。如果你只使用它来运行你喜欢的二进制文件，你就使用了它 1%的特性。

但是让我们从头开始。

您可以点击[此链接](https://docs.docker.com/install/overview/)并从左侧菜单中选择您的平台，在您的机器上安装 Docker。然后，跟着向导走。

一旦你安装了 Docker，打开你喜欢的终端或命令提示符。

#### 基本概念

首先，我们来测试一下你的 Docker 配置是否工作正常。从终端:

```
> docker --version
Docker version 18.03.0-ce, build 0520e24
```

如果 Docker 启动并运行，您应该会看到您的版本号。

你现在需要的是`docker run`命令。

首先要知道的是你要用的图像的名字。对于官方图片，你通常有二进制文件的名字，没有附加。

例如，在 php 的例子中，图像名就是 PHP。而版本呢？同样简单，只需添加版本号(如 7)。

现在让我们运行我们的第一个容器。

#### 情况 1

> 您在 Linux 工作站上，有一段 PHP 代码必须执行。但是这段代码只在 PHP 7 下运行，你的工作站只有 PHP 5。

好，现在让我们想象我们有这个简单的代码。它只在 PHP 7 下工作，因为飞船操作符:

```
<?php echo 1 <=> 0;
```

我们如何用 Docker 执行这段代码？让我们构建我们的`docker run`命令。

```
> docker run -it php:7
Interactive shell
php > echo 1<=>0;
1
```

是的——这正是我们所需要的！

额外的部分是`-it`旗，但这并不难。因为我们是在交互式 shell 中，所以它简单地指定这个容器应该:

*   `-t ( — tty)`:分配一个[伪 TTY](https://unix.stackexchange.com/questions/21147/what-are-pseudo-terminals-pty-tty)
*   `-i ( — interactive)`:保持[标准输入](https://en.wikipedia.org/wiki/Standard_streams)打开，即使没有连接

除了一些例外，大多数时候你都应该使用它们。

#### ***情况二***

> 您正在 MacBook 笔记本电脑上工作，非常需要 Kali Linux 发行版中的 sqlmap 工具。但是您无法访问您的虚拟机。

不幸的是，sqlmap 没有正式的简单图像名称。但也许其他人创造了一个形象。让我们搜索一下。

```
> docker search sqlmap
NAME                     DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
paoloo/sqlmap            Dockered sqlmap. Build instructions: https:/…   6
k0st/alpine-sqlmap       sqlmap on alpine (size: ~113 MB)                3                                       [OK]
jdecool/sqlmap           sqlmap (Automatic SQL injection) in a contai…   2                                       [OK]
harshk13/kali-sqlmap     Kali Linux base image with Sqlmap               1
marcomsousa/sqlmap       Simple image that execute Automatic SQL inje…   1                                       [OK]
....
```

我们有几个选择。这种情况经常发生。在大多数情况下，图像应该是第一个(或具有较大星数的图像)。

让我们使用它。

```
> docker run -it paoloo/sqlmap --url http://localhost
         _
 ___ ___| |_____ ___ ___  {1.0.9.32#dev}
|_ -| . | |     | .'| . |
|___|_  |_|_|_|_|__,|  _|
      |_|           |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program.
...
```

所有在`[docker run -it {image}]` 之后的参数都被传递给 Docker 中执行的二进制文件，在本例中是 sqlmap。

很简单，对吧？是的，但是有一个缺点。

sqlmap 将日志文件写到磁盘上的`~/.sqlmap`路径中。但是由于 Docker 容器运行在一个隔离的环境中，我们失去了一切！！

这是一个特性，但是在这种情况下对我们来说代表了一个 bug 让我们来修复它。

为了实现持久性，使我们不会丢失日志文件，我们必须在工作站(主机)和 Docker 容器之间创建一个绑定挂载。

让我们决定我们的主机绑定挂载目录是`/tmp/sqlmap`。这应该是一个专门为此目的创建的空目录！

```
> docker run -it -v \
  /tmp/sqlmap:/root/.sqlmap \
  paoloo/sqlmap \
  --url http://localhost
```

使用`-v`选项，我们将创建一个绑定挂载。第一个参数是主机路径，第二个参数是我们想要映射的容器上的路径。

事实上，所有的东西都被保存了——包括我们的报告。

#### 情况 3

> 你在你的 Windows PC 上，你立刻需要一个 NGINX 服务器来服务你目录中的静态文件。

你可能已经注意到，第一次运行`docker run`**时，它会从[Docker Hub](https://hub.docker.com)下载图像。**

**这可能是数百千兆字节。这是因为我们下载了图像的标签 latest(默认)。**

**但是大多数图像也有相同图像的“阿尔卑斯”版本。它使用 Linux Alpine OS。这是 Linux 的优化版，大概占用 130MB。**

**让我们在这种情况下使用它。我们知道前面的图像名称是`nginx`(因为它是官方图像)。**

**所以最终的图像名称会是`nginx:alpine`。如果想要特定版本(比如 1.14)，使用`nginx:1.14-alpine.`**

**你可能会有更多的问题。我们如何知道 NGINX 容器使用哪个目录来服务我们的文件？我们怎么知道它暴露了哪个端口？**

**幸运的是，你所有问题的答案都在 Docker Hub 中。**

**所以，概括一下:**

*   **我们必须共享我们的目录来为容器提供服务。同样，这可以使用绑定挂载来完成:`-v $(pwd):/usr/share/nginx/html`**
*   **通过在末尾添加`:ro`，我们确信容器以只读模式使用我们的文件。**
*   **我们必须将容器暴露的端口绑定到主机，然后在我们的主机上通过 TCP 进行通信:`-p 80:80`**

```
`> docker run \
  -v $(pwd):/usr/share/nginx/html:ro \
  -p 80:80 \
  nginx:alpine`
```

#### ****情况四****

> **不管是哪个平台，都得启动你的 Node.js 10 项目。但是您的平台上没有安装 Node.js。**

**也许你现在明白它是如何工作的了。在这里，我们必须共享我们的内容并绑定端口。**

**但是，我们不知道容器工作目录。相反，我们将使用`-w`标志显式地将其设置为我们选择的自定义目录。例如，您可以选择`/src`——只是不要覆盖现有的目录！**

```
`> docker run \
  -p 3000:3000 \
  -v $(pwd):/src \
  -w /src \
  node:10-alpine \
  node main.js
Example app listening on port 3000!
...`
```

**够简单够强大？**

**此外，您是否想要一个“快捷方式”来只执行二进制文件而不搜索第三方映像？**

**为什么不试试我的简单工具 DR ？**

**我希望你在未来的二进制文件中使用 Docker！:)**