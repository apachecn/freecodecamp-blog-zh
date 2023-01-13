# 如何在 macOS 上首次设置 Git

> 原文：<https://www.freecodecamp.org/news/setup-git-on-mac/>

如果你是第一次在 MacBook 上设置 Git，你不需要费力去完成它。

也许你刚买了一台新的笔记本电脑，或者你第一次用 MacBook 接触科技。不管怎样，我会保护你的。

这篇短文将帮助您了解如何在 macOS 上设置 Git，以便您可以立即开始工作。

我假设在阅读本文之前，您已经知道 Git 是什么以及它是做什么的。但是如果你不知道并且需要 Git 和版本控制的介绍，你可以看看这篇关于什么是 Git 的文章。Git 版本控制初学者指南。

让我们现在开始吧。

## 如何在 Mac 上安装 Git

在 Mac 电脑上安装 Git 有很多方法，但最简单的方法是使用自制软件。你可以在本文档或[这里](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)找到[其他方法以及如何让它们工作。](https://git-scm.com/download/mac)

### 如何用自制软件安装 Git

[Homebrew](https://brew.sh/) 是一款免费开源的软件包管理系统，简化了苹果操作系统(macOS)上的软件安装。你可以用它来安装你将来需要的所有类型的包，而不仅仅是 Git。这使得它非常有用。

你不需要安装一个应用程序或任何东西来安装家酿。您只需打开终端，通过运行以下命令安装 [Homebrew](https://brew.sh/) :

```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)" 
```

**注意:**一旦你输入命令，它会要求你输入密码。

一旦成功，您就可以在终端中通过下面的命令安装 Git 了:

```
$ brew install git 
```

此时，如果成功，您已经在 Mac 上安装了 Git。现在，您可以在终端中运行以下命令进行验证:

```
$ Git --version 
```

## 如何在 macOS 中配置 Git

到目前为止，您已经学习了如何安装 Git——但是仅仅安装 Git 并不能让您从 Git 版本控制中推、拉、提交代码和执行其他 Git 操作。

要使用 Git，您必须使用`git config`命令设置您的 Git 环境。这将使您能够访问控制 Git 如何在您的系统上工作的配置变量。

您需要的两个重要 git 配置变量是 identity 变量。这些让你设置你的用户名和电子邮件。这是您用 GitHub、GitLab 等设置版本控制系统时使用的用户名和电子邮件。

```
$ git config --global user.name "olawanlejoel"
$ git config --global user.email "mymail@gmail.com" 
```

**注意:**用你的名字和邮箱替换。您还应该知道,`--global`选项确保这些值在整个系统中使用。

一旦你完成了这些，你还可以做一些其他的配置，为你的 Git 控制台设置默认的文本编辑器和颜色:

```
$ git config --global core.editor emacs
$ git config --global color.ui true 
```

你可以选择任何你经常使用的编辑器。我选择了 EMACS。

现在 Git 已经准备好供您使用了。您可以使用以下命令检查 Git 配置，以确保它们是正确的:

```
$ git config --list 
```

这将显示以下内容(包含您自己的信息):

```
user.name=olawanlejoel
user.email=mymail@gmail.com
color.ui=true 
```

假设有一个错误，您希望更改任何配置。请随意重新运行针对该错误的 config 命令。

例如，如果我的电子邮件有错误，我可以重新运行电子邮件配置来纠正它:

```
$ git config --global user.email "mynewmail@gmail.com" 
```

现在，当您重新运行`--list`命令时，您将获得更新后的值:

```
user.name=olawanlejoel
user.email=mynewmail@gmail.com
color.ui=true 
```

## 包扎

在本文中，您已经学习了如何首次在 Mac 计算机上设置 Git。

如果你也想知道如何在其他系统上实现，比如 Windows 和 Linux，[看看这个全面的指南](https://www.freecodecamp.org/news/git-first-time-setup/)。

另外，如果你也有兴趣学习一些基本的 Git 命令，这里有一个[详细的备忘单](https://www.freecodecamp.org/news/git-cheat-sheet/)。

祝编码愉快！