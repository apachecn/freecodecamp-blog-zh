# 如何编写更干净的 React 代码

> 原文：<https://www.freecodecamp.org/news/how-to-write-cleaner-react-code/>

作为 React 开发人员，我们都希望编写更简洁易读的代码。

在本指南中，我总结了七种你现在就可以开始编写更干净的 React 代码的方法，以使构建 React 项目和审查代码变得更加容易。

总的来说，学习如何编写更干净的 React 代码会让你成为一个更有价值、更快乐的 React 开发人员，所以让我们开始吧！

> 想要从头到尾编写干净的 React 代码的完整指南吗？看看 React 训练营。

## 1.利用 JSX 的人手不足

如何将 true 值传递给给定的属性？

在下面的例子中，我们使用 prop `showTitle`在 Navbar 组件中显示应用程序的标题。

```
// src/App.js

export default function App() {
  return (
    <main>
      <Navbar showTitle={true} />
    </main>
  );
}

function Navbar({ showTitle }) {
  return (
    <div>
      {showTitle && <h1>My Special App</h1>}
    </div>
  )
}
```

我们需要显式地将`showTitle`设置为布尔值`true`吗？我们没有！要记住的一个简单方法是，组件上提供的任何属性都有一个默认值 true。

因此，如果我们在 Navbar 上添加道具`showTitle`，我们的 title 元素将会显示:

```
// src/App.js

export default function App() {
  return (
    <main>
      <Navbar showTitle />
    </main>
  );
}

function Navbar({ showTitle }) {
  return (
    <div>
      {showTitle && <h1>My Special App</h1>} // title shown!
    </div>
  )
}
```

另一个需要记住的有用的速记包括传递字符串属性。当你传递一个字符串形式的属性值时，你不需要用花括号把它括起来。

如果我们用`title`属性设置导航条的标题，我们可以用双引号括起它的值:

```
// src/App.js

export default function App() {
  return (
    <main>
      <Navbar title="My Special App" />
    </main>
  );
}

function Navbar({ title }) {
  return (
    <div>
      <h1>{title}</h1>
    </div>
  )
}
```

## 2.将不相关的代码移到单独的组件中

可以说，编写更干净的 React 代码的最简单也是最重要的方法是善于将我们的代码抽象成单独的 React 组件。

让我们看看下面的例子。我们的代码在做什么？

我们的应用程序正在显示一个导航栏组件。我们用`.map()`遍历一组帖子，并在页面上显示它们的标题。

```
// src/App.js

export default function App() {
  const posts = [
    {
      id: 1,
      title: "How to Build YouTube with React"
    },
    {
      id: 2,
      title: "How to Write Your First React Hook"
    }
  ];

  return (
    <main>
      <Navbar title="My Special App" />
      <ul>
        {posts.map(post => (
          <li key={post.id}>
            {post.title}
          </li>
        ))}
      </ul>
    </main>
  );
}

function Navbar({ title }) {
  return (
    <div>
      <h1>{title}</h1>
    </div>
  );
} 
```

我们如何能使这变得更干净？

我们为什么不将循环的代码——我们的帖子——抽象出来，并在一个单独的组件中显示它们，我们称之为 FeaturedPosts。

让我们这样做，看看结果:

```
// src/App.js

export default function App() {
 return (
    <main>
      <Navbar title="My Special App" />
      <FeaturedPosts />
    </main>
  );
}

function Navbar({ title }) {
  return (
    <div>
      <h1>{title}</h1>
    </div>
  );
}

function FeaturedPosts() {
  const posts = [
    {
      id: 1,
      title: "How to Build YouTube with React"
    },
    {
      id: 2,
      title: "How to Write Your First React Hook"
    }
  ];

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
} 
```

如您所见，我们现在可以只查看我们的应用程序组件。通过读取其中组件的名称，Navbar 和 FeaturedPosts，我们可以准确地看到我们的应用程序正在显示什么。

## 3.为每个组件创建单独的文件

与前面的示例不同，我们将所有组件都包含在一个文件中，即 app.js 文件。

类似于我们如何将代码抽象成单独的组件，以使我们的应用程序更可读，为了使我们的应用程序文件更可读，我们可以将每个组件放入一个单独的文件中。

这再次帮助我们在应用程序中分离关注点。这意味着每个文件只负责一个组件，如果我们想在整个应用程序中重用它，就不会混淆组件来自哪里:

```
// src/App.js
import Navbar from './components/Navbar.js';
import FeaturedPosts from './components/FeaturedPosts.js';

export default function App() {
  return (
    <main>
      <Navbar title="My Special App" />
      <FeaturedPosts />
    </main>
  );
}
```

```
// src/components/Navbar.js

export default function Navbar({ title }) {
  return (
    <div>
      <h1>{title}</h1>
    </div>
  );
}
```

```
// src/components/FeaturedPosts.js

export default function FeaturedPosts() {
  const posts = [
    {
      id: 1,
      title: "How to Build YouTube with React"
    },
    {
      id: 2,
      title: "How to Write Your First React Hook"
    }
  ];

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
} 
```

此外，通过将每个单独的组件包含在其自己的文件中，我们避免了一个文件变得过于臃肿。如果我们想将所有组件添加到 app.js 文件中，我们很容易看到该文件变得非常大。

## 4.将共享功能转移到 React 挂钩中

看看我们的 FeaturedPosts 组件，假设我们不显示静态的文章数据，而是想从 API 中获取文章数据。

我们可以用 fetch API 来实现。您可以看到下面的结果:

```
// src/components/FeaturedPosts.js

import React from 'react';

export default function FeaturedPosts() {
  const [posts, setPosts] = React.useState([]);  	

  React.useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then(res => res.json())
      .then(data => setPosts(data));
  }, []);

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

但是，如果我们想要跨多个组件执行这个数据请求，该怎么办呢？

假设除了 FeaturedPosts 组件之外，我们还想创建一个名为 just Posts with same data 的组件。我们必须复制用于获取数据的逻辑，并将其粘贴到组件中。

为了避免这样做，我们为什么不使用一个新的 React 钩子，我们可以称之为`useFetchPosts`:

```
// src/hooks/useFetchPosts.js

import React from 'react';

export default function useFetchPosts() {
  const [posts, setPosts] = React.useState([]);  	

  React.useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/posts')
      .then(res => res.json())
      .then(data => setPosts(data));
  }, []);

  return posts;
}
```

一旦我们在专用的“hooks”文件夹中创建了这个钩子，我们就可以在我们喜欢的任何组件中重用它，包括我们的 FeaturedPosts 组件:

```
// src/components/FeaturedPosts.js

import useFetchPosts from '../hooks/useFetchPosts.js';

export default function FeaturedPosts() {
  const posts = useFetchPosts()

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

## 5.尽可能多地删除 JSX 中的 JavaScript 代码

另一个非常有用但经常被忽视的清理组件的方法是从我们的 JSX 中移除尽可能多的 JavaScript。

让我们看看下面的例子:

```
// src/components/FeaturedPosts.js

import useFetchPosts from '../hooks/useFetchPosts.js';

export default function FeaturedPosts() {
  const posts = useFetchPosts()

  return (
    <ul>
      {posts.map((post) => (
        <li onClick={event => {
          console.log(event.target, 'clicked!');
        }} key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

我们正在尝试处理一个帖子的点击事件。你可以看到我们的 JSX 变得更加难以阅读。假设我们的函数是作为内联函数包含在内的，那么它就模糊了这个组件的用途，以及它的相关函数。

我们能做些什么来解决这个问题？我们可以将连接到`onClick`的内联函数提取到一个单独的处理程序中，我们可以给它一个合适的名字，比如`handlePostClick`。

一旦我们做到了，我们的 JSX 又变得可读了:

```
// src/components/FeaturedPosts.js

import useFetchPosts from '../hooks/useFetchPosts.js';

export default function FeaturedPosts() {
  const posts = useFetchPosts()

  function handlePostClick(event) {
    console.log(event.target, 'clicked!');   
  }

  return (
    <ul>
      {posts.map((post) => (
        <li onClick={handlePostClick} key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

## 6.格式化内联样式以减少代码臃肿

React 开发人员的一个常见模式是在他们的 JSX 中编写内联样式。但是同样，这使得我们的代码更难阅读，也更难编写额外的 JSX:

```
// src/App.js

export default function App() {
  return (
    <main style={{ textAlign: 'center' }}>
      <Navbar title="My Special App" />
    </main>
  );
}

function Navbar({ title }) {
  return (
    <div style={{ marginTop: '20px' }}>
      <h1 style={{ fontWeight: 'bold' }}>{title}</h1>
    </div>
  )
}
```

我们希望将这种关注点分离的概念应用到我们的 JSX 样式中，方法是将我们的内联样式移动到一个 CSS 样式表中，我们可以将它导入到我们喜欢的任何组件中。

重写内联样式的另一种方法是将它们组织成对象。您可以在下面看到这样的模式:

```
// src/App.js

export default function App() {
  const styles = {
    main: { textAlign: "center" }
  };

  return (
    <main style={styles.main}>
      <Navbar title="My Special App" />
    </main>
  );
}

function Navbar({ title }) {
  const styles = {
    div: { marginTop: "20px" },
    h1: { fontWeight: "bold" }
  };

  return (
    <div style={styles.div}>
      <h1 style={styles.h1}>{title}</h1>
    </div>
  );
}
```

## 7.使用反应上下文减少正确钻探

React 项目的另一个基本模式是使用 React 上下文(特别是如果您希望在组件间重用一些公共属性，并且您发现自己编写了许多重复的道具)。

例如，如果我们想要跨多个组件共享用户数据，而不是多个重复的道具(一种称为道具钻取的模式)，我们可以使用 React 库中内置的上下文特性。

在我们的例子中，如果我们想要跨 Navbar 和 FeaturedPosts 组件重用用户数据，我们需要做的就是将整个应用程序包装在一个 provider 组件中。

接下来，我们可以在 value prop 上传递用户数据，并在`useContext`钩子的帮助下在我们的单个组件中使用该上下文:

```
// src/App.js

import React from "react";

const UserContext = React.createContext();

export default function App() {
  const user = { name: "Reed" };

  return (
    <UserContext.Provider value={user}>
      <main>
        <Navbar title="My Special App" />
        <FeaturedPosts />
      </main>
    </UserContext.Provider>
  );
}

// src/components/Navbar.js

function Navbar({ title }) {
  const user = React.useContext(UserContext);

  return (
    <div>
      <h1>{title}</h1>
      {user && <a href="/logout">Logout</a>}
    </div>
  );
}

// src/components/FeaturedPosts.js

function FeaturedPosts() {
  const posts = useFetchPosts();
  const user = React.useContext(UserContext);

  if (user) return null;

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

## 结论

我希望当你试图改进你自己的 React 代码，使它更整洁，更易读，并最终更愉快地创建你的 React 项目时，你会发现这个指南是有用的。

## 喜欢这篇文章吗？加入 React 训练营

**[React 训练营](http://bit.ly/join-react-bootcamp)** 将你应该知道的关于学习 React 的一切打包成一个全面的包，包括视频、备忘单，外加特殊奖励。

获得数百名开发人员已经使用的内部信息，以掌握 React、找到他们梦想的工作并掌控他们的未来:

[![The React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](http://bit.ly/join-react-bootcamp) 
*打开时点击此处通知*