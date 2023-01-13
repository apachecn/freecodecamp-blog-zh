# 如何用 CSS 和 JavaScript 制作自定义鼠标光标

> 原文：<https://www.freecodecamp.org/news/how-to-make-a-custom-mouse-cursor-with-css-and-javascript/>

你是否曾经访问过一个网站，并完全被其惊人的功能所震撼？其中之一可能是一个很酷的鼠标光标，它不同于你习惯的常规箭头或指针光标。

这确实可以改善用户体验，最近我一直在想它是如何工作的。所以我开始做一些研究，我发现它是如何做到的。

在这篇文章中，我将解释如何自定义鼠标光标。到本文结束时，你将学会如何用 CSS 和 JavaScript 这两种不同的方法制作这些光标。然后你就可以用不同的创意光标来充实你的网站，以吸引你的观众。准备好了吗？让我们开始吧。

## 先决条件

本文适合初学者，但是要理解一些概念，您应该具备以下基本知识:

*   超文本标记语言
*   基本 CSS
*   基本 JavaScript

## 如何用 CSS 自定义鼠标光标

用 CSS 定制鼠标光标非常简单，因为 CSS 已经有了一个属性来处理这个问题。我们需要做的就是识别这个属性并使用它。

作为前端工程师，我们经常使用这个属性——它就是全能的`cursor`属性。是的，正是这个属性赋予了我们定制光标的能力。

在我们开始实际的例子之前，让我们看看与 CSS `cursor`属性相关的值。虽然大多数开发人员只使用几个重要的，但我们应该关注更多。

[https://codepen.io/developeraspire5/embed/preview/XWeBEXo?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=XWeBEXo](https://codepen.io/developeraspire5/embed/preview/XWeBEXo?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=XWeBEXo)

A visual representation of all the CSS cursors.

从上面的代码片段和结果中，您可以通过将鼠标光标悬停在包含每个 CSS `cursor`属性值名称的每个框上来查看和测试 CSS 拥有的不同鼠标光标。

现在，我如何使用 CSS 自定义鼠标光标？要使用它，您只需告诉 CSS 您打算使用什么图像，并使用`url`值将光标属性指向图像 URL。

```
body {
  cursor: url('image-path.png'),auto;
}
```

从上面的代码片段中，您可以看到我在文档主体上设置了这个，因此无论光标移动到哪里，它都适用于光标。它具有在`url()`中指定的图像。

该属性的下一个值是一个后备值，以防图像由于一些内部故障而无法加载或找不到。我敢肯定，你不会希望你的网站是“无光标的”，所以添加一个后备是非常重要的。您也可以添加尽可能多的回退 URL。

```
body {
  cursor: url('image-path.png'), url('image-path-2.svg), 
          url('image-path-3.jpeg'), auto;
}
```

您也可以在网页的特定元素或部分自定光标。下面是一个 CodePen 示例:

[https://codepen.io/developeraspire5/embed/preview/GRMBxWN?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=GRMBxWN](https://codepen.io/developeraspire5/embed/preview/GRMBxWN?default-tabs=css%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=GRMBxWN)

Using different customized cursor on different element on a webpage.

这就是 CSS 中自定义光标的全部内容。现在让我们看看 JavaScript 是如何做到这一点的。

## 如何用 JavaScript 制作自定义鼠标光标

要用 JavaScript 实现这一点，您需要操纵 DOM 来获得想要的结果。

首先，让我们看看 HTML:

### HTML

```
<div class="cursor rounded"></div>
<div class="cursor pointed"><div>
```

根据上面的代码片段，我创建了两个`divs`来表示光标。计划是从 JavaScript 操纵这些 div，以便 JavaScript `mousemove`事件使用鼠标移动的 X 和 Y 坐标滚动它们在网页上的移动。

现在让我们来看 CSS 部分，它将一点一点地变得有意义。

### 如何用 CSS 设计自定义光标的样式

```
body{
  background-color: #1D1E22;
  cursor: none;
}

.rounded{
  width: 30px;
  height: 30px;
  border: 2px solid #fff;
  border-radius: 50%;
}

.pointed{
  width: 7px;
  height: 7px;
  background-color: white;
  border-radius: 50%;
}
```

看看上面的 CSS 代码，我禁用了光标(还记得`cursor:none`？).这将使光标变得不可见，只允许我们的自定义光标显示。

我给它们设计了一个独特的“类似光标”的外观。你绝对可以用它做更多的事情，也许添加一个背景图像，表情符号，贴纸等等，只要有图像。现在，让我们来看看 JavaScript

### 如何使用 JavaScript 让光标移动

```
const cursorRounded = document.querySelector('.rounded');
const cursorPointed = document.querySelector('.pointed');

const moveCursor = (e)=> {
  const mouseY = e.clientY;
  const mouseX = e.clientX;

  cursorRounded.style.transform = `translate3d(${mouseX}px, ${mouseY}px, 0)`;

  cursorPointed.style.transform = `translate3d(${mouseX}px, ${mouseY}px, 0)`;

}

window.addEventListener('mousemove', moveCursor)
```

我在全局窗口对象上添加了一个事件监听器来监听任何鼠标移动。当鼠标移动时，`moveCursor`函数表达式被调用，它接收事件对象作为参数。有了这个参数，我可以在页面上的任何一点获得鼠标的 X 和 Y 坐标。

我已经用 JavaScript `querySelector`从 DOM 中选择了每一种光标。所以我所要做的就是通过用`translate3d`值控制样式上的转换属性，根据鼠标的 X 和 Y 坐标移动它们。这将使 div 能够在鼠标移动到网页上的任何点时移动。

你看到的反斜线叫做模板文字。这使得编写变量很容易将它们附加到字符串中。另一种方法是将变量连接到字符串上。

简单吧？就是这样！

以下是 CodePen 示例和上述代码片段的结果:

[https://codepen.io/developeraspire5/embed/preview/gOGjeZG?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=gOGjeZG](https://codepen.io/developeraspire5/embed/preview/gOGjeZG?default-tabs=js%2Cresult&height=300&host=https%3A%2F%2Fcodepen.io&slug-hash=gOGjeZG)

Custom mouse cursor using JavaScript.

## 哪种方法效果最好？

现在，作为开发人员，您可以选择最适合您的方法。如果你想使用一些漂亮的表情符号或图像作为光标，你可以选择使用 CSS。另一方面，您可能希望使用 JavaScript，这样您就可以定制自己选择的复杂形状，并制作光标移动的动画。

任何一种方式都可以，只要你得到你想要的结果，让所有的访问者惊叹。

我希望你能从这篇文章中学到很多，并且我期待着看到你用这些知识做些什么。

更多 CSS 技巧，请关注我的 Twitter。

感谢阅读，下次见。