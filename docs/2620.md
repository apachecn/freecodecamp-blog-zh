# Fetch API Cheatsheet:九个最常见的 API 请求

> 原文：<https://www.freecodecamp.org/news/fetch-api-cheatsheet/>

几乎每个项目都需要和外界沟通。如果您正在使用 JavaScript 框架，您很可能会使用 Fetch API 来完成这项工作。

但是当你使用 API 的时候，你能记住语法吗，或者你需要一点帮助吗？

我已经写了很多关于 JavaScript 和相关事物的文章，只是发现自己后来经常(重新)访问它们来刷新我的记忆或获得一些我知道“在某个地方”的示例代码

在本文中，我的目标是创建另一个类似的资源。我将列出 9 个最常见的获取 API 请求。

我敢肯定你都用过很多次了。但是，避免在旧项目中寻找您半年前使用的特定请求的语法不是很好吗？:)

## 为什么要使用 Fetch API？

如今，我们被那些提供抽象出实际 API 请求的漂亮 SDK 的服务宠坏了。我们只是使用典型的语言结构来请求数据，并不关心实际的数据交换。

但是如果你选择的平台没有 SDK 呢？或者，如果您同时构建了服务器和客户端，该怎么办？在这些情况下，您需要自己处理请求。这就是使用 Fetch API 的方法。

### 使用 Fetch API 的简单 GET 请求

```
fetch('{url}')
    .then(response => console.log(response)); 
```

### 使用 Fetch API 的简单 POST 请求

```
fetch('{url}', {
    method: 'post'
})
    .then(response => console.log(response)); 
```

### 在获取 API 中使用授权令牌(载体)获取

```
fetch('{url}', {
    headers: {
        'Authorization': 'Basic {token}'
    }
})
    .then(response => console.log(response)); 
```

### 在 Fetch API 中使用 querystring 数据获取

```
fetch('{url}?var1=value1&var2=value2')
    .then(response => console.log(response)); 
```

### 在获取 API 中使用 CORS

```
fetch('{url}', {
    mode: 'cors'
})
    .then(response => console.log(response)); 
```

### 在 Fetch API 中使用授权令牌和 querystring 数据进行 POST

```
fetch('{url}?var1=value1&var2=value2', {
    method: 'post',
    headers: {
        'Authorization': 'Bearer {token}'
    }
})
    .then(response => console.log(response)); 
```

### 在获取 API 中用表单数据发布

```
let formData = new FormData();
formData.append('field1', 'value1');
formData.append('field2', 'value2');

fetch('{url}', {
    method: 'post',
    body: formData
})
    .then(response => console.log(response)); 
```

### 在 Fetch API 中用 JSON 数据发布

```
fetch('{url}', {
    method: 'post',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({
        'field1': 'value1',
        'field2': 'value2'
    })
})
    .then(response => console.log(response)); 
```

### 在获取 API 中使用 JSON 数据和 CORS 进行发布

```
fetch('{url}', {
    method: 'post',
    mode: 'cors',
    headers: {
        'Content-Type': 'application/json'
    },
    body: JSON.stringify({
        'field1': 'value1',
        'field2': 'value2'
    })
})
    .then(response => console.log(response)); 
```

## 如何处理获取 API 请求的结果

获取 API 返回一个*承诺*。这就是为什么我总是使用`.then()`和一个回调函数来处理响应:

```
fetch(...).then(response => {
    // process the response
} 
```

但是，如果您在异步函数中，您也可以等待结果:

```
async function getData(){
    let data = await fetch(...);
    // process the response
} 
```

现在让我们看看如何从响应中提取数据:

### 如何检查获取 API 响应的状态代码

当发送 POST、PATCH 和 PUT 请求时，我们通常对返回状态代码感兴趣:

```
fetch(...)
    .then(response => {
        if (response.status == 200){
            // all OK
        } else {
            console.log(response.statusText);
        }
    }); 
```

### 如何获取获取 API 响应的简单值

一些 API 端点可能会发回使用您的数据创建的新数据库记录的标识符:

```
var userId;

fetch(...)
    .then(response => response.text())
    .then(id => {
        userId = id;
        console.log(userId)
    }); 
```

### 如何转换获取 API 响应的 JSON 数据

但是在大多数情况下，您会在响应体中收到 JSON 数据:

```
var dataObj;

fetch(...)
    .then(response => response.json())
    .then(data => {
        dataObj = data;
        console.log(dataObj)
    }); 
```

请记住，只有在两个承诺都解决之后，您才能访问数据。这有时有点令人困惑，所以我总是喜欢使用异步方法并等待结果:

```
async function getData(){
    var dataObj;

    const response = await fetch(...);
    const data = await response.json();
    dataObj = data;
    console.log(dataObj);
} 
```

## 结论

这些样品应该可以满足你的大多数情况。

我是否遗漏了什么，一个你每天都在使用的请求？或者其他你正在挣扎的事情？请在 Twitter 上告诉我，我会在另一篇文章中介绍它:-)

哦，你也可以得到这种[可打印形式的备忘单](https://ondrabus.com/fetch-api-cheatsheet)。