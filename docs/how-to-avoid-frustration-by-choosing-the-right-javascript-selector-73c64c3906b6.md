# 如何通过选择正确的 JavaScript 选择器来避免挫折

> 原文：<https://www.freecodecamp.org/news/how-to-avoid-frustration-by-choosing-the-right-javascript-selector-73c64c3906b6/>

#### 选择器如何影响代码的快速指南

在进行一个项目时，我在代码中遇到了一个问题。我试图将多个 HTML 元素定义到一个集合中，然后根据一些预设条件更改这些元素。我花了大约四个小时的编码时间(两天)调试我的代码，并试图找出为什么我没有得到我想要的结果。

原来我使用了*document . queryselectorall()*将我的元素分配给一个变量，然后我试图更改这些元素。唯一的问题是特定的 JavaScript 选择器返回一个**静态[节点列表](https://developer.mozilla.org/en-US/docs/Web/API/NodeList)** 。因为它不是元素的实时表示，所以我不能在后面的代码中修改它们。

### **假设**

在本文中，我假设一些事情是真实的:

*   您正在使用“普通的”JavaScript(没有框架/库)
*   您对 JavaScript 元素/选择器有基本的了解
*   您已经对 DOM 有了基本的了解

### 事实真相

如果我假设得太多，我在文章中提供了相关材料的链接，希望对您有所帮助。

JavaScript 是一个巨大而丰富的生态系统，因此有很多方法来完成同样的任务也就不足为奇了。根据你的任务，完成任务的方式在一定程度上很重要。

你可以用手挖一个洞，但是用铲子会更容易和更有效率。

为此，我希望在你读完这篇文章后“给你一把铲子”。

![IXLL54yngArOlJqNfITubIlTMhXOrsMuhVkk](img/5da1e92bce3dd0050811d4a4169701fd.png)

“A long-exposure shot of a group of people on a beach with children digging a deep hole” by [Khürt Williams](https://unsplash.com/@khurtwilliams?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

### **为工作选择合适的工具**

我有一个问题，“我应该使用哪个元素选择器？”好几次。直到现在，只要我的代码产生了期望的结果，我就没有太多的愿望或需要去了解这种区别。毕竟，出租车的颜色有什么关系，只要它能让你安全及时地到达目的地……对吗？

让我们从 JavaScript 中选择 [**DOM**](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction) 元素的一些方法开始。有更多的方法(我相信)来选择元素，但是这里列出的是我遇到的最普遍的方法。

#### [**document . getelementbyid()**](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById)

这些函数将只返回一(1)个元素，因为从本质上讲，ID 是唯一的，一次在页面上只能有一个(同名的)元素。

它返回一个与传递给它的字符串相匹配的对象。如果在 HTML 中没有找到匹配的 ID，它将返回空值。

> 语法示例-& g*t；document . getelementbyid(' main _ conten*t ')

与我们将在本文后面讨论的一些选择器不同，不需要用#来表示元素 id。

#### [***document . getelementsbytagname()***](https://developer.mozilla.org/en-US/docs/Web/API/Element/getElementsByTagName)

注意元素中的“S”——该选择器返回类似于**数组的结构** 中的**倍数***,称为 [HTML 集合](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection)——搜索包括[根节点](https://javascript.info/dom-navigation)(文档对象)在内的所有文档以找到匹配的名称。这个元素选择器从文档的顶部开始，一直向下，搜索与传入的字符串匹配的标签。*

*如果你想使用原生的[数组方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)，那你就没那么幸运了。也就是说，除非您将返回的结果转换成一个数组来迭代它们，或者使用 ES6 [扩展操作符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)——这两者都超出了本文的范围。但是我想让你知道，如果你愿意，使用数组方法是可能的。*

> *语法示例-& g*t；document . getelementsbytagname(' header _ Li*nks ')。该选择器还接受 **ts p、div、body 或任何其他有效的 HTML t** ag。*

#### *[***document . getelementsbyclassname()***](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementsByClassName)*

*类名选择器——再次注意元素中的“S”——该选择器返回一个类似于**数组的结构** 中的**的倍数**，该结构被称为类名的 [HTML 集合](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection)。它匹配传入的字符串(可以接受多个类名，尽管它们之间用空格隔开)，搜索整个文档，可以在任何元素上调用，并且只返回传入的类的后代。*

*还有，没有。需要用句点来表示类名*

> *语法示例:*document . getelementsbyclassname(' classNam*e ')*

#### *[***【document . query selector()***](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)*

*该选择器将只返回一(1)个元素。*

*只会返回与传入字符串匹配的第一个元素。如果没有找到所提供字符串的匹配项，则返回空值 。*

> *语法示例:*document.querySelector(' # side _ note*')*或 document . query selector('。header _ link*s’)*

*与我们之前的所有例子不同，这个选择器需要一个.(句点)来表示类，或者一个 *#* (octothorp)来表示 ID，并与所有 CSS 选择器一起工作。*

#### *[***document . queryselectorall()***](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll)*

*该选择器返回与传入字符串匹配的**倍数**，并将它们排列到另一个类似**数组的结构**中，称为[节点列表](https://developer.mozilla.org/en-US/docs/Web/API/NodeList)。*

*和前面的一些例子一样，节点列表不能像[一样使用本机](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)[数组方法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)。forEach( )。所以如果你想使用它们，你必须首先把节点列表转换成一个数组。如果您不希望转换，您仍然可以在语句中用 f [或…遍历节点列表。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)*

*传入的字符串必须与有效的 CSS 选择器匹配— id、类名、类型、属性、属性值等。*

> *语法示例: *document.querySelectorAll('。页脚 _ 链接*s’)*

### *包扎*

*通过为您的编码需求选择正确的选择器，您将避免我已经陷入的许多陷阱。当您的代码不起作用时，这可能会令人非常沮丧，但是通过确保您的选择器与您的情况需要相匹配，您将不会有“用铲子挖”的麻烦:)*

*感谢你通读这篇文章。如果你喜欢它，请考虑捐赠一些掌声，帮助其他人也找到它。我在我的[博客](https://www.powerofgoose.com/blog)上发布(积极管理我的时间表以写更多)相关内容。我也活跃在 [Twitter](https://twitter.com/jj_goose) 上，并且总是很乐意与其他开发者联系！*