# JavaScript Get 请求——如何用 JS 发出 HTTP 请求

> 原文：<https://www.freecodecamp.org/news/javascript-get-request-tutorial/>

构建应用程序时，您必须在后端和前端之间进行交互，以获取、存储和操作数据。

您的前端应用程序和后端服务器之间的这种交互可以通过 HTTP 请求来实现。

有五种流行的 HTTP 方法可以用来发出请求并与服务器交互。一种 HTTP 方法是 GET 方法，它可以从服务器检索数据。

本文将教您如何通过发出 GET 请求从服务器请求数据。你将学习目前流行的方法和其他一些替代方法。

对于本指南，我们将从免费的 [JSON 占位符帖子 API](https://jsonplaceholder.typicode.com/posts) 中检索帖子。

在 JavaScript 中，有两种流行的方法可以用来发出 HTTP 请求。这些是获取 API 和 Axios。

## 如何使用 Fetch API 发出 GET 请求

Fetch API 是一个内置的 JavaScript 方法，用于检索资源并与后端服务器或 API 端点进行交互。Fetch API 是内置的，不需要安装到您的项目中。

Fetch API 接受一个强制参数:API 端点/URL。该方法还接受一个**选项**参数，这是一个在发出 GET 请求**时的可选对象，因为它是默认请求**。

```
 fetch(url, {
      method: "GET" // default, so we can ignore
  }) 
```

让我们创建一个 GET 请求，从 [JSON 占位符 posts API](https://jsonplaceholder.typicode.com/posts) 中获取一篇文章。

```
fetch("https://jsonplaceholder.typicode.com/posts/1")
  .then((response) => response.json())
  .then((json) => console.log(json)); 
```

这将返回一个帖子，您现在可以将它存储在一个变量中并在您的项目中使用。

> 注意:对于其他方法，如 POST 和 DELETE，您需要将该方法附加到选项数组。

## 如何使用 Axios 发出 GET 请求

Axios 是一个 HTTP 客户端库。这个库基于简化向 REST 端点发送异步 HTTP 请求的承诺。我们将向 JSONPlaceholder Posts API 端点发送一个 GET 请求。

与 Fetch API 不同，Axios 不是内置的。这意味着您需要将 Axios 安装到 JavaScript 项目中。

要将一个依赖项安装到您的 JavaScript 项目中，您将首先通过在终端中运行以下命令来初始化一个新的`npm`项目:

```
$ npm init -y 
```

现在，您可以通过运行以下命令将 Axios 安装到您的项目中:

```
$ npm install axios 
```

一旦 Axios 成功安装，您就可以创建 GET 请求了。这非常类似于 Fetch API 请求。您将把 API 端点/URL 传递给`get()`方法，该方法将返回一个承诺。然后，您可以用`.then()`和`.catch()`方法处理承诺。

```
axios.get("https://jsonplaceholder.typicode.com/posts/1")
.then((response) => console.log(response.data))
.catch((error) => console.log(error)); 
```

> **注意:**主要区别在于，对于 Fetch API，你先把数据转换成 JSON，而 Axios 直接把你的数据作为 JSON 数据返回。

至此，您已经学习了如何使用 Fetch API 和 Axios 发出 GET HTTP 请求。但是还有一些其他的方法仍然存在。其中一些方法是 [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) 和 jQuery。

## 如何使用`XMLHttpRequest`发出 GET 请求

您可以使用 XMLHttpRequest 对象与服务器进行交互。该方法可以从 web 服务器的 API 端点/URL 请求数据，而无需进行完整的页面刷新。

> **注意:**所有现代浏览器都有一个内置的 XMLHttpRequest 对象，用于向服务器请求数据。

让我们通过创建一个新的 XMLHttpRequest 对象来执行相同的请求。然后，您将通过指定请求类型和端点(服务器的 URL)来打开一个连接，然后发送请求，最后监听服务器的响应。

```
const xhr = new XMLHttpRequest();
xhr.open("GET", "https://jsonplaceholder.typicode.com/posts/1");
xhr.send();
xhr.responseType = "json";
xhr.onload = () => {
  if (xhr.readyState == 4 && xhr.status == 200) {
    console.log(xhr.response);
  } else {
    console.log(`Error: ${xhr.status}`);
  }
}; 
```

在上面的代码中，创建了一个新的 XMLHttpRequest 对象，并存储在一个名为`xhr`的变量中。现在，当您指定请求类型(GET)和您想要请求数据的端点/URL 时，您可以使用变量(如`.open()`方法)访问它的所有对象。

您将使用的另一种方法是`.send()`，它将请求发送到服务器。您还可以使用`responseType`方法指定数据返回的格式。此时，GET 请求被发送，您所要做的就是使用`onload`事件监听器监听它的响应。

如果客户端状态为完成( **4)、**且状态码为成功( **200)** ，则数据将被记录到控制台。否则，将出现显示错误状态的错误消息。

## 如何使用 jQuery 发出 GET 请求

在 jQuery 中发出 HTTP 请求相对简单，类似于 Fetch API 和 Axios。要发出 GET 请求，首先要安装 jQuery 或者在项目中使用它的 CDN:

```
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.2/jquery.min.js" integrity="sha512-tWHlutFnuG0C6nQRlpvrEhE4QpkG1nn2MOUMWmUeRePl4e3Aki0VB6W1v3oLjFtd0hVOtRQ9PHpSfN6u6/QXkQ==" crossorigin="anonymous" referrerpolicy="no-referrer"></script> 
```

使用 jQuery，您可以访问 GET 方法`$.get()`，它接受两个参数，API 端点/URL 和请求成功时运行的回调函数。

```
$.get("https://jsonplaceholder.typicode.com/posts/1", (data, status) => {
  console.log(data);
}); 
```

> **注意:**在回调函数中，您可以访问请求的**数据**和请求的**状态**。

您还可以使用 jQuery AJAX 方法，这是一种非常不同的方法，可用于发出异步请求:

```
$.ajax({
  url: "https://jsonplaceholder.typicode.com/posts/1",
  type: "GET",
  success: function (data) {
    console.log(data);
  }
}); 
```

## 包扎

在本文中，您已经学习了如何用 JavaScript 发出 HTTP GET 请求。您现在可能会开始思考——我应该使用哪种方法？

如果是新项目，可以在 Fetch API 和 Axios 之间选择。此外，如果您想为一个小项目使用基本的 API，没有必要使用 Axios，这需要安装一个库。

祝编码愉快！