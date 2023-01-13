# 如何撤销 Git 添加

> 原文：<https://www.freecodecamp.org/news/how-to-undo-a-git-add/>

要在提交前撤消`git add`，请运行`git reset <file>`或`git reset`取消所有更改。

在旧版本的 Git 中，命令分别是`git reset HEAD <file>`和`git reset HEAD`。这在 Git 1.8.2 中有所改变

您可以在这些有用的文章中了解更多关于其他常用 Git 操作的信息:

*   [Git checkout](https://guide.freecodecamp.org/git/git-checkout/)
*   [Git pull vs Git fetch](https://guide.freecodecamp.org/miscellaneous/git-pull-vs-git-fetch/)
*   git ignore

## 这里有更多关于 Git 的背景信息

### **了解 Git 项目的三个部分**

Git 项目将包含以下三个主要部分:

1.  Git 目录
2.  工作目录(或工作树)
3.  部队从一个战场转往另一个战场的集结地

****Git 目录**** (位于`YOUR-PROJECT-PATH/.git/`)是 Git 存储精确跟踪项目所需的一切的地方。这包括元数据和包含项目文件压缩版本的对象数据库。

****工作目录**** 是用户对项目进行本地修改的地方。工作目录从 Git 目录的对象数据库中提取项目文件，并将它们放在用户的本地机器上。

****暂存区**** 是一个文件(也称为“索引”、“暂存”或“缓存”)，它存储了关于下一次提交的内容的信息。提交是指您告诉 Git 保存这些分阶段的更改。Git 获取文件的快照，并将该快照永久存储在 Git 目录中。

通过三个部分，文件在任何给定时间都可以处于三种主要状态:提交、修改或暂存。每当你在你的工作目录中对一个文件做了改变，你就修改了这个文件。接下来，当您将它移动到暂存区时，它就是*暂存的*。最后，在提交之后是*提交*。

## **安装 Git**

*   Ubuntu: `sudo apt-get install git`
*   Windows: [下载](https://git-scm.com/download/win)
*   Mac: [下载](https://git-scm.com/download/mac)

## 配置 Git 环境

Git 有一个`git config`工具，允许你定制你的 Git 环境。您可以通过设置某些配置变量来改变 Git 的外观和功能。从机器上的命令行界面(Mac 中的终端、Windows 中的命令提示符或 Powershell)运行这些命令。

这些配置变量有三个存储级别:

1.  系统:位于`/etc/gitconfig`，将默认设置应用于计算机的每个用户。要对这个文件进行修改，使用带有`git config`命令的`--system`选项。
2.  用户:位于`~/.gitconfig`或`~/.config/git/config`，将设置应用于单个用户。要修改这个文件，使用`git config`命令的`--global`选项。
3.  项目:位于`YOUR-PROJECT-PATH/.git/config`，仅将设置应用于项目。要修改这个文件，使用`git config`命令。

如果存在相互冲突的设置，项目级配置将重写用户级配置，用户级配置将重写系统级配置。

【Windows 用户注意 : Git 在您的`$HOME`目录(`C:\Users\$USER`)中查找用户级配置文件(`.gitconfig`)。Git 还会寻找`/etc/gitconfig`，尽管它是相对于 MSys 根目录的，也就是您在运行安装程序时决定在 Windows 系统上安装 Git 的位置。如果您使用的是 Git for Windows 的 2.x 或更高版本，那么在 Windows XP 上的`C:\Documents and Settings\All Users\Application Data\Git\config`中还有一个系统级配置文件，在 Windows Vista 和更高版本上的`C:\ProgramData\Git\config`中也有。这个配置文件只能由管理员`git config -f FILE`修改。

### 添加您的姓名和电子邮件

Git 将用户名和电子邮件作为提交信息的一部分。您需要在用户级配置文件中使用以下命令进行设置:

```
git config --global user.name "My Name"
git config --global user.email "myemail@example.com"
```

### 更改您的文本编辑器

Git 自动使用默认的文本编辑器，但是您可以更改它。下面是一个使用 Atom 编辑器的例子(`--wait`选项告诉 shell 等待文本编辑器，这样您就可以在程序继续运行之前在其中工作):

```
git config --global core.editor "atom --wait"
```

### 为 Git 输出添加颜色

您可以使用以下命令配置您的 shell 来为 Git 输出添加颜色:

```
git config --global color.ui true
```

要查看所有配置设置，请使用命令`git config --list`。

## 在项目中初始化 Git

一旦在您的计算机上安装并配置了 Git，您需要在您的项目中初始化它，以便开始使用它的版本控制功能。在命令行中，使用`cd`命令导航到项目的顶层(或根)文件夹。接下来，运行命令`git init`。这将安装一个 Git 目录文件夹，其中包含 Git 跟踪项目所需的所有文件和对象。

Git 目录必须安装在项目根文件夹中，这一点很重要。Git 可以跟踪子文件夹中的文件，但是它不会跟踪位于相对于 Git 目录的父文件夹中的文件。

## 在 Git 中获得帮助

如果您忘记了任何命令在 Git 中是如何工作的，您可以通过几种方式从命令行访问 Git 帮助:

```
git help COMMAND
git COMMAND --help
man git-COMMAND
```

这将在您的 shell 窗口中显示该命令的手册页。若要导航，请使用上下箭头键滚动或使用以下键盘快捷键:

*   f 或空格键向前翻页
*   b 向后翻页
*   问退出