# 如何在 Express.js 应用程序中执行自定义验证(第 2 部分)

> 原文：<https://www.freecodecamp.org/news/how-to-perform-custom-validation-in-your-express-js-app-432eb423510f/>

在[之前的文章](https://medium.freecodecamp.org/how-to-make-input-validation-simple-and-clean-in-your-express-js-app-ea9b5ff5a8a7)中，我展示了如何在 express.js 应用程序中开始输入验证。我使用了 [express-validator](https://github.com/ctavan/express-validator) 模块，并讨论了它的重要特性和实现。

如果你还没看过，请点击这里阅读第一篇文章[。](https://medium.freecodecamp.org/how-to-make-input-validation-simple-and-clean-in-your-express-js-app-ea9b5ff5a8a7)

所以现在让我们开始吧。在本教程的第 2 部分中，您将学习如何在 Express.js 应用程序中执行自定义验证。

### 使用自定义验证可以实现什么

*   它可用于验证数据库中实体的存在。
*   也可以测试某个值是否存在于数组、对象、字符串等中。
*   如果你想改变数据格式本身。

还有更多…

[express-validator](https://express-validator.github.io/docs/) 库提供了一个`custom`方法，您可以用它来进行各种定制验证

自定义验证器的实现使用了 chain 方法[。自定义()](https://express-validator.github.io/docs/validation-chain-api.html#customvalidator)。它采用一个验证器函数。

自定义验证器返回承诺显示异步验证或`throw`任何值/拒绝承诺给[使用自定义错误消息](https://express-validator.github.io/docs/custom-error-messages.html#custom-validator-level)。

现在，我将向您展示上述自定义验证用例的示例。

### 检查数据库中是否存在该实体

这是我日常使用的一个重要工具——我猜你会用它来对照数据库验证一个实体

例如，如果有人请求更新他们的名字，你可以用它来完成一个基本的`PUT`请求`/api/users/:userId`。

为了确保用户应该存在于我们的数据库中，我创建了一个函数来检查数据库。

```
param('userId')
.exists()
.isMongoId()
.custom(val => UserSchema.isValidUser(val))
```

`isValidUser()`是一个静态函数，它将对数据库进行异步调用，并判断用户是否存在。

让我们用 mongose`Schema`写一个静态函数:

```
UserSchema.statics = {
   isValid(id) {
      return this.findById(id)
             .then(result => {
                if (!result) throw new Error('User not found')
      })
   },
}
```

由于我们不能只根据客户端发送的`userId`的格式来信任它，我们需要确保它是一个真实的帐户。

### **验证数组或对象中的某些值**

例如，如果你想对用户名应用一个规则，它必须有一个字符`@`。

所以在你的`POST`请求用户创建或者更新的时候，你可以这样做:

```
body('username', 'Invalid Username')
.exists()
.isString().isLowercase()
.custom(val => {   

   if (val.indexOf('@') !== -1) return true

   return false
}),
```

> *记住:永远从`.custom()`函数的回调中返回一个布尔值。否则，您的验证可能无法按预期进行。*

如您所见，我们可以在中间件本身中进行所有这些验证，包括异步验证，而不是在控制器中进行验证

[https://giphy.com/embed/TkERwbWzAxvfa](https://giphy.com/embed/TkERwbWzAxvfa)

### 更改输入数据格式

该库有一个[清理](https://express-validator.github.io/docs/sanitization.html)功能，使用`customerSanitizer()`执行自定义清理。

我用它将逗号分隔值的字符串改为字符串数组。

例如，我们有一个医生数据库。有人想只招心脏病专家和 T2 精神病专家。

我们已经将这两个专门化作为`**type**` 存储在我们的数据库中。

一个简单的`GET`请求如下所示:

```
GET /api/doctors?type=cardiologists,psychiatrist
```

现在在`mongodb`中，我们可以使用`**$in**` 操作符来搜索属性的多个值。

一个基本的数据库查询可以是:

```
Doctors.find({
   type: {

     $in: ['cardiologists', 'psychiatrist']

   }
})
```

这会给你所有的心脏病专家和精神病专家。

从`GET`查询:

```
req.query = {

  type: "cardiologists,psychiatrist"

}
```

正如你在`req.query`中看到的，你将得到一个属性`type`，它的类型是一个`string`。

在`.customSanitizer()`的帮助下，我们能够将一个字符串转换成一个字符串数组。

在验证级别:

```
const commaToArray  = (value = '') => value.split(',')

sanitizeQuery('type').customSanitizer(commaToArray),
```

现在我们可以直接将它输入到数据库中，查询到`**$in**`操作符。

如果我想对数组中的所有项或对象中的键应用一些规则，该怎么办？

[https://giphy.com/embed/a5viI92PAF89q](https://giphy.com/embed/a5viI92PAF89q)

[通过 GIPHY](https://giphy.com/gifs/reaction-a5viI92PAF89q)

#### 正文:

```
{
  items:[
    {_id: 'someObjectId', number: '200'},
    ...
  ]
}
```

### 通配符

通配符是本模块的一大特色。它允许您遍历一组项或对象键，并验证每个项或其属性。

`*`字符也称为通配符。

假设我想验证所有项目的`_id, number`。

```
check('items.*._id')
.exists()
.isMongoId()
.custom(val => ItemSchema.isValid(val)), //similar to isValidUser() 
sanitize('items.*.number').toInt()
```

这就是使用 express-validator 模块进行输入验证的介绍

如果您遇到任何问题，请随时*进入[联系](https://101node.io)或在下面评论。*
我很乐意帮忙:)

如果你认为这是一本值得一读的书，请不要犹豫鼓掌！

关注 Shailesh Shekhawat 以便在我发布新帖子时获得通知。

*最初发布于 2018 年 9 月 22 日 [101node.io](https://101node.io/blog/how-to-make-input-validation-in-express-js-app-part-2/) 。*