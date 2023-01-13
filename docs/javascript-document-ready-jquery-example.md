# JavaScript Document . Ready()–文档就绪 JS 和 jQuery 示例

> 原文：<https://www.freecodecamp.org/news/javascript-document-ready-jquery-example/>

当使用 JavaScript 和文档对象模型(DOM)时，您可能希望脚本只在 DOM 加载后运行。

您可以使用 jQuery 中的`$(document).ready()`方法，或者普通 JavaScript 中的`DOMContentLoaded`事件来实现这一点。

在本文中，您将了解如何让您的 JavaScript 代码仅在使用 jQuery 和普通 JavaScript 加载 DOM 时运行。

## 如何在 jQuery 中使用`$(document).ready()`方法

在浏览器中运行 JavaScript 之前，它会等待文档内容的加载。这包括样式表、图像等等。

按照惯例，将 script 元素放在结束的 body 标记之前会使脚本在运行之前等待文档的其余部分加载。

我们还可以通过使用`$(document).ready()`方法在 jQuery 中加快这个过程。`$(document).ready()`方法只等待 DOM 加载，它不等待样式表、图像和 iframes。

这里有一个例子:

```
$(document).ready(function () {
  console.log("Hello World!");
});
```

在上面的代码中，`$(document).ready()`方法只会在 DOM 加载后运行。所以你只会看到“你好，世界！”在控制台中`$(document).ready()`方法已经开始运行后。

总之，您可以在`$(document).ready()`方法中编写所有的 jQuery 代码。这样，您的代码将在运行前等待 DOM 加载。

## 如何在 JavaScript 中使用`DOMContentLoaded`事件

就像 jQuery 的`$(document).ready()`方法一样，一旦 DOM 加载完毕，就会触发`DOMContentLoaded`事件——它不会等待样式表和图像。

下面是如何使用`DOMContentLoaded`事件:

```
document.addEventListener("DOMContentLoaded", () => {
  console.log("Hello World!");
});
```

一旦 DOM 加载完毕，`DOMContentLoaded`事件将检测到它并运行。

在以下情况下，您应该使用`DOMContentLoaded`事件:

*   您的网页中有一些功能应该立即启动，而不需要等待页面内容的其余部分。
*   在 head 元素中放置了一个脚本标记。

## 摘要

在本文中，我们讨论了 jQuery 中的`$(document).ready()`方法，以及普通 JavaScript 中的`DOMContentLoaded`事件。

当 DOM 加载后，我们用它们来执行 JavaScript 代码。

这些功能的有趣之处在于，它们让 JavaScript 代码运行，而无需等待图像和样式表完全加载到网页中。

编码快乐！