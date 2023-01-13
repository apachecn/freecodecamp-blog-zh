# 如何用 71 行代码编写类似库的 jQuery 了解 DOM

> 原文：<https://www.freecodecamp.org/news/how-to-write-a-jquery-like-library-in-71-lines-of-code-learn-about-the-dom-e9fb99dbc8d2/>

作者库尔特

# 如何用 71 行代码编写类似库的 jQuery 了解 DOM

![1*FLAKTYk8B7EpCYlMqtAFkw](img/9e30a7481bc0fc3d9da896c541ba9a28.png)

JavaScript 框架风靡一时。您打开的任何与 JavaScript 相关的新闻提要都有可能充斥着对 ReactJS、AngularJS、Meteor、RiotJS、BackboneJS、jQuery 等工具的引用。

任何学习编码的人(甚至是有经验的开发人员)都会感受到学习这些新工具的巨大压力。炒作创造需求。如果你跟不上最新的需求，它会让人觉得你的服务没有需求。

我注意到一种趋势，人们一头扎进学习这些工具，却不知道*他们做什么，更不用说 ***他们如何做了。这最终使得调试和概念化该工具异常困难。有数以千计的误用案例，其中整个项目被简单地创建用于双向数据绑定，或者用于动画效果，或者甚至仅仅用于显示图像滑块。****

#### *开发人员忽略了学习 DOM 本身*

*DOM，或者说[文档对象模型](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)，是你的网络浏览器的心脏和灵魂。你现在正看着它。澄清一下，DOM 不是 JavaScript 的特性——它甚至不是用 JavaScript 编写的——它是一个编程接口，是语言和浏览器之间的 API。语言控制计算等，浏览器控制显示和事件。*

*下面我将演示如何创建一个简单的类似 jQuery 的 DOM 操作库。这将能够使用著名的$ selector 定位元素、创建新元素、添加 html 和控制事件绑定。*

### *入门指南*

*我们需要创建我们的基本对象。姑且称之为 *domElement* 。这个对象将作为目标元素的包装器。*

```
*`var domElement = function(selector) {
 this.selector = selector || null; //The selector being targeted
 this.element = null; //The actual DOM element
};`*
```

#### *现在我们可以开始添加功能了。*

*我们将复制的 jQuery 方法是选择器/创建者 ***$()。在()，。off()，。val()，。追加，。前置()**和**。**html()**

*让我们先来看看事件绑定。这是迄今为止我们将创建的最复杂的方法，也是最有用的方法。这是双向数据绑定模型中的粘合剂。(当诸如 update 之类的事件被触发时，模型更新它的订阅者，订阅者也这样做。)*

*我们将使用[发布/订阅设计模式](https://addyosmani.com/resources/essentialjsdesignpatterns/book/#observerpatternjavascript)。*

***时*。*(event，callback)**上叫我们 ***上叫*** 下叫事件，类似地当 ***。***off(事件)是叫我们**从事件中退订。***

***事件处理程序将是它自己的对象。***

***让我们从创建一个基础对象开始，并用它扩展原型***【DOM element】***。***

```
**`domElement.prototype.eventHandler = {
 events: [] //Array of events & callbacks the element is subscribed to.
}`**
```

**很好，现在让我们创建订阅者方法。我们称它为 ***bindEvent*** ，因为它将一个事件监听器绑定到我们的 DOM 元素。**

```
**`domElement.prototype.eventHandler = {
 events: [], //Array of events the element is subscribed to.

bindEvent: function(event, callback, targetElement) {
    //remove any duplicate event 
    this.unbindEvent(event,targetElement);

    //bind event listener to DOM element
    targetElement.addEventListener(event, callback, false);

    this.events.push({
      type: event,
      event: callback,
      target: targetElement
    }); //push the new event into our events array.
  }

}`**
```

#### **就是这样！让我们快速分解这个函数**

1.  **我们移除元素上具有正在绑定的类型的任何现有事件。这纯粹是个人喜好问题。我更喜欢使用单一的事件处理程序，因为它们更容易管理和调试。移除该行将允许同一类型的多个处理程序。稍后我们将创建 ***unbindEvent*** 函数。**
2.  **我们将事件绑定到 DOM 元素，使其具有生命力。**
3.  **我们将事件及其所有信息放入事件数组，这样元素就可以跟踪我们的侦听器。**

**现在，在我们移除一个事件之前，我们需要一个方法来从事件数组中找到并返回它，如果它存在的话。让我们创建一个快速的方法，使用内置的 [***数组过滤器***](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 方法，根据事件的类型查找并返回事件。**

```
**`domElement.prototype.eventHandler = {
 events: [], //Array of events the element is subscribed to.

bindEvent: function(event, callback, targetElement) {
    //remove any duplicate event 
    this.unbindEvent(event,targetElement);

    //bind event listener to DOM element
    targetElement.addEventListener(event, callback, false);

    this.events.push({
      type: event,
      event: callback,
      target: targetElement
    }); //push the new event into our events array.
  },

  findEvent: function(event) {
    return this.events.filter(function(evt) {
      return (evt.type === event); //if event type is a match return
    }, event)[0];
  }
}`**
```

#### **现在我们可以添加我们的 ***unbindEvent*** 方法。**

```
**`domElement.prototype.eventHandler = {
 events: [], //Array of events the element is subscribed to.

bindEvent: function(event, callback, targetElement) {
    //remove any duplicate event 
    this.unbindEvent(event,targetElement);

    //bind event listener to DOM element
    targetElement.addEventListener(event, callback, false);

this.events.push({
      type: event,
      event: callback,
      target: targetElement
    }); //push the new event into our events array.
  },

findEvent: function(event) {
    return this.events.filter(function(evt) {
      return (evt.type === event); //if event type is a match return
    }, event)[0];
  },

unbindEvent: function(event, targetElement) {
    //search events
    var foundEvent = this.findEvent(event);

    //remove event listener if found
    if (foundEvent !== undefined) {
      targetElement.removeEventListener(event, foundEvent.event, false);
    }

    //update the events array
    this.events = this.events.filter(function(evt) {
      return (evt.type !== event);
    }, event);
  }
};`**
```

#### **那是我们的事件处理器！下面试试吧…**

 **[//codepen.io/kurtr/embed/eJbEWb/?height=265&theme-id=0&default-tab=js,result](//codepen.io/kurtr/embed/eJbEWb/?height=265&theme-id=0&default-tab=js,result)

See the Pen [domElement Events](https://codepen.io/kurtr/pen/eJbEWb/) by kurt rohlandt ([@kurtr](https://codepen.io/kurtr)) on [CodePen](https://codepen.io).

这是一个非常有用的小工具，但是您可能想知道这与 jQuery 有什么关系，为什么事件处理程序的方法没有命名为“on”和“off”

这是我们接下来要做的。由于我们要求事件处理程序是一个对象，并且我们不想调用***$(' element '). eventhandler . on(..)*** 我们的方法将简单地指向正确的函数。

下面是 ***on*** 和 ***off*** 方法的代码:

```
domElement.prototype.on = function(event, callback) {
   this.eventHandler.bindEvent(event, callback, this.element);
}
domElement.prototype.off = function(event) {
   this.eventHandler.unbindEvent(event, this.element);
}
```

明白这是怎么回事了吗？现在让我们添加我们的其他实用功能…

```
domElement.prototype.val = function(newVal) {
 return (newVal !== undefined ? this.element.value = newVal : this.element.value);
};
domElement.prototype.append = function(html) {
 this.element.innerHTML = this.element.innerHTML + html;
};
domElement.prototype.prepend = function(html) {
 this.element.innerHTML = html + this.element.innerHTML;
};
domElement.prototype.html = function(html) {
 if(html === undefined){
 return this.element.innerHTML;
 }
 this.element.innerHTML = html;
};
```

这些都很简单。唯一需要关注的是 ***。html()。*** 该方法有两种调用方式，如果不带参数调用，它将返回元素的 ***innerHTML*** ，但如果带参数调用，它将设置元素的 ***HTML*** 。这通常被称为***getter/setter***函数。

### 初始化

#### 在初始化时，我们需要做两件事之一…

1.  如果选择器以左括号开始，将创建一个新元素。
2.  否则我们将使用***document . query selector***来选择一个*现有的*元素。

为了简单起见，在创建元素的情况下，我只做了最少的 HTML 验证工作，并且在选择元素时，我使用了***document . query selector***这意味着不管匹配的数量是多少，它都只返回一个元素(第一个匹配)。

这可以通过使用***document . query selector all***并重构方法以使用元素数组来改变，而无需太多的努力来选择所有匹配的元素。

```
domElement.prototype.init = function() {
 switch(this.selector[0]){
 case ‘<’ :
 //create element
 var matches = this.selector.match(/<([\w-]*)>/);
 if(matches === null || matches === undefined){
 throw ‘Invalid Selector / Node’;
 return false;
 }
 var nodeName = matches[0].replace(‘<’,’’).replace(‘>’,’’);
 this.element = document.createElement(nodeName);
 break;
 default :
 this.element = document.querySelector(this.selector);
 }
};
```

#### 让我们浏览一下上面的代码。

1.  我们使用一个 ***开关*** 语句，并将我们选择器的第一个字符作为参数传递。
2.  如果它以一个括号开始，我们做一个快速的 ***正则表达式*** 匹配来找到在开括号和闭括号之间的文本。如果失败，我们抛出一个选择器无效的错误。
3.  如果匹配成功，我们去掉括号，将文本传递给***document . createelement***来创建一个新元素。
4.  或者，我们使用 ***document 查找匹配。querySelector*** 如果没有找到匹配，则返回 null。
5.  最后，我们将 ***domElement*** 上的元素属性设置为匹配/创建的元素。

### 用$来引用***DOM element***

#### 最后我们将指定 ***$*** 符号来初始化一个新的 ***domElement*** 。

```
$ = function(selector){
 var el = new domElement(selector); // new domElement
 el.init(); // initialize the domElement
 return el; //return the domELement
}
```

***$*** 符号只是一个变量！这就是我们完整的类似 jQuery 的库，全部由 71 行可读性强、间距合理的代码组成。

#### 这里有一支笔运行完整的库…使用您的控制台。

[//codepen.io/kurtr/embed/wMRgJK/?height=265&theme-id=0&default-tab=html,result](//codepen.io/kurtr/embed/wMRgJK/?height=265&theme-id=0&default-tab=html,result)

See the Pen [javascriptDom](https://codepen.io/kurtr/pen/wMRgJK/) by kurt rohlandt ([@kurtr](https://codepen.io/kurtr)) on [CodePen](https://codepen.io).

### 下一步做什么？

1.  为什么不试着复制你最喜欢的效用函数呢？
2.  潜入 [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
3.  使用事件侦听器绑定双向数据。

### 重要注意事项

特别感谢昆西·拉尔森(Quincy Larson),他修复了我对英语语言的破坏，视觉上的调整和伟大的标题图像，使这篇文章更人性化。

这段代码是作为一个简单的例子编写的，用来说明 JavaScript 库如何与 DOM 交互以及如何修改 DOM。应该如此对待。

我使用简单明了的陈述来帮助读者理解和遵循示例，并且轻描淡写地回避——或者完全忽略——失败和验证点。

返回的元素只会将创建的方法绑定到包装。您可以通过调用***$(' selector '). element .***来访问实际的 DOM 元素及其方法。这是为了避免扩展 DOM，这是一个需要单独发布的敏感话题。

如果您正确地遵循了这些步骤，您应该得到如下所示的完整文件:

```
var domElement = function(selector) {
 this.selector = selector || null;
 this.element = null;
};
domElement.prototype.init = function() {
 switch (this.selector[0]) {
 case ‘<’:
 var matches = this.selector.match(/<([\w-]*)>/);
 if (matches === null || matches === undefined) {
 throw ‘Invalid Selector / Node’;
 return false;
 }
 var nodeName = matches[0].replace(‘<’, ‘’).replace(‘>’, ‘’);
 this.element = document.createElement(nodeName);
 break;
 default:
 this.element = document.querySelector(this.selector);
 }
};
domElement.prototype.on = function(event, callback) {
 var evt = this.eventHandler.bindEvent(event, callback, this.element);
}
domElement.prototype.off = function(event) {
 var evt = this.eventHandler.unbindEvent(event, this.element);
}
domElement.prototype.val = function(newVal) {
 return (newVal !== undefined ? this.element.value = newVal : this.element.value);
};
domElement.prototype.append = function(html) {
 this.element.innerHTML = this.element.innerHTML + html;
};
domElement.prototype.prepend = function(html) {
 this.element.innerHTML = html + this.element.innerHTML;
};
domElement.prototype.html = function(html) {
 if (html === undefined) {
 return this.element.innerHTML;
 }
 this.element.innerHTML = html;
};
domElement.prototype.eventHandler = {
 events: [],
 bindEvent: function(event, callback, targetElement) {
 this.unbindEvent(event, targetElement);
 targetElement.addEventListener(event, callback, false);
 this.events.push({
 type: event,
 event: callback,
 target: targetElement
 });
 },
 findEvent: function(event) {
 return this.events.filter(function(evt) {
 return (evt.type === event);
 }, event)[0];
 },
 unbindEvent: function(event, targetElement) {
 var foundEvent = this.findEvent(event);
 if (foundEvent !== undefined) {
 targetElement.removeEventListener(event, foundEvent.event, false);
 }
 this.events = this.events.filter(function(evt) {
 return (evt.type !== event);
 }, event);
 }
};
$ = function(selector) {
 var el = new domElement(selector);
 el.init();
 return el;
}
```

如果你喜欢这篇文章，看看我写的其他东西。

[**预防性编程——如何在错误发生前将其修复**](https://medium.com/p/9df82cf215c5)
[*…以及为什么福尔摩斯会成为一名出色的程序员*](https://medium.com/p/9df82cf215c5)

[**学习编程时要记住的 5 件事**](https://medium.com/p/1ed8e734b04f)
[*学习编程很有挑战性。除了选择语言或建立开发环境之外，您还需要…*](https://medium.com/p/1ed8e734b04f)

[**我是如何成为程序员的。当我开始称自己为 One**](https://medium.com/p/54a0533c4335)
[*时，我已经想开始写关于编程的博客好几个月了，就像我之前的许多人一样，我满怀…*](https://medium.com/p/54a0533c4335)

[**制作天雨代码—矩阵风格**](https://medium.com/p/ec6e1386084e)
[*HTML 5 画布动画简介*](https://medium.com/p/ec6e1386084e)

将代码变成现金——作为一名网络开发人员如何赚钱并活下去。
[*所以你才学会了编码。你很渴望，任何不会编程的人都认为你是天才，消息传出去，所有人……*medium.com](https://medium.com/p/f5eedc557b3e)**