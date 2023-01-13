# 如何编写一个生产就绪的节点和 Express app

> 原文：<https://www.freecodecamp.org/news/how-to-write-a-production-ready-node-and-express-app-f214f0b17d8c/>

### 项目结构

当我开始构建 Node & Express 应用程序时，我不知道构建应用程序有多重要。Express 没有严格的规则或指导方针来维护项目结构。

你可以随意使用任何你想要的结构。当你的代码库增长时，你最终会拥有很长的`route`处理程序。这使得你的代码难以理解，并且包含潜在的错误。

如果你在为一家初创公司工作，大多数时候你没有时间来折射你的项目或模块化它。你可能会陷入一个无休止的错误修复和修补的循环中。

[https://giphy.com/embed/6csVEPEmHWhWg](https://giphy.com/embed/6csVEPEmHWhWg)

随着时间的推移，在与小型团队和大型团队一起工作时，我意识到什么样的结构可以随着项目的发展而发展，并且仍然易于维护。

#### 模型视图控制器

MVC 模式有助于快速和并行开发。例如，一个开发人员可以处理视图，而另一个开发人员可以在控制器中创建业务逻辑。

让我们看一个简单的用户 CRUD 应用程序的例子。

```
project/
  controllers/
    users.js
  util/
    plugin.js
  middlewares/
    auth.js
  models/
    user.js
  routes/
    user.js
    router.js
  public/
    js/
    css/
    img/
  views/
    users/
      index.jade
  tests/
    users/
      create-user-test.js 
      update-user-test.js
      get-user-test.js
  .gitignore
  app.js
  package.json
```

*   **控制器:**定义你的应用程序路由处理器和业务逻辑
*   **util:** 在这里写可以被任何控制器使用的工具/助手函数。比如你可以写一个类似`mergeTwoArrays(arr1, arr2)`的函数。
*   **中间件:**您可以编写中间件来解释所有传入的请求，然后再转到路由处理器。例如
    `router.post('/login', auth, controller.login)`，其中`auth`是在`middlewares/auth.js`中定义的中间件函数。
*   **型号:**也是控制器和数据库之间的一种中间件。您可以定义一个模式，并在写入数据库之前进行一些验证。例如，您可以使用像[mongose](https://mongoosejs.com/)这样的 ORM，它有很好的特性和方法可以在模式本身中使用
*   **路由:**用 HTTP 方法定义你的 app 路由。例如，您可以定义与用户相关的所有内容。

```
router.post('/users/create', controller.create)
router.put('/users/:userId', controller.update)
router.get('/users', controller.getAll)
```

*   **public:** 在`/img`中存储静态图片，自定义 JavaScript 文件，CSS `/css`
*   **视图:**包含由服务器渲染的模板。
*   测试:您可以在这里为 API 服务器编写所有的单元测试或验收测试。
*   **app.js:** 作为项目的主文件，在这里初始化 app 和项目的其他元素。
*   **package.json:** 负责依赖项、用`npm`命令运行的脚本以及项目的版本。

### **异常和错误处理**

当用任何语言创建任何项目时，这是需要考虑的最重要的方面之一。让我们看看如何在 Express 应用程序中优雅地处理错误和异常。

#### **利用承诺**

使用承诺优于回调的一个优点是，它们可以处理异步代码块中隐式或显式的异常/错误，以及在`.then()`中定义的同步代码，一个承诺回调

在承诺链的末尾加上`.catch(next)`就可以了。例如:

```
router.post('/create', (req, res, next) => {

   User.create(req.body)    // function to store user data in db
   .then(result => {

     // do something with result

     return result 
   })
   .then(user => res.json(user))
   .catch(next)
})
```

#### **使用试抓法**

Try-catch 是捕获异步代码中异常的传统方法。

让我们看一个有可能得到异常的例子:

```
router.get('/search', (req, res) => {

  setImmediate(() => {
    const jsonStr = req.query.params
    try {
      const jsonObj = JSON.parse(jsonStr)

      res.send('Success')
    } catch (e) {
      res.status(400).send('Invalid JSON string')
    }
  })
})
```

#### **避免使用同步代码**

同步代码也称为阻塞代码，因为它阻塞执行，直到它们被执行。

所以避免使用可能需要几毫秒或几微秒的同步函数或方法。对于一个高流量的网站来说，这将是复杂的，并可能导致 API 请求的高延迟或响应时间。

尤其不要在生产中使用它们:)

许多 Node.js 模块都带有`.sync`和`.async`方法，所以在生产中使用异步。

但是，如果您仍然想使用同步 API，请使用`--trace-sync-io`命令行标志。每当您的应用程序使用同步 API 时，它都会打印一个警告和一个堆栈跟踪。

有关错误处理基础的更多信息，请参见:

*   [node . js 中的错误处理](https://www.joyent.com/developers/node/design/errors)
*   [构建健壮的节点应用程序:错误处理](https://strongloop.com/strongblog/robust-node-applications-error-handling/)(strong 循环博客)

> *你应该**而不是**做的是监听`uncaughtException`事件，当一个异常一路冒泡回到事件循环时发出。使用它一般是[不首选](https://nodejs.org/api/process.html#process_event_uncaughtexception)。*

### **正确记录**

日志对于调试和应用程序活动至关重要。它主要用于开发目的。我们使用`console.log`和`console.error`，但是这些是[同步函数](https://nodejs.org/api/console.html#console_console_1)。

#### **用于调试目的**

你可以使用类似于[调试](https://www.npmjs.com/package/debug)的模块。该模块使您能够使用调试环境变量来控制发送给`console.err()`的调试消息，如果有的话。

#### **用于 app 活动**

一种方法是将它们写入数据库。

看看[我如何使用 mongoose 插件来审计我的应用程序](https://medium.freecodecamp.org/how-to-log-a-node-js-api-in-an-express-js-app-with-mongoose-plugins-efe32717b59)。

另一种方式是写入文件**或**使用日志库，如[温斯顿](https://www.npmjs.com/package/winston)或[班扬](https://www.npmjs.com/package/bunyan)。有关这两个库的详细比较，请参见 StrongLoop 博客文章[比较 Winston 和 Bunyan Node.js 日志](https://strongloop.com/strongblog/compare-node-js-logging-winston-bunyan/)。

### 要求(“。/../../../../../../)乱七八糟

这个问题有不同的解决方法。

如果您发现某个模块越来越受欢迎，并且它在逻辑上独立于应用程序，那么您可以将其转换为私有 npm 模块，并像使用 package.json 中的任何其他模块一样使用它。

运筹学

```
const path  = require('path');
const HOMEDIR  = path.join(__dirname,'..','..');
```

其中`__dirname` 是命名包含当前文件的目录的内置变量，`..`，`..`是到达项目根目录所需的目录树的步数。

简单来说就是:

```
const foo = require(path.join(HOMEDIR,'lib','foo'));
const bar = require(path.join(HOMEDIR,'lib','foo','bar'));
```

在项目中加载任意文件。

如果你有更好的想法，请在下面的评论中告诉我:)

### 将 NODE_ENV 设置为“生产”

**NODE_ENV** 环境变量指定应用程序运行的环境(通常是开发或生产)。提高性能最简单的方法之一就是将`**NODE_ENV**`设置为“生产”

将 **NODE_ENV** 设置为“**生产**使 Express:

*   缓存视图模板。
*   缓存从 CSS 扩展生成的 CSS 文件。
*   生成更少的详细错误消息。

[测试表明](http://apmblog.dynatrace.com/2015/07/22/the-drastic-effects-of-omitting-node_env-in-your-express-js-applications/)这样做可以将应用性能提高三倍！

### **使用流程管理器**

对于生产，你不应该简单地使用`node app.j`——如果你的应用崩溃，它将离线，直到你重新启动它。

Node 最受欢迎的流程管理器有:

*   [strong 循环流程管理器](http://strong-pm.io/)
*   [PM2](https://github.com/Unitech/pm2)
*   [永远](https://www.npmjs.com/package/forever)

我个人使用 **PM2。**

关于三个流程管理器的特性对比，请参见[http://strong-pm.io/compare/](http://strong-pm.io/compare/)。关于这三者的更详细介绍，请参见[快速应用程序流程管理器](https://expressjs.com/en/advanced/pm.html)。

### 在集群中运行您的应用

在多核系统中，您可以通过启动一个进程集群来多次提高节点应用的性能。

一个集群运行该应用的多个实例，理想情况下每个 CPU 内核上运行一个实例。这将在实例之间分配负载和任务。

#### 使用节点的集群模块

使用节点的[集群模块](https://nodejs.org/dist/latest-v4.x/docs/api/cluster.html)可以实现集群。这使得主进程能够产生工作进程。它将传入的连接分配给工作线程。

然而，与其直接使用这个模块，不如使用众多工具中的一个来自动完成。例如[节点-pm](https://www.npmjs.com/package/node-pm) 或[集群-服务](https://www.npmjs.com/package/cluster-service)。

#### 使用 PM2

对于 pm2，您可以通过命令直接使用集群。举个例子，

```
# Start 4 worker processes
pm2 start app.js -i 4

# Auto-detect number of available CPUs and start that many worker processes
pm2 start app.js -i max 
```

如果您遇到任何问题，请随时*进入[联系](https://101node.io)或在下面评论。*
我很乐意帮忙:)

如果你认为这是一本值得一读的书，请不要犹豫鼓掌！

参考资料:[https://express js . com/en/advanced/best-practice-performance . html](https://expressjs.com/en/advanced/best-practice-performance.html)

*最初发布于 2018 年 9 月 30 日 [101node.io](https://101node.io/blog/how-to-write-production-ready-node-express-app/) 。*