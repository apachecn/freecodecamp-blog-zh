# 如何使用 MongoDB Atlas 将 MERN 应用程序部署到 Heroku

> 原文：<https://www.freecodecamp.org/news/deploying-a-mern-application-using-mongodb-atlas-to-heroku/>

## MERN 简介

在本文中，我们将为 Heroku 构建和部署一个使用 MERN 堆栈构建的应用程序。

MERN，代表 MongoDB、Express、React 和 Node.js，是一个用于构建 web 应用程序的流行技术栈。它涉及前端工作(使用 React)、后端工作(使用 Express 和 NodeJS)和数据库(使用 MongoDB)。

另一方面，Heroku 是一个平台即服务(PaaS ),它使开发者能够完全在云中构建、运行和操作应用程序。

对于数据库，我们将使用 MongoDB Atlas，这是一种面向现代应用程序的全球云数据库服务。这比本地安装在我们服务器上的 MongoDB 更安全，也给我们的服务器提供了更多资源的空间。

对于前端，我们将构建一个简单的 React 应用程序，它向 API 发出 POST 请求来添加用户，还可以发出 GET 请求来获取所有用户。

使用下面列出的目录，您可以跳到任何步骤。

## 目录

*   [MERN 简介](#introduction-to-mern)
*   [让我们开始建造](#let-s-start-building)
*   [构建 React 应用](#building-the-react-app)
*   [创建后端](#creating-the-backend)
*   [连接 MongoDB Atlas 数据库](#connect-the-mongodb-atlas-database)
*   [在前端调用 APIs】](#calling-apis-on-the-frontend)
*   [部署到 Heroku](#deploying-to-heroku)
*   [创建 Heroku 应用](#create-a-heroku-app)
*   [配置 package.json](#configure-package-json)
*   [总结](#wrap-up)

## 让我们开始建造吧

### 构建 React 应用程序

**注意:**在我们开始我们的项目之前，`node`必须安装在你的电脑上。`node`还为我们提供了`npm`，用于安装包。

#### 安装`create-react-app`

`create-react-app`用于创建一个 starter React app。

如果您没有安装`create-react-app`，请在命令行中键入以下内容:

```
npm i create-react-app -g 
```

`-g`标志全局安装软件包。

#### 创建项目目录

```
create-react-app my-project
cd my-project 
```

上面创建了一个“my-project”目录，并安装了将在 React starter 应用程序中使用的依赖项。安装完成后，第二个命令会切换到项目目录。

#### 启动应用程序并进行必要的编辑

```
npm start 
```

上面的命令启动 React 应用程序，它提供了一个 URL，您可以在其中预览项目。然后，您可以进行必要的编辑，如更改图像或文本。

#### 安装 axios

```
npm i axios --save 
```

`axios`是一个 JavaScript 库，用于简化 HTTP 请求。它将用于从前端向后端提供的 API 发送请求(反应)。

### 创建后端

后端管理 API，处理请求，还连接到数据库。

#### 安装后端软件包

```
npm i express cors mongoose body-parser --save 
```

1.  `express`:“Express 是一个最小且灵活的 Node.js web 应用程序框架，它为 web 应用程序提供了一组强大的功能”- Express [文档](http://expressjs.com/)
2.  `cors`:“CORS 是一个 node.js 包，用于提供一个连接/快速中间件，可用于启用具有各种选项的 CORS”——[CORS 文档](https://www.npmjs.com/package/cors)
3.  `mongoose`:“mongose 是一个 MongoDB 对象建模工具，设计用于在异步环境中工作。猫鼬同时支持承诺和回拨”——[猫鼬文档](https://www.npmjs.com/package/mongoose)
4.  `body-parser`:“node . js 体解析中间件。”- [主体解析器文档](https://www.npmjs.com/package/mongoose)

#### 创建后端文件夹

```
mkdir backend
cd backend 
```

#### 配置后端

##### 创建一个入口点`server.js`

首先，创建一个`server.js`文件，这将是后端的入口点。

```
touch server.js 
```

在`server.js`中，键入以下内容:

```
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const path = require('path')
const app = express();
require('./database');
-----
app.use(bodyParser.json());
app.use(cors());
-----
// API
const users = require('/api/users');
app.use('/api/users', users);
-----
app.use(express.static(path.join(__dirname, '../build')))
app.get('*', (req, res) => {
    res.sendFile(path.join(__dirname, '../build'))
})
-----
const port = process.env.PORT || 5000;
app.listen(port, () => {
    console.log(`Server started on port ${port}`);
}); 
```

`express.static`交付静态文件，这些文件是在 React 项目上运行`npm run build`时构建的。记住，构建的文件在构建文件夹中。

在我们的配置中，任何发送到`/api/users`的请求都将被发送到我们将要配置的`users` API。

##### 配置`users` API

```
mkdir api
touch api/users.js 
```

在`api/users.js`中，添加以下内容:

```
const express = require('express');
const router = express.Router()
-----
const User = require('../models/User');
-----
router.get('/', (req, res) => {
    User.find()
        .then(users => res.json(users))
        .catch(err => console.log(err))
})
-----
router.post('/', (req, res) => {
    const { name, email } = req.body;
    const newUser = new User({
        name: name, email: email
    })
    newUser.save()
        .then(() => res.json({
            message: "Created account successfully"
        }))
        .catch(err => res.status(400).json({
            "error": err,
            "message": "Error creating account"
        }))
})
module.exports = router 
```

在上面的代码中，我们创建了一个 GET 和 POST 请求处理程序，它获取所有用户并发布用户。我们将创建的`User`模型有助于获取用户并将其添加到数据库中。

##### 创建`User`模型

```
mkdir models
touch models/user.js 
```

在`models/user.js`中，添加以下内容:

```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;
-----
const userSchema = new Schema({
    name: {
        type: String,
        required: true
    },
    email: {
        type: String,
        required: true
    }
})
module.exports = mongoose.model("User", userSchema, "users") 
```

在上面的代码中，为用户创建了一个包含用户字段的模式。在文件的末尾，模型(“用户”)与模式和集合(“用户”)一起导出。

##### 连接 MongoDB Atlas 数据库

根据[的文档](https://www.mongodb.com/cloud/atlas)，“MongoDB Atlas 是现代应用的全球云数据库服务。”

首先我们需要在 Mongo cloud 上注册。浏览[该文档](https://docs.atlas.mongodb.com/getting-started/)以创建一个 Atlas 帐户并创建您的集群。

值得注意的一点是**将你的连接 IP 地址**列入白名单。如果忽略这一步，您将无法访问集群，因此请注意这一步。

集群是一个小型服务器，它将管理我们的集合(类似于 SQL 数据库中的表)。为了将您的后端连接到集群，创建一个文件`database.js`，正如您所看到的，这在`server.js`中是必需的。然后输入以下内容:

```
const mongoose = require('mongoose');
const connection = "mongodb+srv://username:<password>@<cluster>/<database>?retryWrites=true&w=majority";
mongoose.connect(connection,{ useNewUrlParser: true, useUnifiedTopology: true, useFindAndModify: false})
    .then(() => console.log("Database Connected Successfully"))
    .catch(err => console.log(err)); 
```

在`connection`变量中，输入您的`username`(对于 MongoDB cloud)、您的`password`(集群密码)、您的`cluster`(集群地址)和`database`(数据库名称)。如果您遵循文档，所有这些都很容易发现。

## 在前端调用 API

所有的 API 都将在本地的`localhost:5000`上可用，就像我们在`server.js`中设置的一样。当部署到 Heroku 时，服务器将使用服务器提供的端口(`process.env.PORT`)。

为了使事情变得更简单，React 允许我们指定请求将被发送到哪个代理。

打开`package.json`，在最后一个花括号之前，添加以下内容:

```
"proxy": "http://localhost:5000" 
```

这样我们可以直接向`api/users`发送请求。当部署和构建我们的站点时，我们的应用程序的默认端口将与相同的 API 一起使用。

打开`App.js`进行反应，并添加以下内容:

```
import React, {useState, useEffect} from 'react'
import axios from 'axios';
-----
const App = function () {
	const [users, setUsers] = useState(null);

	const [username, setUsername] = useState("");
	const [email, setEmail] = useState("");
	useEffect(() => {
		axios
			.get("/api/users")
			.then((users) => setUsers(users))
			.catch((err) => console.log(err));
	}, []);

	function submitForm() {
		if (username === "") {
			alert("Please fill the username field");
			return;
		}
		if (email === "") {
			alert("Please fill the email field");
			return;
		}
		axios
			.post("/api/users", {
				username: username,
				email: email,
			})
			.then(function () {
				alert("Account created successfully");
				window.location.reload();
			})
			.catch(function () {
				alert("Could not creat account. Please try again");
			});
	}
	return (
		<>
			<h1>My Project</h1>
			{users === null ? (
				<p>Loading...</p>
			) : users.length === 0 ? (
				<p>No user available</p>
			) : (
				<>
					<h2>Available Users</h2>
					<ol>
						{users.map((user, index) => (
							<li key={index}>
								Name: {user.name} - Email: {user.email}
							</li>
						))}
					</ol>
				</>
			)}

			<form onSubmit={submitForm}>
				<input
					onChange={(e) => setUsername(e.target.value)}
					type="text"
					placeholder="Enter your username"
				/>
				<input
					onChange={(e) => setEmail(e.target.value)}
					type="text"
					placeholder="Enter your email address"
				/>
				<input type="submit" />
			</form>
		</>
	);
};
export default App 
```

`useState`和`useEffect`钩子用于处理状态和`sideEffects`。基本上正在发生的是，用户的第一个状态是`null`和‘正在加载...’显示在浏览器中。

在`useEffect`中，`[]`用于指定在`componentDidMount`阶段(组件挂载时)，向运行在`localhost:5000`上的 API 发出 Axios 请求。如果它得到结果并且没有用户，则显示“没有用户可用”。否则，将显示用户的编号列表。

如果你想了解更多关于`useState`和`useEffect`的知识，看看这篇文章——[React Hooks 到底是什么？](https://blog.soshace.com/what-the-heck-is-react-hooks/)

有了这个表单，就可以发出 POST 请求来发布一个新用户。输入的状态受到控制，并在提交时在`localhost:5000`发送给 API。之后，页面会刷新并显示新用户。

## 部署到 Heroku

要将您的应用程序部署到 Heroku，您必须拥有一个 Heroku 帐户。

前往[他们的页面](https://www.heroku.com/)创建账户。然后浏览。也可以在 Heroku CLI 上查看[文档](https://devcenter.heroku.com/articles/heroku-cli)。

### 创建 Heroku 应用程序

首先，登录 Heroku:

```
heroku login 
```

这将把您重定向到浏览器中的一个 URL，您可以在那里登录。完成后，您可以在终端中继续。

在同一个 React 项目目录中，运行以下命令:

```
heroku create 
```

这将创建一个 Heroku 应用程序，并为您提供访问该应用程序的 URL。

### 配置 package.json

Heroku 使用您的 package.json 文件来了解要成功运行您的项目需要运行哪些脚本和安装哪些依赖项。

在您的`package.json`文件中，添加以下内容:

```
{
    ...
    "scripts": {
        ...
        "start": "node backend/server.js",
        "heroku-postbuild": "NPM_CONFIG_PRODUCTION=false npm install npm && run build"
    },
    ...
    "engines": {
        "node": "10.16.0"
    }
} 
```

Heroku 运行 post build，正如您所看到的，它安装您的依赖项并运行 React 项目的构建。然后它用基本上启动你的服务器的`start`脚本启动你的项目。之后，您的项目应该会运行良好。

`engines`指定要安装的`node`和`npm`等引擎的版本。

#### 向 Heroku 推进

```
git push heroku master 
```

这会将您的代码推送到 Heroku。记得在`.gitignore`中包含不必要的文件。

几秒钟后，你的网站就准备好了。如果有任何错误，您可以检查您的终端或者在浏览器中查看您的仪表板来查看构建日志。

现在你可以在运行`heroku create`时 Heroku 发送的 URL 上预览你的站点。

这就是全部了。很高兴你读到这里。

## 包裹

当然，MERN 堆栈应用还不止这些。

这篇文章没有深入讨论身份验证、登录、会话等等。它只是介绍了如何将 MERN 堆栈应用程序部署到 Heroku 并使用 MongoDB Atlas。

你可以在我的博客上找到其他类似的文章-[dillionmegida.com](https://dillionmegida.com)

感谢阅读。