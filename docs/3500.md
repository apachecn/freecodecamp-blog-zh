# 如何使用 React 测试库和 Jest 开始测试 React 应用

> 原文：<https://www.freecodecamp.org/news/8-simple-steps-to-start-testing-react-apps-using-react-testing-library-and-jest/>

测试通常被认为是一个乏味的过程。这是您必须编写的额外代码，在某些情况下，老实说，这是不需要的。但是每个开发人员至少应该知道测试的基本知识。这增加了他们对产品的信心，对于大多数公司来说，这是一个要求。

在 React 世界中，有一个叫做`react-testing-library`的神奇库，它可以帮助你更有效地测试你的 React 应用。你用它来开玩笑。

在本文中，我们将看到像老板一样开始测试 React 应用程序的 8 个简单步骤。

*   [先决条件](#prerequisites)
*   [基础知识](#basics)
*   [什么是 React 测试库？](#what-is-react-testing-library)
*   [1。如何创建测试快照？](#1-how-to-create-a-test-snapshot)
*   [2。测试 DOM 元素](#2-testing-dom-elements)
*   [3。测试事件](#3-testing-events)
*   [4。测试异步动作](#4-testing-asynchronous-actions)
*   [5。测试 React Redux](#5-testing-react-redux)
*   [6。测试 React 上下文](#6-testing-react-context)
*   7 .[。测试 React 路由器](#7-testing-react-router)
*   [8。测试 HTTP 请求](#8-testing-http-request)
*   [最终想法](#final-thoughts)
*   [接下来的步骤](#next-steps)

## 先决条件

本教程假设您至少对 React 有基本的了解。我将只关注测试部分。

为了跟进，您必须通过在您的终端中运行来克隆项目:

```
 git clone https://github.com/ibrahima92/prep-react-testing-library-guide 
```

接下来，运行:

```
 yarn 
```

或者，如果你用 NPM:

```
npm install 
```

就是这样！现在让我们深入一些基础知识。

## 基础

一些关键的东西会在本文中大量使用，了解它们的作用可以帮助你的理解。

`it or test`:描述测试本身。它将测试名和保存测试的函数作为参数。

`expect`:测试需要通过的条件。它会将收到的参数与匹配器进行比较。

`a matcher`:应用于预期条件的函数。

`render`:用于渲染给定组件的方法。

```
import React from 'react'
import {render} from '@testing-library/react'
import App from './App'

 it('should take a snapshot', () => {
    const { asFragment } = render(<App />)

    expect(asFragment(<App />)).toMatchSnapshot()
   })
}); 
```

如你所见，我们用`it`描述测试，然后，用`render`显示 App 组件，并期望`asFragment(<App />)`匹配`toMatchSnapshot()`(由 [jest-dom](https://github.com/testing-library/jest-dom) 提供的匹配器)。

顺便说一下，`render`方法返回了几个我们可以用来测试我们特性的方法。我们还使用析构来获得方法。

也就是说，让我们在下一节继续学习更多关于 React 测试库的知识。

## 什么是 React 测试库？

React 测试库是由[肯特·c·多兹](https://twitter.com/kentcdodds)创建的一个非常轻量级的包。它是[酶](https://enzymejs.github.io/enzyme/)的替代品，并在`react-dom`和`react-dom/test-utils`之上提供光效用函数。

React 测试库是一个 DOM 测试库，这意味着它不是处理呈现的 React 组件的实例，而是处理 DOM 元素以及它们在真实用户面前的行为。

这是一个很棒的库，开始使用(相对)容易，并且它鼓励良好的测试实践。注意——你也可以不开玩笑地使用它。

"你的测试越像你的软件被使用的方式，它们就能给你越多的信心."

所以，让我们在下一节开始使用它。顺便说一下，您不需要安装任何包，因为`create-react-app`附带了库及其依赖项。

## 1.如何创建测试快照

顾名思义，快照允许我们保存给定组件的快照。当你更新或者做一些重构，想要获取或者比较变化的时候，它很有帮助。

现在，让我们来拍一张`App.js`文件的快照。

*   `App.test.js`

```
import React from 'react'
import {render, cleanup} from '@testing-library/react'
import App from './App'

 afterEach(cleanup)

 it('should take a snapshot', () => {
    const { asFragment } = render(<App />)

    expect(asFragment(<App />)).toMatchSnapshot()
   })
}); 
```

要拍摄快照，我们首先必须导入`render`和`cleanup`。这两种方法将在本文中大量使用。

`render`，正如你可能猜到的，帮助渲染一个 React 组件。并且`cleanup`被作为参数传递给`afterEach`,以便在每次测试后清理所有东西，避免内存泄漏。

接下来，我们可以使用`render`呈现 App 组件，并从方法中获取作为返回值的`asFragment`。最后，确保应用程序组件的片段与快照匹配。

现在，要运行测试，请打开您的终端并导航到项目的根目录，然后运行以下命令:

```
 yarn test 
```

或者，如果您使用 npm:

```
 npm test 
```

因此，它将在`src`中创建一个新文件夹`__snapshots__`和一个文件`App.test.js.snap`，如下所示:

*   `App.test.js.snap`

```
// Jest Snapshot v1, https://goo.gl/fbAQLP

exports[`Take a snapshot should take a snapshot 1`] = `
<DocumentFragment>
  <div class="App">
    <h1>Testing</h1>
  </div>
</DocumentFragment>
`; 
```

如果您在`App.js`中再做一次更改，测试将会失败，因为快照将不再符合条件。要使其通过，只需按下`u`即可更新。你会在`App.test.js.snap`看到更新的快照。

现在，让我们继续前进，开始测试我们的元素。

## 2.测试 DOM 元素

为了测试我们的 DOM 元素，我们首先要看一下`TestElements.js`文件。

*   `TestElements.js`

```
import React from 'react'

const TestElements = () => {
 const [counter, setCounter] = React.useState(0)

 return (
  <>
    <h1 data-testid="counter">{ counter }</h1>
    <button data-testid="button-up" onClick={() => setCounter(counter + 1)}> Up</button>
    <button disabled data-testid="button-down" onClick={() => setCounter(counter - 1)}>Down</button>
 </>
    )
  }

export default TestElements 
```

在这里，你唯一要保留的就是`data-testid`。它将用于从测试文件中选择这些元素。现在，让我们编写单元测试:

测试计数器是否等于 0:

`TestElements.test.js`

```
import React from 'react';
import { render, cleanup } from '@testing-library/react';
import TestElements from './TestElements'

afterEach(cleanup);

  it('should equal to 0', () => {
    const { getByTestId } = render(<TestElements />); 
    expect(getByTestId('counter')).toHaveTextContent(0)
   }); 
```

如您所见，语法与之前的测试非常相似。唯一的区别是我们使用`getByTestId`来选择必要的元素(记住`data-testid`)并检查它是否通过了测试。换句话说，我们检查文本内容`<h1 data-testid="counter">{ counter }</h1>`是否等于 0。

测试按钮是启用还是禁用:

`TestElements.test.js`(将以下代码块添加到文件中)

```
 it('should be enabled', () => {
    const { getByTestId } = render(<TestElements />);
    expect(getByTestId('button-up')).not.toHaveAttribute('disabled')
  });

  it('should be disabled', () => {
    const { getByTestId } = render(<TestElements />); 
    expect(getByTestId('button-down')).toBeDisabled()
  }); 
```

这里，像往常一样，我们使用`getByTestId`来选择元素，并检查按钮是否有`disabled`属性。第二个问题是，按钮是否被禁用。

如果您保存文件或者在您的终端`yarn test`中再次运行，测试将通过。

*祝贺你！你的第一次测试已经通过了！*

![congrats](img/954bafe6a92a841f986bb268bd99a2b7.png)

现在，让我们在下一节学习如何测试一个事件。

## 3.测试事件

在编写单元测试之前，让我们先看看`TestEvents.js`是什么样子的。

*   `TestEvents.js`

```
import React from 'react'

const TestEvents = () => {
  const [counter, setCounter] = React.useState(0)

return (
  <>
    <h1 data-testid="counter">{ counter }</h1>
    <button data-testid="button-up" onClick={() => setCounter(counter + 1)}> Up</button>
    <button data-testid="button-down" onClick={() => setCounter(counter - 1)}>Down</button>
 </>
    )
  }

  export default TestEvents 
```

现在，让我们编写测试。

测试当我们点击按钮时，计数器是否正确递增和递减:

`TestEvents.test.js`

```
import React from 'react';
import { render, cleanup, fireEvent } from '@testing-library/react';
import TestEvents from './TestEvents'

  afterEach(cleanup);

  it('increments counter', () => {
    const { getByTestId } = render(<TestEvents />); 

    fireEvent.click(getByTestId('button-up'))

    expect(getByTestId('counter')).toHaveTextContent('1')
  });

  it('decrements counter', () => {
    const { getByTestId } = render(<TestEvents />); 

    fireEvent.click(getByTestId('button-down'))

    expect(getByTestId('counter')).toHaveTextContent('-1')
  }); 
```

如您所见，除了预期的文本内容之外，这两个测试非常相似。

第一个测试用`fireEvent.click()`触发一个 click 事件，检查当按钮被单击时计数器是否增加到 1。

第二个检查当按钮被点击时计数器是否减少到-1。

`fireEvent`有几种方法可以用来测试事件，所以请随意阅读文档以了解更多信息。

现在我们知道了如何测试事件，让我们继续，在下一节学习如何处理异步动作。

## 4.测试异步操作

异步操作需要花费时间来完成。它可以是 HTTP 请求、计时器等等。

现在，让我们检查一下`TestAsync.js`文件。

*   `TestAsync.js`

```
import React from 'react'

const TestAsync = () => {
  const [counter, setCounter] = React.useState(0)

  const delayCount = () => (
    setTimeout(() => {
      setCounter(counter + 1)
    }, 500)
  )

return (
  <>
    <h1 data-testid="counter">{ counter }</h1>
    <button data-testid="button-up" onClick={delayCount}> Up</button>
    <button data-testid="button-down" onClick={() => setCounter(counter - 1)}>Down</button>
 </>
    )
  }

  export default TestAsync 
```

这里，我们使用`setTimeout()`将递增事件延迟 0.5s。

测试计数器是否在 0.5 秒后增加:

`TestAsync.test.js`

```
import React from 'react';
import { render, cleanup, fireEvent, waitForElement } from '@testing-library/react';
import TestAsync from './TestAsync'

afterEach(cleanup);

  it('increments counter after 0.5s', async () => {
    const { getByTestId, getByText } = render(<TestAsync />); 

    fireEvent.click(getByTestId('button-up'))

    const counter = await waitForElement(() => getByText('1')) 

    expect(counter).toHaveTextContent('1')
  }); 
```

为了测试递增事件，我们首先必须使用 async/await 来处理这个动作，因为正如我前面所说的，它需要时间来完成。

接下来，我们使用一个新的助手方法`getByText()`。这与`getByTestId()`类似，除了`getByText()`选择文本内容而不是 id 或 data-testid。

现在，点击按钮后，我们等待计数器增加`waitForElement(() => getByText('1'))`。一旦计数器增加到 1，我们现在可以移动到条件，并检查计数器是否有效地等于 1。

话虽如此，现在让我们转向更复杂的测试用例。

你准备好了吗？

![ready](img/3ff5a832e8de54ec37e515bc369f97f9.png)

## 5.测试反应冗余

如果你是 React Redux 的新手，[这篇文章](https://www.ibrahima-ndaw.com/blog/7-steps-to-understand-react-redux/)可能会帮到你。否则，我们来看看`TestRedux.js`是什么样子的。

*   `TestRedux.js`

```
import React from 'react'
import { connect } from 'react-redux'

const TestRedux = ({counter, dispatch}) => {

 const increment = () => dispatch({ type: 'INCREMENT' })
 const decrement = () => dispatch({ type: 'DECREMENT' })

 return (
  <>
    <h1 data-testid="counter">{ counter }</h1>
    <button data-testid="button-up" onClick={increment}>Up</button>
    <button data-testid="button-down" onClick={decrement}>Down</button>
 </>
    )
  }

export default connect(state => ({ counter: state.count }))(TestRedux) 
```

对于减速器:

*   `store/reducer.js`

```
export const initialState = {
    count: 0,
  }

  export function reducer(state = initialState, action) {
    switch (action.type) {
      case 'INCREMENT':
        return {
          count: state.count + 1,
        }
      case 'DECREMENT':
        return {
          count: state.count - 1,
        }
      default:
        return state
    }
  } 
```

正如你所看到的，没有什么特别的——它只是 React Redux 处理的一个基本的计数器组件。

现在，让我们编写单元测试。

测试初始状态是否等于 0:

`TestRedux.test.js`

```
import React from 'react'
import { createStore } from 'redux'
import { Provider } from 'react-redux'
import { render, cleanup, fireEvent } from '@testing-library/react';
import { initialState, reducer } from '../store/reducer'
import TestRedux from './TestRedux'

const renderWithRedux = (
  component,
  { initialState, store = createStore(reducer, initialState) } = {}
) => {
  return {
    ...render(<Provider store={store}>{component}</Provider>),
    store,
  }
}

 afterEach(cleanup);

it('checks initial state is equal to 0', () => {
    const { getByTestId } = renderWithRedux(<TestRedux />)
    expect(getByTestId('counter')).toHaveTextContent('0')
  }) 
```

我们需要导入一些东西来测试 React Redux。在这里，我们创建自己的助手函数`renderWithRedux()`来呈现组件，因为它将被多次使用。

`renderWithRedux()`接收要呈现的组件、初始状态和存储作为参数。如果没有存储，它将创建一个新的，如果没有收到初始状态或存储，它将返回一个空对象。

接下来，我们使用`render()`来呈现组件，并将存储传递给提供者。

也就是说，我们现在可以将组件`TestRedux`传递给`renderWithRedux()`来测试计数器是否等于`0`。

测试计数器是否正确递增和递减:

`TestRedux.test.js`(将以下代码块添加到文件中)

```
it('increments the counter through redux', () => {
  const { getByTestId } = renderWithRedux(<TestRedux />, 
    {initialState: {count: 5}
})
  fireEvent.click(getByTestId('button-up'))
  expect(getByTestId('counter')).toHaveTextContent('6')
})

it('decrements the counter through redux', () => {
  const { getByTestId} = renderWithRedux(<TestRedux />, {
    initialState: { count: 100 },
  })
  fireEvent.click(getByTestId('button-down'))
  expect(getByTestId('counter')).toHaveTextContent('99')
}) 
```

为了测试递增和递减事件，我们将初始状态作为第二个参数传递给`renderWithRedux()`。现在，我们可以单击按钮，测试预期结果是否与条件匹配。

现在，让我们转到下一节，介绍 React 上下文。

React 路由器和 Axios 将紧随其后，你还明白吗？

![of-course](img/2a583ff8c281ce568b44248ac56d5105.png)

## 6.测试反应上下文

如果你是 React Context 的新手，先看看[这篇文章](https://www.ibrahima-ndaw.com/blog/redux-vs-react-context-which-one-should-you-choose/)。否则，我们检查一下`TextContext.js`文件。

*   `TextContext.js`

```
import React from "react"

export const CounterContext = React.createContext()

const CounterProvider = () => {
  const [counter, setCounter] = React.useState(0)
  const increment = () => setCounter(counter + 1)
  const decrement = () => setCounter(counter - 1)

  return (
    <CounterContext.Provider value={{ counter, increment, decrement }}>
      <Counter />
    </CounterContext.Provider>
  )
}

export const Counter = () => {  
    const { counter, increment, decrement } = React.useContext(CounterContext)   
    return (
     <>
       <h1 data-testid="counter">{ counter }</h1>
       <button data-testid="button-up" onClick={increment}> Up</button>
       <button data-testid="button-down" onClick={decrement}>Down</button>
    </>
       )
}

export default CounterProvider 
```

现在，计数器状态是通过 React 上下文管理的。让我们编写单元测试来检查它是否如预期的那样运行。

测试初始状态是否等于 0:

`TextContext.test.js`

```
import React from 'react'
import { render, cleanup,  fireEvent } from '@testing-library/react'
import CounterProvider, { CounterContext, Counter } from './TestContext'

const renderWithContext = (
  component) => {
  return {
    ...render(
        <CounterProvider value={CounterContext}>
            {component}
        </CounterProvider>)
  }
}

afterEach(cleanup);

it('checks if initial state is equal to 0', () => {
    const { getByTestId } = renderWithContext(<Counter />)
    expect(getByTestId('counter')).toHaveTextContent('0')
}) 
```

与前面的 React Redux 部分一样，这里我们使用相同的方法，通过创建一个助手函数`renderWithContext()`来呈现组件。但是这一次，它只接收组件作为参数。为了创建新的上下文，我们将`CounterContext`传递给提供者。

现在，我们可以测试计数器最初是否等于 0。

测试计数器是否正确递增和递减:

`TextContext.test.js`(将以下代码块添加到文件中)

```
 it('increments the counter', () => {
    const { getByTestId } = renderWithContext(<Counter />)

    fireEvent.click(getByTestId('button-up'))
    expect(getByTestId('counter')).toHaveTextContent('1')
  })

  it('decrements the counter', () => {
    const { getByTestId} = renderWithContext(<Counter />)

    fireEvent.click(getByTestId('button-down'))
    expect(getByTestId('counter')).toHaveTextContent('-1')
  }) 
```

如您所见，这里我们触发了一个 click 事件来测试计数器是否正确地递增到 1 和递减到-1。

也就是说，我们现在可以进入下一部分，介绍 React 路由器。

## 7.测试 React 路由器

如果你想深入 React 路由器，[这篇文章](https://www.ibrahima-ndaw.com/blog/the-complete-guide-to-react-router/)也许能帮到你。否则，我们检查一下`TestRouter.js`文件。

*   `TestRouter.js`

```
import React from 'react'
import { Link, Route, Switch,  useParams } from 'react-router-dom'

const About = () => <h1>About page</h1>

const Home = () => <h1>Home page</h1>

const Contact = () => {
  const { name } = useParams()
  return <h1 data-testid="contact-name">{name}</h1>
}

const TestRouter = () => {
    const name = 'John Doe'
    return (
    <>
    <nav data-testid="navbar">
      <Link data-testid="home-link" to="/">Home</Link>
      <Link data-testid="about-link" to="/about">About</Link>
      <Link data-testid="contact-link" to={`/contact/${name}`}>Contact</Link>
    </nav>

      <Switch>
        <Route exact path="/" component={Home} />
        <Route path="/about" component={About} />
        <Route path="/about:name" component={Contact} />
      </Switch>
    </>
  )
}

export default TestRouter 
```

这里，当导航主页时，我们有一些组件要呈现。

现在，让我们编写测试:

*   `TestRouter.test.js`

```
import React from 'react'
import { Router } from 'react-router-dom'
import { render, fireEvent } from '@testing-library/react'
import { createMemoryHistory } from 'history'
import TestRouter from './TestRouter'

const renderWithRouter = (component) => {
    const history = createMemoryHistory()
    return { 
    ...render (
    <Router history={history}>
        {component}
    </Router>
    )
  }
}

it('should render the home page', () => {

  const { container, getByTestId } = renderWithRouter(<TestRouter />) 
  const navbar = getByTestId('navbar')
  const link = getByTestId('home-link')

  expect(container.innerHTML).toMatch('Home page')
  expect(navbar).toContainElement(link)
}) 
```

要测试 React 路由器，我们首先要有一个导航历史。因此，我们使用`createMemoryHistory()`以及猜测的名称来创建导航历史。

接下来，我们使用助手函数`renderWithRouter()`来呈现组件，并将`history`传递给`Router`组件。这样，我们现在可以测试开始时加载的页面是否是主页。以及导航栏是否加载了预期的链接。

测试当我们点击链接时，它是否导航到带有参数的其他页面:

`TestRouter.test.js`(将以下代码块添加到文件中)

```
it('should navigate to the about page', ()=> {
  const { container, getByTestId } = renderWithRouter(<TestRouter />) 

  fireEvent.click(getByTestId('about-link'))

  expect(container.innerHTML).toMatch('About page')
})

it('should navigate to the contact page with the params', ()=> {
  const { container, getByTestId } = renderWithRouter(<TestRouter />) 

  fireEvent.click(getByTestId('contact-link'))

  expect(container.innerHTML).toMatch('John Doe')
}) 
```

现在，为了检查导航是否工作，我们必须在导航链接上触发一个 click 事件。

对于第一个测试，我们检查内容是否等于 About 页面中的文本，对于第二个测试，我们测试路由参数并检查它是否正确通过。

我们现在可以进入最后一部分，学习如何测试 Axios 请求。

我们快完成了！

![still-here](img/b167badacc64f27c1f7ab99b3e05d6c3.png)

## 8.测试 HTTP 请求

像往常一样，我们先来看看`TextAxios.js`文件是什么样子的。

*   `TextAxios.js`

```
import React from 'react'
import axios from 'axios'

const TestAxios = ({ url }) => {
  const [data, setData] = React.useState()

  const fetchData = async () => {
    const response = await axios.get(url)
    setData(response.data.greeting)    
 }     

 return (
  <>
    <button onClick={fetchData} data-testid="fetch-data">Load Data</button>
    { 
    data ?
    <div data-testid="show-data">{data}</div>:
    <h1 data-testid="loading">Loading...</h1>
    }
  </>
     )
}

export default TestAxios 
```

正如您在这里看到的，我们有一个简单的组件，它有一个按钮来发出请求。如果数据不可用，它将显示一条加载消息。

现在，让我们编写测试。

测试数据获取和显示是否正确:

`TextAxios.test.js`

```
import React from 'react'
import { render, waitForElement, fireEvent } from '@testing-library/react'
import axiosMock from 'axios'
import TestAxios from './TestAxios'

jest.mock('axios')

it('should display a loading text', () => {

 const { getByTestId } = render(<TestAxios />)

  expect(getByTestId('loading')).toHaveTextContent('Loading...')
})

it('should load and display the data', async () => {
  const url = '/greeting'
  const { getByTestId } = render(<TestAxios url={url} />)

  axiosMock.get.mockResolvedValueOnce({
    data: { greeting: 'hello there' },
  })

  fireEvent.click(getByTestId('fetch-data'))

  const greetingData = await waitForElement(() => getByTestId('show-data'))

  expect(axiosMock.get).toHaveBeenCalledTimes(1)
  expect(axiosMock.get).toHaveBeenCalledWith(url)
  expect(greetingData).toHaveTextContent('hello there')
}) 
```

这个测试用例有点不同，因为我们必须处理一个 HTTP 请求。为此，我们必须在`jest.mock('axios')`的帮助下模拟 axios 请求。

现在，我们可以使用`axiosMock`并对其应用一个`get()`方法。最后，我们将使用 Jest 函数`mockResolvedValueOnce()`来传递模拟数据作为参数。

现在，对于第二个测试，我们可以单击按钮来获取数据，并使用 async/await 来解析它。现在我们要测试三样东西:

1.  如果 HTTP 请求已经正确完成
2.  如果 HTTP 请求已经用`url`完成
3.  如果获取的数据符合预期。

对于第一个测试，我们只是检查在没有数据显示时是否显示了加载消息。

也就是说，我们现在已经完成了开始测试 React 应用程序的 8 个简单步骤。

不要再害怕考试了。

![not-scared](img/3068407ec4d2dcf6a7687640b5350761.png)

## 最后的想法

React 测试库是测试 React 应用程序的一个很棒的包。它让我们能够访问`jest-dom`匹配器，我们可以用它来更有效地测试我们的组件，并采用良好的实践。希望这篇文章是有用的，它将帮助您在未来构建健壮的 React 应用程序。

你可以在这里找到完成的项目

感谢阅读！

[阅读更多文章](https://www.ibrahima-ndaw.com/) - [订阅我的简讯](https://ibrahima-ndaw.us5.list-manage.com/subscribe?u=8dedf5d07c7326802dd81a866&id=5d7bcd5b75) - [在 twitter 上关注我](https://twitter.com/ibrahima92_)

你可以在我的博客上阅读其他类似的文章。

## 后续步骤

[React 测试库文件](https://testing-library.com/docs/react-testing-library/intro)

[React 测试库备忘单](https://testing-library.com/docs/react-testing-library/cheatsheet)

[有一个拳手的房子名叫](https://github.com/testing-library/jest-dom)

[是 Docs](https://jestjs.io/docs/en/getting-started.html) 的意思