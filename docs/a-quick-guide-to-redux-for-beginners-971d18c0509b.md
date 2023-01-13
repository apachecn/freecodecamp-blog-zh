# 初学者 Redux 快速指南

> 原文：<https://www.freecodecamp.org/news/a-quick-guide-to-redux-for-beginners-971d18c0509b/>

> 对学习 JavaScript 感兴趣？在 jshandbook.com 获得我的电子书

Redux 是一个状态管理器，通常与 React 一起使用。它不依赖于那个库，也可以与其他技术一起使用。但是为了这篇文章，我们将坚持 React。

React 有自己的管理状态的方法，你可以在我的 [React 初学者指南](https://flaviocopes.com/react-beginners-guide/)中阅读我对在 React 中管理状态的介绍。

我在那篇文章中没有提到的是，我讨论的方法**不能扩展**。

在简单的情况下，在树中向上移动状态是可行的，但是在复杂的应用程序中，您可能会发现自己使用 props 上下移动几乎所有的状态。更好的方法是使用一个外部全局存储库。

Redux 是管理应用程序状态的一种方式。

有几个概念需要掌握，但是一旦你掌握了它们，Redux 是解决这个问题的一个非常简单的方法。

再次提醒:Redux 在 React 应用程序中非常受欢迎，但它绝不是 React 独有的。几乎每个流行的框架都有绑定。也就是说，我将分享一些使用 React 的例子，因为它太常见了。

### 什么时候应该使用 Redux？

Redux 是大中型应用的理想选择。只有当您在使用 React 的默认状态管理(或您使用的任何其他库)管理状态时遇到困难时，才应该使用它。

简单的 app 应该根本不需要(简单的 app 也没什么不好)。

### 不可变状态树

在 Redux 中，应用的整个状态由**一个** JavaScript 对象表示，称为**状态**或**状态树**。

我们称之为**不可变状态树**，因为它是只读的:不能直接改变。

它只能通过分派一个**动作**来改变。

### 行动

一个**动作**是**，一个以极简方式描述变化的 JavaScript 对象**(只包含需要的信息):

```
{   type: 'CLICKED_SIDEBAR' } 
```

```
// e.g. with more data {   type: 'SELECTED_USER',   userId: 232 }
```

动作对象的唯一要求是拥有一个`type`属性，它的值通常是一个字符串。

### 动作类型应该是常量

在一个简单的应用程序中，一个动作类型可以定义为一个字符串(正如我在上一篇文章的例子中所做的)。

当应用程序增长时，最好使用常量:

```
const ADD_ITEM = 'ADD_ITEM' const action = { type: ADD_ITEM, title: 'Third item' }
```

并将动作分离到它们自己的文件中，然后导入它们:

```
import { ADD_ITEM, REMOVE_ITEM } from './actions'
```

### 动作创建者

**动作创建者**是创建动作的函数。

```
function addItem(t) {   return {     type: ADD_ITEM,     title: t   } }
```

您通常结合触发调度程序来运行操作创建器:

```
dispatch(addItem('Milk'))
```

或者通过定义动作分派器功能:

```
const dispatchAddItem = i => dispatch(addItem(i)) dispatchAddItem('Milk')
```

### 还原剂

当一个动作被触发时，必须发生一些事情，应用程序的状态必须改变。

这是**减速器**的工作。

### 什么是减速器

一个**缩减器**是一个**纯函数**，它基于前一个状态树和被调度的动作计算下一个状态树。

```
(currentState, action) => newState
```

一个纯函数接受一个输入并返回一个输出，而不改变输入或其他任何东西。因此，一个 reducer 返回一个全新的状态树对象来代替之前的对象。

### 减压器不应该做什么

减速器应该是一个纯函数，所以它应该**而不是**:

*   改变其论点
*   改变状态——应该用`Object.assign({}, ...)`创建一个新的状态
*   产生副作用(没有 API 调用改变任何事情)
*   调用非纯函数，非纯函数是根据输入以外的因素改变输出的函数(如`Date.now()`或`Math.random()`

没有强化，但是你要坚持规则。

### 多重减速器

因为一个复杂的应用程序的状态可能非常广泛，所以对于任何类型的操作，都不是只有一个缩减器，而是有许多缩减器。

### 减速器的模拟

就其核心而言，Redux 可以通过以下模型进行简化:

#### 国家

```
{   list: [     { title: "First item" },     { title: "Second item" },   ],   title: 'Grocieries list' }
```

#### 行动清单

```
{ type: 'ADD_ITEM', title: 'Third item' } { type: 'REMOVE_ITEM', index: 1 } { type: 'CHANGE_LIST_TITLE', title: 'Road trip list' }
```

#### 国家每一部分的缩减者

```
const title = (state = '', action) => {  if (action.type === 'CHANGE_LIST_TITLE') {     return action.title   } else {     return state   } } 
```

```
const list = (state = [], action) => {   switch (action.type) {     case 'ADD_ITEM':       return state.concat([{ title: action.title }])     case 'REMOVE_ITEM':       return state.map((item, index) =>         action.index === index           ? { title: item.title }           : item     default:       return state   } }
```

#### 整个国家的减速器

```
const listManager = (state = {}, action) => {   return {     title: title(state.title, action),     list: list(state.list, action),   } }
```

### 故事

**存储**是一个对象，它:

*   **保存应用程序的状态**
*   **通过`getState()`暴露状态**
*   允许您通过`dispatch()`用**更新状态**
*   允许您使用`subscribe()`注册(注销)为**状态改变监听器**

一个店铺在 app 中**是唯一的**。

下面是 listManager 应用程序商店的创建过程:

```
import { createStore } from 'redux' import listManager from './reducers' let store = createStore(listManager)
```

### 我可以用服务器端数据初始化存储吗？

当然，**只是经过一个起始状态**:

```
let store = createStore(listManager, preexistingState)
```

### 获取状态

```
store.getState()
```

### 更新状态

```
store.dispatch(addItem('Something'))
```

### 倾听状态变化

```
const unsubscribe = store.subscribe(() =>   const newState = store.getState() ) 
```

```
unsubscribe()
```

### 数据流

Redux 中的数据流总是**单向**。

你在商店上调用`dispatch()`，传递一个动作。

存储负责将动作传递给缩减器，生成下一个状态。

存储更新状态并警告所有侦听器。

> 对学习 JavaScript 感兴趣？在 jshandbook.com 获得我的电子书