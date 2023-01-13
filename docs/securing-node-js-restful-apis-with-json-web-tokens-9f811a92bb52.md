# 使用 JSON Web 令牌保护 Node.js RESTful APIs

> 原文：<https://www.freecodecamp.org/news/securing-node-js-restful-apis-with-json-web-tokens-9f811a92bb52/>

你有没有想过认证是如何工作的？所有复杂和抽象背后的东西。其实没什么特别的。这是一种加密值的方式，进而创建一个用户用作标识符的唯一令牌。这个令牌验证你的身份。它可以验证您的身份，并授权您访问各种资源。如果你碰巧不知道这些关键词中的任何一个，请耐心等待，我会在下面解释一切。

这将是如何将基于令牌的认证添加到现有 REST API 的一步一步的教程。正在讨论的认证策略是 JWT (JSON Web Token)。如果这不能告诉你太多，没关系。当我第一次听到这个术语时，我也同样感到奇怪。

从现实的角度来看，JWT 到底意味着什么？让我们来分解一下官方的定义:

> JSON Web Token (JWT)是一种紧凑的、URL 安全的方式，用于表示在双方之间传输的声明。JWT 中的声明被编码为 JSON 对象，该对象被用作 JSON Web 签名(JWS)结构的有效载荷，或者被用作 JSON Web 加密(JWE)结构的明文，从而能够对声明进行数字签名或使用消息认证码(MAC)进行完整性保护和/或加密。
> - [**【互联网工程任务组】**](https://tools.ietf.org/html/rfc7519)

那是一口。让我们把它翻译成英语。JWT 是一个编码字符串，如果两台计算机都有 HTTPS，它可以在两台计算机之间安全地发送。令牌代表一个值，该值只能由能够访问加密密钥的计算机访问。很简单，对吧？

这在现实生活中是什么样子的？假设一个用户想要登录他们的帐户。他们向服务器发送带有所需凭据(如电子邮件和密码)的请求。服务器检查凭证是否有效。如果是，服务器将使用所需的有效负载和密钥创建一个令牌。加密产生的字符串称为令牌。然后服务器将它发送回客户端。反过来，客户端保存令牌，以便在用户发送的其他请求中使用它。向请求头添加令牌的做法是授权用户访问资源的一种方式。这是 JWT 如何运作的一个实际例子。

好了，说够了！本教程的其余部分将是编码，随着我们的进展，我希望你能跟着我一起编码。每段代码后面都会有解释。我相信正确理解它的最好方法是自己编写代码。

在开始之前，您需要了解一些关于 Node.js 和我将使用的一些 EcmaScript 标准的信息。我不会使用 ES6，因为它不像传统 JavaScript 那样对初学者友好。但是，我希望你已经知道如何用 Node.js 构建一个 RESTful API。如果没有，你可以绕道而行，在继续之前[查看一下](https://hackernoon.com/restful-api-design-with-node-js-26ccf66eab09)。

另外，如果你想看完整版的话，GitHub 上有整个演示。

### 让我们开始写一些代码，好吗？

实际上还没有。我们需要首先设置环境。代码至少还要等几分钟。这部分很无聊，所以为了快速上手，我们将从上面的教程中克隆这个库。打开终端窗口或命令行提示符，运行以下命令:

```
git clone https://github.com/adnanrahic/nodejs-restful-api.git
```

你会看到一个文件夹出现，打开它。我们来看看文件夹结构。

```
> user
  - User.js
  - UserController.js
- db.js
- server.js
- app.js
- package.json
```

我们有一个用户文件夹，里面有一个模型和一个控制器，基本的 CRUD 已经实现了。我们的 **app.js** 包含了基本配置。 **db.js** 确保应用程序连接到数据库。 **server.js** 确保我们的服务器加速运转。

继续安装所有需要的节点模块。切换回您的终端窗口。确保你在名为*'***nodejs-restful-API***'*的文件夹中，运行`npm install`。等待一两秒钟，以便安装模块。现在您需要在 **db.js** 中添加一个数据库连接字符串。

跳转到 [mLab](https://mlab.com/) ，创建一个帐户(如果你还没有)，然后打开你的数据库仪表板。创建一个新的数据库，根据需要命名，然后进入其配置页面。将数据库用户添加到数据库中，并将连接字符串从仪表板复制到您的代码中。

![Z5HFF8CQYR9cwiRY8f08tlWbz8PQARSVDHGL](img/28b3dc87e29eb2c146937fbb774ea499.png)

现在您需要做的就是更改`<dbuser>`和`<dbpassword>`的占位符值。用您为数据库创建的用户的用户名和密码替换它们。这个过程的详细的一步一步的解释可以在上面链接的[教程中找到。](https://hackernoon.com/restful-api-design-with-node-js-26ccf66eab09)

假设我为数据库创建的用户名为`wally`，密码为`theflashisawesome`。记住这一点， **db.js** 文件现在应该看起来像这样:

```
var mongoose = require('mongoose');
mongoose.connect('mongodb://wally:theflashisawesome@ds147072.mlab.com:47072/securing-rest-apis-with-jwt', { useMongoClient: true });
```

继续启动服务器，回到您的终端窗口键入`node server.js`。您应该看到`Express server listening on port 3000`登录到终端。

### 最后，一些代码。

让我们从头脑风暴开始，看看我们想要做什么。首先，我们要添加用户认证。也就是说，实现一个用户注册和登录的系统。

其次，我们要添加授权。授予用户访问 REST API 上某些资源的权限的行为。

首先，在项目的根目录中添加一个新文件。给它起个名字 **config.js** *。*您可以在这里输入应用程序的配置设置。目前我们需要的只是为我们的 JSON Web 令牌定义一个密钥。

免责声明:记住，在任何情况下，你都不应该。)让你的秘钥像这样公开可见。总是把你所有的键放在环境变量中！我这样写只是为了演示的目的。

```
// config.js
module.exports = {
  'secret': 'supersecret'
};
```

添加了这些之后，您就可以开始添加身份验证逻辑了。创建一个名为 **auth** 的文件夹，然后添加一个名为 **AuthController.js** 的文件。这个控制器将是我们的认证逻辑的家。

将这段代码添加到 **AuthController.js** *的顶部。*

```
// AuthController.js

var express = require('express');
var router = express.Router();
var bodyParser = require('body-parser');
router.use(bodyParser.urlencoded({ extended: false }));
router.use(bodyParser.json());
var User = require('../user/User');
```

现在您已经准备好添加使用 [JSON Web 令牌](https://github.com/auth0/node-jsonwebtoken)和[加密密码](https://github.com/dcodeIO/bcrypt.js)的模块了。将这段代码粘贴到 **AuthController.js** 中:

```
var jwt = require('jsonwebtoken');
var bcrypt = require('bcryptjs');
var config = require('../config');
```

在项目文件夹中打开一个终端窗口，安装以下模块:

```
npm install jsonwebtoken --save
npm install bcryptjs --save
```

这就是我们实现所需身份验证所需的所有模块。现在您已经准备好创建一个`/register`端点。将这段代码添加到您的 **AuthController.js** 中:

```
router.post('/register', function(req, res) {

  var hashedPassword = bcrypt.hashSync(req.body.password, 8);

  User.create({
    name : req.body.name,
    email : req.body.email,
    password : hashedPassword
  },
  function (err, user) {
    if (err) return res.status(500).send("There was a problem registering the user.")
    // create a token
    var token = jwt.sign({ id: user._id }, config.secret, {
      expiresIn: 86400 // expires in 24 hours
    });
    res.status(200).send({ auth: true, token: token });
  }); 
});
```

这里我们期望用户给我们发送三个值，一个名字，一个电子邮件和一个密码。我们将立即获取密码，并使用 Bcrypt 的哈希方法对其进行加密。然后获取散列密码，包括姓名和电子邮件，并创建一个新用户。成功创建用户后，我们就可以轻松地为该用户创建令牌了。

`jwt.sign()`方法将有效载荷和在 **config.js** 中定义的密钥作为参数。它创建一个代表有效载荷的独特字符串。在我们的例子中，有效负载是一个只包含用户 id 的对象。让我们编写一段代码，根据从 register 端点获得的令牌来获取用户 id。

```
router.get('/me', function(req, res) {
  var token = req.headers['x-access-token'];
  if (!token) return res.status(401).send({ auth: false, message: 'No token provided.' });

  jwt.verify(token, config.secret, function(err, decoded) {
    if (err) return res.status(500).send({ auth: false, message: 'Failed to authenticate token.' });

    res.status(200).send(decoded);
  });
});
```

在这里，我们期望令牌在消息头中与请求一起发送。HTTP 请求头中令牌的默认名称是`x-access-token`。如果请求中没有提供令牌，服务器会发回一个错误。更准确地说，一个响应消息为 *'* **的`401 unauthorized`状态，没有提供** *'* 令牌。如果令牌存在，将调用`jwt.verify()`方法。这个方法对令牌进行解码，使得查看原始有效负载成为可能。如果有错误，我们将处理错误，如果没有，将解码值作为响应发送回来。

最后，我们需要将路线添加到主 **app.js** 文件中的 **AuthController.js** 中。首先从 **AuthController.js** 导出路由器:

```
// add this to the bottom of AuthController.js
module.exports = router;
```

然后在主应用程序中添加对控制器的引用，就在您导出应用程序的位置上方。

```
// app.js
var AuthController = require('./auth/AuthController');
app.use('/api/auth', AuthController);
module.exports = app;
```

### 让我们来测试一下。为什么不呢？

打开你选择的 REST API 测试工具，我用的是 [Postman](https://www.getpostman.com/postman) 或者[失眠](https://insomnia.rest/)，但是任何一个都可以。

回到你的终端，运行`node server.js`。如果它正在运行，停止它，保存对您的文件的所有更改，并再次运行`node server.js`。

打开 Postman 并点击注册端点(`/api/auth/register`)。确保选择发布方法和`x-www-form-url-encoded`。现在，添加一些值。我的用户名是 Mike，密码是“thisisasecretpassword”。老实说，这不是我见过的最好的密码，但也可以。点击发送！

![vshJBCITdZGicsO1sw5prUHlSGR9bNugRtjv](img/be6d4885417d5990068372aa9a080d7e.png)

/register

看到回应了吗？该令牌是一个又长又乱的字符串。要试用`/api/auth/me`端点，首先复制令牌。将 URL 改为`/me`而不是`/register`，并修改获取的方法。现在，您可以将令牌添加到请求头中。

![Lh6dOAOqy3A5tpOPy8xUQLKONDUsVmT-uVup](img/c16c631a612924196130d450191413c8.png)

/me

瞧啊。令牌已被解码为一个带有 id 字段的对象。想要确定 id 确实属于我们刚刚创建的用户 Mike 吗？你当然知道。跳回你的代码编辑器。

```
// in AuthController.js change this line
res.status(200).send(decoded);

// to
User.findById(decoded.id, function (err, user) {
  if (err) return res.status(500).send("There was a problem finding the user.");
  if (!user) return res.status(404).send("No user found.");

  res.status(200).send(user);
});
```

现在，当您向`/me`端点发送请求时，您将看到:

![JfGjoyqG9zttRCEPsXTkUiNQ12NbKbRiqzyy](img/8b47591533475bbfcaeaaca1e6c74132.png)

响应现在包含了整个用户对象！酷！但是，不好。密码不应与用户的其他数据一起返回。让我们解决这个问题。我们可以在查询中添加一个投影，并省略密码。像这样:

```
User.findById(decoded.id, 
  { password: 0 }, // projection
  function (err, user) {
    if (err) return res.status(500).send("There was a problem finding the user.");
    if (!user) return res.status(404).send("No user found.");

    res.status(200).send(user);
});
```

![GSTNoNziXzH43m9kEYX499bgB012YenWiHAF](img/cb20132f302c3d2494a97d055fb7a627.png)

这样更好，现在我们可以看到除密码以外的所有值。迈克看起来不错。

### 有人说登录吗？

实现注册后，我们应该为现有用户创建一种登录方式。让我们考虑一下。注册端点要求我们创建一个用户，散列一个密码，并颁发一个令牌。登录端点需要我们实现什么？它应该检查给定电子邮件的用户是否存在。还要检查所提供的密码是否与数据库中的哈希密码相匹配。只有到那时，我们才会想要发行代币。将此添加到您的 **AuthController.js** 中。

```
router.post('/login', function(req, res) {

  User.findOne({ email: req.body.email }, function (err, user) {
    if (err) return res.status(500).send('Error on the server.');
    if (!user) return res.status(404).send('No user found.');

    var passwordIsValid = bcrypt.compareSync(req.body.password, user.password);
    if (!passwordIsValid) return res.status(401).send({ auth: false, token: null });

    var token = jwt.sign({ id: user._id }, config.secret, {
      expiresIn: 86400 // expires in 24 hours
    });

    res.status(200).send({ auth: true, token: token });
  });

});
```

首先，我们检查用户是否存在。然后使用 Bcrypt 的`.compareSync()`方法，我们将随请求发送的密码与数据库中的密码进行比较。如果他们匹配我们`.sign()`的令牌。差不多就是这样。让我们试一试。

![sSJkpDDrhTNNLeYSG-b0MD5gfQBFqNMK6uUx](img/01146e55777aad5d63e4533968f06262.png)

酷，它工作了！如果我们输错了密码怎么办？

![QkXrnEyXUKhakmAGysGEIGVd39S3uLmhncLE](img/00cc192ff18de0d5e7dfeacef4cb0a71.png)

太好了，当密码错误时，服务器会发送一个响应状态`401 unauthorized`。正是我们想要的！

为了结束这部分教程，让我们添加一个简单的注销端点来取消令牌。

```
// AuthController.js
router.get('/logout', function(req, res) {
  res.status(200).send({ auth: false, token: null });
});
```

**免责声明**:不需要注销端点。注销的行为只能通过客户端完成。令牌通常保存在 cookie 或浏览器的本地存储中。注销就像销毁客户机上的令牌一样简单。创建这个`/logout`端点是为了从逻辑上描述当您注销时会发生什么。令牌被设置为`null`。

至此，我们已经完成了教程的**认证**部分。想继续授权吗？我打赌你知道。

### 你有到这里来的许可吗？

为了理解授权策略背后的逻辑，我们需要思考一种叫做**中间件**的东西。它的名字在某种程度上是不言自明的，不是吗？中间件是一段代码，Node.js 中的一个函数，在你的代码的某些部分之间充当桥梁。

当请求到达端点时，路由器可以选择将请求传递给队列中的下一个中间件功能。强调这个词**下一个**！因为这正是函数的名字！让我们看一个例子。注释掉将用户作为响应发送回来的那一行。在正下方添加一个`next(user)`。

```
router.get('/me', function(req, res, next) {

  var token = req.headers['x-access-token'];
  if (!token) return res.status(401).send({ auth: false, message: 'No token provided.' });

  jwt.verify(token, config.secret, function(err, decoded) {
    if (err) return res.status(500).send({ auth: false, message: 'Failed to authenticate token.' });

    User.findById(decoded.id, 
    { password: 0 }, // projection
    function (err, user) {
      if (err) return res.status(500).send("There was a problem finding the user.");
      if (!user) return res.status(404).send("No user found.");

      // res.status(200).send(user); Comment this out!
      next(user); // add this line
    });
  });
});

// add the middleware function
router.use(function (user, req, res, next) {
  res.status(200).send(user);
});
```

> ***中间件*** 函数是在应用的请求-响应周期中访问[请求对象](https://expressjs.com/en/4x/api.html#req)(`req`)[响应对象](https://expressjs.com/en/4x/api.html#res) ( `res`)和`next`函数的函数。`next`功能是快速路由器中的功能，当被调用时，执行当前中间件之后的中间件。
> - [使用中间件](https://expressjs.com/en/guide/using-middleware.html)，expressjs.com

跳回 postman，看看点击`/api/auth/me`端点时会发生什么。结果一模一样，这让你吃惊吗？应该是！

**免责声明**:在我们继续之前，请删除此示例，因为它仅用于演示使用`next()`的逻辑。

让我们采用同样的逻辑，并应用它来创建一个中间件函数来检查令牌的有效性。在 **auth** 文件夹下新建一个文件，命名为 **VerifyToken.js** 。将这段代码粘贴到这里。

```
var jwt = require('jsonwebtoken');
var config = require('../config');

function verifyToken(req, res, next) {
  var token = req.headers['x-access-token'];
  if (!token)
    return res.status(403).send({ auth: false, message: 'No token provided.' });

  jwt.verify(token, config.secret, function(err, decoded) {
    if (err)
    return res.status(500).send({ auth: false, message: 'Failed to authenticate token.' });

    // if everything good, save to request for use in other routes
    req.userId = decoded.id;
    next();
  });
}

module.exports = verifyToken;
```

我们来分解一下。我们将使用这个函数作为定制的中间件来检查令牌是否存在以及它是否有效。验证之后，我们将`decoded.id`值添加到请求(`req`)变量中。我们现在可以在请求-响应周期中的下一个函数中访问它。调用`next()`将确保流程继续到下一个排队等待的函数。最后，我们导出函数。

现在，再次打开 **AuthController.js** 。在文件顶部添加对 **VerifyToken.js** 的引用，并编辑`/me`端点。它现在应该是这样的:

```
// AuthController.js

var VerifyToken = require('./VerifyToken');

// ...

router.get('/me', VerifyToken, function(req, res, next) {

  User.findById(req.userId, { password: 0 }, function (err, user) {
    if (err) return res.status(500).send("There was a problem finding the user.");
    if (!user) return res.status(404).send("No user found.");

    res.status(200).send(user);
  });

});

// ...
```

看到我们如何在函数链中添加`VerifyToken`了吗？我们现在处理中间件中的所有授权。这释放了回调中的所有空间，以便只处理我们需要的逻辑。这是一个如何编写干代码的极好例子。现在，每当您需要授权一个用户时，您可以将这个中间件功能添加到这个链中。再次在 Postman 中测试它，以确保它仍然像它应该的那样工作。

![19EpKwBFR9RiWF4P-lDTJYRIiCYsCynW9bU7](img/1aa11a3b33ef1f58dfdd0bcfb7f1225a.png)

您可以随意修改令牌并再次尝试端点。对于无效的令牌，您将看到所需的错误消息，并确保您编写的代码按照您想要的方式工作。

这个为什么这么厉害？您现在可以将`VerifyToken` 中间件添加到任何功能链中，并确保端点是安全的。只有拥有经过验证的令牌的用户才能访问资源！

### 把你的头缠在所有事情上。

如果你没有一下子掌握所有的东西，不要难过。其中一些概念很难理解。在再次尝试之前，退一步让你的大脑休息一下是很好的。这就是为什么我建议您自己浏览代码，并尽最大努力让它工作。

同样，[这里是 GitHub 库](https://github.com/adnanrahic/securing-restful-apis-with-jwt)。您可以补上您可能遗漏的任何内容，或者在遇到问题时更好地查看代码。

记住，**认证**是让用户登录的行为。**授权**是验证用户与资源交互的访问权限的行为。

中间件功能被用作一些代码之间的桥梁。当在端点的函数链中使用时，它们在授权和错误处理中非常有用。

希望你们喜欢读这篇文章，就像我喜欢写这篇文章一样。直到下次，保持好奇，玩得开心。

你认为这个教程对某人有帮助吗？不要犹豫分享。如果你喜欢，请为我鼓掌。