# 如何用 React、Express、MongoDB、Heroku、Netlify 搭建全栈认证 App

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-fullstack-authentication-system-with-react-express-mongodb-heroku-and-netlify/>

没有注册和登录功能，几乎不可能构建一个应用程序。但是对于初学者来说，这可能有点棘手。

在本文中，我将指导您创建一个全栈认证应用程序。您将从后端开始，它将由 Express 构建并托管在 [Heroku](https://www.heroku.com/) 上。然后你将完成用 React 创建并托管在 [Netlify](https://www.netlify.com/) 上的前端。

本教程结束时，您将学会如何使用 Nodejs、Express、React、MongoDB、Heroku、Netlify、bcrypt、jsonwebtoken 和 React-Bootstrap 等工具。

## 目录

1.  [第一部分:如何构建后端](#section-1-how-to-build-the-backend)

*   [如何设置数据库](#how-to-setup-the-database)
*   [如何将 Node.js 连接到 MongoDB](#how-to-connect-node-js-to-mongodb)
*   [如何创建用户模型](#how-to-create-the-users-model)
*   [如何创建注册端点](#how-to-create-the-register-endpoint)
*   [如何创建登录端点](#how-to-create-the-login-endpoint)
*   [如何保护端点](#how-to-protect-the-endpoints)
*   [如何托管后端](#how-to-host-the-backend)
*   [让我们回顾一下](#let-s-review)

2.[第二节:如何构建前端](#section-2-how-to-build-the-frontend)

*   [如何构建用户界面](#how-to-build-the-user-interface)
*   [如何注册用户](#how-to-register-a-user)
*   [如何登录用户](#how-to-login-a-user)
*   [如何保护路线](#how-to-protect-the-routes)
*   [如何使用 useEffect 钩子进行 API 调用](#how-to-make-api-calls-using-the-useeffect-hook)
*   [如何构建注销功能](#how-to-build-the-logout-function)
*   [如何托管前端](#how-to-host-the-frontend)
*   [让我们回顾一下](#let-s-review-1)

3.[所有资源和预览](#all-resources-and-previews)

4.[结论](#conclusion)

## 先决条件

本教程假设您已经了解以下基本知识:

*   Nodejs 和 Express。检查这个[教程](https://www.freecodecamp.org/news/build-a-secure-server-with-node-and-express/)否则。
*   [Github](https://github.com/) 。
*   [反应](https://reactjs.org/)。

## 起始代码

*   请在此克隆启动代码[。](https://github.com/EBEREGIT/auth-backend/tree/starter-code)
*   在项目目录中，运行`npm install`来安装依赖项
*   运行`nodemon index`在端口 3000 上服务项目。
*   在您的浏览器上检查`http://localhost:3000/`以确认

```
 $ git clone -b starter-code https://github.com/EBEREGIT/auth-backend 
```

cloning a repo

# 第 1 部分:如何构建后端

后端代表所有用户看不到的功能。这包括数据库设计和 API 端点。

本节将逐步指导您如何构建认证系统的后端。

我们将从使用 MongoDB 建立一个数据库开始，然后我们将创建端点(**登录**和**注册**)，最后在 Heroku 上托管端点。

我们开始吧！

## 如何设置数据库

这一部分将介绍使用 [mongoDB atlas](https://www.mongodb.com/cloud/atlas) 的数据库设置。

你可以在这里创建一个免费账户[。](https://account.mongodb.com/account/register)

### 如何创建新的数据库用户

在您的仪表板上，单击左侧的`Database Access`链接(这将提示您添加一个新的数据库用户)。

![Database Access photo](img/f38067b7791fcb86977be8264b600fc1.png)

user dashboard

点击“添加新数据库用户”按钮，将打开一个`Add New Database User`对话框。

![Add New Database User dialogue box](img/d746cb11b05c96e868b0f424dc7f07a4.png)

`Add New Database User` dialogue box

选择`Password`作为验证方法，并输入您选择的用户名。

然后键入密码或自动生成安全密码。我建议自动生成一个密码并存储在某个地方。你很快就会需要它。

点击`Add User`完成该过程。

![User Created](img/62ba6e92c2b41776ac5a16a739644fab.png)

### 如何创建集群

在侧边菜单上，点击`clusters`。这将带您进入带有按钮的集群页面:`Build a Cluster`。

![Alt Text](img/7fbcfcb7115c2dd9a4d92d84a805dc05.png)

点击按钮，另一页就会出现。

选择`free cluster`。将会打开设置页面。您不能在此页面上进行任何更改。

![Alt Text](img/eaed25d49fb8beaf0e53b5bd8ff3fb01.png)

点击`Create Cluster`。等待集群创建完成。完成后，您的屏幕应该如下所示:

![Alt Text](img/a55289e35153609d5a517909c8436df3.png)

### 如何将用户连接到集群

点击`connect`按钮:

![Alt Text](img/60363762c80be361e4fa0cdaa0c2e3ff.png)

在出现的`Connect to Cluster0`模式中，选择`Connect from Anywhere`并更新设置。

点击`Choose a connection method`按钮:

![Alt Text](img/9707f074f6d346f29ebe5d74bed4fe56.png)

点击`Connect Your Application`。在打开的页面中，确保`DRIVER`是`nodejs`，而`VERSION`是`3.6 or later`。

![Alt Text](img/d310612fa84881cb442b915da840f9b4.png)

复制连接字符串并将其存储在某个地方。你很快就会需要它。

![Alt Text](img/9a950b71f37ec79a72714bad088f35fc.png)

它应该类似于:

```
 mongodb+srv://plenty:<password>@cluster0.z3yuu.mongodb.net/<dbname>?retryWrites=true&w=majority 
```

关闭对话框。

### 如何创建集合(表格)

回到集群页面，点击`COLLECTIONS`。

![Alt Text](img/fe9b422a82024d0c4efc7160848d9c2e.png)

你应该在下面这一页。点击`Add My Own Data`按钮:

![Alt Text](img/968ffeb4d9ee36719a13badd9ae8c69c.png)

在弹出的对话框中，输入一个`database name`和一个`collection name`。我的数据库名是`authDB`，我的收藏名是`users`。

![Alt Text](img/0943fe65fcb987954de58368491c2852.png)

点击`Create`按钮。

祝贺您创建了那个数据库和集合(表)！

## 如何将 Node.js 连接到 MongoDB

让我们回到启动代码。

还记得您生成的数据库名称、连接字符串和密码吗？你马上就会用到它们。

用您生成的密码和您创建的数据库名替换`<password>`和`<dbname>`,如下所示

```
 mongodb+srv://plenty:RvUsNHBHpETniC3l@cluster0.z3yuu.mongodb.net/authDB?retryWrites=true&w=majority 
```

在根文件夹中创建一个文件，并将其命名为`.env`。

创建一个变量`DB_URL`并将连接字符串赋给它:

```
 DB_URL=mongodb+srv://plenty:RvUsNHBHpETniC3l@cluster0.z3yuu.mongodb.net/authDB?retryWrites=true&w=majority 
```

创建一个文件夹，命名为`db`。

在其中创建一个新文件，命名为`dbConnect.js`。

接下来，我们需要安装[猫鼬](https://www.npmjs.com/package/mongoose):

```
 npm i mongoose -s 
```

![Alt Text](img/6b35a7ff8bbfce1a3eedf7f19648df20.png)

在`dbConnect`文件中，用以下代码要求`mongoose`和`env`:

```
 // external imports
const mongoose = require("mongoose");
require('dotenv').config() 
```

创建并导出一个函数来容纳连接:

```
 async function dbConnect() {

}

module.exports = dbConnect; 
```

在该函数中，使用来自`.evn`文件的连接字符串创建了到数据库的连接:

```
 // use mongoose to connect this app to our database on mongoDB using the DB_URL (connection string)
  mongoose
    .connect(
        process.env.DB_URL,
      {
        //   these are options to ensure that the connection is done properly
        useNewUrlParser: true,
        useUnifiedTopology: true,
        useCreateIndex: true,
      }
    ) 
```

使用`then...catch...`块显示连接是否成功:

```
 .then(() => {
      console.log("Successfully connected to MongoDB Atlas!");
    })
    .catch((error) => {
      console.log("Unable to connect to MongoDB Atlas!");
      console.error(error);
    }); 
```

`dbConnect`文件应该是这样的:

```
 // external imports
const mongoose = require("mongoose");
require('dotenv').config()

async function dbConnect() {
  // use mongoose to connect this app to our database on mongoDB using the DB_URL (connection string)
  mongoose
    .connect(
        process.env.DB_URL,
      {
        //   these are options to ensure that the connection is done properly
        useNewUrlParser: true,
        useUnifiedTopology: true,
        useCreateIndex: true,
      }
    )
    .then(() => {
      console.log("Successfully connected to MongoDB Atlas!");
    })
    .catch((error) => {
      console.log("Unable to connect to MongoDB Atlas!");
      console.error(error);
    });
}

module.exports = dbConnect; 
```

在`app.js`文件中，要求`dbConnect`函数并执行:

```
 // require database connection 
const dbConnect = require("./db/dbConnect");

// execute database connection 
dbConnect(); 
```

检查你的终端。如果您没有错过任何步骤，您应该打印出`"Successfully connected to MongoDB Atlas!"`:

![Terminal showing "Successfully connected to MongoDB Atlas!"](img/452acf574972b63e21b14a3b9f4b3020.png)

## 如何创建用户模型

用户模型告诉数据库如何存储用户传递的数据。以下步骤将向您展示如何为用户创建模型:

在`db`文件夹中创建一个文件，并将其命名为`userModel`。

需要`userModel`文件中的`mongoose`:

```
 const mongoose = require("mongoose"); 
```

创建一个常量(`UserSchema`)并将其分配给 mongoose 模式:

```
 const UserSchema = new mongoose.Schema({}) 
```

在模式中，输入所需的两个字段(`email`和`password`)，并为它们分配一个空对象:

```
const UserSchema = new mongoose.Schema({
  email: {},

  password: {},
}) 
```

通过添加一些[mongose 选项](https://mongoosejs.com/docs/guide.html)来指定字段应该如何工作:

```
 email: {
    type: String,
    required: [true, "Please provide an Email!"],
    unique: [true, "Email Exist"],
  },

  password: {
    type: String,
    required: [true, "Please provide a password!"],
    unique: false,
  }, 
```

最后，用下面的代码导出`UserSchema`:

```
 module.exports = mongoose.model.Users || mongoose.model("Users", UserSchema); 
```

上面的代码是这样说的:*“如果还没有同名的表，就创建一个用户表或集合”。*

您已经为用户完成了模型。`user`集合现在已经准备好接收将要传递给它的数据。

## 如何创建注册端点

在本节中，您将创建一个端点，用于向数据库添加用户。请遵循以下步骤:

安装 [bcrypt](https://www.npmjs.com/package/bcrypt) 。我们将用它来散列从用户那里收到的密码。

```
 npm install --save bcrypt 
```

要求`bcrypt`在`app.js`文件的顶部:

```
 const bcrypt = require("bcrypt"); 
```

在您需要数据库的行的正下方需要`userModel`:

```
 const User = require("./db/userModel"); 
```

在`module.exports = app;`线之前创建一个`register`端点:

```
 app.post("/register", (request, response) => {

}); 
```

在将`email`和`password`保存到数据库之前，使用以下代码散列密码:

```
 bcrypt.hash(request.body.password, 10)
  .then()
  .catch() 
```

上面的代码告诉`bcrypt`将从`request body`收到的`password`散列 10 次或 10 轮 salt。

如果哈希成功，继续在`then`块中操作，并将`email`和`hashed password`保存在数据库中，否则在`catch`块中返回一个错误。

在`catch`块中，返回一个错误:

```
 .catch((e) => {
      response.status(500).send({
        message: "Password was not hashed successfully",
        e,
      });
    }); 
```

在`then`块中，保存您现在拥有的数据。创建一个`userModel`的新实例，并收集更新的数据:

```
 .then((hashedPassword) => {
      const user = new User({
        email: request.body.email,
        password: hashedPassword,
      });
}); 
```

将数据保存为:

```
 user.save() 
```

就是这样。如果你停在这一点上，这一切都很好。它保存了，但是你得不到反馈。

要获得反馈，使用`then...catch...`块:

```
 user.save().then((result) => {
        response.status(201).send({
          message: "User Created Successfully",
          result,
        });
      })
      .catch((error) => {
        response.status(500).send({
          message: "Error creating user",
          error,
        });
      }); 
```

最后，`register`端点看起来像这样:

```
 // register endpoint
app.post("/register", (request, response) => {
  // hash the password
  bcrypt
    .hash(request.body.password, 10)
    .then((hashedPassword) => {
      // create a new user instance and collect the data
      const user = new User({
        email: request.body.email,
        password: hashedPassword,
      });

      // save the new user
      user
        .save()
        // return success if the new user is added to the database successfully
        .then((result) => {
          response.status(201).send({
            message: "User Created Successfully",
            result,
          });
        })
        // catch error if the new user wasn't added successfully to the database
        .catch((error) => {
          response.status(500).send({
            message: "Error creating user",
            error,
          });
        });
    })
    // catch error if the password hash isn't successful
    .catch((e) => {
      response.status(500).send({
        message: "Password was not hashed successfully",
        e,
      });
    });
}); 
```

### 如何测试注册端点

如果尚未在终端中启动服务器，请执行以下操作:

![Start your terminal](img/fe13beb1996fabc23aa65e07f84ab838.png)

去邮递员那里测试:

![Alt Text](img/7bc85b5614635f0ad50b1e89a54395d9.png)

去你的 MongoDB 图集。

点击`Collections`，您应该会看到刚刚添加的数据:

![Alt Text](img/b29661cd2639e935bf15c7ea8f26ed99.png)

## 如何创建登录端点

这部分将用 [`jasonwebtoken (JWT)`](https://www.npmjs.com/package/jsonwebtoken) 覆盖登录。最后，您将学会如何交叉检查用户，并将`hashed password`与`plain text password`相匹配。

这些是步骤:

首先，您需要安装 JWT:

```
 npm i jsonwebtoken -s 
```

在`app.js`文件顶部的`const bcrypt = require("bcrypt");`行正下方导入`JWT`:

```
 const jwt = require("jsonwebtoken"); 
```

在`register`端点的正下方，输入以下函数:

```
 app.post("/login", (request, response) => {

}) 
```

检查用户登录时输入的电子邮件是否存在:

```
 User.findOne({ email: request.body.email }) 
```

使用`then...catch...`模块检查上述电子邮件搜索是否成功。如果不成功，在`catch`块中捕获:

```
 User.findOne({ email: request.body.email })
    .then()
    .catch((e) => {
      response.status(404).send({
        message: "Email not found",
        e,
      });
    }); 
```

如果成功，将输入的密码与数据库中的哈希密码进行比较。在`then...`块中进行:

```
 .then((user)=>{
      bcrypt.compare(request.body.password, user.password)
   }) 
```

再次使用`then...catch...`块检查比较是否成功。如果比较不成功，在`catch`块中返回错误信息:

```
 .then((user)=>{
      bcrypt.compare(request.body.password, user.password)
      .then()
      .catch((error) => {
        response.status(400).send({
          message: "Passwords does not match",
          error,
        });
      })
    }) 
```

检查`then`块中的密码是否正确:

```
 .then((passwordCheck) => {

          // check if password matches
          if(!passwordCheck) {
            return response.status(400).send({
              message: "Passwords does not match",
              error,
            });
          }
        }) 
```

如果密码匹配，那么用`jwt.sign()`函数创建一个随机令牌。它需要 3 个参数:`jwt.sign(payload, secretOrPrivateKey, [options, callback])`。这里可以阅读更多[。](https://www.npmjs.com/package/jsonwebtoken#usage)

```
 bcrypt.compare(request.body.password, user.password)
      .then((passwordCheck) => {

          // check if password matches
          if(!passwordCheck) {
            return response.status(400).send({
              message: "Passwords does not match",
              error,
            });
          }

        //   create JWT token
        const token = jwt.sign(
          {
            userId: user._id,
            userEmail: user.email,
          },
          "RANDOM-TOKEN",
          { expiresIn: "24h" }
        );
      }) 
```

最后，返回一条成功消息，其中包含已创建的令牌:

```
 .then((user)=>{
      bcrypt.compare(request.body.password, user.password)
      .then((passwordCheck) => {

          // check if password matches
          if(!passwordCheck) {
            return response.status(400).send({
              message: "Passwords does not match",
              error,
            });
          }

        //   create JWT token
        const token = jwt.sign(
          {
            userId: user._id,
            userEmail: user.email,
          },
          "RANDOM-TOKEN",
          { expiresIn: "24h" }
        );

         //   return success response
         response.status(200).send({
          message: "Login Successful",
          email: user.email,
          token,
        });
      }) 
```

登录端点现在看起来像这样:

```
 // login endpoint
app.post("/login", (request, response) => {
  // check if email exists
  User.findOne({ email: request.body.email })

    // if email exists
    .then((user) => {
      // compare the password entered and the hashed password found
      bcrypt
        .compare(request.body.password, user.password)

        // if the passwords match
        .then((passwordCheck) => {

          // check if password matches
          if(!passwordCheck) {
            return response.status(400).send({
              message: "Passwords does not match",
              error,
            });
          }

          //   create JWT token
          const token = jwt.sign(
            {
              userId: user._id,
              userEmail: user.email,
            },
            "RANDOM-TOKEN",
            { expiresIn: "24h" }
          );

          //   return success response
          response.status(200).send({
            message: "Login Successful",
            email: user.email,
            token,
          });
        })
        // catch error if password does not match
        .catch((error) => {
          response.status(400).send({
            message: "Passwords does not match",
            error,
          });
        });
    })
    // catch error if email does not exist
    .catch((e) => {
      response.status(404).send({
        message: "Email not found",
        e,
      });
    });
}); 
```

### 如何测试登录端点

让我们尝试使用在上一部分中注册的凭据登录。查看成功登录时生成的随机`token`:

![Login Success Image](img/b9cddc8f7d4c3de46b609837c097a5a0.png)

如果`email`不正确或不存在，您会得到:

![Email incorrect or does not exist](img/fc1f37eb7616166250fe6671ea038494.png)

如果`password`不正确，您会看到:

![Password incorrect](img/e0ac5f6f9db84c477642e17498cdf624.png)

## 如何保护端点

这一部分将教你如何保护一些端点免受未认证用户的攻击。

### 创建两个端点

您需要两个端点才能看到它是如何工作的。复制以下端点并粘贴到最后一行之前的`app.js`文件中。

```
// free endpoint
app.get("/free-endpoint", (request, response) => {
  response.json({ message: "You are free to access me anytime" });
});

// authentication endpoint
app.get("/auth-endpoint", (request, response) => {
  response.json({ message: "You are authorized to access me" });
}); 
```

### 创建身份验证功能

接下来，您需要创建一个函数来保护特定的端点免受未经身份验证的用户的攻击。

在根目录下创建一个文件，命名为`auth.js`。

在文件顶部导入`jasonwebtoken`:

```
 const jwt = require("jsonwebtoken"); 
```

创建并导出一个异步函数，授权码将存在于该函数中:

```
 module.exports = async (request, response, next) => {

} 
```

在该函数中，使用`try...catch...`块检查用户是否登录:

```
 try {

    } catch (error) {
        response.status(401).json({
            error: new Error("Invalid request!"),
          });
    } 
```

在`try{}`块中，从`authorization header`获取认证令牌:

```
 //   get the token from the authorization header
    const token = await request.headers.authorization.split(" ")[1]; 
```

检查生成的令牌是否与最初输入的令牌字符串( **RANDOM-TOKEN** )匹配:

```
 //check if the token matches the supposed origin
    const decodedToken = await jwt.verify(
      token,
      "RANDOM-TOKEN"
    ); 
```

将`decodedToken`的详细信息传递给`user`常量:

```
 // retrieve the user details of the logged in user
    const user = await decodedToken; 
```

将`user`传递到端点:

```
 // pass the the user down to the endpoints here
    request.user = user; 
```

最后，打开通往终点的路:

```
 // pass down functionality to the endpoint
    next(); 
```

`auth.js`文件现在看起来像这样:

```
 const jwt = require("jsonwebtoken");

module.exports = async (request, response, next) => {
  try {
    //   get the token from the authorization header
    const token = await request.headers.authorization.split(" ")[1];

    //check if the token matches the supposed origin
    const decodedToken = await jwt.verify(token, "RANDOM-TOKEN");

    // retrieve the user details of the logged in user
    const user = await decodedToken;

    // pass the user down to the endpoints here
    request.user = user;

    // pass down functionality to the endpoint
    next();

  } catch (error) {
    response.status(401).json({
      error: new Error("Invalid request!"),
    });
  }
}; 
```

### 如何保护端点

这是最后也是最简单的一步。首先将认证函数导入到`app.js`文件中，如下所示:

```
 const auth = require("./auth"); 
```

在`app.js`文件的认证端点中添加`auth`作为第二个参数:

```
 // authentication endpoint
app.get("/auth-endpoint", auth, (request, response) => {
  response.json({ message: "You are authorized to access me" });
}); 
```

仅此而已。这就是你需要保护的路线。让我们来测试一下。

### 如何测试终端保护

如果用户未登录:

![Alt Text](img/7c02a26299121f9de3bbdd5da64ce203.png)

如果用户已登录:

*   像这样登录:

![Alt Text](img/dfdb34e0165ce0d8e451c3bbf572ee24.png)

复制令牌，然后在`postman`上打开一个新标签。

在认证类型中选择`bearer token`。

将令牌粘贴到`token`字段并发送请求。

![Alt Text](img/eeb5a12117cc2642b06b646652ded8bc.png)

### 如何处理 CORS 错误

最后一件事！你需要处理 CORS 错误。这将允许前端用户毫无问题地使用您创建的 API。

为此，导航到`app.js`文件。

在`dbConnect()`行的正下方添加以下代码:

```
 // Curb Cores Error by adding a header here
app.use((req, res, next) => {
  res.setHeader("Access-Control-Allow-Origin", "*");
  res.setHeader(
    "Access-Control-Allow-Headers",
    "Origin, X-Requested-With, Content, Accept, Content-Type, Authorization"
  );
  res.setHeader(
    "Access-Control-Allow-Methods",
    "GET, POST, PUT, DELETE, PATCH, OPTIONS"
  );
  next();
}); 
```

有了这个，你就是后端认证冠军！！

## 如何托管后端

这一部分教你如何在 Heroku 上托管后端应用程序。到最后，它将在互联网上对每个人开放。请遵循以下步骤:

在 [Heroku](https://heroku.com/) 上创建一个帐户。

如果您已创建帐户，系统可能会提示您创建应用程序(即存放应用程序的文件夹)。创造它。我的名字叫`nodejs-mongodb-auth-app`。

转到你的应用仪表板:

![Alt Text](img/3a5c3ebe04c1eb444b7d8102077591ea.png)

选择`GitHub`部署方式:

![Alt Text](img/120c94959d728f791d647feb3f548e81.png)

搜索并选择一个回购。

点击`connect`:

![Alt Text](img/44cc3e544046a76b2dca0570b5c90c99.png)

选择您想要部署的分支(在我的例子中，它是`master`分支):

![Alt Text](img/8eac9e7ebc31575fa1a227be29cefdb7.png)

如上图所示，通过点击`Enable automatic deployment`按钮启用自动部署。

点击手动部署中的`Deploy`按钮:

![Alt Text](img/b3fff43b82fe34a82a90ac2b9469762c.png)

对于后续部署，您不必做所有这些工作。

现在您有一个按钮，告诉您在构建完成后“查看站点”。点击它。这将在一个新标签页中打开你的应用。

![Alt Text](img/0c97e0a07c29c3926b3aefc9724475ce.png)

哦不不！！！！一只虫子？应用错误？

这只是一个小问题。这是您在进行部署时永远不要忘记的事情。大多数主机服务都需要它。

### 如何修复 Heroku 应用程序错误

在项目的根目录下，创建一个文件，并将其命名为`Procfile`。它没有扩展。

在该文件中，输入以下内容:

```
web: node index.js 
```

![Alt Text](img/04e1206c222c8f1dafffb0eab7eb002f.png)

这将 Heroku 定向到服务器文件(`index.js`)，这是应用程序的入口点。如果您的服务器在不同的文件中，请根据需要进行修改。

保存文件，并将新的更改推送到 GitHub。

等待 2 到 5 分钟，让 Heroku 自动检测您的 GitHub repo 中的更改，并在应用程序上反映这些更改。

现在，您可以刷新错误页面，看到您辛勤工作得到了回报:

![Alt Text](img/7a5aa1e49a92d5847013e679a01256b4.png)

### 如何添加 MongoDB

您可能已经注意到其他路由不起作用。这是因为你没有包括数据库。

记住数据库的 URL 在`.env`文件中。但是`.env`文件在你推送后并没有包含在 GitHub 上的项目中。所以你得直接把 MongoDB 的网址加到 Heroku app 里。

让我们这样做...

导航至您的应用程序设置:`https://dashboard.heroku.com/apps/<your_app_name>/settings`

![Alt Text](img/0f9af4ff7e896ad2e823b6f54b3892ef.png)

向下滚动到`Config Vars`部分。

添加数据库的键和值:

![Alt Text](img/90679c5d0e3c9ed7e093a532d09b16d6.png)

仅此而已！你的应用现在应该工作正常。

### 托管后如何测试端点

测试它是否工作的最简单方法是尝试登录端点。

![Alt Text](img/f22150285e8ce1434fb54f6d1aa0ea8d.png)

成功了！

## 让我们回顾一下

本节的目的是创建一个后端身份验证应用程序。在开发应用程序的过程中，您了解了`bycrypt`、`jsonwebtoken`、`heroku`、`CORS`、`database`、`MongoDB`、`Clusters`、`collections`和`models`。

您首先在 MongoDB Atlas 上建立了一个数据库。然后，您继续创建两个端点——注册和登录——使我们能够将用户输入到数据库中，并交叉检查该用户是否存在。接下来，您看到了如何保护端点以及如何处理 CORS 错误。最后，您学习了如何托管您构建的应用程序。

这部分的代码可以在[这里](https://github.com/EBEREGIT/auth-backend)找到。这里是 Heroku [的现场直播。](https://nodejs-mongodb-auth-app.herokuapp.com/)

下一节将通过构建一个前端来连接用户和后端，从而帮助我们使这个应用程序对最终用户有用。

# 第 2 节:如何构建前端

前端表示用户可以看到的和与之交互的内容。您将构建一个用户界面，使用户能够通过单击按钮与后端进行通信。

您将从使用 React-Bootstrap 构建 UI 开始。接下来，您将使用`axios`将 UI 连接到端点。然后，您将保护一些路线，防止未经授权的用户。最后，您将在 Netlify 上托管前端。

我们开始工作吧！

## 如何构建用户界面

这一部分将向您介绍 React-Bootstrap，并指导您构建注册和登录表单。

[Bootstrap](https://getbootstrap.com/) 这些年来已经偷走了许多开发者的心。这是可以理解的，因为它可以帮助你编写更短更简洁的代码，节省时间，而且它足够复杂，可以处理很多开发人员关心的问题，尤其是如果你不喜欢编写 CSS 的话。

还有 [React](https://reactjs.org/) ，它已经成为最流行的前端 JavaScript 框架/库之一。它周围也建起了一个大型社区。

为了确保 React 的开发更加容易和快速，Bootstrap 已经开发了一个新的代码库，名为 [React-Bootstrap](https://react-bootstrap.netlify.app/) 。

React-Bootstrap 仍然是 Bootstrap，但是它已经被设计成适合 React。这确保了在构建应用程序时很少或没有错误。

### 为什么要用 React-Bootstrap 而不是 Bootstrap？

React-Bootstrap 是专门为 React 应用程序构建和定制的。这意味着它更加兼容。

React-Bootstrap 代码通常比 Bootstrap 代码短。例如，如果要在一行中创建三个网格的列，可以通过以下方式实现:

使用引导程序:

```
 <div class="container">
  <div class="row">
    <div class="col-sm">
      One of three columns
    </div>
    <div class="col-sm">
      two of three columns
    </div>
    <div class="col-sm">
      three of three columns
    </div>
  </div>
</div> 
```

使用 React 引导程序:

```
 <Container>
  <Row>
    <Col>One of three columns</Col>
    <Col>two of three columns</Col>
    <Col>three of three columns</Col>
  </Row>
</Container> 
```

### 如何使用反应引导

以下步骤将向您展示如何使用 React-Bootstrap 创建一个简单的 UI:

#### 如何设置项目

创建一个 React 项目并将其命名为`react-auth`。

```
 npx create-react-app react-auth 
```

在终端中打开项目，并导航到项目文件夹。我会用 VS 代码。

```
 cd react-auth 
```

安装 React 引导程序:

```
 npm install react-bootstrap bootstrap 
```

在`index.js`文件中导入引导 CSS 文件:

```
 import 'bootstrap/dist/css/bootstrap.min.css'; 
```

### 如何创建组件

这一部分将向您展示如何使用已经制作好的组件来创建 UI。您将在这里创建注册和登录表单。

在`src`文件夹中创建一个新文件。取名:`Register.js`。

在文件中，从以下代码开始:

```
 import React from 'react'

export default function Register() {
    return (
        <>

        </>
    )
} 
```

在`return`语句中输入以下代码:

```
 <h2>Register</h2>
      <Form>
        {/* email */}
        <Form.Group controlId="formBasicEmail">
          <Form.Label>Email address</Form.Label>
          <Form.Control type="email" placeholder="Enter email" />
        </Form.Group>

        {/* password */}
        <Form.Group controlId="formBasicPassword">
          <Form.Label>Password</Form.Label>
          <Form.Control type="password" placeholder="Password" />
        </Form.Group>

        {/* submit button */}
        <Button variant="primary" type="submit">
          Submit
        </Button>
      </Form> 
```

现在，您必须通知 Bootstrap 您想要使用`Form`和`Button`组件。所以在顶部导入它们:

```
 import { Form, Button } from "react-bootstrap"; 
```

您也可以选择单独完成，如下所示:

```
 import Form from 'react-bootstrap/Form'
import Button from 'react-bootstrap/Button' 
```

在页面上显示注册组件。首先，用以下代码替换`App.js`文件中的代码:

```
 import { Container, Col, Row } from "react-bootstrap";
import "./App.css";

function App() {
  return (
    <Container>
      <Row>

      </Row>
    </Container>
  );
}

export default App; 
```

在`Row`组件中，输入以下内容:

```
 <Col xs={12} sm={12} md={6} lg={6}></Col>
    <Col xs={12} sm={12} md={6} lg={6}></Col> 
```

这将确保大型和中型设备有两列，而小型和超小型设备每行有一列。

在第一列中，添加您创建的`Register`组件，并将其导入到文件的顶部。`App.js`文件将如下所示:

```
 import { Container, Col, Row } from "react-bootstrap";
import Register from "./Register";

function App() {
  return (
    <Container>
      <Row>
        <Col xs={12} sm={12} md={6} lg={6}>
          <Register />
        </Col>

        <Col xs={12} sm={12} md={6} lg={6}></Col>
      </Row>
    </Container>
  );
}

export default App; 
```

在终端中运行`npm start`并在浏览器上查看输出。

![Alt Text](img/a041369b2f06b905b55251f6ae4af95d.png)

您会注意到只有一列被占用。现在您的工作是用与注册组件相同的代码创建一个登录组件。然后添加到第二列。查看下面我的输出:

![Alt Text](img/ecf074a3d403e59b856f0f564940912c.png)

瓦拉！现在，您可以利用 React-Bootstrap 更快地创建 React 应用程序。

现在，您将开始将这些表单连接到后端。

## 如何注册用户

这一部分将带您将注册表单连接到`register`端点:`https://nodejs-mongodb-auth-app.herokuapp.com/register`。

导航到`Register.js`文件。

设置`email`、`password`和`register`的初始状态。

```
 const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [register, setRegister] = useState(false); 
```

为`email`和`password`输入字段设置一个`name`和`value`属性；

```
 {/* email */}
        <Form.Group controlId="formBasicEmail">
          <Form.Label>Email address</Form.Label>
          <Form.Control
            type="email"
            name="email"
            value={email}
            placeholder="Enter email"
          />
        </Form.Group>

        {/* password */}
        <Form.Group controlId="formBasicPassword">
          <Form.Label>Password</Form.Label>
          <Form.Control
            type="password"
            name="password"
            value={password}
            placeholder="Password"
          />
        </Form.Group> 
```

此时，您会注意到您不能再在注册表字段中输入内容。这是因为您没有将字段设置为从以前的状态更新到当前状态。

让我们这样做...

将`onChange={(e) => setEmail(e.target.value)}`
和`onChange={(e) => setPassword(e.target.value)}`分别添加到`email`和`password`输入栏中:

```
 {/* email */}
        <Form.Group controlId="formBasicEmail">
          <Form.Label>Email address</Form.Label>
          <Form.Control
            type="email"
            name="email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
            placeholder="Enter email"
          />
        </Form.Group>

        {/* password */}
        <Form.Group controlId="formBasicPassword">
          <Form.Label>Password</Form.Label>
          <Form.Control
            type="password"
            name="password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            placeholder="Password"
          />
        </Form.Group> 
```

现在您可以在表单域中键入内容，因为它会根据您键入的内容更新状态。

分别将`onSubmit={(e)=>handleSubmit(e)}`和`onClick={(e)=>handleSubmit(e)}`添加到`form`和`button`元素中。

`onSubmit`使用`Enter`键启用表单提交，而`onClick`通过点击按钮启用表单提交。现在表单看起来像这样:

```
 <Form onSubmit={(e)=>handleSubmit(e)}>
        {/* email */}
        <Form.Group controlId="formBasicEmail">
          <Form.Label>Email address</Form.Label>
          <Form.Control
            type="email"
            name="email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
            placeholder="Enter email"
          />
        </Form.Group>

        {/* password */}
        <Form.Group controlId="formBasicPassword">
          <Form.Label>Password</Form.Label>
          <Form.Control
            type="password"
            name="password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            placeholder="Password"
          />
        </Form.Group>

        {/* submit button */}
        <Button
          variant="primary"
          type="submit"
          onClick={(e) => handleSubmit(e)}
        >
          Register
        </Button>
      </Form> 
```

为了测试这是否有效，在`return`行之前创建以下函数:

```
 const handleSubmit = (e) => {
    // prevent the form from refreshing the whole page
    e.preventDefault();
    // make a popup alert showing the "submitted" text
    alert("Submited");
  } 
```

如果您单击按钮或按回车键，这应该是您的结果:

![Testing the handleSubmit function](img/2a0073dcc0c84e4a3933521ff32ffe37.png)

### 如何构建`handleSubmit`函数

现在从`handleSubmit`函数中删除`alert`语句。

安装`axios`。您将使用它来调用端点或将前端连接到后端，视情况而定。

```
 npm i axios 
```

在文件顶部导入`axios`:

```
 import axios from "axios"; 
```

在 handleSubmit 函数中，构建`axios`成功连接前端和后端所需的配置。

```
 // set configurations
    const configuration = {
      method: "post",
      url: "https://nodejs-mongodb-auth-app.herokuapp.com/register",
      data: {
        email,
        password,
      },
    }; 
```

`method`告诉数据将如何被处理，`url`是被调用的端点，`data`包含后端期望的所有输入或`request body`。

配置完成后，打电话。API 调用只是一行语句:

```
 axios(configuration) 
```

至此，API 调用已经完成。但是，您需要确保它成功了。并可能向用户展示结果。

为了解决这个问题，您将使用一个 then...捕捉...阻止。

现在你有了这个:

```
 // make the API call
    axios(configuration)
    .then((result) => {console.log(result);})
    .catch((error) => {console.log(error);}) 
```

注册一个新用户，并检查控制台的结果。您得到的结果应该是这样的:

![Alt Text](img/6b4c301087b94b0fad0a16c382c0d340.png)

当然，您不会将您的用户引导到控制台来检查他们的注册结果。你需要一种与用户交流的方式。

用以下代码替换该代码:

```
 // make the API call
    axios(configuration)
      .then((result) => {
        setRegister(true);
      })
      .catch((error) => {
        error = new Error();
      }); 
```

通过将`register`设置为`true`，您现在可以知道注册过程何时完成。在`Form`元素中使用下面的代码告诉用户:

```
 {/* display success message */}
        {register ? (
          <p className="text-success">You Are Registered Successfully</p>
        ) : (
          <p className="text-danger">You Are Not Registered</p>
        )} 
```

该代码是一个条件语句，用于在`register`为`true`时显示成功消息。现在试试吧！

![Alt Text](img/3048b3a5183fc8a2b51403ced05b261b.png)

如果你得到了和上面一样的结果，那么你做到了！

你太棒了。

## 如何登录用户

现在是时候把注意力转向`Login.js`文件了。

如下设置`email`、`password`和`login`的初始状态:

```
 const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [login, setLogin] = useState(false); 
```

为`email`和`password`输入字段设置一个`name`和`value`属性；

```
 {/* email */}
        <Form.Group controlId="formBasicEmail">
          <Form.Label>Email address</Form.Label>
          <Form.Control
            type="email"
            name="email"
            value={email}
            placeholder="Enter email"
          />
        </Form.Group>

        {/* password */}
        <Form.Group controlId="formBasicPassword">
          <Form.Label>Password</Form.Label>
          <Form.Control
            type="password"
            name="password"
            value={password}
            placeholder="Password"
          />
        </Form.Group> 
```

将`onChange={(e) => setEmail(e.target.value)}`
和`onChange={(e) => setPassword(e.target.value)}`分别添加到`email`和`password`输入栏。这是我的:

```
 {/* email */}
        <Form.Group controlId="formBasicEmail">
          <Form.Label>Email address</Form.Label>
          <Form.Control
            type="email"
            name="email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
            placeholder="Enter email"
          />
        </Form.Group>

        {/* password */}
        <Form.Group controlId="formBasicPassword">
          <Form.Label>Password</Form.Label>
          <Form.Control
            type="password"
            name="password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            placeholder="Password"
          />
        </Form.Group> 
```

现在，您可以在表单中键入内容。

分别将`onSubmit={(e)=>handleSubmit(e)}`和`onClick={(e)=>handleSubmit(e)}`添加到`form`和`button`元素中。

`onSubmit`使用`Enter`键启用表单提交，而`onClick`通过点击按钮启用表单提交。

现在表单看起来像这样:

```
 <Form onSubmit={(e)=>handleSubmit(e)}>
        {/* email */}
        <Form.Group controlId="formBasicEmail">
          <Form.Label>Email address</Form.Label>
          <Form.Control
            type="email"
            name="email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
            placeholder="Enter email"
          />
        </Form.Group>

        {/* password */}
        <Form.Group controlId="formBasicPassword">
          <Form.Label>Password</Form.Label>
          <Form.Control
            type="password"
            name="password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            placeholder="Password"
          />
        </Form.Group>

        {/* submit button */}
        <Button
          variant="primary"
          type="submit"
          onClick={(e) => handleSubmit(e)}
        >
          Login
        </Button>
      </Form> 
```

为了测试这是否有效，在`return`行之前创建以下函数:

```
 const handleSubmit = (e) => {
    // prevent the form from refreshing the whole page
    e.preventDefault();
    // make a popup alert showing the "submitted" text
    alert("Submited");
  } 
```

如果您点击按钮或按下`Enter`键，这应该是您的结果:

![Testing the handleSubmit function](img/2a0073dcc0c84e4a3933521ff32ffe37.png)

### 如何构建`handleSubmit`函数

现在您需要从`handleSubmit`函数中移除`alert`语句。

在文件顶部导入`axios`:

```
 import axios from "axios"; 
```

在`handleSubmit`函数中，构建`axios`成功连接前端和后端所需的配置。

```
 // set configurations
    const configuration = {
      method: "post",
      url: "https://nodejs-mongodb-auth-app.herokuapp.com/login",
      data: {
        email,
        password,
      },
    }; 
```

`method`告诉数据将如何被处理，`url`是访问 API 函数的端点，`data`包含后端期望的所有输入或`request body`。

配置完成后，打电话。

```
 // make the API call
    axios(configuration)
    .then((result) => {console.log(result);})
    .catch((error) => {console.log(error);}) 
```

尝试登录一个用户并检查控制台的结果

![Alt Text](img/f0be16bbd08621f8a155a3e8dad36e3b.png)

用以下代码替换该代码:

```
 // make the API call
    axios(configuration)
      .then((result) => {
        setLogin(true);
      })
      .catch((error) => {
        error = new Error();
      }); 
```

通过将`login`设置为`true`，您现在可以知道登录过程何时完成。用下面的代码在`Form`元素中实现:

```
 {/* display success message */}
        {login ? (
          <p className="text-success">You Are Logged in Successfully</p>
        ) : (
          <p className="text-danger">You Are Not Logged in</p>
        )} 
```

该代码是一个条件语句，用于在`login`为`true`时显示成功消息。试试看。

![Alt Text](img/61d9e32241fc5dc827dfcd9d79ebda06.png)

如果你得到了和上面一样的结果，那么你做到了！

你是个摇滚明星！

## 如何保护路线

如果任何用户仍然可以以某种方式访问应用程序，那么身份验证将是无用的。也许是通过在地址栏中键入所需的 URL。在这一部分中，您将能够保护每一条需要身份验证才能访问的路由。

首先，您将创建另外两个组件。接下来，您将为这些组件设置路线。最后，您将创建一个组件来保护路由，并将该组件应用于所需的路由。

### 首先，创建两个组件

在`src`目录下创建一个新文件，并将其命名为`FreeComponent.js`

该文件应包含以下内容:

```
 import React from "react";

export default function FreeComponent() {
  return (
    <div>
      <h1 className="text-center">Free Component</h1>
    </div>
  );
} 
```

创建一个文件，命名为`AuthComponent.js`。该文件应包含以下内容:

```
 import React from "react";

export default function AuthComponent() {
  return (
    <div>
      <h1 className="text-center">Auth Component</h1>
    </div>
  );
} 
```

### 如何设置路线

安装`react-router-dom`:

```
 npm install --save react-router-dom 
```

导航至`index.js`文件。

导入`BrowserRouter`:

```
 import { BrowserRouter } from "react-router-dom"; 
```

用`</BrowserRouter>`组件包裹`<App>`组件。所以`index.js`的文件现在看起来是这样的:

```
 import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";
import "bootstrap/dist/css/bootstrap.min.css";
import { BrowserRouter } from "react-router-dom";

ReactDOM.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>,
  document.getElementById("root")
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals(); 
```

现在导航到`App.js`文件。

导入文件顶部的`Switch`和`Route`:

```
 import { Switch, Route } from "react-router-dom"; 
```

用以下代码替换`Account`组件:

```
 <Switch>
        <Route exact path="/" component={Account} />
        <Route exact path="/free" component={FreeComponent} />
        <Route exact path="/auth" component={AuthComponent} />
      </Switch> 
```

你会注意到什么都没有改变。这是因为传送时帐户组件仍然是默认组件。但是你现在可以使用多条路线。

在`React Authentication Tutorial`标题下添加导航链接:

```
 <Row>
        <Col className="text-center">
          <h1>React Authentication Tutorial</h1>

          <section id="navigation">
            <a href="/">Home</a>
            <a href="/free">Free Component</a>
            <a href="/auth">Auth Component</a>
          </section>
        </Col>
      </Row> 
```

导航至`index.css`添加以下美学风格:

```
 #navigation{
  margin-top: 5%;
  margin-bottom: 5%;
}

#navigation a{
  margin-right: 10%;
}

#navigation a:last-child{
  margin-right: 0;
} 
```

### 受保护的路由组件

成功设置路由后，您现在想要保护一个路由(即`AuthComponent`)。为此，您需要创建一个新的组件，在允许用户访问该路由之前，帮助检查是否满足了特定的条件。

在这种情况下，您将使用在`login`期间生成的令牌。因此，在创建这个`ProtectedRoute`组件之前，让我们从`Login`组件获取令牌，并使它在应用程序的所有部分都可用。

#### 如何获得令牌

安装`universal-cookie`。这是一个 cookie 包，帮助我们在整个应用程序中共享一个值或变量:

```
 npm i universal-cookie -s 
```

导航到`Login.js`文件。

在顶部导入`universal-cookie`并初始化:

```
 import Cookies from "universal-cookie";
const cookies = new Cookies(); 
```

接下来，在`axios`调用的`then`块中添加以下代码:

```
 // set the cookie
        cookies.set("TOKEN", result.data.token, {
          path: "/",
        }); 
```

上面的代码用`cookie.set()`设置了 cookie。它有三个参数:cookie 的`Name`(这里是`"TOKEN"`，但它可以是您选择的任何东西)、cookie 的`Value`(`result.data.token`)，以及您希望它在哪个页面或路径上可用(将`path`设置为`"/"`会使 cookie 在所有页面上都可用)。希望这是有意义的。

在`cookie.set()`下面，添加以下代码行，在成功登录后将用户重定向到`authComponent`

```
 // redirect user to the auth page
        window.location.href = "/auth"; 
```

#### 如何创建受保护的路由组件

由于令牌现在在整个应用程序中都是可用的，所以您现在可以在所有已经创建或将要创建的组件或页面上访问它。让我们继续...

创建一个文件名为`ProtectedRoutes.js`的文件。

在文件中输入以下代码:

```
 import React from "react";
import { Route, Redirect } from "react-router-dom";
import Cookies from "universal-cookie";
const cookies = new Cookies();

// receives component and any other props represented by ...rest
export default function ProtectedRoutes({ component: Component, ...rest }) {
  return (

    // this route takes other routes assigned to it from the App.js and return the same route if condition is met
    <Route
      {...rest}
      render={(props) => {
        // get cookie from browser if logged in
        const token = cookies.get("TOKEN");

        // returns route if there is a valid token set in the cookie
        if (token) {
          return <Component {...props} />;
        } else {
          // returns the user to the landing page if there is no valid token set
          return (
            <Redirect
              to={{
                pathname: "/",
                state: {
                  // sets the location a user was about to access before being redirected to login
                  from: props.location,
                },
              }}
            />
          );
        }
      }}
    />
  );
} 
```

停下来。停下来。！在`ProtectedRoutes`组件中发生了什么？

这就像一个模板。改变的是`ProtectedRoutes`组件所基于的条件。在这种情况下，它基于登录时从 cookie 接收的`token`。因此，在另一个应用中，条件可能不同。

这里发生的事情是这样的:`ProtectedRoutes`组件接收一个`component`，然后决定该组件是否应该返回给用户。

为了做出这个决定，它检查是否有来自 cookie 的有效的`token`(在成功登录时设置令牌)。如果令牌是`undefined`，那么它将重定向到默认的`path`(在本例中是登录页面)。

代码中的注释也将帮助您理解组件中正在发生的事情。耐心跟随...

### 如何使用`ProtectedRoutes`组件

是时候使用`ProtectedRoutes`组件来保护 AuthComponent 了，因为它应该只对经过身份验证的用户开放。

导航到`App.js`文件。

导入`ProtectedRoutes`组件:

```
 import ProtectedRoutes from "./ProtectedRoutes"; 
```

将`<Route exact path="/auth" component={AuthComponent} />`替换为`<ProtectedRoutes path="/auth" component={AuthComponent} />`。

此时的`App.js`看起来像这样:

```
 import { Switch, Route } from "react-router-dom";
import { Container, Col, Row } from "react-bootstrap";
import Account from "./Account";
import FreeComponent from "./FreeComponent";
import AuthComponent from "./AuthComponent";
import ProtectedRoutes from "./ProtectedRoutes";

function App() {
  return (
    <Container>
      <Row>
        <Col className="text-center">
          <h1>React Authentication Tutorial</h1>

          <section id="navigation">
            <a href="/">Home</a>
            <a href="/free">Free Component</a>
            <a href="/auth">Auth Component</a>
          </section>
        </Col>
      </Row>

      {/* create routes here */}
      <Switch>
        <Route exact path="/" component={Account} />
        <Route exact path="/free" component={FreeComponent} />
        <ProtectedRoutes path="/auth" component={AuthComponent} />
      </Switch>
    </Container>
  );
}

export default App; 
```

尝试在不登录的情况下访问`http://localhost:3000/auth`,注意它是如何将你重定向到登录页面的。

![Alt Text](img/87885ed5c4be76133d1e0f6ccaaf251c.png)

That is amazing. Right?

## 如何使用`useEffect`钩子进行 API 调用

> [React 挂钩](https://reactjs.org/docs/hooks-overview.html#:~:text=Hooks%20are%20functions%20that%20let,if%20you'd%20like.))是让你从功能组件中“挂钩”React 状态和生命周期特性的功能。钩子在类内部不起作用——它们让你在没有类的情况下使用 React。

钩子的例子包括`useState`、`useEffect`和`useRef`。

这一部分将向您展示如何使用`useEffect`钩子进行 API 调用。`useEffect`钩子对`functional component`的反应和`componentDidMount()`对`class component`的反应一样

以下是您将呼叫的端点:

*   **自由端点:** `https://nodejs-mongodb-auth-app.herokuapp.com/free-endpoint`
*   **受保护端点:** `https://nodejs-mongodb-auth-app.herokuapp.com/auth-endpoint`

### 如何调用自由端点

按照下面的步骤调用`https://nodejs-mongodb-auth-app.herokuapp.com/free-endpoint`。

导航到`FreeComponent.js`文件。

通过调整您的`react`导入行，导入`useEffect`和`useState`，如下所示:

```
 import React, { useEffect, useState,  } from "react"; 
```

导入`axios`:

```
 import axios from "axios"; 
```

设置`message`的初始状态:

```
 const [message, setMessage] = useState(""); 
```

在`return`语句的正上方，声明`useEffect`函数:

```
 useEffect(() => {

  }, []) 
```

空数组(即`[]`)对于避免 API 调用完成后继续执行非常重要。

在功能中，设置以下配置:

```
 useEffect(() => {
    // set configurations for the API call here
    const configuration = {
      method: "get",
      url: "https://nodejs-mongodb-auth-app.herokuapp.com/free-endpoint",
    };
  }, []) 
```

接下来，使用`axios`进行 API 调用:

```
 // useEffect automatically executes once the page is fully loaded
  useEffect(() => {
    // set configurations for the API call here
    const configuration = {
      method: "get",
      url: "https://nodejs-mongodb-auth-app.herokuapp.com/free-endpoint",
    };

    // make the API call
    axios(configuration)
      .then((result) => {
        // assign the message in our result to the message we initialized above
        setMessage(result.data.message);
      })
      .catch((error) => {
        error = new Error();
      });
  }, []) 
```

`setMessage(result.data.message);`将结果中的消息(即 result.data.message)分配给上面初始化的消息。现在您可以在组件中显示`message`。

要显示您在`FreeComponent`页面上获得的`message`，请在`<h1 className="text-center">Free Component</h1>`行下面输入以下代码:

```
 <h3 className="text-center text-danger">{message}</h3> 
```

React 将把`message`作为变量读取，因为有花括号。如果`message`没有花括号，React 会将其作为字符串读取。

这是此时的`FreeComponent.js`文件:

```
 import React, { useEffect, useState } from "react";
import axios from "axios";

export default function FreeComponent() {
  // set an initial state for the message we will receive after the API call
  const [message, setMessage] = useState("");

  // useEffect automatically executes once the page is fully loaded
  useEffect(() => {
    // set configurations for the API call here
    const configuration = {
      method: "get",
      url: "https://nodejs-mongodb-auth-app.herokuapp.com/free-endpoint",
    };

    // make the API call
    axios(configuration)
      .then((result) => {
        // assign the message in our result to the message we initialized above
        setMessage(result.data.message);
      })
      .catch((error) => {
        error = new Error();
      });
  }, []);

  return (
    <div>
      <h1 className="text-center">Free Component</h1>

      {/* displaying our message from our API call */}
      <h3 className="text-center text-danger">{message}</h3>
    </div>
  );
} 
```

这是现在的第`FreeComponent`页:

![Alt Text](img/d6f8f9bff4c0c735919ec4ac1172149e.png)

### 如何调用受保护的端点

是时候叫`https://nodejs-mongodb-auth-app.herokuapp.com/auth-endpoint`了。

导航到`AuthComponent.js`文件。

通过调整您的`react`导入行，导入`useEffect`和`useState`，如下所示:

```
 import React, { useEffect, useState,  } from "react"; 
```

导入`axios`:

```
 import axios from "axios"; 
```

导入并初始化通用 cookie:

```
 import Cookies from "universal-cookie";
const cookies = new Cookies(); 
```

获取登录时生成的令牌:

```
 const token = cookies.get("TOKEN"); 
```

设置`message`的初始状态:

```
 const [message, setMessage] = useState(""); 
```

在`return`语句的正上方，声明`useEffect`函数:

```
 useEffect(() => {

  }, []) 
```

在功能中，设置以下配置:

```
 useEffect(() => {
    // set configurations for the API call here
    const configuration = {
      method: "get",
      url: "https://nodejs-mongodb-auth-app.herokuapp.com/auth-endpoint",
      headers: {
        Authorization: `Bearer ${token}`,
      },
    };
  }, []) 
```

注意，这个配置包含一个`header`。这是与`free-endpoint`配置的主要区别。

这是因为`auth-endpoint`是一个受保护的端点，只能使用`Authorization token`访问。因此，您在头中指定了`Authorization token`。如果没有这个头，API 调用将返回一个`403:Forbidden`错误。

进行 API 调用:

```
 // useEffect automatically executes once the page is fully loaded
  useEffect(() => {
    // set configurations for the API call here
    const configuration = {
      method: "get",
      url: "https://nodejs-mongodb-auth-app.herokuapp.com/auth-endpoint",
      headers: {
        Authorization: `Bearer ${token}`,
      },
    };

    // make the API call
    axios(configuration)
      .then((result) => {
        // assign the message in our result to the message we initialized above
        setMessage(result.data.message);
      })
      .catch((error) => {
        error = new Error();
      });
  }, []); 
```

要显示您在`AuthComponent`页面上获得的`message`，请在`<h1 className="text-center">Auth Component</h1>`行下面输入以下代码:

```
 <h3 className="text-center text-danger">{message}</h3> 
```

这是现在的第`AuthComponent`页:

![Alt Text](img/d98d0d1f8bbc910b5f3c5b5d52709075.png)

## 如何构建注销功能

如果您出于任何原因与他人共享设备，他们仍有可能在您登录后访问`authComponent`页面。

为了确保这种情况不会发生，您需要在每次完成后销毁您的授权令牌。

为此，在`authComponent`页面上添加一个按钮。

导入`Button`组件:

```
 import { Button } from "react-bootstrap"; 
```

在文本下方添加以下代码:

```
 <Button type="submit" variant="danger">Logout</Button> 
```

当点击该按钮时，将触发注销功能。所以把`onClick={() => logout()}`加到按钮选项里。该按钮现在看起来像这样:

```
 {/* logout */}
<Button type="submit" variant="danger" onClick={() => logout()}>
   Logout
</Button> 
```

创建函数。在返回上方输入以下代码:

```
 // logout
  const logout = () => {

  } 
```

将以下代码添加到 logout 函数中，以删除或销毁登录期间生成的令牌:

```
 // logout
  const logout = () => {
    // destroy the cookie
    cookies.remove("TOKEN", { path: "/" });
  } 
```

使用以下代码将用户重定向到登录页面:

```
 // logout
  const logout = () => {
    // destroy the cookie
    cookies.remove("TOKEN", { path: "/" });
    // redirect user to the landing page
    window.location.href = "/";
  } 
```

将`className="text-center"`添加到`AuthComponent`的父`div`中，只是为了将整个页面居中。您现在可以从其他地方删除它。`AuthComponent.js`文件现在有以下内容:

```
 import React, { useEffect, useState } from "react";
import { Button } from "react-bootstrap";
import axios from "axios";
import Cookies from "universal-cookie";
const cookies = new Cookies();

// get token generated on login
const token = cookies.get("TOKEN");

export default function AuthComponent() {
  // set an initial state for the message we will receive after the API call
  const [message, setMessage] = useState("");

  // useEffect automatically executes once the page is fully loaded
  useEffect(() => {
    // set configurations for the API call here
    const configuration = {
      method: "get",
      url: "https://nodejs-mongodb-auth-app.herokuapp.com/auth-endpoint",
      headers: {
        Authorization: `Bearer ${token}`,
      },
    };

    // make the API call
    axios(configuration)
      .then((result) => {
        // assign the message in our result to the message we initialized above
        setMessage(result.data.message);
      })
      .catch((error) => {
        error = new Error();
      });
  }, []);

  // logout
  const logout = () => {
    // destroy the cookie
    cookies.remove("TOKEN", { path: "/" });
    // redirect user to the landing page
    window.location.href = "/";
  }

  return (
    <div className="text-center">
      <h1>Auth Component</h1>

      {/* displaying our message from our API call */}
      <h3 className="text-danger">{message}</h3>

      {/* logout */}
      <Button type="submit" variant="danger" onClick={() => logout()}>
        Logout
      </Button>
    </div>
  );
} 
```

您可以看到下面的工作应用程序:

![Alt Text](img/7a575292d7fc709a5de562c39aefa335.png)

这就是 React 认证！

恭喜你！您现在是 React 认证专家:)

## 如何托管前端

React 应用程序将托管在 Netlify 上。这将只需要你几个步骤来设置，所以跟着做吧:

导航到[https://app.netlify.com/signup](https://app.netlify.com/signup)并注册。

按照这个过程，直到您到达您的仪表板。

向下滚动一点，您将看到这个屏幕:

![Alt Text](img/9a9dd23eb777cf7a994f870b40452f87.png)

你可以把你的项目文件夹拖到盒子里，你的托管就完成了！或者你可以把它连接到你的远程回购。

连接到远程存储库的优势在于持续部署。如果您将来有理由对应用程序进行更改，您将不必再次执行这些步骤。

所以点击`New Site from Git`按钮。

选择您想要的 Git 平台，并授权将其同步到您的 Netlify 应用程序。

选取您想要同步的回购。

![Alt Text](img/6a24c6fe432b5e5b5bdee05dead63196.png)

点击您被重定向到的页面上的`Deploy Site`按钮。

等待您的站点发布。这应该不到两分钟。之后，你现在可以点击你看到的链接来访问你的网站。

请注意页面顶部的网站 URL。这是 Netlify 给你的一个随机网址。

![Alt Text](img/d56d226fa79b27ccb6c2c02dedee0012.png)

您可以点击`Site Settings`按钮进行更改。

在`Site details`部分，点击`change site name`按钮。

![Alt Text](img/e4ca2a9555effd27f87a06dcdf4b8cd4.png)

更改名称并点击`Save`。

![Alt Text](img/1edbee5a33e88475e58ddd3baf30ab32.png)

请注意，站点名称已经更改。见下面我的:

![Alt Text](img/3126187d7175bc486108262dbc6995b2.png)

你可能会面临托管后重定向到另一个页面的问题。该错误可能如下图所示:

![Alt Text](img/477691fc3f8bd41f30aa46d9b9bde3ed.png)

### 如何修复“找不到页面”错误

进入 React 项目的公共文件夹。

创建一个文件，命名为`_redirects`，输入以下内容:

```
 /*  /index.html 200 
```

保存并推回到托管应用程序的 Git 平台。

等待一段时间，应用程序将自动发布，你应该都很好。

![Alt Text](img/4d26debf83862b52ac3e7fb7954ea41e.png)

*The error is gone*

恭喜你！你现在是全栈工程师...:)

## 让我们回顾一下

本节通过构建用户界面帮助您将用户连接到后端。你能够了解`React-Bootstrap`、`axios`、`React Hooks`和`react-router-dom`。

您从对`React-Bootstrap`的简要介绍开始，在这一部分，您构建了注册和登录表单。

接下来，您将表单连接到它们各自的端点，并保护一些路由免受未授权用户的访问。最后，您使用 Netlify 来托管应用程序。

这部分的所有代码可以在这里找到[。这里是网上直播的](https://github.com/EBEREGIT/react-auth)

## 所有资源和预览

#### 后端

*   [node . js 代码在这里](https://github.com/EBEREGIT/auth-backend)
*   [后端在这里运行](https://nodejs-mongodb-auth-app.herokuapp.com/)

#### 前端

*   [react . js 代码在这里](https://github.com/EBEREGIT/react-auth)
*   [前端在这里直播](https://react-auth-app.netlify.app/)

## 结论

这是一个很长的教程，但我希望它在每一点上都充满了有用的宝石。我希望你能像我准备它一样喜欢经历它。我真诚地希望降低进入科技行业的门槛。

本教程的主要目的是教任何人如何在后端和前端创建认证。但除此之外，本教程旨在帮助希望过渡到全栈开发人员的初学者和高级开发人员。

添加到每个部分的托管部分教初学者如何将他们的项目公之于众。当您公开您的项目以供预览时，招聘人员可以很容易地了解您可以做什么。这让你在招聘时有更好的机会。

在这一点上，如果你继续建设，你肯定会继续获胜。没有什么能阻止你了。