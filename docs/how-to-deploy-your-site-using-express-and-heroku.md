# 如何使用 Express.js 和 Heroku 将您的应用程序部署到 web

> 原文：<https://www.freecodecamp.org/news/how-to-deploy-your-site-using-express-and-heroku/>

如果你是 web 开发的新手，你会花很多时间学习如何用 HTML、CSS 和 JavaScript 构建静态网站。

然后你可能开始学习如何使用流行的框架，比如 [React](https://reactjs.org/) 、 [VueJS](https://vuejs.org/) 或者 [Angular](https://angular.io/) 。

但是在尝试了一些新想法并在本地运行了一些网站后，你可能会想知道如何实际部署你的网站或应用程序。事实证明，有时很难知道从哪里开始。

就我个人而言，我发现在 Heroku 上运行一个 Express 服务器是最简单的方法之一。本文将向您展示如何做到这一点。

Heroku 是一个云平台，支持许多不同的编程语言和框架。

这不是一篇赞助的文章，当然还有许多其他的解决方案，例如:

*   [数字海洋](https://www.digitalocean.com/)
*   [亚马逊网络服务](https://aws.amazon.com/)
*   [蔚蓝色](https://azure.microsoft.com/en-gb/)
*   [谷歌云平台](https://cloud.google.com/)
*   [Netlify](https://www.netlify.com/)
*   [ZEIT Now](https://zeit.co/)

把它们都检查出来，看看哪个最适合你的需要。

我个人觉得 Heroku 是最快最容易开始使用“开箱即用”的。自由层在资源方面有些受限。然而，我可以自信地推荐它用于测试目的。

这个例子将使用 Express 服务器托管一个简单的站点。以下是高级步骤:

1.  使用 Heroku、Git 和 npm 进行设置
2.  创建一个 Express.js 服务器
3.  创建静态文件
4.  部署到 Heroku

总共需要大约 25 分钟(如果您想在静态文件上花费更多时间，可能需要更长时间)。

本文假设您已经知道:

*   一些 HTML、CSS 和 JavaScript 基础知识
*   基本命令行用法
*   用于版本控制的初级 Git

你可以在[这个库](https://github.com/pg0408/lorem-ipsum-demo)里找到所有的代码。

### 安装

任何项目的第一步都是准备好所有你需要的工具。

您需要具备:

*   安装在您本地机器上的节点和 npm(在此阅读如何执行此操作
*   Git 已安装(请阅读本指南
*   Heroku CLI 已安装([下面是如何安装的](https://devcenter.heroku.com/articles/heroku-cli#download-and-install)

**1。创建一个新目录并初始化一个 Git 库**

从命令行中，创建一个新的项目目录并移入其中。

```
$ mkdir lorem-ipsum-demo
$ cd lorem-ipsum-demo
```

现在你在项目文件夹中，初始化一个新的 Git 库。

⚠️This 步骤很重要，因为 Heroku 依赖 Git 将代码从本地机器部署到⚠️的云服务器上

```
$ git init
```

最后一步，您可以创建一个 README.md 文件，供以后编辑。

```
$ echo "Edit me later" > README.md
```

**2。登录 Heroku CLI 并创建一个新项目**

您可以使用 Heroku CLI(命令行界面)登录 Heroku。你需要有一个免费的 Heroku 帐号来完成这个任务。

这里有两个选择。Heroku 的默认设置是让您通过 web 浏览器登录。添加`-i`标志允许您通过命令行登录。

```
$ heroku login -i
```

现在，您可以创建一个新的 Heroku 项目。我把我的叫做`lorem-ipsum-demo`。

```
$ heroku create lorem-ipsum-demo
```

命名您的项目:

*   如果您没有在命令中指定名称，Heroku 将为您的项目生成一个随机名称。
*   该名称将构成您可以用来访问项目的 URL 的一部分，因此请选择您喜欢的名称。
*   这也意味着您需要选择一个别人没有使用过的独特的项目名称。
*   以后可以重命名您的项目(所以现在不要太担心获得一个完美的名称)。

**3。初始化一个新的 npm 项目并安装 Express.js**

接下来，您可以通过创建 package.json 文件来初始化一个新的 npm 项目。使用下面的命令来完成此操作。

⚠️This 迈出了至关重要的一步。Heroku 依靠您提供的 package.json 文件来知道这是一个 Node.js 项目，当它构建您的应用程序⚠️时

```
$ npm init -y
```

接下来，[安装 Express](https://expressjs.com/en/starter/installing.html) 。Express 是一个广泛使用的 NodeJS 服务器框架。

```
$ npm install express --save
```

终于可以开始编码了！

### 编写一个简单的 Express 服务器

下一步是创建一个名为`app.js`的文件，它在本地运行一个 Express 服务器。

```
$ touch app.js
```

该文件将成为应用程序准备就绪时的入口点。这意味着，启动应用程序所需的一个命令将是:

```
$ node app.js
```

但是首先，您需要在文件中编写一些代码。

**4。编辑 app.js 的内容**

在您最喜欢的编辑器中打开`app.js`。编写如下所示的代码，然后单击 save。

```
// create an express app
const express = require("express")
const app = express()

// use the express-static middleware
app.use(express.static("public"))

// define the first route
app.get("/", function (req, res) {
  res.send("<h1>Hello World!</h1>")
})

// start the server listening for requests
app.listen(process.env.PORT || 3000, 
	() => console.log("Server is running..."));
```

这些评论应该有助于指出正在发生的事情。但是让我们快速分解代码以进一步理解它:

*   前两行只需要 Express 模块并创建一个 Express 应用程序的实例。
*   下一行需要使用`express.static`中间件。这允许您从指定的目录中提供静态文件(如 HTML、CSS 和 JavaScript)。在这种情况下，文件将从名为`public`的文件夹中提供。
*   下一行使用`app.get()`来定义一个 URL 路由。对根 URL 的任何 URL 请求都将用一个简单的 HTML 消息来响应。
*   最后一部分启动服务器。它要么查看 Heroku 将使用哪个端口，要么默认为 3000(如果您在本地运行)。

⚠️The 在最后一行使用`process.env.PORT || 3000`对于成功部署你的应用程序非常重要，⚠️

如果保存`app.js`并使用以下命令启动服务器:

```
$ node app.js
```

你可以在你的浏览器中访问 [localhost:3000](http://localhost:3000/) ，亲眼看看服务器正在运行。

### 创建您的静态文件

下一步是创建静态文件。每当用户访问您的项目时，您将提供这些 HTML、CSS 和 JavaScript 文件。

还记得在`app.js`中，您告诉`express.static`中间件从`public`目录中提供静态文件。

第一步当然是创建这样一个目录和其中包含的文件。

```
$ mkdir public
$ cd public
$ touch index.html styles.css script.js
```

**5。编辑 HTML 文件**

在您喜欢的文本编辑器中打开`index.html`。这将是你提供给访问者的网页的基本结构。

下面的例子为一个 Lorem Ipsum 生成器创建了一个简单的登陆页面，但是你可以在这里随心所欲地创造。

```
<!DOCTYPE html>
<head>
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
	<link href="https://fonts.googleapis.com/css?family=Alegreya|Source+Sans+Pro&display=swap" rel="stylesheet">
	<link rel="stylesheet" type="text/css" href="styles.css">
</head>
<html>
<body>
<h1>Lorem Ipsum generator</h1>
  <p>How many paragraphs do you want to generate?</p>
  <input type="number" id="quantity" min="1" max="20" value="1">
  <button id="generate">Generate</button>
  <button id="copy">Copy!</button>
<div id="lorem">
</div>
<script type="text/javascript" src="script.js"></script>
</body>
</html>
```

**6。编辑 CSS 文件**

接下来是编辑 CSS 文件`styles.css`。确保在你的 HTML 文件中有链接。

下面的 CSS 是针对 Lorem Ipsum 示例的。但是，请随意发挥你的创造力。

```
h1 {
	font-family: 'Alegreya' ;
}

body {
	font-family: 'Source Sans Pro' ;
	width: 50%;
	margin-left: 25%;
	text-align: justify;
	line-height: 1.7;
	font-size: 18px;
}

input {
	font-size: 18px;
	text-align: center;
}

button {
	font-size: 18px;
	color: #fff;
}

#generate {
	background-color: #09f;
}

#copy {
	background-color: #0c6;
}
```

**7。编辑 JavaScript 文件**

最后，您可能想要编辑 JavaScript 文件`script.js`。这将使你的页面更具互动性。

下面的代码为 Lorem Ipsum 生成器定义了两个基本函数。是的，我用的是 JQuery，它既快又容易使用。

```
$("#generate").click(function(){
	var lorem = $("#lorem");
	lorem.html("");
	var quantity = $("#quantity")[0].valueAsNumber;
	var data = ["Lorem ipsum", "quia dolor sit", "amet", "consectetur"];
	for(var i = 0; i < quantity; i++){
		lorem.append("<p>"+data[i]+"</p>");
	}
})

$("#copy").click(function() {
	var range = document.createRange();
	range.selectNode($("#lorem")[0]);
	window.getSelection().removeAllRanges();
	window.getSelection().addRange(range);
	document.execCommand("copy");
	window.getSelection().removeAllRanges();
	}
)
```

注意，这里的`data`列表被截断，以便更容易显示。在实际的应用程序中，它是一个更长的完整段落列表。你可以在回购中看到整个文件，或者在这里寻找原始来源。

### 部署您的应用

在编写完静态代码并检查一切正常后，就可以准备部署到 Heroku 了。

然而，还有更多的事情要做。

**8。创建一个过程文件**

Heroku 需要一个 Procfile 来知道如何运行你的应用程序。

Procfile 是一个“进程文件”,它告诉 Heroku 运行哪个命令来管理给定的进程。在这种情况下，该命令将告诉 Heroku 如何启动您的服务器监听 web。

使用下面的命令创建文件。

⚠️This 是重要的一步，因为没有 Procfile，Heroku 就不能把你的服务器联机。⚠️

```
$ echo "web: node app.js" > Procfile
```

请注意，Procfile 没有文件扩展名(例如。txt“，”。json”)。

此外，看看命令`node app.js`是如何在本地运行您的服务器的。

**9。将文件添加并提交到 Git**

记住，您在设置时启动了一个 Git 存储库。也许你一直在添加和提交文件。

在部署到 Heroku 之前，确保添加所有相关文件并提交它们。

```
$ git add .
$ git commit -m "ready to deploy"
```

最后一步是推到你的 Heroku 主分支。

```
$ git push heroku master
```

当 Heroku 构建和部署您的应用程序时，您应该会看到命令行打印出大量信息。

要查找的行是:`Verifying deploy... done.`

这表明您的构建是成功的。

现在你可以打开浏览器访问 your-project-name.herokuapp.com 了。您的应用程序将被托管在 web 上供所有人访问！

### 快速回顾

以下是将简单快捷应用程序部署到 Heroku 的步骤:

1.  创建一个新目录并初始化一个 git 存储库
2.  登录 Heroku CLI 并创建一个新项目
3.  初始化一个新的 npm 项目并安装 Express.js
4.  编辑 app.js 的内容
5.  编辑静态 HTML、CSS 和 JavaScript 文件
6.  创建个人资料
7.  添加并提交到 Git，然后推送到 Heroku master 分支

### 如果您的应用程序不工作，需要检查的事项

有时候，尽管有最好的意图，互联网上的教程并不完全像你预期的那样工作。

以下步骤应该有助于调试您可能会遇到的一些常见错误:

*   你是否在你的项目文件夹中初始化了一个 Git repo？检查你之前是否跑了`git init`。Heroku 依靠 Git 从本地机器部署代码。
*   是否创建了 package.json 文件？检查你之前是否跑了`npm init -y`。Heroku 需要一个 package.json 文件来识别这是一个 Node.js 项目。
*   服务器正在运行吗？确保您的 Procfile 使用正确的文件名来启动服务器。检查你有`web: node app.js`而没有`web: node index.js`。
*   Heroku 知道监听哪个端口吗？检查您是否在 app.js 文件中使用了`app.listen(process.env.PORT || 3000)`。
*   你的静态文件中有错误吗？通过在本地运行并查看是否有任何错误来检查它们。

感谢您的阅读——如果您已经阅读了这么多，您可能想要查看演示项目的完成版本。