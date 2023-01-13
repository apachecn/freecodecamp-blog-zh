# 如何使用结果错误模式简化异步 JavaScript

> 原文：<https://www.freecodecamp.org/news/simplify-asynchronous-javascript-using-the-result-error-pattern/>

在过去 18 年的编程生涯中，我不得不在几乎每个项目中处理异步行为。

自从在 JavaScript 中采用 async-await 以来，我们已经了解到 async-await 使许多代码更加令人愉快，也更容易推理。

最近我注意到，当我处理一个需要异步连接和断开的资源时，我最终会写出这样的代码:

```
// NOT MY FAVORITE PATTERN
router.get('/users/:id', async (req, res) => {
  const client = new Client();
  let user;
  try {
    await client.connect();
    user = await client.find('users').where('id', req.path.id);
  } catch(error) {
    res.status(500);
    user = { error };
  } finally {
    await client.close();
  }
  res.json(user);
});
```

它变得冗长，因为我们必须使用 try/catch 来处理错误。

这种资源的例子包括数据库、ElasticSearch、命令行和 ssh。

在那些用例中，我已经习惯了一种代码模式，我称之为结果错误模式。

考虑像这样重写上面的代码:

```
// I LIKE THIS PATTERN BETTER
router.get('/users/:id', async (req, res) => {
  const { result: user, error } = await withDbClient(client => {
    return client.find('users').where('id', req.path.id);
  });
  if (error) {
    res.status(500);
  }
  res.json({ user, error });
}); 
```

注意一些事情:

1.  数据库客户机为我们创建，我们的回调可以利用它。
2.  我们依靠`withDbClient`返回错误，而不是在 try-catch 块中捕获错误。
3.  结果总是被称为`result`，因为我们的回调可能返回任何类型的数据。
4.  我们不必关闭资源。

那么`withDbClient`是做什么的？

1.  它处理资源的创建、连接和关闭。
2.  它处理 try、catch 和 finally。
3.  它确保不会从`withDbClient`抛出未捕获的异常。
4.  它确保处理程序中抛出的任何异常也在`withDbClient`中被捕获。
5.  它确保了`{ result, error }`总是被返回。

下面是一个实现示例:

```
// EXAMPLE IMPLEMENTATION
async function withDbClient(handler) {
  const client = new DbClient();
  let result = null;
  let error = null;
  try {
    await client.connect();
    result = await handler(client);
  } catch (e) {
    error = e;
  } finally {
    await client.close();
  }
  return { result, error };
} 
```

## 更进一步

![pexels-tom-fisk-1595104](img/250b7f01c2615f47561dfc0faff65e9b.png)

Photo by [Tom Fisk](https://www.pexels.com/@tomfisk) from Pexels

不需要关闭的资源怎么办？结果错误模式仍然可以很好！

考虑以下`fetch`的用法:

```
// THIS IS NICE AND SHORT
const { data, error, response } = await fetchJson('/users/123'); 
```

它的实现可能如下:

```
// EXAMPLE IMPLEMENTATION
async function fetchJson(...args) {
  let data = null;
  let error = null;
  let response = null;
  try {
    const response = await fetch(...args);
    if (response.ok) {
      try {
        data = await response.json();
      } catch (e) {
        // not json
      }
    } else {
      // note that statusText is always "" in HTTP2
      error = `${response.status} ${response.statusText}`;
    }
  } catch(e) {
    error = e;  
  }
  return { data, error, response };
} 
```

## 更高层次的使用

![aerial-g3ccde9887_1920](img/2b599a23bd8f7112aa330370ca77f330.png)

Photo by [16018388](https://pixabay.com/users/16018388-16018388/) from Pixabay

我们不必止步于低级使用。其他可能以结果或错误结束的函数呢？

最近我写了一个有很多 ElasticSearch 交互的 app。我决定在高级函数中也使用结果错误模式。

例如，搜索帖子会产生一组 ElasticSearch 文档，并返回如下结果和错误:

```
const { result, error, details } = await findPosts(query);
```

如果您使用过 ElasticSearch，您会知道响应是冗长的，数据嵌套在响应的几个层中。这里，`result`是一个对象，包含:

1.  `records`–一组文档
2.  `total`–未应用限制时的文档总数
3.  `aggregations`–elastic search 分面搜索信息

正如您可能猜到的，`error`可能是一条错误消息，而`details`是完整的 ElasticSearch 响应，以防您需要错误元数据、突出显示或查询时间等信息。

我用查询对象搜索 ElasticSearch 的实现是这样的:

```
// Fetch from the given index name with the given query
async function query(index, query) {
  // Our Result-Error Pattern at the low level  
  const { result, error } = await withEsClient(client => {
    return client.search({
      index,
      body: query.getQuery(),
    });
  });
  // Returning a similar object also with result-error
  return {
    result: formatRecords(result),
    error,
    details: result || error?.meta,
  };
}

// Extract records from responses 
function formatRecords(result) {
  // Notice how deep ElasticSearch buries results?
  if (result?.body?.hits?.hits) {
    const records = [];
    for (const hit of result.body.hits.hits) {
      records.push(hit._source);
    }
    return {
      records,
      total: result.body.hits.total?.value || 0,
      aggregations: result.aggregations,
    };
  } else {
    return { records: [], total: null, aggregations: null };
  }
} 
```

然后`findPosts`函数变得像这样简单:

```
function findPosts(query) {
  return query('posts', query);
}
```

## 摘要

下面是实现结果错误模式的函数的关键方面:

1.  永远不要抛出异常。
2.  总是返回一个带有结果和错误的对象，其中一个可能为空。
3.  隐藏任何异步资源创建或清理。

下面是调用实现结果错误模式的函数的相应好处:

1.  你不需要使用 try-catch 块。
2.  处理错误案例就像`if (error)`一样简单。
3.  您不需要担心安装或清理操作。

不要相信我的话，你自己试试！