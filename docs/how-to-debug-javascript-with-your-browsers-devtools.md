# 如何用浏览器的开发工具调试 JavaScript

> 原文：<https://www.freecodecamp.org/news/how-to-debug-javascript-with-your-browsers-devtools/>

作为一名开发人员，您可能经常想要调试代码。您可能已经在一些挑战中使用了`console.log`，这是最简单的调试方法。

在这篇文章中，我们将告诉你一些最酷的技巧，使用浏览器自带的调试工具进行调试。

## **FreeCodeCamp 代码编辑器简介:**

在进入调试之前，让我们泄露一些关于 FCC 的*令人敬畏的代码检查引擎*的秘密事实。

我们使用定制的 [CodeMirror](http://codemirror.net/mode/javascript/index.html) ，作为代码编辑器。一个 [`eval()`函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/eval)用于评估编辑器中表示为字符串的 JavaScript 代码。当调用`eval()`时，浏览器将本机执行您的代码。在本文的后面部分，我们将了解为什么这个秘密如此重要。

## **现在我们来看一下技巧:**

## **谷歌 Chrome 开发工具**

谷歌 Chrome 是最受欢迎的浏览器之一，内置了一个名为 [V8](https://developers.google.com/v8/) 的 JavaScript 引擎，并为开发人员提供了一个名为 [Chrome DevTools](https://developer.chrome.com/devtools) 的强大工具集。强烈推荐访问他们的[完整的 JavaScript 调试指南](https://developer.chrome.com/devtools/docs/javascript-debugging)。

### **1:开发工具的基础知识**

#### **启动 Chrome 开发工具**

点击`F12`

。或者，您可以按

`Ctrl` + `Shift` + `I`

在 Windows 和 Linux 上或

`Cmd` + `Shift` + `I`

在 Mac 上，或者如果你只是喜欢你的鼠标，去`Menu > More Tools > Developer Tools`。

#### **了解`Sources`和`console`选项卡。**

这两个选项卡可能会成为您调试时最好的朋友。`Sources`选项卡可用于显示您正在访问的网页上的所有脚本。该选项卡包含代码窗口、文件树、调用堆栈&监视窗口等部分。

`console`选项卡是所有日志输出的位置。此外，您可以使用控制台选项卡的提示来执行 JavaScript 代码。它与 Windows 上的命令提示符或 Linux 上的终端同义。

*提示:使用`Esc`键随时在 DevTools 中切换控制台。*

### **2:常用快捷方式和功能**

虽然您可以访问快捷方式的[完整列表，但以下是一些最常用的快捷方式:](https://developers.google.com/web/tools/chrome-devtools/iterate/inspect-styles/shortcuts)

功能 Windows，Linux Mac
搜索关键字，支持 regex。`Ctrl`+`F``Cmd`+`F`+
搜索并打开一个文件`Ctrl`+`P``Cmd`+`P`+
跳转到第`Ctrl`+`G`+`:line_no``Cmd`+`G`+`:line_no`+
行添加一个断点`Ctrl` + `B`，或者点击第`Cmd` + `B`行，或者点击第
行暂停/恢复脚本执行`F8` `F8`
跳过下一个函数调用`F10`

### **3:为我们的代码使用源地图**

你会喜欢的最酷的功能之一是[通过](https://developer.chrome.com/devtools/docs/javascript-debugging#breakpoints-dynamic-javascript)[源地图](https://developer.chrome.com/devtools/docs/javascript-debugging#source-maps)动态调试动态脚本。

每个脚本都可以在 DevTools 的 Source 选项卡中可视化。source 选项卡包含所有的 JavaScript 源文件。但是代码编辑器中的代码是通过浏览器进程中的一个容器中的`eval()`执行的，这个容器被简单地称为虚拟机(VM)。

你可能已经猜到了，我们的代码实际上是一个没有文件名的脚本。因此我们在 Source 选项卡中看不到它。

![:sparkles:](img/b96b147b7758edcbffa3d02bee1043fe.png ":sparkles:")

*****黑客来了！*****

![:sparkles:](img/b96b147b7758edcbffa3d02bee1043fe.png ":sparkles:")

我们必须利用`Source Maps`从我们的编辑器中为 JavaScript 指定一个名称。这很简单:

假设我们正在进行[阶乘](https://www.freecodecamp.com/challenges/factorialize-a-number)挑战，我们的代码如下所示:

```
function factorialize(num) {
  if(num <= 1){
  ...
}
factorialize(5);
```

我们需要做的就是将`//# sourceURL=factorialize.js`添加到代码的顶部，即第一行:

```
//# sourceURL=factorialize.js

function factorialize(num) {
  if(num <= 1){
  ...
```

![:sparkles:](img/b96b147b7758edcbffa3d02bee1043fe.png ":sparkles:")

*****和尤里卡就是这样！*****

![:sparkles:](img/b96b147b7758edcbffa3d02bee1043fe.png ":sparkles:")

现在打开 DevTools 并搜索文件名。添加断点，调试和享受！

## 关于调试的更多信息:

*   [虫子挤压初学者指南](https://www.freecodecamp.org/news/the-beginner-bug-squashing-guide/)
*   [初学者调试技巧和窍门](https://www.freecodecamp.org/news/debugging-javascript-for-beginners-5d4ac15dd1cd/)
*   [如何充分利用浏览器的调试控制台](https://www.freecodecamp.org/news/how-to-go-beyond-console-log-and-get-the-most-out-of-your-browsers-debugging-console-e185256a1115/)