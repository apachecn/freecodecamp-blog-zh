# 如何构建 JavaScript 警告框或弹出窗口

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-javascript-alert-box-or-popup-window/>

弹出框(或对话框)是模态窗口，用于通知或警告用户，或从用户处获取输入。

弹出框阻止用户访问程序的其他方面，直到弹出框被关闭，所以它们不应该被过度使用。

JavaScript 中使用了三种不同的弹出方法: [window.alert()](https://developer.mozilla.org/en-US/docs/Web/API/Window/alert) 、 [window.confirm()](https://developer.mozilla.org/en-US/docs/Web/API/Window/confirm) 和 [window.prompt()](https://developer.mozilla.org/en-US/docs/Web/API/Window/prompt) 。

### **警报**

[警告方法](https://developer.mozilla.org/en-US/docs/Web/API/Window/alert)显示不需要用户输入响应的消息。调用此函数后，将出现一个带有指定(可选)消息的警告对话框。在警报消失之前，用户需要确认消息。

### **举例:**

`window.alert("Welcome to our website");`

![MDN Alert Example](img/f024b64e5884140bc23895610f901ba1.png)

### **确认**

[确认方法](https://developer.mozilla.org/en-US/docs/Web/API/Window/confirm)与`window.alert()`类似，但也在弹出窗口中显示一个取消按钮。按钮返回布尔值:true 表示确定，false 表示取消。

### **举例:**

```
var result = window.confirm('Are you sure?');
if (result === true) {
    window.alert('Okay, if you're sure.');
} else { 
    window.alert('You seem uncertain.');
}
```

![MDN Confirm Example](img/1d107134f075ce1fb5aedf4206b43a60.png)

### **提示**

[提示方法](https://developer.mozilla.org/en-US/docs/Web/API/Window/prompt)通常用于获取用户输入的文本。该函数可以接受两个参数，这两个参数都是可选的:显示给用户的消息和显示在文本字段中的默认值。

### **举例:**

`var age = prompt('How old are you?', '100');`

![MDN Prompt Example](img/8c122df64399742a0f9c1734f665dcfc.png)

### **其他设计选项:**

如果您对默认的 JavaScript 弹出窗口不满意，您可以在各种 UI 库中替换。例如，SweetAlert 为标准 JavaScript 模态提供了一个很好的替代品。您可以通过 CDN(内容交付网络)将它包含在您的 HTML 中并开始使用。

```
<script src="https://unpkg.com/sweetalert/dist/sweetalert.min.js"></script>
```

语法是这样的:`swal(title, subtitle, messageType)`

```
swal("Oops!", "Something went wrong on the page!", "error");
```

上述代码将产生以下弹出窗口:

![SweetAlert Example](img/011464898bd96d18a81a615992acba49.png)

SweetAlert 绝不是标准模态的唯一替代品，但它干净且易于实现。

#### **更多信息:**

*   [MDN window.alert()](https://developer.mozilla.org/en-US/docs/Web/API/Window/alert)
*   [MDN window.confirm()](https://developer.mozilla.org/en-US/docs/Web/API/Window/confirm)
*   [MDN window.prompt()](https://developer.mozilla.org/en-US/docs/Web/API/Window/prompt)