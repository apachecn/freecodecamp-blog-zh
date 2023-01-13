# 如何在 Ubuntu 上安装 Rails 并将 Ruby 更新到最新版本

> 原文：<https://www.freecodecamp.org/news/how-to-install-rails-on-ubuntu-and-update-ruby-to-the-latest-version/>

几个月前，当我第一次学习 Ruby-on-Rails 时，我不得不与一个编码伙伴一起进行一个协作项目。我们不断遇到问题，因为他为这个项目设置了不同版本的 Rails 和 Buby。我不知道如何安装项目需要的版本。

这是我希望拥有的指南。它还向您展示了如何根据您正在进行的项目来切换您正在使用的 Ruby 或 Rails 的版本。

首先，让我们安装最新版本的 Ruby。为此，我们需要安装一个名为 **RVM - Ruby 版本管理器的包。这个包允许我们在我们的 Ubuntu 机器上安装任何版本的 Ruby，并允许我们在版本之间切换。**

这里的所有代码都将使用 Ubuntu CLI/终端运行。

## 安装 RVM

1.  首先，我们需要安装一个先决条件。打开你的 Ubuntu 终端，输入命令:

```
sudo apt-get install software-properties-common 
```

接下来，我们需要添加 **PPA(个人包存档)**。一个 PPA 就是我们如何获得开发者发布的文件，这些文件还没有被发布到官方的 Ubuntu 包/应用商店。

这也是开发者在等待 Ubuntu 测试并在官方商店发布软件的同时发布其软件最新版本的一种方式。

```
sudo apt-add-repository -y ppa:rael-gc/rvm
```

上面的命令将 PPA 添加到我们可以从 Ubuntu 机器上下载软件包的位置列表中。

接下来，让我们通过运行以下命令来刷新我们的包列表:

`sudo apt-get update`

最后，让我们安装 RVM 本身。

```
sudo apt-get install rvm
```

现在重新启动您的终端以使您的更改生效。然后，键入`rvm version`并点击`enter`来检查 rvm 是否已安装。您应该会得到这样的回应:

`rvm 1.29.10 (manual) by Michal Papis, Piotr Kuczynski, Wayne E. Seguin [[https://rvm.io](https://rvm.io)]`

## 安装 Ruby

现在我们可以安装最新的 Ruby 版本 2.7.1 了。运行命令`rvm install 2.7.1`。或者，您可以运行`rvm install ruby`，它将安装最新的稳定版本(这将安装 v2.7.0)。

运行`rvm ls`来查看你安装了哪些 Ruby 版本。要在 Ruby 版本之间切换，运行`rvm use <version_number>`(例如`rvm use 2.7.1`)。

## 安装 Ruby-on-Rails

Rails 的最新版本是 6.03。Rails 只是一个 Ruby 宝石，安装了 Ruby 我们就可以安装 Rails 了！运行`gem install rails`安装最新版本的 Rails。

最后，为了检查一切顺利，运行`rails -v`。你应该把`Rails 6.0.3.2`拿回来，因为这是发表这篇文章时的最新版本。

您现在可以通过键入`rails new myapp`开始您的第一个 Rails 项目。

嘿，你现在上轨道了！