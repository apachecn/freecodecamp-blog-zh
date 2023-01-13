# 如何在带有 Mongoose 插件的 Express.js 应用程序中记录 Node.js API

> 原文：<https://www.freecodecamp.org/news/how-to-log-a-node-js-api-in-an-express-js-app-with-mongoose-plugins-efe32717b59/>

> 本教程需要预先了解[mongose](https://mongoosejs.com/)对象关系映射(ORM)技术

#### 介绍

随着应用程序的增长，日志记录成为跟踪一切的关键部分。这对于调试尤其重要。

如今，npm 已经有可用的测井模块。这些模块可以将日志以不同的格式或级别存储在一个文件中。我们将使用流行的 ORM Mongoose 来讨论 Node.js Express 应用程序中的 API 日志记录。

那么你如何创建一个**mongose 插件**来以一种更干净的方式为你做日志记录，并使 API 日志记录更容易呢？

#### 猫鼬里的插件是什么？

在 Mongoose 中，模式是可插拔的。插件就像一个函数，可以在模式中使用，并在模式实例中反复使用。

Mongoose 还提供了**全局插件**，可以用于所有模式。例如，我们将编写一个插件，它将创建两个`**jsons**`的一个`**diff**`，并写入`**mongodb**`T5。

### 步骤 1:创建基本日志模式模型

让我们创建一个具有以下六个属性的基本日志模式:

*   **动作:**顾名思义，无论是`create` `update` `delete`还是其他，这都是 API 的一个动作过程。
*   **类别:** API 类别。例如医生和病人。它更像是一门课。
*   **CreatedBy:** 使用或调用 API 的用户。
*   **消息:**在这里你可以包含任何你想显示的在调试过程中有意义或有帮助的消息。
*   **Diff:** 这是将拥有两个*JSON*的 *diff* 的主属性

如果您希望对自己的应用程序有意义，可以添加更多的字段。模式可以根据需要进行更改和升级。

下面是我们的模型:`models/log.js`

```
const mongoose = require('mongoose')
const Schema = mongoose.Schema
const { ObjectId } = Schema

const LogSchema = new Schema({
  action: { type: String, required: true },
  category: { type: String, required: true },
  createdBy: { type: ObjectId, ref: 'Account', required: true },
  message: { type: String, required: true },
  diff: { type: Schema.Types.Mixed },
},{
  timestamps: { createdAt: 'createdAt', updatedAt: 'updatedAt' },
})

LogSchema.index({ action: 1, category: 1 })

module.exports = mongoose.model('Log', LogSchema)
```

### 第二步:写一个函数来得到两个 JSONs 的区别

所以下一步是您需要一个可重用的函数，它将动态地创建两个 JSONs 的`diff`。

姑且称之为`diff.js`

```
const _ = require('lodash')

exports.getDiff = (curr, prev) => {
  function changes(object, base) {
    return _.transform(object, (result, value, key) => {
      if (!_.isEqual(value, base[key]))
        result[key] = (_.isObject(value) && _.isObject(base[key])) ?                 changes(value, base[key]) : value
    })
 }
 return changes(curr, prev)
}
```

我使用了流行的库`[**lodash**](https://lodash.com/docs/4.17.10)`、**、**来提供同样的功能。

让我们分解一下上面的函数，看看是怎么回事:

*   **_。transform:** 它是数组的`.reduce`的替代。基本上，它会迭代你的对象`keys`和`values`。它提供了第一个参数`accumulator`。`result` 是累加器，是可变的。
*   **_。isEqual:** 对两个值进行深度比较，以确定它们是否相等。

> ***isEqual*** *:该方法支持比较数组、数组缓冲区、布尔、日期对象、错误对象、映射、数字、`Object`对象、正则表达式、集合、字符串、符号、类型化数组。`Object`对象通过它们自己的，而不是继承的，可枚举的属性进行比较。函数和 DOM 节点通过严格相等进行比较，即`===`。*

这里，我们迭代每个对象属性和值，并将其与旧的/prev 对象进行比较。

如果当前对象的`value`不等于前一个对象中相同属性的值:`base[key]`并且如果该值是对象本身，我们递归地调用函数`changes`**，直到它得到一个值，该值最终将作为`result[key] = value`存储在`result`中。**

### **步骤 3:创建一个插件来使用 diff 并保存到数据库中**

**现在我们需要跟踪数据库中先前的`document`，并在保存到`mongodb`之前创建一个`diff`。**

```
`const _ = require('lodash')
const LogSchema = require('../models/log')
const { getDiff } = require('../utils/diff')

const plugin = function (schema) {
  schema.post('init', doc => {
    doc._original = doc.toObject({transform: false})
  })
  schema.pre('save', function (next) {
    if (this.isNew) {
      next()
    }else {
      this._diff = getDiff(this, this._original)
      next()
    }
})

  schema.methods.log = function (data)  {
    data.diff = {
      before: this._original,
      after: this._diff,
    }
    return LogSchema.create(data)
  }
}

module.exports = plugin`
```

**在猫鼬，有不同的钩可用。现在，我们需要使用模式上可用的`[init](https://mongoosejs.com/docs/api.html#document_Document-init)`和`[save](https://mongoosejs.com/docs/api.html#document_Document-save)`方法。**

**`this.isNew()`:如果你正在创建新文档，那么只需返回`next()`中间件。**

**在`schema.post('init')` `[toObject()](https://mongoosejs.com/docs/api.html#document_Document-toObject)`中:**

```
`doc._original = doc.toObject({transform: false})`
```

**猫鼬`Model` s 继承`Document` s，后者有一个`toObject()`方法。它会将一个`document`转换成一个`Object()`，`transform:false`表示不允许转换返回的对象。**

### **步骤 4:用法—如何在 express.js API 中使用**

**在您的主`server.js`或`app.js`中:**

**初始化一个全局[插件](https://mongoosejs.com/docs/plugins.html)，这样它将可用于所有模式。通过在模式模型中初始化它，您还可以将它用于特定的模式。**

```
`const mongoose = require('mongoose')

mongoose.plugin(require('./app/utils/diff-plugin'))`
```

**下面是一个`user`更新 API 的基本例子:**

```
`const User = require('../models/user')

exports.updateUser = (req, res, next) => {
  return User.findById(req.params.id)
    .then(user => {
        if (!user)
           throw new Error('Target user does not exist. Failed to update.')
       const { name } = req.body
       if (name) user.name = name
       return user.save()
     })
     .then(result => {
       res.json(result)
       return result
     })
     .catch(next)
     .then(user => {
         if (user && typeof user.log === 'function') { 
            const data = {
              action: 'update-user',
              category: 'users',
              createdBy: req.user.id,
              message: 'Updated user name',
         }
         return user.log(data)
     }
     }).catch(err => {
         console.log('Caught error while logging: ', err)
       })
}`
```

### **结论**

**在本教程中，您学习了如何创建一个 Mongoose 插件，并使用它来记录 API 中的`changes`。您可以使用插件做更多的事情来构建一个健壮的节点应用程序。**

**以下是学习更多关于 Mongoose 和插件使用的资源:**

*   **80/20 猫鼬插件指南:[http://thecodebarbian . com/2015/03/06/Guide-to-mongose-plugins](http://thecodebarbarian.com/2015/03/06/guide-to-mongoose-plugins)**
*   **[https://mongoosejs.com/docs/plugins.html](https://mongoosejs.com/docs/plugins.html)**

**我希望这个教程对你有用，如果你有任何问题，请随时联系 [out](https://101node.io) 。**

**关注 Shailesh Shekhawat 以便在我发布新帖子时获得通知。**

**如果你认为这是一本值得一读的书，请不要犹豫鼓掌！**

***最初发布于 2018 年 9 月 2 日 [101node.io](https://101node.io/blog/better-logging-with-mongoose-plugins-in-node-js-express-app/) 。***