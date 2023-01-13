# Express + Node.js 手册——初学者学习 Express JavaScript 框架[2022 版]

> 原文：<https://www.freecodecamp.org/news/the-express-handbook/>

## 什么是快递？

Express 是一个基于 Node.js 构建的 Web 框架。

Node.js 是构建网络服务和应用程序的神奇工具。

Express 在其特性的基础上构建，提供易于使用的功能，满足 Web 服务器用例的需求。它是开源的，免费的，易于扩展，性能非常好。

也有很多很多预构建的包，您可以直接使用它们来做各种事情。

[您可以获得本快递手册的 PDF 和 ePub 版本](https://thevalleyofcode.com/download/express/)

## 目录

*   [如何安装 Express](#how-to-install-express)
*   [第一个“你好，世界”的例子](#the-first-hello-world-example)
*   [请求参数](#request-parameters)
*   [如何向客户端发送响应](#how-to-send-a-response-to-the-client)
*   [如何发送 JSON 响应](#how-to-send-a-json-response)
*   [如何管理 cookie](#how-to-manage-cookies)
*   [如何使用 HTTP 头](#how-to-work-with-http-headers)
*   [如何处理重定向](#how-to-handle-redirects)
*   [快递路线](#routing-in-express)
*   [Express 中的模板](#templates-in-express)
*   [快递中间件](#express-middleware)
*   [如何使用 Express 服务静态资产](#how-to-serve-static-assets-with-express)
*   [如何向客户端发送文件](#how-to-send-files-to-the-client)
*   [Express 中的会话](#sessions-in-express)
*   [如何在 Express 中验证输入](#how-to-validate-input-in-express)
*   [如何清理 Express 中的输入](#how-to-sanitize-input-in-express)
*   [如何在快递中处理表单](#how-to-handle-forms-in-express)
*   [如何在 Express 中处理表单文件上传](#how-to-handle-file-uploads-in-forms-in-express)

## 如何安装 Express

您可以使用 npm 将 Express 安装到任何项目中。

如果您在一个空文件夹中，首先使用以下命令创建一个新的 Node.js 项目:

```
npm init -y 
```

然后运行以下命令:

```
npm install express 
```

将 Express 安装到项目中。

## 第一个“Hello，World”示例

我们要创建的第一个例子是一个简单的 Express Web 服务器。

复制此代码:

```
const express = require('express')
const app = express()

app.get('/', (req, res) => res.send('Hello World!'))
app.listen(3000, () => console.log('Server ready')) 
```

将它保存到项目根文件夹中的一个`index.js`文件中，并使用以下命令启动服务器:

```
node index.js 
```

您可以在本地主机上打开到端口 3000 的浏览器，您应该会看到`Hello World!`消息。

这 4 行代码在幕后做了很多工作。

首先，我们将`express`包导入到`express`值中。

我们通过调用`express()`方法实例化一个应用程序。

一旦我们有了应用程序对象，我们就告诉它使用`get()`方法监听`/`路径上的 GET 请求。

每个 HTTP **动词**都有一个方法:`get()`、`post()`、`put()`、`delete()`和`patch()`:

```
app.get('/', (req, res) => { /* */ })
app.post('/', (req, res) => { /* */ })
app.put('/', (req, res) => { /* */ })
app.delete('/', (req, res) => { /* */ })
app.patch('/', (req, res) => { /* */ }) 
```

这些方法接受一个回调函数——当一个请求开始时被调用——我们需要处理它。

我们传入一个箭头函数:

```
(req, res) => res.send('Hello World!') 
```

Express 在这个回调中给我们发送了两个对象，我们称之为`req`和`res`。它们代表请求和响应对象。

两者都是标准，你可以在这里和这里阅读更多。

请求是 HTTP 请求。它为我们提供了所有的请求信息，包括请求参数、请求头、请求体等等。

Response 是我们将发送给客户端的 HTTP 响应对象。

我们在这个回调中做的是发送“Hello World！”字符串发送到客户端，使用`Response.send()`方法。

该方法将该字符串设置为主体，并关闭连接。

示例的最后一行实际上启动了服务器，并告诉它监听端口`3000`。我们传递一个回调函数，当服务器准备好接受新的请求时调用这个函数。

## 请求参数

我提到了请求对象如何保存所有 HTTP 请求信息。

这些是您可能会用到的主要属性:

| 财产 | 描述 |
| --- | --- |
| 。应用 | 保存对快速应用程序对象的引用 |
| .baseUrl | 应用程序响应的基本路径 |
| 。身体 | 包含在请求正文中提交的数据(必须先手动解析和填充，然后才能访问) |
| 。饼干 | 包含请求发送的 cookies(需要`cookie-parser`中间件) |
| 。主机名 | 在[主机 HTTP 头](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Host)值中定义的主机名 |
| 。互联网协议（Internet Protocol 的缩写） | 客户端 IP |
| 。方法 | 使用的 HTTP 方法 |
| 。参数 | 路线命名参数 |
| 。小路 | URL 路径 |
| 。草案 | 请求协议 |
| 。询问 | 包含请求中使用的所有查询字符串的对象 |
| 。安全的 | 如果请求是安全的，则为 true(使用 HTTPS) |
| 。签名书 | 包含请求发送的签名 cookies(需要`cookie-parser`中间件) |
| 。xhr | 如果请求是一个 [XMLHttpRequest](https://www.freecodecamp.org/news/xhr/) 则为真 |

## 如何向客户端发送响应

在 Hello World 示例中，我们使用 Response 对象的`send()`方法发送一个简单的字符串作为响应，并关闭连接:

```
(req, res) => res.send('Hello World!') 
```

如果您传入一个字符串，它会将`Content-Type`头设置为`text/html`。

如果你传入一个对象或数组，它会设置`application/json` `Content-Type`头，并将该参数解析成 [JSON](https://www.freecodecamp.org/news/json/) 。

此后，`send()`关闭连接。

`send()`自动设置`Content-Length` HTTP 响应头，不像`end()`要求你这样做。

### 如何使用 end()发送空响应

发送没有任何正文的响应的另一种方法是使用`Response.end()`方法:

```
res.end() 
```

### 如何设置 HTTP 响应状态

对响应对象使用`status()`方法:

```
res.status(404).end() 
```

或者

```
res.status(404).send('File not found') 
```

`sendStatus()`是快捷方式:

```
res.sendStatus(200)
// === res.status(200).send('OK')

res.sendStatus(403)
// === res.status(403).send('Forbidden')

res.sendStatus(404)
// === res.status(404).send('Not Found')

res.sendStatus(500)
// === res.status(500).send('Internal Server Error') 
```

## 如何发送 JSON 响应

当您在 Express 中监听某个路由上的连接时，回调函数将在每个网络调用中使用请求对象实例和响应对象实例来调用。

示例:

```
app.get('/', (req, res) => res.send('Hello World!')) 
```

这里我们使用了`Response.send()`方法，它接受任何字符串。

你可以通过使用`Response.json()`发送 [JSON](https://www.freecodecamp.org/news/json/) 给客户端，这是一个很有用的方法。

它接受一个对象或数组，并在发送之前将其转换为 JSON:

```
res.json({ username: 'Flavio' }) 
```

## 如何管理 Cookies

使用`Response.cookie()`方法来操作你的 cookies。

示例:

```
res.cookie('username', 'Flavio') 
```

该方法接受第三个参数，该参数包含各种选项:

```
res.cookie('username', 'Flavio', { domain: '.flaviocopes.com', path: '/administrator', secure: true })

res.cookie('username', 'Flavio', { expires: new Date(Date.now() + 900000), httpOnly: true }) 
```

您可以设置的最有用的参数是:

| 价值 | 描述 |
| --- | --- |
| `domain` | [cookie 域名](https://www.freecodecamp.org/news/cookies/#set-a-cookie-domain) |
| `expires` | 设置 [cookie 到期日期](https://www.freecodecamp.org/news/cookies/#set-a-cookie-expiration-date)。如果缺少或为 0，则 cookie 是会话 cookie |
| `httpOnly` | 将 cookie 设置为只能由 web 服务器访问。参见 [HttpOnly](https://www.freecodecamp.org/news/cookies/#httponly) |
| `maxAge` | 设置相对于当前时间的到期时间，以毫秒为单位 |
| `path` | [cookie 路径](https://www.freecodecamp.org/news/cookies/#set-a-cookie-path)。默认为“/” |
| `secure` | 标志着[饼干 HTTPS 只](https://www.freecodecamp.org/news/cookies/#secure) |
| `signed` | 设置要签名的 cookie |
| `sameSite` | [`SameSite`](https://www.freecodecamp.org/news/cookies/#samesite) 的值 |

cookie 可以通过以下方式清除:

```
res.clearCookie('username') 
```

## 如何使用 HTTP 头

### 如何从请求中访问 HTTP 头值

您可以使用`Request.headers`属性访问所有 HTTP 头:

```
app.get('/', (req, res) => {
  console.log(req.headers)
}) 
```

使用`Request.header()`方法访问一个单独的请求头的值:

```
app.get('/', (req, res) => {
  req.header('User-Agent')
}) 
```

### 如何更改响应的任何 HTTP 头值

您可以使用`Response.set()`更改任何 HTTP 头值:

```
res.set('Content-Type', 'text/html') 
```

但是，内容类型头有一个快捷方式:

```
res.type('.html')
// => 'text/html'

res.type('html')
// => 'text/html'

res.type('json')
// => 'application/json'

res.type('application/json')
// => 'application/json'

res.type('png')
// => image/png: 
```

## 如何处理重定向

重定向在 Web 开发中很常见。您可以使用`Response.redirect()`方法创建一个重定向:

```
res.redirect('/go-there') 
```

这创建了一个 302 重定向。

301 重定向以这种方式进行:

```
res.redirect(301, '/go-there') 
```

您可以指定绝对路径(`/go-there`)、绝对 URL ( `https://anothersite.com`)、相对路径(`go-there`)或使用`..`返回上一级:

```
res.redirect('../go-there')
res.redirect('..') 
```

您还可以使用以下方法重定向回 Referrer HTTP 头值(如果未设置，则默认为`/`)

```
res.redirect('back') 
```

## 快速路由

路由是确定调用 URL 时应该发生什么，或者应用程序的哪些部分应该处理特定的传入请求的过程。

在 Hello World 示例中，我们使用了以下代码:

```
app.get('/', (req, res) => { /* */ }) 
```

这创建了一个路由，它将使用 HTTP GET 方法访问根域 URL `/`映射到我们想要提供的响应。

### 命名参数

如果我们想监听自定义请求，该怎么办？也许我们想创建一个接受字符串并以大写形式返回的服务——我们不希望参数作为查询字符串发送，而是作为 URL 的一部分。在这种情况下，我们使用命名参数:

```
app.get('/uppercase/:theValue', (req, res) => res.send(req.params.theValue.toUpperCase())) 
```

如果我们向`/uppercase/test`发送请求，我们将在响应体中得到`TEST`。

您可以在同一个 URL 中使用多个命名参数，它们都将存储在`req.params`中。

### 如何使用正则表达式匹配路径

您可以使用[正则表达式](https://flaviocopes.com/javascript-regular-expressions/)用一条语句匹配多条路径:

```
app.get(/post/, (req, res) => { /* */ }) 
```

会匹配`/post`、`/post/first`、`/thepost`、`/posting/something`，以此类推。

## 快速模板

Express 能够处理服务器端模板引擎。

模板引擎允许我们向视图添加数据，并动态生成 HTML。

Express 使用 Jade 作为默认值。Jade 是老版本的 Pug，具体是 Pug 1.0。

注意，由于 2016 年的一个商标问题，名称从 Jade 更改为 Pug，当时该项目发布了版本 2。你仍然可以使用 Jade，也就是 Pug 1.0，但是将来，最好使用 Pug 2.0

虽然 Jade 的上一个版本越来越旧，但出于向后兼容的原因，它仍然是 Express 中的默认版本。

在任何新项目中，你应该使用 Pug 或者你选择的其他引擎。帕格的官方网站是[https://pugjs.org/](https://pugjs.org/)。

你可以使用许多不同的模板引擎，包括哈巴狗，车把，小胡子，EJS 和更多。

要使用 Pug，我们必须首先安装它:

```
npm install pug 
```

而在初始化 Express app 的时候，我们需要设置它:

```
const express = require('express')
const app = express()
app.set('view engine', 'pug') 
```

我们现在可以开始在`.pug`文件中编写模板了。

创建“关于”视图:

```
app.get('/about', (req, res) => {
  res.render('about')
}) 
```

和`views/about.pug`中的模板:

```
p Hello from Flavio 
```

这个模板将创建一个内容为`Hello from Flavio`的`p`标签。

您可以使用以下代码对变量进行插值:

```
app.get('/about', (req, res) => {
  res.render('about', { name: 'Flavio' })
}) 
```

```
p Hello from #{name} 
```

查看 [Pug 指南](https://flaviocopes.com/pug)了解更多关于如何使用 Pug 的信息。

这个从 HTML 到 Pug 的在线转换器将会有很大的帮助:[https://html-to-pug.com/](https://html-to-pug.com/)。

## 快速中间件

中间件是一种与路由过程挂钩的功能，在链中的某个点执行任意操作(取决于我们希望它做什么)。

它通常用于编辑请求或响应对象，或者在请求到达路由处理程序代码之前终止请求。

中间件被添加到执行堆栈，如下所示:

```
app.use((req, res, next) => { /* */ }) 
```

这类似于定义一个路由，但是除了请求和响应对象实例之外，我们还有一个对下一个中间件函数*的引用，我们将它赋给了变量`next`。*

我们总是在中间件函数的末尾调用`next()`，以便将执行传递给下一个处理程序。除非我们想提前结束响应并将其发送回客户端。

你通常使用预制的中间件，以`npm`包的形式。你可以在这里找到一大串可用的[。](https://expressjs.com/en/resources/middleware.html)

一个例子是`cookie-parser`，它用于将 cookies 解析成`req.cookies`对象。你可以使用`npm install cookie-parser`来安装它，你可以这样使用它:

```
const express = require('express')
const app = express()
const cookieParser = require('cookie-parser')

app.get('/', (req, res) => res.send('Hello World!'))

app.use(cookieParser())
app.listen(3000, () => console.log('Server ready')) 
```

我们还可以将中间件功能设置为仅针对特定的路由运行(而不是针对所有路由)，方法是将它用作路由定义的第二个参数:

```
const myMiddleware = (req, res, next) => {
  /* ... */
  next()
}

app.get('/', myMiddleware, (req, res) => res.send('Hello World!')) 
```

如果您需要存储中间件中生成的数据，以便将其传递给后续的中间件功能，或者传递给请求处理器，您可以使用`Request.locals`对象。它会将该数据附加到当前请求:

```
req.locals.name = 'Flavio' 
```

## 如何使用 Express 服务静态资产

通常在`public`子文件夹中有图像、CSS 等，并将它们暴露在根级别:

```
const express = require('express')
const app = express()

app.use(express.static('public'))

/* ... */

app.listen(3000, () => console.log('Server ready')) 
```

如果您在`public/`中有一个`index.html`文件，那么如果您现在点击根域 URL ( `http://localhost:3000`)，它就会被提供

## 如何向客户端发送文件

Express 提供了一种将文件作为附件传输的简便方法:`Response.download()`。

一旦用户点击使用这种方法发送文件的路径，浏览器将提示用户下载。

`Response.download()`方法允许你发送一个附加在请求上的文件，浏览器会把它保存到磁盘上，而不是显示在页面上。

```
app.get('/', (req, res) => res.download('./file.pdf')) 
```

在应用程序的上下文中:

```
const express = require('express')
const app = express()

app.get('/', (req, res) => res.download('./file.pdf'))
app.listen(3000, () => console.log('Server ready')) 
```

您可以用自定义文件名设置要发送的文件:

```
res.download('./file.pdf', 'user-facing-filename.pdf') 
```

此方法提供了一个回调函数，一旦文件被发送，您可以使用该函数来执行代码:

```
res.download('./file.pdf', 'user-facing-filename.pdf', (err) => {
  if (err) {
    //handle error
    return
  } else {
    //do something
  }
}) 
```

## Express 中的会话

默认情况下，快速请求是连续的，任何请求都不能链接到另一个请求。没有办法知道这个请求是否来自以前已经执行过请求的客户端。

除非使用某种机制，否则无法识别用户。

这就是会话。

当实现时，您的 API 或网站的每个用户将被分配一个唯一的会话，这允许您存储用户的状态。

我们将使用由 Express 团队维护的`express-session`模块。

您可以使用以下命令安装它:

```
npm install express-session 
```

一旦你完成了，你可以在你的应用程序中用这个实例化它:

```
const session = require('express-session') 
```

这是一个中间件，所以您使用下面的代码在 Express 中安装它:

```
const express = require('express')
const session = require('express-session')

const app = express()
app.use(session({
  'secret': '343ji43j4n3jn4jk3n'
})) 
```

完成后，所有对应用程序路由的请求现在都使用会话。

`secret`是唯一必需的参数，但是您可以使用更多的参数。对于您的应用程序，它应该是一个随机唯一的字符串。

会话被附加到请求中，因此您可以在这里使用`req.session`来访问它:

```
app.get('/', (req, res, next) => {
  // req.session
} 
```

该对象可用于从会话中获取数据，也可用于设置数据:

```
req.session.name = 'Flavio'
console.log(req.session.name) // 'Flavio' 
```

这些数据在存储时被序列化为 [JSON](https://www.freecodecamp.org/news/json/) ，所以使用嵌套对象是安全的。

您可以使用会话将数据传递给稍后执行的中间件，或者在以后的请求中检索数据。

会话数据存储在哪里？这取决于你如何设置`express-session`模块。

它可以将会话数据存储在:

*   **内存**，不用于生产
*   像 MySQL 或 Mongo 这样的数据库
*   像 Redis 或 Memcached 这样的内存缓存

在[https://github.com/expressjs/session](https://github.com/expressjs/session)有一个很大的第三包列表，它们实现了各种不同的兼容缓存存储。

所有解决方案都将会话 id 存储在 cookie 中，并将数据保存在服务器端。客户机将在一个 cookie 中接收会话 id，并将它与每个 HTTP 请求一起发送。

我们将引用服务器端来将会话 id 与本地存储的数据关联起来。

内存是默认的，它不需要您进行特殊的设置。这是最简单的事情，但它只用于开发目的。

最好的选择是像 Redis 这样的内存缓存，为此您需要建立自己的基础设施。

另一个在 Express 中管理会话的流行包是`cookie-session`，它有一个很大的不同:它在 cookie 中存储客户端数据。

我不建议这样做，因为将数据存储在 cookies 中意味着它存储在客户端，并在用户发出的每个请求中来回发送。它的大小也有限，因为它只能存储 4 千字节的数据。

Cookies 也需要被保护，但是默认情况下它们是不安全的，因为安全的 Cookies 在 HTTPS 站点上是可能的，如果你有代理，你需要配置它们。

## 如何在 Express 中验证输入

让我们看看如何验证作为输入进入 Express 端点的任何数据。

假设您有一个接受姓名、电子邮件和年龄参数的 POST 端点:

```
const express = require('express')
const app = express()

app.use(express.json())

app.post('/form', (req, res) => {
  const name  = req.body.name
  const email = req.body.email
  const age   = req.body.age
}) 
```

如何对这些结果执行服务器端验证，以确保:

*   名称是至少 3 个字符的字符串。
*   邮件是真邮件？
*   年龄是一个数字，在 0 到 110 之间？

在 Express 中处理来自外部的任何类型输入的验证的最佳方式是使用 [`express-validator`包](https://express-validator.github.io):

```
npm install express-validator 
```

您需要包中的`check`和`validationResult`对象:

```
const { check, validationResult } = require('express-validator'); 
```

我们传递一组`check()`调用作为`post()`调用的第二个参数。每个`check()`调用都接受参数名作为自变量。然后我们调用`validationResult()`来验证没有验证错误。如果有，我们告诉客户:

```
app.post('/form', [
  check('name').isLength({ min: 3 }),
  check('email').isEmail(),
  check('age').isNumeric()
], (req, res) => {
  const errors = validationResult(req)
  if (!errors.isEmpty()) {
    return res.status(422).json({ errors: errors.array() })
  }

  const name  = req.body.name
  const email = req.body.email
  const age   = req.body.age
}) 
```

注意我用了:

*   `isLength()`
*   `isEmail()`
*   `isNumeric()`

还有更多这样的方法，都来自于 [validator.js](https://github.com/chriso/validator.js#validators) ，包括:

*   `contains()`，检查值是否包含指定值
*   `equals()`，检查值是否等于指定值
*   `isAlpha()`
*   `isAlphanumeric()`
*   `isAscii()`
*   `isBase64()`
*   `isBoolean()`
*   `isCurrency()`
*   `isDecimal()`
*   `isEmpty()`
*   `isFQDN()`，检查它是否是完全合格的域名
*   `isFloat()`
*   `isHash()`
*   `isHexColor()`
*   `isIP()`
*   `isIn()`，检查该值是否在允许值的数组中
*   `isInt()`
*   `isJSON()`
*   `isLatLong()`
*   `isLength()`
*   `isLowercase()`
*   `isMobilePhone()`
*   `isNumeric()`
*   `isPostalCode()`
*   `isURL()`
*   `isUppercase()`
*   `isWhitelisted()`，对照允许字符的白名单检查输入

您可以使用`matches()`根据正则表达式验证输入。

可以使用以下方式检查日期:

*   `isAfter()`，检查输入的日期是否在您传递的日期之后
*   `isBefore()`，检查输入的日期是否在您传递的日期之前
*   `isISO8601()`
*   `isRFC3339()`

关于如何使用这些验证器的确切细节，[参考这里的文档](https://github.com/chriso/validator.js#validators)。

所有这些检查都可以通过管道进行组合:

```
check('name')
  .isAlpha()
  .isLength({ min: 10 }) 
```

如果有任何错误，服务器会自动发送响应来传达错误。例如，如果电子邮件无效，将返回以下内容:

```
{
  "errors": [{
    "location": "body",
    "msg": "Invalid value",
    "param": "email"
  }]
} 
```

可以使用`withMessage()`为您执行的每个检查覆盖这个默认错误:

```
check('name')
  .isAlpha()
  .withMessage('Must be only alphabetical chars')
  .isLength({ min: 10 })
  .withMessage('Must be at least 10 chars long') 
```

如果你想写你自己的特殊的，定制的验证器呢？您可以使用`custom`验证器。

在回调函数中，您可以通过抛出异常或返回拒绝的承诺来拒绝验证:

```
app.post('/form', [
  check('name').isLength({ min: 3 }),
  check('email').custom(email => {
    if (alreadyHaveEmail(email)) {
      throw new Error('Email already registered')
    }
  }),
  check('age').isNumeric()
], (req, res) => {
  const name  = req.body.name
  const email = req.body.email
  const age   = req.body.age
}) 
```

自定义验证程序:

```
check('email').custom(email => {
  if (alreadyHaveEmail(email)) {
    throw new Error('Email already registered')
  }
}) 
```

可以改写成这样:

```
check('email').custom(email => {
  if (alreadyHaveEmail(email)) {
    return Promise.reject('Email already registered')
  }
}) 
```

## 如何净化 Express 中的输入

您已经看到了如何验证来自外部世界的输入到您的 Express 应用程序。

当你运行一个面向公众的服务器时，你很快就会明白一件事:永远不要相信输入。

即使您清理并确保人们不能使用客户端代码输入奇怪的东西，您仍然会受到人们使用工具(甚至只是浏览器开发工具)直接向您的端点发布的影响。

或者机器人尝试人类已知的各种可能的利用方式。

您需要做的是净化您的输入。

您已经用来验证输入的 [`express-validator`包](https://express-validator.github.io)也可以方便地用来执行净化。

假设您有一个接受姓名、电子邮件和年龄参数的 POST 端点:

```
const express = require('express')
const app = express()

app.use(express.json())

app.post('/form', (req, res) => {
  const name  = req.body.name
  const email = req.body.email
  const age   = req.body.age
}) 
```

您可以使用以下方法验证它:

```
const express = require('express')
const app = express()

app.use(express.json())

app.post('/form', [
  check('name').isLength({ min: 3 }),
  check('email').isEmail(),
  check('age').isNumeric()
], (req, res) => {
  const name  = req.body.name
  const email = req.body.email
  const age   = req.body.age
}) 
```

您可以通过在验证方法之后管道化清理方法来添加清理:

```
app.post('/form', [
  check('name').isLength({ min: 3 }).trim().escape(),
  check('email').isEmail().normalizeEmail(),
  check('age').isNumeric().trim().escape()
], (req, res) => {
  //...
}) 
```

这里我使用的方法是:

*   `trim()`修剪字符串开头和结尾的字符(默认为空白)
*   `escape()`用对应的 HTML 实体替换`<`、`>`、`&`、`'`、`"`和`/`
*   `normalizeEmail()`规范化电子邮件地址。接受几个小写电子邮件地址或子地址的选项(例如`flavio+newsletters@gmail.com`

其他消毒方法:

*   `blacklist()`删除出现在黑名单中的角色
*   `whitelist()`删除未出现在白名单中的字符
*   `unescape()`用`<`、`>`、`&`、`'`、`"`和`/`替换 HTML 编码的实体
*   `ltrim()`与 trim()类似，但只修剪字符串开头的字符
*   `rtrim()`与 trim()类似，但只修剪字符串末尾的字符
*   `stripLow()`删除通常不可见的 ASCII 控制字符

强制转换为一种格式:

*   `toBoolean()`将输入字符串转换成布尔值。除“0”、“false”和“”之外的所有内容都返回 true。在严格模式下，只有“1”和“true”返回 true。
*   `toDate()`将输入字符串转换为日期，如果输入不是日期，则为 null
*   `toFloat()`将输入字符串转换为浮点数，如果输入不是浮点数，则转换为 NaN
*   `toInt()`将输入字符串转换为整数，如果输入不是整数，则转换为 NaN

与自定义验证器一样，您可以创建一个自定义杀毒器。

在回调函数中，您只需返回净化后的值:

```
const sanitizeValue = value => {
  //sanitize...
}

app.post('/form', [
  check('value').customSanitizer(value => {
    return sanitizeValue(value)
  }),
], (req, res) => {
  const value  = req.body.value
}) 
```

## 如何在 Express 中处理表单

这是一个 HTML 表单的示例:

```
<form method="POST" action="/submit-form">
  <input type="text" name="username" />
  <input type="submit" />
</form> 
```

当用户按下提交按钮时，浏览器会自动向页面同一原点的`/submit-form` URL 发出`POST`请求。浏览器发送包含的数据，编码为`application/x-www-form-urlencoded`。在这个特定的例子中，表单数据包含`username`输入字段值。

表单也可以使用`GET`方法发送数据，但是您将构建的绝大多数表单将使用`POST`。

表单数据将在 POST 请求正文中发送。

要提取它，您需要使用`express.urlencoded()`中间件:

```
const express = require('express')
const app = express()

app.use(express.urlencoded({
  extended: true
})) 
```

现在，您需要在`/submit-form`路线上创建一个`POST`端点，任何数据都可以在`Request.body`上获得:

```
app.post('/submit-form', (req, res) => {
  const username = req.body.username
  //...
  res.end()
}) 
```

在使用`express-validator`之前，不要忘记验证数据。

## 如何在 Express 中处理表单中的文件上传

这是一个允许用户上传文件的 HTML 表单示例:

```
<form method="POST" action="/submit-form" enctype="multipart/form-data">
  <input type="file" name="document" />
  <input type="submit" />
</form> 
```

不要忘记在表单中添加`enctype="multipart/form-data"`，否则文件将无法上传。

当用户按下提交按钮时，浏览器会自动向页面同一原点的`/submit-form` URL 发出一个`POST`请求。浏览器发送包含的数据，不像普通格式`application/x-www-form-urlencoded`那样编码，而是像`multipart/form-data`那样编码。

在服务器端，处理多部分数据可能很棘手并且容易出错，所以我们将使用一个名为**Harvard**的实用程序库。[这是 GitHub repo](https://github.com/felixge/node-formidable)——它有超过 4000 颗星星，维护得很好。

您可以使用以下方式安装它:

```
npm install formidable 
```

然后将它包含在 Node.js 文件中:

```
const express = require('express')
const app = express()
const formidable = require('formidable') 
```

现在，在`/submit-form`路线的`POST`端点，我们使用`formidable.IncomingForm()`实例化一个新的强大表单:

```
app.post('/submit-form', (req, res) => {
  new formidable.IncomingForm()
}) 
```

这样做之后，我们需要能够解析表单。我们可以通过提供回调来同步完成，这意味着所有文件都被处理。一旦完成了令人生畏的工作，它们就变得可用了:

```
app.post('/submit-form', (req, res) => {
  new formidable.IncomingForm().parse(req, (err, fields, files) => {
    if (err) {
      console.error('Error', err)
      throw err
    }
    console.log('Fields', fields)
    console.log('Files', files)
    for (const file of Object.entries(files)) {
      console.log(file)
    }
  })
}) 
```

或者，您可以使用事件来代替回调。例如，当每个文件被解析时，或者当发生其他事件(如文件处理完成、接收到非文件字段或发生错误)时，您会收到通知:

```
app.post('/submit-form', (req, res) => {
  new formidable.IncomingForm().parse(req)
    .on('field', (name, field) => {
      console.log('Field', name, field)
    })
    .on('file', (name, file) => {
      console.log('Uploaded file', name, file)
    })
    .on('aborted', () => {
      console.error('Request aborted by the user')
    })
    .on('error', (err) => {
      console.error('Error', err)
      throw err
    })
    .on('end', () => {
      res.end()
    })
}) 
```

无论你选择哪种方式，你都会得到一种或多种令人生畏的东西。文件对象，它为您提供关于上传文件的信息。以下是您可以调用的一些方法:

*   `file.size`，文件大小以字节为单位
*   `file.path`，文件写入的路径
*   `file.name`，文件的名称
*   `file.type`，文件的 MIME 类型

该路径默认为临时文件夹，如果您收听`fileBegin`事件，可以修改该路径:

```
app.post('/submit-form', (req, res) => {
  new formidable.IncomingForm().parse(req)
    .on('fileBegin', (name, file) => {
        file.path = __dirname + '/uploads/' + file.name
    })
    .on('file', (name, file) => {
      console.log('Uploaded file', name, file)
    })
    //...
}) 
```

## 感谢您的阅读！

这本手册到此为止。不要忘记[如果你愿意，你可以得到这本快速手册](https://thevalleyofcode.com/download/express/)的 PDF 和 ePub 版本。