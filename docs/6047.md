# 如何在 MongoDB 中自动化数据库迁移

> 原文：<https://www.freecodecamp.org/news/how-to-automate-database-migrations-in-mongodb-d6b68efe084e/>

### **简介**

作为一名软件开发人员，有时您可能不得不以某种方式处理数据库迁移。

随着软件或应用程序的不断发展和改进，您的数据库也必须如此。我们必须确保数据在整个应用程序中保持一致。

有许多不同的方法可以将模式从应用程序的一个版本更改到下一个版本。

*   **新成员加入**
*   **成员被移除**
*   **成员被重命名为**
*   **成员类型改变**
*   **成员的代表被改变**

那么，你如何应对上述所有变化呢？

[https://giphy.com/embed/a5viI92PAF89q](https://giphy.com/embed/a5viI92PAF89q)

[通过 GIPHY](https://giphy.com/gifs/reaction-a5viI92PAF89q)

有两种策略:

*   编写一个脚本，负责将模式升级以及降级到以前的版本
*   使用时更新您的文档

第二个更依赖于代码，必须留在你的代码库中。如果代码以某种方式被删除，那么许多文档是不可升级的。

例如，如果一个文档有 3 个版本，[1、2 和 3]，并且我们删除了从版本 1 到版本 2 的升级代码，那么任何仍然作为版本 1 存在的文档都是不可升级的。我个人认为这是维护代码的开销，并且变得不灵活。

因为这篇文章是关于自动化迁移的，所以我将向您展示如何编写一个简单的脚本来处理模式变更和单元测试。

### 添加了一个成员

当一个成员被添加到模式中时，现有的文档将不包含该信息。所以你需要查询这个成员不存在的所有文档并更新它们。

让我们继续写一些代码。

已经有相当多的 npm 模块可用，但是我使用了库[节点迁移](https://github.com/tj/node-migrate)。我也尝试过其他的，但是其中一些已经没有很好的维护了，而且我在和其他人建立关系时遇到了问题。

#### 先决条件

*   [节点迁移](https://github.com/tj/node-migrate) —节点的抽象迁移框架
*   [MongoDB](https://www.npmjs.com/package/mongodb)—MongoDB for Nodejs 的原生驱动
*   [摩卡](https://mochajs.org/) —测试框架
*   柴 —用于编写测试用例的断言库
*   [蓝鸟](http://bluebirdjs.com/docs/install.html):处理异步 API 调用的 Promise 库
*   [mkdirp](https://www.npmjs.com/package/mkdirp) :类似`mkdir -p`但是在 Node.js 中
*   [rimraf](https://www.npmjs.com/package/rimraf) : `rm -rf`为节点

### 迁移状态

迁移状态是跟踪当前迁移的最重要的关键。没有它，我们就无法追踪:

*   已经完成了多少次迁移
*   最后一次迁移是什么
*   我们正在使用的模式的当前版本是什么

没有状态，就无法回滚、升级到不同的状态，反之亦然。

#### 创建迁移

要创建一个迁移，执行带有标题的`migrate create <tit` le >。

默认情况下，`./migrations/`中将创建一个包含以下内容的文件:

```
'use strict'

module.exports.up = function (next) {
  next()
}

module.exports.down = function (next) {
  next()
}
```

让我们以一个`User`模式为例，其中我们有一个属性`name`，它包括`first`和`last`名称。

现在，我们希望将模式更改为具有单独的`last` name 属性。

因此，为了实现自动化，我们将在运行时读取`name`,提取姓氏并将其保存为新属性。

使用以下命令创建迁移:

```
$ migrate create add-last-name.js
```

这个调用将在根目录下的`migrations`文件夹下创建`./migrations/{timestamp in milliseconds}-add-last-name.js`。

让我们编写代码，将姓氏添加到模式中，并删除它。

#### 向上迁移

我们将找到所有不存在属性`lastName`的用户，并在这些文档中创建新属性`lastName`。

```
'use strict'
const Bluebird = require('bluebird')
const mongodb = require('mongodb')
const MongoClient = mongodb.MongoClient
const url = 'mongodb://localhost/Sample'
Bluebird.promisifyAll(MongoClient)

module.exports.up = next => {
  let mClient = null
  return MongoClient.connect(url)
  .then(client => {
    mClient = client
    return client.db();
  })
  .then(db => {
    const User = db.collection('users')
    return User
      .find({ lastName: { $exists: false }})
      .forEach(result => {
        if (!result) return next('All docs have lastName')
        if (result.name) {
           const { name } = result
           result.lastName = name.split(' ')[1]
           result.firstName = name.split(' ')[0]
        }
        return db.collection('users').save(result)
     })
  })
  .then(() => {

    mClient.close()
    return next()
  })
   .catch(err => next(err))
}
```

#### 向下迁移

类似地，让我们写一个函数，其中我们将删除`lastName`:

```
module.exports.down = next => {
let mClient = null
return MongoClient
   .connect(url)  
   .then(client => {
    mClient = client
    return client.db()
  })
  .then(db =>
    db.collection('users').update(
    {
       lastName: { $exists: true }
    },
    {
      $unset: { lastName: "" },
    },
     { multi: true }
  ))
  .then(() => {
    mClient.close()
    return next()
  })
  .catch(err => next(err))

}
```

#### 运行迁移

在这里查看迁移是如何执行的:[运行迁移](https://github.com/tj/node-migrate#running-migrations)。

### 写入自定义状态存储

默认情况下，`migrate`将已运行的迁移状态存储在一个文件中(`.migrate`)。

`.migrate`文件将包含以下代码:

```
{
  "lastRun": "{timestamp in milliseconds}-add-last-name.js",
  "migrations": [
    {
      "title": "{timestamp in milliseconds}-add-last-name.js",
      "timestamp": {timestamp in milliseconds}
    }
  ]
}
```

但是如果您想做一些不同的事情，比如将它们存储在您选择的数据库中，您可以提供一个定制的存储引擎。

存储引擎有一个简单的接口`load(fn)`和`save(set, fn)`。

只要在`set`时输入的内容在`load`时输出的内容是相同的，那么您就可以开始了！

让我们在项目的根目录下创建文件`db-migrate-store.js`。

```
const mongodb = require('mongodb')
const MongoClient = mongodb.MongoClient
const Bluebird = require('bluebird')

Bluebird.promisifyAll(MongoClient)
class dbStore {
   constructor () {
     this.url = 'mongodb://localhost/Sample' . // Manage this accordingly to your environment
    this.db = null
    this.mClient = null
   }
   connect() {
     return MongoClient.connect(this.url)
      .then(client => {
        this.mClient = client
        return client.db()
      })
   }
    load(fn) {
      return this.connect()
      .then(db => db.collection('migrations').find().toArray())
      .then(data => {
        if (!data.length) return fn(null, {})
        const store = data[0]
        // Check if does not have required properties
          if (!Object
               .prototype
               .hasOwnProperty
               .call(store, 'lastRun') 
                ||
              !Object
              .prototype
              .hasOwnProperty
             .call(store, 'migrations'))
            {
            return fn(new Error('Invalid store file'))
            }
        return fn(null, store)
      }).catch(fn)
    }
   save(set, fn) {
     return this.connect()
      .then(db => db.collection('migrations')
      .update({},
       {
         $set: {
           lastRun: set.lastRun,
         },
         $push: {
            migrations: { $each: set.migrations },
         },
      },
      {
         upsert: true,
         multi: true,
       }
      ))
       .then(result => fn(null, result))
       .catch(fn)
   }
}

module.exports = dbStore
```

`**load(fn)**` 在这个函数中我们只是验证已经加载的现有迁移文档是否包含`lastRun`属性和`migrations`数组。

`**save(set,fn)**` 这里的`set`是由库提供的，我们正在更新`lastRun`的值并将`migrations`追加到现有的数组中。

您可能想知道上面的文件`db-migrate-store.js`在哪里使用。我们创建它是因为我们希望将状态存储在数据库中，而不是代码库中。

下面是您可以看到其用法的测试示例。

### 自动化迁移测试

安装摩卡:

```
$ npm install -g mocha
```

> 我们在全球范围内安装了这个，所以我们将能够从终端运行`mocha`。

#### 结构

要设置基本测试，在项目根目录下创建一个名为“test”的新文件夹，然后在该文件夹中添加一个名为 *migrations* 的文件夹。

您的文件/文件夹结构现在应该如下所示:

```
├── package.json
├── app
│   ├── server.js
│   ├── models
│   │   └── user.js
│   └── routes
│       └── user.js
└── test
       migrations
        └── create-test.js
        └── up-test.js 
        └── down-test.js
```

#### 测试—创建迁移

**目标:**它应该创建迁移目录和文件。

`$ migrate create add-last-name`

这将隐式地在根目录的`migrations`文件夹下创建文件`./migrations/{timestamp in milliseconds}-add-last-name.js`。

现在将以下代码添加到`create-test.js`文件中:

```
const Bluebird = require('bluebird')
const { spawn } = require('child_process')
const mkdirp = require('mkdirp')
const rimraf = require('rimraf')
const path = require('path')
const fs = Bluebird.promisifyAll(require('fs'))

describe('[Migrations]', () => {
    const run = (cmd, args = []) => {
    const process = spawn(cmd, args)
    let out = ""
    return new Bluebird((resolve, reject) => {
       process.stdout.on('data', data => {
         out += data.toString('utf8')
       })
      process.stderr.on('data', data => {
        out += data.toString('utf8')
      })
      process.on('error', err => {
         reject(err)
      })
     process.on('close', code => {
      resolve(out, code)
     })
   })
 }

const TMP_DIR = path.join(__dirname, '..', '..', 'tmp')
const INIT = path.join(__dirname, '..', '..', 'node_modules/migrate/bin', 'migrate-init')
const init = run.bind(null, INIT)
const reset = () => {
   rimraf.sync(TMP_DIR)
   rimraf.sync(path.join(__dirname, '..', '..', '.migrate'))
}

beforeEach(reset)
afterEach(reset)
describe('init', () => {
   beforeEach(mkdirp.bind(mkdirp, TMP_DIR))

   it('should create a migrations directory', done => {
      init()
      .then(() => fs.accessSync(path.join(TMP_DIR, '..', 'migrations')))
      .then(() => done())
      .catch(done)
   })
 })
})
```

在上面的测试中，我们使用`migrate-init`命令创建迁移目录，并在每个测试用例之后使用`rimraf`(在 Unix 中是`rm -rf`)删除它。

稍后我们将使用`fs.accessSync`函数来验证`migrations`文件夹是否存在。

#### 测试迁移

**目标:**它应该将`lastName`添加到模式中，并存储迁移状态。

将以下代码添加到`up-test.js`文件中:

```
const chance = require('chance')()
const generateUser = () => ({
   email: chance.email(),
   name: `${chance.first()} ${chance.last()}`
 })
const migratePath = path.join(__dirname, '..', '..', 'node_modules/migrate/bin', 'migrate')
const migrate = run.bind(null, migratePath)

describe('[Migration: up]', () => {
   before(done => {
     MongoClient
     .connect(url)
     .then(client => {
       db = client.db()
      return db.collection('users').insert(generateUser())
      })
      .then(result => {
       if (!result) throw new Error('Failed to insert')
       return done()
      }).catch(done)
   })
   it('should run up on specified migration', done => {
     migrate(['up', 'mention here the file name we created above', '--store=./db-migrate-store.js'])
    .then(() => {
       const promises = []
       promises.push(
        db.collection('users').find().toArray()
       )
     Bluebird.all(promises)
    .then(([users]) => {
       users.forEach(elem => {
         expect(elem).to.have.property('lastName')
      })
      done()
    })
   }).catch(done)
 })
after(done => {
    rimraf.sync(path.join(__dirname, '..', '..', '.migrate'))
    db.collection('users').deleteMany()
    .then(() => {
      rimraf.sync(path.join(__dirname, '..', '..', '.migrate'))
      return done()
   }).catch(done)
 })
})
```

同样，你可以写下迁移和`before()`和`after()`函数基本保持不变。

### 结论

希望您现在可以通过适当的测试来自动化您的模式更改。:)

从[库](https://github.com/thatshailesh/mongodb-migration)中获取最终代码。

如果你认为这是一本值得一读的书，请不要犹豫鼓掌！