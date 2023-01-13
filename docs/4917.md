# 如何破坏反作用钩子的基本原理

> 原文：<https://www.freecodecamp.org/news/how-to-destructure-the-fundamentals-of-react-hooks-d13ff6ea6871/>

钩子已经成为 React 的一个非常强大的新特性。如果你真的不知道幕后发生了什么，它们可能会令人生畏。美妙之处在于现在能够在功能组件中以简单(且可重用)的方式管理状态。

但是为什么不用类呢？在不离题太远的情况下，函数提供了一种更直接的方式来编写组件，指导您以一种更干净、更可重用的方式来编写。额外收获:它通常使编写测试更容易。

钩子有很多用例，所以我不会深入举例。用几行快速的台词来达到速度应该不会太差。为了这篇文章，让我们假设浏览器 cookies 不是一个东西，它们是可食用的类型。

### 潜入饼干罐

这里我们有`MyCookies`，一个函数组件，我们可以把它看作我们的 cookie jar。假设我们想在内部跟踪罐子里有多少饼干。有了新的 hooks API，我们可以使用`useState`添加一行简单的代码来处理这项工作。

```
const MyCookies = () => {
  const [ cookies, setCookieCount ] = useState(0);
  ...
};
```

### 等等，我们怎么从里面取出饼干？

如果你认为上面的很神奇，并且想知道数组中的值是如何设置的，你需要理解数组析构的基础。

当析构一个对象时，无论你试图从哪里取出它，都将使用同一个键，而数组析构使用数组中项目的顺序。

```
const [ one, two ] = [ 1, 2 ];
console.log(one); // 1
console.log(two); // 2
```

虽然上面看起来是按照特定的顺序命名的，但事实并非如此:

```
const [ two, one ] = [ 1, 2 ];
console.log(two); // 1
console.log(one); // 2
```

`useState`是一个函数，它返回一个数组，我们正在构造这个数组，以便在组件中使用。

对`useState`本身的调用里面的 0 呢？这只是我们设置状态实例的初始值。在这种情况下，我们将遗憾地从 0 cookies 开始。

### 实际上，使用状态

一旦我们有了析构的`cookies`和`setCookiesCount`函数，我们就可以开始与组件的本地状态交互，就像你在一个类组件中使用`setState`一样。

在呈现时，我们`cookies`值将是对`useState`的内部状态值的调用，类似于您可能在`this.state`中看到的。要更新该值，我们可以简单地调用`setCookiesCount`。

```
const MyCookies = () => {
  const [ cookies, setCookieCount ] = useState(0);
  return (
    <>
      <h2>Cookies: { cookies }</h2>
      <button onClick={() => setCookieCount(cookies + 1)} >
        Add Cookie
      </button>
    </>
  );
};
```

如果您更习惯于类语法，您可以使用类似下面的`this.setState`来更新状态:

```
const MyCookies = () => {
  const [ cookies, setCookieCount ] = useState(0);
  useEffect(() => {
    getCookieCount().then((count) => {
      setCookieCount(count);
    })
  });
  ...
};
```

### 如何使用效果

通常，组件需要一种方法来创建副作用，这种副作用不一定会中断功能组件的功能流。假设我们在某个服务器上保存了 cookies 的数量，我们可能希望在应用程序加载时获取该数量。

```
const MyCookies = () => {
  const [ cookies, setCookieCount ] = useState(0);
  useEffect(() => {
    getCookieCount().then((count) => {
      setCookieCount(count);
    })
  }, []);
  ...
};
```

组件渲染后，`useEffect`中的所有东西都将运行。任何源于`useEffect`的副作用只会在渲染完成后出现。也就是说，一旦`useEffect`确实运行了，我们就触发`getCookieCount`并使用我们之前的`setCookieCount`函数来更新组件的状态。

### 等等，有点不对劲…

不过，上面的代码中有一个问题。这种效果每次都会运行，从根本上消除了我们最初的添加 cookie 按钮对 Cookie 值的任何新的增加。

为了解决这个问题，我们可以为`useEffect`函数设置第二个参数，让 React 知道何时再次运行它。在上面的例子中，将第二个参数设置为空数组将使它只运行一次。

```
const MyCookies = ({cookieType = 'chocolate'}) => {
  const [ cookies, setCookieCount ] = useState(0);
  useEffect(() => {
    getCookieCount().then((count) => {
      setCookieCount(count);
    })
  }, [ cookieType ]);
  ...
};
```

不过在大多数情况下，您会希望传递一个依赖关系数组，当它被更改时，会导致`useEffect`再次触发。比方说，您正在获取特定 cookie 类型的计数，并希望在该类型发生变化时再次获取该计数。

```
import BasketContext from 'context';

const Basket = ({children}) => {
  return (
    <BasketContext.Provider value={basketItems}>
      <h1>My Basket</h1>
      { children }
    </BasketContext.Provider>
  );
}

// MyCookies.js
const MyCookies = ({cookieType = 'chocolate'}) => {
  const basketItems = useContext(BasketContext);
  ...
};
```

在上面的代码中，每当我们的道具`cookieType`改变时，React 知道我们依赖它来获得效果，并将重新运行该效果。

### 试图利用上下文

我不打算[深入 React 的上下文 API](https://reactjs.org/docs/context.html) 的细节，因为那有点超出范围。然而，如果你熟悉它的话，`useContext`钩子可以让你很容易地在你的函数组件中使用你的上下文。在上面的代码中，给定我们已经创建的上下文，我们可以立即“使用”所述上下文，并收集传递给上下文提供者的值。

```
import BasketContext from 'context';

const Basket = ({children}) => {
  return (
    <BasketContext.Provider value={basketItems}>
      <h1>My Basket</h1>
      { children }
    </BasketContext.Provider>
  );
}

// MyCookies.js
const MyCookies = ({cookieType = 'chocolate'}) => {
  const basketItems = useContext(BasketContext);
  ...
};
```

### 清洗你的钩子

让钩子更加强大的是以一种更干净的方式组合和抽象它们，让你的代码变得更加简洁。作为最后一个简单的例子，我们可以把我们的 cookie 例子`useState`和`useEffect`抽象成它们自己的`use[Name]`函数，有效地[创建一个定制的钩子](https://reactjs.org/docs/hooks-custom.html)。

```
// useCookies.js
function useCookies(initialCookieCount) {

  const [ cookies, setCookieCount ] = useState(initialCookieCount);

    useEffect(() => {
    getCookieCount().then((count) => {
      setCookieCount(count);
    })
  }, []);

  function addCookie() {
    setCookieCount(cookies + 1);
    console.log('?');
  }

  function removeCookie() {
    setCookieCount(cookies - 1);
    console.log('?');
  }

  return {
    cookies,
    addCookie,
    removeCookie
  }
};

// MyCookies.js
const MyCookies = () => {
  const { cookies, addCookie, removeCookie } = useCookies(0);
  ...
};
```

我们能够安全地抽象我们的状态逻辑，并仍然利用它来管理我们的 cookies。

### 还有更多的东西值得一试

这些是 React 给我们的 3 个基本钩子，但是[他们还提供了更多现成的钩子](https://reactjs.org/docs/hooks-reference.html)，所有钩子都有 React 文档很好解释的基本原理。

[![Follow me for more Javascript, UX, and other interesting things!](img/1c93a38209f03fa2ee013e5b17071f07.png)](https://twitter.com/colbyfayock)

*   [？在 Twitter 上关注我](https://twitter.com/colbyfayock)
*   [？️订阅我的 Youtube](https://youtube.com/colbyfayock)
*   [✉️注册我的简讯](https://www.colbyfayock.com/newsletter/)

*最初发布于[https://www . colbyfayock . com/2019/04/destructing-the-fundamentals-of-react-hooks](https://www.colbyfayock.com/2019/04/destructuring-the-fundamentals-of-react-hooks)。*