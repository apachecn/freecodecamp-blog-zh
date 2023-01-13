# JavaScript Onclick 事件解释

> 原文：<https://www.freecodecamp.org/news/javascript-onclick-event-explained/>

JavaScript 中的`onclick`事件让程序员在点击一个元素时执行一个函数。

## 按钮 Onclick 示例

```
<button onclick="myFunction()">Click me</button>

<script>
  function myFunction() {
    alert('Button was clicked!');
  }
</script>
```

在上面的简单例子中，当用户点击按钮时，他们会在浏览器中看到一个显示`Button was clicked!`的警告。

## 动态添加 onclick

还可以使用以下示例中的代码以编程方式将`onclick`事件添加到任何元素中:

```
<p id="foo">click on this element.</p>

<script>
  var p = document.getElementById("foo"); // Find the paragraph element in the page
  p.onclick = showAlert; // Add onclick function to element

  function showAlert(event) {
    alert("onclick Event triggered!");
  }
</script>
```

### **注**

需要注意的是，使用 onclick 我们只能添加一个侦听器函数。如果您想添加更多，只需使用 addEventListener()，这是添加事件侦听器的首选方式。

在上面的例子中，当用户点击`html`中的`paragraph`元素时，他们会看到一个显示`onclick Event triggered`的警告。

## 防止默认操作

然而，如果我们将`onclick`附加到链接(HTML 的`a`标签)上，我们可能希望阻止默认动作发生:

```
<a href="https://guide.freecodecamp.org" onclick="myAlert()">Guides</a>

<script>
  function myAlert(event) {
    event.preventDefault();
    alert("Link was clicked but page was not open");
  }
</script>
```

在上面的例子中，我们使用`onclick`回调函数中的`event.preventDefault()`来防止`a`元素(打开链接)的默认行为。

[MDN](https://developer.mozilla.org/en-US/docs/Web/API/GlobalEventHandlers/onclick)

### 其他资源

[jQuery。on()事件处理程序附件](https://api.jquery.com/on/)