# React 样式组件:内联样式+ 3 种其他 CSS 样式方法(带示例)

> 原文：<https://www.freecodecamp.org/news/react-styled-components-inline-styles-3-other-css-styling-approaches-with-examples/>

没有一种正确的方法来设计 React 组件的样式。这完全取决于您的前端应用程序有多复杂，以及哪种方法最适合您。

有四种不同的方法来设计 React 应用程序的风格，在这篇文章中，你将会学到所有的方法。让我们从内嵌样式开始。

# 内嵌样式

React 组件由 JSX 元素组成。但是仅仅因为你不写常规的 HTML 元素并不意味着你不能使用旧的内联样式方法。

与 JSX 的唯一区别是内联样式必须写成一个对象而不是一个字符串。

这里有一个简单的例子:

```
import React from "react";

export default function App() {
  return (
      <h1 style={{ color: "red" }}>Hello World</h1>
  );
}
```

在上面的样式属性中，第一组花括号将告诉您的 JSX 解析器括号之间的内容是 JavaScript(而不是字符串)。第二组花括号将初始化一个 JavaScript 对象。

多于一个单词的样式属性名称用 camelCase 书写，而不是使用传统的连字符样式。例如，通常的`text-align`属性在 JSX 必须写成`textAlign`:

```
import React from "react";

export default function App() {
  return (
      <h1 style={{ textAlign: "center" }}>Hello World</h1>
  );
}
```

因为 style 属性是一个对象，所以也可以通过将它写成常量来分隔样式。这样，您可以根据需要在其他元素上重用它:

```
import React from "react";

const pStyle = {
  fontSize: '16px',
  color: 'blue'
}

export default function App() {
  return (
      <p style={pStyle}>The weather is sunny with a small chance of rain today.</p>
  );
}
```

如果需要进一步扩展段落样式，可以使用对象扩展操作符。这将允许您向已经声明的样式对象添加内联样式:

```
import React from "react";
const pStyle = {
  fontSize: "16px",
  color: "blue"
};
export default function App() {
  return (
    <>
      <p style={pStyle}>
        The weather is sunny with a small chance of rain today.
      </p>
      <p style={{ ...pStyle, color: "green", textAlign: "right" }}>
        When you go to work, don't forget to bring your umbrella with you!
      </p>
    </>
  );
} 
```

内联样式是 JS 样式技术中 CSS 最基本的例子。

使用内联样式方法的一个好处是，您将拥有一种简单的以组件为中心的样式技术。通过使用对象进行样式设置，您可以通过扩展对象来扩展您的样式。然后，如果您愿意，可以向其中添加更多的样式属性。

但是在一个大型复杂的项目中，您需要管理数百个 React 组件，这可能不是您的最佳选择。

不能使用内联样式指定伪类。这意味着`:hover`、`:focus`、`:active`或`:visited`离开窗口而不是组件。

此外，您不能为响应样式指定媒体查询。让我们考虑另一种方式来设计您的 React 应用程序。

## CSS 样式表

当您使用`create-react-app`构建 React 应用程序时，您将自动使用 webpack 来处理资产导入和处理。

webpack 的伟大之处在于，由于它处理您的资产，您还可以使用 JavaScript `import`语法将`.css`文件导入到您的 JavaScript 文件中。然后，您可以在想要设置样式的 JSX 元素中使用 CSS 类名，如下所示:

```
.paragraph-text {
  font-size: 16px;
  color: 'blue';
}
```

```
import React, { Component } from 'react';
import './style.css';

export default function App() {
  return (
    <>
      <p className="paragraph-text">
        The weather is sunny with a small chance of rain today.
      </p>
    </>
  );
}
```

这样，CSS 将从您的 JavaScript 文件中分离出来，您可以像往常一样编写 CSS 语法。

使用这种方法，您甚至可以在 React 应用程序中包含一个 CSS 框架，如 [Bootstrap](https://create-react-app.dev/docs/adding-bootstrap/) 。您需要做的就是将 CSS 文件导入到您的根组件中。

这个方法将使您能够使用所有的 CSS 特性，包括伪类和媒体查询。但是使用样式表的缺点是您的样式不会本地化到您的组件。

所有 CSS 选择器都有相同的全局范围。这意味着一个选择器可能会产生不必要的副作用，破坏应用程序的其他视觉元素。

就像内联样式一样，使用样式表仍然会给你带来在大项目中维护和更新 CSS 的问题。

# CSS 模块

CSS 模块是一个常规的 CSS 文件，默认情况下，它的所有类和动画名称都在本地范围内。

每个 React 组件都有自己的 CSS 文件，您需要将所需的 CSS 文件导入到您的组件中。

为了让 React 知道您正在使用 CSS 模块，请使用`[name].module.css`约定命名您的 CSS 文件。

这里有一个例子:

```
.BlueParagraph {
  color: blue;
  text-align: left;
}
.GreenParagraph {
  color: green;
  text-align: right;
}
```

```
import React from "react";
import styles from "./App.module.css";
export default function App() {
  return (
    <>
      <p className={styles.BlueParagraph}>
        The weather is sunny with a small chance of rain today.
      </p>
      <p className={styles.GreenParagraph}>
        When you go to work, don't forget to bring your umbrella with you!
      </p>
    </>
  );
} 
```

当您构建应用程序时，webpack 会自动查找具有`.module.css`名称的 CSS 文件。Webpack 将获取这些类名，并将它们映射到一个新的、生成的本地化名称。

这是上面例子的沙箱。如果检查蓝色段落，您会看到元素类被转换成了`_src_App_module__BlueParagraph`。

[https://codesandbox.io/embed/css-modules-example-eqh5o?fontsize=14&hidenavigation=1&theme=dark](https://codesandbox.io/embed/css-modules-example-eqh5o?fontsize=14&hidenavigation=1&theme=dark)

CSS 模块确保 CSS 语法的作用域是本地的。

使用 CSS 模块的另一个好处是，你可以通过继承你编写的其他类来构造一个新的类。这样，您将能够重用您以前编写的 CSS 代码，如下所示:

```
.MediumParagraph {
  font-size: 20px;
}
.BlueParagraph {
  composes: MediumParagraph;
  color: blue;
  text-align: left;
}
.GreenParagraph {
  composes: MediumParagraph;
  color: green;
  text-align: right;
}
```

最后，为了编写具有全局作用域的普通样式，可以在类名前使用`:global`选择器:

```
:global .HeaderParagraph {
  font-size: 30px;
  text-transform: uppercase;
}
```

然后，您可以像 JS 文件中的普通类一样引用全局作用域样式:

```
<p className="HeaderParagraph">Weather Forecast</p>
```

# 样式组件

Styled Components 是一个为 React 和 React Native 设计的库。它结合了 JS 中的 CSS 和 CSS 模块方法来设计组件的样式。

让我给你看一个例子:

```
import React from "react";
import styled from "styled-components";

// Create a Title component that'll render an <h1> tag with some styles
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`;

export default function App() {
  return <Title>Hello World!</Title>;
}
```

当您编写自己的样式时，您实际上是在创建一个附加了您的样式的 React 组件。通过利用 JavaScript 的标记模板文字，`styled.h1`后跟反勾号的有趣语法成为可能。

创建样式组件是为了解决以下问题:

*   自动关键 CSS :样式化组件跟踪页面上呈现的组件，并自动注入它们的样式。结合代码分割，这意味着您的用户加载最少的必要代码。
*   **没有类名错误**:样式化组件为你的样式生成唯一的类名。你永远不必担心重复、重叠或拼写错误。
*   更容易删除 CSS :很难知道一个类名是否已经在你的代码库中被使用。Styled-components 使这一点显而易见，因为每一点样式都与特定的组件相关联。如果组件未被使用(工具可以检测到)并被删除，那么它的所有样式都会随之被删除。
*   **简单的动态样式**:根据组件的道具或全局主题来调整组件的样式简单而直观，无需手动管理许多类。
*   **无痛维护**:你永远不必在不同的文件中寻找影响你的组件的样式，所以无论你的代码库有多大，维护都是小菜一碟。
*   自动厂商前缀:根据当前标准编写你的 CSS，让样式化组件处理剩下的事情。

你在获得所有这些好处的同时，仍然可以编写你所熟悉和喜爱的 CSS 只是绑定到单独的组件上。

如果你想了解更多关于样式化组件的知识，你可以访问[文档](https://styled-components.com/docs)并查看更多的例子。

# 结论

许多开发人员仍然在争论设计 React 应用程序的最佳方式。用非传统的方式编写 CSS 既有好处也有坏处。

很长一段时间以来，将 CSS 文件和 HTML 文件分开被认为是最佳实践，尽管这会导致很多问题。

但是今天，您可以选择编写面向组件的 CSS。这样，您的样式将利用 React 的模块化和可重用性。您现在能够编写更持久和可伸缩的 CSS 了。