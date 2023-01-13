# REACT–简单的介绍组件没有渲染？

> 原文：<https://www.freecodecamp.org/news/react-simple-intro-component-not-rendering/>

React 的一大优点是它灵活的组件系统。一旦您掌握了它，您就可以将您的应用程序分解成可重用的组件，并将它们包含在整个项目中。

问题是，有几个陷阱使得新手很难对组件进行操作。

例如，假设您有以下组件，`mainIntro.js`:

```
const mainIntro = props => (
    <div id="quote-box">
     <h1> Hunter x Hunter Quotes </h1>

     <div id="text">
        "When I say it doesn't hurt me, that means I can bear it."
     </div>

     <div id="author">
        - Killua Zoldyck
     </div>

     <button id="new-quote"> Next Quote </button>

     <a href="#" id="tweet-quote" target="_blank"> Tweet this quote </a>

    </div>
)

export default mainIntro;
```

并想导入到`App.js`:

```
import mainIntro from './components'

class App extends React.Component{
    render(){
        return(
            <mainIntro />
        );
    }
}

const mainNode = document.getElementById("quoter");
ReactDOM.render(<App />,mainNode);
```

但是由于某种原因`mainIntro`没有加载。让我们仔细看看发生了什么。

## 命名您的组件

对于任何熟悉面向对象编程的人来说，用大写字母命名每个类是一种常见的习惯。例如，描述一个人的类将被称为`Person`以表明它是一个类。

在 React 中，它使用 JSX 而不是普通的 JavaScript，标签的第一个字母表示它是哪种元素。大写的第一个字符用于指定 React 组件，因此`mainIntro`应该改为称为`MainIntro`:

```
const MainIntro = props => (
    <div id="quote-box">
     <h1> Hunter x Hunter Quotes </h1>

     <div id="text">
        "When I say it doesn't hurt me, that means I can bear it."
     </div>

     <div id="author">
        - Killua Zoldyck
     </div>

     <button id="new-quote"> Next Quote </button>

     <a href="#" id="tweet-quote" target="_blank"> Tweet this quote </a>

    </div>
)

export default MainIntro;
```

虽然文件名仍然可以是`mainIntro.js`，但是将第一个字符大写也是一个好主意。稍后当您扫描目录的内容时，您将很快能够挑选出`MainIntro.js`包含一个组件。

现在`App.js`应该是这样的:

```
import MainIntro from './components/MainIntro.js';

class App extends React.Component{
    render(){
        return(
            <MainIntro />
        );
    }
}

const mainNode = document.getElementById("quoter");
ReactDOM.render(<App />,mainNode);
```

## React 是如何安装的？

使用 React 有两种主要方式。首先，在本地安装和设置它，可能通过`create-react-app`。第二，通过一个 CDN。

您可能已经注意到上面的代码片段实际上没有在项目中包含 React with`import React from'react';`。如果您在本地使用 React，这将抛出一个错误。

但是，如果您使用 CDN 来加载 React，它是全局可用的，您不需要像上面那样导入它。

## 箭头功能

在深入 React 之前，对 JavaScript，尤其是 ES6 语法有一个扎实的理解是很重要的。

再看一下`MainIntro`组件:

```
const MainIntro = props => (
    <div id="quote-box">
     <h1> Hunter x Hunter Quotes </h1>

     <div id="text">
        "When I say it doesn't hurt me, that means I can bear it."
     </div>

     <div id="author">
        - Killua Zoldyck
     </div>

     <button id="new-quote"> Next Quote </button>

     <a href="#" id="tweet-quote" target="_blank"> Tweet this quote </a>

    </div>
)

export default MainIntro;
```

如果仔细观察第一行，您会注意到一个语法错误:

```
const MainIntro = props => (
```

您正在编写一个功能组件，它通常是简单的 JavaScript 函数，可以接受属性作为参数并返回有效的 JSX。当然，语法需要正确才能正确返回。

[箭头函数](https://www.freecodecamp.org/news/arrow-function-javascript-tutorial-how-to-declare-a-js-function-with-the-new-es6-syntax/)可以用很多方式编写，但是对于这个例子，你需要加上花括号(`{}`)，并确保从组件本身返回 JSX:

```
const MainIntro = props => {
  return (
    <div id="quote-box">
     //... rest of the code   
    </div>
  );
}
```

实现了上面提到的所有更改后，您的组件现在应该可以正确呈现了。

尽管 React 中函数组件和类组件之间的主要区别曾经是前者是“无状态的”,而后者是“有状态的”,但是 React 钩子已经模糊了它们之间的界限。在这个[概述](https://www.freecodecamp.org/news/functional-components-vs-class-components-in-react/)中阅读关于这两个组件的更多信息，并且更深入地研究带有 React 挂钩的[功能组件。](https://www.freecodecamp.org/news/a-few-questions-on-functional-components/)