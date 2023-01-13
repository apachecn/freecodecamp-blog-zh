# 如何使用 Redux 进行应用程序范围的状态管理

> 原文：<https://www.freecodecamp.org/news/how-to-use-redux-for-application-wide-state-management/>

让我们面对现实吧——跨多个组件的状态管理并不容易。

有时，我们可能正确地设置了状态或逻辑处理，但却无法使用状态。或者，我们可能让一切正常工作，但在这个过程中弄乱了代码库，使其难以阅读、适应和扩展。

拥有单一真实来源(单一存储)的能力使 Redux 比传统的上下文 API 更胜一筹。

因此，不再赘言，让我们看看如何通过编写更干净、更优化的代码，来充分利用 Redux 进行应用程序范围的状态管理。

## 开始之前

本文主要着眼于改进现有 Redux 存储的不同方法。因此，您需要了解 Redux 的基础知识，比如设置 reducers、Redux 存储和分派动作。

如果你需要复习 Redux，看看这些有用的指南:

*   [适合初学者的 Redux】](https://www.freecodecamp.org/news/redux-for-beginners/)
*   大脑友好的学习指南 Redux

已经了解了 Redux 的基础知识，并且有一个想要改进的 Redux 商店？很好。

以下是我们将在本文中涉及的内容:

*   快速回顾如何使用 Redux 商店
*   如何通过为电子商务应用程序创建 Redux store 来使用 Redux Toolkit 库。注意:我们不会构建整个应用程序，而只是它的状态管理部分
*   如何使用动作创建器处理 reduces 和组件之外的异步代码

就这样，让我们开始吧。

## 快速回顾一下 Redux 商店

在研究如何改进 Redux 代码之前，让我们先简单了解一下到目前为止我们是如何使用 Redux 的。

这是一个 Redux 商店的基本例子。这是一个简单的计数器应用程序，可以增加和减少按钮点击的计数，计数器的状态由 Redux 管理和处理:

```
import { createStore } from 'redux'

const initialState = {
  counter: 0,
}

const reducer = (state = initialState, action) => { // Reducer function
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, counter: state.counter + 1 }
    case 'DECREMENT':
      return { ...state, counter: state.counter - 1 }
    default:
      return state
  }
}

const store = createStore(reducer) // redux-store

export default store 
```

redux-store.js

```
import { useDispatch, useSelector } from 'react-redux'

export default function Component(props) {
  const dispatch = useDispatch()
  const counter = useSelector((state) => state.counter)

  const incrementHandler = () => {
    dispatch({ type: 'INCREMENT' })
  }
  const decrementHandler = () => {
    dispatch({ type: 'DECREMENT' })
  }

  return (
    <div>
      <h1>Counter:{counter}</h1>
      <button onClick={incrementHandler}>Increment</button>
      <button onClick={decrementHandler}>Decrement</button>
    </div>
  )
} 
```

Component.jsx

这是一个非常简单的例子，你可以使用状态钩子来创建。但是它很好地概述了我们通常如何使用 Redux。

如果没有太多不同类型的逻辑需要处理或分派，上面的 Redux 存储非常好。

另外，请注意，我使用了`const initialState`而不是`var`或`let`，尽管由于不变性的特性，我们正在更新状态。也就是说，我们并没有真正改变现有的状态，而是用更新后的值重新构建一个新的状态。

这很好。但是，如果应用程序的复杂性增加，需要我们执行多种状态和逻辑处理，该怎么办呢？

这正是我们将在本指南中讨论的内容。

## 如何用 Redux Toolkit 创建 Redux 商店

在本文的其余部分，让我们考虑如何跨电子商务应用程序管理状态。在一个电子商务应用程序中，我们会希望跟踪和广播各种组件的多个状态。

作为一个经验法则，在动手之前，对我们将如何实现一个设计有一个粗略的想法总是好的。

当我们考虑电子商务应用程序时，可能的重要状态可能如下:

1.  证明
2.  购物车跟踪
3.  观察列表

我们可以将状态处理逻辑和状态划分到不同的模块中，然后将其全部合并到一个存储中。

为此，我们将使用 Redux 工具包。

Redux Toolkit 是一个类似 Redux 的第三方库，由 Redux 团队创建和维护。

您可能想知道为什么 Redux 团队首先创建了 Redux Toolkit。答案就在[文档](https://redux-toolkit.js.org/introduction/getting-started)的开头:

> Redux 工具包旨在成为编写 Redux 逻辑的标准方法。最初创建它是为了帮助解决关于 Redux 的三个常见问题:
> 
> ——“配置 Redux 存储太复杂”
> ——“我必须添加许多包才能让 Redux 做任何有用的事情”
> ——“Redux 需要太多样板代码”

总而言之，使用 Redux Toolkit 的全部目的是使 Redux 的使用更简单、更高效。

### 如何安装 Redux 工具包

您可以使用`npm`或`yarn`安装 Redux Toolkit，如下所示:

*   `npm install @reduxjs/toolkit`
*   `yard add @reduxjs/toolkit`

## 如何使用 Redux 工具包

创建 Redux 存储的第一步是设置状态处理。

我们将使用 Redux Toolkit 提供的`createSlice`,而不是使用 reducers。

`createSlice`接受 3 个强制参数，它们是:

1.  **名称**:切片的名称
2.  **initialState** :切片的初始状态(类似于减速器的初始状态)
3.  **减速器**:影响状态(动作)的功能

简而言之，一个切片就像一个模块化的国家及其行动捆绑器。

让我们首先为身份认证及其相关操作创建一个存储片:

```
const authSlice = createSlice({
  name: 'Auth', // Could name it anything
  initialState: {
    isLoggedIn: false,
  },
  reducers: {
    login: (state) => {
      state.isLoggedIn = true
    },
    logout: (state) => {
      state.isLoggedIn = false
    },
  },
}) 
```

auth-slice.js

如您所见，上面的部分专用于用户认证。`initialState`是一个对象，包含不同的初始状态值，以及还原器需要作用的状态。reducer 中的函数就像常规的 reducer 一样，接受状态和动作作为参数。

上面代码片段中一个值得注意的地方是我们如何处理用`state.isLoggedIn = true`更新状态。这显然是在改变状态，违反了不变性的属性。

或者，是吗？

在妄下结论之前，让我们先了解一下发生了什么。

从表面上看，我们似乎在改变现有的状态。但是改变现有状态不会触发向订阅者广播更新的值。

所以当我们在 reduces 中使用变异语法时，`createSlice`在内部使用一个名为 [Immer](https://redux-toolkit.js.org/usage/immer-reducers) 的库。这个库描述了现有状态和更新状态之间的差异，并用更新后的值重新构造一个新对象。

这意味着，当我们改变状态时，Immer 负责构建一个新的对象，并符合不变性的属性，使我们更容易编写代码。

现在，让我们为其他州写切片:

```
const cartSlice = createSlice({
  name: 'Cart',
  initialState: {
    cart: [],
    qty: 0,
    total: 0,
  },
  reducers: {
    addItem: (state, action) => {
      state.cart.push(action.payload.cartItem)
      state.qty += action.payload.cartItem.qty
      state.total += action.payload.cartItem.price * action.payload.cartItem.qty
    },
    removeItem: (state, action) => {
      const itemIndex = state.cart.findIndex(
        (obj) => obj.id === action.payload.id
      )
      if (itemIndex !== -1 && state.cart[itemIndex].qty >= 1) {
        state.cart[itemIndex].qty -= 1
      }
    }
  }
}) 
```

cart-slice.js

这里的主要部分是与从购物车中添加和删除商品相关的逻辑。

对于观察列表，我们遵循类似的逻辑，但是没有总数量和总成本:

```
const watchlistSlice = createSlice({
  name: 'Watchlist',
  initialState: {
    watchlist: [],
  },
  reducers: {
    addItem: (state, action) => {
      state.watchlist.push(action.payload.watchlistItem)
    },
    removeItem: (state, action) => {
      state.watchlist.filter((item) => item.id !== action.payload.id)
    }
  }
}) 
```

watchlist-slice.js

请注意操作项是如何通过 payload 属性访问的，而不是直接访问的。

这是因为 Redux Toolkit 在默认情况下为动作添加了`payload`属性。`payload`是一个可以进一步包含其他嵌套对象的对象，或者简单的键值对。它主要保存传递给动作调度程序的参数。

现在你可能会问自己，我们实际上要如何分派动作？

在使用 Redux Toolkit 之前，我们通常基于动作`type`来执行动作。然后根据`type`我们将执行不同的操作。

但是这里我们没有使用任何`type`属性，因为我们将把 reducer 函数导出为动作，然后可以在 dispatcher 中调用这些动作。

通过调用创建的切片上的`actions`属性，可以将 Reducer 函数导出为动作:

`export const authActions = authSlice.actions`

`export const cartActions = cartSlice.actions`

`export const wathclistActions = watchlistSlice.actions`

这里的所有切片彼此之间并不直接相关，所以将它们放在单独的文件中是安全的，也是一种好的做法。

如果您将切片添加到不同的文件，请确保也将其导出。

### 如何将切片合并到一个商店中

一个存储只能有一个 reducer，因此将所有片及其 reducer 组合成一个 reducer 而不丢失其身份和功能是很重要的。

为此，我们将使用`configureStore`到`createStore`。

`configureStore`类似于`createStore`，但是可以将多个切片的减速器合并成一个减速器。

它有一个接受一个或多个切片的`reducer`对象，如下所示:

```
import { authSlice } from './auth-slice'
import { cartSlice } from './cart-slice'
import { wathclistSlice } from './watchlist-slice'

const store = configureStore({
  reducer: {
    authSliceReducer: authSlice.reducer,
    cartSliceReducer: cartSlice.reducer,
    watchlistSliceReducer: watchlist.reducer,
  },
})

export defualt store
```

这将设置存储供应用程序中的多个组件使用。

## 如何在组件中使用 Redux 状态

现在我们已经准备好了 Redux 存储，我们可以使用`useSelector`和`useDispatch`钩子从组件中消费和分派动作。

这里有一个简单的例子:

```
import { useDispatch, useSelector } from 'react-redux'
import { cartActions } from '...'

export default function Cart(props) {
  const dispatch = useDispatch()
  const selector = useSelector((state) => state.watchlistSliceReducer) // Since the store has multiple reducers, we need to drill into the appropriate slice state.

  const addToCartHandler = () => {
    const dummyitem = {
      id: Math.random(),
      name: `Dummy Item ${Math.random()}`,
      price: 20 * Math.random(),
    }
    dispatch(cartActions.addItem(cartItem.dummyitem))
  }
  const removeFromCartHandler = (id) => {
    dispatch(cartActions.removeItem(id)) //Passing id as an argument to the reducer function.
  }

  return (
    <div>
      {selector.cart.length &&
        selector.cart.map((item) => {
          return (
            <div>
              <p>Name: {item.name}</p>
              <p>Price: {item.price}</p>
              <p>Quantity: {item.qty}</p>
              <button onClick={removeFromCartHandler}>Remove item</button>
            </div>
          )
        })}
      <h3>Total cart value:{selector.cart.total}</h3>
      <button onClick={addToCartHandler}>Add dummy item</button>
    </div>
  )
} 
```

Component.jsx

## 如何使用动作创建器处理异步代码

到目前为止，一切顺利。

但是等等，还有一个重要的话题我们没有涉及到——如何用 Redux 处理副作用或者异步代码。

考虑一个场景，您想要分派一个动作，该动作需要处理产生副作用的代码块。但同时，还原剂应该是纯净的，无副作用的，同步的。

这意味着在 reducers 中添加任何会产生副作用的代码都违背了核心 Redux 原则，而且非常糟糕。

为了处理这样的实例，我们有两个选择:要么使用`useEffect` / `componentDidMount`，要么通过编写动作创建器。

### 如何用`useEffect`或`componentDidMount`处理副作用

处理产生副作用的代码的一种方法是使用`useEffect`。通过这样做，我们将产生副作用的代码从分派的动作本身中分离出来，因此 reducers 保持纯净和同步。

但是，使用`useEffect`的一个主要缺点是代码冗余和重复。

如果有两个或更多的组件产生相同的副作用，我们会希望在这些组件的`useEffect`钩子中运行相同的逻辑，这是一个糟糕的做法。

一个快速的解决方法是将逻辑放入一个 helper 函数中，并在根组件上运行这个函数，让所有其他组件通过 Redux state 监听这些更改。

这是允许的，也不一定是个坏习惯，但更好的方法是使用动作创建器 thunk。

## 如何处理动作创作者的副作用

thunk 基本上是一个返回另一个不被立即调用的函数的函数。事实上，当我们分派动作函数时，我们一直在不知不觉中使用动作创建器。

这是因为 Redux Toolkit 从我们这里抽象出了所有这些。实际上，这个函数返回一个 action 对象，它对应于适当的 reducer 函数。

例如，当我们这样做时:

`dispatch(actions.dispatchActions(args))`

方法`dispatchActions(...)`返回一个带有类型和有效负载属性的动作对象。粗略地说，`dispatchActions()`函数应该是这样的:

```
function dispatchActions(args) {
  return { type: 'UNIQUE', payload: { ...args } }
}
```

`type: 'UNIQUE'`是一个占位符，但是在内部，一个惟一的 ID 被分配给不同的动作调度程序，然后这些调度程序被挂接到它们各自的 reducer 函数。

所以，`dispatch(actions.dispatchActions(args))`有效的意思是，`dispatch({ type: 'UNIQUE_ID', args: args })`。这也应该清楚为什么`payload`属性被附加在动作调度器上。

所以 thunks 就像一个用户定义的动作创建器，它返回一个函数而不是一个动作对象。Action creator thunks 是独立的函数，而不是 reducer 函数，所以我们可以在那里编写异步代码。

action creator thunk 是一个函数，它接受用户传递的参数，并返回一个函数，该函数进一步接受 Redux Toolkit 为我们传递的调度参数。稍后调用这个返回函数的是 Redux Toolkit 本身。

动作创建者 thunk 的样板代码如下所示:

```
export const actionCreatorThunk = async(args) => {
  return (dispatch) => {
    // async code here
    dispatch(actions.actionDispatcher(args))
    // more async code
    // more dispatch functions
  }
} 
```

返回的函数也可以是一个`async`函数，因为很明显它正在处理其他的`async`代码。动作创建者可以有多个分派功能来分派多个分派动作。

然后，可以按如下方式分派操作创建者 thunks:

```
import {actionCreatorThunk} from '...'
import {useDispatch} from 'react-redux'
export default function Component(args){
    const dispatch = useDispatch()

    dispatch(actionCreatorThunk(dataToBePassed))
    ...
    ...
    ...
 }
```

Redux Toolkit 的好处在于，它不仅接受由 action reducer 函数返回的 action 对象，还接受由 action creators 返回的函数。

当确定返回的是函数而不是动作对象时，Redux Toolkit 会自动调用返回的函数，并将调度函数作为参数传递。

我们可以在进行网络调用的地方使用 action creators，从数据库发送或查询数据，然后根据发送/接收的数据设置 Redux 状态。这确保了后端和前端系统之间的正确协调。

## 概括起来

如果你已经做到了，恭喜你。我真的很感谢你花时间阅读到最后。

简而言之，同步和无副作用的代码应该放在 reducers 中，而异步代码应该用在动作创建器或副作用处理程序中，比如`useEffect`。

本文到此为止。我希望这能帮助你学到一些关于编写更好的 Redux 代码用于应用程序范围的状态管理的新知识。

如果您有任何问题或反馈，请随时通过 [Twitter](https://twitter.com/prajwalinbizz) 联系我。

你也可以在 LinkedIn 上和我联系。

如果你觉得这篇文章很有帮助，可以考虑与你正在学习 React 的朋友分享。

干杯！