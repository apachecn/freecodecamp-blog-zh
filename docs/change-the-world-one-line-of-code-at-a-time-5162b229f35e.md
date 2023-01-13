# 改变世界，一次一行代码

> 原文：<https://www.freecodecamp.org/news/change-the-world-one-line-of-code-at-a-time-5162b229f35e/>

人们喜欢把改变世界视为一项大任务。我相信改变世界可以一步一步来。

关键是发现问题，然后迈出一小步。

我的旅程开始于 2018 年 9 月 7 日**周五**。那天，我决定为 freeCodeCamp 测试套件构建一个 React 插件。我注意到了一个问题，并采取了行动。

有一个[工作版本](https://www.npmjs.com/package/react-fcctest)安装在节点包管理器注册表上。这对我来说是一个里程碑，因为这个项目是我的第一个开源贡献。

我使用了某些关键技术来构建这个项目，如 Webpack、React、NPM 和 Node.js。我在构建它的过程中得到了很多乐趣，也学到了很多东西。

我试了几次(实际上是一整天)才成功地让插件工作。

在使其工作之后，在 React 应用中实现是一个挑战。尽管我面临着技术上的困难，但最终，插件成功了。

### 该过程

这个项目背后的想法很简单。我想做的只是找到一种简单的方法来添加 freeCodeCamp 测试套件以反应应用程序。

我的第一个计划是用 Create-React-App 来构建它。

我觉得既然我可以用它来构建 React 应用程序，我也可以用它来构建一个插件。我错了。

Create-React-App 对于我需要构建的东西来说太重了。

我发现为了使插件易于导出，我需要一些额外的配置。

我上网搜索了几次，发现了 Webpack 和 react-helmet。起初，我遇到的情况既令人惊讶又令人困惑。

尽管如此，我知道他们是我所需要的。我继续寻找更多。

在 Webpack 之前，我曾经尝试过在没有额外配置的情况下将插件作为一个模块导出和发布。它不起作用。新手的错误，我知道。

这是一个我必须克服的巨大挑战。

谢天谢地，我们在成长中学习！

当我开发这个插件的时候，经常会断电。在尼日利亚，电力形势并不稳定。

我不得不工作到我的笔记本电脑断电，然后深入思考当电源恢复时该做什么。

这一切都发生在第二天(周六)。

### 神奇，美丽

使用 Webpack，我开始构建插件。

我将核心代码放在 index.js 文件中。下面是代码:

```
import React from 'react';
import { Helmet } from 'react-helmet';
import './styles.css';

const ReactFCCtest = () => {
  return (
    <div>
      <Helmet>
        <script type="text/javascript" 
                src="https://cdn.freecodecamp.org/testable-projects-fcc/v1/bundle.js" >
        </script>
      </Helmet>
      <h5>react-fcctest running</h5>
    </div>
  );
};

export default ReactFCCtest;
```

上面的代码是我将脚本添加到任何我想要的 React 应用程序的 head 标签所需要的全部内容。

我偶然看到一篇关于媒体的文章，这对我帮助很大。

它帮助我理解了如何使用 Webpack 创建一个节点模块，我可以成功地将它发布到节点包管理器注册中心。

我遵循了那篇文章中的说明。在做了一些修改后，我构建了下面的 **webpack.config.js** 文件:

```
const path = require('path');
const HtmlWebpackPlugin = require("html-webpack-plugin");
const htmlWebpackPlugin = new HtmlWebpackPlugin({
    template: path.join(__dirname, "demo/src/index.html"),
    filename: "./index.html"
});
module.exports = {
    entry: path.join(__dirname, "demo/src/index.js"),
    output: {
        path: path.join(__dirname, "demo/dist"),
        filename: "bundle.js"
    },
    module: {
        rules: [
            {
                test: /\.(js|jsx)$/,
                use: "babel-loader",
                exclude: /node_modules/
            },
            {
                test: /\.css$/,
                use: ["style-loader", "css-loader"]
            }
        ]
    },
    plugins: [htmlWebpackPlugin],
    resolve: {
        extensions: [".js", ".jsx"]
    },
    devServer: {
        port: 3001
    }
};
```

让我解释一下这个文件的作用:

>>首先，它使用 HtmlWebpackPlugin 创建一个 HTML 文件来服务我的 webpack 包。

>>接下来它将导出我创建的插件作为节点模块。

>>是说我插件的入口点在位置`demo/src/index.js`。这意味着这是将要导出的代码的来源。

>>接下来就是说我插件的输出目录是`demo/dist`。在这个目录**中，react-fcctest** 插件将被导出到一个名为`bundle.js`的文件中。

>>接下来，它为要导出的文件引入了一组规则。

>>规则，告诉文件做两件事。第一，在处理`.js`和`.jsx`文件时使用 babel-loader，不要包含`node_modules`文件夹。第二，使用样式加载器和 css 加载器处理`.css`文件。

>>文件的解析和扩展部分允许我在导入文件时从文件的末尾删除`.js`和`.jsx`。

>>最后，我的开发服务器位于端口 3001 上。这个港口可以是我选择的任何其他港口。

> 我只是注意到美丽需要努力工作…

我在周日把 Webpack 添加到项目中，然后插件就工作了！

这样，我就能够创建一个可以轻松导出的模块。这个模块是**reactfctest**。

我不能说**阅读-搜索-提问**方法在整个项目中对我有多大帮助。

这里是成品插件的演示。建造它非常有趣。

我在一个 freeCodeCamp 项目中测试了它，它运行得非常好。

![1*OL4Q9xvDLtsMcgY21--tOQ](img/6982f7f4f3595abf8b44a79be081269b.png)

Credit: [https://giphy.com](https://giphy.com/)

我创建了一个 [Github 存储库](https://github.com/Usheninte/react-fcctest)，其中保存了该项目的所有开源代码。

### **如何安装和使用“react-FCC test”**

运行`npm i react-fcctest`或`yarn add react-fcctest`安装 React 插件。

将`import ReactFCCtest from 'react-fcctest';`放入您的 App.js:

```
import React, { Component } from 'react';
import ReactFCCtest from 'react-fcctest';

class App extends Component {
  render() {
    return (
      <div>
        <ReactFCCtest />
      </div>
    );
  }
};

export default App;
```

这就是全部了！

#### 最终注释

到目前为止，我的 2018 年很棒。

我现在是我所在大学的开发者学生俱乐部的负责人，在一个由撒哈拉以南非洲的谷歌开发者发起的项目中。

我的目标是伟大，在外太空——也许我会登上月球。跟随我踏上我的旅程。