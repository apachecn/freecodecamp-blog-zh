# 如何用 Bootstrap4 和 ES6 创建自定义确认框

> 原文：<https://www.freecodecamp.org/news/custom-confirm-box-with-bootstrap-4-377aa67723c2/>

普拉尚·亚达夫

# 如何用 Bootstrap4 和 ES6 创建自定义确认框

![1*8tI8tEqNrAaaNkDAbXEQsw](img/90a812d0100021d93dd26bb2bb9ab027.png)

Image source: Pixabay

我们为实现一个好的设计付出了大量的汗水，但是想象一下这样一个场景，我们不得不使用浏览器默认的东西。它毁了我们的辛勤工作，因为它改变了浏览器。

同样的事情也发生在我身上:我花了很多精力来创建一个 SPA，但是客户想要一个带有业务逻辑的确认框。不幸的是，使用内置的确认框是设计中唯一奇怪的因素。所以我创建了一个自定义确认框。

让我们看看如何用 Bootstrap4 和 [ES6](https://learnersbucket.com/tutorials/es6/es6-intro/) 创建你自己的自定义确认框。

一个**模式**是一个弹出对话框，可用于 Bootstrap。我们可以使用它的功能来处理弹出窗口，并根据我们的需要来设计它。它有不同的方法，我们可以用来实现我们的目标。

![1*jrEUFkPpPS4MUkcTpS9Chw](img/f94a6c0a69a448f41cbba88e2be83c93.png)

Custom Confirm Box

点击查看现场演示[。](https://learnersbucket.com/examples/bootstrap4/custom-confirm-box-with-bootstrap/)

*   我们将使用 Bootstrap4 模式来创建弹出对话框。
*   我们将使用 Javascript *回调*函数来处理响应。
*   由于 Bootstrap 有一个 JQuery 依赖项，我们将把它用于事件。

### #Bootstrap 模态方法

。模式(选项):使用以下选项将内容激活为模式。

。modal("toggle "):根据当前状态显示或隐藏模式。

。modal("show "):打开模式。

。modal("hide "):隐藏模态。

### #引导模式选项

{ background:true 或 false，default: true}:在模式外部单击时，模式是否应该隐藏。

{ keyboard: true 或 false，默认值:true}:当按下键盘按钮时是否应该隐藏模式。

{ show: true 或 false，default: true}:在初始化时显示模式。

### #引导模式事件

。on(' show . bs . modal '):modal 即将显示但尚未显示时。

。on(' showed . bs . modal '):显示模态时。

。on(' hide.bs.modal '):当 modal 即将隐藏但尚未隐藏时。

。on(' hidden.bs.modal '):隐藏模式时。

#### 属国

使用 CDN 导入所有依赖项。

我们现在将创建一个函数，该函数将生成带有自定义消息的确认框，并返回用户选择的输出。

我们不需要模态的页眉和页脚。我们将只使用模态体，并在一个

#### 标签中显示我们的消息。然后，我们将在它下面创建两个按钮，使用不同的 cl `asses b` tn-ye `s and` btn-no 来处理响应。

#### 说明

*   我们创建了一个`function`，它接受一个`message`和一个回调`handler`。
*   在这里，我们用 JQuery 的`appendto`方法在`body`标签的末尾添加了模态。
*   在添加了模态之后，我们用 Bootstrap 的`modal`方法来触发或显示模态。我们还传递了两个参数`{backdrop: 'static', keyboard:false}`来防止模态隐藏键盘事件，比如按下 **ESC** 按钮或者在背景区域点击。
*   我们用 bootstrap 的`hidden.bs.modal`检查模态是否隐藏，然后用 JQuery 的`remove`方法删除模态。这将防止它每次都添加一个模态。

我们已经创建了一个函数来呈现和打开我们的自定义确认框，所以现在我们必须处理用户提供的响应。

我们将使用回调函数来处理响应。在 JavaScript 中，我们可以将函数作为参数传递给另一个函数并调用它。作为参数传递的函数称为回调函数。

### 处理响应

#### 说明

如果**是**或**否**按钮被按下，那么我们将`true`或`false`传递给回调函数并调用它。之后，我们隐藏模态。

我们可以在任何地方像这样调用我们的确认框。

点击查看现场演示[。](https://learnersbucket.com/examples/bootstrap4/custom-confirm-box-with-bootstrap/)

有了自定义的确认框，我们可以完全控制设计和一些功能。

如果你喜欢这篇文章，请给它一些？并分享一下！如果你有任何与此相关的问题，请随时问我。

*更多类似的和 Javascript 中的算法解决方案，请关注我的* [**Twitter**](https://twitter.com/LearnersBucket) *。*我写关于 [ES6](https://learnersbucket.com/tutorials/es6/es6-intro/) ，React，Nodejs，[数据结构](https://learnersbucket.com/tutorials/topics/data-structures/)，以及[算法](https://learnersbucket.com/examples/topics/algorithms/)关于[*learnersbucket.com*](https://learnersbucket.com/)*。*