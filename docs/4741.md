# 在浏览器中索引数据库和存储数据的快速而完整的指南

> 原文：<https://www.freecodecamp.org/news/a-quick-but-complete-guide-to-indexeddb-25f030425501/>

> 对学习 JavaScript 感兴趣？在[jshandbook.com](https://jshandbook.com/)获取我的 JavaScript 电子书

## 指数化介绍 b

IndexedDB 是近年来浏览器中引入的存储功能之一。这是一个键/值存储(noSQL 数据库),被认为是在浏览器中存储数据的权威解决方案**。**

 **它是一个异步 API，这意味着执行高成本的操作不会阻塞 UI 线程，从而为用户提供一个松散的体验。它可以存储无限量的数据，尽管一旦超过某个阈值，用户就会被提示给网站更高的限制。

所有现代浏览器都支持它。

它支持事务、版本控制并提供良好的性能。

在浏览器中，我们还可以使用:

*   [**cookie**](https://flaviocopes.com/cookies/):可以托管非常少量的字符串
*   [**Web Storage**](https://flaviocopes.com/web-storage-api/) (或 DOM Storage)，一个常用来标识 localStorage 和 sessionStorage 的术语，两个键/值存储。sessionStorage 不保留数据，数据在会话结束时被清除，而 localStorage 跨会话保留数据

本地/会话存储的缺点是被限制在一个小的(不一致的)大小，浏览器实现为每个站点提供 2MB 到 10MB 的空间。

过去我们也有 **Web SQL** ，一个 SQLite 的包装器，但现在这是**弃用的**，并且在一些现代浏览器上不受支持，它从来都不是一个公认的标准，所以不应该使用它，尽管根据我能使用吗 83%的用户在他们的设备上有这项技术[。](http://caniuse.com/#feat=sql-storage)

虽然从技术上来说，您可以为每个站点创建多个数据库，但是您通常会**创建一个单独的数据库**，并且在该数据库中，您可以创建**多个对象存储库**。

一个数据库对于一个域名来说是私有的，所以任何其他网站都不能访问另一个被索引的网站。

每个商店通常包含一组*东西*，可以是

*   用线串
*   数字
*   目标
*   数组
*   日期

> 例如，您可能有一个包含帖子的存储，另一个包含评论。

一个商店包含许多具有唯一键的商品，该键代表识别一个对象的方式。

您可以通过执行添加、编辑和删除操作，并迭代它们包含的项目，使用事务来更改这些存储。

自从 ES6 中[承诺](https://flaviocopes.com/javascript-promises)的出现，以及随后 API 转向使用承诺，IndexedDB API 似乎有点*老派*。

虽然这没有什么错，但在我将要解释的所有例子中，我将使用 Jake Archibald 的 [IndexedDB Promised Library](https://github.com/jakearchibald/idb) ，它是 IndexedDB API 之上的一个小层，使它更易于使用。

> 这个库也用于 Google 开发者网站上所有关于 IndexedDB 的例子

## 创建一个索引数据库

最简单的方法是使用 *unpkg* ，将其添加到页面标题:

```
<script type="module">
import { openDB, deleteDB } from 'https://unpkg.com/idb?module'
</script> 
```

在使用 IndexedDB API 之前，请务必检查浏览器中的支持，即使它已被广泛使用，您也永远不知道用户使用的是哪种浏览器:

```
(() => {
  'use strict'

  if (!('indexedDB' in window)) {
    console.warn('IndexedDB not supported')
    return
  }

  //...IndexedDB code
})() 
```

### 如何**创建数据库**

使用`openDB()`:

```
(async () => {
  //...

  const dbName = 'mydbname'
  const storeName = 'store1'
  const version = 1 //versions start at 1

  const db = await openDB(dbName, version, {
    upgrade(db, oldVersion, newVersion, transaction) {
      const store = db.createObjectStore(storeName)
    }
  })
})() 
```

前两个参数是数据库名和版本号。第三个参数是可选的，它是一个包含函数**的对象，只有当版本号高于当前安装的数据库版本**时才会调用这个函数。在函数体中，您可以升级数据库的结构(存储和索引)。

## 将数据添加到存储中

### 创建存储时添加数据，初始化它

您使用对象存储的`put`方法，但是首先我们需要一个对它的引用，这可以在我们创建它的时候从`db.createObjectStore()`中获得。

使用`put`时，值是第一个参数，键是第二个。这是因为如果在创建对象存储时指定了`keyPath`，就不需要在每个 put()请求中输入键名，只需写入值即可。

我们一创建它，它就会填充`store0`:

```
(async () => {
  //...
  const dbName = 'mydbname'
  const storeName = 'store0'
  const version = 1

  const db = await openDB(dbName, version,{
    upgrade(db, oldVersion, newVersion, transaction) {
      const store = db.createObjectStore(storeName)
      store.put('Hello world!', 'Hello')
    }
  })
})() 
```

### 使用事务在已经创建存储时添加数据

为了在以后添加条目，您需要创建一个读/写事务**来确保数据库的完整性(如果一个操作失败，事务中的所有操作都会回滚，并且状态会返回到一个已知的状态)。**

为此，使用对我们在调用`openDB`时获得的`dbPromise`对象的引用，并运行:

```
(async () => {
  //...
  const dbName = 'mydbname'
  const storeName = 'store0'
  const version = 1

  const db = await openDB(/* ... */)

  const tx = db.transaction(storeName, 'readwrite')
  const store = await tx.objectStore(storeName)

  const val = 'hey!'
  const key = 'Hello again'
  const value = await store.put(val, key)
  await tx.done
})() 
```

## 从存储中获取数据

### 从商店获取一件商品:`get()`

```
const key = 'Hello again'
const item = await db.transaction(storeName).objectStore(storeName).get(key) 
```

### 从商店获取所有商品:`getAll()`

存储所有的密钥

```
const items = await db.transaction(storeName).objectStore(storeName).getAllKeys() 
```

获取所有存储的值

```
const items = await db.transaction(storeName).objectStore(storeName).getAll() 
```

## 从索引中删除数据 b

删除数据库、对象存储和数据

### 删除整个 IndexedDB 数据库

```
const dbName = 'mydbname'
await deleteDB(dbName) 
```

### 删除对象存储中的数据的步骤

我们使用一个事务:

```
(async () => {
  //...

  const dbName = 'mydbname'
  const storeName = 'store1'
  const version = 1

  const db = await openDB(dbName, version, {
    upgrade(db, oldVersion, newVersion, transaction) {
      const store = db.createObjectStore(storeName)
    }
  })

  const tx = await db.transaction(storeName, 'readwrite')
  const store = await tx.objectStore(storeName)

  const key = 'Hello again'
  await store.delete(key)
  await tx.done
})() 
```

## 从数据库的早期版本迁移

`openDB()`函数的第三个(可选)参数是一个对象，该对象可以包含一个`upgrade`函数**，只有当版本号高于当前安装的数据库版本**时才会调用该函数。在这个函数体中，您可以升级数据库结构(存储和索引):

```
const name = 'mydbname'
const version = 1
openDB(name, version, {
  upgrade(db, oldVersion, newVersion, transaction) {
    console.log(oldVersion)
  }
}) 
```

在这个回调中，您可以检查用户正在更新哪个版本，并相应地执行一些操作。

您可以使用以下语法从以前的数据库版本执行迁移

```
(async () => {
  //...
  const dbName = 'mydbname'
  const storeName = 'store0'
  const version = 1

  const db = await openDB(dbName, version, {
    upgrade(db, oldVersion, newVersion, transaction) {
      switch (oldVersion) {
        case 0: // no db created before
          // a store introduced in version 1
          db.createObjectStore('store1')
        case 1:
          // a new store in version 2
          db.createObjectStore('store2', { keyPath: 'name' })
      }
      db.createObjectStore(storeName)
    }
  })
})() 
```

## 唯一键

正如您在`case 1`中看到的,`createObjectStore()`接受第二个参数，该参数表示数据库的索引键。这在存储对象时非常有用:`put()`调用不需要第二个参数，只需要取值(一个对象)，键将被映射到同名的对象属性。

索引为您提供了一种在以后通过特定的键检索值的方法，并且它必须是惟一的(每个项目必须有一个不同的键)

可以将一个键设置为自动递增，因此您不需要在客户端代码中跟踪它:

```
db.createObjectStore('notes', { autoIncrement: true }) 
```

如果您的值不包含唯一键(例如，如果您收集没有关联名称的电子邮件地址)，请使用自动递增。

### 检查商店是否存在

您可以通过调用`objectStoreNames()`方法来检查对象存储是否已经存在:

```
const storeName = 'store1'

if (!db.objectStoreNames.contains(storeName)) {
  db.createObjectStore(storeName)
} 
```

## 从索引中删除 b

删除数据库、对象存储和数据

### 删除数据库

```
await deleteDB('mydb') 
```

### 删除对象存储

打开数据库时，只能在回调中删除对象存储，并且只有当您指定的版本高于当前安装的版本时，才会调用该回调:

```
const db = await openDB('dogsdb', 2, {
  upgrade(db, oldVersion, newVersion, transaction) {
    switch (oldVersion) {
      case 0: // no db created before
        // a store introduced in version 1
        db.createObjectStore('store1')
      case 1:
        // delete the old store in version 2, create a new one
        db.deleteObjectStore('store1')
        db.createObjectStore('store2')
    }
  }
}) 
```

### 使用事务删除对象存储中数据

```
const key = 232 //a random key

const db = await openDB(/*...*/)
const tx = await db.transaction('store', 'readwrite')
const store = await tx.objectStore('store')
await store.delete(key)
await tx.complete 
```

## 还有呢！

这些只是基本的。我没有谈论光标和更高级的东西。还有更多的东西需要索引，但我希望这能给你一个好的开始。

> 对学习 JavaScript 感兴趣？在[jshandbook.com](https://jshandbook.com/)拿到我的 JavaScript 书**