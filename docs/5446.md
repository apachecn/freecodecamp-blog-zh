# 如何在 MacOS 上设置 PHP、CaddyServer 和 Kirby 以及为什么应该这样做

> 原文：<https://www.freecodecamp.org/news/php-caddyserver-und-kirby-on-macos-96811af35394/>

菲利普·海登鲍尔

# 如何在 MacOS 上设置 PHP、CaddyServer 和 Kirby 以及为什么应该这样做

![8-zxHXFMXpzFd54O33-70iNYm8E0xVgPvLFl](img/3a3c8d7df9824fd97a4994fd5309ce87.png)

Photo by [Max Nelson](https://unsplash.com/@maxcodes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

最近 [Kirby 版本](https://getkirby.com/) 3.0 发布。由于我在过去相当长的一段时间里都在使用版本 2，我想为什么不试一试呢？

就在圣诞节前，我买了一台全新的 MacBook Air，所以很明显，我想马上试用一下 Kirby。我偶然发现了一篇关于如何在 MacOS 上设置 [CaddyServer](https://caddyserver.com/) 和 PHP 的文章。但是我改了一些。

### CaddyServer，PHP 和 Kirby

让我们从我所说的软件的简要概述开始。

#### [PHP](http://www.php.net/)

我想我对 PHP 没什么好说的。它可能是网络世界中最古老的脚本语言之一。PHP 是让你的网站“动态”的语言。这也是我发现和学习的第一种语言。

#### [球童服务器](https://caddyserver.com/)

是我几个月前发现的一个小但非常强大的网络服务器。它有一些非常好的特性，比如自动 SSL / HTTPs 和一个非常简单的配置文件(稍后您会看到)。而且速度非常快。:)

#### 柯比

几年前我发现的另一个伟大的工具。基本上，它是一个基于简单文件结构的 CMS(内容管理系统)。即使你不太了解 PHP，为页面创建模板并扩展整个功能也是相对简单的。

#### 为什么？

既然您已经了解了这三个项目，您可能会问为什么要一起使用它们。这有多种原因:

*   MacOS 有一个默认的 apache2 安装，但是你可能知道，apache 是最大的 HTTP 服务器之一。它被广泛采用，但有一个缺点。它不像其他网络服务器那样消耗内存和 CPU。所以，如果你在路上，它也会疯狂地消耗电池，我不喜欢这样。
*   Caddy 非常轻量级，并不真正使用太多内存/ CPU /电池。
*   如我所说，Kirby 易于定制和扩展。因为它是 CMS，所以从一开始就不需要担心数据库或特定于应用程序的逻辑。这样你可以很快做出原型，从而在你完成任务的时候带来更快的结果和更多的满足感。:)

事不宜迟，让我们开始设置它:

### 首先

安装[自制软件](https://brew.sh/index_de)是必要的，因为有了它，软件包管理变得容易多了。

您可以用一个简单的命令来安装它:

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 服务器端编程语言（Professional Hypertext Preprocessor 的缩写）

现在我们有了一个包管理器，是时候安装正确的 PHP 版本了。Kirby 至少需要 PHP7.1 所以我装了 7.2:)

```
brew install php@7.2
```

还需要一些扩展:

```
brew install freetype jpeg libpng gd
```

### CaddyServer

现在我们已经安装了 PHP 和一些依赖项，我们现在可以安装 CaddyServer 了。

只需从[https://caddyserver.com/download](https://caddyserver.com/download)下载合适的版本。

您将收到一个 zip 文件。提取它并将可执行文件复制到您的$PATH 中的一个路径(在我的例子中是`/Users/phaidenbauer/bin/`):

```
unzip caddy_v0.11.2_darwin_amd64_personal.zipcp caddy /Users/phaidenbauer/bin/caddy
```

### 柯比

下一个是科比。再次从 https://getkirby.com/try 下载压缩文件。
将它提取到你有项目的地方。对我来说，那是`/Users/phaidenbauer/development/`。

让这一切运行起来的下一个重要的东西是 Caddyfile，它告诉 Caddy 要做什么:)

```
localhost:8080tls offroot ./gzip
```

```
rewrite {    r .*    to {path} {path}/ /index.php?{path}&{query}}
```

```
fastcgi / 127.0.0.1:9000 phpon startup launchctl load -w /Users/phaidenbauer/Library/LaunchAgents/homebrew.mxcl.php@7.2.pliston shutdown launchctl unload -w /Users/phaidenbauer/Library/LaunchAgents/homebrew.mxcl.php@7.2.plist
```

让我们过一遍:

告诉它应该监听哪个端口。如果你不想以 root 身份(或通过`set_cap`)运行它，你应该使用 1024 以上的代码。

`tls off`禁用内置的 SSL / HTTPS 功能，因为我们只在本地工作。

`root ./`设置服务目录的根路径。

`gzip`对响应启用 gzip 压缩。

根据 Kirby 的需求重写所有接收到的 URL。(这可能不是最好的解决方案，但对我的开发环境来说很好。)

告诉 caddy 将请求转发给 FastCGI 服务器。在我们的例子中是 PHP。

现在我们有两个特殊的函数。

```
on startup launchctl load -w /Users/phaidenbauer/Library/LaunchAgents/homebrew.mxcl.php@7.2.pliston shutdown launchctl unload -w /Users/phaidenbauer/Library/LaunchAgents/homebrew.mxcl.php@7.2.plist
```

正如我前面提到的，它只是一台 MacBook Air，所以我不想让 PHP 一直运行。尤其是我不需要的时候。最棒的是 caddy 可以在事件上运行命令。在这种情况下，我们使用 startup 和 shutdown 事件来启动和停止 PHP。太好了！检查邮件时不再浪费内存和 CPU。

### 开始整件事:)

现在我们差不多准备好了。最后要做的事情是将它们一起启动并开始工作:

```
cd /Users/phaidenbauer/development/fly.phaidenbauer.comcaddy
```

您应该会看到类似这样的内容:

```
Activating privacy features... done.http://localhost:8080WARNING: File descriptor limit 4864 is too low for production servers. At least 8192 is recommended. Fix with `ulimit -n 8192`.
```

仅此而已。带上你最喜欢的浏览器，去 [http://localhost:8080](http://localhost:8080) 冲浪。根据你下载的是 Kirby-Plainkit 还是 Kirby-Starter kit，你应该会看到一个简单的“Hello”或者一个简单的图库。

仅此而已。黑客快乐:)