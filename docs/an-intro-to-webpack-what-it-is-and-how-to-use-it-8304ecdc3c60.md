# Webpack 简介:它是什么以及如何使用它

> 原文：<https://www.freecodecamp.org/news/an-intro-to-webpack-what-it-is-and-how-to-use-it-8304ecdc3c60/>

阿什什·南丹·辛格

### 介绍

好的，我假设你听说过 web pack——这就是你来这里的原因，对吗？真正的问题是你对它了解多少？你可能知道一些关于它的事情，比如它是如何工作的，或者你可能完全不知道。不管怎样，我可以向你保证，读完这篇文章后，你可能会对整个 webpack 的情况感到足够舒服。

毕竟— **必要性**是**发明之母……**

一个完美的方式来说明为什么 webpack 存在是上面的引用。但是为了更好地理解它，我们需要回到，很久以前，当 JavaScript 不是一个新的性感的东西的时候，在那些旧时代，当一个网站只是一个小小的旧的好东西的时候。 html，CSS，某些情况下可能还有一个或几个 JavaScript 文件。但是很快这一切都将改变。

#### 出了什么问题？

围绕使用和构建 javascript/web 应用程序，整个开发社区都在不断寻求改善用户和开发人员的整体体验。因此，我们看到了很多新的**库和框架**的引入。

一些**设计模式**也随着时间的推移而发展，为开发人员提供了一种更好、更强大但非常简单的方式来编写复杂的 JavaScript 应用程序。以前的网站不再只是一个包含奇数个文件的小包裹。他们表示，随着 **JavaScript 模块**的引入，编写封装的小块代码成为新趋势，变得越来越庞大。最终，所有这一切导致我们在整个应用程序包中拥有 4 到 5 倍的文件。

**不仅应用程序的整体规模是一个挑战，**而且开发人员编写的代码类型和浏览器能够理解的代码类型之间存在巨大差距。开发人员不得不使用大量名为 **polyfills** 的辅助代码来确保浏览器能够解释他们包中的代码。

为了解决这些问题，webpack 应运而生。Webpack 是一个静态模块捆绑器。

#### 那么 Webpack 是怎样的答案呢？

简而言之，Webpack 检查你的包并创建一个所谓的**依赖图**，它由各种**模块**组成，你的 webapp 需要这些模块来实现预期的功能。然后，根据这个图，它创建一个新的包，这个包由最少数量的所需文件组成，通常只有一个 bundle.js 文件，这个文件可以很容易地插入到 html 文件中，供应用程序使用。

在本文的下一部分，我将带您一步一步地安装 webpack。在它结束的时候，我希望你能理解 Webpack 的基础知识。所以让我们开始吧…

### 我们在建造什么？

你可能听说过 ReactJS。如果你了解 reactJS，你很可能知道 **create-react-app** 是什么。对于那些不知道这两个东西是什么的人来说， **reactJS 是一个 UI 库**，它对构建智能复杂 UI 非常有帮助， **create-react-app 是一个 CLI 工具**，用于设置或引导一个样板开发设置来创建 React 应用程序。

今天，我们将创建一个简单的 React 应用程序，但不使用 create-react-app CLI。我希望这对你来说足够有趣。:)

### 安装阶段

#### npm 内部

没错，几乎所有的好事都是这样开始的:普通的 npm init。我将使用 VS 代码，但是可以随意使用你喜欢的任何代码编辑器来开始。

但是，在你做这些之前，要确保你已经在你的机器上安装了最新的 [nodeJS](https://nodejs.org/en/download/) 和 [npm](https://www.npmjs.com/get-npm) 版本。单击其中的每个链接，了解有关该流程的更多信息。

```
$ npm init
```

这将为我们创建一个启动包并添加一个 package.json 文件。这里将提到构建这个应用程序所需的所有依赖项。

现在，为了创建一个简单的 React 应用程序，我们需要两个主要的库:React 和 ReactDOM。因此，让我们使用 npm 将它们作为依赖项添加到我们的应用程序中。

```
$ npm i react react-dom --save
```

接下来，我们需要添加 webpack，这样我们就可以将我们的应用程序捆绑在一起。不仅是 bundle，我们还需要热重装，这可以使用 webpack dev 服务器来实现。

```
$ npm i webpack webpack-dev-server webpack-cli --save--dev
```

`--save--dev`是指定这些模块只是开发依赖。现在，由于我们正在使用 React，我们必须记住 React 使用 ES6 类和导入语句，这可能不是所有的浏览器都能理解的。为了确保代码对所有浏览器都是可读的，我们需要一个像 babel 这样的工具来把我们的代码转换成浏览器正常可读的代码。

```
$ npm i babel-core babel-loader @babel/preset-react     @babel/preset-env html-webpack-plugin --save-dev
```

我能说什么呢，这是我承诺的最大安装数量。在 babel 的情况下，我们首先加载了核心 babel 库，然后是加载器，最后是 2 个插件或预设，专门用于 React 和所有新的 ES2015 和 ES6 及以后的代码。

接下来，让我们添加一些代码，并开始 webpack 配置。

到目前为止，所有安装完成后，package.json 文件应该是这样的。根据您阅读本文的时间，您可能会有不同的版本号。

### 代码

让我们从在应用程序结构的根中添加一个 **webpack.config.js** 文件开始。在 webpack.config 文件中添加以下代码。

```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  //This property defines where the application starts
  entry:'./src/index.js',

  //This property defines the file path and the file name which will be used for deploying the bundled file
  output:{
    path: path.join(__dirname, '/dist'),
    filename: 'bundle.js'
  },

  //Setup loaders
  module: {
    rules: [
      {
        test: /\.js$/, 
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      }
    ]
  },

  // Setup plugin to use a HTML file for serving bundled js files
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ]
}
```

好的，让我们来理解上面的线。

首先，我们要求默认路径模块访问文件位置，并对文件位置进行更改。

接下来，我们需要 HTMLWebpackPlugin 生成一个 HTML 文件，用于提供绑定的 JavaScript 文件。点击链接阅读更多关于 [HTMLWebpackPlugin](https://github.com/jantimon/html-webpack-plugin) 的信息。

然后我们有了包含一些属性的 export.module 对象。第一个是**条目属性，**，用于指定 webpack 应该从哪个文件开始创建内部依赖图。

```
module.exports = {
  entry:'./src/index.js'
}
```

接下来是输出属性，指定应该在哪里生成捆绑文件以及捆绑文件的名称。这是由**输出路径**和**输出文件名**属性完成的。

```
module.exports = {
  //This property defines the file path and the file name which will be used for deploying the bundled file
  output:{
    path: path.join(__dirname, '/dist'),
    filename: 'bundle.js'
  },
}
```

接下来是装载机。这是为了指定 webpack 应该为特定类型的文件做些什么。请记住，webpack 开箱即用只理解 JavaScript 和 JSON，但是如果您的项目使用任何其他语言，这将是指定如何使用新语言的地方。

```
module.exports = {
  //Setup loaders
  module: {
    rules: [
      {
        test: /\.js$/, 
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      }
    ]
  }
}
```

应该在对象中为每个模块属性指定信息，该对象还有一系列规则。每个案例都会有一个对象。我还指定排除 node_modules 文件夹中的所有内容。

接下来是插件属性。这用于扩展 webpack 的功能。在插件可以在模块导出对象内部的插件数组中使用之前，我们需要同样的。

```
module.exports = {
  // Setup plugin to use a HTML file for serving bundled js files
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html'
    })
  ]
}
```

如前所述，这个特定的插件将使用我们的 src 文件夹中的指定文件。然后，它将使用它作为我们的 HTML 文件的模板，所有捆绑的文件将被自动注入。我们可以使用许多其他现成的插件——查看官方页面了解更多信息。

我们需要做的最后一件事是创建一个. babelrc 文件，以使用我们安装的 babel 预置，并在我们的代码中处理 ES6 类和导入语句。将下列代码行添加到。babelrc 文件。

```
{
  "presets": ["env", "react"]
}
```

就这样，现在巴别塔将能够使用这些预置。好了，设置到此为止——让我们添加一些 React 代码来看看这是如何工作的。

### 反应代码

因为应用程序的起点是 src 文件夹中的 index.js 文件，所以让我们从它开始。我们将首先要求 **React** 和 **ReactDOM** 在本例中使用。在 index.js 文件中添加以下代码。

```
import React from 'react';
import ReactDOM from 'react-dom';
import App from './Components/App';

ReactDOM.render(<App />, document.getElementById('app'));
```

因此，我们只需从您将创建的 components 文件夹中导入另一个文件，并在名为 App.js 的文件夹中添加另一个文件，让我们看看 App.js 文件中有什么:

```
import React, { Component } from 'react'

class App extends Component {
  render() {
    return (
      <div>
        <h1>Webpack + React setup</h1>
      </div>
    )
  }
}

export default App;
```

我们差不多完成了。现在唯一剩下的就是启用热重装了。这意味着每次检测到更改时，浏览器会自动重新加载页面，并能够在时机成熟时构建和捆绑整个应用程序。

我们可以通过在 package.json 文件中添加脚本值来实现这一点。删除 package.json 文件的 scripts 对象中的 test 属性，并添加以下两个脚本:

```
"start": "webpack-dev-server --mode development --open --hot",
"build": "webpack --mode production"
```

你都准备好了！转到您的终端，导航到根文件夹，并运行 **npm start。它应该在您的计算机上启动一个开发服务器，并在您的浏览器中提供 HTML 文件。如果您进行任何小/大的更改并保存代码，您的浏览器将自动刷新以显示最新的更改。**

一旦你认为你已经准备好捆绑应用程序，你只需要点击命令， **npm build，**，webpack 将在你的项目文件夹中创建一个优化的捆绑包，可以部署在任何 web 服务器上。

### 结论

这只是 webpack 和 babel 的一个小应用或用例，但应用是无限的。我希望你有足够的兴奋去探索更多的选项和用 webpack 和 babel 做事的方法。请参考他们的官网了解更多，深入阅读。

我已经创建了一个 Github repo，里面有所有的代码，所以如果有任何问题，请参考它。

[**ashishcodes 4/web pack-react-setup**](https://github.com/ashishcodes4/webpack-react-setup)
[*不使用 CLI 从头开始设置 react 应用程序*](https://github.com/ashishcodes4/webpack-react-setup)

我对 webpack 的看法？嗯，有时你可能会认为它只不过是一个工具，为什么你要为一个工具而烦恼呢？但是请相信我:在学习 webpack 的过程中最初的挣扎将会为你节省大量的时间，否则你将会在没有 webpack 的情况下投资开发。

这就是现在的全部，希望很快能带来另一篇有趣的文章。我希望你喜欢读这本书！

如果您在执行上述任何步骤/流程时遇到任何困难或问题，请随时联系我们并留下您的意见。

领英:[https://www.linkedin.com/in/ashish-nandan-singh-490987130/](https://www.linkedin.com/in/ashish-nandan-singh-490987130/)

推特:[https://twitter.com/ashishnandansin](https://twitter.com/ashishnandansin)