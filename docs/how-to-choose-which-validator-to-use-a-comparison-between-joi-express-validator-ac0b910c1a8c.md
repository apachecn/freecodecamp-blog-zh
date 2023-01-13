# 如何选择使用哪个验证器:Joi 和 express-validator 的比较

> 原文：<https://www.freecodecamp.org/news/how-to-choose-which-validator-to-use-a-comparison-between-joi-express-validator-ac0b910c1a8c/>

假设你有一个电子商务网站，你允许用户使用他们的名字和电子邮件创建账户。你要确保他们用真实姓名注册，而不是像 cool_dud3 这样的东西。

这就是我们使用验证来验证输入并确保输入数据遵循特定规则的地方。

在市场上，我们已经有了一堆验证库，但是我将比较两个重要的验证库: [Joi](https://github.com/hapijs/joi) 和 [express-validator](https://github.com/express-validator/express-validator) ，用于基于 **express.js 的应用**。

当您决定为构建在 **expressjs** 上的应用程序使用外部输入验证库，但又不确定使用哪一个时，这种比较非常有用。

### 谁是什么？

#### 约伊

Joi 允许你为 JavaScript 对象(一个存储信息的对象)创建*蓝图*或*模式*，以确保关键信息的*验证*。

#### 快速验证器

*express-validator* 是一套 [express.js](http://expressjs.com/) 中间件，包装了 [validator.js](https://github.com/chriso/validator.js) 验证器和杀毒器功能。

所以根据定义，我们可以说:

*   Joi 可用于创建模式(就像我们使用 mongoose 创建 NoSQL 模式一样)，您可以将它用于普通的 Javascript 对象。它就像一个即插即用库，易于使用。
*   另一方面， *express-validator* 使用 [validator.js](https://github.com/chriso/validator.js) 来验证 expressjs 路线，它主要是为 express.js 应用程序构建的。这使得这个库更加适合，并提供了开箱即用的自定义验证和净化。还有，我个人觉得很好理解:)

在 Joi 中有太多的方法和 API 来进行某些验证，可能会让您感到不知所措，所以您可能会最终关闭标签页。

但是我可能错了——所以让我们把观点放在一边，比较两个库。

### 实例化

#### 约伊

在Joi *，*中，你需要使用`**Joi.object()**` 来实例化一个 Joi 模式对象。

所有模式都需要`Joi.object()`来处理验证和其他 Joi 特性。

您需要分别读取`req.body`、`req.params`、`req.query`来请求主体、参数和查询。

```
const Joi = require('joi');

const schema = Joi.object().keys({
   // validate fields here
})
```

#### 快速验证器

你可以只要求 *express-validator* 和开始使用它的方法。您不需要分别从`req.body`、`req.params`和`req.query`中读取值。

您只需要使用下面的`param, query, body`方法来分别验证输入，正如您在这里看到的:

```
const {
  param, query, cookies, header 
  body, validationResult } = require('express-validator/check')

app.post('/user', [   

// validate fields here

], (req, res) => {
const errors = validationResult(req);

  if (!errors.isEmpty()) {     
    return res.status(422).json({ errors: errors.array() });   
  }
}
```

#### 字段是必填的

让我们举一个非常基本的例子，我们希望确保一个`username`应该是必需的`string`，并且是带有`min`和`max`字符的`alphaNumeric`。

*   **Joi:**

```
const Joi = require('joi');
const schema = Joi.object().keys({
    username: Joi.string().alphanum().min(3).max(30).required()
})

app.post('/user', (req, res, next) => {   
  const result = Joi.validate(req.body, schema)
  if (result.error) {
    return res.status(400).json({ error: result.error });
  }
});
```

*   **快速验证器**

```
const { body, validationResult } = require('express-validator/check')

app.post('/user', [   
 body('username')
  .isString()
  .isAlphanumeric()
  .isLength({min: 3, max: 30})
  .exists(), 
], (req, res) => {
  const errors = validationResult(req);

  if (!errors.isEmpty()) {     
    return res.status(422).json({ errors: errors.array() });   
  }
}
```

### 卫生处理

净化基本上是检查输入以确保它没有噪音，例如，我们都在字符串上使用了`.trim()`来删除空格。

或者，如果您遇到一个数字以`"1"`的形式传入的情况，那么在这种情况下，我们希望在运行时清理并转换该类型。

遗憾的是，Joi 不提供开箱即用的净化，但 *express-validator* 提供。

#### 示例:转换为 MongoDB 的 ObjectID

```
const { sanitizeParam } = require('express-validator/filter');  

app.post('/object/:id',  
   sanitizeParam('id')
  .customSanitizer(value => {
     return ObjectId(value); 
}), (req, res) => {   // Handle the request });
```

### 自定义验证

#### Joi: **。延伸(** `extension` **)**

这将创建一个新的 Joi 实例，使用您提供的扩展进行定制。

该扩展利用了一些需要首先描述的公共结构:

*   `value`-Joi 正在处理的值。
*   `state` -包含当前验证上下文的对象。
*   `key` -当前值的键。
*   `path` -当前值的完整路径。
*   `parent` -当前值的潜在父代。
*   `options` -通过`[any().options()](https://github.com/hapijs/joi/blob/master/API.md#anyoptionsoptions)`或`[Joi.validate()](https://github.com/hapijs/joi/blob/master/API.md#validatevalue-schema-options-callback)`提供的选项对象。

#### 延长

`extension`可以是:

*   单个扩展对象
*   生成扩展对象的工厂函数
*   或者一系列这样的东西

扩展对象使用以下参数:

*   `name` -您正在定义的新类型的名称，这可以是现有的类型。必需的。
*   您的类型所基于的现有 Joi 模式。默认为`Joi.any()`。
*   `coerce` -可选函数，在基类之前运行，通常在你想强制使用不同类型的值时使用。它需要 3 个参数`value`、`state`和`options`。
*   `pre` -在验证链中首先运行的可选函数，通常在需要转换值时使用。它需要 3 个参数`value`、`state`和`options`。
*   `language` -添加错误定义的可选对象。每个键都将以类型名为前缀。
*   `describe` -一个可选函数，采用完整形式的描述对其进行后处理。
*   `rules` -要添加的可选规则数组。
*   `name` -新规则的名称。必需的。
*   `params` -可选对象，包含每个参数的 Joi 模式。也可以传递一个 Joi 模式，只要它是一个`Joi.object()`。当然，一些方法，比如`pattern`或者`rename`，在这个给定的环境中是没有用的或者根本不起作用的。
*   `setup` -一个可选的函数，它接受一个带有所提供参数的对象，以允许在设置规则时对模式进行内部操作。您可以选择返回一个新的 Joi 模式，它将作为新的模式实例。必须至少提供`setup`或`validate`中的一个。
*   `validate` -验证值的可选函数，采用 4 个参数`params`、`value`、`state`和`options`。必须提供`setup`或`validate`中的至少一个。
*   `description` -可选字符串或函数，将参数作为参数来描述规则正在做什么。

**举例**:

```
joi.extend((joi) => ({
    base: joi.object().keys({
        name: joi.string(),
        age: joi.number(),
        adult: joi.bool().optional(),
    }),
    name: 'person',
    language: {
        adult: 'needs to be an adult',
    },
rules: [
        {
            name: 'adult',
            validate(params, value, state, options) {

                if (!value.adult) {
                    // Generate an error, state and options need to be passed
                    return this.createError('person.adult', {}, state, options);
                }

                return value; // Everything is OK
            }
        }
    ]
})
```

#### 快速验证器

可以通过使用链方法`[.custom()](https://express-validator.github.io/docs/validation-chain-api.html#customvalidator)`来实现自定义验证器。它采用一个验证器函数。

定制验证器可能会返回承诺来指示异步验证(这将被等待)，或者`throw`任何值/拒绝承诺给[使用定制错误消息](https://express-validator.github.io/docs/custom-error-messages.html#custom-validator-level)。

```
const {
  param, query, cookies, header 
  body, validationResult } = require('express-validator/check')

app.get('/user/:userId', [   
 param('userId')
  .exists()
  .isMongoId()
  .custom(val => UserSchema.isValidUser(val)), 
], (req, res) => {

const errors = validationResult(req);

  if (!errors.isEmpty()) {     
    return res.status(422).json({ errors: errors.array() });   
  }
}
```

### 条件验证

到目前为止，express-validator 还不支持条件验证，但是已经有了一个 PR，你可以查看[https://github . com/express-validator/express-validator/pull/658](https://github.com/express-validator/express-validator/pull/658)

让我们看看它在 Joi 中是如何工作的:

#### `any.when(condition, options)`

`**any:**` 生成匹配任何数据类型的模式对象。

```
const schema = Joi.object({
    a: Joi.any().valid('x'),
    b: Joi.any()
}).when(
    Joi.object({ b: Joi.exist() })
    .unknown(), {
    then: Joi.object({
        a: Joi.valid('y')
    }),
    otherwise: Joi.object({
        a: Joi.valid('z')
    })
});
```

#### `alternatives.when(condition, options)`

基于另一个键(与`any.when()`不同)值或查看当前值的模式，添加条件替代模式类型，其中:

*   `condition` -键名或[引用](https://github.com/hapijs/joi/blob/master/API.md#refkey-options)，或一个模式。
*   `options` -具有以下特征的物体:
*   `is` -所需条件 joi 类型。当`condition`是一个模式时被禁止。
*   `then` -条件为真时尝试的替代模式类型。如果缺少`otherwise`则需要。
*   `otherwise` -条件为假时尝试的替代模式类型。如果缺少`then`则需要。

```
const schema = Joi
     .alternatives()
     .when(Joi.object({ b: 5 }).unknown(), {
        then: Joi.object({
           a: Joi.string(),
           b: Joi.any()
      }),
      otherwise: Joi.object({
        a: Joi.number(),
        b: Joi.any()
      })
});
```

### 嵌套验证

当您想要验证对象/项目的数组或只是对象键时

两个库都支持嵌套验证

那么 express-validator 呢？

#### 通配符

通配符允许您遍历项目或对象键的数组，并验证每个项目或其属性。

`*`字符也称为通配符。

```
const express = require('express'); 
const { check } = require('express-validator/check'); 
const { sanitize } = require('express-validator/filter');  
const app = express(); 

app.use(express.json());  
app.post('/addresses', [   
    check('addresses.*.postalCode').isPostalCode(),
    sanitize('addresses.*.number').toInt() 
], 
(req, res) => {   // Handle the request });
```

**Joi**

```
const schema = Joi.object().keys({
    addresses: Joi.array().items(
        Joi.object().keys({
            postalCode: Joi.string().required(),
        }),
    )
});
```

### 自定义错误消息

#### 约伊

#### `any.error(err, [options])`

用自定义错误覆盖默认 joi 错误

```
let schema = Joi.string().error(new Error('Was REALLY expecting a string'));
```

#### 快速验证器

```
const { check } = require('express-validator/check'); 

app.post('/user', [   
   // ...some other validations...   
   check('password')     
   .isLength({ min: 5 }).withMessage('must be at 5 chars long')
   .matches(/\d/).withMessage('must contain a number') 
], 
(req, res) => {   // Handle the request somehow });
```

### 结论

我介绍了两个库最重要的部分，你可以自己决定使用哪个。如果我在比较中遗漏了什么重要的东西，请在下面的评论中告诉我。

我希望在为您的 express.js 应用程序决定下一个输入验证模块时，它对您有所帮助。

我在这里写了一篇关于它的深入文章:[如何验证输入](https://medium.freecodecamp.org/how-to-make-input-validation-simple-and-clean-in-your-express-js-app-ea9b5ff5a8a7)。一定要去看看。

如果你认为这是一本值得一读的书，请不要犹豫鼓掌！

*原载于 2019 年 3 月 31 日 [101node.io](https://101node.io/blog/javascript-validators-comparison-using-joi-vs-express-validator/) 。*