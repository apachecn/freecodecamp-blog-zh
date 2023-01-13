# 使用 Express.js 起步

> 原文：<https://www.freecodecamp.org/news/getting-off-the-ground-with-expressjs-89ada7ef4e59/>

作者维克多·奥弗格布

# 使用 Express.js 起步

#### 使用 Node.js 框架编写 web 应用程序

![1*ofMfrqFMfpaTH_RMvI_RQA](img/3bf34f7d760559ebf1b89dc2f2843455.png)

一个常见的关键时刻是当你开发许多需要相同或相同功能的应用程序时。现在很明显，复制和粘贴代码是不可伸缩的——你需要一个为灵活性、最小化和可重用性而设计的工具。

这就是 Express 在 MERN 堆栈中的位置。这是一个用于构建可伸缩 web 应用程序的 Node.js 框架。

在这篇文章中，我们将深入探讨使用 Express 框架构建 web 应用程序。我们将关注数据库集成、会话和 cookies、模板引擎以巩固我们的工作流程，最后关注生产和安全问题。

本教程要求您安装 Node.js 和 npm，并且您应该对 Node.js 和 JavaScript 有一个基本的了解。如果你不熟悉 Node.js，这里是我的最后一篇文章:[你唯一需要的 NodeJs 介绍](https://codeburst.io/the-only-nodejs-introduction-youll-ever-need-d969a47ef219?source=activity---post_recommended_rollup)。这是开始钻研 Node.js 的好地方。

### 我们做快递吧！

Express 由 TJ Holowaychuk 构建，他从 Ruby on Rails 框架 Sinatra 中获得了灵感。Express 的最新版本是 4.16.1。Express 4 通过放弃一些开发人员不经常使用的模块变得更加轻便。所以他们可以很容易地只在需要的时候进口。有道理对吗？

要开始使用 Express，您需要安装它并将其放入您的程序中。

1)创建一个名为`express-app`、`cd`的文件夹，然后点击`npm init`。这就创建了我们的`package.json`文件。

2)仍然在终端或命令行上，点击`npm install --save express`。这将在项目中安装 express。双划线`save`标志基本上意味着我们正在将它添加到我们的`package.json`文件中。

3)在我们的 express-app 文件夹的根目录下，创建一个`server.js`文件并将它复制到里面。

```
const express = require('express'),
      app = express();

app.get('/',(request,response)=>{
  response.send(‘Hello world’);
});

//Binding the server to a port(3000)
app.listen(3000,()=>console.log(‘express server started at port 300’));
```

const express = require('express ')，app = express()；app.get('/')，(request，response)= > { response . send(' Hello world ')；});//将服务器绑定到一个端口(3000)app.listen(3000，()= > console . log(' express server started at port 300 '))；

4)回到终端，仍然在同一个文件夹中，点击`node server.js`。打开浏览器，访问本地主机。

我们需要我们的快速模块。如果你很善于观察，你一定注意到我们不像在纯 Node.js 应用中那样需要`http`模块。那是因为 Express 要求的，所以我们不需要再那样做了。

当我们`require ('express’)`的时候，输出给我们的是一个函数，所以我们基本上可以直接调用它，并把它赋给`app`变量。在这一点上，在我们真正处理请求之前，什么都不会起作用。这叫做`routing`,意思是给客户他们想要的东西。

Express 给了我们一些做 HTTP 操作的基本动词(比如 GET、POST、PUT、DELETE)。在我们的例子中，我们使用了一个`app.get()`方法来处理对服务器的 get 请求。它有两个参数:请求的`path` 和处理请求的回调。

如果你没收到，我会解释更多。

路径是计算机上资源的地址。**回调是一个函数作为参数传递给另一个函数，在特定条件发生时被调用。**

你可能记得这个:

```
$('p').click(function(){
   console.log('You clicked me')
});
```

这里，我们添加了一个回调函数，以便在单击 p 时触发。看到了吗？很简单。

在`server.js`的最后一行，我们在启动服务器时监听端口 3000 和 console.log。

我打赌你不能用它写应用程序。让我们吃肉吧。

### 快速路由

路由意味着分配功能来响应用户的请求。快速路由器基本上是中间件(意味着它们可以访问`request`和`response`对象，并为我们做一些繁重的工作)。

Express 中的路由遵循以下基本格式:

```
app.VERB(‘path’, callback…);
```

**其中`VERB`是`GET`、`POST`、`PUT`和`DELETE`动词中的任意一个。**

我们可以添加任意多的回调函数。请看这里的例子:

```
const express = require('express'),
      app = express();

function sayHello(request,response,next){
  console.log(‘I must be called’);
  next();
}

app.get('/', sayHello, (request, response)=>{
  response.send('sayHello');
});

app.listen(3000,()=>console.log('Express server started at port 3000'));
```

在你的终端或命令提示符下点击`node server.js`。您将看到在响应被发送到浏览器之前，`sayHello`函数被触发。

`sayHello` 函数有三个参数(`request`、`response`和`next`)。

调用`next()`函数时，将控制转移到下一个中间件或路由。

#### 请求和响应对象

`request`对象包含关于传入请求的信息。以下是最有用的属性和方法:

`**request.params**` 变量存储一个名为 GET 请求参数的对象。例如，清除您的`server.js` 文件并将它粘贴到:

```
const express = require('express'),
      app = express();

app.get('/:name/:age', (request, response)=>{
   response.send(request.params);
});

app.listen(3000,()=>console.log(‘Express server started at port 3000’));
```

用`node server.js`运行这个，然后在你的浏览器上运行:`[https://localhost:3000/john/5](https://localhost:3000/john/5)`

在上面的代码中，我们从 URL 获取变量并将它们发送到浏览器。关键是`request.params` 是一个保存所有 GET 参数的对象。注意参数前的冒号。它们将布线参数与普通布线路径区分开来。

属性存储 POST 表单参数。

`**request.query**`属性用于提取获取表单数据。这里有一个例子:

1)创建另一个名为`express-query`的文件夹，然后创建两个文件:`server.js`和`form.html`。将此粘贴到`server.js`:

```
const express = require('express'),
      app = express();

//route serves both the form page and processes form data
app.get('/', (request, response)=>{
  console.log(request.query);
  response.sendFile(__dirname +'/form.html');
});

app.listen(3000,()=>console.log('Express server started at port 3000'));
```

2)将其复制到`form.html`文件中:

```
<!--action specifies that form be handled on the same page-->
<form action='/' method='GET'>
  <input type='text' name='name'/>
  <input type='email' name='email'/>
  <input type='submit'/>
</form>
```

用`node server.js`运行代码，点击`[localhost:3000](http://localhost:3000)`，在浏览器中填写并提交表单。提交表单后，您填写的数据会被记录到控制台。

`**request.headers**` 保存服务器收到的请求的密钥/对值。服务器和客户机利用头来交流它们的兼容性和约束。

`**request.accepts([‘json’,’html’])**`保存一个数据格式数组，并返回浏览器收集数据时首选的格式。在处理 Ajax 表单时，我们也会看到这一点。

`**request.url**` 存储关于请求的 URL 的数据。

`**request.ip:**` 保存请求信息的浏览器的 IP(互联网协议)地址。

`response`对象还附带了一些方便的方法来获取关于传出请求的有用信息。

`**response.send(message)**` 将其参数发送给浏览器。

`**response.json()**` 以 JSON 格式将其参数作为数据发送给浏览器。

`**response.cookie(name,value,duration)**` 提供了在浏览器上设置 cookies 的接口。我们也会谈到饼干。

`**response.redirect([redirect status], url)**`将浏览器重定向到具有可选状态的指定 URL。

向浏览器呈现一个视图，并获取一个包含路由器可能需要的数据的对象。当我们看风景的时候你会更好地理解它。

`**response.set(name,value)**` 设置表头数值。但是你不想这样做，因为这会妨碍浏览器的工作。

`**response.status(status)**`设置特定响应的状态(404、200、401 等)。

这些不用都背下来。当我们使用它们时，你会下意识地掌握它们。

### 快速路由器

使用 Express Router，我们可以将我们的应用程序分成片段，这些片段可以有自己的 Express 实例来处理。我们可以用一种非常简洁和模块化的方式将它们整合在一起。

让我们举个例子。这四个随机的网址:

本地主机:3000/用户/约翰

本地主机:3000/用户/马克

本地主机:3000/posts/newpost

本地主机:3000/api

对于 Express，我们通常的做法是:

```
const express = require('express'),
      app = express();

//Different routes
app.get('/users/john',(request,response)=>{
    response.send(`John’s page`);
});

app.get('/users/mark',(request,response)=>{
    response.send(`John’s page`);
});

app.get('/posts/newpost',(request,response)=>{
  response.send(`This is a new post`);
});

app.get('/api',(request,response)=>{
  response.send(‘Api endpoint’);
});

app.listen(3000,()=>console.log(‘Server started at port 3000’));
```

这种模式在这一点上没有错。但是它有可能出错。当我们的路线基本上只有五条时，没有太大的问题。但是当事情发展到需要大量功能时，将所有代码放入我们的`server.js` 中并不是我们想要做的事情。

#### **我们应该让路由器为我们做这件事**

在我们项目的根目录下创建一个名为`react-router`的文件夹，在里面创建一个文件，命名为`basic_router.js`。将此权利复制到:

```
const express = require('express'),
      router = express.Router();

//making use of normal routes
router.get('/john',(request,response)=>{
  response.send('Home of user');
});

router.get('/mark',(request,response)=>{
  response.send('Home of user');
});

//exporting thee router to other modules
module.exports = router;
```

我们基本上是在创建另一个 Express 实例。这一次，我们调用 Express 的`Router()`函数。可以直接调用`express()`作为函数(如我们的`**server.js**`)，也可以调用`express.Router()`。这是因为 Express 是作为函数导出的，而在 JavaScript 中，函数也是对象。

我们像普通的快递应用程序一样给它添加路线。但是我们不绑定任何端口。`router`对象包含我们定义的所有路线，所以我们只导出该对象，这样程序的其他部分也可以使用它。

我们创建我们的 main `server.js`，并将其作为中间件添加。是的，中间件使工作更容易，记得吗？

```
const express = require('express'),
      app = express();

//requiring the basic_router.js
app.use('/users',require('.react-router/basic_router'));

//routes
app.get('/posts/newpost',(request,response)=>{
  response.send('new post');
});

app.get('/api',(request,response)=>{
  response.send('API route');
});

app.listen(3000,()=>console.log('Express server started at port 3000'));
```

现在运行这个。导航至`localhost:3000/user/john`和`localhost:3000/user/mark`。看到了吗？在这一点上，事情很容易归类。

我们可以对每条其他路线都这样做。为我们的 API 创建另一个文件。我们就叫它`api_route.js`。将此权利复制到:

```
const express = require('express'),
      router = express.Router();

router.get('/',(request,response)=>{
  response.send('Home of user');
});

//some other endpoints to submit data
module.exports = router;
```

现在，回到我们的`**server.js**`，将代码改为:

```
const express = require('express'),
      app = express();

app.use('/users',require('./basic_router'));

app.use('/api',require('./api_route'));

app.get('/posts/newpost',(request,response)=>{
   response.send('new post');
});

app.listen(3000,()=>console.log('Express server started at port 3000'));
```

这些信息足以构建基本的 web 应用程序路线。

### 模板引擎

![1*2jgQ7PxyWZKsORaDoWe6_w](img/915c000369aa5869eb135c868252975d.png)

Photo by [Michael Mroczek](https://unsplash.com/photos/JaWmkrSFR2Q?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/engines?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

大多数时候，你并不确定你希望你的网站有多少页面。这意味着你希望保持事物的灵活性、可重用性和整洁性。

假设您有一个可能要在每一页上使用的页脚。如果你只是把它放在一个文件里，并在每一页上嵌入一行代码，那不是很酷吗？或者你希望失去网址上的`.html`？

这些只是模板引擎可以为我们做的一些事情。

目前有很多模板引擎。但是我们将使用把手来看看模板是如何工作的。幸运的是，相同的原则适用于几乎所有的模板引擎——只是语法发生了变化。

为了利用车把，我们安装了它。

```
npm install --save express-handlebars
```

`require`将它保存在您的文件中，并配置您的应用程序，如下所示:

```
const express = require('express'),
      hbs = require('express-handlebars').create({defaultLayout:'main.hbs'}),
      app = express();

app.engine('hbs', hbs.engine);
app.set('view engine','hbs');
```

所以让我们用车把做一个基本的渲染:

1.  创建一个名为`express-handlebars`的文件夹，创建一个`views`文件夹，并在`views` 文件夹中创建另一个名为`layouts`的文件夹。

2)创建一个`server.js`文件，并将其粘贴到:

```
const express = require('express'),
      hbs = require('express-handlebars').create({defaultLayout:'main.hbs'}),
      app = express();

//setting our app engine to handlebars
app.engine('hbs', hbs.engine);
app.set('view engine', 'hbs');
app.get('/',(request,response)=>{
  response.render('home',{title: 'Home'});
});

app.get('/about',(request,response)=>{
  response.render(‘about’,{title: ‘About’});
});

app.listen(3000,()=>console.log('Express server started at port 3000'));
```

3)在`layouts`文件夹中，创建一个文件`main.hbs`。将此粘贴到内部:

```
<!-- The main.hbs file will act as a default template for every view on the site -->

<!DOCTYPE html>
<html>
<head>
<meta charset='UTF-8'>

<!-- The title variable will be replaced with the title of every page -->

<title>{{title}}</title>
</head>

<body>
<!-- Content of other pages will replace the body variable -->
{{{body}}}
</body>
</html>
```

4)接下来，我们将创建单独的视图。在`views`文件夹中，创建两个文件——`home.hbs`和`about.hbs`。在`home.hbs`内粘贴以下内容:

```
//home.hbs
<!-- This is the Home view and will render into the main.hbs layout -->

<div>
  Hello, I’m the Home page and you’re welcome
</div>
```

而在我们的`about.hbs`:

```
//about.hbs
<!-- This is the About view and will also render into the main.hbs layout -->

<div>
  Hello, I’m the about page, what do you want to know about
</div>
```

在你的终端上做一个`node server.js`，在你的浏览器上点击`[http://localhost:3000](http://localhost:3000)`。

这里发生了什么事？

我们首先需要`express-handlebars`并创建一个`defaultLayout`，将其分配给`main.hbs`。这意味着我们所有的视图都将呈现在`main.hbs`布局中。

看一看`server.js`。只是一些事情改变了，对吗？让我们从这两行开始:

```
app.engine('hbs', hbs.engine);
app.set(‘view engine’,’hbs’);
```

第一行将应用引擎设置为`hbs.engine` ，第二行将视图引擎属性设置为 handlebars。很简单，对吧？

我们`server.js`里的路线也有点不一样。罪魁祸首在这里:

```
response.render('home',{title: 'Home'});
```

当`.send()`向浏览器发送纯文本时，`render()`在`views` 文件夹中查找第一个参数，并将其呈现给浏览器。大多数时候，我们可能也想将动态内容传递给视图。我们给 render 方法一个对象作为第二个参数。该对象包含要在视图内部传递的数据的键和值。

将这一行放到我们布局文件夹的`main.hbs`文件中。

```
//main.hbs
<title>{{title}}</title>
```

用视图传递的任何内容替换`{{title}}`。在我们的例子中，`{title: 'Home'}`。我们可以向视图传递任意多的值。只是把它作为对象的一个属性添加进去。

当我们做一个`response.render()`的时候，Express 知道从哪里得到我们要的文件。让我们来看看`about.hbs`。

```
<!-- This is the About view and will render into the main.handlebars layout -->
<div>
  Hello, I’m the about page, what do you want to know about
</div>
```

这个文件的内容替换了我们的`layout.handlebars`中的 body 变量:

```
{{{body}}}
```

如果你问我们为什么对{{title}}使用两个大括号，对{{{body}}}使用三个大括号，那你就对了。

当我们使用两个大括号时，我们把所有东西都吐出来了，甚至是 HTML 标签(未转义的)。我的意思是。

如果我们要发送给浏览器的内容是`<b>Hello wor` ld < /b >，用两个大括号，Express 会 r`ender it as <b&g`t；你好世界< /b >。如果我们使用三个大括号，Express 将理解我们想要一个 bold**d 文本，并将其呈现为 Hello world(粗体)。**

这就是模板引擎在 Express 中的基本工作方式。[车把](http://handlebarsjs.com/)提供了一页的文档。我认为这是一个良好的开端。

### 在 express 中呈现静态内容

你有没有想过我们应该把 CSS、JavaScript 文件和图片存放在哪里？Express 提供了一个中间件，让服务器知道在哪里可以找到静态内容。

下面是如何利用它:

```
app.use(express.static(__dirname +'public'));
```

把这个放在你的`server.js`的顶部，就在 require 语句之后。`__dirname` 保存程序运行的路径。

如果你不明白，试试这个。

删除您`server.js`上的所有内容，并将此内容放入:

`console.log(__dirname);`

回到你的命令行，运行`node server.js`。它显示了正在运行的文件节点的路径。

我们在哪里存储我们的静态内容取决于我们自己。我们可能想把它命名为`assets`或别的什么，但是你必须确保你把它附加到`dirname`后面，就像这样:

```
express.static(__dirname + ‘static_folder_name’).
```

### 快速中间件

中间件是封装功能的功能。它们对 HTTP 请求执行操作，并给我们一个高级接口来定制它们。大多数中间件采用三个参数:**请求**，**响应**对象，以及一个**下一个**函数。在错误处理中间件中，有一个额外的参数: **err** 对象，它可以告诉我们错误，并让我们将其传递给其他中间件。

我们通过使用`**app.use(name_of_middleware)**`向我们的服务器添加中间件。同样重要的是要注意中间件的使用顺序与它们被添加的顺序相同。如果你不明白，我稍后给你举个例子。

有了这个定义，我们也可以把像`app.get()`、`app.post()`等路由函数看作中间件，只不过它们适用于特定的 HTTP 动词请求。您可能还会觉得有趣的是，有一个`app.all()`路由适用于所有 HTTP 请求，而不考虑它们是 GET、POST 还是其他请求。

```
//This middleware will be called for every request. GET or POST
app.all((request,response)=>{
  console.log('Hello world');
})
```

路由处理程序有两个参数，要匹配的路径和要执行的中间件。

```
app.get('/',(request,,response)=>{
  response.send(‘Hello world’);
});
```

如果忽略了路径，中间件将应用于每个 GET 请求。

```
//Every GET request will call this middleware
app.get((request,response)=>{
  response.send(‘Hello world’);
});
```

在我们上面的例子中，一旦我们发送了一个`GET`请求，我们的服务器通过发送一个`‘Hello world’`消息来响应浏览器，然后终止，直到出现另一个请求。

但是我们可能希望调用不止一个中间件。有一种方法可以做到这一点。还记得我们的`next` 函数吗？我们可以利用它将控制权推给另一个中间件。

让我们看看这是如何工作的。将这段代码复制并粘贴到我们的`server.js:`

```
const express = require('express'),
      app = express();

//setting the port
app.set(‘port’, process.env.PORT || 3000);

//first middleware
app.use((request,respone,next)=>{
  console.log(`processing for data for ${request.url}`);
  next();
});

//second middleware
app.use((request,response,next)=>{
  console.log(`The response.send will terminate the request`);
response.send(`Hello world`);
});

//third middleware
app.use((request,respone,next)=>{
  console.log(`I’ll never get called`);
});

app.listen(3000,()=>console.log('Express server started at port 3000'));
```

在终端上，点击`node server.js`并查看终端。打开你的浏览器，打开`localhost:3000`。再次查看您的控制台，您会看到类似的内容。

```
Express server started at port 3000
processing for data for /
The response.send will terminate the request
```

我们的第一个中间件在每次收到请求时执行。它向控制台写一些东西，并调用`next()`函数。调用`next()`函数告诉 Express 不要终止`request`对象，而是将控制发送给下一个中间件**。每当我们编写一个没有调用`next`函数的中间件时，Express 就会终止`request`对象。**

在第二个中间件中，我们将`next()`函数作为参数传递，但我们从不调用它。这终止了`request`对象，第三个中间件永远不会被调用。注意，如果我们没有向第二个中间件中的浏览器发送任何东西，客户端最终会超时。

以下是 Express.js 中一些有用的中间件:

*   [摩根](https://github.com/expressjs/morgan) —记录每个请求
*   [CORS](https://github.com/expressjs/cors) —支持跨来源请求共享
*   [body-parser](https://github.com/expressjs/body-parser) —解析 Express 应用中`request.body`的中间件
*   [Multer](https://github.com/expressjs/multer) —用于处理`multipart/form-data`的 Node.js 中间件
*   [会话](https://github.com/expressjs/session)—express . js 的简单会话中间件
*   [错误处理程序](https://github.com/expressjs/errorhandler) —仅用于开发的错误处理程序中间件
*   [serve-favicon](https://github.com/expressjs/serve-favicon) — favicon 服务中间件
*   [csurf](https://github.com/expressjs/csurf) — Node.js CSRF 保护中间件
*   [护照](http://www.passportjs.org/) —简单、低调的身份验证
*   一个 RESTful 友好的快速中间件，用于 HTTP 错误处理和错误响应
*   [Expressa](https://github.com/thomas4019/expressa) —轻松制作 REST APIs 的 express 中间件

### 在 Express 中处理表单数据

网络的主要功能是交流。Express 为我们提供了了解客户需求以及如何正确回应的工具。

Express 基本上有两个地方存储客户端数据。 **request.querystring(用于 GET 请求)**和 **request.body(用于 POST 请求)**。在客户端，使用 POST 方法提交表单是最理想的，因为大多数浏览器都限制了`querystring`的长度，额外的数据会丢失。如果您不知道什么是查询字符串，那么它是 URL 后面包含数据的部分，不适合您的路由路径系统。如果你不太明白什么是查询字符串，这里有一个例子:

```
facebook.com/users?name=Victor&age=100&occupation=whatever
```

从问号开始的点称为查询字符串。它将数据传递给服务器，但在 URL 中公开数据。

尽可能保持查询字符串的整洁也是一个好习惯。用 GET 请求发送大型数据会使查询字符串变得混乱。

让我们看一个演示。我们将通过 GET 从我们的客户那里获取一些数据，并发送给他们。

创建一个文件夹，命名为`form-data`，里面创建两个文件:`server.js`和`form.html`。分别粘贴到`server.js`文件和`form.html`文件中:

```
//server.js file

const express = require('express'),
      app = express();

//setting the port 
app.set('port', process.env.PORT || 3000);

//
app.get('/',(request,response)=>{
  response.sendFile(__dirname +'/form.html');
});

app.get('/process',(request,response)=>{
  console.log(request.query);
  response.send(`${request.query.name} said ${request.query.message}`);
});

app.listen(3000,()=>{
  console.log('Express server started at port 3000');
});
```

```
//form.html

<!DOCTYPE html>
<html>
  <head>
    <meta charset='UTF-8'>
    <title>Form page</title>
    <style>
      *{
        margin:0;
        padding:0;
        box-sizing: border-box;
        font-size: 20px;
      }
      input{
        margin:20px;
        padding:10px;
      }
      input[type=”text”],
      textarea {
        width:200px;
        margin:20px;
        padding:5px;
        height:30px;
        display: block;
      }
      textarea{
        resize:none;
        height:60px;
      }
    </style>
  </head>
<body>
  <form action='/process' method='GET'>
    <input type='text' name='name' placeholder='name'/>
    <textarea name='message' placeholder='message'></textarea>
    <input type='submit'/>
  </form>
</body>
</html>
```

运行`node server.js`，前往`localhost:3000`，填写表格并提交。

这是结果的样子。

在我们这里的`server.js`文件中，我们有两条 GET 路线。一个给`localhost:3000`和`localhost:3000/process`。

```
app.get(‘/’,(request,response)=>{
   response.sendFile(__dirname + ‘/form.html’);
});
```

和

```
app.get(‘/process’,(request,response)=>{
  response.send(`${request.query.name} said ${request.query.message}`);
});
```

去你的控制台。你会看到一个物体。这证明了我们的`request.query`是一个包含所有查询及其值的对象。

```
{
  name: 'victor',
  message: 'Hello world'
}
```

如果你在`form.html`页面看一下我们的表单，你会注意到我们的表单有`action`和`method`属性。`action`属性指定应该处理表单数据的页面或路径(在本例中为“process”)。当表单提交后，它向`process`路由发送一个 GET 请求，将表单内容作为`querystring`数据。

我们的`server.js`文件还处理对流程路径的请求，并将从我们的`form.html`传递的数据发送到浏览器和控制台。

让我们看看如何用 POST 方法处理这个问题。是时候清理我们的`server.js`文件了。将这段代码复制并粘贴到`server.js`中:

```
//server.js

const express = require('express'),
      app = express(),

//You must require the body-parser middleware to access request.body in express
bodyParser = require('body-parser');

//configuring bodyparser
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));

//setting our port
app.set('port', process.env.PORT || 3000);

//Get route for localhost:3000
app.get('/',(request,response)=>{
  response.sendFile(__dirname +'/form.html');
});

//POST route for form handling
app.post('/',(request,response)=>{
  console.log(request.body);  
  response.send(`${request.body.name} said ${request.body.message}`);
});

app.listen(3000,()=>{
  console.log('Express server started at port 3000');
});
```

```
//form.html file

<!DOCTYPE html>
<html>
  <head>
  <meta charset='UTF-8'>
  <title>Form page</title>
    <style>
      *{
        margin:0;
        padding:0;
        box-sizing: border-box;
        font-size: 20px;
      }
      input{
        margin:20px;
        padding:10px;
      }
      input[type='text'],
      textarea {
        width:200px;
        margin:20px;
        padding:5px;
        height:30px;
        display: block;
      }
      textarea{
        resize:none;
        height:60px;
      }
    </style>
  </head>
<body>
  <form action='/' method='POST'>
    <input type='text' name='name' placeholder='name'/>
    <textarea name='message' placeholder='message'></textarea>
    <input type='submit'/>
  </form>
</body>
</html>
```

Using the POST method

如果你仔细观察，我们正在做的第一件不同的事情是要求和使用`body-parser`。Body-parser 是一个中间件，它使 POST 数据在我们的`request.body`中可用。请记住，没有主体解析器中间件,`request.body` 就无法工作。

您可能还注意到我们有 GET 和 POST 路由处理程序。GET 中间件显示表单，POST 中间件处理表单。因为它们有不同的方法，所以它们都可以使用一个路由路径。

我们不能在第一个例子中这样做，因为我们的 form 方法是 GET。显然，您不能对同一路由发出两个 GET 请求，并让它们都向浏览器发送数据。这就是为什么我们的第一个例子在`/process`路径上处理表单。

### 处理 AJAX 表单

用 Express 处理 Ajax 表单非常简单。Express 为我们提供了一个`request.xhr`属性，告诉我们请求是否通过 AJAX 发送。我们可以将它与我们之前谈到的`request.accepts()`方法结合起来。它帮助我们确定浏览器想要的数据格式。如果客户喜欢 JSON，我们就给它 JSON。

让我们修改我们的`form.html`来使用 AJAX，修改我们的`server.js`来接受 AJAX 并发送 JSON。

```
<!DOCTYPE html>
<html>
  <head>
  <meta charset='UTF-8'>
  <title>Form page</title>
    <script src='https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.js'></script>
  <style>
    *{
      margin:0;
      padding:0;
      box-sizing: border-box;
      font-size: 20px;
    }
    input{
      margin:20px;
      padding:10px;
    }
    input[type=”text”],
    textarea {
      width:200px;
      margin:20px;
      padding:5px;
      height:30px;
      display: block;
    }
    textarea{
      resize:none;
      height:60px;
    }
  </style>
  </head>
<body>
  <div></div>

  <form action='/' method='POST'>
    <input type='text' name='name' placeholder='name'/>
    <textarea name='message' placeholder='message'></textarea>
    <input type='submit'/>
  </form>

  <script>
    $('form').on('submit',makeRequest);
      function makeRequest(e){
        e.preventDefault();
        $.ajax({
          url:'/',
          type : 'POST',
          success: function(data){
            if(data.message){
              $('div').html(data.message);
            } else {
              $('div').html('Sorry, an error occured');
            }
          },
          error: function(){
            $('div').html('Sorry, an error occurred');
          }
      });
    }
  </script>
  </body>
</html>
```

```
//server.js

const express = require('express'),
      app = express(),
      bodyParser = require('body-parser');

//configuring the body-parser middleware
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));

//setting our app port
app.set('port', process.env.PORT || 3000);

//Route for  get requests.
app.get('/',(request,response)=>{
  response.sendFile(__dirname +'/form.html');
});

//Route to handle POST form requests.
app.post('/',(request,response)=>{
//we check if the request is an AJAX one and if accepts JSON
  if(request.xhr || request.accepts('json, html')==='json'){
    response.send({message:'Just wanted to tell you. It worked'});
   } else {
    //Do something else by reloading the page.
   }
});

app.listen(3000,()=>{
  console.log('Express server started at port 3000');
});
```

#### **这是如何工作的**

这里没有太多变化——我们只是添加了一种方法来检查请求是否是用 AJAX 发出的。

所以我们要做的是。我们用 POST 方法提出了一个 AJAX 请求。我们链接到了 jQuery CDN。在脚本标记中，我们为提交事件附加了一个事件处理程序。当我们这样做时，我们防止了重新加载页面的默认行为。

然后，我们使用 jQuery `$.ajax()`方法发出一个 AJAX 请求。服务器用一个带有`message`属性的对象来响应，然后我们将它添加到空的 div 中。

如果你不熟悉 AJAX，我曾经写过一些关于 AJAX 的文章。查看它们:[AJAX 简介](https://codeburst.io/a-gentle-introduction-to-ajax-1e88e3db4e79)和[用 jQuery 实现更简单的异步请求。](https://codeburst.io/easier-asynchronous-requests-with-jquery-80507b502e62)

### Node.js 应用程序中的数据库

MongoDB 和 CouchDB 是一些适合 Node.js 应用程序的数据库系统。这并不完全排除使用其他数据库的可能性。我们会看看 MongoDB，但是你可以选择你喜欢的任何一个。

文档取代了 MySQL 等关系数据库中的行。在 MongoDB 和其他基于文档的数据库中，数据以对象格式存储和检索。这意味着我们可以拥有深度嵌套的结构。

如果考虑 JavaScript 中的对象，就没有办法验证对象属性的值是否是特定的类型。我的意思是:

`const obj = { text : 1234}`

没有办法确定`text`的值是一个字符串。

幸运的是，有猫鼬。Mongoose 允许您定义强有力地验证数据的模式，并确保它们与 MongoDB 中的对象或文档相匹配。Mongoose 是一个对象文档映射器(ODM)。

猫鼬简介是一个开始探索和研究猫鼬的好地方。

### Express 中的会话和 Cookies

HTTP 是无状态的。这意味着由浏览器或服务器分别发送的任何请求或响应不保持关于先前或未来请求和响应的信息(状态)。每一个请求都可以引发新的服务器响应。

但是必须有一种方法让服务器在用户浏览网站时记住他们，这样他们就不必在每一页都输入密码。

网络已经足够创新，可以利用 cookies 和会话。Cookies 基本上是存储在客户端机器上的小文件。当客户端发送请求时，服务器用它来识别它们。更像是护照，服务器知道是他们，并应用他们所有的偏好。

因此，我们的想法是将文件存储在客户端的机器上。虽然这不是一个坏主意，但我们希望确保我们不会通过存储大量数据来滥用用户的存储空间。另一方面，我们明白，如果我们想让事情变得更难猜测、更安全，我们就会让事情变得更长、更复杂。如何才能同时实现这两者？

人们想出了会话。因此，会话的思想是，服务器在 cookie 中存储一个标识符(一个小字符串)，而不是将所有信息存储在客户机的 cookie 中。当客户端发送请求时，服务器获取这个唯一的字符串，并将其与服务器上的用户数据进行匹配。这样，我们可以存储任意数量的数据，同时还能记住用户。

为了在 Express 中使用 cookies，我们需要使用`cookie-parser`中间件。记得我们的中间件吗？

我不适合深入解释这件事。但是有人在这里做得更好:[快速会话](https://glebbahmutov.com/blog/express-sessions/)。

### 快速应用中的安全性

默认情况下，网站是不安全的。数据包是数据在网络上传送的方式。默认情况下，这些数据包是未加密的。当我们考虑网络安全时，首先要做的就是保护这些数据包。

HTTPS:这不是一个新词！正如您可能已经猜到的那样，HTTP 和 HTTPS 的区别在于 S(安全性)。HTTPS 对通过网络传输的数据包进行加密，这样人们就不会用它来做恶意的事情。

#### 那么，我怎样才能得到 HTTPS 呢？

冷静，让我们慢慢来。要获得 HTTPS，您需要联系证书颁发机构(CA)。HTTPS 基于拥有公钥证书的服务器，有时称为 SSL。ca 将证书分配给合格的服务器。您还必须了解，ca 会生成根证书，这些证书会在您安装浏览器时安装。所以浏览器也可以很容易地与有证书的服务器通信。

好消息:任何人都可以制作自己的证书。

坏消息:浏览器无法识别这些证书，因为它们不是作为根证书安装的。

**不可能**:你不可能在安装的时候配置世界上所有的浏览器都认可你的证书。

我知道你现在在想什么。您认为您可以为测试和开发创建自己的证书，并在生产中获得一个。嗯，这很聪明，也有可能。

浏览器会给你警告，但是你知道这个问题，所以不会有太大的问题。这里有一篇[帖子](https://deliciousbrains.com/https-locally-without-browser-privacy-errors/)，带你创建自己的证书。

如何在本地设置 HTTPS 而不出现恼人的浏览器隐私错误
[*在本地设置 HTTPS 可能是件棘手的事情。即使您设法将自签名证书转换成…*](https://deliciousbrains.com/https-locally-without-browser-privacy-errors/)
[deliciousbrains.com](https://deliciousbrains.com/https-locally-without-browser-privacy-errors/)

说够了。让我们假设您现在拥有 SSL 证书。以下是如何让它与您的 Express 应用程序一起工作。

#### 在节点应用程序中启用 HTTPS

我们需要为 HTTPS 使用`https`模块。从证书颁发机构获得我们的凭证后，我们将把它作为一个参数包含到`**createServer()**` 方法中。

```
//server.js

const express = require('express'),
	  https = require('https'),
	  http = require('http'),
	  fs = require('fs'),
	  app = express();

//credentials obtained from a Certificate Authority
var options = {
  key: fs.readFileSync('/path/to/key.pem'),
  cert: fs.readFileSync('/path/to/cert.pem')
};

//Binding the app on different ports so the app can be assessed bt HTTP and HTTPS
http.createServer(app).listen(80);
https.createServer(options, app).listen(443);
```

请注意，我们需要`http`和`https.`，这是因为我们希望对两者都做出响应。在我们的程序中，我们使用了`fs`模块(文件系统)。

我们基本上提供了存储 SSL 密钥和证书的路径。一个模块什么的。注意，我们正在使用`**readFileSync**` 方法，而不是`**readFile**`。如果您理解 Node.js 架构，您将会推断出我们希望在运行任何其他代码行之前同步读取该文件。

异步运行这段代码可能会导致需要文件内容的代码不能及时得到。

最后两行将 HTTP 和 HTTPS 绑定到两个不同的端口，并采用不同的参数。我们为什么要这么做？

大多数时候，我们希望我们的服务器仍然监听 HTTP 请求，并可能将它们重定向到 HTTPS。

**注意**:HTTPS 的默认端口是 443。

为了完成这个基本的重定向，我们将在程序的顶部安装并要求一个模块`express-force-ssl`,如下所示:

```
npm install express-force-ssl
```

并像这样配置:

```
const express_force_ssl = require('express-force-ssl');
app.use(express_force_ssl);
```

现在，我们的服务器可以有效地处理 HTTP 和 HTTPS 请求。

#### 跨站点请求伪造(CSRF)

这是你想要保护自己免受的另一件大事。当请求到达您的服务器而不是直接来自您的用户时，就会发生这种情况。例如，您有一个关于 Facebook.com 的活动会话，并且打开了另一个选项卡。恶意脚本可以在另一个网站上运行，并向脸书的服务器发出请求。

处理这个问题的一个方法是确保请求只来自你的网站。那很容易。我们为用户分配一个 ID，并将其附加到表单上，这样当他们提交表单时，我们可以匹配 ID，如果不匹配，就拒绝访问。

幸运的是，有一个中间件可以处理这个问题— `csurf`中间件。下面是它的使用方法:

```
npm install csurf
```

在我们的程序中使用它:

```
const express = require('express'),
	  body_parser = require('body-parser'),
	  hbs = require('express-handlebars').create({defaultLayout: 'main',extname:'hbs'});
	  session = require('express-session'),
	  csurf = require('csurf'),

	  app = express();

//setting the app port
app.set('port', process.env.PORT || 3000);

//configuring the app for handlebars
app.engine('hbs', hbs.engine);
app.set('view engine', 'hbs');

//setting up a session csrf
app.use(session({
	name: 'My session csrf',
	secret: 'My super session secret',
	  cookie: {
	  	maxAge: null,
	    httpOnly: true,
	    secure: true
	  }
	})
  );

app.use(csurf());

//configuring the body parser middleware
app.use(body_parser.urlencoded());

//Route to login
app.get('/login', (request,response)=>{
	console.log(request.csrfToken());
	response.render('login',{
		csrfToken : request.csrfToken(),
		title: 'Login'
	});
});

app.listen(3000,()=>console.log('Express app started at port 3000'));
```

```
<b>Here's the generated csrf token</b> ({{csrfToken}})<br><br>

<form method='POST' action='/process'>
  <!-- We pass the _csrf token as a hidden input -->
  <input type='hidden' name='_csrf' csurf={{csrfToken}}/>
  <input type='text' name='name'/>
  <input type='submit'/>
</form>
```

运行`node server.js`，进入浏览器`localhost:3000`，填写表格并提交。另外，检查您的命令行并查看记录的令牌。

我们正在做的是生成`csrfToken`并将其传递给我们的登录视图。

**注意:**`csurf`模块需要`express-session`模块才能工作。我们配置我们的会话 CSRF，并通过`response.render()`方法将其传递给视图。

我们的视图现在可以将它附加到表单或任何其他敏感的请求中。

那么，当浏览器没有从浏览器表单中获取 CSRF 令牌时会发生什么呢？它抛出一个错误。确保您的 Express 应用程序中有一个错误处理路径，否则您的应用程序可能会出现问题。

#### 证明

减少认证问题的一个步骤是让人们注册并登录第三方应用程序(脸书、Twitter、谷歌、+等等)。很多人都有这些账户，你也可以访问他们的一些数据，比如电子邮件和用户名。像`passport.js`这样的模块提供了一个非常优雅的接口来处理这样的认证。

这里是[官方 passport.js 文档](http://www.passportjs.org/docs/)。我认为这是一个很好的开始。

减少认证问题的另一个步骤是始终加密所有密码，并在向用户显示密码时解密。

还有一件事。我在很多网站上都看到这个。他们在网站上为用户的密码设置了疯狂的标准。我知道他们想让密码更安全，但是想想吧。这是谁的工作？开发商还是用户？

用户应该最不担心安全问题。当像这样的标准被设置在密码上时，用户别无选择，只能使用他们永远不会记住的密码。我知道网络正在变得越来越好，我们会想出办法让认证变得更好。

在那之前，我想我们可以在这里结束这一切。

这是大量的信息。但是要构建可伸缩的 web 应用程序，您需要的不止这些。这里有一些有见地的书籍，可以让你了解更多关于 Express 的知识。

如果这有用，你应该在 Twitter 上关注我，获取更多有用的信息。

1.  [用行动表达](https://www.amazon.com/Express-Action-Writing-building-applications/dp/1617292427)作者[埃文·哈恩](https://www.freecodecamp.org/news/getting-off-the-ground-with-expressjs-89ada7ef4e59/undefined)。
2.  [由](https://www.amazon.com/Getting-MEAN-Mongo-Express-Angular/dp/1617294756)[西蒙·霍尔姆斯](https://www.freecodecamp.org/news/getting-off-the-ground-with-expressjs-89ada7ef4e59/undefined)用 Express、Mongo、Angular 和 Node 来表示意思。