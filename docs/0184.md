# Crud 操作–编程中的 CRUD 定义

> 原文：<https://www.freecodecamp.org/news/crud-operations-crud-definition-in-programming/>

当与数据库交互或使用 API 时，您经常会遇到术语 CRUD。它是模型(在 API 的情况下)或数据库管理系统使用的四个基本操作或功能的流行缩写。

这是每个学习计算机编程的人都会遇到的首字母缩写词，熟悉一下它的意思是有好处的。

在本文中，您将了解缩写的每个部分的含义，CRUD 操作符的作用，以及它们与数据库和 API 的关系。

## 什么是 CRUD？

CRUD 是 **C** reate、 **R** ead、 **U** pdate 和 **D** elete 的缩写。其中每一个都执行不同的操作，但是它们都旨在跟踪和管理来自数据库、API 或其他任何东西的数据。

在创建数据库或构建 API 时，您会希望用户能够通过获取这些数据、更新数据、删除数据或添加更多数据来操作任何可用的数据。这些操作是通过 CRUD 操作实现的。

这些操作在数据库中具有功能，因为您可以将它们映射到标准的结构化查询语言(SQL)语句。此外，在使用 RESTful APIs 时，这些操作可以映射到超文本传输协议(HTTP)方法。

在 SQL 中，创建操作可以映射到插入函数，就像 HTTP 请求中的 POST 方法一样。下表总结了每个 CRUD 操作可以映射到 HTTP 请求和 SQL 函数的内容:

| 信 | 操作 | HTTP 请求 | SQL 函数 |
| --- | --- | --- | --- |
| C | 创造 | 邮政 | 插入 |
| 稀有 | 阅读 | 得到 | 挑选 |
| U | 更新 | 修补/上传(如果您有`id`或`uuid` | 更新 |
| D | 删除 | 删除 | 删除 |

通过一些例子，我们现在来理解这些缩写词是如何处理 SQL 和 HTTP 请求的。

## 创建操作的工作原理

顾名思义，您可以使用 create 操作来创建新的记录或条目。该记录可以是用户数据、新项目、信息或要添加到数据库中的新行。

例如，假设您有一个数据库或一组用户，其中包含每个用户的姓名、年龄、用户名、密码和惟一的 ID。您可以向数据库或用户列表中添加新用户(这称为新记录或条目)。

当使用 SQL 时，这被映射到插入函数。一个简单的更新函数如下所示:

```
INSERT INTO table_name (column1, column2, column3, ...)
  VALUES (value1, value2, value3, ...); 
```

在上面的例子中，您使用 **INSERT** 函数通过名称将新值匹配到它们各自的列。您也可以对此进行调整，但重点是插入函数。

当使用 RESTful APIs 时，您将使用 POST HTTP 方法。例如，让我们使用 JavaScript 获取 API:

```
fetch('https://jsonplaceholder.typicode.com/posts', {
  method: 'POST',
  body: JSON.stringify({
    title: 'foo',
    body: 'bar',
    userId: 1,
  }),
  headers: {
    'Content-type': 'application/json; charset=UTF-8',
  },
})
  .then((response) => response.json())
  .then((json) => console.log(json)); 
```

在上面的例子中，一个包含新的`post`的`title`、`body`和`userId`的新对象被添加到`[posts](https://jsonplaceholder.typicode.com/posts)` [API](https://jsonplaceholder.typicode.com/posts) 中。服务器应该返回一个带有 HTTP 响应代码 201(已创建)的头。

## 读取操作如何工作

您可以使用 Read 操作来读取整个数据库，或者基于某些参数来搜索一个或多个条目。

例如，如果您有一个用户数据库，您可以检索整个用户列表或获取特定用户...或者任何你想要的。检索的想法可以称为阅读。

使用 SQL 时，这将映射到 SELECT 函数。一个简单的选择函数如下所示:

```
SELECT * FROM menu; 
```

在上图中，您选择了菜单表中的所有数据。您还可以通过列名请求特定的值:

```
SELECT CustomerID, FirstName, LastName, Email, PhoneNumber
    FROM   Customer 
```

您也可以使用参数等等，但重点是您将始终使用 SELECT 函数。

当使用 RESTful APIs 时，您将使用 GET HTTP 方法。虽然大多数情况下，您不需要指定方法，因为它是默认方法，例如，当使用 JavaScript Fetch API 时:

```
fetch('https://jsonplaceholder.typicode.com/posts')
  .then((response) => response.json())
  .then((json) => console.log(json)); 
```

当没有错误时，这将从 API 返回 JSON 数据，以及表示 OK 的 200 响应代码。如果有错误，它通常会返回 404 响应代码(未找到)。

## 更新操作的工作原理

您可以使用更新操作来修改现有数据。这就像编辑数据一样。也许您想将某个姓名的拼写从 Jon Doe 更正为 John Doe。

当使用 SQL 时，这被映射到更新函数。一个简单的更新函数如下所示:

```
UPDATE user
  SET user_name = 'John Doe', age = 62
  WHERE item_id = 1; 
```

上面的请求将更改指定用户`id`的姓名和年龄。

当使用 RESTful APIs 时，您将使用 PUT 或 PATCH HTTP 方法。

```
fetch('https://jsonplaceholder.typicode.com/posts/1', {
  method: 'PUT',
  body: JSON.stringify({
    id: 1,
    title: 'foo',
    body: 'bar',
    userId: 1,
  }),
  headers: {
    'Content-type': 'application/json; charset=UTF-8',
  },
})
  .then((response) => response.json())
  .then((json) => console.log(json)); 
```

如果操作成功，这将返回一个状态代码为 200 (OK)的响应。

## 删除操作的工作原理

您一定已经猜到，您使用这个操作来删除指定的条目或记录。这与创建操作正好相反，但是对于这一点，您将指定想要删除的 ID(唯一值)。

当使用 SQL 时，这被映射到删除函数。一个简单的删除函数如下所示:

```
DELETE FROM user WHERE user_name='John Doe'; 
```

这将从表中删除名为“John Doe”的行。如果想删除表中的所有记录，可以使用以下命令:

```
DELETE FROM user; 
```

当使用 RESTful APIs 时，您将使用 DELETE 方法:

```
fetch('https://jsonplaceholder.typicode.com/posts/1', {
  method: 'DELETE',
}); 
```

如果成功，这将返回响应代码 204(无内容)。

## 包扎

在本教程中，您已经学习了 CRUD 缩写中每个操作的含义、它们的作用以及它们如何处理 SQL 和 HTTP 请求。

总之，C 代表 Create，用于创建一个新条目。r 代表 Read，用于读取和检索条目。u 代表更新和更新一个条目。d 表示删除，用于删除条目。

在[乔伊·沙赫布](https://www.freecodecamp.org/news/author/joy/)撰写的这篇文章中，您可以通过构建一个 Todo 应用程序来了解更多关于 JavaScript 中的 [CRUD 操作。](https://www.freecodecamp.org/news/learn-crud-operations-in-javascript-by-building-todo-app/)

感谢您的阅读，祝您编码愉快！