# 用简单的英语解释浏览器事件

> 原文：<https://www.freecodecamp.org/news/javascript-events-explained-in-simple-english/>

## 什么是浏览器事件？

一个[事件](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events)指的是在你正在编程的系统中发生的一个动作或事件。然后，系统会通知您该事件，以便您可以在必要时以某种方式做出响应。

在本文中，我将关注 web 浏览器环境中的事件。从本质上讲，事件是一个指示器，它显示某个动作已经发生，以便您可以做出适当的响应。

为了说明我所说的，让我们想象一下，你正站在一个人行横道上，等待交通灯改变，以便你可以安全地过马路。事件是交通灯的变化，这使你随后采取行动——在这种情况下，是过马路。

类似地，在 web 开发中，每当我们感兴趣的事件发生时，我们都可以采取行动。

您在 web 开发中可能遇到的一些常见事件包括:

1.  鼠标事件

*   `click`
*   `dblclick`
*   `mousemove`
*   `mouseover`
*   `mousewheel`
*   `mouseout`
*   `contextmenu`
*   `mousedown`
*   `mouseup`

2.触摸事件

*   `touchstart`
*   `touchmove`
*   `touchend`
*   `touchcancel`

3.键盘事件

*   `keydown`
*   `keypress`
*   `keyup`

4.表单事件

*   `focus`
*   `blur`
*   `change`
*   `submit`

5.窗口事件

*   `scroll`
*   `resize`
*   `hashchange`
*   `load`
*   `unload`

要获得事件的完整列表以及它们所属的不同类别，您可以查看 [MDN 文档](https://developer.mozilla.org/en-US/docs/Web/Events)。列出的一些事件是官方规范中的标准事件，而另一些是特定浏览器内部使用的事件。

## 什么是事件处理程序？

如上所述，我们监视事件，以便每当我们收到事件已经发生的通知时，程序可以采取适当的行动。

这个动作通常在被称为**事件处理程序**的函数中执行，这些函数也被称为**事件监听器**。如果事件发生并且事件处理程序被调用，我们说事件已经被注册。下面的代码说明了这一点。

如果点击了带有`id` `btn`的按钮，则调用事件处理程序，并显示文本为“Button has clicked”的警告。`onclick`属性已经被分配给一个函数，它是事件处理程序。这是向 DOM 元素添加事件处理程序的三种方式之一。

```
const button = document.getElementById("btn");
button.onclick = function(){
   alert("Button has been clicked");
} 
```

值得指出的是，**事件处理程序**大多声明为函数，但也可以是对象。

## 如何分配事件处理程序

将事件处理程序附加到 HTML 元素有多种方式。我们将在下面讨论这些方法，以及它们的优缺点。

### 分配带有 HTML 属性的事件处理程序

这是将事件处理程序附加到 HTML 元素的最简单的方法，尽管这是最不推荐的方法。它包括使用名为`on<event>`的内联 HTML 事件属性，其值是事件处理程序。比如`onclick`、`onchange`、`onsubmit`等等。

注意，名为`onClick`、`onChange`或`onSubmit`的 HTML 事件属性并不少见，因为 HTML 属性不区分大小写。本质上使用`onclick`、`onClick`或`ONCLICK`在句法上是正确的。但是通常的做法是用小写字母。

```
<button onclick = "alert('Hello world!')"> Click Me </button>
<button onclick = "(() => alert('Hello World!'))()"> Click Me Too </button>
<button onclick = "(function(){alert('Hello World!')})()"> And Me </button> 
```

在上面的例子中，JavaScript 代码被分配给 HTML 事件属性。

注意最后两个`button`元素中立即调用的函数表达式(IIFE)格式。虽然这看起来简单明了，但是分配内联 HTML 事件属性是低效的，并且难以维护。

假设您的标记中有超过 20 个这样的按钮。为每个按钮编写相同的 JavaScript 代码是重复的。最好将 JavaScript 写在它自己的文件中，这样你就可以轻松地将相同的代码用于多个 HTML 文件。

此外，您不能内嵌多行 JavaScript 代码。由于上述原因，内联 JavaScript 代码被认为是一种反模式。所以尽量避免它，除非你正在尝试一些快速的东西。

### 在`script`标签中声明一个事件处理程序

除了以上所述，您还可以在一个`script`标签中声明事件处理程序，并以内联方式调用它，如下所示。

```
<!DOCTYPE html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="stylesheet" href="./index.css" type="text/css" />
    <script>
      function onClickHandler(){
         alert("Hello world!");
       }
    </script> 
  </head>
  <body>
    <div class="wrapper">
       <button onclick = "onClickHandler()"> Click me </button>
    </div>
  </body>
</html> 
```

但是请注意，简单地将函数名指定为 HTML 事件属性的值(如`onclick = "onClickHandler"`)是行不通的。您需要如上所示调用它，就像任何 HTML 属性的值一样，用引号将调用括起来。

### 使用 DOM 属性分配事件处理程序

除了使用上面说明的内联 HTML 事件属性，还可以将事件处理程序指定为 DOM 元素上的事件属性值。这只有在`script`标签或 JavaScript 文件中才有可能。

这种方法的一个限制是，同一个事件不能有多个事件处理程序。如下图所示，如果同一个事件有多个处理程序，那么只会应用最后一个。其他的将被覆盖。

```
const button = document.getElementById("btn");
button.onclick = function(){
   alert("Button has been clicked");
}
// Only this is applied
button.onclick = function(){
   console.log("Button has been clicked");
} 
```

如果您想从`onclick`事件中移除事件监听器，您可以简单地将`button.onclick`重新分配给`null`。

```
button.onclick = null 
```

### 如何改进添加事件侦听器的 DOM 方法

上述添加事件监听器的方法比使用内联 JavaScript 更可取。但是，它有一个限制，即一个元素的每个事件只能有一个事件处理程序。

例如，不能对一个元素上的 click 事件应用多个事件处理程序。

为了弥补这一缺陷，引入了`addEventListener`和`removeEventListener`。这使您能够为同一元素上的同一事件添加多个事件处理程序。

```
const button = document.getElementById('btn');
button.addEventListener('click', () => {
  alert('Hello World');
})
button.addEventListener('click', () => {
  console.log('Hello World');
}) 
```

在上面的代码中，选择了一个带有`id` `btn`的元素，然后通过附加两个事件处理程序来监视一个`click`事件。将调用第一个事件处理程序，并弹出`Hello World`的警告消息。随后`Hello World`也会被登录到控制台。

从上面的例子中你可能已经注意到了，`element.addEventListener`的函数签名是:

```
element.addEventListener(event, eventHandler, [optional parameter]) 
```

#### `addEventListener`方法的参数

1.  **事件**

第一个参数`event`(这是一个必需的参数)是一个指定事件名称的字符串。比如`"click"`、`"mouseover"`、`"mouseout"`等等。

2.**事件处理程序**

与第一个参数一样，第二个参数也是必需的，它是一个在事件发生时调用的函数。事件对象作为其第一个参数传递。事件对象取决于事件的类型。例如，为点击事件传递一个`MouseEvent`对象。

3.**可选参数**

第三个参数是可选参数，是一个具有以下属性的对象:

*   `once`:其值为布尔值。如果`true`，监听器触发后被移除。
*   `capture`:它的值也是一个布尔值。它设置处理事件的阶段，可以是冒泡阶段，也可以是捕获阶段。默认值为`false`，因此事件在冒泡阶段被捕获。你可以在这里了解更多关于[的信息。由于历史原因，选项也可以是`true`或`false`。](https://javascript.info/bubbling-and-capturing)
*   `passive`:它的值也是一个布尔值。如果是`true`，那么处理程序不会调用`preventDefault()`。`preventDefault()`是事件对象的一种方法。

同样，如果您想停止监控`click`事件，您可以使用`element.removeEventListener`。但是这只在事件监听器已经使用`element.addEventListener`注册的情况下才有效。功能签名与`element.addEventListener`类似。

```
element.removeEventListener(event, eventHandler, [options]) 
```

为了让我们使用`element.removeEventListener`删除一个`event`，在添加事件监听器时，作为第二个参数传递给`element.addEventListener`的函数必须是一个命名函数。这确保了如果我们想移除它，同样的函数可以传递给`element.removeEventListener`。

这里值得一提的是，如果您将可选参数传递给事件处理程序，那么您也必须将相同的可选参数传递给`removeEventListener`。

```
const button = document.getElementById('btn');
button.removeEventListener('click', clickHandler) 
```

## 什么是事件对象？

事件处理程序有一个名为**事件对象**的参数，它保存了关于事件的附加信息。

存储在**事件对象**中的信息取决于事件的类型。例如，传递给`click`事件处理程序的**事件对象**有一个名为`target`的属性，该属性引用了产生点击事件的元素。

在下面的例子中，如果你点击带有`id` `btn`的元素，`event.target`将引用它。所有的点击事件处理程序都被传递了一个带有`target`属性的**事件对象**。正如已经指出的，不同的事件具有存储不同信息的**事件对象**参数。

```
const button = document.getElementById("btn");
button.addEventListener("click", event => {
  console.log(event.target);
}) 
```

## `this`的值

在`event`处理程序中，`this`的值是事件处理程序注册的元素。请注意，注册事件处理程序的元素不一定与发生事件的元素相同。

例如，在下面的代码中，事件处理程序在包装器上注册。正常情况下，`this`的值与`event.currentTarget`相同。如果你点击了`button`,`onClickHandler`中`this`的值是`div`而不是`button`，因为是`div`注册了事件处理程序，尽管点击来自于按钮。

这叫`event propagation`。这是一个非常重要的概念，如果你感兴趣的话，你可以在这里读到这个概念。

```
<!DOCTYPE html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="stylesheet" href="./index.css" type="text/css" />
    <script>
      function onClickHandler(){
         console.log(this)
         alert("Hello world!");
       }
       const wrapper = document.querySelector(".wrapper");
       wrapper.addEventListener("click", onClickHandler);
    </script> 
  </head>
  <body>
    <div class="wrapper">
       <button> Click me </button>
    </div>
  </body>
</html> 
```

## 结论

在本文中，我们了解了:

*   浏览器事件和它们是什么
*   向 DOM 元素添加事件处理程序的不同方式
*   事件处理程序的事件对象参数
*   事件处理程序中`this`的值