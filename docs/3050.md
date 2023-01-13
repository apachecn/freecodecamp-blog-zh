# 如何在 Ubuntu 上安装 Node.js 并将 npm 更新到最新版本

> 原文：<https://www.freecodecamp.org/news/how-to-install-node-js-on-ubuntu-and-update-npm-to-the-latest-version/>

如果您尝试使用 apt-package manager 安装最新版本的 node，您将会得到 **v10.19.0** 。这是 ubuntu 应用商店的最新版本，但不是 NodeJS 的最新发布版本。

这是因为当一个软件的新版本发布时，Ubuntu 团队可能需要几个月的时间来测试并在官方 Ubuntu 商店发布。因此，要获得任何软件的最新版本，我们可能必须使用开发者发布的私有软件包。

在本教程中，我们想要做的是获得 Node 的 **v12.18.1** (LTS -长期支持)或 **v14.4** 。要获得最新版本，我们可以使用**节点源**或 **nvm** (节点版本管理器)。我将向您展示如何使用这两者。

这里的所有命令都将使用 Ubuntu CLI/终端运行。

## 使用 NVM -我的首选方法

我喜欢 nvm，因为它允许我为不同的项目使用不同的节点版本。有时，您可能正在与使用不同版本 node 的人合作一个项目，您需要根据项目需要切换 node 版本。对于这一点，nvm 是最好的工具。

## 安装 NVM

`curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash`

要检查是否安装了 nvm，请键入`nvm --version`。如果你像`0.35.3`一样拿回一个版本号，那么你就知道 nvm 安装成功了。

**重新启动终端以使您的更改生效。**

## 安装 NodeJS

接下来，让我们安装 Nodejs 版。

只需运行`nvm install 14.4.0`。

您可以使用类似的命令来安装您想要的任何版本的节点，例如`nvm install 12.18.1`。

该命令自动安装 **nodejs** 以及最新的 **npm** 版本，该版本位于`v6.14.5`。

如果您需要切换节点版本，您可以简单地运行`nvm use <version-number>`，例如`nvm use v12.18.1`。

要列出您随 nvm 安装的不同节点版本，请运行`nvm ls`。

## 安装节点源

运行下面的命令告诉 Ubuntu 我们想从 nodesource 安装 Nodejs
包。

`curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -`

**注意**v 14 . 4 . 0 是 Node 的最新版本，但目前不提供 LTS 长期支持。要安装最新版本的 LTS 节点，请将上面命令中的`14`更改为`12`。

可能会提示您输入 root 用户的密码。输入并按回车键。

## 安装 NodeJS

一旦我们完成了 Nodesource 的设置，我们现在就可以安装 Nodejs v14.4。

完成后，我们可以检查是否安装了最新版本的 Node。
只需在你的终端键入`nodejs -v`，它应该会返回`v14.4.0`。

此时，您应该已经自动安装了 npm。要检查您拥有的 npm 版本，请运行`npm version`。如果您没有获得包含最新版本 npm 6 . 14 . 5 的对象，`{ npm: '6.14.5' }`，则可以通过运行以下命令手动更新 NPM:

`npm install -g npm@latest`。

如果您遇到 npm 由于未安装而无法更新的任何问题，您可以首先使用`sudo apt-get install -y npm`安装 npm，然后运行上面的命令来更新它。

为了运行某些 npm 包，我们还需要运行下面的命令
`sudo apt install build-essential`。

就是这样！

你已经在你的 Ubuntu 机器上安装了最新版本的 NodeJS 和 NPM。

去打造伟大的产品:)