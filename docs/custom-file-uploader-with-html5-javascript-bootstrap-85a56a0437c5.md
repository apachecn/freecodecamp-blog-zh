# 如何使用 HTML5、JavaScript 和 Bootstrap 构建自定义文件上传程序

> 原文：<https://www.freecodecamp.org/news/custom-file-uploader-with-html5-javascript-bootstrap-85a56a0437c5/>

普拉尚·亚达夫

# 如何使用 HTML5、JavaScript 和 Bootstrap 构建自定义文件上传程序

![1*B6zah6pnLHvaYJ1Gppp-hQ](img/b79ef5fd43fd675da3f3c192e33e257d.png)

Pixabay

在这篇短文中，我们将学习如何使用 JQuery、 [ES6](https://learnersbucket.com/tutorials/es6/es6-intro) 和 Bootstrap4 创建自定义文件上传程序。

我们将创建一个自定义设计的文件上传器和一个选项来预览选定的文件和删除它们。

*支持我看这篇文章[这里](https://learnersbucket.com/examples/bootstrap4/custom-fileuploader-in-javascript/)。*

### 演示

点击查看现场演示[。](https://codepen.io/Learnersbucket/pen/drWENz)

### 履行

*   我们将使用 html5 文件上传程序来上传文件。
*   然后，在 Bootstrap popover 的帮助下，我们将预览选定的文件。
*   在预览文件时，我们将提供一个选项来删除选定的文件。
*   由于 JQuery 是 Bootstrap popover 的依赖项之一，我们将使用它来简化我们的工作。

### 属国

### 文件上传程序的 HTML 布局

说明

*   我们已经创建了一个名为`custom-file-picker`的容器。
*   在这里，我们有自定义文件上传`picture-container`和 popover 预览器`popover-container`。
*   每个文件拾取器都有一个唯一的 id `a8755cf0-f4d1-6376-ee21-a6defd1e7c08`，其对应的弹出器引用该 id `data-target="a8755cf0-f4d1-6376-ee21-a6defd1e7c08"`来预览文件。

### 设计我们的组件

### 处理功能

既然我们已经设计了组件的样式，现在是处理功能的时候了。我们将使用 Jquery 和 [ES6](https://learnersbucket.com/tutorials/es6/es6-intro) 来简化事情。

### 存储文件

我们将创建一个全局变量来存储文件。

我们将使用这个变量，借助它的 id 来存储相应文件选择器的所有文件。

现在我们将创建一个管理文件存储和显示文件数量的函数。该函数将`id`和`array of files`作为输入。

`$(`[data-id="${id}"] > .file-total-viewer`).text(files.lengt`h)；将更新 popover 预览器中的文件数。

### 处理文件挑选

我们已经准备好更新计数和存储文件的函数。一旦文件被选择或更改，我们就将数据传递给这个函数。

一旦文件被选中，我们将用 SVG 显示完整的动画来通知用户文件已被更改。

现在，我们已经存储了文件，并且可以看到计数。让我们用引导弹出窗口创建文件预览器。

Bootstrap 为我们提供了一种动态生成弹出窗口内容的方法。所以我们将 popover 连接到`[data-toggle="popover"]`。点击了解更多信息。

#### 它是如何工作的

*   每次弹出窗口将要呈现时，它将使用其`[data-target]` id 并从`fileStorage`中提取所有文件。
*   如果有文件，那么使用删除按钮呈现这些文件。
*   如果没有文件，那么显示一些消息。

现在，如果您有多个文件上传器，并且希望一次只打开一个 popover，请添加以下代码。

如果您选择某个文件并点击`view`，您应该能够查看它。现在，我们要做的最后一件事是处理文件的删除。

### 删除文件

我们已经通过`data-target`向删除按钮提供了文件拾取器的 id，并通过`data-name`提供了文件名。每次点击删除图标时，我们将使用这些值来删除文件。

因为我们是动态生成 popover 的内容，并且它还不存在于 DOM 中，所以我们不能给它分配一个事件。所以我们必须使用一种变通方法，在 DOM 上分配一个事件，并检查是否用`$(document).on('click', '.popover-content-remove', function (e) {});`点击了删除图标。

#### 它是如何工作的

*   单击删除图标后，我们将要求用户确认。
*   如果用户想要继续，那么我们通过`data-target`和`data-name`获取分配给删除按钮的 id 和名称。
*   我们使用 filter()方法删除该特定文件。
*   一旦文件从数组中移除，我们就通过将值传递给助手函数`storeFile(id, newArr);`来更新它的计数。
*   此外，我们从 popover 中移除元素。如果数组是空的，那么显示一些信息。

注意:您应该为每个文件选取器及其弹出预览器提供一个唯一的 id。

### 完全码

如果你喜欢这篇文章，请给它 50+？并分享一下！如果你有任何与此相关的问题，请随时问我。

*更多类似的和 Javascript 中的算法解决方案，请关注我的* [**Twitter**](https://twitter.com/LearnersBucket) *。*我写关于 [ES6](https://learnersbucket.com/tutorials/es6/es6-intro/) ，React，Nodejs，[数据结构](https://learnersbucket.com/tutorials/topics/data-structures/)，以及[算法](https://learnersbucket.com/examples/topics/algorithms/)关于[*learnersbucket.com*](https://learnersbucket.com/)*。*