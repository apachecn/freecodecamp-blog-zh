# 什么是休息？面向初学者的 Rest API 定义

> 原文：<https://www.freecodecamp.org/news/what-is-rest-rest-api-definition-for-beginners/>

在本文中，您将了解术语 REST 的含义，以及它如何让我们高效地与服务器通信。

在我们开始休息之前，让我们了解一下什么是 API。相信这会帮助你更好的理解 REST。

## 什么是 API？

既然我们在谈论 REST APIs，我们对 API 的定义就不会超出 web 的范围。

API 代表应用程序编程接口。API 在程序之间建立连接，以便它们可以传输数据。

一个有 API 的程序意味着它的部分数据被暴露给客户使用。客户端可以是同一程序或外部程序的前端。

为了获得这些数据，必须向 API 发送一个结构化请求。如果请求满足期望的要求，包含数据的响应将被发送回发出请求的地方。这种响应通常以 JSON 或 XML 数据的形式出现。

在某些情况下，您需要某种授权来访问外部 API 数据。

每个 API 都有文档，告诉您哪些数据是可用的，以及如何组织您的请求以获得有效的响应。

### API 示例

很困惑吗？

我们用一个真实的生活场景来举例。

想象一下去一家新餐馆。你去那里点餐，因为你以前没有去过那里，你不知道他们提供什么类型的食物。

然后服务员会给你一份菜单，让你挑选你想吃的。做出选择后，服务员会去厨房给你拿食物。

在这种情况下，服务器就是将您连接到厨房的 API。API 的文档就是菜单。当你选择你想吃的东西时，这个请求就产生了，而得到的回应就是提供的食物。

我希望这能帮助你理解什么是 API 以及它是如何工作的。

## 什么是休息？

REST 代表代表性状态转移。它是一个标准，指导我们与存储在 web 服务器上的数据进行交互的过程的设计和开发。

上面的定义可能看起来不像您在互联网上遇到的那些那样复杂或“专业”，但是这里的目标是让您理解 REST APIs 的基本目的。

符合 REST 的六个指导约束中的一些或全部的 API 被认为是 **RESTful 的。**

我们能够使用 HTTP 协议与服务器通信。有了这些协议，我们可以**创建**、**读取**、**更新**和**删除**数据——也称为 **CRUD** 操作。

但是我们如何执行这些 CRUD 操作并与服务器上的数据进行通信呢？

我们可以通过发送 HTTP 请求来做到这一点，这就是 REST 的用武之地。REST 通过提供各种 HTTP 方法/操作/动词来简化通信过程，我们可以使用这些方法/操作/动词向服务器发送请求。

## 如何使用 REST APIs 与服务器通信

正如我们在上一节中讨论的，REST APIs 通过提供各种 HTTP 请求方法，使我们与服务器的通信过程变得更加容易。最常用的方法有:

*   **GET**:GET 方法用于**读取服务器上的**数据。
*   **POST**:POST 方法用于**创建**数据。
*   **PATCH/PUT**:PATCH 方法用于**更新**数据。
*   **删除**:DELETE 方法用于**删除**数据。

REST 提供的这些方法允许我们轻松地执行 CRUD 操作。那就是:

创建= >发布。
Read = > GET。
更新= >补丁/上传。
删除= >删除。

因此，如果我们向服务器发出请求，比方说检索数据，我们将向服务器提供的端点/资源发出一个 **GET** 请求。端点类似于 URL。

如果请求是有效的，那么服务器将用我们请求的数据来响应我们。它还发送一个状态代码，其中 200 表示成功，400 表示客户端错误。

下面是一个使用 JavaScript 向 JSONPlaceholder API 发出请求的示例:

```
fetch('https://jsonplaceholder.typicode.com/todos/1')
  .then(response => response.json())
  .then(json => console.log(json))
```

当使用`fetch` API 发出请求时，默认方法是 GET 方法——所以我们没有被强制指定它。但是我们确实需要指定何时使用其他方法发出请求。

在上面的代码示例中，端点是[https://jsonplaceholder.typicode.com](https://jsonplaceholder.typicode.com)，我们请求的特定数据是一个 todo 项。数据将以 JSON 格式返回给我们。

如果我们要发出一个 POST 请求，那么我们将包含 POST 方法类型和一个请求体，请求体将包含我们正在创建的要发送到服务器的数据。

删除将要求我们使用相应的请求方法以及要删除的 todo 项的 id。像这样:

```
fetch('https://jsonplaceholder.typicode.com/posts/3', {
  method: 'DELETE',
});
```

更新需要 id 和用于更新数据的请求主体。这里有一个例子:

```
fetch('https://jsonplaceholder.typicode.com/posts/5', {
  method: 'PATCH',
  body: JSON.stringify({
    title: 'new todo',
  }),
  headers: {
    'Content-type': 'application/json; charset=UTF-8',
  },
})
  .then((response) => response.json())
  .then((json) => console.log(json));
```

## 结论

在本教程中，您了解了什么是 REST，以及它如何帮助我们高效地与服务器通信。

我们定义了一个 API，并举例说明了它的含义。我们还了解了 REST 提供的一些创建、读取、更新和删除存储在服务器上的数据的方法。

感谢您的阅读！