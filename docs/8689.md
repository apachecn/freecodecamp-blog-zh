# React 的 JSX vs Vue 的模板:在前端摊牌

> 原文：<https://www.freecodecamp.org/news/reacts-jsx-vs-vue-s-templates-a-showdown-on-the-front-end-b00a70470409/>

React.js 和 Vue.js 是世界上最流行的两个 JavaScript 库。它们都很强大，而且相对容易拿起和使用。

React 和 Vue:

*   使用虚拟 DOM
*   提供反应视图组件
*   保持对发展的一个方面的关注。在这种情况下，视图。

有这么多相似之处，你可能会认为它们是同一事物的不同版本。

这两个库之间有一个主要的区别:它们如何让开发人员创建视图组件，进而创建应用程序。

React 使用 JSX(React 团队创造的术语)将内容呈现到 DOM 上。那么什么是 JSX 呢？基本上，JSX 是一个 JavaScript 渲染函数，可以帮助你将 HTML 直接插入到 JavaScript 代码中。

Vue 采用不同的方法，使用类似 HTML 的模板。使用 Vue 模板就像使用 JSX，因为它们都是用 JavaScript 创建的。主要的区别是 JSX 函数从来不会在实际的 HTML 文件中使用，而 Vue 模板会。

### **反应 JSX**

让我们更深入地了解一下 JSX 是如何工作的。假设您有一个想要在 DOM 上显示的名字列表。贵公司最近招聘的新员工名单。

如果你使用普通的 HTML，你首先需要创建一个 index.html 文件。然后，您将添加以下代码行。

```
<ul>
  <li> John </li>
  <li> Sarah </li>
  <li> Kevin </li>
  <li> Alice </li>
<ul>
```

这里没什么特别的，只是普通的 HTML 代码。

那么你如何使用 JSX 做同样的事情呢？第一步是创建另一个 index.html 文件。但是您将只添加一个简单的`div`元素，而不是像以前那样添加完整的 HTML。这个`div`将是容器元素，所有的 React 代码都将在这里呈现。

`div`需要有一个惟一的 ID，以便 React 知道在哪里可以找到它。脸书倾向于使用 root 关键字，所以让我们坚持使用它。

```
<div id=root></div>
```

现在，到了最重要的一步。创建保存所有 React 代码的 JavaScript 文件。把这个叫做 app.js。

现在你已经把所有的事情都解决了，接下来是主要的事情。使用 JSX 向 Dom 显示所有新员工

您首先需要创建一个包含新雇员姓名的数组。

```
const names = [‘John’, ‘Sarah’, ‘Kevin’, ‘Alice’];
```

在那里，您需要创建一个 React 元素，该元素将动态呈现整个名称列表。这不需要您手动显示每一个。

```
const displayNewHires = (
  <ul>
    {names.map(name => <li>{name}</li> )}
  </ul>
);
```

这里要注意的关键是，你不必创建单个的李和元素。你只需要描述一下你想让他们看起来是什么样子，剩下的就交给他们了。这是一件非常强大的事情。虽然你只有几个名字，想象一下你有几十万个名字！你可以看到这是一个更好的方法。特别是 i `f th` e < li >元素比这里用的更复杂。

将内容呈现到屏幕上所需的最后一段代码是主要的 ReactDom 呈现函数。

```
ReactDOM.render(
  displayNewHires,
  document.getElementById(‘root’)
);
```

在这里，您告诉 React 使用根元素来呈现`div`中`displayNewHires`的内容。

您最终的 React 代码应该是这样的:

```
const names = [‘John’, ‘Sarah’, ‘Kevin’, ‘Alice’];
const displayNewHires = (
  <ul>
    {names.map(name => <li>{name}</li> )}
  </ul>
);
ReactDOM.render(
  displayNewHires,
  document.getElementById(‘root’)
);
```

这里要记住的一个关键点是，这都是 React 代码。这意味着它将全部编译成普通的旧 JavaScript。这是它最终的样子:

```
‘use strict’;
var names = [‘John’, ‘Sarah’, ‘Kevin’, ‘Alice’];
var displayNewHires = React.createElement(
  ‘ul’,
  null,
  names.map(function (name) {
    return React.createElement(
      ‘li’,
      null,
      name
    );
  })
);
ReactDOM.render(displayNewHires, document.getElementById(‘root’));
```

这就是全部了。现在您有了一个简单的 React 应用程序，它将显示一个姓名列表。没有什么值得大书特书的，但是它应该让您对 React 的能力有所了解。

### vista . js templates

与上一个示例保持一致，您将再次创建一个简单的应用程序，它将在浏览器上显示一个姓名列表。

您需要做的第一件事是创建另一个空的 index.html 文件。在该文件中，您将创建另一个 id 为 root 的空的`div`。请记住，根只是个人喜好。你可以随便叫这个 id。您只需要在稍后将 html 同步到 JavaScript 代码时确保它匹配即可。

这个 div 将像它在 React 中那样工作。它将告诉 JavaScript 库，在本例中是 Vue，当它想要开始进行更改时，在 DOM 中的何处查找。

完成后，您将创建一个包含所有 Vue 代码的 JavaScript 文件。称之为 app.js，以保持一致。

现在你已经准备好了文件，让我们来看看 Vue 是如何在浏览器上显示元素的。

在操作 DOM 时，Vue 使用类似模板的方法。这意味着你的 HTML 文件不会只有一个空的`div`，就像在 React 中一样。实际上，您将在 HTML 文件中编写一部分代码。

为了给你一个更好的想法，回想一下使用普通 HTML 创建一个名字列表需要做些什么。一个`<` **u** l >元素里面带着 `som` e <李>元素。在 Vue 中，你将做几乎完全相同的事情，只是增加了一些变化。

创建一个`<` ul >元素。

```
<ul>
</ul>
```

现在添加一个空的`<`李>元素 insid `e th` e < ul >元素。

```
<ul>
  <li>
  </li>
</ul>
```

还没有新消息。通过向您的`<**l**i>`元素添加一个指令，一个定制的 Vue 属性来改变这一点。

```
<ul>
  <li v-for=’name in listOfNames’>
  </li>
</ul>
```

指令是 Vue 将 JavaScript 功能直接添加到 HTML 中的方式。它们都以 v-开头，后面是描述性的名字，让你知道它们在做什么。在这种情况下，这是一个 for 循环。对于您的姓名列表中的每个姓名，`listOfNames`，您想要复制这个`<` **l** i >元素，并将其替换为带有您的列表中的姓名的`a ne` w < li >元素。

现在，代码只需要最后一次修改。目前，它会为你列表中的每个名字显示一个`<` li >元素，但是你实际上并没有告诉它在浏览器上显示实际的名字。为了解决这个问题，您将在元素 `you` **r** < li >中插入一些类似 mustache 的语法。你可能在其他 JavaScript 库中看到过。

```
<ul>
  <li v-for=’name in listOfNames’>
    {{name}}
  </li>
</ul>
```

现在`<` **l** i >元素完成了。它现在将显示名为 listOfNames 的列表中的每一项。请记住，**一词的**名称是任意的。你可以把它叫做**ed**item，它也可以达到同样的目的。关键字所做的只是充当一个占位符，用于遍历列表。

您需要做的最后一件事是创建数据集，并在您的应用程序中实际初始化 Vue。

为此，您需要创建一个新的 Vue 实例。通过将它赋给一个名为 app 的变量来实例化它。

```
let app = new Vue({
});
```

现在，这个对象将接受几个参数。第一个是最重要的，`el` (element)参数告诉 Vue 从哪里开始向 DOM 添加内容。就像你在 React 例子中做的一样。

```
let app = new Vue({
  el:’#root’,
});
```

最后一步是将数据添加到 Vue 应用程序中。在 Vue 中，传递给应用程序的所有数据都将作为 Vue 实例的参数。此外，每个 Vue 实例每种类型只能有一个参数。虽然有很多，但是在这个例子中，您只需要关注两个，`el`和`data`。

```
let app = new Vue({
  el:’#root’,
  data: {
    listOfNames: [‘Kevin’, ‘John’, ‘Sarah’, ‘Alice’]
  }
});
```

数据对象将接受一个名为`listOfNames`的数组。现在，每当您想在应用程序中使用该数据集时，您只需使用指令来调用它。简单吧？

这是最后一个应用程序:

#### **HTML**

```
<div id=”root”>
  <ul>
    <li v-for=’name in listOfNames’>
      {{name}}
    </li>
  </ul>
</div>
```

#### **JavaScript**

```
new Vue({
  el:”#root”,
  data: {
    listOfNames: [‘Kevin’, ‘John’, ‘Sarah’, ‘Alice’]
  }
});
```

### **结论**

您现在知道了如何使用 React 和 Vue 创建两个简单的应用程序。它们都提供了大量的功能，尽管 Vue 更容易使用。另外，请记住，Vue 允许使用 JSX，尽管这不是首选的实现方法。

不管怎样，这是两个强大的库，使用任何一个都不会出错。

如果你想了解更多关于 web 开发的知识，请访问我在 juanmvega.com 的网站，获取关于最新 JavaScript 标准和框架的视频和文章！