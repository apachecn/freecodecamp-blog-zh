# JavaScript DOM 简介

> 原文：<https://www.freecodecamp.org/news/an-introduction-to-the-javascript-dom-512463dd62ec/>

Javascript DOM(文档对象模型)是一个允许开发者操作网站内容、结构和风格的接口。在本文中，我们将学习什么是 DOM，以及如何使用 Javascript 操作它。本文也可以作为基本 DOM 操作的参考。

### 这只狗是什么

在最基本的层面上，一个网站由一个 HTML 和 CSS 文档组成。浏览器创建文档的表示，称为文档对象模型(DOM)。这个文档使 Javascript 能够访问和操作网站的元素和样式。该模型建立在对象的树形结构中，并定义了:

*   作为对象的 HTML 元素
*   HTML 元素的属性和事件
*   方法来访问 HTML 元素

![g42eKZ-RmFNQVN5EZ1lF2wj67VqIdX7DMk4Z](img/599080da3b4336c44e8a1ba00de2e62c.png)

HTML DOM model

元素的位置被称为节点。不仅元素获得节点，元素和文本的属性也获得它们自己的节点(属性节点和文本节点)。

### DOM 文档

DOM 文档是网页中所有其他对象的所有者。这意味着，如果你想访问网页上的任何对象，你必须从文档开始。它还包含许多重要的属性和方法，使我们能够访问和修改我们的网站。

### 查找 HTML 元素

现在我们已经了解了什么是 DOM 文档，我们可以开始获取我们的第一个 HTML 元素了。使用 Javascript DOM 有许多不同的方法，下面是最常见的方法:

#### 按 ID 获取元素

*getElementById()* 方法用于根据 Id 获取单个元素。让我们看一个例子:

```
var title = document.getElementById(‘header-title’);
```

在这里，我们获取带有 header-title id 的元素，并将其保存到一个变量中。

#### 通过类名获取元素

我们还可以使用返回元素数组的 *getElementsByClassName()* 方法来获取多个对象。

```
var items = document.getElementsByClassName(‘list-items’);
```

在这里，我们获取所有带有类 *list-items* 的条目，并将它们保存到一个变量中。

#### getelementsbytagname

我们还可以使用 *getElementsByTagName()* 方法通过标记名来获取元素。

```
var listItems = document.getElementsByTagName(‘li’);
```

在这里，我们获取 HTML 文档的所有元素，并将它们保存到一个变量中。

#### 查询选择器

方法返回第一个匹配指定的 *CSS 选择器的元素。*这意味着您可以通过 id、类、标签和所有其他有效的 CSS 选择器来获取元素。这里我只是列出了几个最受欢迎的选择。

**按 id 获取:**

```
var header = document.querySelector(‘#header’)
```

**按类获取:**

```
var items = document.querySelector(‘.list-items’)
```

**按标签获取:**

```
var headings = document.querySelector(‘h1’);
```

**获取更多具体元素:**

我们还可以使用 *CSS 选择器*获得更多特定的元素。

```
document.querySelector(“h1.heading”);
```

在这个例子中，我们同时搜索一个标签和一个类，并返回通过 CSS 选择器的第一个元素。

#### Queryselectorall

*querySelectorAll()* 方法与 *querySelector()* 方法完全相同，只是它返回符合 CSS 选择器的所有元素。

```
var heading = document.querySelectorAll(‘h1.heading’);
```

在这个例子中，我们获取了所有具有类别*标题*的 *h1* 标签，并将它们存储在一个数组中。

### 更改 HTML 元素

HTML DOM 允许我们通过改变 HTML 元素的属性来改变它的内容和样式。

#### 更改 HTML

innerHTML 属性可用于更改 HTML 元素的内容。

```
document.getElementById(“#header”).innerHTML = “Hello World!”;
```

在本例中，我们获得了 id 为 header 的元素，并将内部内容设置为“Hello World！”。

InnerHTML 也可以用于将标签放入另一个标签中。

```
document.getElementsByTagName("div").innerHTML = "<h1>Hello World!</h1>"
```

这里我们将一个 h1 标签放入所有已经存在的 div 中。

#### **改变属性值**

您还可以使用 DOM 更改属性的值。

```
document.getElementsByTag(“img”).src = “test.jpg”;
```

在本例中，我们将所有 img/>t*ag 的 src 更改为 te*st.jpg。

#### 改变风格

要改变 HTML 元素的样式，我们需要改变元素的样式属性。以下是更改样式的语法示例:

```
document.getElementById(id).style.property = new style
```

现在让我们来看一个例子，在这个例子中，我们得到一个元素，并将下边框改为黑色实线:

```
document.getElementsByTag(“h1”).style.borderBottom = “solid 3px #000”;
```

css 属性需要用 camelcase 编写，而不是普通的 CSS 属性名。在这个例子中，我们使用 borderBottom 代替 border-bottom。

### 添加和删除元素

现在我们来看看如何添加新元素和删除现有元素。

#### 添加元素

```
var div = document.createElement(‘div’);
```

这里我们只是使用 *createElement()* 方法创建一个 div 元素，该方法将一个标记名作为参数，并将其保存到一个变量中。之后，我们只需要给它一些内容，然后将其插入到我们的 DOM 文档中。

```
var newContent = document.createTextNode("Hello World!"); 
div.appendChild(newContent);
document.body.insertBefore(div, currentDiv);
```

这里，我们使用 createTextNode()方法创建内容，该方法将一个字符串作为参数，然后在文档中已经存在的 div 之前插入新的 div 元素。

#### 删除元素

```
var elem = document.querySelector('#header');
elem.parentNode.removeChild(elem);
```

这里我们获得一个元素，并使用 removeChild()方法删除它。

#### 替换元素

现在，我们来看看如何替换物品。

```
var div = document.querySelector('#div');
var newDiv = document.createElement(‘div’);
newDiv.innerHTML = "Hello World2"
div.parentNode.replaceChild(newDiv, div);
```

这里我们使用 *replaceChild()* 方法替换一个元素。第一个参数是新元素，第二个参数是我们想要替换的元素。

#### 直接写入 HTML 输出流

我们还可以使用 write()方法将 HTML 表达式和 JavaScript 直接写入 HTML 输出流。

```
document.write(“<h1>Hello World!</h1><p>This is a paragraph!</p>”);
```

我们也可以像传递日期对象一样传递 JavaScript 表达式。

```
document.write(Date());
```

write()方法还可以接受多个参数，这些参数将按照出现的顺序附加到文档中。

### 事件处理程序

HTML DOM 还允许 Javascript 对 HTML 事件做出反应。我在这里列出了一些最重要的例子:

*   鼠标点击
*   页面加载
*   鼠标移动
*   输入字段更改

#### 分配事件

您可以使用标签上的属性直接在 HTML 代码中定义事件。下面是一个 *onclick* 事件的例子:

```
<h1 onclick=”this.innerHTML = ‘Hello!’”>Click me!</h1>
```

在这个例子中，

# 的文本将变成“Hello！”当你点击按钮时。

您还可以在事件被触发时调用函数，如下例所示。

```
<h1 onclick=”changeText(this)”>Click me!</h1>
```

这里，当按钮被点击时，我们调用 *changeText()* 方法，并将元素作为属性传递。

我们也可以在 Javascript 代码中分配相同的事件。

```
document.getElementById(“btn”).onclick = changeText();
```

#### 分配事件监听器

现在让我们看看如何将事件侦听器分配给 HTML 元素。

```
document.getElementById(“btn”)addEventListener('click', runEvent);
```

这里，我们刚刚分配了一个 clickevent，当我们的 btn 元素被单击时，它调用 runEvent 方法。

您也可以将多个事件分配给单个元素:

```
document.getElementById(“btn”)addEventListener('mouseover', runEvent);
```

### 节点关系

DOM 文档中的节点相互之间有层次关系。这意味着节点的结构像一棵树。我们使用术语父、兄弟和子来描述节点之间的关系。

顶层节点称为根节点，是唯一没有父节点的节点。普通 HTML 文档中的根是标签，因为它没有父标签，是文档的顶部标签。

#### 在节点间导航

我们可以使用这些属性在节点之间导航:

*   parentNode
*   子节点
*   第一个孩子
*   最后一个孩子
*   下一个兄弟姐妹

下面是一个如何获取 h1 的父元素的例子。

```
var parent = document.getElementById(“heading”).parentNode
```

### 结论

你一直坚持到最后！希望这篇文章能帮助你理解 Javascript DOM 以及如何使用它来操作你的网站上的元素。

如果你想阅读更多像这篇文章一样的文章，你可以访问我的[网站](https://gabrieltanner.org/)或者开始关注我的[时事通讯](https://gabrieltanner.us20.list-manage.com/subscribe/post?u=9d67fc028348a0eb71318768e&amp;id=6845ed3555)。

如果你有任何问题或反馈，请在下面的评论中告诉我。