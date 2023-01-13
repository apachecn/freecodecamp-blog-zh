# addEventListener()方法–JavaScript 事件监听器示例代码

> 原文：<https://www.freecodecamp.org/news/javascript-addeventlistener-example-code/>

JavaScript addEventListener()方法允许您设置在特定事件发生时调用的函数，比如当用户单击按钮时。本教程向您展示了如何在代码中实现 addEventListener()。

## 了解事件和事件处理程序

**事件**是用户或浏览器操作页面时发生的动作。它们扮演着重要的角色，因为它们可以导致网页的元素动态变化。

例如，当浏览器加载完一个文档时，就会发生一个`load`事件。如果用户点击页面上的按钮，那么就发生了一个`click`事件。

许多事件可能发生一次、多次或永远不会发生。您也可能不知道事件将在何时发生，尤其是如果它是用户生成的。

在这些场景中，您需要一个**事件处理程序**来检测事件何时发生。通过这种方式，您可以设置代码，以便在事件发生时即时做出反应。

JavaScript 以`addEventListener()`方法的形式提供了一个事件处理程序。此处理程序可以附加到您希望监视其事件的特定 HTML 元素，并且该元素可以附加多个处理程序。

## addEventListener()语法

下面是语法:

```
target.addEventListener(event, function, useCapture) 
```

*   **target** :您希望添加事件处理程序的 HTML 元素。该元素作为文档对象模型(DOM)的一部分存在，您可能希望了解一下[如何选择 DOM 元素](https://1000mileworld.com/dom-manipulation-using-javascript/#select)。
*   **事件**:指定事件名称的字符串。我们已经提到了`load`和`click`事件。出于好奇，这里有一份 [HTML DOM 事件](https://www.w3schools.com/jsref/dom_obj_event.asp)的完整列表。
*   **功能**:指定检测到事件时运行的功能。这就是能让你的网页动态变化的魔力。
*   **useCapture** :可选布尔值(真或假)，指定事件是否应该在[捕获或冒泡阶段](https://javascript.info/bubbling-and-capturing)执行。对于带有附加事件处理程序的嵌套 HTML 元素(如`div`中的`img`),该值决定了哪个事件先被执行。默认情况下，它被设置为 false，这意味着首先执行最内层的 HTML 事件处理程序(冒泡阶段)。

## addEventListener()代码示例

[https://codepen.io/1000mileworld/embed/preview/JjGVRjw?height=300&slug-hash=JjGVRjw&default-tabs=css,result&host=https://codepen.io](https://codepen.io/1000mileworld/embed/preview/JjGVRjw?height=300&slug-hash=JjGVRjw&default-tabs=css,result&host=https://codepen.io)

Simple example demonstrating addEventListener()

这是我做的一个简单的例子，向你展示了它的作用。

当用户单击该按钮时，会显示一条消息。另一个按钮点击隐藏消息。下面是相关的 JavaScript:

```
let button = document.querySelector('#button');
let msg = document.querySelector('#message');

button.addEventListener('click', ()=>{
  msg.classList.toggle('reveal');
}) 
```

按照前面显示的`addEventListener()`的语法:

*   **目标**:带有`id='button'`的 HTML 元素
*   **函数**:匿名(箭头)函数，设置显示/隐藏消息所需的代码
*   **使用捕捉**:左侧为`false`的默认值

我的函数能够通过添加/删除一个名为“reveal”的 CSS 类来显示/隐藏消息，这个 CSS 类改变了消息元素的可见性。

当然，在您的代码中，可以随意定制这个函数。您也可以用自己的命名函数替换匿名函数。

## 将事件作为参数传递

有时我们可能想知道关于事件的更多信息，比如点击了什么元素。在这种情况下，我们需要向函数传递一个事件参数。

此示例显示了如何获取元素的 id:

```
button.addEventListener('click', (e)=>{
  console.log(e.target.id)
}) 
```

在这里，事件参数是一个名为`e`的变量，但它也可以很容易地被称为任何其他名称，比如“event”。此参数是一个对象，包含有关事件的各种信息，如目标 id。

您不需要做任何特殊的事情，当您以这种方式向函数传递参数时，JavaScript 会自动知道该做什么。

## 移除事件处理程序

如果出于某种原因，您不想再激活某个事件处理程序，以下是删除它的方法:

```
target.removeEventListener(event, function, useCapture); 
```

参数同`addEventListener()`。

## 熟能生巧

更好地使用事件处理程序的最好方法是用自己的代码来练习。

这是我做的一个[示例项目](https://1000mileworld.com/Portfolio/Flex/flexPanels.html)，其中我使用了事件处理程序来改变在网页背景上流动的气泡的颜色、大小和速度(你需要点击中央面板来显示控件)。

玩得开心点，去做些很棒的东西吧！

想了解更多初学者友好的 web 开发知识，请访问我在 [1000 英里世界](https://1000mileworld.com/)的博客，并在 [Twitter](https://twitter.com/1000mileworld) 上关注我。