# 如何优化 Node.js API

> 原文：<https://www.freecodecamp.org/news/how-to-optimize-nodejs-apis/>

在本文中，我将向您介绍一些优化 Node.js 中编写的 API 的最佳方法。

### 先决条件

为了从本文中获得最大收益，您需要理解以下概念:

*   Node.js 设置和安装
*   如何用 Node 构建 API
*   如何使用邮差工具
*   JavaScript async/await 如何工作
*   如何使用基本的 Redis 应用程序

## API 优化实际上意味着什么

优化包括改进 API 的响应时间。响应时间越短，API 就越快。

我将在本文中分享的技巧将帮助您减少响应时间、降低延迟、管理错误和吞吐量，以及最小化 CPU 和内存的使用。

# 如何优化 Node.js APIs

## 1.总是使用异步函数

异步函数就像 JavaScript 的心脏。因此，优化 CPU 使用的最佳方法是编写异步函数来执行非阻塞 I/O 操作。

I/O 操作包括执行读写数据操作的进程。它可以是执行 I/O 操作的数据库、云存储或任何本地存储磁盘。

在大量使用 I/O 操作的应用程序中使用异步函数将会改善这种情况。这是因为由于非阻塞 I/O，CPU 将能够同时处理多个请求，而这些请求中的一个正在进行输入/输出操作。

这里有个例子:

```
var fs = require('fs');
// Performing a blocking I/O
var file = fs.readFileSync('/etc/passwd');
console.log(file);
// Performing a non-blocking I/O
fs.readFile('/etc/passwd', function(err, file) {
    if (err) return err;
    console.log(file);
});
```

*   我们使用 **fs** 节点包来处理文件。
*   **readFileSync()** 是同步的，并且在完成之前阻止执行。
*   readFile() 是异步的，在后台运行时立即返回。

## 2.避免 API 中的会话和 Cookies，只在 API 响应中发送数据。

您使用 cookies 和会话在服务器中存储临时状态。服务器的成本很高。

现在，无状态 API 很常见，它提供了 JWT、OAuth 和其他认证机制。这些身份验证令牌保存在客户端，保护服务器管理状态。

JWT 是用于 API 认证的基于 JSON 的安全令牌。jwt 可以被看到，但是一旦发送就不可修改。JWT 只是连载，没有加密。OAuth 不是一个 API 或服务，而是一个授权的开放标准。OAuth 是获取令牌的一组标准步骤。

另外，不要浪费时间让 Node.js 服务器为静态文件服务。请改用 NGINX 和 Apache，因为在这方面它们比 Node 好得多。

在 Node 中构建 API 时，不要在 API 的响应中发送完整的 HTML 页面。当 API 只发送数据时，节点服务器工作得更好。通常，这种应用程序处理 JSON 数据。

## 3.优化数据库查询

查询优化是在 Node 中构建优化的 API 的重要部分。尤其是在大型应用程序中，您需要多次查询数据库。因此，一个糟糕的查询会降低应用程序的整体性能。

索引是一种优化数据库性能的方法，它通过在处理查询时最小化所需的磁盘访问次数来实现。它是一种数据结构技术，用于快速定位和访问数据库中的数据。索引是使用几个数据库列创建的。

假设我们有一个没有索引的数据库模式，数据库包含 100 万条记录。与带索引的模式相比，简单的查找查询将遍历大量记录来查找匹配的记录。

*   没有索引的查询:

```
> db.user.find({email: 'ofan@skyshi.com'}).explain("executionStats")
```

*   带索引的查询:

```
> db.getCollection("user").createIndex({ "email": 1 }, { "name": "email_1", "unique": true })
{
 "createdCollectionAutomatically" : false,
 "numIndexesBefore" : 1,
 "numIndexesAfter" : 2,
 "ok" : 1
}
```

扫描的文档数量相差巨大~ 1038:

| 方法 | 扫描的文档 |
| --- | --- |
| 无索引 | One thousand and thirty-nine |
| 带索引 | one |

## **4。使用 PM2 集群优化 APIs】**

PM2 是一个为 Node.js 应用程序设计的生产过程管理器。它有一个内置的负载平衡器，允许应用程序作为多个进程运行，而无需修改代码。

使用 PM2，应用程序停机时间几乎为零。总的来说，PM2 确实可以提高 API 的性能和并发性。

在生产环境中部署代码并运行以下命令，查看 PM2 集群如何在所有可用的 CPU 上扩展:

```
pm2 start  app.js -i 0
```

## **5。减少 TTFB(到达第一个字节的时间)**

到达第一个字节的时间是一种度量，用于指示 web 服务器或其他网络资源的响应性。TTFB 测量从用户或客户端发出 HTTP 请求到客户端浏览器收到页面的第一个字节的持续时间。

所有用户在网络浏览器上访问的页面不太可能在 100 毫秒内加载。这仅仅是因为服务器和用户之间的物理距离。

在这里，我们可以通过使用 CDN 和在全球各地的本地数据中心缓存内容来缩短第一个字节的时间。这有助于用户以最小的延迟访问内容。Cloudflare 是您可以开始使用的 CDN 解决方案之一。

## **6。使用带有日志记录的错误脚本**

监控 API 正常运行的最佳方式是跟踪它们的活动。这就是记录数据发挥作用的地方。

日志记录的一个常见例子是将日志打印到控制台(使用`console.log()`)。

与 console.log 相比，更有效的日志模块是 Morgan、Buyan 和 Winston。这里，我以温斯顿为例。

### 如何登录 Winston–功能

*   提供 4 个我们可以使用的自定义级别，如信息、错误、详细、调试、愚蠢和警告。
*   支持查询日志
*   简单剖析
*   您可以使用同一类型的多个传输
*   捕获并记录未捕获的异常

您可以使用以下命令设置 Winston:

```
npm install winston --save
```

下面是 Winston 日志记录的基本配置:

```
const winston = require('winston');

let logger = new winston.Logger({
  transports: [
    new winston.transports.File({
      level: 'verbose',
      timestamp: new Date(),
      filename: 'filelog-verbose.log',
      json: false,
    }),
    new winston.transports.File({
      level: 'error',
      timestamp: new Date(),
      filename: 'filelog-error.log',
      json: false,
    })
  ]
});

logger.stream = {
  write: function(message, encoding) {
    logger.info(message);
  }
};
```

## **7。用 HTTP/2 代替 HTTP**

除了这些技术，我们还可以应用其他一些技术，比如在 HTTP 上使用 HTTP/2，因为它有以下优点:

*   多路技术
*   标题压缩
*   服务器推送
*   二进制格式

它着重于以前版本的 HTTP 所具有的性能和问题。它使网页浏览更快更容易，消耗更少的带宽。

## **8。并行运行任务**

使用 [async.js](https://caolan.github.io/async/v3/) 帮助你运行任务。并行化任务对 API 的性能有很大的影响。它减少了延迟并最大限度地减少了阻塞操作。

并行是指同时运行多件事情。但是，当你并行运行事物时，你不需要控制程序的执行顺序。

下面是一个对数组使用异步并行的简单示例:

```
const async = require("async");
// an example using an object instead of an array
async.parallel({
  task1: function(callback) {
    setTimeout(function() {
      console.log('Task One');
      callback(null, 1);
    }, 200);
  },
  task2: function(callback) {
    setTimeout(function() {
      console.log('Task Two');
      callback(null, 2);
    }, 100);
    }
}, function(err, results) {
  console.log(results);
  // results now equals to: {task2: 2, task1: 1}
});
```

在这个例子中，我们使用 [async.js](https://caolan.github.io/async/v3/) 以异步模式执行这两个任务。任务 1 需要 200 毫秒才能完成，但任务 2 不会等待其完成，它会在指定的 100 毫秒延迟后执行。

并行化任务对 API 的性能有很大的影响。它减少了延迟并最大限度地减少了阻塞操作。

## **9。用 Redis 缓存 App**

Redis 是 Memcached 的高级版本。它通过在服务器的主内存中存储和检索数据来优化 API 的响应时间。它提高了数据库查询的性能，也减少了访问延迟。

在下面的代码片段中，我们分别调用了不带 Redis 和带 Redis 的 API，并比较了响应时间。

响应时间相差巨大~ 899.37ms:

| 方法 | 响应时间 |
| --- | --- |
| 不要再说了 | 900 毫秒 |
| 带缰绳 | 0.621 毫秒 |

这是没有 Redis 的节点:

```
'use strict';

//Define all dependencies needed
const express = require('express');
const responseTime = require('response-time')
const axios = require('axios');

//Load Express Framework
var app = express();

//Create a middleware that adds a X-Response-Time header to responses.
app.use(responseTime());

const getBook = (req, res) => {
  let isbn = req.query.isbn;
  let url = `https://www.googleapis.com/books/v1/volumes?q=isbn:${isbn}`;
  axios.get(url)
    .then(response => {
      let book = response.data.items
      res.send(book);
    })
    .catch(err => {
      res.send('The book you are looking for is not found !!!');
    });
};

app.get('/book', getBook);

app.listen(3000, function() {
  console.log('Your node is running on port 3000 !!!')
});
```

这是 Redis 节点:

```
'use strict';

//Define all dependencies needed
const express = require('express');
const responseTime = require('response-time')
const axios = require('axios');
const redis = require('redis');
const client = redis.createClient();

//Load Express Framework
var app = express();

//Create a middleware that adds a X-Response-Time header to responses.
app.use(responseTime());

const getBook = (req, res) => {
  let isbn = req.query.isbn;
  let url = `https://www.googleapis.com/books/v1/volumes?q=isbn:${isbn}`;
  return axios.get(url)
    .then(response => {
      let book = response.data.items;
      // Set the string-key:isbn in our cache. With the contents of the cache : title
      // Set cache expiration to 1 hour (60 minutes)
      client.setex(isbn, 3600, JSON.stringify(book));

      res.send(book);
    })
    .catch(err => {
      res.send('The book you are looking for is not found !!!');
    });
};

const getCache = (req, res) => {
  let isbn = req.query.isbn;
  //Check the cache data from the server redis
  client.get(isbn, (err, result) => {
    if (result) {
      res.send(result);
    } else {
      getBook(req, res);
    }
  });
}
app.get('/book', getCache);

app.listen(3000, function() {
  console.log('Your node is running on port 3000 !!!')
)};
```

## 结论

在本指南中，我们学习了如何优化 Node.js APIs 的响应时间。

JavaScript 非常依赖函数。因此，使用异步函数可以使脚本更快且无阻塞。

除此之外，我们还使用了缓存(Redis)、数据库索引、TTFB 和 PM2 集群来缩短响应时间。

最后，请记住，关注路线的安全性并确保它们尽可能优化是非常重要的。我们不能因为安全漏洞而牺牲 API 的快速响应。因此，在 Node 中构建优化的 API 时，您应该保留所有的标准安全检查。

在 [LinkedIn](https://www.linkedin.com/in/kadeniyi/) 上与我联系。

再见了！