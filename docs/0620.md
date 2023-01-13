# 如何在 React 中使用 REST APIs 初学者指南

> 原文：<https://www.freecodecamp.org/news/how-to-consume-rest-apis-in-react/>

React 是一个流行的前端库，开发人员用它来创建应用程序。如果您想构建生产就绪的应用程序，您需要将 API 集成到 React 应用程序中。

每个想要用 React 构建现代、健壮的 web 应用程序的开发人员都必须了解如何使用 API 将数据提取到他们的 React 应用程序中。

在这本初学者指南中，您将学习如何在 React 中使用 RESTful API，包括获取、删除和添加数据。我们还将介绍使用 RESTful APIs 的两种主要方式，以及如何将它们与 React 挂钩一起使用。

## 什么是 REST API？

如果您曾经花时间编程或研究编程，您可能会遇到术语“API”

API 代表应用程序编程接口。它是一种媒介，允许不同的应用程序以编程方式相互通信，并实时返回响应。

Roy Fielding 在 2000 年将 REST 定义为一种通常用于开发互联网服务(如分布式超媒体系统)的架构风格和方法。它是代表“代表性状态转移”的首字母缩写词

当通过 REST API 发出请求时，它向请求者或端点发送资源当前状态的表示。这种状态表示可以采用 JSON (JavaScript 对象表示法)、XML 或 HTML 的形式。

JSON 是使用最广泛的文件格式，因为它与语言无关，人和机器都可以阅读。

**例如:**

```
[
   {
      "userId": 1,
      "id": 1,
      "title": "sunt excepturi",
      "body": "quia et suscipit\nsuscipit recusandae consequuntur "
   },
   {
      "userId": 1,
      "id": 2,
      "title": "qui est esse",
      "body": "est rerum tempore vitae\nsequi sint nihil"
   }
]
```

## 如何在 React 中使用 REST API

您可以通过多种方式在 React 应用程序中使用 REST APIs，但是在本指南中，我们将研究两种最流行的方法:Axios(一种基于 promise 的 HTTP 客户端)和 Fetch API(一种浏览器内置的 web API)。

**注意:**要完全理解本指南，您应该熟悉 JavaScript、React 和 React 钩子，因为它们是本指南的核心。

在我们开始讨论如何使用 API 之前，理解在 React 中使用 API 与在 JavaScript 中使用 API 有很大不同是很重要的。这是因为这些请求现在是在 React 组件中完成的。

在我们的例子中，我们将使用功能组件，这意味着我们需要使用两个主要的 React 钩子:

*   **useEffect 钩子:**在 React 中，我们在`useEffect()`钩子中执行 API 请求。它要么在应用装载时立即呈现，要么在达到特定状态后呈现。这是我们将使用的一般语法:

```
useEffect(() => {
    // data fetching here
}, []); 
```

*   **useState Hook:** 当我们请求数据时，我们必须准备一个状态，当数据被返回时，它将被存储在这个状态中。我们可以将它保存在一个状态管理工具中，比如 Redux，或者保存在一个上下文对象中。为了简单起见，我们将返回的数据存储在 React 本地状态中。

```
const [posts, setPosts] = useState([]); 
```

现在让我们进入本指南的核心部分，在这里我们将学习如何使用 [JSONPlaceholder posts API](https://jsonplaceholder.typicode.com/posts) 来获取、添加和删除数据。这些知识适用于任何类型的 API，因为本指南是面向初学者的。

## 如何使用 Fetch API 消费 API

Fetch API 是一个 JavaScript 内置方法，用于从服务器或 API 端点检索资源。它是内置的，所以你不需要安装任何依赖或软件包。

`fetch()`方法需要一个强制参数，它是您想要获取的资源的路径或 URL。然后它返回一个承诺，这样你就可以使用`then()`和`catch()`方法处理成功或失败。

一个基本的获取请求很容易编写，看起来像下面的代码。我们只是从一个以 JSON 形式返回数据的 URL 获取数据，然后将它记录到控制台:

```
fetch('https://jsonplaceholder.typicode.com/posts?_limit=10')
   .then(response => response.json())
   .then(data => console.log(data)); 
```

默认响应通常是常规的 HTTP 响应，而不是实际的 json，但是我们可以通过使用响应的 JSON()方法将输出作为 JSON 对象。

### 如何在 React With Fetch API 中执行 GET 请求

您可以使用 HTTP GET 方法从端点请求数据。

如前所述，Fetch API 接受一个强制参数，这是真的。它还接受一个选项参数，这是可选的，特别是在使用 GET 方法时，这是默认的。但是对于 POST 和 DELETE 等其他方法，您需要将该方法附加到选项数组:

```
fetch(url, {
    method: "GET" // default, so we can ignore
}) 
```

到目前为止，我们已经了解了事情是如何工作的，所以让我们把所学的东西放在一起，执行一个 get 请求来从我们的 API 获取数据。

同样，我们将使用[免费在线 API JSONPlaceholder](https://jsonplaceholder.typicode.com/posts) 将帖子列表提取到我们的应用程序中:

```
import React, { useState, useEffect } from 'react';

const App = () => {
   const [posts, setPosts] = useState([]);
   useEffect(() => {
      fetch('https://jsonplaceholder.typicode.com/posts?_limit=10')
         .then((response) => response.json())
         .then((data) => {
            console.log(data);
            setPosts(data);
         })
         .catch((err) => {
            console.log(err.message);
         });
   }, []);

return (
   // ... consume here
);
}; 
```

我们在前面的代码中创建了一个状态来存储我们将从 API 中检索的数据，以便我们可以在以后的应用程序中使用它。我们还将默认值设置为一个空数组。

```
const [posts, setPosts] = useState([]); 
```

然后，主要操作发生在 useEffect 状态中，因此应用程序一加载，数据/帖子就被获取。获取请求产生一个承诺，我们可以接受或拒绝:

```
useEffect(() => {
   fetch('https://jsonplaceholder.typicode.com/posts?_limit=10').then(
      (response) => console.log(response)
   );
}, []); 
```

这个响应包含了大量的数据，比如状态代码、文本和其他我们以后处理错误时需要用到的信息。

到目前为止，我们已经使用`.then()`处理了一个解析，但是它返回了一个响应对象，这不是我们想要的。所以我们需要使用`json()`方法将响应对象解析为 JSON 格式。这也为我们返回了使用第二个`.then()`来获得实际数据的承诺。

```
useEffect(() => {
   fetch('https://jsonplaceholder.typicode.com/posts?_limit=10')
      .then((response) => response.json())
      .then((data) => {
         console.log(data);
         setPosts(data);
      });
}, []); 
```

如果我们看一下控制台，我们会看到我们已经从 API 中检索了 10 篇文章，我们还将其设置为我们之前指定的状态。

这并不完整，因为我们只处理了承诺的决心，而没有处理承诺的拒绝，我们将使用`.catch()`方法来处理:

```
useEffect(() => {
   fetch('https://jsonplaceholder.typicode.com/posts?_limit=10')
      .then((response) => response.json())
      .then((data) => {
         console.log(data);
         setPosts(data);
      })
      .catch((err) => {
         console.log(err.message);
      });
}, []); 
```

到目前为止，我们已经看到了如何执行一个`GET`请求。通过遍历我们的数组，这可以很容易地在我们的应用程序中使用:

```
const App = () => {
// ...

   return (
   <div className="posts-container">
      {posts.map((post) => {
         return (
            <div className="post-card" key={post.id}>
               <h2 className="post-title">{post.title}</h2>
               <p className="post-body">{post.body}</p>
               <div className="button">
               <div className="delete-btn">Delete</div>
               </div>
            </div>
         );
      })}
   </div>
   );
};

export default App; 
```

### 如何在 React With Fetch API 中执行 POST 请求

您可以使用 HTTP `POST`方法向端点发送数据。它的工作方式类似于`GET`请求，主要区别在于您需要向可选对象添加方法和两个额外的参数:

```
const addPosts = async (title, body) => {
await fetch('https://jsonplaceholder.typicode.com/posts', {
method: 'POST',
body: JSON.stringify({
   title: title,
   body: body,
   userId: Math.random().toString(36).slice(2),
}),
headers: {
   'Content-type': 'application/json; charset=UTF-8',
},
})
.then((response) => response.json())
.then((data) => {
   setPosts((posts) => [data, ...posts]);
   setTitle('');
   setBody('');
})
.catch((err) => {
   console.log(err.message);
});
}; 
```

可能看起来奇怪的主要参数是主体和头。

主体保存了我们想要传递给 API 的数据，我们必须首先对其进行字符串化，因为我们要将数据发送到 web 服务器。头部告诉我们数据的类型，在使用 REST API 时总是相同的。我们还将状态设置为保存新数据，并将剩余数据分发到数组中。

看看我们创建的`addPost()`方法，它期望这些数据来自一个表单或其他什么。在我们的例子中，我创建了一个表单，通过状态获取表单数据，然后在提交表单时将其添加到方法中:

```
import React, { useState, useEffect } from 'react';
const App = () => {
const [title, setTitle] = useState('');
const [body, setBody] = useState('');
// ...
const addPosts = async (title, body) => {
   await fetch('https://jsonplaceholder.typicode.com/posts', {
      method: 'POST',
      body: JSON.stringify({
         title: title,
         body: body,
         userId: Math.random().toString(36).slice(2),
      }),
      headers: {
         'Content-type': 'application/json; charset=UTF-8',
      },
   })
      .then((response) => response.json())
      .then((data) => {
         setPosts((posts) => [data, ...posts]);
         setTitle('');
         setBody('');
      })
      .catch((err) => {
         console.log(err.message);
      });
};

const handleSubmit = (e) => {
   e.preventDefault();
   addPosts(title, body);
};    

return (
   <div className="app">
      <div className="add-post-container">
         <form onSubmit={handleSubmit}>
            <input type="text" className="form-control" value={title}
               onChange={(e) => setTitle(e.target.value)}
            />
            <textarea name="" className="form-control" id="" cols="10" rows="8" 
               value={body} onChange={(e) => setBody(e.target.value)} 
            ></textarea>
            <button type="submit">Add Post</button>
         </form>
      </div>
      {/* ... */}
   </div>
);
};

export default App; 
```

### 如何在 React With Fetch API 中执行删除请求

您可以使用 HTTP `DELETE`方法从端点移除数据。它的工作方式类似于`GET`请求，主要区别是增加了方法:

```
const deletePost = async (id) => {
await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
   method: 'DELETE',
}).then((response) => {
   if (response.status === 200) {
      setPosts(
         posts.filter((post) => {
            return post.id !== id;
         })
      );
   } else {
      return;
   }
});
}; 
```

这在按钮被点击时被触发，我们得到按钮被点击的具体文章的`id`。然后，我们从整个返回的数据中删除该数据。

这将从 API 中删除，但不会立即从 UI 中删除，这就是为什么我们添加了一个过滤器来删除数据。对于循环中的每一项，您的删除按钮将如下所示:

```
const App = () => {
// ...

   return (
   <div className="posts-container">
      {posts.map((post) => {
         return (
            <div className="post-card" key={post.id}>
               {/* ... */}
               <div className="button">
                  <div className="delete-btn" onClick={() => deletePost(post.id)}>
                     Delete
                  </div>
               </div>    
            </div>
         );
      })}
   </div>
   );
};

export default App; 
```

### 如何在获取 API 中使用异步/等待

到目前为止，我们已经看到了如何使用 promise 语法正常地发出 fetch 请求，这有时会令人困惑。然后是链接。我们可以通过使用 Async/await 来避免`.then()`链接，并编写更可读的代码。

要使用 async/await，首先在函数中调用`async`。然后，当发出请求并期待响应时，在函数前面添加`await`语法，等待直到承诺与结果达成一致。

当我们使用 async/await 时，我们所有的 Fetch 请求将如下所示:

```
import React, { useState, useEffect } from 'react';

const App = () => {
   const [title, setTitle] = useState('');
   const [body, setBody] = useState('');
   const [posts, setPosts] = useState([]);

   // GET with fetch API
   useEffect(() => {
      const fetchPost = async () => {
         const response = await fetch(
            'https://jsonplaceholder.typicode.com/posts?_limit=10'
         );
         const data = await response.json();
         console.log(data);
         setPosts(data);
      };
      fetchPost();
   }, []);

   // Delete with fetchAPI
   const deletePost = async (id) => {
      let response = await fetch(
         `https://jsonplaceholder.typicode.com/posts/${id}`,
         {
            method: 'DELETE',
         }
      );
      if (response.status === 200) {
         setPosts(
            posts.filter((post) => {
               return post.id !== id;
            })
         );
      } else {
         return;
      }
   };

   // Post with fetchAPI
   const addPosts = async (title, body) => {
      let response = await fetch('https://jsonplaceholder.typicode.com/posts', {
         method: 'POST',
         body: JSON.stringify({
            title: title,
            body: body,
            userId: Math.random().toString(36).slice(2),
         }),
         headers: {
            'Content-type': 'application/json; charset=UTF-8',
         },
      });
      let data = await response.json();
      setPosts((posts) => [data, ...posts]);
      setTitle('');
      setBody('');
   };

   const handleSubmit = (e) => {
      e.preventDefault();
      addPosts(title, body);
   };

   return (
      // ...
   );
};

export default App; 
```

### 如何用 Fetch API 处理错误

在这一节中，我们将看看如何用传统方法和 async/await 来处理错误。

我们可以使用响应数据来处理 Fetch API 中的错误，或者在使用 async/await 时使用 try/catch 语句。

让我们看看如何在 Fetch API 中实现这一点:

```
const fetchPost = () => {
fetch('https://jsonplaceholder.typicode.com/posts?_limit=10')
   .then((response) => {
      if (!response.ok) {
         throw Error(response.statusText);
      }
      return response.json();
   })
   .then((data) => {
      console.log(data);
      setPosts(data);
   })
   .catch((err) => {
      console.log(err.message);
   });
}; 
```

你可以在这里阅读更多关于获取 API 错误的信息。

对于异步/等待，我们可以像这样使用`try`和`catch`:

```
const fetchPost = async () => {
   try {
      const response = await fetch(
         'https://jsonplaceholder.typicode.com/posts?_limit=10'
      );
      const data = await response.json();
      setPosts(data);
   } catch (error) {
      console.log(error);
   }
}; 
```

## 如何使用 Axios 消费 API

Axios 是一个基于 promises 的 HTTP 客户端库，它使得向 REST 端点发送异步 HTTP 请求变得简单。在我们的例子中，这个端点是 JSONPlaceholder Posts API，我们将向它发出`GET`、`POST`和`DELETE`请求。

### 如何安装和配置 Axios 实例

与 Fetch API 不同，Axios 不是内置的，所以我们需要将它合并到我们的项目中才能使用它。

您可以通过运行以下命令将 Axios 添加到项目中:

```
npm install axios 
```

一旦您成功安装了 Axios，我们可以继续创建一个实例，这是可选的，但建议您这样做，因为这样可以避免不必要的重复。

为了创建一个实例，我们使用了`.create()`方法，我们可以用它来指定信息，比如 URL 和可能的头:

```
import axios from "axios";

const client = axios.create({
   baseURL: "https://jsonplaceholder.typicode.com/posts" 
}); 
```

### 如何在 Axios 中执行 GET 请求

我们将使用前面声明的实例来执行 GET 请求。我们所要做的就是设置参数(如果有的话)，并默认以 JSON 的形式获得响应。

与 Fetch API 方法不同，声明该方法不需要任何选项。我们只需将方法附加到实例并查询它。

```
useEffect(() => {
   client.get('?_limit=10').then((response) => {
      setPosts(response.data);
   });
}, []); 
```

### 如何在 Axios 中执行 POST 请求

如前所述，您可以使用`POST`方法向端点发送数据。它的功能类似于`GET`请求，主要区别是需要包含方法和一个选项来保存我们发送的数据:

```
const addPosts = (title, body) => {
   client
      .post('', {
         title: title,
         body: body,
      })
      .then((response) => {
         setPosts((posts) => [response.data, ...posts]);
      });
}; 
```

### 如何在 Axios 中执行删除请求

我们可以使用 delete 方法执行删除请求，该方法获取`id`并将其从 API 中删除。我们还将使用 filter 方法将其从 UI 中移除，就像我们使用 Fetch API 方法一样:

```
const deletePost = (id) => {
   client.delete(`${id}`);
   setPosts(
      posts.filter((post) => {
         return post.id !== id;
      })
   );
}; 
```

### 如何在 Axios 中使用 Async/Await

到目前为止，我们已经看到了如何使用 promise 语法发出 Axios 请求。但是现在让我们看看如何使用 async/await 来编写更少的代码并避免`.then()`链接。

当我们使用 async/await 时，我们所有的 Axios 请求看起来都像这样:

```
import React, { useState, useEffect } from 'react';

const App = () => {
   const [title, setTitle] = useState('');
   const [body, setBody] = useState('');
   const [posts, setPosts] = useState([]);

   // GET with Axios
   useEffect(() => {
      const fetchPost = async () => {
         let response = await client.get('?_limit=10');
         setPosts(response.data);
      };
      fetchPost();
   }, []);

   // Delete with Axios
   const deletePost = async (id) => {
      await client.delete(`${id}`);
      setPosts(
         posts.filter((post) => {
            return post.id !== id;
         })
      );
   };

   // Post with Axios
   const addPosts = async (title, body) => {
      let response = await client.post('', {
         title: title,
         body: body,
      });
      setPosts((posts) => [response.data, ...posts]);
   };

   const handleSubmit = (e) => {
      e.preventDefault();
      addPosts(title, body);
   };

   return (
      // ...
   );
};

export default App; 
```

### 如何处理 Axios 的错误

对于基于承诺的 Axios 请求，我们可以使用`.then()`和。`catch (`)方法，但是对于 async/await，我们可以使用`try...catch`块。这与我们实现 Fetch API 的方式非常相似，并且`try...catch`块将如下所示:

```
const fetchPost = async () => {
   try {
      let response = await client.get('?_limit=10');
      setPosts(response.data);
   } catch (error) {
      console.log(error);
   }
}; 
```

你可以在这里阅读更多关于使用 Axios [处理错误的信息。](https://stackabuse.com/handling-errors-with-axios/)

## 获取 API 与 Axios

您可能已经注意到了一些差异，但是让我们将它们放在一个方便的表格中，以便我们可以正确地比较 Fetch 和 Axios。

这些区别将帮助您决定对特定项目使用哪种方法。这些区别包括:

| 阿克斯 | 取得 |
| --- | --- |
| Axios 是一个独立的第三方软件包，易于安装。 | Fetch 内置于大多数现代浏览器中。  **不需要安装** 。 |
| Axios 使用了  **数据** 属性。 | Fetch 使用了  **体** 体属性。 |
| Axios 数据包含了  **对象**。 | Fetch 的主体必须是  **字符串化的**。 |
| 当状态为 200 且 statusText 为‘OK’时，Axios 请求被接受。 | 当  **响应对象包含 ok 属性**时，获取请求是 ok 的。 |
| Axios 执行 JSON 数据的  **自动转换。** | Fetch 是一个  **两步流程** 处理 JSON 数据时——首先，做出实际请求；第二，调用。响应上的 json()方法。 |
| Axios 允许  **取消请求和请求超时**。 | Fetch 没有。 |
| Axios 有  **内置支持下载进度**。 | Fetch 不支持上载进度。 |
| Axios 拥有  **广浏览器支持**。 | Fetch 只兼容 Chrome 42+，Firefox 39+，Edge 14+，Safari 10.1+。(这就是所谓的向后兼容性)。 |

## 结论

在本指南中，我们学习了如何使用 Fetch API 或 Axios 在 React 中使用 REST APIs。

这将帮助您开始使用 React 中的 API，然后您将能够以更复杂的方式使用数据，并按照您选择的方式操作您的 API。