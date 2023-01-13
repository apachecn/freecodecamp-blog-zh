# webpack 2 中的树摇动 ES6 模块

> 原文：<https://www.freecodecamp.org/news/tree-shaking-es6-modules-in-webpack-2-1add6672f31b/>

杰克·威斯勒

# webpack 2 中的树摇动 ES6 模块

![sNyvMPcIpE8JJsOx32rbN0DtzmKgFLKCZv2M](img/113cda8e6fc59ba1c707788566bda47b.png)

Webpack 2 上周刚刚从测试版发布。它带来了各种预期的特性，包括对 ES6 模块的本地支持。

webpack 2 不使用`var module = require('module')`语法，而是支持 ES6 `imports`和`exports`。这为像**树摇动**这样的代码优化打开了大门。

### 什么是摇树？

由 Rich Harris 的 [Rollup.js](http://rollupjs.org/) module bundler、 *tree-shaking* 推广的是只包含正在被*使用的代码的能力。*

当我第一次使用 Rollup 时，我惊讶于它与 ES6 模块的良好配合。开发体验就是觉得…对。我可以创建用“未来 JavaScript”编写的独立模块，然后将它们包含在我的代码中的任何地方。任何未使用的代码都不会进入我的包。天才！

#### 它解决什么问题？

如果你在 2017 年编写 JavaScript，并且*了解*(参见: [JavaScript 疲劳](https://hackernoon.com/how-it-feels-to-learn-javascript-in-2016-d3a717dd577f#.nk6chuvta))周围的各种工具，你的开发体验可能会感觉相当流畅。这很重要，但同样重要的是*用户体验*。这些现代工具中有很多最终导致 web 应用程序膨胀，包含大量 JavaScript 文件，导致性能下降。

我喜欢 Rollup 的一点是，它尝试解决了这个问题，并将一个解决方案带到了 JavaScript 社区的前沿。现在，像 webpack 这样的大公司正试图对其进行迭代。

摇树可能不是*“终结所有解决方案的解决方案”*但它是更大的馅饼中的重要一块。

#### 简单的例子

在你开始之前，我想给你提供一个摇树的小例子。您的应用程序由两个文件组成，`index.js`和`module.js`。

在`module.js`中，您导出了两个命名的箭头函数:

```
// module.js 

export const sayHello = name => `Hello ${name}!`; 

export const sayBye = name => `Bye ${name}!`
```

只有`sayHello`被导入到`index.js`文件中:

```
// index.js 

import { sayHello } from './module'; 

sayHello('World');
```

`sayBye`出口，但*从未*进口。任何地方。因此，由于树抖动，它不会包含在您的捆绑包中:

```
// bundle.js 

const sayHello = name => `Hello ${name}!`; 

sayHello('World');
```

根据所使用的捆绑器，上面的输出文件可能会有所不同。这只是一个简化的版本，但你明白了。

最近我读了罗曼·柳蒂科夫写的一篇文章，他做了一个很好的类比来形象地描述摇动树木的概念:

> *“如果你想知道为什么叫摇树:把你的应用想象成一个依赖图，这是一棵树，每个导出都是一个分支。所以你摇一摇树，枯枝就会掉下来。”—罗曼·柳蒂科夫*

### webpack 2 中的树摇动

不幸的是，对于我们这些使用 webpack 的人来说，摇树是“在开关后面”，如果你愿意的话。与 Rollup 不同，在获得您所寻找的功能之前，需要完成一些配置。“开关后面”的部分可能会让一些人感到困惑。我会解释的。

#### 步骤 1:项目设置

我将假设您了解 webpack 的基础知识，并且能够找到一个基本的 webpack 配置文件。

让我们从创建一个新目录开始:

```
mkdir webpack-tree-shaking && cd webpack-tree-shaking
```

一旦进入，让我们初始化一个新的`npm`项目:

```
npm init -y
```

`-y`选项快速生成`package.json`,不需要你回答一堆问题。

接下来，让我们安装一些项目依赖项:

```
npm i --save-dev webpack@beta html-webpack-plugin
```

上面的命令将在您的项目中本地安装 webpack 2 的最新测试版，以及一个名为`html-webpack-plugin`的有用插件。后者对于本演练的目标来说不是必需的，但会使事情变得更快一些。

**注**:在撰写本文时，webpack 团队仍然推荐使用命令`npm i --save-dev webpack@beta`。`webpack@beta`最终将被淘汰，取而代之的是`webpack`的最新命令。查看*如何下载？ [webpack 最新发布帖子](https://medium.com/webpack/webpack-2-2-the-final-release-76c3d43bf144#.soqt6oma5)的* 部分了解更多详情。

打开`package.json`并确保它们已经作为`devDependencies`安装。

#### 步骤 2:创建 JS 文件

为了看到树的晃动，你需要一些 JavaScript 来玩。在项目的根目录下，创建一个包含 2 个文件的`src`文件夹:

```
mkdir src && cd src 

touch index.js 

touch module.js
```

**注意:**`touch`命令通过终端创建一个新文件。

将下面的代码复制到正确的文件中:

```
// module.js 

export const sayHello = name => `Hello ${name}!`; 

export const sayBye = name => `Bye ${name}!`;
```

```
// index.js 

import { sayHello } from './module'; 

const element = document.createElement('h1'); 

element.innerHTML = sayHello('World'); 

document.body.appendChild(element);
```

如果您已经做到这一步，您的文件夹结构应该如下所示:

```
/ 
| - node_modules/ 
| - src/ 
|    | - index.js 
|    | - module.js 
| - package.json
```

#### 步骤 3:从 CLI 使用 Webpack

因为您没有为您的项目创建配置文件，所以现在让 webpack 做任何工作的唯一方法是通过 webpack CLI。让我们做一个快速测试。

在终端中，在项目的根目录下运行以下命令:

```
node_modules/.bin/webpack
```

运行此命令后，您应该会看到如下输出:

```
No configuration file found and no output filename configured via CLI option. A configuration file could be named 'webpack.config.js' in the current directory. Use --help to display the CLI options.
```

该命令不做任何事情，webpack CLI 证实了这一点。你没有给 webpack 任何关于你想捆绑什么文件的信息。您可以通过命令行*或配置文件*提供这些信息。让我们选择前者来测试一切是否正常:

```
node_modules/.bin/webpack src/index.js dist/bundle.js
```

您现在所做的是通过 CLI 向 webpack 传递一个`entry`文件和一个`output`文件。这个信息告诉 webpack，“去`src/index.js`，把所有必要的代码捆绑到`dist/bundle.js`。它就是这样做的。您会注意到现在有了一个包含`bundle.js`的`dist`目录。

打开看看吧。捆绑包中有一些额外的 javascript 代码，是 webpack 完成其工作所必需的，但是在文件的底部，您也应该看到自己的代码。

#### 步骤 4:创建 webpack 配置文件

Webpack 可以处理很多事情。我花了大量的空闲时间潜心研究这个捆绑器，但仍然只是皮毛。一旦您完成了琐碎的示例，最好是离开 CLI，创建一个配置文件来处理繁重的工作。

在项目的根目录下，创建一个`webpack.config.js`文件:

```
touch webpack.config.js
```

这个文件可以有多复杂就有多复杂。为了这篇文章，我们将保持轻松:

```
// webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: 'dist'
  },
  plugins: [
    new HtmlWebpackPlugin({ title: 'Tree-shaking' })
  ]
}
```

该文件为 webpack 提供了与您之前向 CLI 提供的信息相同的信息。您已经将`index.js`定义为您的`entry`文件，将`bundle.js`定义为您的`output`文件。您还添加了您的`html-webpack-plugin`，它将在您的`dist`目录中生成一个 html 文件。方便。

继续测试，以确保它仍然在工作。删除您的`dist`目录，并在命令行中键入:

```
webpack
```

如果一切顺利，你可以打开`dist/index.html`看到“Hello World！”。

**注意:**配置文件的使用给了我们输入`webpack`而不是`node_modules/.bin/webpack`的便利。小赢。

#### 巴别塔

我之前提到过 webpack 2 带来了对 ES6 模块的本地支持。这都是真的，但这并不能改变 ES6 不被所有浏览器完全支持的事实。正因为如此，你需要使用像[巴别塔](http://babeljs.io/)这样的工具*将你的 ES6 代码*转换成容易接受的 JavaScript。结合 webpack，Babel 让我们能够编写您的“未来 JavaScript ”,而不用担心不受支持的浏览器的影响。

让我们继续在您的项目中安装 Babel:

```
npm i --save-dev babel-core babel-loader babel-preset-es2015
```

注意`babel-preset-es2015`包。这个小家伙是我坐下来写这些的原因。

#### 第六步:`babel-loader`

Webpack 可以配置成通过[加载器](https://webpack.js.org/concepts/#loaders)将特定文件转换成模块。一旦它们被转换，它们就被添加到依赖图中。Webpack 使用图来解析依赖关系，并且只将需要的内容包含到最终的包中。这是 webpack 工作的基础。

您现在可以配置 webpack 使用`babel-loader`来转换您所有的`.js`文件:

```
// webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: { filename: 'bundle.js', path: 'dist' },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        options: { 
          presets: [ 
            'es2015' 
          ] 
        }
      }
    ]
  },
  plugins: [ 
    new HtmlWebpackPlugin({ title: 'Tree-shaking' }) 
  ]
};
```

属性为 webpack 提供了一组指令。它说，“获取任何以`.js`结尾的文件并使用`babel-loader`转换它们，但是不要转换任何在`node_modules`里面的文件！”

我们还将`babel-preset-es2015`包作为选项传递给`babel-loader`。这只是告诉`babel-loader`*如何转换 JavaScript。*

*再次运行`webpack`以确保一切正常。什么事？太好了！我们所做的是把你的 JavaScript 文件捆绑起来，同时把它们编译成跨浏览器支持的 JavaScript。*

### *潜在的问题*

*包`babel-preset-es2015`包含另一个名为`babel-plugin-transform-es2015-modules-commonjs`的包，它将你所有的 ES6 模块转换成`CommonJS`模块。这并不理想，原因如下。*

*webpack 和 Rollup 等 Javascript 捆绑器只能在具有静态结构的模块上执行树抖动。如果一个模块是静态的，那么 bundler 可以在构建时确定它的结构，安全地删除没有被导入到任何地方的代码。*

*模块没有静态结构。因此，webpack 将无法从最终的捆绑包中对未使用的代码进行树抖动。幸运的是，Babel 缓解了这个问题，它为开发者提供了一个选项，你可以将它和`babel-preset-es2015`一起传递给你的`presets`数组:*

```
*`options: { presets: [ [ 'es2015', { modules: false } ] ] }`*
```

*根据巴别塔的[文档](https://github.com/babel/babel/tree/master/packages/babel-preset-es2015#options):*

*`*“modules*` *-启用 ES6 模块语法到另一种模块类型的转换(默认启用“commonjs”)。可以是`false`到不转换模块，或者是`["amd", "umd", "systemjs", "commonjs"]`*之一。*

*将这些额外的代码放进你的配置中，你就可以用花生油做饭了。*

*`webpack.config.js`的最终状态是这样的:*

```
*`// webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry: './src/index.js',
  output: { filename: 'bundle.js', path: 'dist' },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        options: { 
          presets: [ 
            [ 'es2015', { modules: false } ] 
          ] 
        }
      }
    ]
  },
  plugins: [ new HtmlWebpackPlugin({ title: 'Tree-shaking' }) ]
};`*
```

### *压轴戏*

*再次运行`webpack`并弹出打开你的`bundle.js`文件。你不会注意到任何不同。在你发疯之前，知道这一点！没关系。我们一直在开发模式下运行 webpack。Webpack 知道您的代码中有未使用的导出。即使它被包含在最终的包中，`sayBye`也永远不会投入生产。*

*如果你还是不相信我，在你的终端里运行`webpack -p`。`-p`选项代表*生产*。Webpack 将执行一些额外的性能优化，包括缩小，删除任何未使用的代码。*

*打开`bundle.js`。既然缩小了，那就继续搜索`Hello`吧。它*应该*在那里。搜索`Bye`。它*不该*。*

*瞧啊。在 webpack 2 中，您现在已经有了一个树摇动的工作实现！*

*出于好奇，我一直在 GitHub Repo 中慢慢迭代我自己的轻量级 webpack 配置:*

*[**Jake-wies/webpack-hot plate**](https://github.com/jake-wies/webpack-hotplate)
[*web pack-hot plate-个人项目的 web pack 样板文件*](https://github.com/jake-wies/webpack-hotplate)
[github.com](https://github.com/jake-wies/webpack-hotplate)*

*它并不意味着过于冗长和臃肿。它的重点是成为一个平易近人的样板，每一步都要走一遍。有兴趣的话可以去看看！*

*如果您有任何问题，请随时联系 [Twitter](https://twitter.com/jakewies) ！*