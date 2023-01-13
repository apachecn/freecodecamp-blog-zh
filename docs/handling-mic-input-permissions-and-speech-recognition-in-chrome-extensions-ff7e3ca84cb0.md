# 如何处理 Chrome 扩展中的麦克风输入权限和语音识别

> 原文：<https://www.freecodecamp.org/news/handling-mic-input-permissions-and-speech-recognition-in-chrome-extensions-ff7e3ca84cb0/>

用帕拉希塔尼亚语说

# 如何处理 Chrome 扩展中的麦克风输入权限和语音识别

![1*kCWG1pjJyxUfKwblYn1gKA](img/b758eec83555bbf87ed81628fe7e0b61.png)

本教程假设你对 Chrome 扩展和相关的配置文件有基本的了解。如果没有，那么你可以参考[这篇文章](https://medium.freecodecamp.org/how-to-create-a-chrome-extension-part-1-ad2a3a77541)其中设置了本教程的文件。

在 Chrome 扩展中设置麦克风录音权限是一个灰色地带。没有官方记录的方法来做到这一点，开发者被迫使用“黑客”来获得他们的 Chrome 扩展的 mic 权限。

在这篇短文中，我们使用`options.html`页面获得麦克风权限，并使用流行的`[annyang.js](https://github.com/TalAter/annyang)` [库](https://github.com/TalAter/annyang)来检测用户的语音。

### 1.创建权限触发器脚本和页面

为了绕过 Chrome 的限制，我们使用扩展中的一个网页，如`options.html`和`popup.html`来获得整个扩展的 mic 权限。

首先，我们需要创建一个 JavaScript 文件来获得 web 页面上的 mic 权限。我创建了一个最小文件，请求 chrome 允许我使用麦克风。

```
$(document).ready(function(){
    navigator.mediaDevices.getUserMedia({audio: true})
});
```

如果您不喜欢使用 JQuery，那么您可以将这段 JS 代码嵌入到 HTML 页面文件的末尾，用作权限触发器。

对于我们的扩展 TalkToMe，我们使用`options.html`作为权限触发页面。

### 2.自动打开触发页面

只有在当前浏览器会话中打开触发页面时，mic 权限弹出窗口才会打开。让用户每次都手动打开它是不好的 UX。我们创建了一个背景脚本来解决这个问题。

```
setTimeout(() => {
    navigator.mediaDevices.getUserMedia({ audio: true })
    .catch(function() {
        chrome.tabs.create({
            url: chrome.extension.getURL("options.html"),
            selected: true
        })
    });
}, 100);
```

它每 100 毫秒尝试访问一次麦克风，如果请求被 Chrome 拒绝，就会打开权限触发页面。

为了让脚本工作，您需要将它和其他后台脚本一起添加到您的`manifest.json`中。

```
... 

"background": {
    "scripts": [
      ..
      "js/auto-trigger.js", // add the script here
      ..
    ],
    "persistent": false
  },

...
```

### 3.为语音识别连接 Annyang

Annyang 提供了一个用于语音识别的多功能库，并且 100%免费使用。

它工作在基于命令的结构上，因为它根据用户的语音调用函数。这个特性使它非常适合 TalkToMe。

您可以将`annyang.js`代码添加到后台脚本中并开始使用它。在这里，我向你们展示了文档中的 Hello World 示例。

```
if (annyang) {
  // Let's define a command.
  var commands = {
    'hello': function() { alert('Hello world!'); }
  };

  // Add our commands to annyang
  annyang.addCommands(commands);

  // Start listening.
  annyang.start();
}
```

就像你复制了我们几个小时的搜索 StackOverflow 为你的 chrome 扩展添加 mic 权限一样。

如果本教程对你有所帮助，我们真的希望你能对我们的扩展功能 [TalkToMe](https://chrome.google.com/webstore/detail/talktome/nefaaifpggpfdjlfhfbcgfcjimlgpocc) 给出你的想法，即使你可能不是一个视力受损的用户。