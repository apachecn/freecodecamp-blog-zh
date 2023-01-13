# Git 首次设置

> 原文：<https://www.freecodecamp.org/news/git-first-time-setup/>

Git 是一个免费的开源分布式版本控制系统。

到目前为止，Git 是当今世界上使用最广泛的现代版本控制系统。Git 是一个分布式的、积极维护的开源项目，最初由 Linux 操作系统内核的著名创建者 Linus Torvalds 于 2005 年开发。

与早期的集中式版本控制系统(如 SVN 和 CVS)不同，Git 是分布式的:每个开发人员在本地都有他们代码库的完整历史。Git 还可以在各种操作系统和 ide(集成开发环境)上很好地工作。

在本文中，我将向您展示如何安装 git，第一次设置它，了解更多/学习高级 Git 概念的有用提示和资源。我们走吧！

我假设你已经知道什么是版本控制，如果你不知道，请查看这张[幻灯片](http://slides.com/bolajiayodeji/introduction-to-version-control-with-git-and-github)以了解更多。

这里快速回顾一下版本控制的含义:版本控制是:随着时间的推移，管理对源代码或文件集的更改的过程。

版本控制是管理源代码或文件集随时间变化的过程。

版本控制软件在一种特殊的数据库中跟踪对代码的每一次修改。如果出现错误，开发人员可以恢复并比较代码的早期版本，以帮助修复错误，同时最大限度地减少对所有团队成员或贡献者的干扰。

* * *

现在你知道了版本控制和 Git 的意思，让我们安装它。

### 对于 MAC OS:

[下载 Git for macOS](http://git-scm.com/download/mac) 或使用[自制软件](https://brew.sh/)安装

```
brew install git 
```

### 对于 LINUX 操作系统:

[为 Linux 下载 Git](https://git-scm.com/download/linux)或者为基于 Debian 的 Linux 系统安装

```
sudo apt-get update
 sudo apt-get upgrade
 sudo apt-get install git 
```

或者为基于 Red Hat 的 Linux 系统安装

```
sudo yum upgrade
 sudo yum install git 
```

### 对于 WINDOWS 操作系统:

[下载 Windows 版 Git](https://git-scm.com/download/win)

#### 这里有一个更详细的不同系统的安装指南，在 [GIT 官方文档](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)上

* * *

现在您的系统上已经有了 Git，让我们来设置 Git 环境。
Git 附带了一个名为`git config`的工具，它允许您获取和设置配置变量，这些变量控制 Git 外观和操作的所有方面。

*   首先设置您的身份、姓名和电子邮件地址，如下所示:

```
git config --global user.name "bolajiayodeji"
 git config --global user.email mailtobolaji@gmail.com 
```

`--global`选项确保这些值在整个系统中使用

*   接下来，设置缺省的文本编辑器，当您需要在 Git 中输入消息时，您将使用这个编辑器。这不是强制性的，如果你不这样配置，Git 将使用你的默认编辑器。如果您想使用其他东西，请按如下方式配置:

```
git config --global core.editor emacs 
```

*   接下来，为 Git 控制台设置颜色。

对于 Linux 操作系统用户，您可以使用第三方 Zsh 配置器，如 [oh my zsh](https://ohmyz.sh/) 来定制您的带有主题的终端外观:)。
要对此进行配置，请执行以下操作:

```
git config --global color.ui true 
```

color.ui 是一个元配置，包括所有不同的颜色。git 命令提供的配置。

现在 Git 已经可以使用了。

## 检查您的设置

```
git config --list 
```

```
user.name=bolajiayodeji
 user.email=mailtobolaji@gmail.com
 color.ui=true 
```

* * *

想学习一些超级 Git 命令？

我写了一篇文章: [Git 备忘单](https://www.bolajiayodeji.com/git-cheat-sheet/)，它涵盖了您将需要的一些重要的 Git 命令。

# 结论

版本控制软件是现代软件开发人员日常实践的重要组成部分。一旦习惯了版本控制系统的强大好处，许多开发人员就不会考虑在没有它的情况下工作，即使是非软件项目。