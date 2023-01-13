# 如何在 Heroku 上使用 Express 服务器部署 React 应用程序

> 原文：<https://www.freecodecamp.org/news/how-to-deploy-a-react-app-with-an-express-server-on-heroku-32244fe5a250/>

阿什什·南丹·辛格

# 如何在 Heroku 上使用 Express 服务器部署 React 应用程序

![YPGVGgxYKzPpRZzTZIgfWgTvQ4T0E8zaLpkZ](img/cc680be84248774c49a9e4b918c3531f.png)

你好，世界！最近，我不得不为我正在做的一项自由职业工作在 Heroku 上部署一个网站。我认为这个过程可能有些困难，关于如何做到这一点的详细教程或文章应该有所帮助。所以这个会非常简单，希望非常短。

我们将首先创建一个 Express 应用程序，它将充当我们的服务器。一旦服务器完成，我们将创建一个简单的 create-react-app 应用程序，连接服务器和前端，最后，将整个部署到一个托管平台，如 Heroku。

在我们进一步讨论之前，我希望你明白，在 web 开发的世界里，几乎一切都取决于个人的偏好。你们中的一些人可能在某些事情上有不同意见，你可以继续你想做的事情，这完全没问题。在我们破坏应用程序之前，我认为一切都很好。

让我们开始吧。

### 创建节点/快速应用程序

首先为整个项目创建一个文件夹。这个文件夹将包含客户端应用程序——在本例中是我们的 React 应用程序。导航到终端中的目录，并键入以下命令。

```
$ touch server.js$ npm init
```

上面代码片段中的最后一个命令将带您完成一些步骤，并用一个`package.json`文件初始化您的项目。如果您对此完全陌生，您可以将该文件视为一个分类帐，其中记录了您将在应用程序的构建过程中使用的所有依赖项。

继续，现在我们已经准备好了`package.json`文件，我们可以开始为项目添加依赖项。

添加快递:

```
$ npm i -g express --save
```

这会将 Express 作为一个依赖项添加到您的 package.json 中。现在我们有了这个，我们所需要的就是多一个依赖项，这是为了在我们对代码进行一些更改时热重新加载应用程序:

```
$ npm i -g nodemon --save --dev
```

这将向应用程序添加 nodemon。如果你想了解更多关于 nodemon 的信息，你可以查看[这个](https://nodemon.io/)链接来获得更多信息。

既然我们已经添加了这些，我们就可以在 Express 中创建我们最基本的服务器了。让我们看看这是怎么做到的。

```
const express = require('express');const app = express();const port = process.env.PORT || 5000;
```

```
//Route setupapp.get('/', (req, res) => {    res.send('root route');
```

```
})
```

```
//Start serverapp.listen(port, (req, res) => {
```

```
console.log(`server listening on port: ${port}`)
```

```
 });
```

就是这样。只需导航到终端，确保您在项目的根目录中，并键入:

```
$ nodemon <name-of-the-file> (index.js/server.js)
```

在我们的例子中，既然我们把它命名为`server.js`，那么它就是`nodemon server.js` *。*这将在本地启动计算机端口 5000 上的服务器。如果你去访问浏览器，打开 [https://localhost:5000/](https://localhost:5000/) 你会看到文本“根路由”，这意味着服务器已经启动。如果你有任何问题，欢迎在下面的评论中提出来。

现在服务器已经设置好了，并且正在工作，让我们继续安装 React 应用程序。

### React 应用

```
$ npm i -g create-react-app$ create-react-app <name-of-the-app>
```

出于本教程的目的，我们将应用程序命名为“client”，因此我们的命令看起来像这样`create-react-app client` *。*

一旦完成，设置将花费一些时间，并将在主应用程序中创建一个名为“client”的漂亮的小文件夹。

目前，我们不会对整个 React 应用程序进行任何重大更改——这超出了本教程的范围。

现在的挑战是，我们需要调用并连接运行在本地主机上的服务器。为此，我们需要进行 API 调用。

#### 添加代理

React 通过向我们的`package.json`文件添加一个代理值，让我们能够做到这一点。导航到目录中的`package.json`文件，并添加下面这段代码。

```
"proxy": "http://localhost:5000",
```

在我们的例子中，服务器运行在端口 5000，因此代理值为 5000。如果您完全使用不同的端口，该值可能会有所不同。

现在，我们需要从 React 组件中调用 Express 定义的端点，或 API 端点。这实际上意味着，现在我们可以简单地从客户端调用“api/users/all”，它将代理我们的请求，看起来像这样“https://localhost:5000/API/users/all”。这使我们不必发出跨来源请求，因为出于安全原因，大多数现代浏览器都不允许这样做。

接下来，我们将对`src/app.js`文件进行一些修改。

```
import React, { Component } from 'react';import './App.css';import Navbar from './Components/Layout/Navbar';import { BrowserRouter as Router, Route } from 'react-router-dom';import Footer from './Components/Layout/Footer';import Home from './Components/Layout/Home';import Social from './Components/social/Social';
```

```
class App extends Component {  constructor(props) {    super(props);    this.state = {}    this.connecToServer = this.connecToServer.bind(this);  }
```

```
 connecToServer() {    fetch('/');  }
```

```
 componentDidMount() {    this.connecToServer();  }
```

```
 render() {    return (      <Router>      <div className="container">         <Navbar />         <Route exact path="/" component={Home} />         <Route exact path="/social" component={Social} />         <Footer />      </div>      </Router>    );  }}
```

```
export default App;
```

我们所做的是创建一个构造函数，并将它的值绑定到我们的函数中，这个函数将调用 fetch API。然后组件一挂载，我们就调用这个函数。接下来我们有 render 函数，它有应用程序的整体标记。因此，这是我们将在 React 或我们的前端应用程序中进行的最后一项更改。

您的`package.json`文件应该类似于下面的代码片段。

```
{  "name": "project-name",  "version": "0.1.0",  "private": true,  "dependencies": {    "react": "^16.6.3",    "react-dom": "^16.6.3",    "react-scripts": "2.1.1",    "react-router-dom": "^4.3.1"  },
```

```
 "scripts": {    "start": "react-scripts start",    "build": "react-scripts build",    "test": "react-scripts test",    "eject": "react-scripts eject"  },
```

```
 "eslintConfig": {    "extends": "react-app"  },
```

```
 "proxy": "http://localhost:5000",
```

```
 "browserslist": [    ">0.2%",    "not dead",    "not ie <= 11",    "not op_mini all"  ]}
```

现在让我们暂停一分钟，想想我们下一步需要做什么。有什么想法吗？

你们中的一些人认为我们需要确保我们的 React 文件由我们的 Express 服务器提供服务，这是对的。让我们对`server.js`文件做一些修改，以确保 Express 服务器能够提供 React 文件。

#### 服务器文件更改

```
const express = require('express');const app = express();const path = require('path');const port = process.env.PORT || 5000;
```

```
//Static file declarationapp.use(express.static(path.join(__dirname, 'client/build')));
```

```
//production modeif(process.env.NODE_ENV === 'production') {  app.use(express.static(path.join(__dirname, 'client/build')));  //  app.get('*', (req, res) => {    res.sendfile(path.join(__dirname = 'client/build/index.html'));  })}
```

```
//build modeapp.get('*', (req, res) => {  res.sendFile(path.join(__dirname+'/client/public/index.html'));})
```

```
//start serverapp.listen(port, (req, res) => {  console.log( `server listening on port: ${port}`);})
```

在上面的代码片段中，首先您需要使用 node 中内置的 path 模块，并声明您希望在这个 Express 服务器中使用的静态文件夹。

然后检查流程**是否是生产**，一旦应用程序被部署到 Heroku，就会是生产【】。在这种情况下，您希望从构建文件夹**而不是公共文件夹**中提供`index.html`文件。

如果**不是生产模式，**，并且您正在测试某个特性，并且您的服务器运行在本地主机上，那么您希望来自公共文件夹的`index.html`被提供服务。

现在，我们需要确保首先启动 Express 服务器，然后启动 React 服务器。现在有很多方法可以做到这一点，为了本教程的简单起见，我们将使用一个名为`concurrently` 的模块用一个命令来运行两个服务器。

确保您在根目录中，然后从终端运行下面的命令。

```
npm i concurrently --save
```

完成这些之后，让我们对 Express server `package.json`文件中的脚本进行一些修改。

```
"scripts": {    "client-install": "npm install --prefix client",    "start": "node index.js",    "server": "nodemon index.js",    "client": "npm start --prefix client",    "dev": "concurrently \"npm run server\" \"npm run client\"",
```

```
}
```

*   将安装 React 应用程序的所有依赖项
*   `npm start`将启动服务器，并且在检测到任何变化后不会重新加载
*   `npm run server`将启动服务器，监听代码中的任何更改，并在浏览器上热重新加载页面以反映更改。
*   `npm run client`将运行 React 应用程序而不启动服务器
*   `npm run dev`将在您的浏览器上同时运行服务器和客户端

### Heroku 设置

确保你有 Heroku 的账户。如果你没有，你可以用你的 GitHub 证书很快创建一个。

接下来，我们将安装 Heroku CLI，它将帮助我们从终端部署应用程序。[点击此处](https://devcenter.heroku.com/articles/heroku-cli)获取 macOS 和 Windows 上的安装说明。

```
$ heroku login
```

输入 Herkou 的登录凭据，然后:

```
$ heroku create
```

这将为您创建一个应用程序。如果您现在访问 Heroku 仪表板，它会在那里有应用程序。

现在，我们需要确保在将项目推送到 Heroku 存储库之前，我们的项目中有一个构建文件夹。将下面的脚本添加到您的`package.json`文件中。

```
"scripts": {    "client-install": "npm install --prefix client",
```

```
 "start": "node server.js",
```

```
 "server": "nodemon server.js",
```

```
 "client": "npm start --prefix client",
```

```
 "dev": "concurrently \"npm run server\" \"npm run client\"",
```

```
 "heroku-postbuild":      "NPM_CONFIG_PRODUCTION=false npm install --prefix client        && npm run build --prefix client"  },
```

完成之后，保存文件并将整个项目存储库推送到 Heroku 应用程序分支。

```
//add remote
```

```
$ heroku git:remote -a application-name
```

```
$ git add .
```

```
$ git commit -am 'prepare to deploy'
```

```
$ git push heroku master
```

应该就是这样了。

一旦这些都完成了，你就会得到一个你的实时托管项目的 URL。分享和展示您可以使用这些技术构建的东西。

如果您有任何问题或意见，请随时添加您的意见或直接联系。

github:[https://github.com/ashishcodes4](https://github.com/ashishcodes4)

推特:[https://twitter.com/ashishnandansin](https://twitter.com/ashishnandansin)

领英:[https://www.linkedin.com/in/ashish-nandan-singh-490987130/](https://www.linkedin.com/in/ashish-nandan-singh-490987130/)