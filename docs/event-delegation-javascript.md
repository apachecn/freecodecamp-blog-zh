# JavaScript 中的事件委托——用一个例子解释

> 原文：<https://www.freecodecamp.org/news/event-delegation-javascript/>

事件委托是一种基于[事件冒泡](https://www.freecodecamp.org/news/event-bubbling-in-javascript/)概念的模式。这是一种事件处理模式，允许您在 DOM 树中的更高层次上处理事件，而不是在第一次接收事件的层次上。

如果你喜欢这个话题，有一个[视频版本。](https://www.youtube.com/watch?v=aZ3JWv0ofuA)

## 事件传播简介

默认情况下，元素上触发的事件会沿着 DOM 树向上传播到元素的父元素、祖先元素，直到根元素(`html`)。

看看这个例子:

```
<div>
  <span>
    <button>Click Me!</button>
  </span>
</div> 
```

这里我们有一个`div`，它是一个`span`的父元素，而后者又是一个`button`元素的父元素。

由于事件冒泡，当按钮接收到一个事件，比如说**点击**，该事件就会在树中冒泡，所以`span`和`div`也会分别接收到该事件。

如果你想更详细地了解事件冒泡，你可以在这里阅读我的文章。

## 事件委托是如何工作的？

通过事件委托，您可以在`div`上处理点击事件，而不是在`button`上处理。

其思想是您"**将事件的处理委托给不同的元素(在本例中是 div，它是一个父元素)而不是接收事件的实际元素(按钮)。**

这就是我在代码中的意思:

```
const div = document.getElementsByTagName("div")[0]

div.addEventListener("click", (event) => {
  if(event.target.tagName === 'BUTTON') {
    console.log("button was clicked")
  }
}) 
```

`event`对象有一个`target`属性，它包含关于实际接收事件的元素的信息。在`target.tagName`上，我们获取元素标签的名称，并检查它是否是**按钮**。

使用这段代码，当您单击`button`时，事件会冒泡到处理该事件的`div`。

## 事件委托的好处

事件委托是一种有用的模式，它允许您编写更简洁的代码，并创建更少的具有相似逻辑的事件侦听器。

我这么说是什么意思？看看这段代码:

```
<div>
  <button>Button 1</button>
  <button>Button 2</button>
  <button>Button 3</button>
</div> 
```

这里我们有 3 个按钮。假设我们想要处理每个按钮上的单击事件，这样当它被单击时，按钮的文本被记录到控制台。我们可以这样实现:

```
const buttons = document.querySelectorAll('button')

buttons.forEach(button => {
  button.addEventListener("click", (event) => {
    console.log(event.target.innerText)
  })
}) 
```

我在这里使用`querySelectorAll`,因为它返回一个`NodeList`,我可以使用`forEach`方法对其进行处理。`getElementsByTagName`返回一个没有`forEach`方法的`HTMLCollection`。

当您点击第一个按钮时，您已经将“按钮 1”登录到控制台。对于第二个按钮，“按钮 2”被记录，对于第三个按钮，“按钮 3”被记录。

虽然这正如我们所希望的那样工作，但是我们已经为三个按钮创建了三个事件侦听器。

因为这些按钮上的点击事件在 DOM 树中向上传播，所以我们可以使用一个公共的父节点或祖先节点来处理事件。在这种情况下，我们委托一个他们共享的公共父节点来处理我们想要的逻辑。

方法如下:

```
const div = document.querySelector('div')

div.addEventListener("click", (event) => {
  if(event.target.tagName === 'BUTTON') {
    console.log(event.target.innerText)
  }
}) 
```

现在，我们只有一个事件侦听器，但逻辑是相同的:当您单击第一个按钮时，“Button 1”将被记录到控制台，其他按钮也是如此。

即使我们像这样增加一个额外的按钮:

```
<div>
  <button>Button 1</button>
  <button>Button 2</button>
  <button>Button 3</button>
  <button>Button 4</button>
</div> 
```

我们不需要修改 JavaScript 代码，因为这个新按钮也与其他按钮共享`div`父按钮(我们委托它来处理事件)。

## 包扎

使用事件委托，您可以创建更少的事件侦听器，并在一个位置执行类似的基于事件的逻辑。这使您可以更容易地添加和移除元素，而不必添加新的或移除现有的事件侦听器。

由于 DOM 中的事件传播，事件委托是可能的，其中子元素接收的事件也被传递给子元素的父元素和祖先元素。同样，你可以[在这里](https://www.freecodecamp.org/news/event-bubbling-in-javascript/)阅读更多关于事件冒泡的信息。

感谢您的阅读，祝您编码愉快！