# 如何用 Node.js、MongoDB、Fastify 和 Swagger 构建超快的 REST APIs

> 原文：<https://www.freecodecamp.org/news/how-to-build-blazing-fast-rest-apis-with-node-js-mongodb-fastify-and-swagger-114e062db0c9/>

想必没有一个 web 开发人员对 REST APIs 和设计一个高效的 T2 API 解决方案所带来的挑战并不陌生。

这些挑战包括:

*   速度(API 响应时间)
*   文档(清晰简洁的文档，描述 API)
*   架构和可持续性(可维护和可扩展的代码库)

在本教程中，我们将结合使用 **Node.js** 、 **MongoDB** 、 **Fastify** 和 **Swagger** 来解决上述所有问题。

该项目的源代码可以在 [GitHub](https://github.com/siegfriedgrimbeek/fastify-api) 上获得。

### 在我们开始之前…

你应该有一些初级/中级 **JavaScript 知识**，听说过 **Node.js** 和 **MongoDB、**并且知道什么是**REST API**。

以下是让您了解最新情况的一些链接:

*   [JavaScript](https://developer.mozilla.org/bm/docs/Web/JavaScript)
*   [Node.js](https://codeburst.io/the-only-nodejs-introduction-youll-ever-need-d969a47ef219)
*   [MongoDB](https://docs.mongodb.com/manual/introduction/)
*   [REST API](https://restful.io/an-introduction-to-api-s-cee90581ca1b)

#### 我们将使用的技术:

*   [加速](https://www.fastify.io/)
*   [猫鼬](https://mongoosejs.com/)
*   [大摇大摆](https://swagger.io/)

最好在新标签页中打开以上页面，以便于参考。

#### 您需要安装以下软件:

*   nodejs/NPM
*   [MongoDB](https://docs.mongodb.com/manual/installation/)
*   [邮递员](https://www.getpostman.com/)

你还需要一个 [**IDE**](https://ourcodeworld.com/articles/read/200/top-7-best-free-web-development-ide-for-javascript-html-and-css) 和一个**终端，**我用的是 Mac 的 [iTerm2](https://www.iterm2.com/) 和 Windows 的 [Hyper](https://hyper.is/) 。

### 我们开始吧！

![1*Jw9V__6jYhm2amP74D_0lw](img/cc6e269db4ce29946b80a1ab75d466f8.png)

打开您的**终端，**执行以下代码行，初始化一个新项目:

```
mkdir fastify-api
cd fastify-api
mkdir src
cd src
touch index.js
npm init
```

在上面的代码中，我们创建了两个新目录，导航到它们，创建了一个`index.js`文件，并通过 [npm](https://www.npmjs.com/) 启动了我们的项目。

初始化新项目时，系统会提示您输入几个值，这些值可以留空，以后再更新。

一旦完成，在`src`目录中就会生成一个 [package.json](https://alligator.io/nodejs/package-json/) 文件。在此文件中，您可以更改项目初始化时输入的值。

接下来，我们安装我们将需要的所有**依赖项**:

```
npm i nodemon mongoose fastify fastify-swagger boom
```

下面是对每个软件包功能的简要描述，引自它们各自的网站:

[**nodemon**](https://github.com/remy/nodemon)

> nodemon 是一个工具，通过在检测到目录中的文件更改时自动重新启动节点应用程序，来帮助开发基于 node.js 的应用程序。
> 
> nodemon 不需要*对你的代码或开发方法做任何*额外的修改。nodemon 是对`node`的替换包装，在执行脚本时使用`nodemon`替换命令行上的单词`node`。

为了设置 **nodemon** ，我们需要在脚本对象中的`package.json`文件中添加以下代码行:

```
“start”: “./node_modules/nodemon/bin/nodemon.js ./src/index.js”,
```

我们的`package.json`文件现在应该如下所示:

```
{
  "name": "fastify-api",
  "version": "1.0.0",
  "description": "A blazing fast REST APIs with Node.js, MongoDB, Fastify and Swagger.",
  "main": "index.js",
  "scripts": {
  "start": "./node_modules/nodemon/bin/nodemon.js ./src/index.js",
  "test": "echo \"Error: no test specified\" && exit 1"
},
  "author": "Siegfried Grimbeek <siegfried.grimbeek@gmail.com> (www.siegfriedgrimbeek.co.za)",
  "license": "ISC",
  "dependencies": {
  "boom": "^7.2.2",
  "fastify": "^1.13.0",
  "fastify-swagger": "^0.15.3",
  "mongoose": "^5.3.14",
  "nodemon": "^1.18.7"
  }
}
```

[**獴**](https://mongoosejs.com/)

> Mongoose 提供了一个直接的、基于模式的解决方案来建模您的应用程序数据。它包括内置的类型转换、验证、查询构建、业务逻辑挂钩等等。

[](https://www.fastify.io/)

> **Fastify 是一个高度专注于以最少的开销和强大的插件架构提供最佳开发者体验的 web 框架。它的灵感来自哈比神和 Express，据我们所知，它是目前最快的 web 框架之一。**

**[**fastify-swagger**](https://github.com/fastify/fastify-swagger)**

> **Fastify 的文档生成器。它使用您在路由中声明的模式来生成 swagger 兼容文档。**

**[**轰**](https://github.com/hapijs/boom)**

> **boom 提供了一组用于返回 HTTP 错误的实用程序。**

### **设置服务器并创建第一条路由！**

**![1*rocnESJrNWsRGXMygLfDCQ](img/3c559da11629281593defdb2d2c7a124.png)**

**将以下代码添加到您的`index.js`文件中:**

```
`// Require the framework and instantiate it
const fastify = require('fastify')({
  logger: true
})

// Declare a route
fastify.get('/', async (request, reply) => {
  return { hello: 'world' }
})

// Run the server!
const start = async () => {
  try {
    await fastify.listen(3000)
    fastify.log.info(`server listening on ${fastify.server.address().port}`)
  } catch (err) {
    fastify.log.error(err)
    process.exit(1)
  }
}
start()`
```

**我们需要 **Fastify** 框架，声明我们的第一个路由并在`port 3000`上初始化服务器，代码非常简单明了，但请注意初始化 **Fastify** 时传递的选项对象:**

```
`// Require the fastify framework and instantiate it
const fastify = require('fastify')({
  logger: true
})`
```

**上面的代码启用了默认情况下禁用的 **Fastify 的**内置记录器。**

**现在，您可以在您的**终端**的`src`目录中运行以下代码:**

```
`npm start`
```

**现在，当您导航到 [http://localhost:3000/](http://localhost:3000/) 时，您应该看到返回的`{hello:world}`对象。**

**我们将回到`index.js`文件，但现在让我们继续设置我们的数据库。**

### **启动 MongoDB 并创建模型！**

**![1*Ce0gUe0LbnhL7ebnDGTp5w](img/ccdc392087e39ba9e088991205a70ce2.png)**

**一旦成功安装了 **MongoDB** ，您就可以打开一个新的终端窗口，并通过运行以下命令启动一个 **MongoDB** 实例:**

```
`mongod`
```

**使用 **MongoDB** ，我们不需要创建数据库。我们只需在设置中指定一个名称，一旦我们存储数据， **MongoDB** 就会为我们创建这个数据库。**

**将以下内容添加到您的`index.js`文件中:**

```
`...

// Require external modules
const mongoose = require('mongoose')

// Connect to DB
mongoose.connect(‘mongodb://localhost/mycargarage’)
 .then(() => console.log(‘MongoDB connected…’))
 .catch(err => console.log(err))

...`
```

**在上面的代码中，我们需要**mongose**并连接到我们的 **MongoDB** 数据库。数据库名为`mycargarage`，如果一切顺利，您将在终端上看到`MongoDB connected...` 。**

***请注意，由于我们之前添加的`Nodemon`包，您不必重启应用程序。***

**现在我们的数据库已经建立并运行了，我们可以创建我们的第一个模型了。在`src`目录下创建一个名为`models`的新文件夹，在其中创建一个名为`Car.js`的新文件，并添加以下代码:**

```
`// External Dependancies
const mongoose = require('mongoose')

const carSchema = new mongoose.Schema({
  title: String,
  brand: String,
  price: String,
  age: Number,
  services: {
    type: Map,
    of: String
  }
})

module.exports = mongoose.model('Car', carSchema)`
```

**上面的代码声明了我们的`carSchema`，它包含了与我们的汽车相关的所有信息。除了两个明显的数据类型:`String`和`Number`。我们还使用了一个相对于**獴**来说比较新的`Map`，你可以在这里阅读更多关于它的信息。然后我们导出我们的`carSchema`,在我们的应用程序中使用。**

**我们可以继续在`index.js`文件中设置我们的路线、控制器和配置，但是本教程的一部分是演示一个可持续的代码库。因此，每个组件都有自己的文件夹。**

### **创建汽车控制器**

**为了开始创建控制器，我们在`src`目录中创建了一个名为`controllers`的文件夹，在这个文件夹中，我们创建了一个`carController.js`文件:**

```
`// External Dependancies
const boom = require('boom')

// Get Data Models
const Car = require('../models/Car')

// Get all cars
exports.getCars = async (req, reply) => {
  try {
    const cars = await Car.find()
    return cars
  } catch (err) {
    throw boom.boomify(err)
  }
}

// Get single car by ID
exports.getSingleCar = async (req, reply) => {
  try {
    const id = req.params.id
    const car = await Car.findById(id)
    return car
  } catch (err) {
    throw boom.boomify(err)
  }
}

// Add a new car
exports.addCar = async (req, reply) => {
  try {
    const car = new Car(req.body)
    return car.save()
  } catch (err) {
    throw boom.boomify(err)
  }
}

// Update an existing car
exports.updateCar = async (req, reply) => {
  try {
    const id = req.params.id
    const car = req.body
    const { ...updateData } = car
    const update = await Car.findByIdAndUpdate(id, updateData, { new: true })
    return update
  } catch (err) {
    throw boom.boomify(err)
  }
}

// Delete a car
exports.deleteCar = async (req, reply) => {
  try {
    const id = req.params.id
    const car = await Car.findByIdAndRemove(id)
    return car
  } catch (err) {
    throw boom.boomify(err)
  }
}`
```

**以上内容可能看起来有点难以理解，但实际上非常简单。**

*   **我们需要 **boom** 来处理我们的错误:`boom.boomify(err)`。**
*   **我们导出将在我们的路由中使用的每个函数。**
*   **每个函数都是一个**异步**函数，可以包含一个**等待**表达式，该表达式暂停**异步函数**的执行，并等待传递的承诺的解析，然后恢复**异步函数的**执行并返回解析后的值。[点击此处了解更多信息。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)**
*   **每个函数都包装在一个 try / catch 语句中。点击此处了解更多信息。**
*   **每个函数都有两个参数:`req`(请求)和`reply`(回复)。在我们的教程中，我们只使用请求参数。我们将使用它来访问请求体和请求参数，从而允许我们处理数据。[在此了解更多信息。](https://www.fastify.io/docs/latest/Reply/)**
*   **注意第 31 行的代码:
    `const car = new Car({ …req.body })`
    这使用了 **JavaScript** 扩展操作符。[点击此处了解详情。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)**
*   **注意第 42 行的代码:
    `const { …updateData } = car`
    它结合 spread 操作符使用了 **JavaScript** 析构。[点击此处了解详情。](https://codeburst.io/use-es2015-object-rest-operator-to-omit-properties-38a3ecffe90)**

**除此之外，我们使用一些标准的**mongose**特性来操作我们的数据库。**

**你可能正在燃烧启动你的 API 并进行健全性检查，但是在我们这样做之前，我们只需要将**控制器**连接到**路线**，然后最后将**路线**连接到应用程序。**

#### **创建和导入路线**

**同样，我们可以从在我们项目的根目录中创建一个文件夹开始，但是这一次，它被称为`routes`。在该文件夹中，我们用以下代码创建一个`index.js`文件:**

```
`// Import our Controllers
const carController = require('../controllers/carController')

const routes = [
  {
    method: 'GET',
    url: '/api/cars',
    handler: carController.getCars
  },
  {
    method: 'GET',
    url: '/api/cars/:id',
    handler: carController.getSingleCar
  },
  {
    method: 'POST',
    url: '/api/cars',
    handler: carController.addCar,
    schema: documentation.addCarSchema
  },
  {
    method: 'PUT',
    url: '/api/cars/:id',
    handler: carController.updateCar
  },
  {
    method: 'DELETE',
    url: '/api/cars/:id',
    handler: carController.deleteCar
  }
]

module.exports = routes`
```

**这里我们需要我们的**控制器**，并将我们在控制器中创建的每个功能分配给我们的路线。**

**如您所见，每条路由都由一个方法、一个 url 和一个处理程序组成，指示应用程序在访问其中一条路由时使用哪个函数。**

**跟随某些路线的`:id`是向路线传递参数的常见方式，这将允许我们如下访问 **id** :**

**`[http://127.0.0.1:3000/api/cars/5bfe30b46fe410e1cfff2323](http://127.0.0.1:3000/api/cars/5bfe30b46fe410e1cfff2323)`**

#### **将所有这些放在一起并测试我们的 API**

**现在我们已经构建了大部分的部件，我们只需要将它们连接在一起，开始通过我们的 **API** 提供数据。首先，我们需要导入我们创建的**路线**，方法是将下面一行代码添加到我们的主`index.js`文件中:**

```
`const routes = require(‘./routes’)`
```

**然后我们需要遍历我们的 routes 数组，用 **Fastify 初始化它们。**我们可以用下面的代码做到这一点，这些代码也需要添加到主`index.js`文件中:**

```
`routes.forEach((route, index) => {
 fastify.route(route)
})`
```

**现在我们准备开始测试了！**

**这项工作的最佳工具是 [Postman](https://www.getpostman.com/) ，我们将用它来测试我们所有的路线。我们将在请求体中以原始对象和参数的形式发送数据。**

**查找所有汽车:**

**![1*YoxRE054Q7qgrAxaCgrzLw](img/d0288add42e1bb449178feac1fec8b05.png)**

**寻找一辆车:**

**![1*1T4C1-LmgWv0S5W-bgk4wQ](img/4f2984baf601398647fc6f8050fbe1c6.png)**

**添加新车**:**

**![1*UYqA6GVUv_dcKArTRJ_NPA](img/f75766a548b78eb956cb78b504ecea17.png)**

****服务看起来是空的，但是信息实际上保存在数据库中。**

**更新汽车:**

**![1*BcZdrz0X3dyfDfOhkDFwFg](img/c596fd25276e996f8bf9843b4cafb330.png)**

**删除汽车:**

**![1*9PwntcmeC1wfYMqo3raXdQ](img/67965e35c0a49d76f135a860a59bdbca.png)**

**我们现在有了一个全功能的 API——但是文档呢？这就是 **Swagger** 真正得心应手的地方。**

**![1*7iaXjYojG6kWxLTZaW1x4Q](img/746da5c90bec57bd4a42824d69deba4b.png)**

#### **加入招摇和总结。**

**现在我们将创建名为 **config 的最终文件夹。**我们将创建一个名为`swagger.js`的文件，代码如下:**

```
`exports.options = {
  routePrefix: '/documentation',
  exposeRoute: true,
  swagger: {
    info: {
      title: 'Fastify API',
      description: 'Building a blazing fast REST API with Node.js, MongoDB, Fastify and Swagger',
      version: '1.0.0'
    },
    externalDocs: {
      url: 'https://swagger.io',
      description: 'Find more info here'
    },
    host: 'localhost',
    schemes: ['http'],
    consumes: ['application/json'],
    produces: ['application/json']
  }
}`
```

**上面的代码是一个带有选项的对象，我们将把它传递给我们的 **fastify-swagger** 插件。为此，我们需要将以下内容添加到我们的`index.js`文件中:**

```
`// Import Swagger Options
const swagger = require(‘./config/swagger’)

// Register Swagger
fastify.register(require(‘fastify-swagger’), swagger.options)`
```

**然后，在我们初始化了我们的 **Fastify** 服务器之后，我们需要添加下面一行:**

```
`...
await fastify.listen(3000)
fastify.swagger()
fastify.log.info(`listening on ${fastify.server.address().port}`)
...`
```

**就是这样！如果您现在导航到[http://localhost:3000/documentation](http://localhost:3000/documentation)，您应该会看到以下内容:**

**![1*HV-5eImCMs7LtiLodTz7CQ](img/8d76ef4749bddbfb86d98a42eeadf6a5.png)**

**就这么简单！现在你有了自我更新的 API 文档，它将与你的 API 一起发展。您可以轻松地为您的路线添加更多信息，请点击此处查看更多。**

#### **接下来是什么？**

**现在我们已经有了一个基本的 API，可能性是无限的。它可以作为任何可以想象的应用程序的基础。**

**在下一个教程中，我们将集成 **GraphQL** ，并最终将前端与 **Vue.js** 也集成在一起！**