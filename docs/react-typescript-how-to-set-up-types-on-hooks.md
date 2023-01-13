# React 类型脚本备忘单——如何在钩子上设置类型

> 原文：<https://www.freecodecamp.org/news/react-typescript-how-to-set-up-types-on-hooks/>

TypeScript 允许您对代码进行类型检查，以使代码更加健壮和易于理解。

在本指南中，我将向您展示如何在 React 挂钩上设置 TypeScript 类型(useState、useContext、useCallback 等等)。

*   [设置使用状态的类型](#set-types-on-usestate)
*   [在 useRef 上设置类型](#set-types-on-useref)
*   [设置使用上下文的类型](#set-types-on-usecontext)
*   [在 useReducer 上设置类型](#set-types-on-usereducer)
*   [在用户备忘录上设置类型](#set-types-on-usememo)
*   [在 useCallback 上设置类型](#set-types-on-usecallback)

让我们开始吧。

## 在 useState 上设置类型

`useState`钩子允许你在你的 React 应用中管理状态。它相当于类组件中的`this.state`。

```
import * as React from "react";

export const App: React.FC = () => {
 const [counter, setCounter] = React.useState<number>(0)

 return (
    <div className="App">
      <h1>Result: { counter }</h1>
      <button onClick={() => setCounter(counter + 1)}>+</button>
      <button onClick={() => setCounter(counter - 1)}>-</button>
    </div>
  );
} 
```

要在`useState`钩子上设置类型，您需要将状态的类型传递给`<>`。如果没有初始状态，也可以使用像`<number | null>`这样的联合类型。

## 在 useRef 上设置类型

`useRef`钩子返回一个可变的 ref 对象，允许您访问 DOM 元素。

```
import * as React from "react";

export const App: React.FC = () => {
  const myRef = React.useRef<HTMLElement | null>(null)

  return (
    <main className="App" ref={myRef}>
      <h1>My title</h1>
    </main>
  );
} 
```

如您所见，`useRef`接收类型的方式与`useState`钩子相同。你只需要把它传入`<>`。而且，如果你有多个类型注释，就像我在这里做的那样使用联合类型。

## 在 useContext 上设置类型

`useContext`是一个钩子，允许你在 React 应用中访问和使用给定的上下文。

```
import * as React from "react";

interface IArticle {
  id: number
  title: string
}

const ArticleContext = React.createContext<IArticle[] | []>([]);

const ArticleProvider: React.FC<React.ReactNode> = ({ children }) => {
  const [articles, setArticles] = React.useState<IArticle[] | []>([
    { id: 1, title: "post 1" },
    { id: 2, title: "post 2" }
  ]);

  return (
    <ArticleContext.Provider value={{ articles }}>
      {children}
    </ArticleContext.Provider>
  );
}

const ShowArticles: React.FC = () => {
  const { articles } = React.useContext<IArticle[]>(ArticleContext);

  return (
    <div>
      {articles.map((article: IArticle) => (
        <p key={article.id}>{article.title}</p>
      ))}
    </div>
  );
};

export const App: React.FC = () => {
  return (
    <ArticleProvider>
      <h1>My title</h1>
      <ShowArticles />
    </ArticleProvider>
  );
} 
```

在这里，我们首先创建`IArticle`接口，它是我们上下文的类型。
接下来，我们在`createContext()`方法上使用它来创建一个新的上下文，然后用`[]`初始化它。如果你愿意，也可以用`null`作为初始状态。

有了这些，我们现在可以处理上下文的状态，并在`useContext`上设置类型，以期望类型为`IArticle`的数组作为一个值。

## 在 useReducer 上设置类型

`useReducer`钩子帮助你管理更复杂的状态。它是`useState`的替代物——但是请记住它们是不同的。

```
import * as React from "react";

enum ActionType {
  INCREMENT_COUNTER = "INCREMENT_COUNTER",
  DECREMENT_COUNTER = "DECREMENT_COUNTER"
}

interface IReducer {
  type: ActionType;
  count: number;
}

interface ICounter {
  result: number;
}

const initialState: ICounter = {
  result: 0
};

const countValue: number = 1;

const reducer: React.Reducer<ICounter, IReducer> = (state, action) => {
  switch (action.type) {
    case ActionType.INCREMENT_COUNTER:
      return { result: state.result + action.count };
    case ActionType.DECREMENT_COUNTER:
      return { result: state.result - action.count };
    default:
      return state;
  }
};

export default function App() {
  const [state, dispatch] = React.useReducer<React.Reducer<ICounter, IReducer>>(
    reducer,
    initialState
  );

  return (
    <div className="App">
      <h1>Result: {state.result}</h1>
      <button
        onClick={() =>
          dispatch({ type: ActionType.INCREMENT_COUNTER, count: countValue })
        }> +
      </button>
      <button
        onClick={() =>
          dispatch({ type: ActionType.DECREMENT_COUNTER, count: countValue })
        }> -
      </button>
    </div>
  );
} 
```

这里，我们首先声明允许处理计数器的动作类型。接下来，我们分别为 reducer 函数和计数器状态设置两种类型。

减速器需要一个类型为`ICounter`的`state`和一个类型为`IReducer`的`action`。有了那个，现在可以处理计数器了。

`useReducer`钩子接收 reducer 函数和一个初始状态作为参数，并返回两个元素:计数器的`state`和`dispatch`动作。

要设置由`ueReducer`返回的值的类型，只需将您的数据类型传递到`<>`中。

这样，现在可以通过`useReducer`增加或减少计数器。

### 在用户备忘录上设置类型

钩子允许你记忆一个给定函数的输出。它返回一个记忆值。

```
const memoizedValue = React.useMemo<string>(() => {
  computeExpensiveValue(a, b)
}, [a, b]) 
```

要在`useMemo`上设置类型，只需将您想要记忆的数据类型传入`<>`即可。这里，钩子期望一个`string`作为返回值。

## 在 useCallback 上设置类型

钩子允许你记忆一个函数来防止不必要的重新渲染。它返回一个内存化的回调。

```
type CallbackType = (...args: string[]) => void

const memoizedCallback = React.useCallback<CallbackType>(() => {
    doSomething(a, b);
  }, [a, b]); 
```

在这里，我们声明了`CallbackType`类型，它被用作我们想要记忆的回调的类型。

它期望接收类型为`string`的参数，并且应该返回类型为`void`的值。

接下来，我们在`useCallback`上设置该类型——如果您向回调或依赖项数组传递了错误的类型，TypeScript 会对您大喊大叫。

你可以在[我的博客](https://www.ibrahima-ndaw.com)上找到类似这样的精彩内容，或者在 Twitter 上关注我[以获得通知。](https://twitter.com/ibrahima92_)

感谢阅读。