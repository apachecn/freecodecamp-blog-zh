# JavaScript 事件处理程序——如何在 JS 中处理事件

> 原文：<https://www.freecodecamp.org/news/javascript-event-handlers/>

## 什么是事件？

事件是当用户与页面交互时发生的操作，比如单击元素、在字段中键入内容或加载页面。

浏览器通知系统发生了一些事情，需要处理。它通过注册一个名为`event handler`的函数来处理，这个函数监听特定类型的事件。

## “处理一个事件”是什么意思？

简单地说，考虑一下这个——让我们假设你有兴趣参加你当地社区的网络开发聚会活动。

为此，你可以注册一个名为“女性编码”的本地聚会，并订阅通知。这样，每当安排新的会议时，您都会收到提醒。这就是事件处理！

这里的“事件”是一个新的 JS meetup。当一个新的聚会被发布时，网站 meetup.com 捕捉到这个变化，从而“处理”这个事件。然后它会通知您，从而对事件采取“行动”。

在浏览器中，事件的处理方式类似。浏览器检测到变化，并提醒正在监听特定事件的函数(事件处理程序)。然后，这些函数根据需要执行操作。

让我们看一个`click`事件处理程序的例子:

```
<div class="buttons">
  <button>Press 1</button>
  <button>Press 2</button>
  <button>Press 3</button>
</div>
const buttonContainer = document.querySelector('.buttons');
console.log('buttonContainer', buttonContainer);

buttonContainer.addEventListener('click', event => {
  console.log(event.target.value)
}) 
```

## 有哪些不同类型的事件？

每当用户与页面交互时，都会触发一个事件。这些事件可以是用户滚动页面、点击项目或加载页面。

下面是一些常见的事件-`onclick``dblclick``mousedown``mouseup``mousemove``keydown``keyup``touchmove``touchstart``touchend``onload``onfocus``onblur``onerror` `onscroll`

## 事件的不同阶段-捕获、目标、泡沫

当一个事件在 DOM 中移动时——无论是向上冒泡还是向下流淌——这被称为事件传播。事件通过 DOM 树传播。

事件分两个阶段发生:冒泡阶段和捕获阶段。

在捕获阶段，也称为涓滴阶段，事件“涓滴”到引起事件的元素。

它从根级别的元素和处理程序开始，然后向下传播到元素。当事件到达`target`时，捕获阶段完成。

在冒泡阶段，事件“冒泡”到 DOM 树。它首先被最内层的处理程序(离发生事件的元素最近的处理程序)捕获和处理。然后它向上冒泡(或向上传播)到 DOM 树的更高层，进一步向上到它的父节点，最后到它的根节点。

这是帮助你记住这一点的诀窍:

```
trickle down, bubble up 
```

这里有一张来自 [quirksmode](https://www.quirksmode.org/js/events_order.html) 的信息图很好地解释了这一点:

```
 / \
---------------| |-----------------
| element1     | |                |
|   -----------| |-----------     |
|   |element2  | |          |     |
|   -------------------------     |
|        Event BUBBLING           |
-----------------------------------

               | |
---------------| |-----------------
| element1     | |                |
|   -----------| |-----------     |
|   |element2  \ /          |     |
|   -------------------------     |
|        Event CAPTURING          |
----------------------------------- 
```

需要注意的一点是，无论您在哪个阶段注册事件处理程序，两个阶段都会发生。默认情况下，所有事件都会冒泡。

您可以使用函数`addEventListener(type, listener, useCapture)`为冒泡或捕获阶段注册事件处理程序。如果`useCapture`设置为`false`，则事件处理程序处于冒泡阶段。否则就处于捕获阶段。

事件阶段的顺序取决于浏览器。

要检查哪个浏览器首先支持捕获，您可以在 JSfiddle 中尝试以下代码:

```
<div id="child-one">
    <h1>
      Child One
    </h1>
  </div> 
```

```
const childOne = document.getElementById("child-one");

const childOneHandler = () => {
console.log('Captured on child one')
}

const childOneHandlerCatch = () => {
console.log('Captured on child one in capture phase')
}

childOne.addEventListener("click", childOneHandler); 
childOne.addEventListener("click", childOneHandlerCatch, true); 
```

在 Firefox、Safari 和 Chrome 中，输出如下:
![Events in capture phase are fired first](img/bf458cf949896d81a60899c25639d7d5.png)

## 如何收听活动

收听事件有两种方式:

1.  `addEventListener`
2.  内联事件，如`onclick`

```
//addEventListener
document.getElementByTag('a').addEventListener('click', onClickHandler);

//inline using onclick
<a href="#" onclick="onClickHandler">Click me</a> 
```

## 内联事件和`addEventListener`哪个更好？

1.  `addEventListener`让你能够注册无限的事件处理程序。
2.  `removeEventListener`也可以用来删除事件处理程序
3.  `useCapture`标志可用于指示事件是需要在捕获阶段还是捆绑阶段进行处理。

## 代码示例和真人表演

您可以在 JSFiddle 中尝试这些事件并加以利用。

```
<div id="wrapper-div">
  <div id="child-one">
    <h1>
      Child One
    </h1>
  </div>
  <div id="child-two" onclick="childTwoHandler">
    <h1>
      Child Two
    </h1>
  </div>

</div> 
```

```
const wrapperDiv = document.getElementById("wrapper-div");
const childOne = document.getElementById("child-one");
const childTwo = document.getElementById("child-two");

const childOneHandler = () => {
console.log('Captured on child one')
}

const childTwoHandler = () => {
console.log('Captured on child two')
}

const wrapperDivHandler = () => {
console.log('Captured on wrapper div')
}

const childOneHandlerCatch = () => {
console.log('Captured on child one in capture phase')
}

const childTwoHandlerCatch = () => {
console.log('Captured on child two in capture phase')
}

const wrapperDivHandlerCatch = () => {
console.log('Captured on wrapper div in capture phase')
}

childOne.addEventListener("click", childOneHandler); 
childTwo.addEventListener("click", childTwoHandler); 
wrapperDiv.addEventListener("click", wrapperDivHandler); 

childOne.addEventListener("click", childOneHandlerCatch, true); 
childTwo.addEventListener("click", childTwoHandlerCatch, true); 
wrapperDiv.addEventListener("click", wrapperDivHandlerCatch, true); 
```

## TL；速度三角形定位法(dead reckoning)

事件阶段是捕获(DOM ->目标)、冒泡(目标-> DOM)和目标。
事件可以通过使用`addEventListener`或者像`onclick`这样的内嵌方法来监听。

```
 addEventListener can add multiple events, whereas with onclick this cannot be done.
    onclick can be added as an HTML attribute, whereas an addEventListener can only be added within <script> elements.
    addEventListener can take a third argument which can stop the event propagation. 
```

## 进一步阅读

[https://www.quirksmode.org/js/events_order.html](https://www.quirksmode.org/js/events_order.html)
https://jsfiddle.net/r2bc6axg/T5[https://stack overflow . com/questions/6348494/addevent listener-vs-onclick](https://stackoverflow.com/questions/6348494/addeventlistener-vs-onclick)
[https://www . w3 . org/wiki/HTML/Attributes/_ Global # Event-handler _ Attributes](https://www.w3.org/wiki/HTML/Attributes/_Global#Event-handler_Attributes)

为了跟上更多类似的简短教程，[注册我的时事通讯](https://tinyletter.com/shrutikapoor)或者[在 Twitter 上关注我](https://twitter.com/shrutikapoor08)