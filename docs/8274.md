# 如何开始使用 React Native

> 原文：<https://www.freecodecamp.org/news/how-to-get-started-with-react-native-8ef42f65160a/>

作者斯潘塞·卡利

# 如何开始使用 React Native

![VTpwP74hlAqi6DmuXks6XMSZbiFCswQEf0MO](img/4bd5e65b933d36e2045349ffd68f8d6f.png)

在开始之前，我想告诉你一个小故事——我一直想做一个简单的网站。不是 web app，只是一个简单的网站。我已经很久没有这样做了，所以我开始四处寻找如何去做…

…然后我发现自己掉进了一个越来越复杂的兔子洞，使用不同的工具，忘记了我真正想要构建的东西。

我最终放弃了我所做的一切(反正也没用)，注册了一门课程，只是跟着学，这样我就可以在着手我的项目之前对事情有所了解。

我的大部分时间都花在 React Native 上，在网上我看到很多人和我的情况一样。我每周给几十个同样对学习 React Native 感兴趣的人发邮件和聊天。他们从朋友或同事那里听说过它，在 Twitter 上看到过它，一个客户**坚持**在一个项目中使用它，或者十几个其他原因中的一个。技术人员学习新事物的方式和原因各不相同。

有些人来自 web 开发背景，有些人使用过像 Cordova 这样的工具，还有一些人第一次进入 JavaScript 世界。不管一个人的背景如何，都会出现许多相同的事情。

### **JavaScript 语法困境**

```
class App extends React.Component { ... }
```

好吧，我以前看过课。没什么大不了的——但是在 JavaScript 中呢？

```
const { amount, purchaseDate } = this.props;
```

嗯，等号左边的花括号是什么？

```
export default App;export App;
```

但是，有什么区别呢？

不管你是否熟悉 JavaScript，ES2015/ES6 的使用都会让人犯很多错误。这在 React Native 中很常见。使用过 JavaScript 的人通常没有使用过这种(相对)新语法。而且刚学 JavaScript 的人经常参考不使用它的教程。这导致了更多的混乱。

只要知道你在 Javascript 教程中看到的仍然适用，就像你以前学到的一样。ES2015/ES6 只是一个扩展，它使事情变得更容易(一旦你熟悉它)。

要了解 ES2015/ES6，请查看 Babel 的这个[不无聊的介绍。还有一个](https://babeljs.io/learn-es2015/)[尼斯系列](https://medium.freecodecamp.com/learn-es6-the-dope-way-i-const-let-var-ae828580472b)会向你介绍和解释事情。

#### 对反应有基本的了解

我知道你想直接进入 React Native——它太棒了。但是如果你想减少混乱，那么我建议花一点时间理解 React 应用程序的基础。

有一些术语你会想知道，你会想了解你如何组成一个应用程序。React Native 是 React 的扩展。只是不同的客户端目标使用 React 及其原理来创建应用程序。

掌握 React 是对时间的有效利用。光是浏览[主页](https://facebook.github.io/react/)就能帮你不少忙。我也建议你浏览一下[官方教程](https://facebook.github.io/react/tutorial/tutorial.html)，以便更好地掌握。

您不必在这里花费大量时间用 React 构建复杂的 web 应用程序，只需利用这段时间熟悉 React 的思想。

### 设置开发环境

你不需要一个特殊的文本编辑器。无论你一直在使用什么，都可能会工作得很好。现在不要担心编辑。

现在，如果你想开发一个可以在 iOS 和 Android 上运行的 React 原生应用，你必须为这些平台安装所有的开发工具，对吗？

嗯，不。至少现在不是。

有一个名为 [Expo](https://expo.io/) 的工具，它会处理所有原生开发环境的东西，因此您可以专注于学习 React Native 并使用它构建应用程序。

但是等等——还有更好的！有一个命令行工具叫做 [Create React Native App](https://github.com/react-community/create-react-native-app) ，这使得**更容易**开始使用 React Native。它得到了 Expo 的支持，这意味着我们所要做的就是安装命令行界面，然后我们就可以开始 React 本地比赛了！

扫描博览会 app 输出的二维码，开始黑吧！代码将在每次保存文件时更新。

### 我如何…

这里有一个快速的、固执己见的工具列表，用来处理常见的需求:

*   管理状态:[还原](http://redux.js.org/)
*   使用远程 API: [Redux Saga](https://redux-saga.js.org/)
*   导航:[反应导航](https://reactnavigation.org/)
*   分享应用:[发布到博览会](https://docs.expo.io/versions/v17.0.0/guides/exp-cli.html)
*   应用程序样式: [React 本机扩展样式表](https://github.com/vitalets/react-native-extended-stylesheet)
*   一个代码编辑器: [Visual Studio 代码](https://code.visualstudio.com/)

我希望这能帮助你更快地开始使用 React Native，并且减少混乱！

我整理了一个免费的视频课程，带你构建一个 React 原生应用，从设置开发环境到发布到 Expo。花点时间了解 ES2015，对 React 有个大致的了解，然后开始学习本课程。它已经帮助了数百人，我希望它也能帮助你！

[**React 原生基础知识:构建货币转换器**](http://learn.handlebarlabs.com/p/react-native-basics-build-a-currency-converter)
[*学习使用导航、设置 Redux、设计组件、使用远程 API，以及更多*learn.handlebarlabs.com](http://learn.handlebarlabs.com/p/react-native-basics-build-a-currency-converter)

如果你喜欢这篇文章，一定要推荐给想学习 React Native 的人。