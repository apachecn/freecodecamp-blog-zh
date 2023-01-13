# 如何提高你的反应条件

> 原文：<https://www.freecodecamp.org/news/learn-react-conditionals/>

你能在你的 React 应用程序中正确地编写条件吗？

好的条件是任何 React 应用程序的重要组成部分。我们使用条件来显示或隐藏应用程序中的元素或组件。

简而言之——要成为一名有效的 React 开发人员，您必须知道如何编写好的条件。

让我们回顾一下编写简洁明了的条件句需要知道的所有主要模式，以及应该避免的反模式。

### 想要你自己的副本吗？‬ 📄

********[在此下载 PDF 格式的备忘单](http://bit.ly/react-conditionals-2021)******** (耗时 5 秒)。

以下是获取可下载版本的一些快速制胜之道:

*   快速参考指南，可随时查看
*   大量可复制的代码片段，便于重复使用
*   在最适合你的地方阅读这本大部头的指南。在火车上，在你的办公桌前，排队...任何地方。

有很多很棒的东西要介绍，所以让我们开始吧。

## 1.主要使用 if 语句。不需要 else 或者 else-if。

让我们从 React 中最基本的条件类型开始。如果我们有数据，我们想要显示它。如果没有，我们什么都不想显示。

简单！我们该怎么写呢？

假设我们从一个 API 中获取一组文章数据。当获取数据时，`posts`的值为`undefined`。

我们可以用一个简单的 if 语句来检查这个值。

```
export default function App() {
  const { posts } = usePosts(); // posts === undefined at first

  if (!posts) return null;

  return (
    <div>
      <PostList posts={posts} />
    </div>
  );
}
```

这种模式有效的原因是我们提前返回。如果它满足条件(如果`!posts`有一个布尔值`true`，我们通过返回`null`在组件中不显示任何东西。

当您想要检查多个条件时，If 语句也可以工作。

例如，如果您想在显示数据之前检查加载和错误状态:

```
export default function App() {
  const { isLoading, isError, posts } = usePosts();

  if (isLoading) return <div>Loading...</div>;
  if (isError) return <div>Error!</div>;

  return (
    <div>
      <PostList posts={posts} />
    </div>
  );
}
```

请注意，我们可以重用 if 语句，而不必编写 if-else 或 if-else-if，这减少了我们必须编写的代码，并且仍然可读。

## 2.使用三元运算符在 JSX 中编写条件句

如果我们想提前退出，不显示任何内容或显示完全不同的组件，那么 If 语句非常有用。

然而，如果我们不想从返回的 JSX 中单独写一个条件，而是直接写在它里面呢？

在 React 中，我们必须在我们的 JSX 中包含表达式(解析为值的东西)，而不是语句。

这就是为什么我们必须在 JSX 中只写条件句，而不写 if 语句。

例如，如果我们想在移动设备大小的屏幕上显示一个嵌套组件，而在更大的屏幕上显示另一个嵌套组件，三元组将是一个完美的选择:

```
function App() {
  const isMobile = useWindowSize()

  return (
    <main>
      <Header />
      <Sidebar />
      {isMobile ? <MobileChat /> : <Chat />}
    </main>
  )
}
```

大多数开发人员认为这是他们在使用 ternaries 时唯一可以利用的模式。

事实上，您不必通过在返回的 JSX 中直接包含所有这些术语来打乱您的组件树。

因为三元组解析为一个值，记住你可以将三元组的结果赋给一个变量，然后你可以在你喜欢的地方使用它:

```
function App() {
  const isMobile = useWindowSize();

  const ChatComponent = isMobile ? <MobileChat /> : <Chat />;

  return (
    <main>
      <Header />
      <Sidebar />
      {ChatComponent}
    </main>
  )
}
```

## 3.没有别的条件？使用&& (and)运算符

在许多情况下，您会希望在您的 JSX 中使用三元组，但会意识到如果不满足该条件，您就不想显示任何内容。

这个三元组如下所示:`condition ? <Component /> : null`。

如果没有 else 条件，请使用&&运算符:

```
export default function PostFeed() {
  const { posts, hasFinished } = usePosts()

  return (
    <>
      <PostList posts={posts} />
      {hasFinished && (
        <p>You have reached the end!</p>
      )}
    </>
  )
}
```

## 4.多个条件的 Switch 语句

如果我们处在一个有许多不同条件的情况下，而不仅仅是一两个条件，该怎么办？

我们当然可以写多个 if 语句，但是所有这些 if 语句，正如我们前面看到的，都在返回的 JSX 之上。

太多的 if 语句会使我们的组件变得混乱。我们如何使我们的代码更干净？

我们通常可以将多个条件提取到一个包含 switch 语句的单独组件中。

例如，我们有一个菜单组件，可以切换和显示不同的选项卡。

我们有可以显示用户、聊天和房间数据的选项卡，如下所示:

```
export default function Menu() {
  const [menu, setMenu] = React.useState(1);

  function toggleMenu() {
    setMenu((m) => {
      if (m === 3) return 1;
      return m + 1;
    });
  }

  return (
    <>
      <MenuItem menu={menu} />
      <button onClick={toggleMenu}>Toggle Menu</button>
    </>
  );
}

function MenuItem({ menu }) {
  switch (menu) {
    case 1:
      return <Users />;
    case 2:
      return <Chats />;
    case 3:
      return <Rooms />;
    default:
      return null;
  }
} 
```

由于我们使用带有 switch 语句的专用 MenuItem 组件，我们的父菜单组件不会被条件逻辑弄得杂乱无章，我们可以很容易地看到给定`menu`状态时将显示什么组件。

## 5.想要条件句作为成分？尝试 JSX 控制语句

能够在 React 组件中使用普通 JavaScript 非常有益。但是，如果您想要更多的声明性和简单的条件，请查看 React 库 JSX 控制语句。

您可以通过运行以下命令将其引入 React 项目:

```
npm install --save-dev babel-plugin-jsx-control-statements
```

此外，您可以将其列在您的。babelrc 文件是这样的:

```
{
  "plugins": ["jsx-control-statements"]
}
```

这是一个巴别塔插件，允许你直接在你的 JSX 中使用 React 组件来编写非常容易理解的条件。

理解这样一个库的使用的最好方法是看一个例子。让我们在 JSX 控制语句的帮助下重写我们以前的一个例子:

```
export default function App() {
  const { isLoading, isError, posts } = usePosts();

  return (
    <Choose>
      <When condition={isLoading}>
        <div>Loading...</div>
      </When>
      <When condition={isError}>
        <div>Error!</div>
      </When>
      <Otherwise>
        <PostList posts={posts} />
      </Otherwise>
    </Choose>
  );
}
```

你可以看到没有 if 或三元语句，我们有一个非常可读的组件结构。

在您的下一个 React 项目中尝试一下 JSX 控制语句，看看这样的库是否适合您。

## ******接下来是什么******

我希望这篇指南给了你一些有用的模式来编写伟大的反应条件。

如果您希望保留一份备忘单以供学习之用，您可以[在此下载该备忘单的完整 PDF 版本。](http://bit.ly/react-conditionals-2021)

此外，请查看这些终极资源，它们旨在让您的 React 技能更上一层楼，包括:

*   [React for 初学者:完整指南](https://reactbootcamp.com/react-for-beginners-2021/)
*   [如何从前端到后端读取 React 中的数据](https://reactbootcamp.com/fetch-data-in-react/)
*   [如何在 React with Node 中构建全栈应用](https://reactbootcamp.com/react-app-node-backend/)