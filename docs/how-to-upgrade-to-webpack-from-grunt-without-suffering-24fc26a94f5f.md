# 了解如何轻松升级到 Webpack

> 原文：<https://www.freecodecamp.org/news/how-to-upgrade-to-webpack-from-grunt-without-suffering-24fc26a94f5f/>

作者:亚赞·阿德

# 了解如何轻松升级到 Webpack

我写这篇文章是为了讲述我在将 AngularJS 项目从 Grunt 升级到 Webpack 时的经历。

![0*H9-QqXnBR8Rr6MhF](img/e26e1b2f8916e3b8f1fb57906fd5cfee.png)

Photo by [Tyler Franta](https://unsplash.com/@tfrants?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你可以在 twitter 上关注我，或者在我的网站 yaabed.com 上查看我的最新文章。另外，我在 blog.yaabed.com 媒体的[有我的出版物。](https://medium.com/yazanaabed)

存在的主要问题是大约 500 个项目被扔在窗口对象上。这允许您在任何需要的地方访问它们。它还使窗口成为模块和组件的导航工具。项目变得更加耦合，你不知道谁在使用它们。

文件使用模块架构进行组织，但不使用`angular.module.`文件像主页一样按名称划分到文件夹中。主页文件夹包含其控制器、样式和视图。

首先想到的是重构整个应用程序，以使用 webpack、modules、babel 和 es6。经过研究，不需要对代码库进行任何重构就可以做到这一点。但是，在我开始将 webpack 添加到项目中之前，还有许多问题需要解决。

### **开始工作前需要考虑的问题**

*   如何解决窗口对象问题，因为 webpack 把文件显示为互相对话的文件树。
*   如何在不合并问题的情况下减少对项目的更改。
*   如何划分 webpack 的开发和生产？
*   如何去除 bower 依赖，因为 webpack 主要从 npm 解析模块。
*   webpack 升级如何解决 JavaScript 文件大的问题？

![1*8cR1c4pTuS145b7KhVrB-Q](img/d804b41f529bb5f9f11adfc4c9bb863f.png)

[https://www.pexels.com/photo/technology-computer-desktop-source-code-113850/](https://www.pexels.com/photo/technology-computer-desktop-source-code-113850/)

### 开始把事情分成几个步骤

#### 将节点版本从 0.10 升级到可用的最新版本

在我开始使用 webpack 之前，我需要升级 webpack v3 使用的节点版本。但是，Grunt 使用的是废弃的东西——所以当我更新节点版本时，什么都没有用！所以我开始一个接一个地修复错误，以确保升级是可能的。

第一，老`grunt-sass` & `node-sass`上出现错误。此版本的 Node 不再支持它。为了解决这个问题，我将`grunt-sass`从‘0 . 18 . 1’升级到‘2 . 0 . 0’，然后将`node-sass`升级到‘4 . 7 . 2’。

其次，试图将 grunt 从‘0 . 4 . 5’升级到‘1 . 0 . 0’并没有成功，因为 grunt 插件需要 grunt@0.4.5 作为对等依赖。所以我坚持用 0.4.5 版本。

#### 修复 express 节点服务器上显示的错误

我不得不修复 express Node server 的错误，因为 bodyParser 构造函数已经过时，需要修改。我从

![1*zYHhQhSD4VfTrv8HWp7l4A](img/3495fefcd8802c3a47060e016f67c7ea.png)

到

![1*Ty4Il11Y6pwJodIZcBfdYg](img/479bd7f207f0ce79beab89bb11dc7050.png)

#### 删除不赞成使用的内容

*   来自`grunt-express`的调试属性，因为它在新版本的节点检查器中已被弃用。
*   从项目中删除 bower-install 任务。

#### 开始添加 webpack

我使用`npm install webpack--save-dev`将 webpack 添加到项目中。然后我添加了“webpack.config.json”文件。

当我开始这一步的时候，因为项目结构没有入口点而卡住了。整个项目依赖于一个来源，那就是窗户。Webpack 需要一个开始的入口点和一个结束的输出点。

为了解决这个问题，我创建了一个入口点。我在上面设置了所有需要的文件，并在 GruntJS config 上将其命名为相同的名称，以便像旧版本一样连接它。但这将需要很长时间，因为在 index.html 大约有 550 个项目。

为了解决这个问题，我使用了一个 RegExp `/”(.*?)"/ig`并用`require(src)`替换值，以从 src 属性中获取源，并将其转换为`require(src).`，我将它粘贴到`entry.js`中，粘贴顺序与旧的 index.html 相同。

之后，结果是一个包含所有脚本的重要 JS 文件。但是什么都没用！在调查了发生的事情之后，似乎 webpack 在默认情况下是作为模块工作的。如果在同一个文件上有 exports 或 export default，即使您使用 require js 包含它，也不会将任何内容导出到外部。

在寻找解决这个问题的方法之前，我开始向所有需要导出的文件添加 module.exports 在清楚了解 webpack 如何工作之前！经过两天的工作，我发现有一种叫做装载机的东西可以解决这个问题。

通过将其添加到`webpack.config.js`，所有文件现在都可以作为旧行为使用了！

![1*a1w_YDNzXTDVWfIzl5CN1g](img/db560a7f8386c1fbc98f4476f5216bbc.png)

现在一切都正常了。

#### 下一步

在我用 Grunt 完成这个项目后，我需要确保 webpack 和 Grunt 能够一起工作。所以我做了测试，以确保我没有遗漏任何东西。

为了实现这一点，我创建了一个名为`inject-HTML.files.json.`的新文件。这个文件包含了 Grunt 和 webpack 上与`usemenPrepare`一起使用的所有源文件，以从 inject-HTML 文件 JSON 中获取的数组形式创建多个条目。

![1*4CHmK7YvGR-5KdKkDb0shQ](img/dd18b67550a59910892b267411428266.png)

I love this image, write code and drink some coffee :) [https://www.pexels.com/photo/high-angle-view-of-coffee-cup-on-table-317385/](https://www.pexels.com/photo/high-angle-view-of-coffee-cup-on-table-317385/)

#### 更新旧的 Grunt 配置文件

![1*_ACtb1LBsXQulfYWnZP17g](img/4befba53815cbbd102d79c5aaa4b6570.png)

#### 将文件添加到 concat

![1*2AX4IhZxSTV2sFxd2dn8qg](img/328fd9456b4b9a53fb8fee4f29ec9d12.png)

#### 检查 Webpack 是否构建，然后从配置中删除 JS

![1*YaLaQJvEGZf1-U09ii3t0g](img/ca89b9e5bc4383255e047d9fff1fac9a.png)

#### 添加新的 npm 脚本

![1*h72Fb0X9U7Fdt1d3NQ0z-Q](img/6e3c2e696dc7fd64b65ea49b10b107ba.png)

#### Webpack.config.js 文件

![1*o7QEQxqK3HhR4_lMu0zvhA](img/e155d77dac738f1ceed7df7ffc89652b.png)

#### Webpack.prod.js 文件

![1*sZWLlMeMiXaXPdqmOYvXog](img/8e34e84349703f7590187e54afb296a3.png)

### 动机

#### 可维护性和代码质量

*   解决创建文件的问题，因为项目发展很快。
*   解决窗户上无故附着东西太多的问题。
*   让代码库更容易理解。

#### 开发效率

*   Bower 现已弃用。
*   不能在 npm 包中使用任何东西，因为构建过程不提供这个。

#### 表演

*   文件大小每天都在变大，所以需要引入一个解决方案来分割代码。
*   能够分割文件和延迟加载直到需要节省不必要的传输和解析。

#### 代码拆分

*   使用后，webpack 代码拆分将更容易使用。
*   将新功能分解成基于模块的功能。

最后，使用 npm 包是一个游戏改变者。目标是让代码库对其他开发人员来说更容易。此外，我们证明了即使你的代码库很糟糕，明智地升级你的系统也是可能的。

重写整个应用程序是一场灾难，因为你可能会浪费多年的辛勤工作。相反，努力使你的代码库更具可读性、可维护性和模块化。当旧代码需要重构时，可以一步一步来。

不要被你的旧代码库困住，说你对它无能为力。试着自己做出改变——接受让你快乐的新事物、新更新和新技术。

这是我第一次为人们写作！如果你喜欢这篇文章，请与你周围的人分享。

***我在 blog.yaabed.com[写作](https://medium.com/yazanaabed)。如果你喜欢这篇文章，请确保与其他人分享。不要忘了点击关注按钮获取更多类似的文章，还有[在 twitter 上关注我](https://twitter.com/YazanAabed)。***

![1*MSPCzn3l6S8PfjbPj0m7jw](img/eb3b754b39c89506e0c6d479ae713ee2.png)

> 嗨，我叫[亚赞·阿德](https://www.yaabed.com/)。在巴勒斯坦长大。我的专业是计算机科学。我是前端工程师& JavaScript 爱好者？？‍?。主要使用前端框架，如(AngularJs，ReactJS)。你可以叫我#极客？。还有，我喜欢和别人分享我的知识，向他们学习？？？。你可以在 GitHub、 [Mediu](https://github.com/YazanAabeed) m、 [Twitt](https://medium.com/@yazanaabed) er [上找到我。](https://twitter.com/YazanAabed)

[**webpack 学习学院**](https://webpack.academy/)
[*webpack 学习学院的存在是为了提供精选的、高质量的学习内容，致力于 webpack 开源…*webpack . academy](https://webpack.academy/)[**从 Grunt 和 Bower 到 web pack， Babel and Yarn —迁移遗留前端构建系统**](https://medium.com/appifycanada/migrate-to-webpack-from-grunt-bower-legacy-build-system-344526f47873)
[*我为国际癌症基因组联盟的数据门户继承的构建系统相当现代……*medium.com](https://medium.com/appifycanada/migrate-to-webpack-from-grunt-bower-legacy-build-system-344526f47873)[**如何逐步切换到 web pack**](https://medium.com/eventmobi/how-to-incrementally-switch-to-webpack-203a1b431f7a)
[*这是关于我们为什么以及如何将 JavaScript 绑定系统从临时系统切换到临时系统的两部分系列文章的第二部分……*medium.com](https://medium.com/eventmobi/how-to-incrementally-switch-to-webpack-203a1b431f7a) *webpack
[*这是一个两部分系列的第一部分，讲述我们为什么以及如何将 JavaScript 捆绑系统从一个临时系统切换过来…*medium.com](https://medium.com/eventmobi/why-we-switched-to-webpack-69b7396f3ec5)[**从 Grunt 到 web pack 的第一步**](https://advancedweb.hu/2016/02/02/the-first-steps-from-grunt-to-webpack/)
[*使用 Grunt 后开始使用 web pack*advanced web . Hu](https://advancedweb.hu/2016/02/02/the-first-steps-from-grunt-to-webpack/)[**web pack 之旅-服务器密度博客** 发布于 2016 年 1 月 6 日。在过去的几年里，我们建造了…](https://blog.serverdensity.com/the-journey-to-webpack/)*blog.serverdensity.com

> [【讨论】我们是如何从咕噜咕噜到大口大口再到 Webpack 的？](https://www.reddit.com/r/javascript/comments/42z1xl/discussion_how_did_we_go_from_grunt_to_gulp_to/?ref_source=embed&ref=share)来自 [javascript](https://www.reddit.com/r/javascript/)