# 如何使用 Express 生成自述徽章

> 原文：<https://www.freecodecamp.org/news/how-to-generate-readme-badges-with-express-and-squirrelly-77310125dca0/>

本·古布勒

# 如何使用 Express 生成自述徽章

![1*BqQhKwkKqIBn_lL-pmYHNg](img/8012cc5daa4258a187025c0a60368a6f.png)

The kind of badge you’ll be able to generate (enlarged)

我第一次在这个网站上看到这个想法。因为这个教程已经有几年的历史了，所以我用更新的代码写了一个新的教程。

如果你很着急，你可以在这里找到完整的代码[。](https://github.com/nebrelbug/badge-generator)

我们都知道或使用它们——几乎每个自述文件顶部的闪亮徽章，上面写着“**构建:通过**”或“**大小:10KB** ”等内容。本指南将教你如何生成你自己的徽章，除了 Node.js、ExpressJS 和 Squirrelly 之外什么都没有。

### 现在是教程

#### 先决条件

本教程假设您已经安装了 Node.js 和 npm(或 yarn)。如果没有，请转到这里的节点站点(默认情况下，它随 npm 一起安装)。

#### 设置

首先，创建一个新目录并`cd`进入其中:

```
mkdir badge-generator && cd badge-generator
```

接下来，安装必要的依赖项， [Express](https://expressjs.com/) 和[squirrely](https://squirrelly.js.org/)。

使用 npm:

```
npm install express squirrelly
```

或者对那些使用纱线的人来说:

```
yarn add express squirrelly
```

#### 创建服务器

我们的程序只需要两个文件， **index.js** 和 **template.svg** (我们接下来将创建它们)。

创建一个名为 **index.js** 的文件，并粘贴以下代码:

这将在端口 8080 上打开一个服务器，并侦听请求。在本教程结束时，你将能够向[**http://localhost:8080/left-text/right-text/color**](http://localhost:8080/left-text/right-text/color)发出请求，并获得一个令人敬畏的 SVG 徽章！耶！但是带有`Sqrl`的代码部分是关于什么的呢？

```
var badge = Sqrl.renderFile(path.join(__dirname, 'template.svg'), req.params)
```

这就是古怪的地方。我们希望提供一个 SVG 图像文件，但是图像的内容(宽度、长度和文本)每次都会不同。Squirrelly 是一个**模板引擎**，一个程序获取一个叫做模板的文件或字符串并插入数据。它还做了一些其他有趣的事情，比如处理缓存，但是我们不需要担心这个。

上面的代码读取名为`template.svg`的文件，然后使用`req.params`(一个包含路径的对象)填充模板。在这种情况下，`req.params`会是什么样子:

```
{  left: "first-part-of-the-url-path",  right: "second-part-of-the-url-path",  color: "third-part"}
```

#### 创建模板

创建一个名为`template.svg`的新文件，并粘贴以下代码:

你可以在这里阅读完整的奇异文件[，但是本质上，任何在方括号:`{{`和`}}`之间的东西都将被它的实际值代替。](https://squirrelly.js.org/)

但是等等:我们只传入了`left`、`right`、`color`——哪里来的`leftWidth`、`rightWidth`？下面的代码(在模板的顶部)就是这么做的；使用`js`助手(允许您在模板内部编写 JS)，它定义了一个新变量，称为`leftWidth`。

```
{{js(options.leftWidth = options.left.length * 10)/}}
```

还有一件事要做。请注意，第 18 行如下所示:

```
<rect ...stuff... fill="{{returnColor(options.color)/}}"/>
```

对于 SVG 图像，填充属性必须要么包含几个看起来不太好的预定义颜色之一，要么包含一个 [RGB](https://en.wikipedia.org/wiki/RGB_color_model) 或十六进制颜色。我们想使用十六进制代码，但有一个陷阱:你会注意到，如果你在一个浏览器中输入[**http://localhost:8080/some/text/# fff**](http://localhost:8080/ben/gubler/#fff)，它会认为末尾的十六进制代码是 url 末尾的哈希，Express 不识别它。

我们要做的是创建一个助手(名为`returnColor`)，它将把颜色词，如“亮绿色”、“绿色”和“红色”，翻译成十六进制颜色代码。将以下内容粘贴到 index.js 中的任意位置:

#### 看看它是否有效

在你的终端中输入`node index.js`，然后进入[http://localhost:8080/test/badge/bright green](http://localhost:8080/test/badge/brightgreen)。如果一切顺利，你应该看到一个徽章！

![1*s3-kyFWQZL_v7s8xDWn1vw](img/4ac3a81361f4fc2467784fd6ad863362.png)

如果有任何东西抛出错误，将您的代码与工作代码[进行比较。](https://github.com/nebrelbug/badge-generator)

你可以在下面找到更多关于 Squirrelly 的信息。

[**squirrely 文档**](https://squirrelly.js.org/)
[*squirrely 只有 2KB 的 gzipped，有 0 个依赖，速度极快。*squirrelly.js.org](https://squirrelly.js.org/)

感谢您阅读本指南。希望对你有帮助！