# 如何让你的 React 应用程序通过一个定制的钩子做出响应

> 原文：<https://www.freecodecamp.org/news/make-react-apps-responsive/>

如何让您的 React 应用程序响应任何规模的设备？让我们看看如何通过定制 React 钩子来实现这一点。

在我的 React 站点的顶部是一个 Header 组件。随着页面尺寸的减小，我希望显示更少的链接:

![resizing window to show header](img/cef2e2014b1bc97197c1e1bc21902dce.png)

要做到这一点，我们可以使用 CSS 的媒体查询，或者我们可以使用一个自定义的 React 钩子来给出页面的当前大小，并在我们的 JSX 中隐藏或显示链接。

之前，我使用了一个名为`react-use`的 a 库中的钩子来添加这个功能。

然而，我没有带来整个第三方库，而是决定创建自己的钩子来提供窗口的尺寸，包括宽度和高度。我把这个钩子叫做`useWindowSize`。

> 有兴趣创建令人印象深刻的 React 应用程序，使用像这样的自定义 React 挂钩吗？看看 React 训练营。

## 如何创建自定义挂钩

首先，我们将创建一个新文件。js 在我们的 utilities (utils)文件夹中，与钩子`useWindowSize`同名，我将在导出自定义钩子时导入 React(以使用钩子)。

```
// utils/useWindowSize.js

import React from "react";

export default function useWindowSize() {} 
```

现在，因为我在一个 Gatsby 站点中使用它，这是服务器呈现的，所以我需要获得窗口的大小。但我们可能无法访问它，因为我们在服务器上。

为了检查并确保我们不在服务器上，我们可以查看类型`window`是否不等于字符串`undefined`。

在这种情况下，我们可以返回到浏览器的默认宽度和高度，比如说，对象中的 1200 和 800:

```
// utils/useWindowSize.js

import React from "react";

export default function useWindowSize() {
  if (typeof window !== "undefined") {
    return { width: 1200, height: 800 };
  }
} 
```

## 如何从窗口获得宽度和高度

假设我们在客户端上，并且可以获得窗口，我们可以通过与`window`交互来使用`useEffect`钩子执行一个副作用。我们将包含一个空的 dependencies 数组，以确保只在组件(调用这个钩子的组件)挂载后才调用 effect 函数。

为了找出窗口的宽度和高度，我们可以添加一个事件监听器并监听`resize`事件。每当浏览器大小改变时，我们可以更新一个状态(用`useState`创建)，我们称之为`windowSize`，更新它的设置者是`setWindowSize`。

```
// utils/useWindowSize.js

import React from "react";

export default function useWindowSize() {
  if (typeof window !== "undefined") {
    return { width: 1200, height: 800 };
  }

  const [windowSize, setWindowSize] = React.useState();

  React.useEffect(() => {
    window.addEventListener("resize", () => {
      setWindowSize({ width: window.innerWidth, height: window.innerHeight });
    });
  }, []);
} 
```

当调整窗口大小时，将调用回调函数，并且用当前窗口尺寸更新`windowSize`状态。为此，我们将宽度设为`window.innerWidth`，高度设为`window.innerHeight`。

## 如何添加 SSR 支持

然而，我们这里的代码将无法工作。这是因为钩子的一个关键规则是它们不能被有条件地调用。因此，在调用之前，我们不能在我们的`useState`或`useEffect`钩子上有条件。

因此，为了解决这个问题，我们将有条件地设置`useState`的初始值。我们将创建一个名为`isSSR`的变量，它将执行同样的检查来查看窗口是否不等于字符串`undefined`。

我们将使用一个三元组来设置宽度和高度，首先检查我们是否在服务器上。如果是，我们将使用默认值。如果没有，我们就用`window.innerWidth`和`window.innerHeight`。

```
// utils/useWindowSize.js

import React from "react";

export default function useWindowSize() {
  // if (typeof window !== "undefined") {
  // return { width: 1200, height: 800 };
  // }
  const isSSR = typeof window !== "undefined";
  const [windowSize, setWindowSize] = React.useState({
    width: isSSR ? 1200 : window.innerWidth,
    height: isSSR ? 800 : window.innerHeight,
  });

  React.useEffect(() => {
    window.addEventListener("resize", () => {
      setWindowSize({ width: window.innerWidth, height: window.innerHeight });
    });
  }, []);
} 
```

最后，我们需要考虑何时卸载组件。我们需要做什么？我们需要移除我们的 resize 监听器。

### 正在删除调整大小事件侦听器

你可以通过从 useEffectand 返回一个函数来实现，我们将使用`window.removeEventListener`删除监听器。

```
// utils/useWindowSize.js

import React from "react";

export default function useWindowSize() {
  // if (typeof window !== "undefined") {
  // return { width: 1200, height: 800 };
  // }
  const isSSR = typeof window !== "undefined";
  const [windowSize, setWindowSize] = React.useState({
    width: isSSR ? 1200 : window.innerWidth,
    height: isSSR ? 800 : window.innerHeight,
  });

  React.useEffect(() => {
    window.addEventListener("resize", () => {
      setWindowSize({ width: window.innerWidth, height: window.innerHeight });
    });

    return () => {
      window.removeEventListener("resize", () => {
        setWindowSize({ width: window.innerWidth, height: window.innerHeight });
      });
    };
  }, []);
} 
```

但是我们需要对同一个函数的引用，而不是我们这里的两个不同的函数。为此，我们将为两个监听器创建一个名为`changeWindowSize`的共享回调函数。

最后，在钩子的末尾，我们将返回我们的`windowSize`状态。仅此而已。

```
// utils/useWindowSize.js

import React from "react";

export default function useWindowSize() {
  const isSSR = typeof window !== "undefined";
  const [windowSize, setWindowSize] = React.useState({
    width: isSSR ? 1200 : window.innerWidth,
    height: isSSR ? 800 : window.innerHeight,
  });

  function changeWindowSize() {
    setWindowSize({ width: window.innerWidth, height: window.innerHeight });
  }

  React.useEffect(() => {
    window.addEventListener("resize", changeWindowSize);

    return () => {
      window.removeEventListener("resize", changeWindowSize);
    };
  }, []);

  return windowSize;
} 
```

## 如何使用钩子

要使用钩子，我们只需要在我们需要的地方导入它，调用它，并在我们想要隐藏或显示某些元素的地方使用宽度。

在我的情况下，这是在 500 像素的标志。在这里，我想隐藏所有其他链接，只显示“立即加入”按钮，就像您在上面的示例中看到的那样:

```
// components/StickyHeader.js

import React from "react";
import useWindowSize from "../utils/useWindowSize";

function StickyHeader() {
  const { width } = useWindowSize();

  return (
    <div>
      {/* visible only when window greater than 500px */}
      {width > 500 && (
        <>
          <div onClick={onTestimonialsClick} role="button">
            <span>Testimonials</span>
          </div>
          <div onClick={onPriceClick} role="button">
            <span>Price</span>
          </div>
          <div>
            <span onClick={onQuestionClick} role="button">
              Question?
            </span>
          </div>
        </>
      )}
      {/* visible at any window size */}
      <div>
        <span className="primary-button" onClick={onPriceClick} role="button">
          Join Now
        </span>
      </div>
    </div>
  );
} 
```

这个钩子可以在任何服务器渲染的 React 应用上工作，比如 Gatsby 和 Next.js。

## 喜欢这篇文章吗？加入 React 训练营

**[React 训练营](http://bit.ly/join-react-bootcamp)** 将你应该知道的关于学习 React 的一切打包成一个全面的包，包括视频、备忘单，外加特殊奖励。

获得数百名开发人员已经使用的内部信息，以掌握 React、找到他们梦想的工作并掌控他们的未来:

[![The React Bootcamp](img/8879fb8f279f64aae3696886c5d25bc4.png)](http://bit.ly/join-react-bootcamp) 
*打开时点击此处通知*