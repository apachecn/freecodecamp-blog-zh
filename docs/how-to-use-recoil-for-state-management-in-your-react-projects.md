# 如何在 React 项目中使用反冲进行状态管理

> 原文：<https://www.freecodecamp.org/news/how-to-use-recoil-for-state-management-in-your-react-projects/>

如果您是 React 开发人员，您可能使用过一个库来管理 React 应用程序中的状态。你可能听说过 Redux "**React 的状态管理**库。

在很长一段时间里，Redux 是 React 应用程序中唯一可靠且被广泛采用的状态管理解决方案。Redux 已经在大型应用程序中证明了它的用例。

但是开发人员经常面临的主要问题是总体开发体验。在 Redux 的早期版本中，您必须手动设置您的全局数据存储，并手动连接每个组件来使用它和更新全局状态。基本上，开发人员在他们的应用程序中设置和使用 Redux 需要花费大量的时间和精力。

随着时间的推移，Redux 已经有所改进，现在它也提供简单的插件解决方案，如 **redux-toolkit** 。但是现在有更简单的状态管理解决方案可供 React 使用，比如 [zustand](https://github.com/pmndrs/zustand) 、[反冲](https://github.com/facebookexperimental/Recoil)和 [react-query](https://github.com/tannerlinsley/react-query) 等等。

在本文中，我们将通过构建一个简单的传统 todo 应用程序，来看看如何在 React 应用程序中设置和使用**反冲**。

在我们开始之前，我只想提一下，todo 应用程序示例的所有代码都在[这个沙箱](https://codesandbox.io/s/wwlgu)中。请随意参考和修改它。

[https://codesandbox.io/embed/to-do-app-using-recoil-wwlgu?fontsize=14&hidenavigation=1&theme=dark](https://codesandbox.io/embed/to-do-app-using-recoil-wwlgu?fontsize=14&hidenavigation=1&theme=dark)

recoil todo app example

## 如何安装反冲

让我们从安装库开始。如果你在本地电脑上工作，你可以使用`npm`或`yarn`安装反冲。

```
npm i recoil

// or

yarn add recoil 
```

## 如何添加反冲的根组件

您需要做的第一件事是用由`recoil`提供的`RecoilRoot`组件包装您的整个应用程序。

因为反冲使用 100%基于钩子的方法，所以最好用反冲提供的根组件包装整个应用程序，这样就可以从任何组件访问应用程序状态。

您可以简单地通过在 index.js(条目文件)中导入并添加`RecoilRoot`来实现。这是添加 index.js 后的样子:

```
 import { StrictMode } from "react";
import ReactDOM from "react-dom";
import { RecoilRoot } from "recoil";
import App from "./App";

const rootElement = document.getElementById("root");
ReactDOM.render(
  <StrictMode>
    <RecoilRoot>
      <App />
    </RecoilRoot>
  </StrictMode>,
  rootElement
); 
```

## 如何在反冲中创造原子

在这之后，我们需要创建一个**原子。反冲中的原子只是一块保存数据的孤立的存储器。你可以创造尽可能多的原子。**

例如，假设您正在创建一个社交媒体应用程序，用户可以在其中为某篇文章添加书签。为了存储用户加入书签的文章，可以有一个单独的 atom 来保存书签数据。

当 atom 中的一些数据发生变化时——例如，用户为一篇文章添加书签——它将重新呈现订阅或使用该 atom 的组件。

这就是`recoil`的性能部分发挥作用的地方。反冲将确保只有那些订阅了特定原子的组件才被重新渲染。

创造一个原子极其容易。创建一个`src/recoil/atom/todoAtom.js`文件并添加以下代码:

```
import { atom } from "recoil";

export const todoListAtom = atom({
  key: "todoListState",
  default: [],
}); 
```

## 如何创造你的第一个原子

你只需要从`recoil`导入`atom`函数。这个函数接受一个对象作为它的参数。

该对象中的第一个条目是`key`。这是一个唯一的字符串，它将代表这个特定的原子。`default`是这个原子的初始状态。仅此而已。这就是你设置它所需要做的一切。

确保导出`todoListAtom`,因为我们将使用它来引用这个原子。

## 如何向原子添加数据

现在让我们创建一个输入，用户可以在其中键入他们的待办事项。创建`components/TodoItemCreator.js`。

在这个组件中，我们有一个用户可以输入的输入，我们将看到在 atom 中添加一个新的 todo 是多么简单。稍后我们将看到所有使用相同 atom 的组件如何更新以显示新添加的 todo。在设计输入样式时，您可以随心所欲地发挥创造力。

这里我将只展示如何使用`useRecoilState`钩子(由`recoil`库提供，用于获取 atom 中数据的当前状态)和一个方便的函数来更新状态。

如果您已经在 React 中使用了`useState`，这看起来将与您在本地组件状态中所习惯的完全相同。`useRecoilState`钩子接受一个原子作为参数。

```
import { useState } from "react";
import { useRecoilState } from "recoil";
import { todoListAtom } from "../recoil/atoms/todoAtom";
import { generateUID } from "../utils/uuid";

export const TodoItemCreator = () => {
  const [inputValue, setInputValue] = useState("");
  const [_, setTodoList] = useRecoilState(todoListAtom);

  const onChange = (event) => {
    setInputValue(event.target.value);
  };

  const addTodoItem = () => {
    if (inputValue) {
      setTodoList((oldTodoList) => [
        ...oldTodoList,
        {
          id: generateUID(),
          text: inputValue,
          isComplete: false
        }
      ]);
      setInputValue("");
    }
  };

  return (
    <div className="todo-creator">
      <input type="text" value={inputValue} onChange={onChange} />
      <button className="add-btn" onClick={addTodoItem}>
        Add Task
      </button>
    </div>
  );
}; 
```

当用户输入并点击`Add Task`按钮时，就会调用一个`addTodoItem`函数。这个函数简单地调用钩子给出的`setTodoList`函数。

因为建议永远不要直接更新你的全局状态，而是创建一个先前 todos 的浅层副本并添加一个新的。在上面的代码片段中，`generateUID`只是一个实用函数，它将返回一个`uuidv4`唯一 id 来返回一个随机的唯一 id，我们稍后将使用它来更新一个来自`todos`列表的简单 todo。

## 如何消费来自原子的数据

现在，让我们创建一个组件，在列表中显示待办事项，并使用户能够更新、删除或标记待办事项。创建`src/components/TodoMain.js`。

```
import { useRecoilValue } from "recoil";
import { TodoItemCreator } from "./TodoItemCreator";
import { TodoItem } from "./TodoItem";
import { todoListAtom } from "../recoil/atoms/todoAtom";
import "./todo.css";

export const TodoMain = () => {
  const todoList = useRecoilValue(todoListAtom);

  return (
    <div className="parent-container">
      <div>
        <TodoItemCreator />
        {todoList.length > 0 && (
          <div className="todos-list">
            {todoList.map((todoItem) => (
              <TodoItem key={todoItem.id} item={todoItem} />
            ))}
          </div>
        )}
      </div>
    </div>
  );
}; 
```

`useRecoilValue`是由`recoil`提供的一个钩子，它只返回 atom 中日期的当前状态。我们将使用这个钩子把所有的待办事项和`map`放到它们上面，在屏幕上显示出来。

## 如何在 Atom 中更新数据

`TodoItem`是一个组件，它使用相同的`useRecoilState`钩子和一些辅助函数来查找和更新特定 todo 的状态。

```
import { useRecoilState } from "recoil";
import { todoListAtom } from "../recoil/atoms/todoAtom";

export const TodoItem = ({ item }) => {
  const [todoList, setTodoList] = useRecoilState(todoListAtom);
  const index = todoList.findIndex((listItem) => listItem === item);

  const editItemText = (event) => {
    const newList = replatItemAtIndex(todoList, index, {
      ...item,
      text: event.target.value
    });

    setTodoList(newList);
  };

  const toggleItemCompletion = () => {
    const newList = replatItemAtIndex(todoList, index, {
      ...item,
      isComplete: !item.isComplete
    });

    setTodoList(newList);
  };

  const deleteItem = () => {
    const newList = removeItemAtIndex(todoList, index);

    setTodoList(newList);
  };

  return (
    <div className="container">
      <input
        className={item.isComplete.toString() === "true" && "done-task"}
        type="text"
        value={item.text}
        onChange={editItemText}
      />
      <input
        type="checkbox"
        checked={item.isComplete}
        onChange={toggleItemCompletion}
      />
      <button className="del-btn" onClick={deleteItem}>
        X
      </button>
    </div>
  );
};

const replatItemAtIndex = (arr, index, newValue) => {
  return [...arr.slice(0, index), newValue, ...arr.slice(index + 1)];
};

const removeItemAtIndex = (arr, index) => {
  return [...arr.slice(0, index), ...arr.slice(index + 1)];
}; 
```

仅此而已。通过两个挂钩和一个函数，您可以处理 React 应用程序的所有状态管理需求。反冲的力量是其简单和初学者友好的 API 和性能。

至此，非常感谢您花时间阅读这篇文章。如果你觉得有趣，请在 [Twitter](https://twitter.com/abdadeel_) [abdadeel_](https://twitter.com/abdadeel_) 上和我一起分享有趣的 web 开发内容。