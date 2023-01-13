# 如何用 JavaScript 将文本复制到剪贴板

> 原文：<https://www.freecodecamp.org/news/copy-text-to-clipboard-javascript/>

当您构建高级网页和应用程序时，有时会希望添加复制功能。这使得用户只需点击一个按钮或图标来复制文本，而不是高亮显示文本并点击键盘上的几个按钮。

当有人需要复制激活码、恢复密钥、代码片段等时，通常会使用此功能。您还可以添加一些功能，比如在屏幕上显示警告或文本(可以是模态的)，通知用户文本已经被复制到他们的剪贴板上。

以前，您可以用`document.execCommand()`命令来处理这个问题，但是这个方法已经过时了。您现在可以使用剪贴板 API，它允许您响应剪贴板命令(剪切、复制和粘贴),并异步读取和**写入**系统剪贴板。

在本文中，您将学习如何使用剪贴板 API 将文本和图像写入(复制)到剪贴板。

如果你很急，这里是代码:

```
<p id="myText">Hello World</p>
<button class="btn" onclick="copyContent()">Copy!</button>

<script>
  let text = document.getElementById('myText').innerHTML;
  const copyContent = async () => {
    try {
      await navigator.clipboard.writeText(text);
      console.log('Content copied to clipboard');
    } catch (err) {
      console.error('Failed to copy: ', err);
    }
  }
</script> 
```

如果您不着急，让我们了解更多关于剪贴板 API 的知识，并看看它如何与一个演示项目一起工作。

## 如何检查浏览器的权限

重要的是要知道剪贴板 API 只支持通过 HTTPS 提供的页面。在尝试写入剪贴板之前，您还应该检查浏览器权限，以验证您是否具有写权限。您可以使用`navigator.permissions`查询来实现这一点:

```
navigator.permissions.query({ name: "write-on-clipboard" }).then((result) => {
  if (result.state == "granted" || result.state == "prompt") {
    alert("Write access granted!");
  }
}); 
```

## 如何将文本复制到剪贴板

为了用新的*剪贴板 API* 复制文本，您将使用异步的`writeText()`方法。这个方法只接受一个参数——要复制到剪贴板的文本。这可以是字符串、保存变量和其他字符串的模板文本，或者用于保存字符串的变量。

因为这个方法是异步的，所以它返回一个承诺。如果剪贴板已成功更新，此承诺将被解析，否则将被拒绝:

```
navigator.clipboard.writeText("This is the text to be copied").then(() => {
  console.log('Content copied to clipboard');
  /* Resolved - text copied to clipboard successfully */
},() => {
  console.error('Failed to copy');
  /* Rejected - text failed to copy to the clipboard */
}); 
```

您还可以在 try/catch 旁边使用 async/await:

```
async function copyContent() {
  try {
    await navigator.clipboard.writeText('This is the text to be copied');
    console.log('Content copied to clipboard');
    /* Resolved - text copied to clipboard successfully */
  } catch (err) {
    console.error('Failed to copy: ', err);
    /* Rejected - text failed to copy to the clipboard */
  }
} 
```

### 将文本复制到剪贴板示例

这里有一个演示，用一个真实的例子展示了它是如何工作的。在这个例子中，我们从公共报价 API 获取报价。然后，当您单击复制图标时，报价及其作者被复制，这表明您可以调整您复制到`writeText()`方法中的内容。

见 [CodePen](https://codepen.io) 上 Olawanle Joel([@ olawanlejoel](https://codepen.io/olawanlejoel))的笔[抄文 JS](https://codepen.io/olawanlejoel/pen/MWGLXyX) 。