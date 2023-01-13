# 你可能会犯的 4 个常见反应错误——以及如何改正它们

> 原文：<https://www.freecodecamp.org/news/common-react-mistakes-and-how-to-fix-them/>

让我们回顾一下您现在在 React 代码中可能会犯的最常见的错误，以及如何修复它们。

如果您想创建令人惊叹的 React 应用程序，那么在这个过程中避免许多常见错误是非常重要的。

在本文中，我们不仅将介绍如何快速修复您的错误，还将为您提供一些非常棒的设计模式，让您的代码在未来变得更好、更可靠。

> 想在最短的时间内成为一名专业的 React 开发人员吗？查看 [**反应训练营**](https://bit.ly/the-react-bootcamp) 。

![Blue-and-Yellow-Photo-Fun-Job-Post--Vacancy--Announcement-Twitter-Post-2-2](img/9e6ed9a4677c3535e1ce2f429393b510.png)

## 1.不要在 React 中将状态变量传递给 setState

在下面的代码中，我们有一个 todo 应用程序，它显示一个 todo 数组(在`TodoList`中)。

我们可以在`AddTodo`组件中添加新的 todos，更新 App 中的`todos`数组。

我们传给`AddTodo`的道具有什么问题？

```
export default function App() {
  const [todos, setTodos] = React.useState([]);

  return (
    <div>
      <h1>Todo List</h1>
      <TodoList todos={todos} />
      <AddTodo setTodos={setTodos} todos={todos} />
    </div>
  );
}

function AddTodo({ setTodos, todos }) {
  function handleAddTodo(event) {
    event.preventDefault();
    const text = event.target.elements.addTodo.value;
    const todo = {
      id: 4,
      text,
      done: false
    };
    const newTodos = todos.concat(todo);
    setTodos(newTodos);
  }

  return (
    <form onSubmit={handleAddTodo}>
      <input name="addTodo" placeholder="Add todo" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

我们将新的 todo 添加到`todos`数组中，然后设置我们应该设置的状态。这将更新`TodoList`组件中显示的待办事项。

然而，由于新状态是基于以前的状态，我们不需要向下传递 todos 数组。

相反，我们可以通过在 setState 函数中编写一个函数来访问之前的 todos 状态。我们从这个函数返回的任何东西都将被设置为新的状态。

换句话说，我们只需要传递`setTodos`函数来正确地更新状态:

```
export default function App() {
  const [todos, setTodos] = React.useState([]);

  return (
    <div>
      <h1>Todo List</h1>
      <TodoList todos={todos} />
      <AddTodo setTodos={setTodos} />
    </div>
  );
}

function AddTodo({ setTodos }) {
  function handleAddTodo(event) {
    event.preventDefault();
    const text = event.target.elements.addTodo.value;
    const todo = {
      id: 4,
      text,
      done: false
    };
    setTodos(prevTodos => prevTodos.concat(todo));
  }

  return (
    <form onSubmit={handleAddTodo}>
      <input name="addTodo" placeholder="Add todo" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

## 2.让您的 React 组件承担单一责任

在下面的应用程序中，我们从应用程序组件中的 API 获取一些用户，将用户数据放入一个状态中，然后在我们的用户界面中显示出来。

`App`组件有什么问题？

```
export default function App() {
  const [users, setUsers] = React.useState([]);

  React.useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((res) => res.json())
      .then((data) => {
        setUsers(data);
      });
  }, []);

  return (
    <>
      <h1>Users</h1>
      {users.map((user) => (
        <div key={user.id}>
          <div>{user.name}</div>
        </div>
      ))}
    </>
  );
}
```

在我们的组件中，我们正在做多件事情。

我们不仅从服务器远程获取数据，还管理状态，并通过 JSX 显示状态。

我们让我们的组件做多种事情。相反，你的组件应该只做一件事，并且把这件事做好。

这是首字母缩写词 SOLID 的一个关键设计原则，它列出了编写更可靠软件的五条规则。

实线中的 S 代表“单一责任原则”，这是编写 React 组件时使用的基本原则。

我们可以将我们的`App`组件分成独立的组件和钩子，每个组件和钩子都有自己的职责。首先，我们将远程数据提取到一个定制的 React 钩子。

这个钩子，我们称之为 useUserData，将负责获取数据并将其放入本地状态。

```
function useUserData() {
  const [users, setUsers] = React.useState([]);

  React.useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((res) => res.json())
      .then((json) => {
        setUsers(json);
      });
  }, []);

  return users;
}
```

之后，我们将调用`App`中的钩子来访问我们的`users`数组。

然而，我们将创建一个单独的`User`组件，包含显示数组中每个元素所需的所有 JSX，以及任何相关的样式(如果有的话)，而不是直接在`App`的 return 语句中显示用户数据。

```
function User({ user }) {
  const styles = {
    container: {
      margin: '0 auto',
      textAlign: 'center' 
    }
  };  

  return (
    <div style={styles.container}>
      <div>{user.name}</div>
    </div>
  );
}

export default function App() {
  const users = useUserData();

  return (
    <>
      <h1>Users</h1>
      {users.map((user) => (
        <User key={user.id} user={user} />
      ))}
    </>
  );
}
```

经过这次重构，我们的组件现在有了一个清晰的、单独的任务要执行，这使得我们的应用程序更容易理解和扩展。

## 3.让你的副作用成为唯一的责任

在下面的`App`组件中，我们获取用户和发布数据。

当我们应用的位置——URL——发生变化时，我们会获取用户和帖子数据。

```
export default function App() {
  const location = useLocation();

  function getAuthUser() {
    // fetches authenticated user
  }

  function getPostData() {
    // fetches post data
  }

  React.useEffect(() => {
    getAuthUser();
    getPostData();
  }, [location.pathname]);

  return (
    <main>
      <Navbar />
      <Post />
    </main>
  );
}
```

如果 URL 改变了，我们会显示一个新的帖子，但是我们需要在每次位置改变时获取它吗？

我们没有。

在许多 React 代码中，您可能会尝试将所有副作用都放在一个单一的使用效果函数中。但这样做违背了我们刚刚提到的单一责任原则。

这会导致一些问题，比如在我们不需要的时候产生副作用。记住，也要把你的副作用保持在一个责任范围内。

为了修复我们的代码，我们需要做的就是在一个单独的使用效果钩子中调用`getAuthUser`。这确保了在位置路径名改变时不会调用它，而只是在我们的应用程序组件挂载时调用一次。

```
export default function App() {
  const location = useLocation();

  React.useEffect(() => {
    getAuthUser();
  }, []);

  React.useEffect(() => {
    getPostData();
  }, [location.pathname]);

  return (
    <main>
      <Navbar />
      <Post />
    </main>
  );
}
```

## 4.在 JSX 用 ternaries 代替`&&`

假设我们在一个专用组件`PostList`中显示一个帖子列表。

在迭代之前检查我们是否有帖子是有意义的。

由于我们的`posts`列表是一个数组，我们可以使用`.length`属性来检查它是否是一个真值(大于 0)。如果是这样的话，我们可以用 JSX 来映射这个数组。

我们可以用 and `&&`运算符来表达这一切:

```
export default function PostList({ posts }) {
  return (
    <div>
      <ul>
        {posts.length &&
          posts.map((post) => <PostItem key={post.id} post={post} />)}
      </ul>
    </div>
  );
} 
```

然而，如果我们执行这样的代码，您可能会对我们所看到的感到惊讶。如果我们的数组是空的，我们什么也看不到，我们看到的是数字 0！

什么？这是为什么呢？！

这是一个 JavaScript 相关的问题，因为我们数组的长度是 0。因为 0 是一个假值，所以`&&`操作符不看表达式的右边。它只返回左边的-0。

解决这个问题并防止将来出现此类错误的最佳方法是什么？

在许多情况下，我们不应该使用 and 运算符，而是使用三元组来明确定义在不满足条件的情况下将显示什么。

如果我们用三元组编写下面的代码，我们将在 else 条件中包含值`null`,以确保不显示任何内容。

```
export default function PostList({ posts }) {
  return (
    <div>
      <ul>
        {posts.length
          ? posts.map((post) => <PostItem key={post.id} post={post} />)
          : null}
      </ul>
    </div>
  );
}
```

通过使用 ternaries 而不是`&&`，你可以避免许多像这样烦人的 bug。

感谢阅读！

## 学习快速反应的第一方法

**[React 训练营](http://bit.ly/join-react-bootcamp)** 将你应该知道的关于学习 React 的一切打包成一个全面的包，包括视频、备忘单，外加特殊奖励。

获取开发人员已经掌握的内幕信息**100 条【React，找到他们梦想的工作，掌控他们的未来:**

[![The React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](http://bit.ly/join-react-bootcamp) 
*打开时点击此处通知*