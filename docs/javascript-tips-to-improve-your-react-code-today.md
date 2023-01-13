# 改善 React 代码的 5 个 JavaScript 技巧

> 原文：<https://www.freecodecamp.org/news/javascript-tips-to-improve-your-react-code-today/>

让我们看看现在可以用来立即改进 React 应用程序的五个 JavaScript 技巧。

因为 React 是一个 JavaScript 库，我们在 JavaScript 技能上的任何改进都会直接改进我们用 React 构建的应用程序。

出于这个原因，我收集了一些技巧，向您展示如何使用最新的 JavaScript 特性来使您的 React 代码变得更好。

> 想在最短的时间内成为一名专业的 React 开发人员吗？查看 [**反应训练营**](https://bit.ly/the-react-bootcamp) 。

## 如何在 JavaScript 中使用可选的链接操作符

在 JavaScript 中，我们需要首先确保对象存在，然后才能访问它的属性。

如果对象的值为`undefined`或`null`，将导致类型错误。

在我们的例子中，我们有一个 React 应用程序，用户可以在其中编辑他们发表的帖子。

只有当`isPostAuthor`为真时——这意味着经过验证的用户拥有与文章作者相同的 id——我们才会显示`EditButton`组件。

```
export default function EditButton({ post }) {
  const user = useAuthUser();  
  const isPostAuthor = post.author.userId !== user && user.userId;

  return isPostAuthor ? <EditButton /> : null;
}
```

这段代码的问题是我们的`user`值可能有一个`undefined`值。这就是为什么我们在试图获取`userId`属性之前，必须使用`&&`操作符来确保`user`是一个对象。

如果我们要访问一个对象中的一个对象，我们必须包含另一个`&&`条件。这可能会导致我们的代码变得乏味和难以理解。

幸运的是，一个新的 JavaScript 特性允许我们在不使用结束条件的情况下访问属性之前检查对象是否存在，这个特性就是可选的链接操作符。

要在试图访问某个对象的属性之前确保该对象存在，只需在后面加上一个问号:

```
export default function EditButton({ post }) {
  const user = useAuthUser();  
  const isPostAuthor = post.author.userId !== user?.userId;

  return isPostAuthor ? <EditButton /> : null;
}
```

这将防止任何类型错误，并允许我们编写更干净的条件逻辑。

## 如何在 JavaScript 中使用带括号的隐式返回

在 React 应用程序中，我们可以使用`function`关键字的函数声明语法来编写组件，也可以使用箭头函数，该函数必须设置为变量。

需要注意的是，使用`function`关键字的组件在返回任何 JSX 之前必须使用`return`关键字。

```
export default function App() {
  return (
    <Layout>
      <Routes />
    </Layout>
  );
}
```

我们可以从一个隐式返回的函数中返回多行 JavaScript 代码(不使用`return`关键字)，方法是将返回的代码放在一组括号中。

对于用箭头函数构成的组件，我们不需要包含关键字`return`——我们可以用一组括号返回我们的 JSX。

```
const App = () => (
  <Layout>
    <Routes />
  </Layout>
);

export default App;
```

此外，无论何时使用 React `.map()`函数迭代元素列表，也可以跳过 return 关键字，只在内部函数体中使用一组括号返回 JSX。

```
function PostList() {
  const posts = usePostData();  

  return posts.map(post => (
    <PostListItem key={post.id} post={post} />  
  ))
}
```

## 如何在 JavaScript 中使用 Nullish 合并运算符

在 JavaScript 中，如果某个值是 falsy(比如`null`、`undefined,`、`0`、`''`、`NaN`，我们可以使用 or ( `||`)条件来提供一个 fallback 值。

例如，如果我们有一个产品页面组件，我们想要显示给定产品的价格，您可以使用一个`||`条件来显示价格或者显示文本“产品不可用”。

```
export default function ProductPage({ product }) {    
  return (
     <>
       <ProductDetails />
       <span>{product.price || "Product is unavailable"} // if price is 0, we will see "Product is unavailable"
     </>
  );
}
```

然而，我们现有的代码有一个小错误。

如果价格的值为`0`，这是假的，我们将显示文本“产品不可用”，而不是显示价格本身，这不是我们想要的。

我们需要一个更精确的操作符，如果表达式的左边是`null`或`undefined`而不是任何 falsy 值，那么我们只返回表达式的右边。

这在**无效合并操作符**中是可用的。当它的左操作数为`null`或`undefined`时，它将返回它的右操作数。否则，它将返回其左侧操作数:

```
null ?? 'fallback';
// "fallback"

0 ?? 42;
// 0
```

修复上面代码的方法是简单地用 nullish 合并操作符替换 or 条件，以显示正确的价格`0`。

```
export default function ProductPage({ product }) {    
  return (
     <>
       <ProductDetails />
       <span>{product.price ?? "Product is unavailable"}
     </>
  );
}
```

## 如何在 JavaScript 中使用对象扩展操作符更新状态

当谈到在 React 中使用状态时，我们有几个选择:我们可以使用`useState`钩子为单个原始值创建许多状态变量，或者使用一个对象管理一个状态变量中的多个值。

在下面的例子中，我们有一个注册页面，在这里我们记录当前用户的用户名、电子邮件和密码。

当他们提交注册表单时，我们验证他们输入的表单内容并处理用户注册。

```
import React from 'react'

export default function SignUpPage() {
  const [state, setState] = React.useState({ username: '', email: '', password: '' });

  function handleSubmit(event) {   
    event.preventDefault();
    validateForm(state);
    signUpUser(state)
  }

  function handleChange(event) {
    const {name, value} = event.target;
    setState({ ...state, [name]: value });
  }

  return (
    <form onSubmit={handleSubmit}>
      <input name="username" onChange={handleChange} />
      <input name="email" onChange={handleChange} />
      <input name="password" onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

另外要注意的是，当使用`useState`钩子时，为了更新一个单独的键值对，必须扩展所有先前的状态。

每当用户输入一个输入并且 change 事件发生时，就会调用`handleChange`函数。

然后，我们不仅根据某个输入的`name`属性更新它的值，而且还扩展了用户名、电子邮件和密码的所有当前状态值。我们将所有这些值作为单独的属性分布在新对象中，我们用`...`-对象分布操作符来设置状态。

## 如何使用 Ternaries 在 JavaScript 中有条件地应用类/属性

Ternaries 是在 React 组件中编写条件时使用的基本操作符。

我们经常在我们的 JSX 中使用术语，因为它们是可以显示的一个或另一个值的表达式和解析。这使得它们经常用于显示或隐藏组件和元素。

然而，值得注意的是，当涉及到 JSX 中的任何值时，我们可以使用 ternaries。

这意味着，我们可以用一个内联三元组和一个 JavaScript 模板文字来实现，而不是使用像`classnames`这样的第三方库来有条件地添加或删除 React 元素中的类。

你可以在这个例子中看到，如果我们的用户选择了黑暗模式，我们正在应用一个类`body-dark`。否则我们应用类`body-light`给我们的应用程序适当的样式给我们的`Routes`组件中的所有东西。

```
export default function App() {
  const { isDarkMode } = useDarkMode();

  return (
    <main className={`body ${isDarkMode ? "body-dark" : "body-light"}`}>
      <Routes />
    </main>
  );
}
```

值得注意的是，这种条件逻辑也可以应用于任何属性，而不仅仅是类名或内联样式。

这里我们有另一个例子，我们的应用程序检测用户是否在移动设备上使用特殊的钩子。如果是这样，我们通过道具`height`将一个特定的高度值传递给我们的`Layout`组件。

```
export default function App() {
  const { isMobile } = useDeviceDetect();

  return (
    <Layout height={isMobile ? '100vh' : '80vh'}>
      <Routes />
    </Layout>
  );
}
```

## 想要更多吗？加入 React 训练营

**[React 训练营](http://bit.ly/join-react-bootcamp)** 将你应该知道的关于学习 React 的一切打包成一个全面的包，包括视频、备忘单，外加特殊奖励。

获得内幕信息**100 名开发人员**已经成为 React pro，找到他们梦想的工作，掌控他们的未来:

[![The React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](http://bit.ly/join-react-bootcamp) 
*打开时点击此处通知*