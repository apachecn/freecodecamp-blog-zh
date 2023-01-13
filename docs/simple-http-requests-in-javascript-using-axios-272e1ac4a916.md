# 使用 Axios 的 JavaScript 中的简单 HTTP 请求

> 原文：<https://www.freecodecamp.org/news/simple-http-requests-in-javascript-using-axios-272e1ac4a916/>

> 对学习 JavaScript 感兴趣？在 jshandbook.com 获得我的电子书

### 介绍

Axios 是一个非常流行的 JavaScript 库，可以用来执行 HTTP 请求。它在浏览器和 [Node.js](https://flaviocopes.com/nodejs/) 平台上都可以工作。

Is 支持所有现代浏览器，包括 IE8 及更高版本。

它是基于承诺的，这让我们可以非常容易地编写异步/等待代码来执行 [XHR](https://flaviocopes.com/xhr/) 请求。

与原生的 [Fetch API](https://flaviocopes.com/fetch-api/) 相比，使用 Axios 有很多优势:

*   支持较旧的浏览器(获取需要多填充)
*   有办法中止请求
*   有办法设置响应超时
*   内置 CSRF 保护
*   支持上传进度
*   执行自动 JSON 数据转换
*   在 Node.js 中工作

### 装置

可以使用 [npm](https://flaviocopes.com/npm/) 安装 Axios:

```
npm install axios
```

或者[纱](https://flaviocopes.com/yarn/):

```
yarn add axios
```

或者使用 unpkg.com 将它包含在您的页面中:

```
<script src="https://unpkg.com/axios/dist/axios.min.js"><;/script>
```

### Axios API

您可以从`axios`对象启动 HTTP 请求:

```
axios({  url: 'https://dog.ceo/api/breeds/list/all',  method: 'get',  data: {    foo: 'bar'  }})
```

但是为了方便起见，您通常会使用

*   `axios.get()`
*   `axios.post()`

(就像在 jQuery 中一样，您可以使用`$.get()`和`$.post()`而不是`$.ajax()`)

Axios 提供了所有 HTTP 动词的方法，这些方法不太流行，但仍在使用:

*   `axios.delete()`
*   `axios.put()`
*   `axios.patch()`
*   `axios.options()`

它还提供了一种方法来获取请求的 HTTP 头，而丢弃请求体。

### 获取请求

使用 Axios 的一种便捷方式是使用现代(ES2017) async/await 语法。

这个 Node.js 示例使用`axios.get()`查询 [Dog API](https://dog.ceo/) 来检索所有狗品种的列表，并对它们进行计数:

```
const axios = require('axios')const getBreeds = async () => {  try {    return await axios.get('https://dog.ceo/api/breeds/list/all')  } catch (error) {    console.error(error)  }}const countBreeds = async () => {  const breeds = await getBreeds()  if (breeds.data.message) {    console.log(`Got ${Object.entries(breeds.data.message).length} breeds`)  }}countBreeds()
```

如果不想使用 async/await，可以使用 [Promises](https://flaviocopes.com/javascript-promises/) 语法:

```
const axios = require('axios')const getBreeds = () => {  try {    return axios.get('https://dog.ceo/api/breeds/list/all')  } catch (error) {    console.error(error)  }}const countBreeds = async () => {  const breeds = getBreeds()    .then(response => {      if (response.data.message) {        console.log(          `Got ${Object.entries(response.data.message).length} breeds`        )      }    })    .catch(error => {      console.log(error)    })}countBreeds()
```

### 添加参数以获取请求

一个 GET 响应可以在 URL 中包含参数，比如:`[https://site.com/?foo=bar](https://site.com/?foo=bar.)` [。](https://site.com/?foo=bar.)

有了 Axios，您只需使用以下 URL 即可实现这一点:

```
axios.get('https://site.com/?foo=bar')
```

或者您可以在选项中使用一个`params`属性:

```
axios.get('https://site.com/', {  params: {    foo: 'bar'  }})
```

### 发布请求

执行 POST 请求就像执行 GET 请求一样，但是不用`axios.get`，而是使用`axios.post`:

```
axios.post('https://site.com/')
```

包含 POST 参数的对象是第二个参数:

```
axios.post('https://site.com/', { foo: 'bar' })
```

> 对学习 JavaScript 感兴趣？在 jshandbook.com 获得我的电子书