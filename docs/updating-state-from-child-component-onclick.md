# 从子组件 Onclick 更新状态

> 原文：<https://www.freecodecamp.org/news/updating-state-from-child-component-onclick/>

如何从子组件更新父组件的状态是最常被问到的 React 问题之一。

假设您正在尝试编写一个简单的 recipe box 应用程序，这是您目前为止的代码:

```
import React from "react";
import ReactDOM from "react-dom";

import RecipeBox from "./RecipeBox";

const rootElement = document.getElementById("root");
ReactDOM.render(
  <React.StrictMode>
    <RecipeBox />
  </React.StrictMode>,
  rootElement
); 
```

index.js

```
import React from "react";

export default class RecipeBox extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      input: "",
      recipeList: [
        {
          recipe: "Tacos",
          directions: "make tacos",
          ingredients: ["meat"]
        },
        {
          recipe: "pizza",
          directions: "bake",
          ingredients: ["dough"]
        }
      ]
    };
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({
      input: event.target.value
    });
  }

  handleSubmit() {
    const newRecipe = this.state.recipelist[0].recipe;
    setState({
      recipeList[0].recipe: newRecipe
    });
  }

  render() {
    const ITEMS = this.state.recipeList.map(({ directions }) => (
      <li>{directions}</li>
    ));
    return (
      <div>
        <EditList
          input={this.state.input}
          handleChange={this.handleChange}
          onSubmit={this.handleSubmit}
        />
        <ul>{ITEMS}</ul>
      </div>
    );
  }
}

class EditList extends React.Component {
  render() {
    return (
      <div>
        <input
          type='text'
          value={this.props.input}
          onChange={this.props.handleChange}
        />
        <button onClick={this.props.onSubmit}>Submit</button>
      </div>
    );
  }
}
```

RecipeBox.js

最终，您希望`handleChange`捕获用户输入的内容并更新特定的食谱。

但是当你试着运行你的应用程序时，你会在终端和开发控制台看到很多错误。让我们仔细看看这是怎么回事。

## 修复错误

首先在`handleSubmit`，`setState`应该是`this.setState`:

```
handleSubmit() {
  const newRecipe = this.state.recipelist[0].recipe;
  this.setState({
    recipeList[0].recipe: newRecipe
  });
}
```

您已经在进行适当的操作，或者将父组件`RecipeBox`的内容正确地传递给其子组件`EditList`。但是当有人在输入框中输入文本并点击“提交”时，状态并没有像你预期的那样更新。

## 如何从子组件更新状态

这里您遇到了问题，因为您试图更新嵌套数组(`recipeList[0].recipe: newRecipe`)的状态。这在 React 中不起作用。

相反，您需要创建一个完整的`recipeList`数组的副本，修改要在复制的数组中更新的元素的值`recipe`，并将修改后的数组赋回给`this.state.recipeList`。

首先，使用 spread 语法创建一个`this.state.recipeList`的副本:

```
handleSubmit() {
  const recipeList = [...this.state.recipeList];
  this.setState({
    recipeList[0].recipe: newRecipe
  });
}
```

然后更新您想要更新的元素的配方。让我们将第一个元素作为概念验证:

```
handleSubmit() {
  const recipeList = [...this.state.recipeList];
  recipeList[0].recipe = this.state.input;
  this.setState({
    recipeList[0].recipe: newRecipe
  });
}
```

最后，用您的新副本更新当前的`recipeList`。此外，将`input`设置为空字符串，这样在用户单击“提交”后文本框为空:

```
handleSubmit() {
  const recipeList = [...this.state.recipeList];
  recipeList[0].recipe = this.state.input;
  this.setState({
    recipeList,
    input: ""
  });
}
```