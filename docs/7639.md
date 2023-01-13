# 通过示例学习 Webpack:模糊的占位符图像

> 原文：<https://www.freecodecamp.org/news/learn-webpack-by-example-blurred-placeholder-images-4ad8b1751709/>

作者卡拉劳·坎特雷尔

# 通过示例学习 Webpack:模糊的占位符图像

![1*h2X9ckg3FJr4FGSfwh0F2w](img/c49cba0866f44fedab3cbd4f6a2a898b.png)

*Mobile. Image by Rodion Kutsaev on [Unsplash](https://unsplash.com/).*

***这个帖子附带的回购用的是 webpack 3。如果你对学习 webpack 4 感兴趣，你会发现这篇文章很有用，因为概念和配置文件格式是一样的。Webpack 4 确实引入了优化、零配置功能以及新的开箱即用插件，高级用户可能想了解这些，但这超出了本文的目的。***

这是一个通过各种例子学习 webpack 的阶段性指南。欢迎 Webpack 新手。我自己就是其中之一，我会试着用对刚开始了解这个工具的人有意义的术语来解释 webpack 的东西。

所有维护本指南中使用的包的人都值得表扬，因为他们为社区提供了这么棒的工具。因为这是本指南的主题，所以要特别感谢负责维护该项目的[快速响应装载机](https://github.com/herrstucki/responsive-loader/)和[杰瑞米·斯图基](https://www.freecodecamp.org/news/learn-webpack-by-example-blurred-placeholder-images-4ad8b1751709/undefined)。

在第一集里，我们将会看到一种加载图片的技术。这包括 1)在初始页面加载时内嵌我们图像的模糊占位符版本。然后，2)从服务器请求完整的图像。最后，3)当完整的图像最终加载时，它们被淡入，模糊的占位符被移除。

![1*zxTkKZ-oMGJXO0l-_RQZxQ](img/c8198969d792a7901bb3150d7f105a27.png)

*Blurred placeholder images.*

这项技术非常适合慢速连接的设备。它让用户在几秒钟内(想想缓慢的 3G)对页面图像完全加载的感觉。

### 入门指南

如果你想在你的代码编辑器中跟随，你可以下载[这个回购](https://github.com/klcantrell/webpack-through-example-blog/tree/blur-up)或者`git clone`和`checkout`回购的`blur-up`分支。

当你打开项目文件夹时，下面是你应该找到的文件结构。

我们将使用 webpack，特别是**响应式加载器。**我们将为`src/imgs.`中的三个图像调整大小并生成模糊占位符，顺便说一下，这些是作者一直以来最喜欢的视频游戏中的角色。

让我们看看从`index.html`开始的源代码。在我们进行的过程中，我们将看到 webpack 为我们做了什么，我们将停下来讨论如何做。为简洁起见，样板文件已被省略，代之以`<-- ...` - >。

你可能已经注意到有三个`<`；一个>元素，我们每个人的图像一个。但是模板文字是怎么回事呢？那么 `that r`等价是怎么回事呢？这就是我们要求 webpack 做的事情。

当 webpack 解析我们的`HTML`时，它会遇到模板文字，并知道它需要在那里放些东西。`require`函数告诉 webpack *在那里放什么*——在我们的例子中，我们正在放入图像数据(可能还不清楚我们在那里放了什么数据，但请相信我，我们会到达那里的)。那么，webpack 是如何知道做到这一点的呢？是自动的吗？

如果您以前从未见过 webpack 配置文件，您可能只需看一眼就能猜到它不是自动生成的。有许多选项，其中一些特定于 webpack，另一些特定于某个加载程序或插件。那么，什么是**加载器**呢？什么是**插件**？

### 快速定义

在深入配置之前，我将提供这些 webpack 概念的快速定义。我还会提供详细解释它们的文档链接。

*   [**加载器**](https://webpack.js.org/concepts/#loaders) :它的工作是把你的文件，以某种方式转换，并给你转换的结果。您得到的结果取决于您正在处理的文件类型和加载程序的功能。使用我们今天项目中的一个例子，您可以使用一个加载器获取一个图像文件，将其转换成图像数据，然后将该数据内联到您的`HTML`中。
*   [**插件**](https://webpack.js.org/concepts/#plugins) :它的工作是完成比加载器更一般的任务。而加载器将特定的转换应用于特定的文件类型。然而，插件可以执行诸如文件压缩、文本缩小等任务。以我们今天的项目为例，你可以使用一个插件来压缩图像文件。

### HTML 处理

现在让我们看看如何使用加载器和插件来具体处理我们的`HTML`。以下是我们的`webpack.config.js`中与`HTML`有关的部分。我们最终将讨论的其他选项将被省略，并替换为`// ...`。

首先，我们引入 **html-webpack-plugin** ，并将其分配给一个名为`HtmlWebpackPlugin`的变量(creative，对吗？).这个插件的工作是生成我们将在发行版中使用的`HTML`文件。为了启动插件，我们在 config 对象的`plugins`属性中的变量上使用了`new`操作符。我所指的配置对象是分配给`module.exports`的对象，它“告诉”webpack 做什么。

没有任何选项传入，html-webpack-plugin 会生成非常普通的样板文件`HTML`。但是，请注意，我们已经将它的`template`属性设置为等于我们的源`index.html`文件。正如你可能猜到的，这是我们告诉插件在为我们生成一个`HTML`文件时使用我们的`index.html`作为模板。很好，但是你为什么要这么麻烦呢？

这是因为我们想使用加载器来转换我们的源代码`HTML`。我们想改变这种情况:

变成这样:

注意，模板文字和`require`函数已经被替换。现在,`a.href`属性有了一个 URL，指向我们的图片的调整后版本,`300px` wide。另外，`img.src`属性现在有了内嵌图像数据。我展示了一个`<`对我们`HTML`的改造；一个>元素，但这就是我们想要的 al `l t` he < a >元素的样子。

让我们看看如何使用加载器来完成这种转换。让我们放大从`webpack.config.js`开始的代码块，它以`test: /\.html$/`键值对开始。

```
{  test: /\.html$/,  use: {    loader: 'html-loader',    options: {     interpolate: true    }  }}
```

这个模块说，“嘿，webpack，当你遇到`HTML`文件时，请使用 **html-loader** ，并确保它的设置允许插值”。

换句话说，我们`test`为“html”扩展名。我们将`use` **html 加载器**作为该类型文件的`loader`，然后我们在`options`中指定我们想要使用来自 **html 加载器**的`interpolate`特性。

如果你看一下 **html-loader** [文档](https://github.com/webpack-contrib/html-loader)，你会发现当`interpolate`被设置为`true`时，你可以将一些 JavaScript `(JS)`的结果嵌入到我们的`HTML`中。在我们的例子中，我们通过调用`require`函数告诉 webpack 引入图像资产来利用这一点。但是 webpack 如何知道如何处理图像呢？

### 图像处理

我们需要告诉它使用什么加载器和插件。下面是我们的`webpack.config.js`文件中指导 webpack 如何处理图像的部分。

我们正在使用的 **imagemin-webpack-plugin** 有一个非常简单的工作——它只是压缩我们的图像。你可以在这里阅读更多关于那个[的内容，但是更有趣的是我们用来转换图像的加载器。看看以`test: /\.(png|jpg|gif)$/`键值对开始的代码块。](https://www.npmjs.com/package/imagemin-webpack-plugin)

这个模块说，“嘿，网络包，当你遇到图像文件时，使用**响应加载器。**以`300px`宽度生成图像的调整版本。同时，为一个`50px`宽的占位符图像创建数据。

换句话说，我们`test`为“png“或”。jpg“或”。gif "扩展名。我们将`use` **responsive-loader** 作为这些类型文件的`loader`。然后我们在`options`中指定我们想要使用**响应加载器**的`resize`、`placeholder`和`name`特性来转换我们的图像。

让我们详细看看**响应装载机**在这些选项中为我们做了什么。当我们说:

```
require('./imgs/cloud-strife.jpg');
```

然后**响应式装载机**把这个还给我们:

它只是一个`JS`物体。这就是为什么 when 可以使用`.src`和`.placeholder`从我们的`require`语句中获取我们需要的内容，这样当我们这样做时:

```
&lt;img src="${require('./imgs/cloud-strife.jpg').placeholder}"     class="hero-preview"      alt="cloud-strife">
```

webpack 给了我们这个:

```
<!--image data truncated for brevity-->;<img src="data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/          2wCEAAUDBAQEAwUEBAQFBQUGBwwIBwcHBw8LCwkMEQ8SEhEPERETFhw          XExQaFRERGCEYGh0dHx8fExciJCIeJBweHx4BBQUFBwYHDggIDh4U          ERQeHh4eHh4e..."     class="hero-preview"     alt="cloud strife">
```

### 快速回顾

太棒了，所以我们有一个处理我们的`HTML`和图像的工作流程。概括一下:

对于`HTML`，我们使用 **html-webpack-plugin** 生成一个`HTML`文件，使用我们的源`index.html`作为模板。我们使用 **html-loader** 来处理我们的`HTML`，并特别允许插值。插值让我们在`HTML`中使用`require` 语句，这样我们就可以要求 webpack 以某种方式加载图像。

对于图像，我们使用 **responsive-loader** 来生成图像的调整版本。然后，它为我们图像的模糊占位符版本生成图像数据，供我们在线使用。

一旦我们的代码被这些加载器转换，图像路径和图像数据就被嵌入到我们的`HTML`中。不错！

### 源级联样式表(CSS)和 JavaScript(JS)

让我们来看看其余的源代码。关于我们如何使用`JS`和`CSS`在加载完整图像后淡入并移除占位符图像的解释，请参见代码注释。

#### CSS:

#### JS:

### 加载 CSS 和 JS

下面是我们的`index.js`的样子。这个文件是我们告诉 webpack 引入所有我们想要使用的模块，然后使用它们的地方。最简单地说，一个 [**模块**](https://webpack.js.org/concepts/modules/) 就是我们想要导入并使用的另一个文件中的一段代码。

在一个`JS`文件中，我们可以使用 ES2015 `import`语法代替`require`来引入模块。例如，注意`import loadFullImages from './loadImages'`和`const loadFullImages = require('./loadImages)`做同样的事情。

在我们的例子中，我们只有两个模块。注意，webpack 中的模块并不局限于`JS`——如果我们使用正确的加载器，我们也可以将`CSS`文件视为模块。这是强大的，但一开始可能会令人困惑。然而，一旦我向你介绍了 webpack 是如何加载我们的`CSS`文件的，你会看到我们所做的只是缩小我们的源`CSS`并生成一个`main.css`文件:

在上面的选项块中，注意我们可以通过传入一个 loader 对象数组在`use` 属性中指定多个加载器。然后，每个加载程序处理该文件，从数组中的最后一个加载程序开始，到第一个加载程序结束。

这个程序块基本上是说，“嘿，webpack，当你遇到`CSS`文件时，请使用 **css-loader** 把`CSS`带进来并缩小它。然后使用**提取加载器**将其与我们的`JS`分开(更多关于[的信息，请点击](https://webpack.js.org/loaders/extract-loader/))。然后使用**文件加载器**用原始源文件的名称和扩展名为我们创建一个文件(在我们的例子中，它命名为“main.css”)。

这是我们告诉 webpack 加载我们的 JS 的方式:

这个块基本上说的是，“嘿 webpack，当你遇到`JS`文件时，请使用 **babel-loader** 及其 **env** 预置来编译我们的`JS`。Babel 采用了我们用 ES2015+语法编写的源代码`JS` ，并将其编译成浏览器友好的 ES5。`modules: false`选项告诉 Babel 不要担心转换我们的`import` 语法。Webpack 已经在这么做了。

### 建筑

如果你想看到 webpack 生成发行版文件，那么继续安装 [Node.js](https://nodejs.org/) ，如果你还没有安装的话，可以安装 [npm](https://www.npmjs.com/) 附带的。打开命令行控制台，进入项目目录。如果你在 Windows 上并且需要一个*NIX 友好的 shell，使用 Windows Powershell 而不是默认的命令提示符。

一旦进入项目目录，运行`npm install`命令安装我们在本指南中讨论过的所有包。然后运行`npm start` 命令来执行构建。下面是我们仍然需要检查的 webpack 配置的最后一点。这就是 webpack 知道将分发文件发送到哪里的方式:

`path`是一个实用模块。它允许我们容易地构造平台友好的文件和目录路径。无论您平台的文件系统使用“/”还是“\”作为路径分隔符，这些都将有效。这里我们使用`path.join`函数告诉 webpack 在哪里找到并发送我们的文件。

`entry`告诉 webpack 哪个模块是“主”模块，在这个模块中，我们导入我们所依赖的所有其他模块。`app`是我们给主包起的名字，webpack 将通过把我们所有的模块缝合在一起来创建这个包。

最后，`output.path`告诉 webpack 将它为我们创建的所有文件发送到哪里。`output.filename`告诉 webpack 为它创建的包使用什么命名方案。在我们的例子中，我们只是创建了一个包，它将被命名为“app.bundle.js”。

### 结论

我希望您能够通过这个例子了解更多关于 webpack 如何帮助您构建东西的信息。我也希望你从阅读这篇文章中学到了一些东西。例如，图像加载技术，编写模块化`JS`的方法，甚至只是练习阅读别人的代码。最后，我希望您能够在浏览器中启动生成的代码，并看到它的运行。感谢阅读！

如果这对你有所帮助，请鼓掌，有任何问题请在下面评论，并在推特上向[我](https://twitter.com/kalalaucantrell)和[杰里米·斯图基](https://twitter.com/herrstucki)问好。