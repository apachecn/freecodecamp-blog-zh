# 如何设置 Jest 和 Enzyme 来测试 React 原生应用

> 原文：<https://www.freecodecamp.org/news/setting-up-jest-enzyme-for-testing-react-native-40393ca04145/>

山姆·奥拉森

这篇短文分享了我设置测试环境的经验，用 Jest 和 Enzyme 对 React 本地组件进行单元测试。

![nks4F4Jhip65XWA3f1i0GDn6a2TvXE0qhwfh](img/ab427994f7ec96d923962eeca41f24b5.png)

Photo by [Neil Soni](https://unsplash.com/photos/6wdRuK7bVTE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/mobile-app?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

### 测试工具和环境

我学到的第一件事是，为 React 原生应用编写单元测试的方法和基础设施与为 React 应用编写单元测试非常相似……也许这并不奇怪。

然而，尽管工具和测试套件的使用非常相似，但是**测试环境和基础设施必须以稍微不同的方式设置**。这基本上是因为 React 应用程序被设计为在浏览器中使用 DOM，而移动应用程序并不将这种数据结构作为呈现的目标(相反，它们将移动系统上的实际“本地”模块作为目标)。

#### 使用笑话

Jest 是一个用于测试 JavaScript 应用的库。

我想用 Jest 有几个原因:

首先，它是由脸书为他们自己的 React 本地应用创建并积极维护的。

其次，它预打包了我正在使用的 React Native 版本(使用 [react-native](https://github.com/facebook/react-native) 创建)。

第三， **Jest 是一个“全面的”测试框架**，包含了我需要的整套测试工具。例如，Jest 附带了一个库来检查断言，一个测试运行器来实际运行测试，还有一些工具来检查代码覆盖率。对于其他解决方案，人们必须选择和组装测试套件的各个组件。

#### 使用 Jest +酶

我想把笑话和酶结合起来。网上有很多比较“Jest 和 Enzyme”的有点混乱的评论。这有点误导。虽然 Jest 是一个测试框架，但您可以将 Enzyme 视为一个库，它使选择和查询模拟 DOM 中的元素变得更加容易。所以**它经常和 Jest** 一起使用，使得编写的测试逻辑更加清晰易读。

还在迷茫？这类似于 jQuery 引入了简洁明了的语法来查询和选择 DOM 中的元素，而使用普通 JavaScript 的语法(至少在 jQuery 首次引入时)并不清晰易用。人们不经常比较“jQuery 和 JavaScript ”,除非他们比较两种方法用来查询和修改 DOM 元素的特定方式。

注意:你可以在没有酶的情况下使用 Jest(我相信脸书就是这样做的)，但是酶让你的测试更容易创建和阅读。在我看来，把酶和 Jest 结合起来是为了方便。

### 设置 Jest +酶

我必须经历一些困难才能在我的 React 本地环境中成功设置 Jest 和 Enzyme。

Jest 现在包含在使用“react-native”工具创建的 React 本机应用程序中。所以我可以用现成的笑话。精彩！

但是我在尝试使用他们的文档将 Enzyme 和 React Native 结合起来时遇到了一些问题。我从来没有彻底弄清楚潜在的问题是什么，但是我一直收到“模块未找到”的错误，就像这里的[这样的错误。](https://github.com/facebook/react-native/issues/23943)

#### 一个解决方案

最后，我使用了一个解决方案，使用 [jest-enzyme](https://github.com/FormidableLabs/enzyme-matchers/tree/master/packages/jest-enzyme#readme) 库将一些设置抽象到一个预打包的环境中，然后确保 jest‘presets’在我的 package.json 中设置为‘react-native’。

我按照说明安装了这些库:

```
npm install jest-environment-enzyme jest-enzyme enzyme-adapter-react-16 --save-dev
```

当我试图运行我的测试时，错误也引导我自己明确地安装这些:

```
npm install --save-dev react-dom enzyme
```

以下是我必须手动添加到 package.json 中的内容:

```
// package.json before with react-native init

{
...
   "jest": {
       "presets": ["react-native"],
     }
...
}

// package.json after my manual changes:
{
...

"jest": {
       "presets": ["react-native"], // not clear in documentation!
       "setupTestFrameworkScriptFile": "jest-enzyme",
       "testEnvironment": "enzyme",
       "testEnvironmentOptions": {
           "enzymeAdapter": "react16"
       }  
   }
...
}
```

这里可以看到回购[。](https://github.com/SamOllason/jest-enzyme-config-for-react-native/blob/master/README.md)

以这种方式使用 jest-enzyme 库对我来说很容易，这也意味着我有一个稍微干净的设置。这是因为另一种方法(我无法按照 Enzyme 文档进行工作)意味着我还必须建立和维护一个单独的“jest config”脚本。

### 摘要

在 Jest+Enzyme 测试中为 React Native 编写业务逻辑似乎与使用 Jest+Enzyme 为 React 编写测试完全相同。这意味着 React 单元测试的例子和在线文档很容易移植，这非常有用。这是向 web 开发人员能够轻松转移他们的技能来创建跨平台移动应用的愿景迈出的一大步。

然而，为了“测试编写”阶段的易用性，我在设置基础设施和环境时付出了代价，以便各种工具与我的 React 本机生态系统兼容。

此外，从这个领域遇到的 Github 问题来看，React 原生版本之间似乎有许多小的不稳定性，这使得很难找到像我上面描述的基础设施问题的潜在原因。但我认为，在这样一个快速发展的空间里，我们不可能没有一些挑战而保持灵活性。

[这里的](https://github.com/SamOllason/jest-enzyme-config-for-react-native/blob/master/README.md)是我的 jest-enzyme 设置的回购和一些示例测试。

我希望你觉得这很有趣并且有用！请随时在下面添加任何问题或评论。