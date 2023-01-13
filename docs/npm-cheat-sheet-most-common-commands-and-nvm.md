# npm 备忘单-最常见的命令和 nvm

> 原文：<https://www.freecodecamp.org/news/npm-cheat-sheet-most-common-commands-and-nvm/>

或者节点包管理器，是任何 Node.js 开发者最常用的工具之一。这里列出了使用`npm`时最常用的命令。

## **安装`package.json`依赖**

```
npm install
```

### 速记

```
# install
npm i <package>

# uninstall
npm un <package>

# update
npm up <package>
```

### 旗帜

`-S`与`--save`相同，`-D`与`--save-dev`相同。

## **列出全球安装的软件包**

```
npm list -g --depth=0
```

## **卸载全局包**

```
npm -g uninstall <name> 
```

## **在 Windows** 上升级`npm`

```
npm-windows-upgrade
```

## **更新全局包**

要查看哪些包需要更新，请使用:

```
npm outdated -g --depth=0
```

要单独更新全局软件包，您可以使用:

```
npm update -g <package> <package> <package>
```

## **列出可运行的脚本**

```
npm run
```

## **更新`npm`**

```
npm install -g npm@latest

# using windows? Then use
npm-windows-upgrade
```

## **已安装版本**

```
npm list # for local packages
```

## **节点版本管理器`nvm`**

`nvm`使得在 Node.js 的不同版本之间切换变得容易。在该项目的 [GitHub 页面](https://github.com/nvm-sh/nvm)上阅读更多关于它的信息。

一旦安装了`nvm`,如果您想安装最新版本的节点 v12，只需运行:

```
nvm install 12
```

如果您的工作区中安装了多个版本的 Node.js，您可以通过编写以下内容切换到特定版本:

```
nvm use 10.19.0
```

### **使节点版本默认**

要为您的工作区设置 Node 的默认版本，只需键入:

```
nvm alias default 12
```

其中 12 的最新版本是您希望默认使用的版本。

### 更新`npm`

如果您使用通过`nvm`安装的节点，最好使用以下命令更新您的`npm`版本:

```
nvm install-latest-npm
```

## 更多信息:

*   [这些 NPM 技巧将让你成为职业选手](https://www.freecodecamp.org/news/10-npm-tricks-that-will-make-you-a-pro-a945982afb25/)
*   [如何在 Windows 上安装 Node.js 和 NPM](https://www.freecodecamp.org/news/how-to-install-node-js-and-npm-on-windows/)
*   npm 与 npx 有什么不同？