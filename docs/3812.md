# JavaScript DOM 事件:Onclick 和 Onload

> 原文：<https://www.freecodecamp.org/news/javascript-dom-events-onclick-and-onload/>

在互联网的早期，网页是真正静态的——只有文本和图像。当然，有时那个图像是一个动画 gif，但它仍然只是一个图像。

随着 JavaScript 的出现，创建响应点击按钮或滚动动画等动作的交互式页面变得越来越可能。

在 JavaScript 中可以监听许多 DOM(文档对象模型)事件，但是`onclick`和`onload`是最常见的。

## **Onclick 事件**

JavaScript 中的`onclick`事件允许您在单击元素时执行一个函数。

### **例子**

```
<button onclick="myFunction()">Click me</button>

<script>
  function myFunction() {
    alert('Button was clicked!');
  }
</script>
```

在上面的简单例子中，当用户点击按钮时，他们会在浏览器中看到一个显示`Button was clicked!`的警告。

### **动态添加`onclick`**

上面的例子是可行的，但是通常被认为是不好的做法。相反，最好将页面内容(HTML)与逻辑(JS)分开。

为此，可以使用以下示例中的代码以编程方式将`onclick`事件添加到任何元素中:

```
<p id="foo">click on this element.</p>

<script>
  const p = document.getElementById("foo"); // Find the paragraph element in the page
  p.onclick = showAlert; // Add onclick function to element

  function showAlert(event) {
    alert("onclick Event triggered!");
  }
</script>
```

### **注**

值得注意的是，使用`onclick`我们可以只添加一个监听器函数。如果您想添加更多，只需使用`addEventListener()`，这是添加事件的首选方式。

在上面的例子中，当用户点击`html`中的`paragraph`元素时，他们会看到一个显示`onclick Event triggered`的警告。

### **防止默认动作**

然而，如果我们将`onclick`附加到链接(HTML 的`a`标签)上，我们可能希望阻止默认动作发生:

```
<a id="bar" href="https://guide.freecodecamp.org">Guides</a>

<script>
  const link = document.getElementById("bar"); //  Find the link element
  link.onclick = myAlert; // Add onclick function to element

  function myAlert(event) {
    event.preventDefault();
    alert("Link was clicked but page was not open");
  }
</script>
```

在上面的例子中，我们使用`onclick`回调函数中的`event.preventDefault()`来防止`a`元素(打开链接)的默认行为。

## **加载事件**

`onload`事件用于在页面加载后立即执行 JavaScript 函数。

### **举例:**

```
const body = document.body;
body.onload = myFunction;

function myFunction() {
  alert('Page finished loading');
} 
```

可以简称为:

```
document.body.onload = function() {
  alert('Page finished loading');
} 
```

在上面的例子中，一旦网页加载完毕，就会调用`myFunction`函数，向用户显示`Page finished loading`警告。

`onload`事件通常附属于`<body>`元素。然后一旦页面的`<body>`加载完毕，包括所有的图片、CSS 和 JS 文件，你的脚本就会运行。

#### **更多信息:**

这些只是可以用 JavaScript 操纵的众多 DOM 事件中的两个，但却是最常用的。

但是有时候你根本不需要监听 DOM 事件，而是想使用一个基于时间的事件，比如倒计时。关于计时事件的快速教程，请查看本文。