# Express 用例子解释-安装、路由、中间件等等

> 原文：<https://www.freecodecamp.org/news/express-explained-with-examples-installation-routing-middleware-and-more/>

## **快递**

当使用 Node.js 构建 web 应用程序时，创建一个服务器会花费很多时间。这些年来，由于社区的支持，Node.js 已经足够成熟。使用 Node.js 作为 web 应用程序和网站的后端有助于开发人员快速开始开发他们的应用程序或产品。

在本教程中，我们将研究 Express，它是一个用于 web 开发的 Node.js 框架，具有路由和渲染等功能，并支持 REST APIs。

## **什么是快递？**

Express 是最流行的 Node.js 框架，因为它只需要很少的设置就可以启动一个应用程序或 API，而且速度很快，同时又是非终结的。换句话说，与 Rails 和 Django 不同，它并没有强制执行自己的理念，即应用程序或 API 应该以特定的方式构建。它的灵活性可以通过可用的`npm`模块的数量来计算，这使得它可以同时插入。如果您对 HTML、CSS 和 JavaScript 以及 Node.js 的一般工作原理有基本的了解，那么您很快就能开始使用 Express。

Express 由 TJ Holowaychuk 开发，现在由 Node.js 基金会和开源开发者维护。要开始使用 Express 进行开发，您需要安装 Node.js 和 npm。你可以在你的本地机器上安装 [Node.js](https://nodejs.org/en/) ,随之而来的是命令行实用程序`npm`,它将帮助我们在我们的项目中安装插件或所谓的依赖项。

要检查所有安装是否正确，请打开您的终端并键入:

```
node --version
v5.0.0
npm --version
3.5.2
```

如果您得到的是版本号而不是错误，则意味着您已经成功安装了 Node.js 和 npm。

## **为什么要用快递？**

在我们开始使用 Express 作为后端框架的机制之前，让我们先探讨一下为什么我们应该考虑使用它或者它流行的原因。

*   Express 允许您构建单页、多页以及混合的 web 和移动应用程序。其他常见的后端用途是为客户端(无论是 web 还是移动)提供 API。
*   它有一个默认的模板引擎，Jade，它有助于将数据导入网站结构，并支持其他模板引擎。
*   它支持 MVC(模型-视图-控制器)，这是一种非常常见的设计 web 应用程序的架构。
*   它是跨平台的，并且不限于任何特定的操作系统。
*   它利用 Node.js 单线程和异步模型。

每当我们使用`npm`创建一个项目时，我们的项目必须有一个`package.json`文件。

### **创建 package.json**

JSON (JavaScript Object Notation)文件包含任何 Express 项目的所有信息。安装的模块数量、项目名称、版本和其他元信息。要将 Express 作为一个模块添加到我们的项目中，首先我们需要创建一个项目目录，然后创建一个 package.json 文件。

```
mkdir express-app-example
cd express-app-example
npm init --yes
```

这将在项目目录的根目录下生成一个`package.json`文件。要安装来自`npm`的任何模块，我们需要在那个目录中存在`package.json`文件。

```
{
  "name": "express-web-app",
  "version": "0.1.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "license": "MIT"
}
```

### **安装快递**

现在我们有了`package.json`文件，我们可以通过运行命令来安装 Express:

```
npm install --save express
```

我们可以通过两种方式确认 Express 已经正确安装。首先，`package.json`文件中会有一个名为`dependencies`的新部分，我们的 Express 就在这个部分下:

```
{
  "name": "express-web-app",
  "version": "0.1.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "license": "MIT",
  "dependencies": {
    "express": "4.16.0"
  }
}
```

第二种方式是，一个名为`node_modules`的新文件夹突然出现在我们项目目录的根目录下。这个文件夹存储了我们在项目中本地安装的包。

## **使用 Express 构建服务器**

为了使用我们为 Express framework 安装的包并创建一个简单的服务器应用程序，我们将在项目目录的根目录下创建文件`index.js`。

```
const express = require('express');
const app = express();

app.get('/', (req, res) => res.send('Hello World!'));

app.listen(3000, () => console.log('Example app listening on port 3000!'));
```

要启动服务器，请转到您的终端并键入:

```
node index.js
```

这将启动服务器。这个最简单的应用程序将侦听端口 3000。我们在`http://localhost:3000`通过我们的浏览器发出请求，我们的服务器将使用`Hello World`进行响应，浏览器是客户端，消息将显示在那里。

我们代码的第一行使用了`require`函数来包含`express`模块。这就是我们如何在项目的任何 JavaScript 文件中包含和使用从 npm 安装的包。在我们开始使用 Express 之前，我们需要定义一个它的实例来处理从服务器到客户机的请求和响应。在我们的例子中，它是变量`app`。

`app.get()`是一个函数，它告诉服务器在给定路由的`get`请求被调用时该做什么。它有一个回调函数`(req, res)`，监听传入的请求`req`对象，并使用`res`响应对象做出相应的响应。Express 框架使`req`和`res`对我们可用。

`req`对象表示 HTTP 请求，并具有请求查询字符串、参数、主体和 HTTP 头的属性。res 对象表示 Express 应用程序在获得 HTTP 请求时发送的 HTTP 响应。在我们的例子中，每当向路由`/`发出请求时，我们就发送一个文本`Hello World`。

最后，`app.listen()`是启动端口和主机的函数，在我们的例子中,`localhost`用于连接监听来自客户端的请求。我们可以定义端口号，比如`3000`。

## **快速应用的剖析**

Express 服务器文件的典型结构很可能包含以下部分:

****依赖****

导入依赖项，如 express 本身。这些依赖项是使用`npm`安装的，就像我们在前面的例子中所做的那样。

****实例化****

这些是创建对象的语句。要使用 express，我们必须从中实例化`app`变量。

****配置****

这些语句是基于自定义应用程序的设置，它们是在实例化之后定义的，或者是在一个单独的文件中定义的(在讨论项目结构时会详细介绍)，并且是我们的主服务器文件所必需的。

****中间件****

这些函数决定了请求-响应循环的流程。它们在每个传入的请求之后被执行。我们还可以定义定制的中间件功能。我们在下面有关于它们的章节。

****航线****

它们是在我们的服务器中定义的端点，有助于为特定的客户端请求执行操作。

****自举服务器****

Express 服务器中最后执行的是启动服务器的`app.listen()`函数。

我们现在开始讨论之前没有讨论过的部分。

## **路由**

路由是指服务器端应用程序如何响应客户端对特定端点的请求。这个端点由一个 URI(路径如`/`或`/books`)和一个 HTTP 方法如 GET、POST、PUT、DELETE 等组成。

路由可以是好的旧网页，也可以是 REST API 端点。在这两种情况下，语法是相似的。路由的语法可以定义为:

```
app.METHOD(PATH, HANDLER);
```

路由器有助于分离关注点，例如不同的端点，并将源代码的相关部分放在一起。它们有助于构建可维护的代码。所有的路径都是在`app.listen()`的函数调用之前定义的。在典型的 Express 应用程序中，`app.listen()`将是最后执行的函数。

### **路由方法**

HTTP 是客户端和服务器进行通信的标准协议。它为客户端发出请求提供了不同的方法。每个路由至少有一个 hanlder 函数或一个回调函数。这个回调函数决定了服务器对特定路由的响应。例如，路由`app.get()`用于处理 GET 请求，并作为响应发送简单的消息。

```
// GET method route
app.get('/', (req, res) => res.send('Hello World!'));
```

### **路由路径**

路由路径是请求方法的组合，用于定义客户端可以发出请求的端点。路由路径可以是字符串、字符串模式或正则表达式。

让我们在基于服务器的应用程序中再定义两个端点。

```
app.get('/home', (req, res) => {
  res.send('Home Page');
});
app.get('/about', (req, res) => {
  res.send('About');
});
```

把上面的代码看作一个最小的网站，它有两个端点，`/home`和`/about`。如果一个客户端请求主页，它只会用`Home Page`响应，在`/about`它会发送响应:`About Page`。如果选择了定义的两条路由中的任何一条，我们将使用`res.send`函数将字符串发送回客户端。

### **路由参数**

路由参数是命名的 URL 段，用于捕获在 URL 中其位置指定的值。在这种情况下使用 object，因为它可以访问 url 中传递的所有参数。

```
app.get('/books/:bookId', (req, res) => {
  res.send(req.params);
});
```

上述源代码中来自客户端的请求 URL 将是`http://localhost:3000/books/23`。路线参数的名称必须由字符([A-Za-z0-9_])组成。在我们的应用程序中，路由参数的一个非常常见的用例是拥有 404 条路由。

```
// For invalid routes
app.get('*', (req, res) => {
  res.send('404! This is an invalid URL.');
});
```

如果我们现在使用`node index.js`从命令行启动服务器，并尝试访问 URL: `http://localhost:3000/abcd`。作为响应，我们将得到 404 消息。

## **中间件功能**

中间件功能是那些在应用程序的请求-响应周期中可以访问请求对象(`req`)、响应对象(`res`)和`next`功能的功能。这些函数的目标是修改任务的请求和响应对象，如解析请求体、添加响应头、对请求-响应周期进行其他更改、结束请求-响应周期以及调用下一个中间件函数。

`next`功能是快速路由器中的功能，用于执行当前中间件之后的其他中间件功能。如果一个中间件函数包含了`next()`,这意味着请求-响应循环到此结束。这里函数`next()`的名字完全是随意的，你可以随意命名，但重要的是要坚持最佳实践，并尽量遵循一些惯例，尤其是当你和其他开发人员一起工作的时候。

此外，在编写定制中间件时，不要忘记添加`next()`函数。如果您不提及`next()`，请求-响应循环将会停滞不前，您的 servr 可能会导致客户端超时。

让我们使用创建自定义中间件功能来掌握对这一概念的理解。以这段代码为例:

```
const express = require('express');
const app = express();

// Simple request time logger
app.use((req, res, next) => {
   console.log("A new request received at " + Date.now());

   // This function call tells that more processing is
   // required for the current request and is in the next middleware
   function/route handler.
   next();  
});

app.get('/home', (req, res) => {
  res.send('Home Page');
});

app.get('/about', (req, res) => {
  res.send('About Page');
});

app.listen(3000, () => console.log('Example app listening on port 3000!'));
```

要设置任何中间件，无论是定制的还是作为 npm 模块提供的，我们都使用`app.use()`函数。它作为一个可选的参数路径和一个强制的参数回调。在我们的例子中，我们没有使用可选的 paramaeter 路径。

```
app.use((req, res, next) => {
  console.log('A new request received at ' + Date.now());
  next();
});
```

客户端发出的每个请求都会调用上面的中间件函数。运行服务器时，您会注意到，对于端点`/`上的每个浏览器请求，您的终端都会提示一条消息:

```
A new request received at 1467267512545
```

中间件功能可以用于特定的路由。请参见下面的示例:

```
const express = require('express');
const app = express();

//Simple request time logger for a specific route
app.use('/home', (req, res, next) => {
  console.log('A new request received at ' + Date.now());
  next();
});

app.get('/home', (req, res) => {
  res.send('Home Page');
});

app.get('/about', (req, res) => {
  res.send('About Page');
});

app.listen(3000, () => console.log('Example app listening on port 3000!'));
```

这一次，您只会在客户端请求端点`/home`时看到类似的提示，因为在`app.use()`中提到了路由。当客户端请求端点`/about`时，终端不会显示任何内容。

中间件功能的顺序很重要，因为它们定义了何时调用哪个中间件功能。在我们上面的例子中，如果我们在中间件`app.use('/home')...`之前定义路由`app.get('/home')...`，中间件功能将不会被调用。

### **第三方中间件功能**

中间件功能是一种有用的模式，它允许开发人员在他们的应用程序中重用代码，甚至以 NPM 模块的形式与他人共享代码。中间件的基本定义是一个具有三个参数的函数:请求(或 req)、响应(res)和下一个，我们在上一节中观察过。

通常在我们基于 Express 的服务器应用程序中，我们会使用第三方中间件功能。这些功能是由 Express 本身提供的。它们就像可以使用 npm 安装的插件，这就是为什么 Express 是灵活的。

Express 应用程序中一些最常用的中间件功能有:

#### **bodyParser**

它允许开发人员处理传入的数据，如主体有效载荷。有效负载就是我们从客户端接收到的要处理的数据。对 POST 方法最有用。它是通过以下方式安装的:

```
npm install --save body-parser
```

用法:

```
const bodyParser = require('body-parser');

// To parse URL encoded data
app.use(bodyParser.urlencoded({ extended: false }));

// To parse json data
app.use(bodyParser.json());
```

这可能是任何 Express 应用程序中使用最多的第三方中间件功能之一。

#### **cookieParser**

它解析 cookie 头，并用一个以 Cookie 名为关键字的对象填充`req.cookies`。要安装它，

```
$ npm install --save cookie-parser
```

```
const cookieParser = require('cookie-parser');
app.use(cookieParser());
```

#### **会话**

这个中间件函数创建一个带有给定选项的会话中间件。会话通常用于登录/注册等应用程序中。

```
$ npm install --save session
```

```
app.use(
  session({
    secret: 'arbitary-string',
    resave: false,
    saveUninitialized: true,
    cookie: { secure: true }
  })
);
```

### **摩根**

morgan 中间件根据指定的输出格式跟踪所有请求和其他重要信息。

```
npm install --save morgan
```

```
const logger = require('morgan');
// ... Configurations
app.use(logger('common'));
```

`common`是您可以在应用程序中使用的预定义格式案例。还有其他预定义的格式，如 tiny 和 dev，但您也可以使用 morgan 提供的字符串参数定义自己的自定义格式。

最常用的中间件功能列表可在此[链接](https://expressjs.com/en/resources/middleware.html)获得。

## **提供静态文件**

提供静态文件，如 CSS 样式表、图像等。Express 提供了一个内置的中间件功能`express.static`。静态文件是客户端从服务器下载的文件。

它是 Express framework 附带的唯一中间件功能，我们可以在应用程序中直接使用它。所有其他中间件都是第三方。

默认情况下，Express 不允许提供静态文件。我们必须使用这个中间件功能。开发 web 应用程序的一个常见做法是将所有静态文件存储在项目根目录下的“public”目录中。我们可以通过在我们的`index.js`文件中写入以下内容来为该文件夹提供静态文件:

```
app.use(express.static('public'));
```

现在，我们的公共目录中的静态文件将被加载。

```
http://localhost:3000/css/style.css
http://localhost:3000/images/logo.png
http://localhost:3000/images/bg.png
http://localhost:3000/index.html
```

### **多个静态目录**

要使用多个静态资产目录，请多次调用`express.static`中间件函数:

```
app.use(express.static('public'));
app.use(express.static('files'));
```

### **虚拟路径前缀**

固定路径前缀也可以作为第一个参数提供给`express.static`中间件函数。这被称为*虚拟路径前缀*，因为实际路径在项目中并不存在。

```
app.use('/static', express.static('public'));
```

如果我们现在尝试加载文件:

```
http://localhost:3000/static/css/style.css
http://localhost:3000/static/images/logo.png
http://localhost:3000/static/images/bg.png
http://localhost:3000/static/index.html
```

当提供多个目录来服务静态文件时，这种技术非常方便。前缀用于帮助区分多个目录。

## **模板引擎**

模板引擎是允许我们使用不同模板语言的库。模板语言是一组特殊的指令(语法和控制结构)，指示引擎如何处理数据。使用模板引擎很容易。流行的模板引擎如 Pug、EJS、Swig 和 Handlebars 都与 Express 兼容。但是，Express 附带了一个默认的模板引擎 Jade，这是 Pug 的第一个发布版本。

为了演示如何使用模板引擎，我们将使用 Pug。它是一个强大的模板引擎，提供了诸如过滤、包含、插值等功能。要使用它，我们必须首先使用`npm`作为一个模块安装在我们的项目中。

```
npm install --save pug
```

该命令将安装 pug 并验证安装是否正确，只需看一下`package.json`文件。要在我们的应用程序中使用它，首先我们必须将它设置为模板引擎，并创建一个新目录。/views ',我们将在其中存储与模板引擎相关的所有文件。

```
app.set('view engine', 'pug');
app.set('views', './views');
```

因为我们使用了表示服务器文件中配置的`app.set()`，所以我们必须在定义任何路由或中间件功能之前放置它们。

在`views`目录中，创建名为`index.pug`的文件。

```
doctype html
  html
    head
      tite="Hello from Pug"
    body
      p.greetings Hello World! 
```

为了运行这个页面，我们将在我们的应用程序中添加以下路由。

```
app.get('/hello', (req, res) => {
  res.render('index');
});
```

因为我们已经将 Pug 设置为我们的模板引擎，所以在`res.render`中我们不需要提供`.pug`扩展。该函数将任何`.pug`文件中的代码呈现为 HTML，供客户端显示。浏览器只能呈现 HTML 文件。如果您现在启动服务器，并访问路线`http://localhost:3000/hello`，您将看到输出`Hello World`被正确呈现。

在 Pug 中，你必须注意到我们不需要像在 HTML 中那样给元素写结束标签。上述代码将被转换成 HTML 格式，如下所示:

```
<!DOCTYPE html>
<html>
   <head>
      <title>Hello from Pug</title>
   </head>

   <body>
      <p class = "greetings">Hello World!</p>
   </body>
</html>
```

与原始 HTML 文件相比，使用模板引擎的优势在于，它们支持对数据执行任务。HTML 不能直接呈现数据。像 Angular 和 React 这样的框架与模板引擎有着相同的行为。

您也可以直接从路由处理器函数向模板引擎传递值。

```
app.get('/', (req, res) => {
  res.render('index', { title: 'Hello from Pug', message: 'Hello World!' });
});
```

对于上述情况，我们的`index.pug`文件将被写成:

```
doctype html
  html
    head
      title= title
    body
      h1= message
```

输出将与前一种情况相同。

## **一个快递 App 的项目结构**

由于 Express 并没有对使用它的开发人员进行太多的强制要求，所以有时候它会让人对应该遵循什么样的项目结构感到有点不知所措。它没有正式定义的结构，但任何基于 Node.js 的应用程序遵循的最常见的用例是在不同的模块中分离不同的任务。这意味着要有单独的 JavaScript 文件。

让我们来看一个基于 Express 的 web 应用程序的典型结构。

```
project-root/
   node_modules/          // This is where the packages installed are stored
   config/
      db.js                // Database connection and configuration
      credentials.js       // Passwords/API keys for external services used by your app
      config.js            // Environment variables
   models/                 // For mongoose schemas
      books.js
      things.js
   routes/                 // All routes for different entities in different files
      books.js
      things.js
   views/
      index.pug
      404.pug
        ...
   public/                 // All static files
      images/
      css/
      javascript/
   app.js
   routes.js               // Require all routes in this and then require this file in
   app.js
   package.json
```

这种模式就是通常所说的 MVC，模型-视图-控制器。原因很简单，因为我们的数据库模型、应用程序的 UI 和控制器(在我们的例子中是 routes)是在单独的文件中编写和存储的。这种设计模式使得任何 web 应用程序都易于扩展，如果您想在将来引入更多的路由或静态文件，并且代码是可维护的。