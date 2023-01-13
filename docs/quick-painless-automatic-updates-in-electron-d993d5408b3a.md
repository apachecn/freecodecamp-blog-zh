# 快速，无痛，电子自动更新

> 原文：<https://www.freecodecamp.org/news/quick-painless-automatic-updates-in-electron-d993d5408b3a/>

安德里亚·赞恩

# 快速和无痛的电子自动更新

![B-W1RsCzf5EZdetICMxmJMyzL0bqPTlgcgyO](img/4019e354c81904eb20a042d8ed700e67.png)

让我们面对现实:大多数用户不会回到你的网站，下载你的全新电子应用程序的更新。相反，你应该建立某种自动更新系统。

不幸的是，这方面的在线文档既不容易找到也不容易理解。在这里，我将使用 GitHub 作为主机，通过一个快速的过程来引导你设置一个自动更新程序。

### 设置存储库

为了代表您发布，electron-builder 需要一个 GitHub 访问令牌。如果你不知道这些是什么或者如何创建一个，看看 GitHub 的[快速指南](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)。

Electron-builder 需要一个能够访问回购范围的令牌。按照链接中的描述创建一个，并将其复制到安全的地方(您只会看到这个令牌一次！).

### 建立图书馆

我们将使用[electronic-builder](https://github.com/electron-userland/electron-builder)来打包我们的应用程序，所以让我们从安装它开始:

```
npm install electron-builder --save-dev
```

让我们也安装[电子更新器](https://github.com/electron-userland/electron-builder/tree/master/packages/electron-updater)来处理自动更新:

```
npm install electron-updater --save
```

然后，我们需要配置我们的构建。在`package.json`中添加以下代码片段:

我们来一点一点分析一下这个:

*   这个`repository`链接非常简单明了——只要记得用你的替换它就行了！
*   `build`脚本将在本地构建您的应用程序，无需发布。
*   `ship`脚本将构建并发布您的应用程序。

**React 开发者注意事项**:电子构建器和 create-react-app 默认有一些冲突。我创建了一个生成器，它设置了一个电子+反应+电子生成器应用程序，不需要任何配置。你可以在这里找到[。](https://www.npmjs.com/package/generator-react-electron)

现在创建一个名为`electron-builder.yml`的文件，内容如下:

*   `appId`是您的应用程序在操作系统寄存器中的名称。你可以自由选择。
*   `provider`是存储应用程序安装程序的平台。
*   `token`是 GitHub 访问令牌。用您之前创建的文件替换它。

记得把这个文件添加到`.gitignore`中，这样你就不会和全世界分享你的令牌了！；)

### 处理更新逻辑

现在我们需要在我们的电子应用程序中配置更新逻辑。将此整合到您的条目文件中(通常是`index.js`或`electron.js`)。如果您正在创建一个全新的应用程序，那么您可以简单地复制粘贴以下代码:

IPC 模块是在 electronic 进程间发送消息的标准方式。你可以在这里了解更多关于他们的信息。

代码非常简单明了，并且处理更新的电子方面。现在我们必须通知用户。

这是一个 HTML 页面的例子。它显示一个按钮，其标题是“没有更新就绪”或“新版本就绪！”。当按钮被点击时，一个方法被调用，它告诉 Electron 退出并安装新的更新。

### 最后，船

当您准备好发布时，编辑`package.json`中的`version`字段并运行以下命令:

```
npm run ship
```

进入你的库的 GitHub 页面，点击“发布”(与“提交”和“分支”在同一行)。在那里，你会发现一个草案发布。点击【编辑】，然后点击【发布发布】。

当你启动应用程序时，如果按钮显示“没有更新就绪”，不要惊慌。只有在下载完新版本后，这种情况才会改变。

如果您想使用一个功能项目来了解更多信息并开始工作，您可以克隆这个示例库。

如果你觉得这篇文章有帮助，一定要鼓掌？。