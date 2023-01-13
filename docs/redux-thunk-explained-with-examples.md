# Redux Thunk 举例说明

> 原文：<https://www.freecodecamp.org/news/redux-thunk-explained-with-examples/>

Redux Thunk 是一个中间件，它允许您在 Redux 中返回函数，而不仅仅是动作。这允许延迟行动，包括承诺工作。

这个中间件的一个主要用例是处理可能不同步的动作，例如，使用 axios 发送 GET 请求。Redux Thunk 允许我们异步调度这些动作，并解析每个返回的承诺。

## 安装和设置

Redux Thunk 可以通过在命令行运行`npm install redux-thunk --save`或`yarn add redux-thunk`来安装。

因为它是一个 Redux 工具，所以您还需要设置 Redux。安装后，使用`applyMiddleware()`启用:

```
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducers/index';

const store = createStore(
  rootReducer,
  applyMiddleware(thunk)
);
```

## 如何使用 Redux Thunk

一旦安装了 Redux Thunk 并用`applyMiddleware(thunk)`将其包含在您的项目中，您就可以开始异步调度操作了。

例如，这里有一个简单的增量计数器:

```
const INCREMENT_COUNTER = 'INCREMENT_COUNTER';

function increment() {
  return {
    type: INCREMENT_COUNTER
  };
}

function incrementAsync() {
  return dispatch => {
    setTimeout(() => {
      // You can invoke sync or async actions with `dispatch`
      dispatch(increment());
    }, 1000);
  };
}
```

下面是如何在轮询 API 后设置成功和失败操作:

```
const GET_CURRENT_USER = 'GET_CURRENT_USER';
const GET_CURRENT_USER_SUCCESS = 'GET_CURRENT_USER_SUCCESS';
const GET_CURRENT_USER_FAILURE = 'GET_CURRENT_USER_FAILURE';

const getUser = () => {
  return (dispatch) => {     //nameless functions
    // Initial action dispatched
    dispatch({ type: GET_CURRENT_USER });
    // Return promise with success and failure actions
    return axios.get('/api/auth/user').then(  
      user => dispatch({ type: GET_CURRENT_USER_SUCCESS, user }),
      err => dispatch({ type: GET_CURRENT_USER_FAILURE, err })
    );
  };
};
```

## 更多信息:

*   [如何用 React、Redux 和 Thunk 实现数据轮询](https://www.freecodecamp.org/news/how-to-implement-data-polling-with-react-redux-and-thunk-33cd1e47f89c/)
*   [如何在 24 行 JavaScript 中实现 Redux](https://www.freecodecamp.org/news/redux-in-24-lines-of-code/)
*   [如何将 React 连接到 Redux —图解指南](https://www.freecodecamp.org/news/how-to-connect-react-to-redux-a-diagrammatic-guide-d2687c14750a/)