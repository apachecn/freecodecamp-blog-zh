# 如何设置用于 Web 开发的 Mac

> 原文：<https://www.freecodecamp.org/news/how-to-set-up-your-mac-for-web-development-b40bebc0cac3/>

虽然你只需要一个文本编辑器和浏览器就可以构建基本的网站，但你可能想通过在你的工作流程中添加像 [React](https://reactjs.org/) 或 [Vue](https://vuejs.org/) 这样的 JavaScript 框架和像 [Git](https://git-scm.com/) 这样有用的工具来升级你的游戏。

但是等等！你的 Mac 还没准备好。在开始之前，您需要安装几个项目来避免以后出现令人困惑的错误。？

本文将指导您完成在 Mac 上启动和运行基于 JavaScript 的 web 开发所需的最低设置。

我们走吧！

#### 更新您的 Mac

在安装任何新软件之前，请按照苹果公司的这些说明将 macOS 和您当前的软件升级到最新版本。

#### 选择终端应用程序

因为您将在本文中使用命令行与 Mac 进行交互，所以您将需要一个终端应用程序。

以下任何选项都是不错的选择:

*   [超级](https://hyper.is/)
*   [iTerm2](https://www.iterm2.com/)
*   [Visual Studio 代码](https://code.visualstudio.com/docs/editor/integrated-terminal)的集成终端
*   [终端](https://support.apple.com/en-ca/guide/terminal/welcome/mac)(Mac 自带的默认应用)

如果你不确定选择哪一个，请选择 Hyper。

#### 命令行开发工具

首先你需要从命令行安装 Mac 的**命令行开发工具**。现在安装这些将防止[奇怪的错误](https://stackoverflow.com/questions/32893412/command-line-tools-not-working-os-x-el-capitan-macos-sierra-macos-high-sierra)以后。

要检查工具是否已经安装，请在您的终端应用程序中键入以下命令，然后按 return 键:

```
xcode-select --version
```

如果结果不是版本号，请使用以下命令安装工具:

```
xcode-select --install
```

将出现一个对话框，询问您是否要安装工具。点击**安装**，软件包会自行下载安装。

安装完成后，通过重新运行第一个命令来确认工具现已安装:

```
xcode-select --version
```

结果应该是一个版本号。

#### 公司自产自用

我们将使用 [Homebrew](https://brew.sh/) ，而不是通过访问每个工具的网站、找到下载页面、点击下载链接、解压文件并手动运行安装程序来安装接下来的几个工具。

**Homebrew** 是一款可以让你从命令行在 Mac 上安装、更新和卸载软件的工具。这比手工方法更快，更安全，通常会让你的开发更容易。

首先，检查是否已经安装了 Homebrew:

```
brew --version
```

如果您没有看到版本号，请使用以下命令安装 Homebrew:

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

我保证这是你在这篇文章中看到的最奇怪的命令！？多亏了家酿，剩下的就简单了。

在继续之前，确认现在安装了自制程序:

```
brew --version
```

#### 节点和国家预防机制

Node.js 是一个允许你的 Mac 在网络浏览器之外运行 JavaScript 代码的工具。如果你想在 Mac 上运行 React 或 Vue 这样的 JavaScript 框架，你需要安装 Node。

Node 还包括 [npm](https://www.npmjs.com/) (节点包管理器)，它让你可以访问一个巨大的免费代码库，你可以下载并在你的项目中使用。

首先，检查节点是否已经安装:

```
node --version
```

如果没有，用自制软件安装:

```
brew install node
```

最后，确认节点和 npm 现已安装:

```
node --version
```

```
npm --version
```

#### 使用 Git 进行版本控制

Git 是一个工具，它帮助你跟踪代码的变化，并与其他开发者在共享项目上合作。

在每个项目中使用 Git 是一个很好的习惯，可以让你为将来可能需要 Git 的项目做好准备。一些工具(如 [GatsbyJS](https://www.gatsbyjs.org/) )也依赖于安装在 Mac 上的 Git，所以即使你不打算将它添加到你的工作流程中，你也会需要它。

同样，首先检查是否已经安装了 Git:

```
git --version
```

如果没有，请安装它:

```
brew install git
```

并确认安装工作正常:

```
git --version
```

偶尔运行下面的命令，你用 Homebrew 安装的所有东西都会自动更新:

```
brew update && brew upgrade && brew cleanup && brew doctor
```

您只需要一个命令就可以让您的系统保持最新。？

当我开始一个新项目时，我通常会运行它，但是如果你喜欢，随时都可以这样做。(当您运行此命令时，如果 Homebrew 建议您运行其他命令，请继续运行它们。如果命令以`sudo`开头，并且提示您输入密码，请使用 Mac 的管理员密码。)

命令行到此为止！

#### 代码编辑器

虽然您可以在任何文本编辑器中编写代码，但是使用一个突出显示和验证代码的编辑器将会使您的工作更加轻松。

以下任何选项都是不错的选择:

*   [Visual Studio 代码](https://code.visualstudio.com/)
*   [Atom](https://atom.io/)
*   [崇高的文字](https://www.sublimetext.com/)

如果您刚刚入门，请选择 Visual Studio 代码。

#### 浏览器

在您编写代码时，在浏览器中查看您正在构建的应用程序或网站有助于确认其工作正常。虽然您可以使用任何浏览器来实现这一点，但有些浏览器包含额外的开发人员工具，向您显示有关代码的详细信息以及如何改进代码。

以下任一选项都是不错的选择:

*   [铬合金](https://www.google.com/chrome/)
*   [火狐](https://www.mozilla.org/firefox/)

如果你刚入门，选择 Chrome。

#### 探测器

这里有个小提示:你会想要显示你的 Mac 默认隐藏的文件。(例如，git 文件是自动隐藏的，但有时您会想要编辑它们。)

显示 Mac 隐藏文件最简单的方法是使用键盘快捷键 **⌘⇧.** (Command + Shift +句点)。这将交替显示和隐藏这些文件，以便您可以在需要时访问它们。

#### 结论

你都准备好了！？

这就是在 Mac 上启动并运行基于 JavaScript 的前端开发所需的全部内容。

为了防止混淆，我省略了任何不是严格要求的项目。如果你想更深入地了解可选的方式，你可以为网络开发进一步定制你的 Mac，请查看下面的链接。

### 进一步阅读

*   [建立一个全新的 Mac 用于开发](https://www.taniarascia.com/setting-up-a-brand-new-mac-for-development/)Tania Rascia
*   [为前端开发设置 MacBook】作者 Ben Honeywill](https://www.bhnywl.com/blog/setting-up-a-macbook-for-front-end-development/)
*   [离开家园:寻找最好的全面的本地开发环境](https://webdevstudios.com/2018/09/27/finding-the-best-all-around-local-development-environment/)

[在推特上讨论](https://twitter.com/search?q=upandrunningtutorials.com/how-to-set-up-a-mac-for-web-development)

* * *

*最初发表于[michaeluloth.com](https://www.michaeluloth.com/how-to-set-up-a-mac-for-web-development/)。*