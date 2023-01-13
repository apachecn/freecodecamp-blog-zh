# SimpleHTTPServer 解释:如何使用 Python 发送文件

> 原文：<https://www.freecodecamp.org/news/simplehttpserver-explained-how-to-send-files-using-python/>

作为一名 web 开发人员，总有一天你需要创建自己的本地 web 服务器。

也许是因为你将在飞机上，想在远离互联网服务的地方做你的项目。或者，您可能只是想快速访问家庭网络中另一台计算机上的文件。

无论何时何地，建立一个本地 HTTP 服务器都是一项有用的技能。

### 什么是 HTTP 服务器？

简单地说，HTTP 服务器或 web 服务器是一个运行在机器上的进程，它监听传入的请求并提供网页服务。

例如，当你在浏览器中输入`https://www.freecodecamp.org/news/`时，某处有一个服务器在监听这个请求。作为响应，它发送回数据，以便您的浏览器可以呈现 freeCodeCamp 开发者新闻页面。

当然，幕后还有很多事情要做，但是对于本教程来说，这就是你真正需要知道的。

### 如何设置本地 HTTP 服务器

1.  [安装 Python](https://www.freecodecamp.org/news/best-python-tutorial/#installation)
2.  打开命令提示符或终端，运行`python -V`
3.  在*nix 或 MacOS 系统上使用`cd`或在 Windows 上使用`CD`进入项目目录
4.  运行以下命令启动本地 HTTP 服务器:

```
# If python -V returned 2.X.X
python -m SimpleHTTPServer

# If python -V returned 3.X.X
python3 -m http.server

# Note that on Windows you may need to run python -m http.server instead of python3 -m http.server
```

您会注意到两个命令看起来非常不同——一个调用`SimpleHTTPServer`而另一个调用`http.server`。这只是因为在 Python 3 中，`SimpleHTTPServer`模块被整合到了 Python 的`http.server`中。他们都以同样的方式工作。

现在，当你进入 [`http://localhost:8000/`](http://localhost:8000/) 时，你应该会看到你的目录中所有文件的列表。然后你只需点击你想查看的 HTML 文件。

请记住，`SimpleHTTPServer`和`http.server`仅用于局部测试。它们只做非常基本的安全检查，不应该在生产中使用。

### 如何在本地发送文件

要设置一种快速而肮脏的 NAS(网络附加存储)系统:

1.  确定两台电脑通过局域网或 WiFi 连接到同一个网络
2.  打开您的命令提示符或终端并运行`python -V`以确保 Python 已安装
3.  使用 cd(更改目录)命令转到要共享其文件的目录。
4.  在*nix 或 MacOS 系统上使用`cd`或在 Windows 上使用`CD`,转到包含您想要共享的文件的目录
5.  使用`python -m SimpleHTTPServer`或`python3 -m http.server`启动您的 HTTP 服务器
6.  打开新终端，在*nix 或 MacOS 上键入`ifconfig`或在 Windows 上键入`ipconfig`来查找您的 IP 地址

现在在第二台计算机或设备上:

1.  打开浏览器，输入第一台机器的 IP 地址，以及端口 8000: `http://[ip address]:8000`

将打开一个页面，显示从第一台计算机共享的目录中的所有文件。如果页面加载时间过长，您可能需要调整第一台计算机上的防火墙设置。