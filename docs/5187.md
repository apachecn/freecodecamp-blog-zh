# 如何在普通 JavaScript 中操作 DOM

> 原文：<https://www.freecodecamp.org/news/dom-manipulation-in-vanilla-js-2036a568dcd9/>

卡洛斯·达·科斯塔

# 如何在普通 JavaScript 中操作 DOM

![owVHsyOFSe5bUzy2aLJonFtVigim4YThsKU9](img/d89df23364eb225110ec167538caec30.png)

所以你已经学会了变量，选择结构和循环。现在是时候学习 DOM 操作并开始做一些很酷的 JavaScript 项目了。

在本教程中，我们将学习如何用普通的 JavaScript 操作 DOM。事不宜迟，让我们直接开始吧。

### 1.重要的事情先来

在我们深入编码之前，让我们了解一下 Dom 到底是什么:

> 文档对象模型(DOM)是 HTML 和 XML 文档的编程接口。它表示页面，以便程序可以更改文档结构、样式和内容。DOM 将文档表示为节点和对象。这样，编程语言可以连接到页面。[来源](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)

基本上，当浏览器加载一个页面时，它会创建该页面的对象模型并将其打印在屏幕上。对象模型以树形数据结构表示，每个节点是具有属性和方法的对象，最顶层的节点是文档对象。

可以用一种编程语言来访问和修改这个对象模型，这个动作叫做 DOM 操纵。我们会用 JavaScript 来做，因为 JS 很棒。

### 2.实际教程

对于本教程，我们将需要两个文件，一个 index.html，另一个操纵. js

```
<!-- Index.html file --><html>  <head>    <title>DOM Manipulation</title>  </head>  <body>     <div id="division"><h1 id="head"><em>DOM<em> manipulation</h1>      <p class="text" id="middle">Tutorial</p><p>Sibling</p>      <p class="text">Medium Tutorial</p>    </div>    <p class="text">Out of the div</p>    <!-- Then we call the javascript file -->    <script src="manipulation.js"></script>  </body></html>
```

这就是我们的 HTML 文件，如你所见，我们有一个 div，id 为 division。在那里面，我们有一个 h1 元素，在同一行中，你稍后会明白为什么我们有两个 p 元素和 div 结束标记。最后，我们有一个包含一类文本的 p 元素。

#### 2.1.访问元素

我们既可以访问单个元素，也可以访问多个元素。

#### 2.1.1.访问单个元素

为了访问单个元素，我们将研究两种方法:getElementByID 和 querySelector。

```
// the method below selects the element with the id ofheadlet id = document.getElementById('head');
```

```
// the code below selects the first p element inside the first divlet q = document.querySelector('div p');
```

```
/* Extra code */// this changes the color to redid.style.color = 'red';// give a font size of 30pxq.style.fontSize = '30px';
```

现在我们已经访问了两个元素，id 为 **head** 的 h1 元素和 div 中的第一个 **p** 元素。

**getElementById** 将一个 Id 作为参数，而 **querySelector** 将一个 CSS 选择器作为参数，并返回与该选择器匹配的第一个元素。如您所见，我将方法结果分配给了变量，然后在最后添加了一些样式。

#### 2.1.2.访问多个元素

当访问多个元素时，会返回一个节点列表。它不是一个数组，但它的工作方式类似于数组。所以您可以遍历它并使用 length 属性来获取节点列表的大小。如果你想得到一个特定的元素，你可以使用数组符号或者 item 方法。您将在代码中看到它们。

为了访问多个元素，我们将使用三种方法:getElementsByClassName、getElementsByTagName 和 querySelectorAll。

```
// gets every element with the class of textlet className = document.getElementsByClassName('text');
```

```
// prints the node listconsole.log(className);
```

```
/* prints the third element from the node list using array notation */console.log(className[2]);
```

```
/* prints the third element from the node list using the item function */console.log(className.item(2));
```

```
let tagName = document.getElementsByTagName('p');let selector = document.querySelectorAll('div p');
```

代码似乎不言自明，但我还是要解释一下，因为我是个好人。:)

首先，我们使用 **getElementsByClassName** ，它接受一个类名作为参数。它返回一个节点列表，其中每个元素都将**文本**作为一个类。然后我们在控制台上打印节点列表。我们还使用**数组符号**和**项目方法**打印列表中的第三个元素。

其次，我们使用 getElementsByTagName 方法选择每个 **p** 元素，该方法将标记名作为参数，并返回该元素的节点列表。

最后，我们使用 **querySelectorAll** 方法，该方法将 CSS 选择器作为参数。在这种情况下，它采用了 **div p** ，因此它将返回 div 中的 **p** 元素的节点列表。

作为练习，打印标记名和**选择器**节点列表中的所有元素，并找出它们的大小。

#### 2.2.穿越大教堂

到目前为止，我们已经找到了访问特定元素的方法。如果我们想访问已经访问过的元素旁边的元素，或者访问之前访问过的元素的父节点，该怎么办？属性 **firstChild、lastChild、parentNode、nextSibling、**和 **previousSibling** 可以为我们完成这项工作。

**firstChild** 用于获取一个节点的第一个子元素。 **lastChild** ，如你所料，它获取一个节点的最后一个子元素。 **parentNode** 是用来访问一个元素的父节点。 **nextSibling** 为我们获取已经访问过的元素的下一个元素，而 **previousSibling** 为我们获取已经访问过的元素的前一个元素。

```
// gets first child of the element with the id of divisionlet fChild = document.getElementById('division').firstChild;console.log(fChild);
```

```
// gets the last element from element with the id of divisionlet lChild = document.querySelector('#division').lastChild;
```

```
// gets the parent node of the element with the id divisionlet parent = document.querySElector('#division').parentNode;console.log(parent);
```

```
// selects the element with the id of middlelet middle = document.getElementById('middle');// prints ond the console the next sibling of middleconsole.log(middle.nextSibling);
```

上面的代码首先获取 division id 元素的 **firstChild** 元素，然后在控制台上打印出来。然后，它从具有部门 id 的同一个元素中获取 **lastChild** 元素。然后，它获取 id 为 division 的元素的 **parentNode** ，并将其打印在控制台上。最后，它选择 id 为 middle 的元素并打印其 **nextSibling** 节点。

大多数浏览器将元素之间的空格视为文本节点，这使得这些属性在不同的浏览器中以不同的方式工作。

#### 2.3.获取和更新元素内容

2.3.1。设置和获取文本内容

我们可以获取或设置元素的文本内容。为了完成这个任务，我们将使用两个属性:**节点值**和**文本内容**。

**nodeValue** 用于设置或获取文本节点的文本内容。 **textContent** 用于设置或获取包含元素的文本。

```
// get text with nodeValuelet nodeValue = document.getElementById('middle').firstChild.nodeValue;console.log(nodeValue);
```

```
// set text with nodeValuedocument.getElementById('middle').firstChild.nodeValue = "nodeValue text";
```

```
// get text with textContentlet textContent = document.querySelectorAll('.text')[1].textContent;console.log(textContent);
```

```
// set text with textContentdocument.querySelectorAll('.text')[1].textContent = 'new textContent set';
```

你注意到**节点值**和**文本内容**的区别了吗？

如果你仔细观察上面的代码，你会发现，为了用 **nodeValue** 获取或设置文本，我们首先必须选择文本节点。首先，我们得到了 id 为**中间**的元素，然后我们得到了它的**第一个孩子**文本节点，然后我们得到了返回单词 Tutorial 的**节点值**。

现在有了 **textContent** ，就不需要选择文本节点了，我们只需要获取元素，然后获取它的 **textContent** ，既可以设置也可以获取文本。

2.3.2。添加和删除 HTML 内容

您可以在 DOM 中添加和删除 HTML 内容。为此，我们将研究三个方法和一个属性。

让我们从 **innerHTML** 属性开始，因为这是添加和删除 HTML 内容最简单的方法。 **innerHTML** 可以用来获取或设置 HTML 内容。但是在使用 innerHTML 设置 HTML 内容时要小心，因为它会移除元素内部的 HTML 内容并添加新的内容。

```
document.getElementById('division').innerHTML =`<ul>  <li>Angular</li>  <li>Vue</li>  <li>React</li></ul>`;
```

如果您运行代码，您会注意到 div 中 id 为 division 的所有内容都将消失，列表将被添加。

我们可以使用方法: **createElement()** ，c **reateTextNode()** ，以及 **appendChild()** 来解决这个问题。

createElement 用于创建一个新的 HTML 元素。create text node 用于创建文本节点，appendChild 用于将新元素追加到父元素中。

```
//first we create a new p element using the creatElementlet newElement = document.createElement('p');/* then we create a new text node and append the text node to the element created*/let text = document.createTextNode('Text Added!');newElement.appendChild(text);
```

```
/* then we append the new element with the text node into the div with the id division.*/document.getElementById('division').appendChild(newElement);
```

还有一个叫做 **removeChild** 的方法用来移除 HTML 元素。

```
// first we get the element we want to removelet toBeRemoved = document.getElementById('head');// then we get the parent node, using the parentNOde propertylet parent = toBeRemoved.parentNode;/* then we use the removeChild method, with the element to be removed as a parameter.*/parent.removeChild(toBeRemoved);
```

所以首先我们得到想要移除的元素，然后我们得到它的父节点。然后我们调用 removeChild 方法来移除元素。

#### 2.4.属性节点

现在我们知道了如何处理元素，那么让我们来学习如何处理这些元素的属性。有一些方法像 **GetAttribute** 、 **setAttribute** 、 **hasAttribute** 、 **removeAttribute** ，还有一些属性像 **className** 和 **id** 。

**getAttribute** 顾名思义，它是用来获取属性的。比如类名、id 名、链接的 href 或任何其他 HTML 属性。

**setAttribute** 用于给一个元素设置一个新的属性。它有两个参数，第一个是属性，第二个是属性的名称。

**hasAttribute** 用于检查一个属性是否存在，以一个属性作为参数。

**removeAttribute** 用于删除一个属性，它以一个属性作为参数。

这个属性用来设置或获取一个元素的 Id。

**ClassName** 用于设置或获取一个元素的类。

```
// selects the first divlet d = document.querySelector('div');// checks if it has an id attribute, returns true/falseconsole.log('checks id: '+d.hasAttribute('id'));// set a new class attributed.setAttribute('class','newClass');// returns the class nameconsole.log(d.className);
```

我知道我是个好人，但是代码是不言自明的。

### 结论

就是这样！我们已经学习了这么多概念，但是关于 DOM 操作还有更多需要学习。我们在这里介绍的内容为您打下了良好的基础。

继续实践，创造新的东西来巩固这些新知识。

日安，编码不错。