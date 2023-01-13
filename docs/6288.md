# 如何在你的 Express.js 应用程序中使输入验证简单明了

> 原文：<https://www.freecodecamp.org/news/how-to-make-input-validation-simple-and-clean-in-your-express-js-app-ea9b5ff5a8a7/>

> 本教程需要使用 [expressjs](http://expressjs.com) 框架的先验知识

#### 为什么我们需要服务器端验证？

*   您的客户端验证是不够的，它可能会被破坏
*   更容易受到[中间人攻击](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)，并且服务器永远不应该信任客户端
*   用户可以关闭客户端 JavaScript 验证并操作数据

如果您一直在使用 Express framework 或任何其他 Node.js 框架构建 web 应用程序，验证在任何要求您验证请求`body` `param` `query`的 web 应用程序中都扮演着至关重要的角色。

如果出现以下情况，编写自己的中间件函数可能会很麻烦

*   您希望在保持代码质量的同时快速前进，或者
*   您希望避免在定义业务逻辑的主控制器函数中使用`**if** (**req**.body.head)`或`**if** (**req**.params.isCool)`

在本教程中，您将学习如何在 Express.js 应用程序中使用一个叫做 [express-validator](https://github.com/ctavan/express-validator) 的开源流行模块来验证输入。

### 快速验证器简介

Github 上的定义说:

> express-validator 是一组包装了 [validator.js](https://github.com/chriso/validator.js) 验证器和杀毒器功能的 [express.js](http://expressjs.com/) 中间件。

该模块实现了五个重要的 API:

*   检查 API
*   过滤器 API
*   消毒链 API
*   验证链 API
*   验证结果 API

我们来看一个基本用户`route`没有任何验证模块创建一个用户:`/route/user.js`

```
/**
* @api {post} /api/user Create user
* @apiName Create new user
* @apiPermission admin
* @apiGroup User
*
* @apiParam  {String} [userName] username
* @apiParam  {String} [email] Email
* @apiParam  {String} [phone] Phone number
* @apiParam  {String} [status] Status
*
* @apiSuccess (200) {Object} mixed `User` object
*/

router.post('/', userController.createUser)
```

现在在用户控制器`/controllers/user.js`

```
const User = require('./models/user')

exports.createUser = (req, res, next) => {
  /** Here you need to validate user input. 
   Let's say only Name and email are required field
 */

  const { userName, email, phone, status } = req.body
  if (userName && email &&  isValidEmail(email)) { 

    // isValidEmail is some custom email function to validate email which you might need write on your own or use npm module
    User.create({
      userName,
      email,
      phone,
      status,   
    })
    .then(user => res.json(user))
    .catch(next)
  }
}
```

上面的代码只是一个自己验证字段的基本例子。

您可以使用 Mongoose 在您的用户模型中处理一些验证。对于最佳实践，我们希望确保验证发生在业务逻辑之前。

express-validator 将负责所有这些验证以及输入的[净化](https://www.quora.com/What-does-it-mean-to-sanitize-a-field-How-is-that-related-to-escaping-as-in-entering-in-malicious-input-that-escapes-or-something)。

#### **安装**

```
npm install --save express-validator
```

在您的主`server.js`文件中包含**模块**:

```
const express = require('express')
const bodyParser = require('body-parser')
const expressValidator = require('express-validator')
const app = express()
const router = express.Router()

app.use(bodyParser.json())

app.use(expressValidator())

app.use('/api', router)
```

现在使用 [express-validator](https://github.com/ctavan/express-validator) ，你的`/routes/user.js`将会是这样的:

```
router.post(
  '/', 
  userController.validate('createUser'), 
  userController.createUser,
)
```

这里的`userController.validate`是一个中间件功能，将在下面解释。它接受将用于验证的`method`名称。

让我们在我们的`/controllers/user.js`中创建一个中间件函数`validate()`:

```
const { body } = require('express-validator/check')

exports.validate = (method) => {
  switch (method) {
    case 'createUser': {
     return [ 
        body('userName', 'userName doesn't exists').exists(),
        body('email', 'Invalid email').exists().isEmail(),
        body('phone').optional().isInt(),
        body('status').optional().isIn(['enabled', 'disabled'])
       ]   
    }
  }
}
```

请参考[这篇文章](https://express-validator.github.io/docs/check-api.html)了解更多关于函数定义及其用法。

`body`函数将只验证`req.body`并接受两个参数。首先是`property name`。第二个是您的自定义`message`，如果验证失败，将会显示出来。如果您不提供自定义消息，那么将使用默认消息。

如你所见，对于一个`required`字段，我们使用的是`.exists()`方法。我们使用`.optional()`作为`optional`字段。类似地`isEmail()`T5 用于验证`email`和`integer`。

如果您希望输入字段只包含某些值，那么您可以使用`.isIn([])`。这需要一个`array`值，如果您收到的不是上面的值，那么将会抛出一个错误。

例如，上述代码片段中的状态字段只能有一个`enabled`或`disabled`值。如果您提供除此之外的任何值，将会引发错误。

在`/controllers/user.js`中，让我们编写一个`**createUser**`函数，您可以在其中编写业务逻辑。随着验证的结果，它将在`**validate()**` 之后被调用。

```
const { validationResult } = require('express-validator/check');

exports.createUser = async (req, res, next) => {
   try {
      const errors = validationResult(req); // Finds the validation errors in this request and wraps them in an object with handy functions

      if (!errors.isEmpty()) {
        res.status(422).json({ errors: errors.array() });
        return;
      }

      const { userName, email, phone, status } = req.body

      const user = await User.create({

        userName,

        email,

        phone,

        status,   
      })

      res.json(user)
   } catch(err) {
     return next(err)
   }
}
```

#### 如果你想知道什么是验证结果(req)？

****这个函数**** ****找到这个请求中的验证错误，并用方便的函数**** 将它们包装在一个对象中

现在，每当请求包含无效的主体参数或`req.body`中的`userName`字段丢失时，您的服务器将做出如下响应:

```
{
  "errors": [{
    "location": "body",
    "msg": "userName is required",
    "param": "userName"
  }]
}
```

因此，如果`userName`或`email`未能满足验证，那么默认情况下，`.array()`方法返回的每个错误具有以下格式:

```
{   
  "msg": "The error message",

  "param": "param name", 

  "value": "param value",   
  // Location of the param that generated this error.   
  // It's either body, query, params, cookies or headers.   
  "location": "body",    

  // nestedErrors only exist when using the oneOf function
  "nestedErrors": [{ ... }] 
}
```

正如您所看到的，这个模块真的帮助我们处理了大部分的验证工作。它还维护代码质量，并主要关注业务逻辑。

这是对使用 **express-validator** 模块进行输入验证的介绍，并在本系列的[第 2 部分](https://www.freecodecamp.org/news/how-to-perform-custom-validation-in-your-express-js-app-432eb423510f/)中了解如何验证项目数组并进行自定义验证。

我已经尽了最大努力，希望我涵盖了足够的内容来详细解释它，以便您可以开始。

如果您遇到任何问题，请随时*进入[联系](https://101node.io)或在下面评论。*
我很乐意帮忙:)

关注 Shailesh Shekhawat 以便在我发布新帖子时获得通知。

如果你认为这是一本值得一读的书，请不要犹豫鼓掌！

*最初发布于 2018 年 9 月 2 日 [101node.io](https://101node.io/blog/how-to-validate-inputs-in-express-js-app/) 。*