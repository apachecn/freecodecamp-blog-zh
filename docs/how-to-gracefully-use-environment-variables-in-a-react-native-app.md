# 如何在 React 本机应用程序中优雅地使用环境变量

> 原文：<https://www.freecodecamp.org/news/how-to-gracefully-use-environment-variables-in-a-react-native-app/>

API 密钥和秘密总是包含一些敏感数据或令牌，需要优雅地保存。为不同的环境(比如开发或生产)管理不同的键是 JavaScript 开发人员的一种常见做法。因此，`.env`文件的机制存在。

React 本地应用程序中有一种方法可以在不集成任何本地代码的情况下保存 API 密钥和其他敏感信息。在这篇短文中，您将学习如何安装和集成一个小的库，帮助您使用环境变量而不暴露敏感信息。

请注意，本文中提到的安装和集成`[react-native-dotenv](https://www.npmjs.com/package/react-native-dotenv)`的步骤可以以类似于下面描述的方式用于 Expo 项目。

* * *

### 要求

要学习本教程，请确保您在本地开发环境中安装了以下软件，并且能够访问下面提到的服务。

*   安装了 npm/yarn 的 [Nodejs](https://nodejs.org/en/) ( > = 8.x.x)
*   [react-native-cli](https://www.npmjs.com/package/react-native-cli) 创建并运行新的 react 原生应用
*   `watchman`:React 原生项目的文件变更监视器

### 入门指南

首先，在终端窗口中使用`react-native-cli`创建一个新项目。

```
react-native init RNEnvVariables

# navigate inside the project directory
cd RNEnvVariables
```

创建项目目录后，导航它。创建一个名为`.env`的新文件。该文件将保存所有 API 密钥或任何敏感信息。确保将这个文件添加到`.gitignore`中，这样就不会在 Github 这样的版本控制网站上暴露任何秘密密钥。

首先，让我们向文件`.env`添加一个名为`SOME_KEY`的模拟密钥。

```
SOME_KEY=something
```

请注意，`.env`文件认为任何引号内的字符串都是有效的。此外，用大写字母写`SOME_KEY`只是一个非常普遍遵循的命名约定。

### 安装 react-native-dotenv

接下来，安装依赖项`[react-native-dotenv](https://www.npmjs.com/package/react-native-dotenv)`,它将帮助您在整个应用程序中优雅地管理您的环境变量。转到终端窗口，并执行以下命令。

```
yarn add react-native-dotenv
```

模块`react-native-dotenv`允许您从`.env`文件导入环境变量。要使它工作，打开`babel.config.js`文件并修改`presets`，如下所示。

```
module.exports = {   
    presets: ['module:metro-react-native-babel-preset', 'module:react-native-dotenv']
}
```

### 运行应用程序

为了验证它正在工作，打开`App.js`并从包本身导入`SOME_KEY`。`react-native-dotenv`解析`.env`文件，让您导入文件中提到的环境变量。

```
// after other imports
import { SOME_KEY } from 'react-native-dotenv'
```

如果您使用 iOS 模拟器或 Android 模拟器在当前状态下打开此演示 React 本机应用程序，您将看到以下屏幕。

![1*ZISAEh-BOnnS3fe9ELSFlA](img/5d966c9792ea5ee5d9a03ddfa1617984.png)

用如下所示的环境变量编辑`App.js`文件中显示**步骤一**的那一行。

```
<Text style={styles.sectionTitle}>{SOME_KEY}</Text>
```

现在回到模拟器，你会注意到变化。

![1*vHCK4XMZdnDKuFT1IDdhZg](img/90f0fa2fd51e30ddf0dfccdd30989853.png)

## 结论

使用`react-native-dotenv`就是这么简单。您不必添加任何本机代码来分别集成每个移动操作系统平台。一个更实际的例子，你可以看看我最近在 React Native 和 Expo app 中关于 [**Firebase 认证的帖子。您会注意到，在 Expo 应用程序中使用我们上面讨论过的相同模块。**](https://heartbeat.fritz.ai/how-to-build-an-email-authentication-app-with-firebase-firestore-and-react-native-a18a8ba78574)

* * *

我在**有空？**

**✉️** [**在这里加入我的每周简讯。**](https://tinyletter.com/amanhimself)