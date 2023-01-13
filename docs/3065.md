# 如何在 JavaScript 中使用 Fetch 进行 AJAX 调用

> 原文：<https://www.freecodecamp.org/news/how-to-use-fetch-api/>

在这个系列中，我将定期分享一些关于 JavaScript 的小知识。我们将涵盖 JS 基础、浏览器、DOM、系统设计、领域架构和框架。

Fetch 是一个用 JavaScript 发出 AJAX 请求的接口。它被现代浏览器广泛实现，用于调用 API。

```
const promise = fetch(url, [options]) 
```

调用 fetch 返回一个承诺，带有一个响应对象。如果出现网络错误，该承诺将被拒绝，如果连接到服务器没有问题，并且服务器响应了状态代码，则问题得到解决。该状态代码可能是 200s、400s 或 500s。

样本提取请求-

```
 fetch(url)
  .then(response => response.json())
  .catch(err => console.log(err)) 
```

默认情况下，请求作为 GET 发送。要发送 POST / PATCH / DELETE / PUT，您可以使用 method 属性作为`options`参数的一部分。一些其他可能的值`options`可以取-

*   `method`:比如 GET，POST，PATCH
*   `headers`:标题对象
*   `mode`:如`cors`、`no-cors`、`same-origin`
*   `cache`:请求的缓存模式
*   `credentials`
*   `body`

[点击此处查看可用选项的完整列表](https://www.freecodecamp.org/news/how-to-use-fetch-api/'https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch')

这个例子演示了如何使用 fetch 来调用一个 API 并获取一个 git 库列表。

```
const url = 'https://api.github.com/users/shrutikapoor08/repos';

fetch(url)
  .then(response => response.json())
  .then(repos => {
    const reposList = repos.map(repo => repo.name);
    console.log(reposList);
  })
.catch(err => console.log(err)) 
```

要发送 POST 请求，下面是如何将方法参数与 async / await 语法一起使用。

```
const params = {
  id: 123
}

const response = await fetch('url', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(params)
});

const data = await response.json(); 
```

* * *

### 对更多 JSBytes 感兴趣？[注册订阅简讯](https://tinyletter.com/shrutikapoor)