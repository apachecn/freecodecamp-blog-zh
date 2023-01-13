# 如何按照惯例创建 Redux reducer

> 原文：<https://www.freecodecamp.org/news/how-to-create-a-redux-reducer-by-convention-14f7e77bfc/>

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE) 被 book authority****评为 [****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781)****

Redux 是一个非常流行的状态管理库。它通过将所有存储和调度程序组合在一个存储对象中，简化了原始的 Flux 体系结构。

Redux 提倡使用函数式编程来管理状态。它介绍了减速器的功能概念。

### 数据流

让我们看看 Redux 存储内部的数据流。

一个动作是一个普通的对象，它包含了完成这个动作所需的所有信息。

动作创建器是创建动作对象的功能。

### 还原剂

缩减器是一个纯粹的函数，它将状态和动作作为参数，并返回新的状态。

可能有许多管理部分根状态的 reducers。我们可以用`combineReducers()`效用函数将它们结合在一起，创建根缩减器。

下面是`todos`减速器的样子:

```
import matchesProperty from "lodash/matchesProperty";
function todos(todos = [], action) {
 switch (action.type) {
  case "add_todo":
    const id = getMaxId(todos) + 1;
    const newTodo = { ...action.todo, id };
    return todos.concat([newTodo]);
  case "remove_todo":
    const index = todos.findIndex(matchesProperty("id",
                                  action.todo.id));
    return [...todos.slice(0, index), ...todos.slice(index + 1)];
  case "reset_todos":
    return action.todos;
  default:
    return state;
  }
}
export default todos;
```

这里的`state`是待办事项列表。我们可以用它的动作来形容像`add_todo`、`remove_todo`、`reset_todos`。

### 常规减速器

我想去掉 reducer 中的`switch`语句。功能应该很小，只做一件事。

让我们将 reducer 拆分成小的纯函数，其名称与动作类型相匹配。我将调用这些 setter 函数。它们都将状态和动作作为参数，并返回新的状态。

```
function remove_todo(todos, action) {
  const index = todos.findIndex(matchesProperty("id",
                                action.todo.id));
  return [...todos.slice(0, index), ...todos.slice(index + 1)];
}

function reset_todos(todos, action) {
  return action.todos;
}

function add_todo(todos, action) {
  const id = getMaxId(todos) + 1;
  const newTodo = { ...action.todo, id};
  return todos.concat([newTodo]);
}
```

#### 重复操作

我想将所有这些小功能结合在一起，创建原始的减速器功能。为此，我们可以使用 [redux-actions](https://redux-actions.js.org/) 中的`handleActions()`实用函数。

```
import { handleActions } from "redux-actions";

const reducer = handleActions(
  { remove_todo, reset_todos, add_todo },
  []
);

export default reducer;
```

setter 函数将按照惯例运行。当类型为`remove_todo`的动作进来时，将执行`remove_todo()` setter 函数。

[这里是 codesandbox](https://codesandbox.io/s/26m5xrxry) 上的样例代码。

[****发现函数式 JavaScript****](https://read.amazon.com/kp/embed?asin=B07PBQJYYG&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_cm5KCbE5BDJGE&source=post_page---------------------------) 被 book authority****评为[****最佳函数式编程新书之一！****](https://bookauthority.org/books/new-functional-programming-books?t=7p46zt&s=award&book=1095338781&source=post_page---------------------------)****

****关于在 React 中应用函数式编程技术的更多信息，请看一下**** [****函数式 React****](https://read.amazon.com/kp/embed?asin=B07S1NLFTS&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_Pko5CbA30383Y) ****。****

学习 ****功能性 React**** ，以基于项目的方式，用 [****功能性架构用 React****](https://read.amazon.com/kp/embed?asin=B0846NRJYR&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_o.hlEbDD02JB2)****。****

[在 Twitter 上关注](https://twitter.com/cristi_salcescu)