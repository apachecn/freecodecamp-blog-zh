# 为什么赤裸裸的承诺对工作不安全——以及该怎么做

> 原文：<https://www.freecodecamp.org/news/naked-promises-are-not-safe-for-work/>

这篇文章讲述了我个人的发现之旅和采用传统智慧的斗争，因为它与前端的异步工作有关。如果幸运的话，您至少会对跨越同步到异步边界时需要处理的 3 种棘手情况有更深的理解。我们甚至可能会得出这样的结论:你永远也不想再自己手动考虑这些边缘情况了。

我的例子是在 React 中，但我相信它们是在所有前端应用程序中都有相似之处的普遍原则。

## 什么是“赤裸裸的承诺”？

为了在我们的应用程序中做任何有趣的事情，我们可能会在某个时候使用异步 API。在 JavaScript 中，承诺已经取代回调成为异步 API 的首选(特别是当每个平台都开始接受`async` / `await`)。它们甚至已经成为“网络平台”的一部分——这里有一个在所有现代浏览器中使用基于承诺的`fetch` API 的典型例子:

```
function App() {
  const [msg, setMsg] = React.useState('click the button')
  const handler = () =>
    fetch('https://myapi.com/')
      .then((x) => x.json())
      .then(({ msg }) => setMsg(msg))

  return (
    <div className="App">
      <header className="App-header">
        <p>message: {msg}</p>
        <button onClick={handler}> click meeee</button>
      </header>
    </div>
  )
} 
```

这里，我们的按钮的`handler`函数返回一个“裸”承诺——它没有被任何东西包装，它只是被直接调用，所以它可以获取数据和设置状态。这是在所有介绍中教授的一个极其常见的模式。这对于演示应用程序来说很好，但是在现实世界中，用户经常会遇到许多这种模式容易忘记考虑的边缘情况。

## 承诺失败:错误状态

承诺落空。只为“快乐之路”编码太容易了，在这条路上，您的网络总是在工作，您的 API 总是返回一个成功的结果。大多数开发人员都非常熟悉那些只在生产中出现的无法捕捉的异常，这些异常会让你的应用程序看起来无法工作或者陷入某种加载状态。有 [ESlint 规则确保你在承诺上写上`.catch`](https://github.com/xjamundx/eslint-plugin-promise/blob/HEAD/docs/rules/catch-or-return.md) 经手人。

这只对用`.then`链接的承诺有帮助，但对将承诺传递给不受你控制的库或直接调用承诺没有帮助。

无论哪种方式，显示错误状态的责任最终都将落在您的身上，看起来会像这样:

```
function App() {
  const [msg, setMsg] = React.useState('click the button')
  const [err, setErr] = React.useState(null)
  const handler = () => {
    setErr(null)
    fetch('https://myapi.com/')
      .then((x) => x.json())
      .then(({ msg }) => setMsg(msg))
      .catch((err) => setErr(err))
  }

  return (
    <div className="App">
      <header className="App-header">
        <p>message: {msg}</p>
        {err && <pre>{err}</pre>}
        <button onClick={handler}>click meeee</button>
      </header>
    </div>
  )
} 
```

现在，我们的应用程序中的每个异步操作都有两种状态要处理！

## 进行中的承诺:加载状态

当 ping 本地机器上的 API 时(例如，用 Netlify Dev )，获得快速响应是很常见的。然而，这忽略了一个事实，即在现实世界中，尤其是移动环境中，API 延迟可能要慢得多。当按钮被点击时，promise 触发，但是在 UI 中根本没有视觉反馈来告诉用户点击已经被注册并且数据正在传输。因此用户经常再次点击，以防他们误点击，并产生更多的 API 请求。这是一种糟糕的用户体验，没有理由这样编写点击处理程序，除非这是默认的。

通过提供某种形式的加载状态，您可以使您的应用程序响应更快(更少令人沮丧):

```
function App() {
  const [msg, setMsg] = React.useState('click the button')
  const [loading, setLoading] = React.useState(false)
  const handler = () => {
    setLoading(true)
    fetch('https://myapi.com/')
      .then((x) => x.json())
      .then(({ msg }) => setMsg(msg))
      .finally(() => setLoading(false))
  }

  return (
    <div className="App">
      <header className="App-header">
        <p>message: {msg}</p>
        {loading && <pre>loading...</pre>}
        <button onClick={handler} disabled={loading}>
          click meeee
        </button>
      </header>
    </div>
  )
} 
```

我们现在在应用程序中为每个异步操作处理三种状态:结果、加载和错误状态！好极了。

## 承诺是愚蠢的:组件的状态

承诺一旦兑现，就不能取消。这在当时是一个有争议的决定，虽然存在像 T2 这样的平台特定的解决方法，但很明显我们永远不会在语言本身中得到可取消的承诺。当我们做出承诺，然后不再需要它们时，这就会导致问题，例如，当它应该更新的组件已经卸载时(因为用户已经导航到其他地方)。

在 React 中，这将导致一个开发专用的错误，如:

```
Warning: Can only update a mounted or mounting component. This usually means you called setState, replaceState, or forceUpdate on an unmounted component. This is a no-op.

# or

Warning: Can’t call setState (or forceUpdate) on an unmounted component. This is a no-op, but it indicates a memory leak in your application. To fix, cancel all subscriptions and asynchronous tasks in the componentWillUnmount method. 
```

您可以通过跟踪组件的安装状态来避免这种内存泄漏:

```
function App() {
  const [msg, setMsg] = React.useState('click the button')
  const isMounted = React.useRef(true)
  const handler = () => {
    setLoading(true)
    fetch('https://myapi.com/')
      .then((x) => x.json())
      .then(({ msg }) => {
        if (isMounted.current) {
          setMsg(msg)
        }
      })
  }
  React.useEffect(() => {
    return () => (isMounted.current = false)
  })

  return (
    <div className="App">
      <header className="App-header">
        <p>message: {msg}</p>
        <button onClick={handler}>click meeee</button>
      </header>
    </div>
  )
} 
```

我们在这里使用了一个 Ref，因为[更接近于实例变量](https://medium.com/@pshrmn/react-hook-gotchas-e6ca52f49328)的心理模型，但是如果你用`useState`代替，你不会注意到太多的区别。

长期使用 React 的用户还会记得 [isMounted 是一个反模式](https://reactjs.org/blog/2015/12/16/ismounted-antipattern.html)，但是如果不使用可取消承诺，仍然建议将`_isMounted`作为一个实例变量进行跟踪。(仅此而已。的。时间。)

对于那些保持计数的人来说，我们现在有**四个**状态需要为组件中的单个异步操作进行跟踪。

## 解决方法:包起来就好

现在问题应该很清楚了:

在一个简单的演示中，“赤裸裸的”承诺工作良好。

在生产环境中，您需要实现所有这些错误处理、加载和安装跟踪器状态。又来了。再一次。再一次。

听起来像是一个使用图书馆的好地方，不是吗？

幸运的是，相当多的存在。

`react-async`的`useAsync`钩子让你通过一个`promiseFn`，以及几个方便的[选项](https://www.npmjs.com/package/react-async#options)来添加回调和其他高级用例:

```
import { useAsync } from 'react-async'

const loadCustomer = async ({ customerId }, { signal }) => {
  const res = await fetch(`/api/customers/${customerId}`, { signal })
  if (!res.ok) throw new Error(res)
  return res.json()
}

const MyComponent = () => {
  const { data, error, isLoading } = useAsync({ promiseFn: loadCustomer, customerId: 1 })
  if (isLoading) return 'Loading...'
  if (error) return `Something went wrong: ${error.message}`
  if (data)
    return (
      <div>
        <strong>Loaded some data:</strong>
        <pre>{JSON.stringify(data, null, 2)}</pre>
      </div>
    )
  return null
} 
```

它还包括一个方便的`useFetch`钩子，您可以用它来代替原生的`fetch`实现。

`react-use`还为[提供了一个简单的`useAsync`实现](https://github.com/streamich/react-use/blob/master/docs/useAsync.md)，在这里你只需传递一个承诺(又名`async`函数):

```
import { useAsync } from 'react-use'

const Demo = ({ url }) => {
  const state = useAsync(async () => {
    const response = await fetch(url)
    const result = await response.text()
    return result
  }, [url])

  return (
    <div>
      {state.loading ? (
        <div>Loading...</div>
      ) : state.error ? (
        <div>Error: {state.error.message}</div>
      ) : (
        <div>Value: {state.value}</div>
      )}
    </div>
  )
} 
```

最后，加藤大世的 [`react-hooks-async`](https://github.com/dai-shi/react-hooks-async) 也为任何承诺提供了一个非常好的`abort`控制器:

```
import React from 'react'

import { useFetch } from 'react-hooks-async'

const UserInfo = ({ id }) => {
  const url = `https://reqres.in/api/users/${id}?delay=1`
  const { pending, error, result, abort } = useFetch(url)
  if (pending)
    return (
      <div>
        Loading...<button onClick={abort}>Abort</button>
      </div>
    )
  if (error)
    return (
      <div>
        Error: {error.name} {error.message}
      </div>
    )
  if (!result) return <div>No result</div>
  return <div>First Name: {result.data.first_name}</div>
}

const App = () => (
  <div>
    <UserInfo id={'1'} />
    <UserInfo id={'2'} />
  </div>
) 
```

你也可以选择[使用可观察的事物](https://medium.com/@benlesh/promise-cancellation-is-dead-long-live-promise-cancellation-c6601f1f5082)，要么把你的承诺包装在其中，要么直接使用它们。

在任何情况下，您都可以看到这样一种紧急模式，即**您总是想要包装您的承诺**，以便在生产环境中安全地使用它们。在元级别上，这里发生的事情是 JavaScript 允许您使用完全相同的 API 调用同步和异步代码，这是一个不幸的设计约束。这意味着我们需要包装器来安全地将异步执行转换为我们关心的同步变量，尤其是在 React 这样的即时模式呈现范例中。我们不得不选择要么每次自己写这些，要么采用一个库。

如果你有我没有想到的进一步评论和边缘案例，请[联系！](https://twitter.com/swyx)