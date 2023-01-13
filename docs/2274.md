# React 测试库–包含 JavaScript 代码示例的教程

> 原文：<https://www.freecodecamp.org/news/react-testing-library-tutorial-javascript-example-code/>

这篇文章将帮助您了解什么是 React 测试库，以及如何使用它来测试您的 React 应用程序。

本教程将假设您已经了解一些基本的 JavaScript，并理解 React 的基本工作原理。

[React 测试库](https://testing-library.com/docs/react-testing-library/intro)是一个测试实用工具，用于测试 React 在浏览器上渲染的实际 DOM 树。该库的目标是帮助您编写类似用户如何使用您的应用程序的测试。这可以让您更有信心，当真正的用户使用它时，您的应用程序会按预期工作。

该库通过提供实用程序方法来做到这一点，这些方法将以与用户相同的方式查询 DOM。例如，用户会找到一个按钮，通过它的文本“保存”他们的工作，所以库为您提供了`getByText()`方法。稍后您将了解更多关于该库的测试方法。

但是首先，让我们看一个 React 测试库的例子。

## 如何使用 React 测试库

默认情况下，使用 Create React App(或 CRA)创建的 React 应用程序已经包含了 React 测试库和 Jest。所以你需要做的就是编写你的测试代码。

如果您想在 CRA 应用程序之外使用 React 测试库，那么您需要用 NPM 手动安装 React 测试库和 Jest:

```
npm install --save-dev @testing-library/react jest
```

Installing React Testing Library and Jest

您需要安装 Jest，因为 React 测试库只提供帮助您编写测试脚本的方法。因此，您仍然需要一个 JavaScript 测试框架来运行测试代码。

您也可以使用其他测试框架，如 Mocha 或 Jasmine，但我将使用 Jest，因为它可以很好地与 React 和测试库一起工作。

在本教程中，我将使用默认模板，用 CRA 创建一个新的 React 应用程序:

```
npx create-react-app react-test-example
```

Create a new React app with CRA

一旦创建了应用程序，您应该已经在 src/文件夹中生成了一个`App.test.js`文件。该文件的内容如下:

```
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  render(<App />);
  const linkElement = screen.getByText(/learn react/i);
  expect(linkElement).toBeInTheDocument();
}); 
```

Default CRA test code

上面的测试代码使用 React 测试库的`render`方法来虚拟呈现从`App.js`文件导入的`App`组件，并将其附加到`document.body`节点。你可以通过`screen`对象访问渲染后的 HTML。

要查看`render()`调用的结果，可以使用`screen.debug()`方法:

```
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  render(<App />);
  screen.debug();
});
```

Debug the elements rendered by React Testing Library

然后打开你的终端，运行`npm run test`命令。您将看到整个`document.body`树呈现在您的控制台中:

```
<body>
  <div>
    <div class="App">
      <header class="App-header">
        <img alt="logo" class="App-logo" src="logo.svg" />
        <p>
          Edit<code> src/App.js </code>and save to reload.
        </p>
        <a
          class="App-link"
          href="https://reactjs.org"
          rel="noopener noreferrer"
          target="_blank"
        >
          Learn React
        </a>
      </header>
    </div>
  </div>
</body>
```

The document body rendered by React Testing Library

`screen`对象也已经绑定了 DOM 测试方法。这就是为什么上面的测试代码可以使用`screen.getByText()`通过其 **textContent** 值来查询锚`<a>`元素。

最后，测试代码将使用 Jest 中的`expect`方法断言链接元素在`document`对象中是否可用:

```
expect(linkElement).toBeInTheDocument();
```

Assert whether the link element is in the document

当没有找到 link 元素时，Jest 会将测试标记为**失败**。

## React 测试查找元素的库方法

大多数 React 测试用例应该使用方法来寻找元素。除了上面的`getByText()`方法之外，React 测试库还为您提供了几种通过特定属性查找元素的方法:

*   `getByText()`:根据元素的 textContent 值查找元素
*   `getByRole()`:根据其`role`属性值
*   `getByLabelText()`:根据其`label`属性值
*   `getByPlaceholderText()`:根据其`placeholder`属性值
*   `getByAltText()`:根据其`alt`属性值
*   `getByDisplayValue()`:根据其`value`属性，通常用于`<input>`元素
*   `getByTitle()`:根据其`title`属性值

当这些方法还不够时，您可以使用`getByTestId()`方法，该方法允许您通过元素的`data-testid`属性来查找元素:

```
import { render, screen } from '@testing-library/react';

render(<div data-testid="custom-element" />);
const element = screen.getByTestId('custom-element');
```

Get element by data-testid value

但是由于使用`data-testid`属性选择元素与真实用户使用应用程序的方式并不相似，所以文档建议您只在所有其他方法都无法找到您的元素时才使用它。一般来说，通过文本、角色或标签查找应该涵盖大多数情况。

## 如何用 React 测试库测试用户生成的事件

除了发现元素是否出现在文档主体中，React 测试库还可以帮助您测试用户生成的事件，比如单击按钮和在文本框中键入值。

`user-event`库是模拟用户浏览器交互的配套库。假设您有一个按钮组件，用于在亮主题和暗主题之间切换，如下所示:

```
import React, { useState } from "react";

function App() {
  const [theme, setTheme] = useState("light");

  const toggleTheme = () => {
    const nextTheme = theme === "light" ? "dark" : "light";
    setTheme(nextTheme);
  };

  return <button onClick={toggleTheme}>
      Current theme: {theme}
    </button>;
}

export default App; 
```

接下来，创建一个测试，通过使用`userEvent.click()`方法找到按钮并模拟一个点击事件。单击按钮后，您可以通过检查按钮元素文本是否包含“dark”来断言测试成功:

```
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import App from "./App";

test("Test theme button toggle", () => {
  render(<App />);
  const buttonEl = screen.getByText(/Current theme/i);

  userEvent.click(buttonEl);
  expect(buttonEl).toHaveTextContent(/dark/i);
});
```

Testing user click on a button and assert the content

这就是你如何用 React 测试库来模拟用户事件。`user-event`库还有几个其他的方法，比如用于双击元素的`dblClick`和用于在文本框中键入的`type`。你可以查看`user-event`库的[文档以获得更多信息。](https://testing-library.com/docs/ecosystem-user-event)

## 结论

> 你的测试越像你的软件被使用的方式，它们就越能给你信心。
> (来源:[肯特·C·多兹](https://twitter.com/kentcdodds/status/977018512689455106)，React 测试库作者)

真正的用户不会看到实现细节，比如 React 组件中当前的状态或属性。他们只能在浏览器上看到呈现的 HTML 元素。React 测试库鼓励您测试应用程序的行为，而不是实现细节。

通过以用户使用的方式测试您的应用程序，您可以确信当所有测试用例都通过时，您的应用程序将按预期运行。要了解更多信息，您可以访问 React 测试库的[文档。](https://testing-library.com/docs/react-testing-library/example-intro)

感谢您的阅读。我希望你从这篇文章中学到了一些新的东西。