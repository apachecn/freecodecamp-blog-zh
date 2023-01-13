# 探索 Redux 是什么和为什么

> 原文：<https://www.freecodecamp.org/news/exploring-the-what-and-the-why-of-redux-6faadab4768b/>

彼得·姆巴努戈

# 探索 Redux 是什么和为什么

![F6OdxzJFbBYWL8YTaMzMtylfqC7o03b4MQg7](img/e6fa60700c4d4688b513d9e5835a57a5.png)

> “Redux 到底是什么，我为什么需要它？”

当我开始学习如何构建单页应用程序(SPA)以在我的应用程序中包含丰富的交互时，我问过自己这个问题。SPA 能够重新呈现 UI 的不同部分，而不需要服务器往返。

这是通过将代表应用程序状态的不同数据从这些数据的表示中分离出来实现的。

**视图**层将这些数据呈现给 UI。一个视图可以由不同的组件组成。例如，考虑一个带有产品列表页面的在线商店。该页面可以包含表示不同产品及其价格的组件、购物车中所有商品的直观计数，以及建议购买类似商品的组件。

**模型**层包含由视图层渲染的数据。视图中的每个组件相互独立，每个组件为给定的数据呈现一组可预测的 UI 元素，但是多个组件可以共享相同的数据。当模型发生变化时，视图会重新渲染并更新受模型更新影响的组件。

### 问题是

应用程序状态可以存储在内存中的随机对象中。也可以在 DOM 中保存一些状态。

但是分散的状态很容易导致难以管理的代码。很难调试。如果多个视图或组件共享相似的数据，有可能将这些数据存储在不同的内存位置，并且视图组件之间不会同步。

随着视图与模型的分离，数据从模型传递到视图。如果存在基于用户交互的更改，这将更新模型，并且该模型更新可能触发对另一个模型的更新，并且还更新另一个视图组件，这也可以触发对模型的更新。

这种不可预测的数据流的一个已知问题是脸书的通知错误。登录脸书后，您会看到新邮件的通知。当您阅读它时，通知会被清除。在网站上进行一些交互后，通知再次出现，然后您检查并没有新消息，通知被清除。当你与应用程序交互更多时，通知会再次返回，如此循环往复。

### 目标

如果状态管理不当，很容易增加代码的复杂性。因此，最好有一个存放数据的地方，特别是当相同的数据必须在视图中的多个地方显示时。对于杂乱无章的数据流，很难推断状态变化并预测状态变化的可能结果。

### 解决方案:单向数据流和单一事实来源

视图组件应该从这个单一的源读取数据，而不是单独保存它们自己的相同状态的版本。因此，需要一个**单一的真相来源**。

在脸书，他们想要一种更简单的方法来预测状态变化，因此提出了一种叫做**通量**的模式。Flux 是一种用于管理数据流的数据层模式。它规定数据应该只在一个方向上流动，应用程序状态包含在一个位置——事实的来源——而修改状态的逻辑只在一个位置。

**通量**

![s4L26593yAq7tsGDmN8h03mSiso2epkS78dY](img/0ba58498b4996a93eff4c3374f5381fd.png)

上图描述了数据流:

*   数据从**商店**——事实的来源——流向**视图**。视图读取数据并呈现给用户，用户与不同的视图组件交互，如果他们需要修改应用程序状态，他们通过**动作**来表达他们的意图。
*   Action 捕获任何事物与应用程序交互的方式。它是一个带有“类型”字段和一些数据的普通对象。**调度员**负责向商店发出行动。它不包含更改状态的逻辑，而是由存储本身在内部完成。
*   您可以有多个存储，每个存储包含不同应用程序域的数据。存储对与其维护的状态相关的动作做出响应。如果它更新了状态，它还会通过发出事件来通知连接到该存储的视图。
*   视图获得通知并从存储中检索数据，然后重新呈现。当状态需要再次更新时，它会经历相同的循环，这允许一种简单的方法来推理您的应用程序并使状态变化可预测。

通过实现只允许数据单向流动的应用程序体系结构，可以创建更可预测的应用程序状态。如果出现了一个错误，单向的数据流将更容易查明错误在哪里，因为数据遵循严格的通道。

这种模式有多种实现方式。我们有 [Fluxxor](http://fluxxor.com/) 、 [Flummox](https://github.com/acdlite/flummox/) 、[回流](https://github.com/spoike/refluxjs)等等。但是 Redux 比它们都高。

Redux 采用了 Flux 的概念，并对其进行了改进，以创建一个可预测的状态管理库。

Redux 的创造者 Dan Abramov 创建它的目的是为了获得更好的开发工具支持、[热重装和时间旅行调试](https://www.youtube.com/watch?v=xsSnOQynTHs)，同时仍然保持 Flux 带来的可预测性。Redux 试图使状态突变可预测。

追随 Flux 的脚步，Redux 有三个概念:

*   真相的单一来源:我已经提到了这样做的必要性。Redux 有它所谓的**店**。存储区是一个包含整个应用程序状态的对象。不同的状态存储在对象树中。这使得实现撤销/重做变得更加容易。

例如，我们可以使用 Redux 存储和跟踪购物车中的商品以及当前选择的产品，这可以在商店中建模如下:

```
{        "cartItem" : [            {                "productName" : "laser",                "quantity" : 2            },            {                "productName" : "shirt",                "quantity" : 2            }        ],        "selectedProduct" : {            "productName" : "Smiggle",            "description" : "Lorem ipsum ... ",            "price" : "$30.04"        }    }
```

*   **状态是只读的**:状态不能被视图或任何其他进程直接改变(可能是网络回调或其他事件的结果)。
    为了改变状态，你必须通过发出一个动作来表达你的意图。动作是描述您的意图的普通对象，它包含类型属性和一些其他数据。动作可以被记录并在以后重放，这有利于调试和测试。

以购物车为例，我们可以按如下方式启动一个操作:

```
store.dispatch({      type: 'New_CART_ITEM',      payload: {                   "productName" : "Samsung S4",                   "quantity" : 2                }    })    dispatch(action) emits the action, and is the only way to trigger a state change. To retrieve the state tree, you call store.getState().
```

*   **Reducer**:Reducer 负责计算出需要发生什么样的状态变化，然后转换它以反映新的变化。
    Reducer 是一个纯函数，它接受前一个(即将改变的当前状态)和一个动作，根据动作类型决定如何更新状态，对其进行转换，并返回下一个状态(更新后的状态)。

继续我们的购物车示例，假设我们想要向购物车添加一个新商品。我们分派一个类型为`**NEW_CART_ITEM**`的动作，在 reducer 中，我们通过通读动作类型并相应地采取行动来决定如何处理这个新的变更请求。

对于购物车，它将向购物车中添加新产品:

```
function shoppingCart(state = [], action) {      switch (action.type) {        case 'New_CART_ITEM':          return [...state, action.payload]        default:          return state      }    }
```

我们所做的是返回一个新的状态，除了来自动作的新状态之外，它还是旧购物车项目的集合。您应该返回一个新的状态对象，而不是改变以前的状态，这确实有助于时间旅行调试。

有些事情你绝对不能在减速器内做，它们是:

*   改变它的论点。
*   执行副作用，如 API 调用和路由转换。
*   调用非纯函数。

### 实际例子

为了演示 Redux 的工作方式，我们将制作一个简单的 SPA 来展示如何在 Redux 中管理数据，并使用 React 呈现数据。

要进行设置，请在终端中运行以下命令:

```
$ git clone git@github.com:StephenGrider/ReduxSimpleStarter.git    $ cd ReduxSimpleStarter    $ npm install
```

我们刚刚为我们将在本节中构建的内容克隆了一个入门模板。我们已经设置了 React 并下载了 Redux 和 react-redux npm 包。我们将构建一个应用程序，允许我们对待办事项或提醒我们某事的关键字做简短的笔记。

动作是必须有类型的普通 JavaScript 对象，reducers 根据指定的动作决定做什么。让我们定义常量来保存不同的动作。

在`./src/actions`中创建一个名为`types.js`的新文件，内容如下:

```
export const FETCH = 'FETCH';    export const CREATE = 'CREATE';    export const DELETE = 'DELETE';
```

接下来，我们需要定义动作，并在需要时调度它们。动作创建器是帮助创建动作的函数，结果传递给`dispatch()`。

编辑 actions 文件夹中的`index.js`文件，内容如下:

```
import { FETCH, DELETE, CREATE } from './types';
```

```
 export function fetchItems() {      return {        type: FETCH      }    }
```

```
 export function createItem(item) {      let itemtoAdd = {        [Math.floor(Math.random() * 20)]: item      };
```

```
 return {        type: CREATE,        payload: itemtoAdd      }    }
```

```
 export function deleteItem(key) {      return {        type: DELETE,        payload: key      }    }
```

我们定义了三个动作来创建、删除和检索商店中的商品。接下来，我们需要创建一个缩减器。`Math.floor(Math.random() * 20`用于为正在添加的新项目分配唯一键。这不是最佳的，但是为了演示起见，我们将在这里使用它。

在 reducer 目录中添加一个名为`item-reducer.js`的新文件:

```
import _ from 'lodash';    import { FETCH, DELETE, CREATE } from '../actions/types';
```

```
 export default function(state = {}, action) {      switch (action.type) {        case FETCH:          return state;        case CREATE:          return { ...state, ...action.payload };        case DELETE:          return _.omit(state, action.payload);      }
```

```
 return state;    }
```

定义了一个缩减器后，我们需要使用`**combineReducer()**`函数将它连接到我们的应用程序。

在 reducer 文件夹中，打开并编辑文件`index.js`:

```
import { combineReducers } from 'redux';    import ItemReducer from './item-reducer';
```

```
 const rootReducer = combineReducers({      items: ItemReducer    });
```

```
 export default rootReducer;
```

我们将创建的 reducer 传递给`combinedReducer`函数，这里的键是 reducer 负责的状态。

记住，reducers 是返回应用程序状态的纯函数。对于一个较大的应用，我们可以为一个特定的应用领域使用不同的减速器。

通过`**combineReducer**`函数，我们告诉 Redux 如何创建我们的应用程序状态。因此，思考和设计如何在 Redux 中建模应用程序状态是您应该预先做的事情。

Redux 设置了如何管理我们的状态，接下来的事情是将视图(由 React 管理)连接到 Redux。

在**组件**目录下创建一个新文件`item.js`。这将是一个智能组件，因为它知道如何与 Redux 交互来读取状态和请求状态更改。

将以下内容添加到该文件中:

```
import React, { Component } from 'react';    import { connect } from 'react-redux';    import * as actions from '../actions';
```

```
 class Item extends Component {      handleClick() {        this.props.deleteItem(this.props.id);      }
```

```
 render() {        return (          <li className="list-group-item">            {this.props.item}            <button              onClick={this.handleClick.bind(this)}              className="btn btn-danger right">              Delete            </button>          </li>        );      }    }
```

```
 export default connect(null, actions)(Item);
```

该组件显示一个项目，并允许我们删除它。`connect()`函数使 React 组件处于哑状态(它不了解 Redux，也不知道如何与之交互),并生成一个智能组件。它将动作创建者连接到组件，这样，如果调用动作创建者，返回的动作将被分派给 reducers。

我们还将制作第二个智能组件，它将前面的组件呈现为一个项目列表，并允许我们添加新项目。

用以下内容更新 components 文件夹中的文件`app.js`:

```
import _ from 'lodash';    import React, { Component } from 'react';    import { connect } from 'react-redux';    import * as actions from '../actions';    import Item from './item';
```

```
 class App extends Component {      state = { item: '' };
```

```
 componentWillMount() {        this.props.fetchItems();      }
```

```
 handleInputChange(event) {        this.setState({ item: event.target.value });      }
```

```
 handleFormSubmit(event) {        event.preventDefault();
```

```
 this.props.createItem(this.state.item, Math.floor(Math.random() * 20))      }
```

```
 renderItems() {        return _.map(this.props.items, (item, key) => {          return <Item key={key} item={item} id={key} />        });      }
```

```
 render() {        return (          <div>            <h4>Add Item</h4>            <form onSubmit={this.handleFormSubmit.bind(this)} className="form-inline">              <div className="form-group">                <input                  className="form-control"                  placeholder="Add Item"                  value={this.state.item}                  onChange={this.handleInputChange.bind(this)} />                <button action="submit" className="btn btn-primary">Add</button>              </div>            </form>            <ul className="list-group">              {this.renderItems()}            </ul>          </div>        );      }    }
```

```
 function mapStateToProps(state) {      return { items: state.items };    }
```

```
 export default connect(mapStateToProps, actions)(App)
```

这是一个智能组件(或称**容器**，一旦组件被加载，它就调用`fetchItems()`动作创建器。我们还使用了`connect`函数将 Redux 中的应用程序状态链接到 React 组件。这是通过使用函数`mapStateToProps`实现的，该函数将 Redux 状态树对象作为输入参数，并将它的一部分(条目)映射到 React 组件的 props。这允许我们使用`this.props.items`来访问它。文件的其余部分允许我们接受用户输入并将其添加到应用程序状态。

使用`npm start`运行应用程序，并尝试添加一些项目，如下图所示:

![lM02K5jSbKEBTdtx11XowQmN5NNqxdCnm5e5](img/9b52db9c1f10d01e094aa01bdac653fd.png)

### 摘要

支持页面上多个组件的丰富交互意味着这些组件有许多中间状态。SPA 能够呈现和重绘 UI 的任何部分，而无需重新加载整个页面和服务器往返。

如果数据管理不当，分散在整个 UI 中或放在内存中的随机对象中，事情很容易纠缠在一起。因此，最好将视图和视图的模型分开。

Redux 在明确定义管理数据的方式以及数据如何变化方面做得很好。

它由三个核心原则驱动，即:

*   应用程序状态的单一来源。
*   一种只读状态，确保视图和网络回调都不会直接写入该状态。
*   以及通过纯函数转换状态，称为减速器，以实现可预测性和可靠性。

这使得它成为 JavaScript 应用程序的可预测状态容器。

### 进一步阅读

*   [通量概念](https://github.com/facebook/flux/tree/master/examples/flux-concepts)
*   【Redux 入门
*   [时间旅行调试](https://www.youtube.com/watch?v=xsSnOQynTHs)

### 源代码

在这里找到源代码[。](https://github.com/pmbanugo/Simple-Redux-Example)

这最初发表在[推进器](https://blog.pusher.com/the-what-and-why-of-redux/)上。