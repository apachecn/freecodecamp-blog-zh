# 如何使用 Webpack 和 Babel 从头开始设置和部署 React 应用程序

> 原文：<https://www.freecodecamp.org/news/how-to-set-up-deploy-your-react-app-from-scratch-using-webpack-and-babel-a669891033d4/>

你已经使用创建-反应-应用程序(又名 CRA)有一段时间了。这很棒，你可以直接编码。但是什么时候需要从 create-react-app 中弹出，开始配置自己的 react 应用呢？总有一天，我们不得不放下安全检查，开始独自冒险外出。

本指南将涵盖我个人在几乎所有 React 项目中使用的最简单的 React 配置。在本教程结束时，我们将有自己的个人样板文件，并从中学习一些配置。

#### 目录

*   为什么要创建自己的配置？
*   配置 webpack 4
*   配置巴别塔 7
*   添加更漂亮
*   添加源映射以获得更好的错误日志
*   设置 ESLint
*   我发现了错误！我该怎么办？
*   添加 CSS LESS 处理器
*   将 React 应用部署到 Netlify
*   结论

### 为什么要创建自己的配置？

创建自己的 React 配置有一定的道理。你可能很擅长 React，你想学习如何自己使用 webpack 和 Babel 等工具。这些构建工具非常强大，如果您有多余的时间，了解一下它们总是有好处的。

开发人员天生好奇，所以如果你想知道事情是如何工作的，哪个部分做什么，那么让我来帮你。

此外，通过 create-react-app 隐藏 react 配置意味着开发者开始学习 React，因为[配置不应该成为入门](https://youtu.be/G39lKaONAlA?t=873)的障碍。但是当事情变得严重时，你当然需要更多的工具来集成到你的项目中。思考一下:

*   为 less，sass 添加 webpack 加载程序
*   进行服务器端渲染
*   使用新的 ES 版本
*   添加 MobX 和 Redux
*   仅仅为了学习而进行自己的配置

如果你看看互联网，有一些绕过 CRA 限制的方法，比如[创建-反应-应用程序重新连线](https://github.com/timarney/react-app-rewired)。但是说真的，为什么不自己学习 React 配置呢？我会帮助你到达那里。循序渐进。

既然您已经确信要学习一些配置，那么让我们从头开始初始化一个 React 项目。

打开命令行或 Git bash 并创建一个新目录

```
mkdir react-config-tutorial && cd react-config-tutorial
```

通过运行以下命令初始化 NPM 项目:

```
npm init -y
```

现在安装 react

```
npm install react react-dom
```

另外，你可以在 GitHub 上查看[源代码](https://github.com/nsebhastian/my-react-boilerplate)，同时阅读本教程了解关于设置的解释。

### 配置 webpack 4

我们的第一站是网络包。它是一个非常流行和强大的工具，不仅用于配置 React，而且用于几乎所有的前端项目。webpack 的核心功能是获取我们在项目中编写的一堆 JavaScript 文件，并将它们转换成一个单一的、缩小的文件，这样就可以快速提供服务。从 webpack 4 开始，我们根本不需要编写配置文件来使用它，但是在本教程中，我们将编写一个配置文件，以便更好地理解它。

首先，让我们做一些安装

```
npm install --save-dev webpack webpack-dev-server webpack-cli
```

这将安装:

*   **webpack 模块** —包括所有核心 webpack 功能
*   **webpack-dev-server** —当我们的文件被更改时，这个开发服务器自动重新运行 webpack
*   **webpack-cli** —从命令行启用运行 webpack

让我们通过将以下脚本添加到`package.json`来尝试运行 webpack

```
"scripts": {
 "start": "webpack-dev-server --mode development",
},
```

现在在根项目中创建一个`index.html`文件，内容如下:

```
<!DOCTYPE html>
<html>
 <head>
 <title>My React Configuration Setup</title>
 </head>
 <body>
 <div id="root"></div>
 <script src="./dist/bundle.js"></script>
 </body>
</html>
```

创建一个名为`src`的新目录，并在其中创建一个新的`index.js`文件

```
mkdir src && cd src && touch index.js
```

然后将一个 React 组件写入文件:

```
import React from "react";
import ReactDOM from "react-dom";
class Welcome extends React.Component {
  render() {
    return <h1>Hello World from React boilerplate</h1>;
  }
}
ReactDOM.render(<Welcome />, document.getElementById("root"));
```

使用`npm run start` …运行 webpack，将会触发一个错误。

```
You may need an appropriate loader to handle this file type
```

### 配置巴别塔 7

我们上面写的 React 组件使用了`class`语法，这是 ES6 的一个特性。Webpack 需要 Babel 将 ES6 处理成 ES5 语法，这样这个类才能工作。

让我们把巴别塔安装到我们的项目中

```
npm install --save-dev @babel/core @babel/preset-env \@babel/preset-react babel-loader
```

我们为什么需要这些包？

*   **@babel/core** 是包含 babel 转换脚本的主要依赖项。
*   **@babel/preset-env** 是默认的 babel 预设，用于将 ES6+转换为有效的 ES5 代码。可以选择自动配置浏览器聚合填充。
*   **@babel/preset-react** 用于将 JSX 和 react 类语法转换成有效的 JavaScript 代码。
*   babel-loader 是一个 webpack 加载器，它将 babel 与 webpack 挂钩。我们将用这个包从 webpack 运行 Babel。

为了将 Babel 与我们的 webpack 挂钩，我们需要创建一个 webpack 配置文件。让我们写一个`webpack.config.js`文件:

```
module.exports = {
  entry: './src/index.js',
  output: {
    path: __dirname + '/dist',
    publicPath: '/',
    filename: 'bundle.js'
  },
  devServer: {
    contentBase: './dist',
  },
  module: {
    rules: [
    {
      test: /\.(js|jsx)$/,
      exclude: /node_modules/,
      use: ['babel-loader']
    }
    ]
  },
};
```

这个 webpack 配置基本上是说我们的应用程序的`entry`点来自 index.js，所以提取该文件需要的所有内容，然后将绑定过程的`output`放入 *dist* 目录，命名为 *bundle.js* 。哦，如果我们运行在`webpack-dev-server`上，那么告诉服务器提供来自`contentBase`配置的内容，这是这个配置所在的目录。为了所有人。js 或者。jsx 文件，使用`babel-loader`来传输所有的文件。

为了使用 Babel 预设，创建一个新的`.babelrc`文件

```
touch .babelrc
```

写下以下内容:

```
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}
```

现在再次运行`npm run start`。这次会成功的。

### 添加更漂亮

为了进一步加速开发，让我们使用更漂亮的代码格式化程序。在本地安装依赖项，并使用-save-exact 参数，因为更漂亮地引入了补丁版本中的风格变化。

```
npm install --save-dev --save-exact prettier
```

现在我们需要编写`.prettierrc`配置文件:

```
{
 "semi": true,
 "singleQuote": true,
 "trailingComma": "es5"
}
```

这些规则意味着我们要在每条语句的末尾添加分号，在适当的时候使用单引号，并在多行 ES5 代码(如对象或数组)后面加上逗号。

您可以使用以下命令从命令行运行得更好:

```
npx prettier --write "src/**/*.js"
```

或者向我们的`package.json`文件添加一个新脚本:

```
"scripts": {
 "test": "echo \"Error: no test specified\" && exit 1",
 "start": "webpack-dev-server --mode development",
 "format": "prettier --write \"src/**/*.js\""
},
```

现在我们可以使用`npm run format`运行得更漂亮。

此外，如果您使用 VSCode 进行开发，您可以安装[更漂亮的扩展](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)，并通过添加以下设置在每次保存更改时运行它:

```
"editor.formatOnSave": true
```

### 添加源映射以获得更好的错误日志

因为 webpack 捆绑了代码，所以源代码映射是必需的，以便获得对引发错误的原始文件的引用。例如，如果您将三个源文件(`a.js`、`b.js`和`c.js`)捆绑成一个包(`bundler.js`，并且其中一个源文件包含错误，那么堆栈跟踪将简单地指向`bundle.js`。这是有问题的，因为您可能想确切地知道是 a、b 还是 c 文件导致了错误。

您可以告诉 webpack 使用配置的 devtool 属性生成源映射:

```
module.exports = {
  devtool: 'inline-source-map',
  // … the rest of the config
};
```

虽然这会导致构建速度变慢，但对生产没有影响。只有当你打开浏览器 DevTools 时，源地图才会被下载[。](https://stackoverflow.com/a/44316255)

### 设置 ESLint

Linter 是一个程序，它检查我们的代码是否有任何可能导致 bug 的错误或警告。JavaScript 的 linter，ESLint，是一个非常灵活的林挺程序，可以通过多种方式进行配置。

但是在我们开始之前，让我们将 ESLint 安装到我们的项目中:

```
npm --save-dev install eslint eslint-loader babel-eslint eslint-config-react eslint-plugin-react
```

*   eslint 是所有功能的核心依赖，而 eslint-loader 使我们能够将 eslint 与 webpack 挂钩。现在，由于 React 使用了 ES6+语法，我们将添加**babel-eslint**——一个解析器，使 eslint 能够过滤所有有效的 ES6+代码。
*   **eslint-config-react** 和 **eslint-plugin-react** 都是用来让 eslint 使用预先制定的规则。

因为我们已经有了 webpack，所以我们只需要稍微修改一下配置:

```
module.exports = {
  // modify the module
  module: {
    rules: [{
      test: /\.(js|jsx)$/,
      exclude: /node_modules/,
      use: ['babel-loader', 'eslint-loader'] // include eslint-loader
    }]
  },
};
```

然后创建一个名为`.eslintrc`的 eslint 配置文件，内容如下:

```
{
  "parser": "babel-eslint",
  "extends": "react",
  "env": {
    "browser": true,
    "node": true
  },
  "settings": {
    "react": {
      "version": "detect"
    }
  }
}
```

这个配置基本上是在说，*“嘿，ESLint，在你检查代码之前，请使用`babel-eslint`解析代码，当你检查它时，请检查我们的 React 规则配置中的所有规则是否都通过了。从浏览器和节点的环境中获取全局变量。哦，如果是 React 代码，从模块本身获取版本。这样，用户就不必手动指定版本。*

我们没有手动指定自己的规则，而是简单地扩展了由`eslint-config-react`和`eslint-plugin-react`提供的`react`规则。

### 我发现了错误！我该怎么办？

不幸的是，真正找出如何修复 ESLint 错误的唯一方法是查看[规则](https://eslint.org/docs/rules/)的文档。使用`eslint--fix`有一种快速修复 ESLint 错误的方法，实际上这是一种快速修复的好方法。让我们在我们的`package.json`文件中添加一个脚本:

```
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "start": "webpack-dev-server --mode development",
  "format": "prettier --write \"src/**/*.js\"",
  "eslint-fix": “eslint --fix \"src/**/*.js\"", // the eslint script
  "build": "webpack --mode production"
},
```

然后用`npm run eslint-fix`运行。如果你现在对 ESLint 还不清楚，不要担心。在使用 ESLint 的过程中，你会学到更多的东西。

### 添加 CSS LESS 处理器

为了将 less 处理器添加到 React 应用程序中，我们需要 webpack 中的 LESS 和 loader 包:

```
npm install --save-dev less less-loader css-loader style-loader
```

`less-loader`将把我们的 less 文件编译成 css，而`css-loader`将像`import`或`url()`一样解析 css 语法。`style-loader`将获取我们编译的 css，并将其加载到我们包中的`<sty` le >标签中。这对于开发来说非常好，因为它让我们可以动态地更新我们的样式，而不需要刷新浏览器。

现在让我们添加一些 css 文件在`src/style`中创建一个新的样式目录

```
cd src && mkdir style && touch header.less && touch main.less
```

`header.less`内容:

```
.header {
  background-color: #3d3d;
}
```

`main.less`内容:

```
@import "header.less";
@color: #f5adad;
body {
  background-color: @color;
}
```

现在从`index.js`导入我们的`main.less`文件:

```
import "./style/main.less";
```

然后更新我们的 webpack 配置`module`属性:

```
module: {
  rules: [{
    test: /\.(js|jsx)$/,
    exclude: /node_modules/,
    use: ['babel-loader', 'eslint-loader']
  },
  {
    test: /\.less$/,
    use: [
      'style-loader',
      'css-loader',
      'less-loader',
    ],
  },
 ]
},
```

运行启动脚本，我们就可以开始了！

### 将 React 应用部署到 Netlify

最后一步需要部署所有应用程序，对于 React 应用程序，部署非常容易。

首先，让我们在 Webpack 配置中将构建输出和开发`contentBase`从`dist`更改为`build`。

```
module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'build'), // change this
    publicPath: '/',
    filename: 'bundle.js'
  },
  devServer: {
    contentBase: "./build",
  },
//…
```

现在让我们安装一个名为 HtmlWebpackPlugin 的新的 Webpack 插件

```
npm install html-webpack-plugin -D
```

这个插件将在 Webpack 创建我们的`bundle.js`的同一个目录下生成`index.html`文件。在这种情况下，`build`目录。

为什么我们需要这个插件？因为 Netlify 要求将单个目录作为根目录，所以我们不能在使用 Netlify 的根目录中使用`index.html`。您需要更新您的 webpack 配置，如下所示:

```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
module.exports = {
  entry: //…
  output: {
    //…
  },
  devServer: {
    contentBase: "./build",
  },
  module: {
    //…
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.resolve('./index.html'),
    }),
  ]
};
```

请从您的`index.html`上取下`script`标签:

```
<!DOCTYPE html><html>  <head>    <title>My React Configuration Setup</title>  </head>  <body>    <div id="root"></div>  </body></html><!DOCTYPE html>
<html>
  <head>
    <title>My React Configuration Setup</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

现在你可以用`npm run build`命令测试配置。完成后，将样板文件放入 GitHub repo。是时候部署我们的应用程序了！

现在让我们注册一个 [Netlify](https://netlify.com) 账户。如果你以前没有听说过 Netlify，它是一个非常棒的静态网站托管，免费提供你部署静态网站所需的所有工具。什么是静态站点？这是一个由静态 HTML 页面集合创建的网站，没有任何后台。我们的 React 样板文件现在算作一个静态站点，因为我们没有配置后端，它只有 HTML 和 JavaScript。

注册后，从 Git 中选择新网站，并选择 GitHub 作为您的 Git 供应商:

![WfWqORsZjfHKwOpZ63d-nZUC6N2FF9CPzDrg](img/275c62f16ced024869c062db85455095.png)

您需要授予 Netlify 权限，然后选择您的反应样板报告。

![MtKFlYYRZVZ7JNcmP8AHiiDV5OLlJoy4hBk5](img/8e006569951cf0805f920ee18c6c12fd.png)

现在您需要输入构建命令和发布目录。如你所见，这就是我们需要 *HtmlWebpackPlugin* 的原因，因为我们只需要从一个目录中提供一切。我们不需要手动更新我们的根文件`index.html`,而是使用插件生成它。

![aecd4gyxtTE22EuPoHzXRe1yA9I9BqTaPfa1](img/5cf76f3be64d99445089f27209f61e8b.png)

请确保您的命令与上面的截图相同，否则您的应用程序可能无法运行。

![T3GN2LRCZtTIfNNOSVPV-tKgrmlllnRVcmcs](img/8a4b601ae16194ca425c0fdd9722cd01.png)

一旦部署状态变为`published`(上面的 2 号)，您就可以转到 Netlify 为您的应用程序分配的随机站点名称(1 号)。

React 应用程序已经部署。厉害！

### 结论

您刚刚创建了自己的 React 项目样板文件，并将其部署到 Netlify 中。恭喜你！当然，我没有深入研究 webpack 的配置，因为这个样板文件是一个通用的入门。在某些情况下，我们需要像服务器端渲染这样的高级功能，我们需要再次调整配置。

但是放松！你已经走到这一步了，这意味着你已经明白了 webpack，Babel，Prettier 和 ESLint 是做什么的。Webpack 有许多功能强大的加载器，可以帮助您处理在构建 web 应用程序时经常遇到的许多情况。

另外，我最近正在写一本书来帮助软件开发人员学习 React，所以你可能想[看看](https://sebhastian.com/react-distilled/)！

![3ZrRhRwWyXnRsvrtUyYmir0KVQEteEd5yi3G](img/06780a7ee860e21c6faf26bf9b0add2f.png)

你可以在 sebhastian.com 阅读更多我的 React 教程。