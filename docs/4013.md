# React 中的功能组件与类组件

> 原文：<https://www.freecodecamp.org/news/functional-components-vs-class-components-in-react/>

React 中主要有两个组件:

*   功能组件
*   类别组件

## **功能组件**

*   功能组件是基本的 JavaScript 函数。这些通常是箭头函数，但也可以用常规的`function`关键字创建。
*   有时被称为“哑”或“无状态”组件，因为它们只是接受数据并以某种形式显示它们；也就是说，他们主要负责渲染用户界面。
*   React 生命周期方法(例如，`componentDidMount`)不能在功能组件中使用。
*   功能组件中没有使用呈现方法。
*   这些组件主要负责 UI，通常只是表示性的(例如，一个按钮组件)。
*   功能组件可以接受和使用道具。
*   如果不需要使用反应状态，应该优先选择功能组件。

```
import React from "react";

const Person = props => (
  <div>
    <h1>Hello, {props.name}</h1>
  </div>
);

export default Person;
```

## **类组件**

*   类组件利用 ES6 类并扩展 React 中的`Component`类。
*   有时称为“智能”或“有状态”组件，因为它们倾向于实现逻辑和状态。
*   React 生命周期方法可以在类组件内部使用(例如，`componentDidMount`)。
*   你将道具传递给类组件，并用`this.props`访问它们

```
import React, { Component } from "react";

class Person extends Component {
  constructor(props){
    super(props);
    this.state = {
      myState: true;
    }
  }

  render() {
    return (
      <div>
        <h1>Hello Person</h1>
      </div>
    );
  }
}

export default Person;
```

## **更多信息**

*   [反应成分](https://reactjs.org/docs/components-and-props.html)
*   [功能与类组件](https://react.christmas/16)
*   [React 中的有状态与无状态功能组件](https://code.tutsplus.com/tutorials/stateful-vs-stateless-functional-components-in-react--cms-29541)
*   [状态和生命周期](https://reactjs.org/docs/state-and-lifecycle.html)