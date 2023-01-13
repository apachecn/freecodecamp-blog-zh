# 如何编写框架代码——vue . js 的第一行

> 原文：<https://www.freecodecamp.org/news/how-to-code-a-framework-vuejs-example/>

你有没有想过框架是如何构建的？几周前，我写了一篇文章，问自己，尤雨溪构建 Vue.js 的第一行代码是什么？

感谢 Git 和尤雨溪将 Vue 的代码推送到 GitHub，我已经能够回到过去，就像马蒂·小飞侠驾驶他的 DeLorean 时光机一样。但我回到了九年前，回到了 2013 年，“看着”埃文编写他的代码。

![Delorean time machine](img/a3564c8901bb820c0cdca09f426649d2.png)

Photo by wallup.net

## 这篇文章的目的是什么？

我写这篇文章是为了向您展示像 Vuejs 这样的流行工具背后的东西，以及构建类似工具的起点。具体来说，我们将看看埃文的“你”的出发点是什么。

我们将通过查看 Vue 的创建者最早提交的源代码来学习他。我们将研究他为第一个 Vue 应用程序的实现写了什么，以及他如何用普通的 JavaScript 编写逻辑来使 mustache 语法工作。

### 什么是小胡子语法？

如果你想知道 mustach 语法是什么，让我来解释一下。这是 Vuejs 用来在模板中插入文本的一种基本的数据绑定形式。

从 vue 文档中:

> Vue.js 使用基于 HTML 的模板语法，允许您以声明方式将呈现的 DOM 绑定到底层 Vue 实例的数据。数据绑定最基本的形式是使用“小胡子”语法(双花括号)的文本插值:

`<span>Message: {{ msg }}</span>`

> mustache 标签将被相应数据对象上 msg 属性的值替换。每当数据对象的 msg 属性改变时，它也会被更新。

好了，现在你知道那是什么了，在下一部分，我将回答你的下一个问题...

## 看完这篇文章我会学到什么？

好吧，很公平，你想知道为什么你应该读这篇文章，你会从中学到什么。

如果你是一个经验丰富的 Vuejs 开发人员，或者刚刚开始你的旅程，你将了解像 Vue 这样的流行工具是如何开始的。

您还将学习如何寻找框架的特定特性，浏览旧的 GitHub 提交，并了解如何应用简单的 JavaScript 知识来开始构建我们这个时代最流行的框架之一的最初特性。

在下一节中，我们将开始探索 Vue.js 存储库。我们将查看第一次和第二次提交，以了解为框架的初始设置创建了什么文件。

这将帮助我们找到我们正在寻找的特性(mustache 语法),并弄清楚第一个 Vue 应用程序是如何制作的。

## 探索 Vue 最早的提交

好吧，我们开始吧。如果你想跟随我进行这次时间旅行，点击这个[链接](https://github.com/vuejs/vue/commits/0.6.0?after=218557cdec830a629252f4a9e2643973dc1f1d2d+349&branch=0.6.0&qualified_name=refs%2Ftags%2F0.6.0)。在那里，您会发现标记为 0.6.0 的 Vuejs 存储库。我们对它的第一次和第二次提交感兴趣。

我已经在本地下载了源代码的副本，准确地说是第二次提交的源代码。让我们浏览代码。

[https://www.youtube.com/embed/jDeze8rA7cA?feature=oembed](https://www.youtube.com/embed/jDeze8rA7cA?feature=oembed)

## 文件夹结构

第二次提交中的项目结构相当简单。除了一堆 jshing、grund、GitHub 的配置文件和两个 JSON 文件，我们可以看到三个文件夹:

*   试验
*   科学研究委员会
*   探索

最后一个是 Evan 添加的。第一次提交时 exploration 文件夹不在那里。正是在那里，Vue.js 的实际创建开始发生。

我们将在本文稍后回到这里，但在此之前，让我们先看看 fist commit，以找到 Evan 的第一行代码。Spolier:一切都始于测试。

## 第一个 Vue 测试案例

我相信，第一行代码是在 test.js 文件中编写的。在这里，Evan 使用 Mocha 库编写了 Vue 的第一个测试用例，并在名为[初始设置](https://github.com/vuejs/vue/commits/0.6.0?after=218557cdec830a629252f4a9e2643973dc1f1d2d+349&branch=0.6.0&qualified_name=refs%2Ftags%2F0.6.0)的第一次提交中设置了测试框架。

这有什么关系？嗯，我们不只是在寻找一个特定的特性，我们还想了解构建像 Vuejs 这样的工具的起点是什么。

你是从写实现开始的吗？或者你写一个基本的测试用例只是为了设置好一切，以便当你有了你想要实现的想法时，你可以写适当的测试？

好了，下面你有你的答案了！

让我们检查一下 test/test.js 文件的代码:

```
var Element = require('element')

describe('Element', function () {
    it('should have a variable', function () {
        assert.equal(Element, 123)
    })
}) 
```

在第一行，有一个`require`语句来导入一个元素类，这个元素类将在代码的后面某个地方定义。把这个想象成 Vue 类的祖先。

接下来，设置 Mocha `describe`函数来提供一个大致的上下文。
在其中，调用一个`it`函数来编写实际的测试用例，检查导入的`Element`类是否等于`123`，它使用方法`assert.equal()`来完成。

为了运行测试，我们必须安装所有的依赖项`npm i`并运行繁重的任务。但是因为大多数使用的库都被废弃了，所以我们不会这样做(这也不是本文和视频的目的)。

在下一节中，我们将探索旨在实现我们目标的第二个提交——找到 Vue.js 的第一个实现，并理解 mustache 语法是如何工作的。

为此，我们需要查看第二次提交的源代码，这是我已经下载并正在 VSCode 上探索的代码(如果您也在观看视频)。

这里是[直接链接](https://github.com/vuejs/vue/tree/871ed9126639c9128c18bb2f19e6afd42c0c5ad9)。

## 第一个 Vue 应用

在 Vue 中，我们所做的一切都是在 Vue 应用程序中完成的，它被绑定到 Vue 类的一个实例中。所以，我们首先需要找到这个类的第一个实现，在那里我们将找到 mustache 语法背后的逻辑。

好了，我们需要看看探索文件夹里面——这就是神奇发生的地方。

主文件叫做`getset-revitis-style.html`，在这里我们可以看到第一个 Vue 应用的所有逻辑(可以在 body 标签中找到)和它的第一个实现(可以在`script`标签中找到)。

我复制了整个文件，并将所有内容放在一个 index.html 文件中，这样我们可以修改代码，添加一些控制台日志，并探索它是如何工作的。

让我们使用`serve -s`来提供文件。(要运行此命令，您需要安装一个 npm 软件包。只需输入终端`npm install -g serve`。)

在 body 标签中，我们可以看到 Vue 应用程序，在`div`中带有一个`test`的`id`。今天，我们要么在 id 为`app`或`root`的根元素中定义我们的应用程序，但当时它是从一个测试 div 开始的。

```
 <div id="test">
   <p>{{msg}}</p>
   <p>{{msg}}</p>
   <p>{{msg}}</p>
   <p>{{what}}</p>
   <p>{{hey}}</p>
  </div> 
```

在测试`div`中，我们可以看到双髭语法。酷！这是第一次使用，但它是如何工作的？

在下一节中，我们将探索第一个 Vue 类，并寻找使这个`{{msg}}`工作的逻辑。

## 第一个 Vue 实例

好的，我们找到了这个语法的第一个用法，但是我们还没有完成。我们想知道它是如何工作的，记得吗？所以，让我们看看脚本标签，在那里我们会找到第一个 Vue 类的逻辑。

Evan 创建了一个名为`Element`的类——记住我们回到了 9 年前，而 ES6 要到 2015 年才出现。

类声明是用`function Element () {}`写的。这是我们今天所知的 Vue 实例的祖先类，我们通过执行`new Vue()`来实例化它。

```
 function Element (id, initData) {
  // The first implementation is in here
} 
```

接下来，通过实例化 Element 类来创建第一个 Vue 实例:

```
 var app = new Element('test', {
  msg: 'hello'
}) 
```

该类需要一个`id`和一个`initData`。这些作为`test`值和具有属性`msg`的对象`{}`传递给实例。这是我们第一次实现 options 对象。

好的，我们就要到了。现在我们知道了这个类是如何实现和实例化的，让我们深入了解一下 double mustache 语法是如何实现的。

## 小胡子语法是如何工作的

我们到了。下一个代码块将向我们展示语法秘密。这就是我们一直想要理解的，也是本文的目标。

在此之后，你将能够理解这种语法背后的含义，甚至可以用你自己的语法进行编辑和替换。

你可以做一些类似于`[[msg]]`的事情，或许称之为双框语法。🤓

下面的代码用于使双髭语法起作用。在这两者之间，还有更多负责数据绑定的代码。

```
var bindingMark = 'data-element-binding' // <-- data binding mark 

function Element (id, initData) {
// The first implementation is in here
  var self = this,
  el = self.el = document.getElementById(id)
  //console.log(self.el)

  bindings = {} // the internal copy
  data = self.data = {} // the external interface
  content = el.innerHTML.replace(/\{\{(.*)\}\}/g, markToken)

  el.innerHTML = content

  // ....

  function markToken(match, variable) {
    console.log(match) // <-- LOG match = {{msg}}
    console.log(variable) // <-- LOG captured group as variable = msg
    //console.log(bindings)
    bindings[variable] = {}
    //console.log(bindings)

    console.log('<span ' + bindingMark + '="' + variable + '"></span>')
    return '<span ' + bindingMark + '="' + variable + '"></span>'
  }

  // ...
} 
```

我添加了几个控制台日志来弄清楚两个关键参数(`match`和`variables`)的内容以及`markToken`方法返回的内容。

在脚本标签中，第一行是一个变量`var bindingMark = 'data-element-binding'`。这个变量将被用作一个数据属性，并用一个正则表达式`el.innerHTML.replace(/\{\{(.*)\}\}/g, markToken)`使用 replace 方法替换花括号中的内容。

是的，你答对了——在这个语法背后是普通的 JavaScript，特别是这种语言中最古老的方法之一。

`string.replace()`是一个接受两个参数的字符串方法:

*   正则表达式
*   回调函数

使用类似于<regex101.com>的站点检查正则表达式的结果，看看什么匹配正则表达式`\{\{(.*)\}\}`。</regex101.com>

当回调函数`markToken`被调用时，我们可以访问匹配和捕获的组，分别作为参数`match`和`variables`。

您可以使用我在源代码中添加的控制台日志来查看这两个参数值。

在`markToken`方法中，控制台日志之后的第一行是`bindings[variable] = {}`。这是数据的内部副本，稍后将用于框架的数据绑定功能。

对于每个匹配，它在`bindings`对象中设置一个新属性作为空对象。例如，如果我们有`{{msg}}`，一个名为`binginds[msg] = {}`的新属性将被创建。

最后，return 语句构建一个`span`元素，该元素使用`bindingMark`变量的值作为数据属性`data-element-binding`
，并为其分配`variable`参数作为属性。

因此，下面的字符串`'<span ' + bindingMark + '="' + variable + '"></span>'`被创建，而不是`{{mess}}`。结果是下面的代码:

```
<span data-element-binding="msg"></span> 
```

Vue 代码仍处于早期阶段。mustache 语法的实现与 data bingind 一起工作，data bingind 在框架的这一点上还没有完全实现。

## 结论

在这篇文章中，我们发现了尤雨溪构建 Vue.js 的第一步。这是该框架的一个原始实现，我们只看到了它的一小部分代码。但是它可以帮助我们弄清楚其中一个框架特性是如何工作的。嘿，事情总是从小事开始，随着时间的推移而增长。

让我知道你是否喜欢这种内容。如果您想了解 Vuejs 的不同功能背后的内容，请联系我。

也可以考虑订阅我的 [YouTube 频道](https://www.youtube.com/channel/UCTuFYi0pTsR9tOaO4qjV_pQ)。

你可以在 Twitter 和 Linkedin 上关注我。