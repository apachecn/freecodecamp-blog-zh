# 如何使用 Webpack 开始使用 Vue 单文件组件

> 原文：<https://www.freecodecamp.org/news/getting-started-with-vue-single-file-components-using-webpack-2ae078058688/>

作者:Dushyant Sabharwal

# 如何使用 Webpack 开始使用 Vue 单文件组件

![1*tag8gBfSQ9I4dxLJOAg3QQ@2x](img/93926215d7ad61ece3934faae9f9fd4e.png)

Picture from my trip to Iceland in May, 2018

*本指南假设你对`vue`有所了解。它旨在节省您的时间，试图帮助您理解从`vue`开始的`webpack`配置及其单个文件组件。您可以使用`vue-cli`来创建项目模板，但这是为那些想深入研究的人准备的。*

你可能是一个了解前端的开发人员。你已经决定通过采用`vue`作为前端框架来让你的应用程序更上一层楼。你跳到文档中，开始阅读如何开发组件，同时在你的头脑中画出项目中第一个组件的用例。框架和文档非常棒，你迫不及待地想开始使用`vue`。

如果这听起来很熟悉，那太好了！

TL；DR 您可以在这里克隆或派生存储库[并开始。](https://github.com/dushyant89/vue-webpack)

### 我们开始吧

我们的目标是编写我们的第一个组件，但是**不是在**下面完成的方式。虽然像这样加载脚本文件没有任何问题，但是当您以这种方式加载多个脚本文件时，就会变得更加混乱。

我们将使用`[webpack](https://webpack.js.org/configuration/devtool/)`捆绑我们的应用程序。如果你还没有研究过`webpack`，那么现在是时候配置你的第一个应用了。起初看起来令人生畏，但最新版本(v4)使用起来超级简单和直观。

### 安装软件包

为了达到这一点，让我们安装一些我们需要的基本软件包。我们将使用`npm`来管理软件包。如果使用`npm`不自信，也不用担心！只是跟着走。确保您已经在机器上安装了`node`和`npm`。

**注意**:如果你有时间，那么[一定要仔细阅读](https://hackernoon.com/things-which-every-developer-should-know-when-starting-with-modern-front-end-development-7030486bf092)NPM 是如何工作的，以及它对你的应用程序的安全性意味着什么。

向前移动…

```
npm install vue
```

```
npm install webpack --save-dev
```

因为我们将在`ES6`和更高版本中编写代码，我们需要一些东西来转换我们的代码。我们将使用`babel`和`webpack`来帮助我们开发出一个可以在仍然不支持`ES6`的浏览器中运行的代码版本。

这篇文章很好地概述了巴别塔，并将更详细地解释为什么我们需要下面的包。

```
npm install babel-core --save-dev
```

```
npm install babel-loader --save-dev
```

```
npm install babel-preset-env --save-dev
```

```
npm install babel-preset-stage-2 --save-dev
```

你的`package.json`应该看起来像下面这样。当你安装以下软件包时，你的版本可能会有所不同，只要应用程序没有崩溃，这是没有问题的。

如果你想安装你上面看到的特定版本，那么简单

```
npm install webpack-cli@^3.0.2 --save-dev
```

现在我们的基本工具集已经设置好了，让我们把注意力集中在如何编写第一个组件的`template`或`html`部分上。它会在一个单独的`.html`文件里吗？还是会包含一个类似`index.html`的现有文件？或者它会在一个`string`中，然后使用某个库进一步编译吗？我也经历过这种思路。

`Vue`解决了这个问题，它提供了一种编写组件的方法，在这个方法中，您可以将组件的`template`部分和`script`部分关联到一个文件中。多棒啊。

例如，如果您正在构建一个简单的`table`组件，那么您可以将该文件命名为`table.vue`，它包含了组件需要的所有内容。如果我告诉你，你可以在同一个`.vue`文件中也有`styles`，这个文件是特定于那个组件的，会怎么样？我知道！听起来很疯狂！

让我们安装下面的包，这样我们就可以有单个文件组件，或`SFCs`:

```
npm install vue-template-compiler --save-dev
```

```
npm install vue-loader --save-dev
```

```
npm install css-loader --save-dev
```

```
npm install vue-style-loader --save-dev
```

`vue-template-compiler`用于理解组件的`template`部分。

`vue-loader`使`webpack`能够加载单个文件组件。

`css-loader`和`vue-style-loader`允许我们在组件中创作样式。

您的`package.json`现在应该看起来像下面这样:

### 网络包

现在我们的武器库中已经有了我们需要的每一个包，我们所需要的就是一种指导`webpack`的方法。如果你正试图处理`webpack`以及它是如何工作的，最好首先理解为什么这个工具存在的直觉。我们是否使用`webpack`并不重要，我们只是需要一些工具，可以做这样的事情:

*   我们的应用程序中用于启动流程的流程入口点
*   命名输出/处理的文件并指定它们的位置
*   处理不同类型的文件，如`.css`、`.js`或`.vue`
*   热重装改变的文件，以重建整个事情

如果您只是通过一个 config 对象指定需要做什么，Webpack 会做所有这些事情(甚至更多)。

我们将使用`webpack-dev-server`来服务我们项目中的静态和动态资产，因为为什么不呢。

### 查看代码

让我们克隆或分叉(如果你想改进)[这个项目](https://github.com/dushyant89/vue-webpack)。

你会看到这个项目有和上面提到的一样的`package.json`。让我们根据回购协议中的说明安装并运行项目。

`index.html`的第一个组件叫做`main-content`:

```
<div id="mainContent">    <main-content></main-content></div>
```

我们的`main-content.vue`，也就是一个`SFC`，看起来如下图。如你所见，它有三个部分:`template`、`script`和`style`。一切都与我们的组件紧密相连，剩下的由`webpack`负责。

在你的浏览器中进入 [http://localhost:8010/](http://localhost:8010/) ，你会注意到我们的`main-content`组件。现在更改组件中的一些内容，如下所示:

```
<template>    <div class="main-content">        <h1> This is my first modified component in Vue </h1>        <h3> {{ webpack }} </h3>    </div></template>
```

请注意标题在浏览器中的变化。要了解它的工作原理，请看一下`webpack.config.js`。配置中的每个部分都有注释，解释我们为什么需要它。

让我们将`webpack`配置分成三个主要部分。

#### **网络包的输入/输出**

#### **处理 Vue 单个文件组件和其他 JS 模块**

#### **配置 Webpack 开发服务器**

配置中的每个选项都是不言自明的，您可以调整它们以更好地理解它们。例如，您可以删除其中一个属性并注意到错误。

**注意**:每次你改变配置的时候，你必须停止(cmd + C)并运行`npm run start`来反映改变。

你可以通过通读[文档](https://webpack.js.org/configuration/devtool/)来为应用程序添加更多选项，并随时分叉[项目](https://github.com/dushyant89/vue-webpack)进行改进。

如果你认为这篇文章对你有帮助，那么你可以[给我买杯咖啡](https://www.buymeacoffee.com/dushyant)或者只是和别人分享。干杯？

[**给杜斯扬特·萨巴瓦尔买一杯咖啡——BuyMeACoffee.com**](https://www.buymeacoffee.com/dushyant)
[*我是一名全栈开发人员，喜欢写能帮助其他开发人员节省时间的东西*www.buymeacoffee.com](https://www.buymeacoffee.com/dushyant)