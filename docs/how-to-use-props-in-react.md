# 如何在 React 中使用道具

> 原文：<https://www.freecodecamp.org/news/how-to-use-props-in-react/>

在本教程中，我们将讨论 React 中的一个重要概念——道具。我将向您展示如何使用它们来保持应用程序中的数据流动态。

### 先决条件

在本教程的后续部分，您将需要:

*   一款 React 应用。

此外，我假设您已经理解:

*   React 中有哪些组件以及如何使用它们。
*   如何在 React 中使用 ES6 功能(不知道怎么用？阅读[这篇](https://www.freecodecamp.org/news/how-to-use-es6-javascript-features-in-react/)文章)。
*   React 中状态管理的基础知识(不知道怎么用？阅读[这篇](https://www.freecodecamp.org/news/introduction-to-react-hooks/)文章)。

## React 里的道具是什么？

我们在 React 中使用 props 将数据从一个组件传递到另一个组件(从父组件传递到子组件)。Props 只是属性的一种更简短的说法。当您希望应用程序中的数据流是动态的时，它们非常有用。

这是我的`App.js`组件的样子:

```
function App() {
  return (
    <div className="App">

    </div>
  )
}

export default App 
```

现在让我们创建另一个名为`Tool.js`的组件。这个文件将显示产品设计师最喜欢的工具的信息。如果没有道具，代码将如下所示:

```
function Tool() {
    return (
      <div>
        <h1>My name is Ihechikara.</h1>
        <p>My favorite design tool is Figma.</p>
      </div>
    );
}

export default Tool
```

Tool.js

我们现在将这个组件导入到`App`组件中。那就是:

```
import Tool from "./Tool"

function App() {
  return (
    <div className="App">
      <Tool/>
    </div>
  )
}

export default App
```

App.js

让我们假设`Tool`组件可以跨不同的组件重用，以描述不同的设计师和他们喜欢的工具。

尽管 React 使我们无需重写代码就能轻松导入组件的逻辑，但这个特定的组件已经硬编码了数据。这意味着我们要么为每个其他组件重写逻辑，要么——您猜对了——使用 props 来改变不同组件的数据。

如果您还不明白这是如何工作的，props 允许我们动态地重用组件的逻辑。这意味着组件中的数据不是静态的。因此，对于使用该逻辑的每个其他组件，可以修改其数据以满足需求。

## 如何在 React 中使用道具

在这一节中，你将学习两种使用 props 的方法:一种没有析构，另一种有析构。

### 如何在不破坏的情况下使用道具

要使用 props，你必须在你的函数中传递`props`作为参数。这类似于将参数传递给常规的 JavaScript 函数。这里有一个例子:

```
function Tool(props) {
  const name = props.name;
  const tool = props.tool;
    return (
      <div>
        <h1>My name is {name}.</h1>
        <p>My favorite design tool is {tool}.</p>
      </div>
    );
}

export default Tool
```

Tool.js

现在我将一步一步地解释上面发生的一切。

#### 步骤 1 -将道具作为参数传入

我们在上面代码的第一行中做到了:`function Tool(props){}`。这将自动允许您在 React 应用程序的组件中使用道具。

#### 步骤 2 -声明 props 变量

```
const name = props.name;
const tool = props.tool;
```

Tool.js

如上所述，这些变量不同于常规变量，因为其中的数据与道具有关。

如果你不想为你的道具创建变量，你可以像这样直接将它们传递到你的模板中:`<h1> My name is {props.name} </h1>`

#### 步骤 3 -在 JSX 模板中使用变量

现在你已经声明了你的变量，你可以继续把它们放在你想放的地方。

```
return (
      <div>
        <h1>My name is {name}.</h1>
        <p>My favorite design tool is {tool}.</p>
      </div>
    );
```

#### 步骤 4 -将数据传递给`App`组件中的 props

我们已经完成了道具的创建，所以下一步是向它们传递数据。我们已经导入了`Tool`组件，此时它显示在浏览器中:

```
My name is .
My favorite design tool is .
```

您可以为您的属性创建默认数据，这样它们在声明时就不会显示为空。您将在最后一节看到如何做到这一点。

回想一下，这是`App`组件的当前状态:

```
import Tool from "./Tool"

function App() {
  return (
    <div className="App">
      <Tool/>
    </div>
  )
}

export default App
```

您一定想知道数据将被准确地传递到哪里。要做到这一点，您需要像属性一样传递数据。看起来是这样的:

```
import Tool from "./Tool"

function App() {
  return (
    <div className="App">
      <Tool name="Ihechikara" tool="Figma"/>
    </div>
  )
}

export default App 
```

注意到变化了吗？这里:从`<Tool/>`到`<Tool name="Ihechikara" tool="Figma"/>`。这不会给你带来错误，因为这些属性是附加在`Tool`组件中创建的道具上的。

您应该在浏览器中显示以下内容:

```
My name is Ihechikara.
My favorite design tool is Figma.
```

注意，变量名不是道具本身。如果我以这种方式创建了一个变量——`const myPropName = *props*.name`——并像这样在我的模板中使用这个变量:`<h1>My name is {myPropName}.</h1>`，那么如果我这样做的话，代码仍然会完美地工作:`<Tool name="Ihechikara" tool="Figma"/>`。`name`属性来自`props.name`,而不是包含道具的变量名。

现在，您可以使用在`Tool`组件中定义的逻辑为任何组件动态创建数据。你想声明多少道具都可以。

接下来，您将学习如何通过析构来使用 props。

### 如何使用带有析构的道具

除了声明属性的方法之外，这一部分的代码与上一部分完全相同。如果你不知道如何在 JavaScript 中使用析构，那么看看这篇文章

在上一节中，我们这样声明我们的属性:

```
const name = props.name;
const tool = props.tool;
```

但是我们不需要用析构来做这些。您只需这样做:

```
function Tool({name, tool}) {

    return (
      <div>
        <h1>My name is {name}.</h1>
        <p>My favorite design tool is {tool}.</p>
      </div>
    );
}

export default Tool
```

Tool.js

不同之处在于第一行代码。我们没有将`props`作为参数传递，而是将变量析构并作为函数的参数传递。

其他一切都保持不变。

请注意，您不仅仅局限于单个变量作为您的 props 数据——您同样可以传入函数，甚至来自对象的数据。

### 如何设置道具的默认值

如果在创建 props 数据时不希望它们为空，可以传入一个默认值。下面是如何做到这一点:

```
function Tool({name, tool}) {

    return (
      <div>
        <h1>My name is {name}.</h1>
        <p>My favorite design tool is {tool}.</p>
      </div>
    );

  }

  Tool.defaultProps = {
    name: "Designer",
    tool: "Adobe XD"
  }
export default Tool 
```

就在组件导出之前的代码末尾，我们为我们的 props 声明了默认值。为了做到这一点，我们从组件的名称开始，用一个点/句点将它与`defaultProps`连接起来，这是在创建 React 应用程序时内置的。

现在，无论我们在哪里导入这个组件，这些值都将是初始值，而不是空的。当您向子组件传递数据时，它会覆盖默认值。

## 结论

本文涵盖了开始使用 props 和跨组件动态传递数据所需的一切。

理解这些概念的最好方法是用它们来实践和构建令人惊叹的东西，所以确保你不只是通读，也要去构建。

可以在 Twitter [@ihechikara2](https://twitter.com/Ihechikara2) 找到我。编码快乐！