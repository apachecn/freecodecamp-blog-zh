# 如何使用 Node、Express 和 MongoDB 构建 RESTful API

> 原文：<https://www.freecodecamp.org/news/build-a-restful-api-using-node-express-and-mongodb/>

在本文中，我们将使用 Node、Express 和 MongoDB 构建一个 RESTful API。我们将创建端点来创建数据、读取数据、更新数据和删除数据(基本的 CRUD 操作)。

但是在我们开始之前，请确保您的系统中已经安装了 Node。如果没有，请到[https://nodejs.org/en/download/](https://nodejs.org/en/download/)下载并安装。

## 让我们先做基本的设置

在空文件夹中，运行以下命令:

```
npm init
```

这个命令将询问您各种细节，比如项目的名称、作者、存储库等等。然后它会在那个文件夹中生成一个 **package.json** 文件。

![Screenshot-2022-02-19-130254](img/14fe8e1ce81794f5f87f5ce89d386890.png)

```
{
  "name": "rest-api-express-mongo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
} 
```

Package.json file

这个 Package.json 文件将包含所有的脚本，比如如何运行应用程序，或者如何测试应用程序，以及所有的依赖关系。

我们现在需要安装一些依赖项。

```
npm i express mongoose nodemon dotenv
```

这里，

1.  Express 将用于中间件来创建各种 CRUD 端点。
2.  使用各种查询管理 MongoDB 中的数据。
3.  每次保存文件时，Nodemon 都要重启服务器。
4.  管理一个**。env** 文件。

所以继续安装它们吧。

安装完成后，创建一个名为 **index.js.** 的文件，这将是我们应用程序的入口点。

而在这个文件中，让我们添加 Express 和**mongose，并运行文件。**

```
`const express = require('express');
const mongoose = require('mongoose');`
```

**现在，将 Express 的内容转移到名为 **app** 的新常量中。**

```
`const express = require('express');
const mongoose = require('mongoose');

const app = express();`
```

**现在，让我们在端口 3000 上监听这个文件的变化。**

```
`const express = require('express');
const mongoose = require('mongoose');

const app = express();

app.use(express.json());

app.listen(3000, () => {
    console.log(`Server Started at ${3000}`)
})`
```

**现在，服务器设置在**端口 3000** 上。让我们编写脚本来启动我们的服务器。我们还添加了**应用。在其中，我们有一个代码片段，允许我们接受 JSON 格式的数据。****

**在 package.json 文件中，添加如下脚本:**

```
`"scripts": {
    "start": "nodemon index.js"
},`
```

**这意味着我们可以使用 **npm start、**来启动我们的服务器，它将使用我们之前安装的Nodemon 包来运行。**

**在终端中键入 npm start，我们将在终端中看到以下输出:**

**![Screenshot-2022-02-19-132326](img/2cb132f6602eb61c5f7ce32c9bf5b658.png)**

## **如何配置 MongoDB 数据库**

**现在，让我们配置 mongoDB 数据库。前往[https://account.mongodb.com/account/login](https://account.mongodb.com/account/login)创建您的帐户，或者如果您已经有帐户，请登录。**

**登录后，我们必须创建一个数据库。**

**![Screenshot-2022-02-19-132848](img/524890a4864c07f76cc71c2580fe4d93.png)**

**所以，创建一个**自由共享集群。****

**它会问你用户名和密码，所以填写这些。**

**![Screenshot-2022-02-19-132958](img/a4ae10320b9c214fb598054809f8fe75.png)**

**然后，添加您的 IP 地址。**

**![Screenshot-2022-02-19-133131](img/12ee5a8a24eb3a62cdfa2aaf13d5bfad.png)**

**单击完成并关闭。**

**我们的集群需要一些时间来完成，所以让我们等待。同时，创建一个名为**的文件。项目文件夹中的 env** 。**

**在群集主页中，单击连接按钮。**

**![Screenshot-2022-02-19-133319](img/dc06381bbee0c29f243bfb9419efa848.png)**

**将出现以下窗口:**

**![Screenshot-2022-02-19-133404](img/73453bd6791fa26bf75eeca5ec388eda.png)**

**单击 MongoDB Compass，它将返回以下字符串。另外，下载并安装 MongoDB Compass。**

**![Screenshot-2022-02-19-133516](img/e34077d8536edf90a9681ad150086c36.png)**

**将您以前使用过的用户名和密码添加到该字符串中。最终的连接字符串如下所示:**

```
`mongodb+srv://nishant:********@cluster0.xduyh.mongodb.net/testDatabase` 
```

**这里，nishant 是用户名，后面是密码，最后是数据库名。**

**所以，把这个字符串粘贴到**中。env** 文件。**

```
`DATABASE_URL = mongodb+srv://nishant:*******@cluster0.xduyh.mongodb.net/testDatabase` 
```

**现在在 MongoDB Compass 中，也添加这个字符串。**

**![Screenshot-2022-02-19-134347](img/b0d205349744134dd06a0182552e49fc.png)**

**然后，单击连接。**

**这里，我们将得到两个默认的数据库。稍后将自动创建第三个。**

**![Screenshot-2022-02-19-134435](img/73c794254e1be734de18caf139a87c33.png)**

**现在，让我们导入。脚本文件 index.js 中的 **env** 文件。**

```
`require('dotenv').config();

const mongoString = process.env.DATABASE_URL`
```

**这里，我们将字符串存储到一个名为 **mongoString 的变量中。****

**现在，让我们使用 Mongoose 将数据库连接到我们的服务器。**

```
`mongoose.connect(mongoString);
const database = mongoose.connection`
```

**现在，我们必须根据数据库连接是成功还是失败来抛出成功或错误消息。**

```
`database.on('error', (error) => {
    console.log(error)
})

database.once('connected', () => {
    console.log('Database Connected');
})`
```

**这里， **database.on** 表示它将连接到数据库，如果连接失败，将抛出任何错误。而 **database.once** 意味着它只会运行一次。如果成功，它将显示一条消息，说明数据库已连接。**

**![Screenshot-2022-02-19-135414-1](img/a37d8d0cd5ae987a7a437472867c033b.png)**

**以下是到目前为止的全部代码:**

```
`require('dotenv').config();

const express = require('express');
const mongoose = require('mongoose');
const mongoString = process.env.DATABASE_URL;

mongoose.connect(mongoString);
const database = mongoose.connection;

database.on('error', (error) => {
    console.log(error)
})

database.once('connected', () => {
    console.log('Database Connected');
})
const app = express();

app.use(express.json());

app.listen(3000, () => {
    console.log(`Server Started at ${3000}`)
})`
```

## **如何为端点创建路由**

**创建一个名为 routes 的文件夹，并在其中创建一个名为 routes.js 的文件。**

**将这个文件导入我们的主脚本文件 index.js。**

```
`const routes = require('./routes/routes');`
```

**同样，让我们使用这个路由文件。**

```
`const routes = require('./routes/routes');

app.use('/api', routes)`
```

**在这里，这个 app.use 需要两件事情。一个是基本端点，另一个是路由的内容。现在，我们所有的端点都将从“/api”开始。**

**我们将得到一个错误，因为我们在 routes 文件中没有任何内容。所以，我们来补充一下。**

```
`const express = require('express');

const router = express.Router()

module.exports = router;`
```

**这里，我们使用来自 Express 的路由器，我们也使用 module.exports 导出它。现在，我们的应用程序将正常工作。**

## **如何编写我们的端点**

**现在，让我们在这个 routes 文件中写下我们的端点。对于以下操作，我们将有五条路线:**

1.  **将数据发布到数据库。**
2.  **从数据库中获取所有数据。**
3.  **根据 ID 获取数据。**
4.  **基于 ID 更新数据。**
5.  **基于 ID 删除数据。**

**因此，让我们为这些动作创建路线:**

```
`//Post Method
router.post('/post', (req, res) => {
    res.send('Post API')
})

//Get all Method
router.get('/getAll', (req, res) => {
    res.send('Get All API')
})

//Get by ID Method
router.get('/getOne/:id', (req, res) => {
    res.send('Get by ID API')
})

//Update by ID Method
router.patch('/update/:id', (req, res) => {
    res.send('Update by ID API')
})

//Delete by ID Method
router.delete('/delete/:id', (req, res) => {
    res.send('Delete by ID API')
})`
```

**我们有五个方法使用了 Post、Get、Patch 和 Delete 的 REST 方法。**

**该路由器将路由作为第一个参数。然后在第二个参数中，它进行回调。**

**在回调中，我们有一个 **res** 和一个 **req** 。 **res** 表示**响应，**和 **req** 表示**请求。**我们使用 **res** 向我们的客户端发送响应，比如邮递员，或者任何前端客户端。我们使用 **req** 来接收来自 Postman 等客户端应用程序或任何前端客户端的请求。**

**然后在回调体中，我们打印一条消息，说明相应的 API 消息。**

**保存这个，并打开 Postman 来检查端点。没有的话下载[邮差](https://www.postman.com/downloads/)。这是一个测试 API 端点的神奇工具。**

**![Screenshot-2022-02-19-141237](img/07c76c88a13f18741346b6fb3e466f22.png)**

**将此地址添加到地址栏中，然后点按“发送”，或按 enter 键。**

**![Screenshot-2022-02-19-141328](img/259c093238e9f03bdd6b92c71544b27f.png)**

**我们将在 Postman 的正文中获得此消息，因为我们只是使用 **res.send.** 发送消息**

**现在，让我们来看一个客户端应用程序的响应。让我们简单地打印一个 ID。**

**我们必须首先更改 **getOne** 函数。我们使用 **req.params.id** 获取 ID，然后使用 **res.send.** 将其发送到客户端应用程序**

```
`//Get by ID Method
router.get('/getOne/:id', (req, res) => {
    res.send(req.params.id)
})`
```

```
`localhost:3000/api/getOne/1000`
```

**将此端点添加到地址栏中。这里，我们使用的是 **getOne** 端点，后跟 ID。然后，单击发送。**

**![Screenshot-2022-02-19-142619](img/5625750ef3b4cbd463cabe6cc147fbc0.png)**

**我们将在 Postman 的响应正文中获得 ID。**

## **如何创建模型**

**现在，让我们创建一个定义数据库结构的模型。**

**创建一个名为 model 的文件夹，并在其中创建一个名为 model.js 的文件**

```
`const mongoose = require('mongoose');

const dataSchema = new mongoose.Schema({
    name: {
        required: true,
        type: String
    },
    age: {
        required: true,
        type: Number
    }
})

module.exports = mongoose.model('Data', dataSchema)`
```

**这里，我们有一个定义数据库结构的模式。它有一个**名字**和一个**年龄**属性。两个字段都有类型，并且都是必需的。**

**然后，我们简单地导出模式模型。**

**现在，将这个模型导入到 **routes.js** 文件中。**

```
`const Model = require('../models/model');`
```

## **如何将数据发送到数据库**

**让我们使用刚刚创建的模型创建要发布的数据体。**

```
`router.post('/post', (req, res) => {
    const data = new Model({
        name: req.body.name,
        age: req.body.age
    })
})`
```

**我们的姓名和年龄正在接受来自**请求主体**的姓名和年龄。我们从客户端应用程序如**邮差**或任何前端客户端如**反应**或**角度获取这些数据。****

**我们还将创建一个 **try-catch** 块来处理成功消息和错误。**

```
`//Post Method
router.post('/post', (req, res) => {
    const data = new Model({
        name: req.body.name,
        age: req.body.age
    })

    try{

    }
    catch(error){

    }
})`
```

**在 try 块中，我们使用 **data.save()** 保存**数据**。然后，我们将数据存储在一个名为 **dataToSave** 的常量中。**

**我们还会在响应正文中发送包含数据的成功消息。**

**在 catch 块中，如果我们得到任何错误，我们就接收任何错误。**

```
`//Post Method
router.post('/post', (req, res) => {
    const data = new Model({
        name: req.body.name,
        age: req.body.age
    })

    try {
        const dataToSave = data.save();
        res.status(200).json(dataToSave)
    }
    catch (error) {
        res.status(400).json({message: error.message})
    }
})`
```

**现在，让我们添加一些来自 Postman 的数据。但在此之前，这个函数需要异步才能工作。因此，我们将使用 async-await。**

```
`router.post('/post', async (req, res) => {
    const data = new Model({
        name: req.body.name,
        age: req.body.age
    })

    try {
        const dataToSave = await data.save();
        res.status(200).json(dataToSave)
    }
    catch (error) {
        res.status(400).json({message: error.message})
    }
})`
```

**如果我们在正文中添加数据并单击 Send，我们将得到以下内容:**

**![Screenshot-2022-02-19-145714](img/4e920cc058c6a015b67d0141565d4f9b.png)**

**它还会生成一个唯一的 ID。打开 MongoDB Compass 应用程序，您将看到数据库和您刚刚创建的这条记录:**

**![Screenshot-2022-02-19-150007](img/2e31c12fa55b5c06d50bef3c45fef41b.png)**

## **如何获取所有数据**

**获取数据也很简单。只有几行代码:**

```
`router.get('/getAll', async (req, res) => {
    try{
        const data = await Model.find();
        res.json(data)
    }
    catch(error){
        res.status(500).json({message: error.message})
    }
})`
```

**这里，我们使用 **Model.find** 方法从数据库中获取所有数据。然后，我们将它以 JSON 格式返回。如果我们有一个错误，我们也会得到它。**

**![Screenshot-2022-02-19-150423](img/c188a5c51b383640a0fdaf7dbb9f0b49.png)**

**如果我们在 Postman 中调用这个端点，我们将在 Postman 主体中获得一个对象数组。**

## **如何根据 ID 获取数据**

**这个也简单。我们只需在一个名为 **findById** 的方法中传递文档的 ID，即 **req.params.id** 。**

```
`//Get by ID Method
router.get('/getOne/:id', async (req, res) => {
    try{
        const data = await Model.findById(req.params.id);
        res.json(data)
    }
    catch(error){
        res.status(500).json({message: error.message})
    }
})`
```

**如果我们单击 Send，我们将获得基于 ID 的数据。**

**![Screenshot-2022-02-19-150808](img/722d344179c1c58e2a01648c438e6636.png)**

## **如何根据 ID 更新和删除数据**

**首先，让我们使用**补丁**方法来定位更新方法。**

```
`//Update by ID Method
router.patch('/update/:id', async (req, res) => {
    try {
        const id = req.params.id;
        const updatedData = req.body;
        const options = { new: true };

        const result = await Model.findByIdAndUpdate(
            id, updatedData, options
        )

        res.send(result)
    }
    catch (error) {
        res.status(400).json({ message: error.message })
    }
})`
```

**这里我们有三个参数，我们在 **findByIdAndUpdate** 方法中传递它们，我们用它们来通过 ID 查找文档并更新它。**

****req.params.id** 是常量 id， **updatedData** 包含 req.body，**选项**指定是否返回主体中的更新数据。**

**现在让我们来测试一下。只需粘贴特定文档的 ID，然后单击发送。也要改变端点。**

**![Screenshot-2022-02-19-152717](img/10857ca70cb93c25d4d8dd8c0bd8f905.png)**

**我们正在使用一个 ID 进行更新，并且它正在被更新。**

**删除也很简单。让我们来实现它:**

```
`//Delete by ID Method
router.delete('/delete/:id', async (req, res) => {
    try {
        const id = req.params.id;
        const data = await Model.findByIdAndDelete(id)
        res.send(`Document with ${data.name} has been deleted..`)
    }
    catch (error) {
        res.status(400).json({ message: error.message })
    }
})`
```

**我们在这里获取 id，然后使用 Model.findByIdAndDelete 删除该字段，同时传递 ID。**

**我们将更新后的数据存储在 const **data** 中。**

**在响应中，我们将收到消息，表明具有特定名称的文档已被删除。**

**如果我们对此进行测试，我们将得到以下结果:**

**![Screenshot-2022-02-19-153557](img/ec84eb1f44750cca2c5121a2a6467df3.png)**

**所以，五种方法都做好了。我们可以发布数据并获取所有数据(也基于 ID)。我们也可以更新和删除它们。**

### **感谢您的阅读！**

**在本文中，您了解了如何使用 Node、Express 和 MongoDB 设计和开发 RESTful API。**

**现在，您可以使用这些端点构建一个全栈应用程序，包括普通的 JavaScript、React、Angular、Next 或 Vue.js。**

**你也可以看看我关于同一主题的视频，[RESTful API——使用 Node、Express 和 MongoDB](https://youtu.be/paxagc55loU) 构建一个 RESTful API**

**请随意从 [Github](https://github.com/nishant-666/Rest-Api-Express-MongoDB) 下载代码并进行实验。**

> **快乐学习。**