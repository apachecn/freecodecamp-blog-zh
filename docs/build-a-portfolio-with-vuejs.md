# 如何使用 Vue.js Axios、GitHub REST API 和 Netlify 构建和部署投资组合

> 原文：<https://www.freecodecamp.org/news/build-a-portfolio-with-vuejs/>

在这本免费的书中，我们将构建两个简单的项目，并将它们部署在 Netlify 上。我们将使用 Vue.js 作为我们的前端框架，并使用不同的技术来构建我们的项目。

如果您一直遵循本教程，您将使用 GitHub API 构建一个简化版的 Twitter 和一个单页面应用程序。

## 跟随本教程您需要知道什么

要跟上本文，您至少需要一些 HTML、CSS 和 JavaScript 的基础知识。

Vue.js 的知识不是必需的，因为您将首先学习基础知识，然后我们将一起构建项目。

在每个部分的结尾，你会通过 YouTube 链接/嵌入找到视频形式的信息。这样你就可以观看视频来巩固你刚刚读过的知识。

## 目录

*   [简介](#introduction)
*   [如何安装 Vue](#how-to-install-vue)
*   [如何创建 Vue 实例](#how-to-create-a-vue-instance)
*   [如何在 Vue 中使用模板](#how-to-work-with-templates-in-vue)
*   检视指南
*   [方法](#methods-in-vue)
*   [条件句](#conditionalsinvuevifvelseifvelsevshow)
*   [循环](#loopsinvue)
*   [如何通过事件处理来处理用户输入](#howtohandleuserinputwitheventhandlingvoninvue)
*   [双向模型绑定(v-模型)](#twowaymodelbindingvmodelinvue)
*   [计算属性和方法](#computedpropertiesandmethods)
*   [**项目**:简单的 Twitter 克隆](#howtocreateasimpletwitterclone)
*   [组件基础知识](#vuecomponentbasics)
*   [项目更新:带有组件的简单 Twitter 克隆](#howtoupdateyoursimple_twitterprojectwithcomponents)
*   [Axios 和 RestAPI](#howtoperformapicallswithaxios)
*   [使用 VueRouter 进行路由](#howtohandleroutingwithvuerouter)
*   [**最终项目**:用 VueJS、VueRouter、Axios、GitHub API 构建一个组合](#finalprojecthowtobuildaportfoliowithvuejsvuerouteraxiosgithubapianddeploytonetlify)
*   [**部署**带 BitBucket 和 Netlify 的连续部署](#continuosdeploymentwithbitbucketandnetlify)

## 介绍

VueJS 是一个 JavaScript 框架，近年来变得非常流行。

在本指南中，我们将首先从基础知识开始，快速浏览两个库:VueRouter 和 Axios。我们将在最后使用它们来构建一个很酷的投资组合项目。

[https://www.youtube.com/embed/CzgP6GamIMc?feature=oembed](https://www.youtube.com/embed/CzgP6GamIMc?feature=oembed)

点击查看 [YouTube](https://youtu.be/CzgP6GamIMc) 上的视频。

## 如何安装 Vue

你可以在你的项目中使用 Vue，通过使用像 NPM 这样的包管理器或者使用它的 CDN 来安装它。如果你以前从未使用过 Vuejs，我建议你使用 CDN，因为如果你想和我一起编码，它会更容易。

点击查看[库](https://bitbucket.org/fbhood/how-to-vuejs/src/master/1-installation/)

点击查看 [YouTube 视频](https://youtu.be/enz0Vi3NuDA)或在本部分末尾找到它，以巩固您所学的内容。

### Vue CDN

对于 CDN，我们只需要在 HTML 文件中包含下面的脚本标记:

```
<!-- Development version for prototyping and learning -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.12/dist/vue.js"></script> 
```

或者，您可以使用一个使用特定稳定版本的生产就绪脚本，如下所示:

```
<!-- Production version -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script> 
```

在生产中，Vue 建议使用优化版本，将 vue.js 替换为 vue.min.js。

还有一个与 ES 模块兼容的版本:

```
<script type="module">
  import Vue from 'https://cdn.jsdelivr.net/npm/vue@2.6.12/dist/vue.esm.browser.js'
</script> 
```

### 如何通过 NPM 安装 Vue

如果您计划构建大规模的应用程序，我建议通过 NPM 安装，如下所示:

```
npm install vue 
```

正如我上面所说的，我们将使用 Vue CDN，这样任何人都可以遵循这个指南。因此，我们最终的 HTML 文件将如下所示:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VueJS Tutorial</title>

    <!-- vue development version, includes helpful console warnings -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

</head>
<body>

    <script src="./main.js"></script>
</body>
</html> 
```

让我们来分解这个代码。首先，我们为 HTML 文件添加了一个基本标记。然后我们包含了 VueJs 框架的脚本标签。

最后，在关闭 body 标记之前，我们已经添加了 main.js 脚本，其中放置了应用程序的所有 JavaScript 代码。

现在让我们进入下一步，在 main.js 文件中添加我们的第一个 Vue 实例。

[https://www.youtube.com/embed/enz0Vi3NuDA?feature=oembed](https://www.youtube.com/embed/enz0Vi3NuDA?feature=oembed)

## 如何创建 Vue 实例

一旦你安装了 Vue 或者通过它的 CDN 包含了它，你就可以创建一个 Vue 实例。您可以使用`new Vue()`功能来完成。这个函数接受一个选项对象。

如果您阅读文档，您会看到 vue 实例通常存储在一个名为`vm`的变量中，但是您可以随意命名它。在本指南中，我称之为`app`。

现在，在 main.js 文件中，您需要创建一个变量，并在其中存储 Vue 实例，如下所示:

```
let app = new Vue({
    // all options goes here
}) 
```

传递给 Vue 实例的对象称为 options 对象。在 options 对象中，您可以添加 Vue API 参考页面中描述的所有选项来构建我们的应用程序。

选项对象的属性分为多个部分:

*   数据
*   数字正射影像图
*   生命周期挂钩
*   资产
*   作文
*   杂项类别

构建 Vue 应用程序所需的第一个属性是用来连接 Vue 和一个根 DOM 元素。那么您将需要一些数据选项来处理。

让我们从连接 Vue 实例和一个根 DOM 元素开始。

您可以点击此处查看[库](https://bitbucket.org/fbhood/how-to-vuejs/src/master/2-create-vue-instance/)。

你可以点击观看 [YouTube 视频](https://youtu.be/gBJaL7Jqh4w)或者在本部分末尾找到它，这样你就可以回顾你所学的内容。

### Options/DOM:如何选择根 DOM 元素

Options/DOM API 为您提供了一个`el`属性，您可以使用它来选择一个现有的 DOM 元素，Vue 将使用它来挂载您的应用程序实例。

属性接受一个包含元素 CSS 选择器的字符串，或者直接接受一个 DOM 元素。

注意:Vue 不鼓励使用 body 或 HTML 标签，建议使用不同的元素作为挂载点。

让我们开始吧。在 index.html 文件的主体内，您需要放入以下代码:

```
 <div id="app">
    </div> 
```

现在您有了一个可以用来连接 Vue 实例的根元素。
回到 main.js 文件中，让我们在 options 对象中选择这个元素。

您现在可以使用`el`属性来选择您创建的 id 为`app`的元素。

```
let app = new Vue({
    // all options go here
    el: "#app",
}) 
```

您现在有了一个可以使用的元素。您可以继续下一步，将数据对象添加到选项对象中。

你可以在这里的文档中读到更多信息:[[https://vuejs.org/v2/api/#Options-DOM](https://vuejs.org/v2/api/#Options-DOM)

### 选项/数据:如何添加数据对象(或在组件中使用的函数)

当一个新实例被创建时，它将在其数据对象中发现的所有属性添加到 Vue 反应系统中。当数据对象中的值改变时，视图将反映这些改变。这是 VueJS 反应系统的基础。

为了解释它，我们来看一个实际的例子。

#### 创建数据对象

在 main.js 文件中，您可以创建一个以 object 为值的数据属性，如下所示:

```
 let app = new Vue({
    // all options go here
    el: "#app",
    data: {}
}) 
```

数据对象可以直接在 Vue 实例内部定义，如上面的代码所示，也可以在实例外部定义，如下面的代码所示。

```
let dataObject = {}
let app = new Vue({
    // all options go here
    el: "#app",
    data: dataObject
}) 
```

你可以挑你喜欢的。

#### 向数据对象添加属性

因为 VueJs 是一个 JavaScript 框架，所以记住你所知道的 JavaScript 在这里仍然是有价值的。

Vue 只是一个 JavaScript 对象，它有许多方法和属性，可以用来简化和加速您的工作流程。

让我们给数据对象添加一些属性，看看它是如何工作的。

```
// Create a data object
let app = new Vue({
    el:"#app",
    // create a vue instance, add the data property and the dataObject created
    data: {
        alert: "This is an alert message! ",
        projects: [
            {title: "portfolio", languages: ["HTML", "CSS", "VueJS"]},
            {title: "grocery shop", languages: ["HTML", "CSS", "PHP"]},
            {title: "blog", languages: ["HTML", "CSS", "PHP"]},
            {title: "automation script", languages: ["Python"]},
            {title: "eCommerce", languages: ["HTML", "CSS", "PHP"]},
        ];
    }
}) 
```

使用上面的代码，您只需向数据对象添加两个属性:一个`alert`属性和一个`projects`属性。

alert 属性只是一个字符串，而 projects 属性是一个对象数组。

现在您已经有了一些要处理的数据，让我们看看如何访问和修改它们的值。

#### 操纵数据对象中的属性

您可以使用包含 Vue 实例`app`的变量来访问和操作数据对象的属性。然后你可以使用点符号引用属性，比如`app.alert`。

在浏览器中，如果你打开控制台，你可以看到当你写`app`时，你得到了 Vue 实例对象。因此，像任何其他使用点符号的对象一样，您可以获得它的属性和方法。

让我们在控制台内部尝试一下:

```
// Access the alert property in the data object
app.alert // This is an alert message!
// update a data property value
app.alert = "This is a new alert message!" 
app.projects 
```

上面的代码做了三件简单的事情:

*   第一行访问`alert`属性并打印其内容“这是一条警告消息”
*   第二行用等号运算符给属性`alert`赋一个新值
*   最后，第三行返回项目数组的值。

您还可以使用快捷键＄data 或 _data 来访问整个数据对象

回到控制台:

```
// Access the entrie data object
app.$data // {__ob__: Observer} option 1
app._data // {__ob__: Observer} option 2 
```

你可以在这里的文档中了解更多信息:[[https://vuejs.org/v2/api/#Options-Data](https://vuejs.org/v2/api/#Options-Data)

### 选项数据方法

Vue 实例让您可以访问许多属性和方法。
您可以使用`$`符号来访问默认的方法和属性。它用于区分 Vue 定义的方法和用户定义的方法。

有许多预定义的实例方法和属性，分为四个不同的类别:

*   实例属性
*   实例方法/数据
*   实例方法/事件
*   实例方法/生命周期挂钩

例如，使用下面的代码，您可以获得`data`和`options`对象或者访问`watch`或`on`方法。

```
app.$data // returns the data object
app.$options // returns the options object
app.$watch() // function that watched for changes on the vue instance
app.$on() // listen for a custom event on the vue instance 
```

我不会深入探讨这个问题，因为它超出了本指南的范围。但是如果你感兴趣，想了解更多，这里有[文档](https://vuejs.org/v2/api/#Instance-Properties)。

### 生命周期挂钩

Vue 让你可以访问一系列被称为生命周期挂钩的功能。它们允许您在 Vue 初始化步骤的特定阶段运行代码。

在所有的生命周期钩子中，你可以访问指向 Vue 实例的`this`变量。

您将在以后的章节中更详细地了解这是如何工作的。但是现在，这是对可用挂钩的简短总结，以及它们允许您做的事情:

*   beforeCreate(可以在创建 Vue 实例之前运行代码)
*   已创建(可以在创建 Vue 实例后运行代码)
*   beforeMount(可以在元素挂载到 DOM 之前运行代码)
*   挂载(当元素挂载到 DOM 时，可以运行代码)
*   beforeUpdate(可以在 DOM 中更新值之前运行代码)
*   更新(可以在 DOM 中的值更新后运行代码)
*   销毁前(可以在实例销毁前运行代码)
*   销毁(当实例被销毁时，可以运行代码)

在课程中，我们会经常使用安装的挂钩。如果您想了解更多关于这个主题的信息，我建议您先看看文档中的图表。在这里找到生命周期挂钩图。

[https://www.youtube.com/embed/gBJaL7Jqh4w?feature=oembed](https://www.youtube.com/embed/gBJaL7Jqh4w?feature=oembed)

## 如何在 Vue 中使用模板

VueJS 使用 mustache 语法`{{ }}`来呈现 HTML 元素中 Vue 实例的数据。

使用这种语法，您可以获取 Vue 实例中定义的属性和方法。然后，该属性被分析并呈现到页面上。

你可以点击这里查看[库。](https://bitbucket.org/fbhood/how-to-vuejs/src/master/3-work-with-templates/)

你可以点击查看 [YouTube 视频这里](https://youtu.be/pDj3SQ8TNzs)这里，或者在本节末尾找到它来回顾你刚刚学到的内容。

### 文本数据绑定

这称为文本数据绑定。让我们看一个如何在 Vue 实例和模板文件之间绑定数据的例子。

```
 <div id="app">
    <h1>{{ title }}</h1>
</div> 
```

上面的代码在根元素中有一个 id 为`app`的`h1`标签，我们在前一章中已经定义了。

在`h1`标签中，使用双花括号语法将数据对象中的属性值呈现到页面上，该数据对象被称为`title`。

您的数据对象中还没有一个`title`属性，所以让我们添加它。

在 main.js 文件中

```
 let app = new Vue({
    el: "#app",
    data: {
        title: "John Doe portfolio",
        projects: [
            {title: "portfolio", languages: ["HTML", "CSS", "VueJS"]},
            {title: "grocery shop", languages: ["HTML", "CSS", "PHP"]},
            {title: "blog", languages: ["HTML", "CSS", "PHP"]},
            {title: "automation script", languages: ["Python"]},
            {title: "eCommerce", languages: ["HTML", "CSS", "PHP"]},
        ]
    }
}) 
```

现在，使用上面的代码，您可以在模板的`h1`标记中呈现属性`title`的内容。最终的结果将是这样的:

```
<h1>John Doe portfolio</h1> 
```

但是，使用这种方法，只能传递一个字符串。如果你想在字符串中使用 HTML 标签，这些标签将不会被解析，而是显示为简单的字符串。

例如，如果您将以下字符串分配给`title`属性

```
 title: "John Doe <span class='badge'>Portfolio</span>" 
```

然后尝试在我们的 HTML 中呈现它，如下所示:

```
<h1 class="title">{{title}}</h1> 
```

属性`title`将被呈现为一个包含 HTML 标签
ie 的普通字符串。`John Doe <span class='badge'>Portfolio</span>`

当然你也可以解析 HTML。

### 如何解析原始 HTML

为了呈现一个原始的 HTML 元素，我们需要引入另一个重要的 Vue 概念，叫做 directives。

在这种情况下，您将在 html 标记中使用 v-html 指令作为属性，并向其传递属性 title。

当您使用 Vue 指令时，引号内的文本被认为是 JavaScript 表达式。这意味着它被计算并且它的结果被渲染。

让我们用 HTML 标记为标题创建一个单独的属性，这样您就可以看到两者是如何呈现在页面上的。

```
let app = new Vue({
    el: "#app",
    data: {
        title: "John Doe Portfolio", 
        titleHTLM : "John Doe <span class='badge'>Portfolio</span>",
        projects: [
            {title: "portfolio", languages: ["HTML", "CSS", "VueJS"]},
            {title: "grocery shop", languages: ["HTML", "CSS", "PHP"]},
            {title: "blog", languages: ["HTML", "CSS", "PHP"]},
            {title: "automation script", languages: ["Python"]},
            {title: "eCommerce", languages: ["HTML", "CSS", "PHP"]},
        ]
    }
}) 
```

现在，在 HTML 文件中，您将使用这个`{{}}`语法来呈现属性`title`。但是在您想要呈现原始 HTML 的标签上，通过`titleHTML`属性，您使用 v-html 指令来代替。

```
<div id="app">
    <div class="title">{{ title }}</div>
    <div v-html="titleHTML"></div>
</div> 
```

这两个元素现在都可以正确地呈现，包括第二个包含 HTML 标记的属性。

注意:呈现 HTML 可能会暴露 XSS 漏洞。不要在用户提供的内容上使用这种方法。

现在，您已经知道了如何将数据呈现到页面上，让我们更深入地研究指令。

如果你想了解更多，请访问这里的文档
[。](https://vuejs.org/v2/guide/syntax.html#Using-JavaScript-Expressions)

[https://www.youtube.com/embed/pDj3SQ8TNzs?feature=oembed](https://www.youtube.com/embed/pDj3SQ8TNzs?feature=oembed)

## 指令视图

在 HTML 文件中，您可以使用指令与 HTML 属性进行交互。当指令的表达式发生变化时，指令会对 DOM 应用效果。

您可以点击此处查看[库](https://bitbucket.org/fbhood/how-to-vuejs/src/master/4-Directives/)

你可以点击此处查看 [YouTube 视频，或者你可以在本节末尾找到它来回顾你所学的内容。](https://youtu.be/LICvNmhsTEs)

### HTML 属性上的 v-bind 指令

到目前为止，您已经使用了`{{}}`语法来呈现 HTML 开始和结束标记之间的内容。但是在 HTML 标记中，您不能使用{{ }}语法。

那么，如何将 HTML 属性连接到 Vue 实例呢？您可以使用 v-bind 指令，它允许您像以前一样访问数据对象属性。

v-bind 指令是接受冒号后指定的参数的指令之一。在我们的例子中，冒号后面指定的是 HTML 属性名，比如 id、class、href、src 等等。

如果您需要动态地分配一个属性，比如 href 甚至一个类，您可以使用 v-bind 指令将它与 Vue 实例绑定。然后它将能够获得选项对象中的内容，比如数据对象中的属性。

让我们看看 v-bind 的运行，并开始连接`id`和`class`属性，这样您就可以用 Vue 动态地给它们赋值。

在我们的 index.html 档案里:

```
<div id="app">
    <div v-bind:class="dynamicClass" v-bind:id="dynamicId">Dinamically assign a class and an id to the div</div>
</div> 
```

让我们破解上面的代码，看看它在做什么。

首先，在根元素中有一个`div`标记。然后在类和 id 属性上使用 v-bind 指令。

在引号中，您指定了两个属性，稍后您将在 Vue 实例的数据对象中定义这两个属性。

请记住，当使用 Vue 指令时，引号之间的内容被视为 JavaScript 表达式。

让我们在 Vue 实例中定义这两个属性。

```
let app = new Vue({
    el: "#app",
    data: {
        title: "John Doe Portfolio", 
        titleHTLM : "John Doe <span class='badge'>Portfolio</span>",
        projects: [
            {title: "portfolio", languages: ["HTML", "CSS", "VueJS"]},
            {title: "grocery shop", languages: ["HTML", "CSS", "PHP"]},
            {title: "blog", languages: ["HTML", "CSS", "PHP"]},
            {title: "automation script", languages: ["Python"]},
            {title: "eCommerce", languages: ["HTML", "CSS", "PHP"]},
        ],
        dynamicId : "projects_section",
        dynamicClass : "projects"
    }
}) 
```

因此，您已经定义了`dinamicId: "projects_section"`和`dynmicClass: "projects"`属性，并为它们分配了两个值。

由于属性上的数据绑定，您的 HTML 标记将呈现如下(现在您可以动态地更改属性值，并看到它们的反应性变化):

```
<div id="projects_section" class="projects">Dynamically assign a class and an id to the div</div> 
```

### 使用布尔值进行 v 绑定

对于使用布尔值的属性，v-bind 指令的工作方式有所不同。只有当属性值为 true 时，它才会显示属性。在所有其他情况下，它不会呈现属性及其内容。

在下一个示例中，您将使用具有 disabled 属性的按钮。

在根 HTML 元素中:

```
 <div id="app">
        <button v-bind:disabled="disabled">You can't click this button</button>
    </div> 
```

在 Vue 实例内部:

```
let app = new Vue({
    el: '#app',
    data: {
    //disabled: false, // wont render the attribute
    //disabled: null, // wont render the attribute
    //disabled: undefined, // wont render the attribute
    disabled: true // renders the attribute
}
}) 
```

只有当 disabled 属性设置为 true 时，该属性才可见并呈现其属性内容。

```
 <button disabled>You can't click this button</button> 
```

这是在使用这些属性时需要记住的事情。

另一件要考虑的事情是，绑定可以包含单个 JavaScript 表达式，但有一些限制:

*   只允许表达式
*   只有一个表情
*   没有声明
*   没有流量控制工具，但是三元运算符有效。

如果你想了解更多，请访问这里的文档
:[[https://vuejs . org/v2/guide/syntax . html # Using-JavaScript-Expressions](https://vuejs.org/v2/guide/syntax.html#Using-JavaScript-Expressions)

到目前为止，我们只看到了两个 Vue 指令，v-html 和 v-bind。但是有许多可用的指令，这里还有一些(仅列出几个):

*   虚拟 html
*   v 型装订
*   虚拟中频
*   如果，否则
*   v-否则
*   v-for
*   所有的指令都有一个 v 前缀，但是有 v-bind(:)和 v-on (@)的简写。

他们以同样的方式工作。下面是 v-bind 和 v-on 指令的快速参考:

```
<!-- Long syntax -->
<a v-bind:href="url">Some link</a>
<!-- Shot syntax -->
<a :href="url">Some link</a>
<!-- Long syntax with dynamic arguments -->
<a v-bind:[attribute_name]="url">Some link</a>
<!-- Shot syntax with dynamic arguments -->
<a :[attribute_name]="url">Some link</a> 
```

v-on 的简写

```
<!-- Long syntax -->
<a v-on:click="runFunction">Some link</a>
<!-- Shot syntax -->
<a @click="runFunction">Some link</a>
<!-- Long syntax with dynamic arguments -->
<a v-on:[attribute_name]="runFunction">Some link</a>
<!-- Shot syntax with dynamic arguments -->
<a @:[attribute_name]="runFunction">Some link</a> 
```

现在让我们看看什么是动态论点，它们是如何工作的。

### Vue 中的动态参数

从 Vue 2.6.0 开始，指令就可以有动态参数了。如果将 JavaScript 表达式放在方括号中，就可以在指令参数中使用它。

但是有一些限制:

*   表达式的计算结果应该是字符串
*   空格和引号无效

让我们看一个实际的例子

```
 <a v-bind:[attribute_name]="url">Visit my Website</a> 
```

在您的数据对象中，您可以定义指令参数，就像它们是属性一样，其中属性值是您的 HTML 属性的名称，如下例中的`hef`:

```
let app = new Vue({
    el: '#app',
    data: {
        attribute_name: 'href',
        url: 'https://fabiopacifici.com'
    }
}) 
```

当您使用 v-bind 指令绑定属性名称`href`时，上面的代码会动态地呈现它的值。

结果将是这样的:

```
<a href="https://fabiopacifici.com">Visit my Website</a> 
```

### Vue 中的动态事件

您可以将相同的概念应用到像 v-on 这样的事件指令中。这个指令负责 JavaScript 事件监听器的工作。

v-on 接受 click 这样的参数，比如`v-on:click="doSomething"`。

为了应用动态性的概念，让我们创建一个 v-on 指令，并使用它后面的方括号来指定一个动态事件。

在 index.html 文件中，您将放置以下代码:

```
 <div id="app">
        <a v-on:[event_name]="runFunction">Some link</a>
    </div> 
```

我们来分解一下上面的代码。

首先你有你的根元素，带有`app`的`id`的`div`。在根元素中，添加一个锚标记`<a>Some link</a>`。

锚点标签中有一个`v-on`指令。在这个指令之后，你指定一个动态参数`v-on:[event_name]`，其中`event_name`将是你的 Vue 实例中的一个属性，你可以根据需要改变它。

v-on 指令像任何事件监听器一样工作，所以在引号之间，您需要指定当事件被触发时您想要运行的函数的名称，因此是`runFunction`。

现在，在 main.js 文件中:

```
let app = new Vue({
    el: '#app',
    data: {
    event_name: "click"
    },
    methods: {
        runFunction() {
            console.log("test click function");
        }
    }
}) 
```

让我们回顾一下上面的代码是做什么的。

首先，创建 Vue 实例。然后，在数据对象中添加`event_name`属性，并赋予它一个值`click`。这是你将收听的事件。

最后，我们说过当事件被触发时，v-on 指令运行一个函数，因此您需要在您的 Vue 实例中编写一个方法。所以在 methods 对象内部，创建一个名为`runFunction`的新函数，它将简单地在控制台内部输出一条消息。

当您用一个不同的事件名称替换`event_name`属性的值时，动态事件的威力就很明显了。

[https://www.youtube.com/embed/LICvNmhsTEs?feature=oembed](https://www.youtube.com/embed/LICvNmhsTEs?feature=oembed)

## Vue 中的方法

到目前为止，我们已经学习了如何在模板中使用 v-bind 指令来绑定数据。在下一节中，您将学习更多的指令——但是在深入之前，让我们快速讨论一下如何存储您的函数。

你可以在这里查看[库](https://bitbucket.org/fbhood/how-to-vuejs/src/master/5-Methods/)

如果你想回顾你学到的东西，你可以在 YouTube 上看到这部分的视频。

由于您正在一个大对象中工作，Vue 实例函数将采用方法的名称。正如您可能猜到的，Options 对象有一个名为`methods`的属性，您可以像存储数据一样存储函数。

在你的 Vue 实例中，定义一个你可以随意调用的方法——记住使用一个清晰描述你的代码的命名约定。

```
 let app = new Vue({
    el: '#app',
    data: {
        firstName: "Fabio",
        lastName: "Pacific" 
    },
    methods: {
        // es6 syntax
        getFullName(){
            return this.firstName + " " + this.lastName;
        }
        // es5 syntax
    /* getFullName: function(){

        } */
    }
}); 
```

在上面的代码中，您在 methods 对象中创建了一个方法。你称之为`getFullName`。在方法内部，您可以访问引用对象实例的`this`关键字，因此您可以使用它从方法中访问存储在数据对象中的属性。

当您调用方法`getFullName`时，该方法将返回一个包含名字和姓氏的字符串。

现在在你的 HTML 文件中，你可以像你需要访问数据对象`{{ getFullName() }}`中的属性时一样简单地调用这个方法

```
<div>{{ getFullName() }}</div> 
```

现在您已经知道了如何创建一个方法以及将它放在 Vue 实例中的什么位置，让我们继续学习更多关于指令的知识。

[https://www.youtube.com/embed/dESmaEvkZ2I?feature=oembed](https://www.youtube.com/embed/dESmaEvkZ2I?feature=oembed)

## Vue 中的条件句(v-if/v-else-if/v-else/v-show)

现在是时候学习更多关于指令的知识了。我们将从看看 VueJS 中条件是如何工作的开始。本节的第一个指令是`v-if`，它允许您基于特定条件呈现代码块。

你可以点击这里查看[库。](https://bitbucket.org/fbhood/how-to-vuejs/src/master/6-Conditionals/%5D)

您可以点击此处在 [YouTube 上观看视频，或者在本节末尾使用它进行回顾。](https://youtu.be/VNaCsloA1ZU)

像普通 JavaScript 中的`if-else`语句一样，v-if 将检查条件表达式的返回值是否为真。如果是这样，它将呈现 HTML 元素和你放在里面的所有东西。

因为它是一个指令，所以它作用于单个元素(HTML 标签)。如果您想在多个元素上扩展它的行为，那么您需要将它们包装在一个`<template>`标签中。

v-if 指令的工作方式与 v-bind 指令相同:它可以访问数据对象中的属性，并接受引号中的表达式。

如果表达式的返回值或您使用的数据属性的值计算为`true`，那么该指令将呈现 HTML 元素。否则不会。

当然，您可以检查多个条件，如果没有一个条件的计算结果为 true，那么您可以最终呈现一个元素。您可以将 v-if 与 v-else-if 和 v-else 指令一起使用。

让我们看一个简单的例子，并在 main.js 文件中编写一些代码来显示或隐藏一个元素。

首先要注意的是，如果您有一个返回布尔值的属性，那么在 v-if 指令中使用它来显示/隐藏一个元素就足够了，如下所示:

```
<h1 v-if="showTitle">{{movieTitle}}</h1> 
```

和一个 showTitle 属性设置为 true 的 vue 实例。

```
 let app = new Vue({
    el: "#app",
    data: {
        movieTitle: 'Shining',
        showTitle: true,
    }
}) 
```

在这种情况下，您说只有当`showTitle`的值为`true`时才显示 title 属性。如果将其更改为 false，标题将不会显示。

您可以将一个简单的表达式放在 v-if 指令的引号内，一旦计算出来，其结果为布尔值。

```
<h2 v-if="age >= 18">{{movieTitle}}</h2> 
```

在 main.js 文件中

```
let app = new Vue({
    el: "#app",
    data: {
        movieTitle: 'Shining',
        age: 18,
    }
}) 
```

在上面的代码中，我们在 v-if 指令上写了一个表达式，用于检查`age`属性是否大于或等于 18。如果结果为真，那么页面上将显示`h2`。

现在让我们来看一个更复杂的例子，使用 v-else-if 添加另一个条件。

#### 如果/否则如果

在下面的例子中，您将首先创建一个与上面类似的 v-if 条件——但是这次您将使用`&&`操作符检查用户是否超过 18 岁但低于 21 岁。

如果是真的，那么你将显示一个额外的注释时间。如果为 false，并且用户超过 21 岁，那么我们将只显示电影的标题。

```
<h2 v-if="age > 21">{{movieTitle}}</h2>
<h2 v-else-if="age > 18 && age < 21"> {{ movieTitle }} | Watch with an adult</h2> 
```

在 Vue 实例中，您可以拥有一个`age`属性。但是为了让你的简单程序更动态，你可以使用一个提示来询问用户的年龄。

```
let userAge = Number(prompt("What's your age?"))
let app = new Vue({
    el: "#app",
    data: {
        movieTitle: 'Shining',
        age: userAge,
    }
}) 
```

所以这里的代码首先询问用户的年龄，然后将结果作为一个数字存储在变量`userAge`中。

稍后，您将使用数据对象中的`userAge`变量为`age`属性赋值，以便根据其值呈现一个元素或另一个元素。

让我们继续使用 v-else 指令来显示一条不同的消息，以防用户不满 18 岁。

#### v-else 指令:

`v-else`指令的工作方式不同。你不需要传递任何东西。当前面的条件都不为真值时，它就开始起作用。

所以新的 HTML 元素相当简单:

```
 <div id="app">
        <h2 v-if="age > 21">{{movieTitle}}</h2>
        <h2 v-else-if="age > 18 && age < 21"> {{ movieTitle }} | Watch with an adult</h2>
        <p v-else> Sorry You are too young to see this movie</p>
    </div> 
```

这里我们有一个附加了 v-else 指令的`p`标签。正如您所看到的，它看起来像一个没有值的属性(就像禁用的或必需的 HTML 属性)。

您的 JavaScript 文件没有更改。

```
 let userAge = Number(prompt("What's your age?"))
let app = new Vue({
    el: "#app",
    data: {
        movieTitle: 'Shining',
        age: userAge,
    }
}) 
```

这就是你需要知道的关于条件渲染的全部内容，这样你才能继续你的第一个项目。但是如果你想了解更多，这里有文档:[[https://vuejs.org/v2/guide/conditional.html](https://vuejs.org/v2/guide/conditional.html)

在您能够构建您的第一个项目之前，您还需要学习一些东西，这是一个简化的 Twitter 克隆。下一个话题是关于循环的。

[https://www.youtube.com/embed/VNaCsloA1ZU?feature=oembed](https://www.youtube.com/embed/VNaCsloA1ZU?feature=oembed)

## Vue 中的循环

让我们回到前面的例子，学习如何使用 v-for 指令将数组的每个项目输出到页面上。

您可以点击此处查看资源库

你可以点击这里观看 YouTube 视频[，或者你可以在本节末尾找到它来回顾你所学的内容。](https://youtu.be/aViHg80-7Bs)

对于我们的下一个任务，如果我们可以使用一个循环，这将是非常有用的，v-for 指令就是用来帮助我们的。

它的语法与 JavaScript 中的经典`for`循环没有太多共同之处，而是与 Python `for in`循环或用于迭代对象的`for in` JavaScript 循环相似。

通过这个指令，您可以使用语法`project in projects`指定数组的元素和引号之间的单个元素。这里，项目是包含对象数组的数据对象内部的属性，项目是数组的单个元素。

您可以随意调用它，但是请记住，跟在`in`关键字后面的必须是来自您的数据对象的 iterable，而前面的可以是您想要引用 iterable 的每个元素的任何内容。

在您的情况下，project 似乎是最合适的选择，因为您有一系列的项目。

您的 JavaScript 文件将如下所示:

```
let app = new Vue({
    el: "#app",
    data: {
        name: "John Doe",
        title: "Portfolio",
        projects: [
            {title: "portfolio", languages: ["HTML", "CSS", "VueJS"]},
            {title: "grocery shop", languages: ["HTML", "CSS", "PHP"]},
            {title: "blog", languages: ["HTML", "CSS", "PHP"]},
            {title: "automation script", languages: ["Python"]},
            {title: "eCommerce", languages: ["HTML", "CSS", "PHP"]},
        ],

    }

}); 
```

现在在 HTML 文件中，让我们使用 v-for 来呈现每个项目的标题。

```
 <div id="app">
        <h1>{{name}} {{title}}</h1>
        <ul>
            <li v-for="project in projects">{{project.title}}</li>
        </ul>
    </div> 
```

在上面的代码中，您使用了`{{name}} {{title}}`来呈现您的投资组合的主标题。然后您使用 v-for 指令，并在 quests 中指定您想要将迭代的每个元素分配给一个项目变量`v-for="project in projects"`。

现在，在每次迭代中，`project`变量保存一个对象，您可以使用类似于`{{ project.title }}`的点符号从该对象中检索其属性。

需要注意的一点是，v-for 指令还允许您在每次迭代中访问元素的索引。您可以像处理名为 project 的单个元素一样，将它存储在一个变量中。

为此，您需要将它们括在括号中，并用逗号分隔元素及其索引，就像这样`v-for="(project, index) in projects"`。

另外，请注意，当处理对象时，Vue 会显示一个警告，通知您建议使用键。这意味着在呈现元素时，它需要一个键来标识每个元素。

您可以使用`key`属性并绑定它，例如绑定到对象的 id 属性或另一个不同的属性，如下所示

```
 <div id="app">
    <h1>{{name}} {{title}}</h1>
    <ul>
        <li v-for="project in projects" :key="project.title">{{project.title}}</li>
    </ul>
</div> 
```

这里，您使用 v-bind 速记指令将 key 属性绑定到 project.id 属性(如果存在),或者绑定到另一个属性(如果不存在)。

```
let app = new Vue({
    el: "#app",
    data: {
        name: "John Doe",
        title: "Portfolio",
        projects: [
            {title: "portfolio", languages: ["HTML", "CSS", "VueJS"]},
            {title: "grocery shop", languages: ["HTML", "CSS", "PHP"]},
            {title: "blog", languages: ["HTML", "CSS", "PHP"]},
            {title: "automation script", languages: ["Python"]},
            {title: "eCommerce", languages: ["HTML", "CSS", "PHP"]},
        ],

    }

}); 
```

v-for 也可以用来迭代对象。在这种情况下，您可以访问值、键以及索引，就像 so `v-for="(value, key, index) in object"`一样，其中“object”是数据对象中的一个属性。

如果你想了解更多，请访问这里的文档:[[https://vuejs.org/v2/guide/list.html](https://vuejs.org/v2/guide/list.html)

现在让我们转到 Vue 的另一个重要特性:如何处理用户输入和事件。

[https://www.youtube.com/embed/aViHg80-7Bs?feature=oembed](https://www.youtube.com/embed/aViHg80-7Bs?feature=oembed)

## 如何在 Vue 中使用事件处理(v-on)来处理用户输入

为了让应用程序对用户的输入做出反应，Vue 提供了一个名为`v-on`的简单指令。这是接受参数的指令之一，类似于 v-bind 指令。

有了这样的指令，很容易监听用户触发的事件。

v-on 指令允许您运行一个函数，当用户执行一个动作时，比如当他们点击一个按钮、悬停在一个元素上或者按下键盘上的一个特定键时，该函数执行一段代码。

你可以点击查看[库。](https://bitbucket.org/fbhood/how-to-vuejs/src/master/8-user-inputs-events-handling/)

你可以点击在 [YouTube 上观看视频，或者在本节末尾回顾你所学的内容。](https://youtu.be/9_U1eagqOJY)

### V-on 语法和事件

我们可以使用两种类型的语法，长格式或短格式。它们是等价的，所以挑一个你喜欢的。下面只是语法的一个表示，我一会儿会详细解释。

`v-on:EventName='doSomething'` 长语法:
短语法:`@EventName='doSomething'`

您可以收听许多活动，例如:

*   点击
*   使服从
*   鼠标悬停
*   鼠标输入
*   老鼠离开
*   好好享受吧
*   按键
*   键击器

但是您也可以创建自定义事件(当您到达 components 部分时就会看到)。

我们来挑选一下长格式语法:`v-on:EventName='doSomething`。我现在将更多地解释它。

首先，你有指令`v-on`。然后你有一个参数，它是你想要监听的事件名，比如`click`。之后，`doSomething`可以是您在 Vue 实例的 methods 对象中定义的任何方法。

该方法类似于您在 JavaScript 对象中定义的任何其他函数。它可以有参数，也可以没有参数。如果它有这些参数，您可以像往常一样调用该方法并将参数传递给它，如下所示:`doSomething(param, param_2, param3)`。

你可以有这样的东西`<div v-on:click="likeProject">Like</div>`，当用户点击这个元素时，它将触发一个方法并运行一些代码来增加一个项目中的 like 计数器。

让我们首先创建您需要的 HTML:

```
<div class="projects" v-for="(project,index) in projects">
    <h1>{{project.title.toUpperCase()}}</h1>
    <p>Lorem ipsum dolor sit amet.</p>
    <div>Like
        <i class="fas fa-heart fa-lg fa-fw" @click="likeProject(index)">
        </i>
        {{project.likes}}
    </div>
</div> 
```

在这里的代码中，首先您将使用 v-for 指令来遍历项目数组。注意，您应该使用语法`(project, index) in projects`,因为您需要将索引传递给之前定义的 like 方法。

之后，您将一些数据输出到页面上(比如大写字母的项目名称)，然后是描述，以及一个带有图标的`div`标签(记得添加字体 awesome 以获得图标)。

在心形图标上，使用短语法`@click="likeProject(index)"`在用于调用`likeProject(index)`方法的引号之间添加指令 v-on。然后将索引作为参数传递给它，这样就可以找到用户点击的当前项目。

最后，您将使用`{{project.likes}}`语法将 likes 呈现到当前项目的页面上。

现在是时候进入 Vue 实例并编写您的方法了。

```
 let app = new Vue({
    el:"#app",
    data: {
        projects: [
            {title: "My first project", description: "A simplified Twitter clone", likes: 0},
            {title: "My second project", description: "Projects portfolio with GitHub", likes: 0},
        ]
    },
        methods: {
            likeProject(index){
                const project = this.projects[index]
                project.likes++
                console.log(project.likes)
            }

    }
}); 
```

正如我前面说过的，您需要定义一个方法，当用户点击一个链接时调用这个方法。因此您创建了`likeProject`方法，该方法接受一个参数，该参数将是用户单击的元素的索引。

然后，您可以在项目数组中添加一个 likes 属性，并在当前项目中访问它，以便在用户每次单击您的链接时增加它的值。

### 如何访问原始事件

如果出于某种原因，您需要访问原始的 DOM 事件，您可以在方法中使用特殊的`$event`变量，就像在 v-on 指令中这样:`doSomething(param1, param2, $event)`。现在让我们来看一个例子。

您需要在 v-on 指令的方法调用中添加特殊变量，如下所示:

```
<i class="fas fa-heart fa-lg fa-fw" @click="likeProject(index, $event)">
        </i> 
```

然后，您可以访问方法中的原始事件，如下所示:

```
likeProject(index, event){
    console.log(event); // get the original event
    const project = this.projects[index]
    project.likes++
    console.log(project.likes)
} 
```

现在您已经知道了 v-on 指令是如何工作的，让我们改进 Likes 示例并在其中加入更多内容。我们将在下一个例子中使用关键修饰符，所以让我们快速看看它们是什么，以及你可以用它们做什么。

### Vue 中的事件修改器

通过事件，Vue 提供了对许多事件修改器的访问。他们被分成四大类。您可以将这些修饰符添加到指令中，以更改事件的行为方式。它们就像后缀，你可以用点符号来链接它们。

下面有一个快速参考。

类别:

*   事件修改器
*   关键修饰符
*   系统修改键
*   鼠标按钮修饰符

事件修饰符:
。停止
。防止
。捕捉
。自我
。一次
。消极的

按键修饰符:
例如，您可以将这些修饰符添加到@keyup 侦听器中，以便在这些按键被按下时进行侦听，或者将它们与@click 事件结合使用，以便侦听 click+空格。`@click.enter="doSomething"`

。输入
。标签
。删除(捕获“删除”和“退格”键)
。esc
。太空
。上升
。向下
。左
。正确

系统修饰符:
使用这些修饰符，当相应的键被按下时，你可以触发鼠标或键盘事件监听器。
。ctrl
。alt
。转移
。meta
。精确(允许控制触发事件所需的系统修饰符的精确组合)

鼠标按钮修饰符:
这些修饰符允许你在点击相应的鼠标按钮时触发一个鼠标事件监听器。

。左
。右
。中间

如果你想了解更多，请阅读这里的文档。

### 如何喜欢一个有关键修饰词的项目

在前面的例子中，您使用了 v-on:click 指令来触发一个鼠标事件监听器，该监听器的目标是模拟项目中的 like。

但是用户可以通过点击图标来添加他们想要的喜欢。

在下一个例子中，你将做一些不同的事情。

*   首先，您将防止用户向每个项目添加多个 like，
*   然后你会让用户删除一个赞
*   最后，即使在用户刷新页面后，您也要在页面上保留赞。

让我们开始吧。这一次，您将使用鼠标按钮修饰符来监听滴答声。鼠标左键点击将触发添加相似行为，鼠标右键点击将触发移除行为。

在 HTML 文件中:

```
<div id="app">

    <!-- Users can like a project with a left click and dislike it with right click -->

        <div class="projects" v-for="project in projects">
            <h1>{{project.title.toUpperCase()}}</h1>
            <p>Lorem ipsum dolor sit amet.</p>
            <div>Like 
                <i class="fas fa-heart fa-lg fa-fw" 
                    @click.left="addLike(project)" 
                    @click.right="removeLike(project, $event)">
                </i> 
               {{project.likes}}
            </div>
        </div>

</div> 
```

在上面的代码中，您使用了之前的代码，并简单地向 click 事件`@click.left`添加了一个鼠标左键按键修饰符。然后您调用了`addLike`方法。这将使你的项目像我们之前看到的那样增加一个计数器。

然后，您向同一个元素添加了另一个事件监听器，但是这一次您使用了`.right`鼠标按键修饰符来监听用户使用右键`@click.right="removeLike()"`点击我们的图标。

在 remove like 方法中，您还传递了特殊变量$event，这样您就可以在稍后的方法中使用原始事件来防止其默认行为并打开上下文菜单。

但是我们之前说过，你也可以链接按键修饰符，实际上这里有一个`.prevent`按键修饰符可以用来代替`$event`变量。你也可以这样做:`@click.right.prevent="removeLike(project)"`

让我们看看如何构建 main.js 文件:

```
let app = new Vue({
    el: "#app",
    data: {
        name: "John Doe",
        title: "Portfolio",
        projects: [
            {title: "My first project", description: "A simplified Twitter clone", likes: 0},
            {title: "My second project", description: "Projects portfolio with GitHub", likes: 0},
        ]
    },
    methods: {
        addLike(project){
           console.log(project)  
        },
        removeLike(project, event){
            console.log(project)
            console.log(event)
        }
    }

}); 
```

所以在数据对象中，有一个`projects`属性，它是一个对象数组。每个对象都有一个 likes 属性，您可以根据用户单击的鼠标按钮增加或减少该属性。

在`methods`对象中，您创建了在 v-on 指令中引用的两个方法`addLike()`和`removeLike()`。目前，您只将项目参数值和事件值记录到控制台。您将在一分钟内实现该逻辑。

让我们从添加喜欢的方法开始——它可能是这样的:

```
addLike(project){
    const projectTitle = project.title;
    if(!localStorage.getItem(projectTitle)) {
        project.likes++;
        localStorage.setItem(projectTitle, true);
    }              
} 
```

这里发生了一些事情。在第一行中，您将项目标题存储在`projectTitle`变量中。然后，您说如果您刷新页面，您希望数据持久化，所以您使用`localStorage` API 在客户端浏览器中存储信息。

您将赞数加 1，但这取决于本地存储中的一个值。

您可以通过首先检查`localStorage`中是否有一个键与您的项目标题`if(!localStorage.getItem(projectTitle))`相匹配来做到这一点。

如果结果为 false，那么您将运行 If 块中的代码，并首先递增 like`project.likes++`。

其次，使用本地存储 API 的`.setItem()`方法设置一个键-值对，将项目标题作为键，将一个布尔值作为其值`localStorage.setItem(projectTitle, true)`。

要将一个项目放入本地存储，您将使用`localStorage.setItem()`。set item 方法接受键值对。您的密钥将是您保存在变量`projectTitle`中的标题，值将是布尔值`true`。

现在让我们看看这是否可行。

```
removeLike(project, event){
    event.preventDefault(); // This can be omitted if we use the prevent key modifier
    const projectTitle = project.title;
    if(project.likes > 0 && localStorage.getItem(projectTitle)) {
        project.likes--;
        localStorage.removeItem(projectTitle);
    }
} 
```

这个函数的作用与前面的相反。当用户点击他们的鼠标右键时，方法`removeLikes()`被执行，你执行以下操作:

首先，你需要防止它的默认行为。否则，当用户右键单击图标时，上下文菜单将弹出，我们不希望这样。因此，您将对原始事件使用`event.preventDefault()`方法，该事件由方法中的`event`参数表示。

或者，如果您在 v-on 指令`@click.right.prevent="removeLike(project)`中使用了防止键修饰符，您可以省略它。

接下来就是抢项目标题了。因为您还向该方法传递了一个参数来表示当前项目对象`removeLike(project, event)`，所以您可以将项目标题存储在变量`projectTitle`中。

然后你需要做一些检查。首先，只有当 likes 的值大于零时，才希望减少 likes。然后，您希望确保项目标题在本地存储中作为一个带值的键。

所以，以你的身体状况，你已经完成了两项检查。现在，如果两个条件都为真，if 块中的代码就可以运行了。

首先你通过减少它的值`project.likes--`来删除它。然后您使用`removeItem`方法从本地存储中删除项目标题，并向其传递您想要删除的键(这是项目标题`localStorage.removeItem(projectTitle)`)。

综上所述，您现在应该有以下代码:

```
let app = new Vue({
    el: "#app",
    data: {
        name: "John Doe",
        title: "Portfolio",
        projects: [
            {title: "My first project", description: "A simplified Twitter clone", likes: 0},
            {title: "My second project", description: "Projects portfolio with GitHub", likes: 0},
        ]
    },
        methods: {
        addLike(project)
        {
            //console.log(project, "like");
            const projectTitle = project.title;
            // check if the current project is not in the local storage
            if(!localStorage.getItem(projectTitle)) {
                // set the item in the storage and increase the likes counter
                project.likes++;
                localStorage.setItem(projectTitle, true);
            }

        },
        removeLike(project){ 
            const projectTitle = project.title;
            console.log(project, "dislike");  
            if(project.likes > 0 && Boolean(localStorage.getItem(projectTitle))) {
                project.likes--;
                localStorage.removeItem(projectTitle);
            }

        }
    },
    mounted(){
        this.projects.forEach(project => {
            if(localStorage.getItem(project.title) !== null) {
                project.likes = 1; 
            }
        });
    }

}); 
```

为了让代码工作，您还需要添加一个名为 mounted 的生命周期挂钩。这将允许您在 Vue 实例上挂载根元素时运行代码。使用它，您可以检查 localStorage 是否有与您的项目标题对应的键，如果有，更新 likes 计数器的值。

而你的 HTML 还是老样子:

```
<div id="app">
    <!-- Users can like a project with a left click and dislike it with right click -->

    <div class="projects" v-for="project in projects">
        <h1>{{project.title.toUpperCase()}}</h1>
        <p>Lorem ipsum dolor sit amet.</p>
        <div>Like 
            <i class="fas fa-heart fa-lg fa-fw" 
                @click.left="addLike(project)" 
                @click.right="removeLike(project, $event)">
            </i> 
            {{project.likes}}
        </div>
    </div>
</div> 
```

请记住，您可以通过使用事件键修饰符来删除传递给`removeLike`方法的`$event`变量，如下所示:`@click.right.prevent="removeLike(project)"`

到目前为止你已经学到了很多！现在您已经看到了关键修饰符的作用，我们可以进入下一个主题:双向模型绑定和 v-model 指令。然后，我们将开始构建我们的 Twitter 克隆。

[https://www.youtube.com/embed/9_U1eagqOJY?feature=oembed](https://www.youtube.com/embed/9_U1eagqOJY?feature=oembed)

## Vue 中的双向模型绑定(v 模型)

好了，到目前为止，我们已经看到了如何将数据对象的属性绑定到我们的 HTML 标签和内部属性，
如何循环一系列元素，以及如何使用条件将有条件的元素显示到我们的模板上。

我们已经介绍了如何在 methods 对象中定义方法，这样我们就可以对数据执行更复杂的操作，并且我们已经学习了如何使用 v-on 指令处理事件。

在下一节中，我们将看看 Vue 如何在表单的输入和数据对象中定义的属性
之间打开一个双向通信通道。然后我们会用这些知识来共同构建我们的第一个项目。

你可以点击查看[仓库。](https://bitbucket.org/fbhood/how-to-vuejs/src/master/9-two-way-binding/)

如果你想回顾你所学的内容，你可以点击在 [YouTube 上观看视频，或者在本节末尾观看。](https://youtu.be/pBUXTUvDRCo)

### v-model 指令是如何工作的？

v-model 是另一个 Vue 指令。您可以直接使用它，它有助于简化输入标签与数据对象中 Vue 实例属性的通信方式。

它像所有其他指令一样工作。主要区别在于，当它被实现时，您的应用程序将使用这个 v-model 指令监听输入中的变化
,并立即更新数据对象中附加属性的值，反之亦然。

它实际上是模板和 Vue 实例之间的双向沟通渠道。这是与用户输入交互并简化开发人员生活的 Vue 方式。

让我们看一个简单的例子。

### 如何在输入标签上使用 v 模型

首先，在 Vue 实例中定义为根元素的元素内需要一个输入标记，即 id 为`app`的 div。

```
<div id="app">
    <h2>What do you want to tweet about today?</h2>
    <input type="text" v-model="tweet" placehoder="What's happening today?">
</div> 
```

在 HTML 中有一个 input 标记，它将 v-model 指令作为 HTML 属性附加到它上面。引号之间的任何内容都被计算为一个 JavaScript 表达式，因此您可以编写将在 Vue 数据对象中创建的属性名称`tweet`。

所以让我们开始吧。

```
let app = new Vue({
    el: '#app',
    data: {
        tweet: ""
    }
}); 
```

现在您有了一个 Vue 实例，其中有一个带有 tweet 属性的数据对象，该属性的值为空字符串。

如果打开控制台并检查 Vue 元素，可以看到双向数据绑定正在运行。

通过更改 tweet 属性的值，您将立即更新输入标记中的值，反之亦然。

数据对象中有这个`tweet`属性，并且已经知道如何将其内容呈现到页面上。所以现在您可以更新您的标记并在 input 标记下添加一个段落，以便在您键入时查看动态变化的值。

```
<div id="app">
    <h2>What do you want to tweet about today?</h2>
    <input type="text" v-model="tweet" placehoder="What's happening today?">
    <p>{{tweet}}<p>
</div> 
```

多酷啊。现在，您可以看到 tweet 属性在您键入时实时变化。

这就是双向数据绑定。如果您直接更改 tweet 属性的内容，它也会反映在您的模板中。

如果你想了解更多，请务必阅读官方的[文档](https://vuejs.org/v2/guide/forms.html)。

### 如何建立一个推特盒子

现在，让我们把标准提高一点，一起建造一些东西。

*   我们将创建一个简单的带有提交按钮的`textarea`，
*   我们将显示用户输入时剩余的字符数，以便他们可以提交表单，而不会超过允许的最大字符数。
*   就像在 tweet 中一样，最多 200 个字符。

#### 如何定义初始标记

现在您需要定义一个标记。因此，在我们的 index.html 文件中，编写以下代码:

```
<div id="app">
    <h2>What do you want to tweet about today?</h2>
    <form v-on:submit.prevent="submitData">
       <!-- Code here -->

    </form>
    <!-- More code here -->
</div> 
```

首先，您已经为 Vue 实例创建了根元素，这样它就可以监视标记并发挥它的魔力。

然后您使用指令 v-on 创建了一个带有事件监听器的表单标签，它监听提交事件并运行一个您仍然需要创建的函数`submitData`。

您还添加了`.prevent`修饰符，这样当您提交表单时页面就不会刷新。

#### 如何定义 Vue 实例和方法

让我们定义我们的 Vue 实例并创建`submitData`方法，以便您以后需要时可以使用它。

```
 let app = new Vue({
    el: '#app',
    data: {
        // data object props here
    },

    methods: {
          submitData(){
             // Code here
        }
    },

}); 
```

#### 如何添加文本区域和提交按钮

现在回到 HTML:让我们在表单中添加文本区域。

```
<div id="app">
    <h2>What do you want to tweet about today?</h2>
    <form v-on:submit.prevent="submitData">
       <!-- Code here -->
        <div class="form_group">
            <label for="name">Tweet</label>
            <textarea name="tweet" id="tweet" cols="80" rows="10" v-model="tweet" maxlength="200"></textarea>

        </div>

        <button type="submit">Tweet</button>

    </form>
    <!-- More code here -->
</div> 
```

在表单标签中，您为您的 tweet box 创建一个标签和一个`textarea`。
在`textarea`上，您使用指令 v-model 将`textarea`值绑定到`tweet`属性，反之亦然。所以现在当一个改变时，另一个也会改变。

注意:v-model 指令用于表单元素，如输入、文本区域、复选框等。

在`textarea`之后，你放置一个 submit 类型的按钮，这样当用户点击它时，表单的数据被发送到你的应用程序的`submitData()`方法，你可以处理它们。

#### 如何向 Vue 实例添加属性

现在，在您的 JavaScript 文件中，您需要在数据对象中创建 tweet 属性，并对该信息做一些处理，以便稍后可以显示发送的 tweet 列表。

我们还说过，我们希望将字符限制在 200 个以内，当超出时会显示错误。

因此，让我们在这里添加一些属性，如:

*   `tweet`对于用户在文本区输入的当前 tweet 消息
*   `tweets`获取推文列表
*   `max_length`为字符限制

```
 let app = new Vue({
    el: '#app',
    data: {
        tweets: [],
        tweet: "",
        max_length: 200,  
    },

    methods: {
          submitData(){
              /* Handle the tweet */
        }
    },

}); 
```

所以现在使用 tweets 属性作为数组，并使用 tweet 属性和`textarea`之间的两天绑定，当用户通过触发
`submitData`方法提交表单时，您可以将所有 tweets 推送到`tweets`数组中。

#### 如何实现字符计数器

在实现`submitData`方法之前，您可以在用户在 textarea 中键入时显示一个字符计数器。

让我们实现这个特性，这样用户就知道他们是否可以提交推文。

回到 HTML 文件，您可以添加一个带有几个 span 元素的 div，并使用 v-if 指令来检查字符的长度。当用户在字符限制内时，它将显示计数器，否则它将显示一条错误消息。

```
<div id="app">
    <h2>What do you want to tweet about today?</h2>
    <form v-on:submit.prevent="submitData">
        <div class="form_group">
            <label for="name">Tweet</label>
            <textarea name="tweet" id="tweet" cols="80" rows="10" v-model="tweet"></textarea>        
        </div>

        <button type="submit">Tweet</button>

    </form>
    <!-- Show character limits here -->
    <div>
        <span v-if="tweet.length < max_length"> {{ `Max: ${tweet.length} of ${max_length} characters` }}
        </span>
        <span class="errorMessage" v-else>{{`Max char limit reached! excess chars: ${max_length - tweet.length}`}}</span>
    </div>

</div> 
```

上面的代码使用 tweet 属性和`textarea`之间的双向数据绑定来确定用户是否达到了您定义为`max_lenght`属性的字符限制。

因为 tweet 属性连接到了`textarea`，所以您可以使用结合了`tweet.length`和`max_length`属性的`v-if`指令来进行比较。

现在，每当用户在`textarea`中键入一些内容，保存在`tweet`属性中的字符串就会增加一个字符。然后，您可以使用`.length`属性查看整个字符串有多长，并将其与您的`max_length`属性进行比较。

您使用指令`v-if="tweet.length <= max_length"`进行比较。当这个比较返回 true 时，用户将看到 span 标签及其内容，即计数器。

在 span 标记中，您使用了小胡子语法向用户显示属性`tweet`的当前长度和字符限制。

```
<span v-if="tweet.length < max_length"> {{ `Max: ${tweet.length} of ${max_length} characters` }}</span> 
```

在`v-if`指令之后，`v-else`指令处理当没有剩余字符可用时显示给用户的错误消息。

这里，span 元素的内容显示了一条消息，它通过从`max_lenght`属性中减去 tweet 长度来告诉我们有多少字符是多余的。

```
<span class="errorMessage" v-else>{{`Max char limit reached! excess chars: ${max_length - tweet.length}`}}</span> 
```

#### 如何提交表格

剩下的就是将 tweet 添加到 tweet 列表中，并在用户提交表单时显示在页面上。

让我们完成`submitData`方法，这样每次执行时，它都会向 tweets 数组推送一个新对象。

在方法对象内部,`submitData`方法现在看起来像这样:

```
 submitData(){
    if (this.tweet.length <= this.max_length) {
        this.tweets.unshift(this.tweet);
        this.tweet = "";
    } 
} 
```

上面的方法首先检查`tweet`属性的长度是否小于或等于`max_length`属性。如果条件评估为真，那么您可以使用`unshift`方法将`tweet`内容添加到数组中(将其添加到数组的开头)。

最后，您需要清除`tweet`属性的值。为此，您可以再次为其分配一个空字符串。

注意，因为您在一个方法内部，所以您需要使用`this`关键字来获取 Vue 实例内部的属性和最终方法。

#### 如何显示推文列表

现在，您还可以在模板中显示推文列表。

为此，您将使用一个`v-for`指令并遍历`tweets`数组来显示每条 tweet。

```
<ul>
    <li v-for="text in tweets">{{text}}</li>
</ul> 
```

### 把它们放在一起

最终的代码现在看起来像这样:

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VueJs v-model</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"
        integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">

    <!-- Fontawesome CDN -->
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.1.1/css/all.css"
        integrity="sha384-O8whS3fhG2OnA5Kas0Y9l3cfpmYjapjI0E4theH4iuMD+pLhbf6JI0jIMfYcK3yZ" crossorigin="anonymous">
    <!-- VueJS CDN -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.12/dist/vue.js"></script>
    <style>

    </style>
</head>

<body>
    <div id="app" class="container">

        <h2>What do you wanna tweet about today</h2>

        <!-- Tweet form -->
        <form v-on:submit.prevent="submitData">

            <div class="form-group">
                <label>Tweet</label>
                <textarea class="form-control" cols="30" rows="5" v-model="tweet"></textarea>
            </div>

            <button type="submit" class="btn btn-primary">Tweet</button>
        </form>

        <!-- Alert the user  -->
        <div class="my-3">
            <span v-if="tweet.length < max_length">
                {{ ` Max: ${tweet.length} of ${max_length} characters` }}
            </span>
            <span class="alert alert-danger" v-else> {{ `Max char limit reached! excess characters: ${max_length -
                tweet.length} ` }}</span>

        </div>

        <!-- Tweets message -->
        <ul>

            <li v-for="tweet in tweets">
                {{tweet}}
            </li>
        </ul>

    </div>

    <script src="./main.js"></script>

</body>

</html> 
```

我们最终的 javascript 文件

```
 let app = new Vue({
    el: '#app',
    data: {
        tweet: "",
        tweets: [],
        max_length: 200        
    }, 
    methods: {
        submitData(){
            // Handle the tweet submission
            if(this.tweet.length <= this.max_length){
                this.tweets.unshift(this.tweet);
                this.tweet = "";
            }
        }
    }
}) 
```

### 您可以做出的改进

如果你从你的 index.html 文件中取出这段代码，你可以做一些事情来清理我们的代码...

```
<!-- Show the max char messages -->
<div>
    <span v-if="tweet.length < max_length"> {{ `Max: ${tweet.length} of ${max_length} characters` }}
    </span>
    <span class="errorMessage" v-else>{{`Max char limit reached! excess chars: ${max_length - tweet.length}`}}</span>

</div> 
```

要清理这个模板文件，我们可以遵循两种完全相同的方法，只是一种是缓存的，另一种不是。

*   计算属性(缓存)
*   方法(未缓存)

在下一节中，我们将了解什么是计算属性，以及它们与方法有何不同。

[https://www.youtube.com/embed/pBUXTUvDRCo?feature=oembed](https://www.youtube.com/embed/pBUXTUvDRCo?feature=oembed)

## 计算属性和方法

对于复杂的逻辑，您应该使用计算的属性而不是模板中的表达式，这些逻辑可以改变数据的表示，而不是数据本身。

如果我们需要改变数据，那么你应该使用方法。计算属性基于它们的依赖关系进行缓存，这意味着只有当它的依赖关系发生变化时，它才会重新计算。

对于计算属性，如果依赖关系没有改变，则返回先前运行的函数的结果。

你可以点击这里查看[知识库，点击这里](https://bitbucket.org/fbhood/how-to-vuejs/src/master/10-computed-properties/)观看 [YouTube 上的视频。视频也列在这一部分的末尾，以便您可以回顾您所学的内容。](https://youtu.be/VxFT6cgTHhw)

在下面的例子中，我们将使用计算的属性，但是我们也可以使用方法。一般来说，当我们有一个昂贵的操作要执行并缓存时，我们使用计算属性，这样下次我们就不必再次运行它，除非发生了变化。

让我们为下面的消息实现一个计算属性。我们的 HTML 文件现在将改变如下:

```
<!-- Show the max char messages -->
<div>
    <span v-if="tweet.length < max_length"> {{ `Max: ${tweet.length} of ${max_length} characters` }}
    </span>
    <span class="errorMessage" v-else>{{`Max char limit reached! excess chars: ${max_length - tweet.length}`}}</span>

</div> 
```

到这个更干净的版本:

```
<!-- Show the max char messages -->
<div>
    <span v-if="tweet.length < max_length"> {{ maxCharsText }}
    </span>
    <span class="errorMessage" v-else>{{errorMessage}}</span>

</div> 
```

我们用两个新属性替换了两个跨度的内容，这两个新属性将作为方法放在我们的计算对象中。

现在，在我们的 Vue 实例中，我们将创建一个名为`computed`的新对象，其中我们将定义两个方法来返回我们之前拥有的消息。

```
let app = new Vue({
    el: '#app',
    data: {
        tweets: [],
        tweet: "",
        max_length: 200,  
        error: ""
    },
    // Computed Properties
 computed: {
        maxCharsText: function(){
            return `Max: ${this.tweet.length} of ${this.max_length} characters`;
        },
        errorMessage: function(){
            return `Max char limit reached! excess chars: ${this.max_length - this.tweet.length}`
        }
    },
    // Methods
 methods: {
          submitData(){
              if (this.tweet.length <= this.max_length) {
                  this.tweets.unshift(this.tweet);
                  this.tweet = "";
              } 
        }
    },

}); 
```

第一个方法`maxCharsText`返回与我们之前在 HTML 文件中的字符串完全相同的字符串。唯一的区别是我们使用关键字`this`来引用我们需要在 Vue 实例`this.tweet.length`和`this.max_length`中获取的属性。

第二种方法以完全相同的方式工作，它也使用关键字`this`来选择在 Vue 实例`this.max_length`和`this.tweet.length`中定义的属性。

### 一起

```
<div id="app">
    <h2>What do you want to tweet about today?</h2>
    <form v-on:submit.prevent="submitData">
        <div class="form_group">
            <label for="name">Tweet</label>
            <textarea name="tweet" id="tweet" cols="80" rows="10" v-model="tweet"></textarea>

        </div>

        <button type="submit">Next</button>

    </form>

    <div>
        <span v-if="tweet.length < max_length"> {{ maxCharsText }}
        </span>
        <span class="errorMessage" v-else>{{errorMessage}}</span>

    </div>
    <ul>
        <li v-for="text in tweets">{{text}}</li>
    </ul>
</div> 
```

JavaScript 文件

```
let app = new Vue({
    el: '#app',
    data: {
        tweets: [],
        tweet: "",
        max_length: 200,  
        error: ""
    },
    // Computed Properties
 computed: {
        maxCharsText: function(){
            return `Max: ${this.tweet.length} of ${this.max_length} characters`;
        },
        errorMessage: function(){
            return `Max char limit reached! excess chars: ${this.max_length - this.tweet.length}`
        }
    },
    // Methods
 methods: {
          submitData(){
              if (this.tweet.length <= this.max_length) {
                  this.tweets.unshift(this.tweet);
                  this.tweet = "";
              } 
        }
    },

}); 
```

如果我们想使用方法而不是计算属性，我们可以简单地将两个方法从`computed`对象移到`methods`对象中，并在 HTML 文件中用括号调用它们，如下所示:

```
<div>
    <span v-if="tweet.length < max_length"> {{ maxCharsText() }}
    </span>
    <span class="errorMessage" v-else>{{errorMessage() }}</span>

</div> 
```

请记住，计算出的属性会被缓存，而方法不会。如果你想了解更多，请务必阅读官方的[文档](https://vuejs.org/v2/guide/computed.html)。

[https://www.youtube.com/embed/VxFT6cgTHhw?feature=oembed](https://www.youtube.com/embed/VxFT6cgTHhw?feature=oembed)

## 如何创建一个简单的 Twitter 克隆

现在，让我们把我们到目前为止学到的所有东西放在一起，并建立我们的第一个项目。这将是一个极简和简化的类似 twitter 的网络应用。

我们希望创建一个简单的应用程序，它有某种注册表单，一个添加新推文的框，以及一个可以显示所有推文的部分。我们也希望能够删除推文。

所有数据必须是持久的，以便在页面刷新后，推文列表仍然可见，而注册表单将被隐藏。

你可以点击在 [YouTube 上观看视频，如果你想回顾，也可以在本部分末尾观看。](https://youtu.be/v1j_bDDd6jI)

您还可以在这里查看存储库。

### 定义我们的任务

让我们先把它分解成大任务。然后我们会看到我们需要做些什么来完成每一个。

*   创建注册表单
*   创建一个 tweets box 表单
*   创建一个推特板块

为了做我们的项目，我们需要研究我们需要使用什么工具来实现我们的目标，所以让我们把它们写下来:

*   应用程序逻辑
*   本地存储(使数据持久)
*   字体真棒(图标)

这个应用程序没有数据库，所以我们无法记录多个用户和他们的推文。这只是一个概念的证明，是我们用新知识构建的东西。

### 创建项目结构

现在我们知道要做什么了，让我们开始创建我们的项目结构并导入我们需要的工具来完成第一个任务，注册表单。

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple tweetter clone</title>
    <!-- CDN Fontawesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/css/all.min.css" integrity="sha512-+4zCK9k+qNFUR5X+cKL9EIR+ZOhtIloNl9GIKS57V1MyNsYpYcUrUeQc9vNfzsWfV28IaLL3i96P9sdNyeRssA==" crossorigin="anonymous" />

    <!-- VueJS development version, includes helpful console warnings -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <!-- Style sheet -->
    <link rel="stylesheet" href="style.css">
</head>

<body>
<div id="app">
    <!-- Register an account -->

    <!-- Add a tweet -->

    <!-- Show all tweets -->

</div>
<!-- Link our main.js file -->
<script src="./main.js"></script>
</body>

</html> 
```

现在我们的 HTML 文件已经准备好了，让我们创建 main.js 文件并创建一个 Vue 实例。

```
 let app = new Vue({
    el: '#app',
    data: {

    },
    methods: {

    }

}); 
```

最后，我们需要创建一个 style.css 文件，现在我们将它放在项目的根文件夹中。

我们将使用一个我已经写好的 CSS 文件，你可以从[这里](https://bitbucket.org/fbhood/simple-tweet-app/src/master/style.css)下载。

好了，我们的基本结构已经准备好了。在我们的 HTML 文件中，我们有一些反映我们 3 个主要任务的评论:创建一个注册表单，创建一个添加 tweet 框，并显示 tweet 列表。

让我们从第一个任务开始，模拟一个注册表单。

### 如何模拟一个注册表单

在根元素`<div id="app"></div>`中，我们需要创建一个包含以下字段的注册表单:姓名、电子邮件、密码和提交按钮。该表单包含在一个卡片中，所以我们将把所有内容包装在一个 div 中，并给它分配一个 card 类。

该表单不会向服务器提交数据，而是简单地模拟注册并更新 Vue 实例的数据对象中的一些属性。

我们将在 HTML 文件中放置以下代码:

```
<!-- Register an account -->
<div class="card">
    <i class="fab fa-twitter fa-lg fa-fw"></i>

    <h2>Create your account</h2>
    <form v-on:submit.prevent="registerAccount">
        <div class="form_group">
            <label for="name">Name</label>
            <input type="text" v-model="name" maxlength="25" required>
        </div>
        <div class="form_group">
            <label for="email">Email</label>
            <input type="email" v-model="email" maxlength="25" required>
        </div>
        <div class="form_group">
            <label for="password">Password</label>
            <input type="password" v-model="password" maxlength="16" required>
        </div>
        <button type="submit">Register</button>
    </form>

</div> 
```

我们来分析一下。首先，form 标签有一个带`submit`参数的`v-on`指令，因此它将监听提交事件。它还有一个事件修饰符`.prevent`，所以当我们点击提交按钮时，页面不会刷新。

在`v-on`指令中有一个叫做`registerAccount`的方法，我们需要在 Vue 实例的方法中创建它。

在表单中，我们有三个输入字段:名称、电子邮件和带标签的密码。我们用类`form_group`将每个字段包装在一个 div 中。稍后，我们可以复制 Twitter 注册字段的样式，并显示一个字符计数器。

每个输入字段都有一个`v-model`指令，将输入绑定到它的数据属性。

### 如何模拟一个注册表单

让我们转到 Vue 实例，在表单和数据对象属性之间进行绑定。

这里我们还需要一个地方来存储用户提交的详细信息，因此我们将为此创建另一个属性。

查看输入字段，我们还为它们添加了一个`maxlength`属性，25 表示姓名和电子邮件，16 表示密码。

就像我们之前学习 v-model 时所做的那样，我们可以为这些限制创建一个属性，这样我们就可以用它来显示用户还剩多少字符。

Vue 实例将如下所示:

```
 let app = new Vue({
    el: '#app',
    data: {
        userData: {}
        name: "",
        email: "",
        password: "",
        max_length: 25,
        max_pass_length: 16
    },
    methods: {
        registerAccount(){
            // record user details
            // add registration to localStorage
            // clear the registration fields

        }
    }

}); 
```

我们来分析一下。首先在`data:{}`对象内部，我们定义了一个对象来存储和检索`userData`。然后，我们添加了属性`name`、`email`和`password`，以实现双向数据绑定。
最后我们添加了`max_length`和`max_pass_length`属性。

### 如何显示输入字符计数器

好了，现在我们在输入字段和属性之间有了一个绑定，我们可以在用户输入时向他们显示一个字符计数器。

这相当简单——所要做的就是显示每个输入属性的长度，并将其与我们在 Vue 实例中设置的最大长度属性进行比较。

```
<div class="form_group">
    <label for="name">Name
        <span> {{ name.length + '/' + max_length }}</span>
    </label>
    <input type="text" v-model="name" :maxlength="max_length" required>
</div> 
```

所以这里我们使用模板内表达式创建了一个字符串。我们已经显示了`name`属性的长度和`max_length`的值，因此当用户键入时，我们显示类似这样的内容:13/25。

我们还在`maxlength`属性上使用了一个`v-bind`指令，因此它的值被绑定到我们在 Vue 实例中定义的属性值。因此，如果我们想改变它，我们可以在一个地方这样做。

我们将对其他领域采取同样的做法。

```
<form id="register" v-on:submit.prevent="registerAccount">
    <div class="form_group">
        <label for="name">Name
            <span> {{ name.length + '/' + max_length }}</span>
        </label>
        <input type="text" v-model="name" :maxlength="max_length" required>
    </div>
    <div class="form_group">
        <label for="email">Email
            <span> {{ email.length + '/' + max_length }}</span>
        </label>
        <input type="email" v-model="email" :maxlength="max_length" required>
    </div>
    <div class="form_group">
        <label for="password">Password
            <span> {{ password.length + '/' + max_pass_length }}</span>
        </label>
        <input type="password" v-model="password" :maxlength="max_pass_length" required>
    </div>
    <button type="submit">Register</button>

</form> 
```

### 如何给`registerAccount`方法添加逻辑

现在是处理表单提交逻辑的时候了。当用户提交表单时，我们将简单地填充属性`userData`中存储的对象。

在`registerAccount`方法中，我们将添加用户传递的细节并构建我们的对象。

```
 registerAccount(){
            // record user details
            this.userData.name = this.name,
            this.userData.email = this.email,
            this.userData.password = this.password

            // add registration to localStorage

            // clear the registration fields
            this.name = "";
            this.email = "";
            this.password = "";
        } 
```

这里我们取了属性 name、email 和 password 的值，并将它们分配给我们在`userData`对象中创建的属性。

这看起来很好，主要是因为我们在输入字段中加入了`required`属性——但是如果我们移除它，我们将能够提交一个空表单，这是我们不希望的。

因此，让我们添加一个非常基本的验证表单，至少检查用户是否在表单中输入了内容，否则我们会显示一个错误。

为此，我们需要在方法中添加一个 if 块，并在数据对象中添加一个 error 属性。我们的文件现在看起来像这样:

```
 let app = new Vue({
    el: '#app',
    data: {
        userData: {},
        usersID: 0,
        name: "",
        email: "",
        password: "",
        max_length: 25,
        max_pass_length: 16,
        error: "",

    },  
    methods: {
        registerAccount(){
            if (this.name !== "" && this.email !== "" && this.password !== "" ) 
            {
                this.userData.id = ++this.usersID,
                this.userData.name = this.name,
                this.userData.email = this.email,
                this.userData.password = this.password

            } else {
                this.error = "Complete all the form fields"
            }

        /* Add registration data to the local storage */

        /* Clear the registration inputs */
        this.name = "";
        this.email = "";
        this.password = "";
        }

    }

}); 
```

在这里的`registerAccount`中，我们编写了一个条件来检查 name 属性的长度是否不为空，email 属性是否不为空，以及密码是否不为空`this.name !== "" && this.email !== "" && this.password !== ""`。

如果所有这些检查都评估为真值，那么我们运行块内的代码。否则我们运行`else`块中的代码来更新`error`属性的值，我们现在将在模板中使用它来显示错误消息。

我们还添加了一个新的属性`usersID: 0,`，我们在 if 块中使用它来为对象`userData`分配一个 id 属性，这只是为了让我们的应用程序更加真实。但是当然这是没有用的，因为我们没有一个数据库来存储所有的用户信息。我们只是将单个用户存储在他们浏览器的本地存储中。

```
<form id="register" v-on:submit.prevent="registerAccount">
    <div class="form_group">
        <label for="name">Name
            <span> {{ name.length + '/' + max_length }}</span>
        </label>
        <input type="text" v-model="name" :maxlength="max_length" required>
    </div>
    <div class="form_group">
        <label for="email">Email
            <span> {{ email.length + '/' + max_length }}</span>
        </label>
        <input type="email" v-model="email" :maxlength="max_length" required>
    </div>
    <div class="form_group">
        <label for="password">Password
            <span> {{ password.length + '/' + max_pass_length }}</span>
        </label>
        <input type="password" v-model="password" :maxlength="max_pass_length" required>
    </div>
    <button type="submit">Register</button>
</form>

<div v-if="error.length > 0"> {{error}}</div> 
```

现在我们的表单完成了，如果从标记中删除了所需的属性，我们还会向用户显示一条错误消息。

但是我们的数据不会持久，当页面刷新时，一切都消失了。让我们使用 localStorage API 来解决这个问题。

在我们的 Vue 实例中，我们需要在本地存储中设置一个条目。但是我们还需要保存整个`userData`对象，以便稍后我们可以使用它的数据向注册用户显示消息。

```
/* Add registration data to the local storage */
localStorage.setItem('simple_tweet_registered', true)
/* Add the whole userData object as JSON string */
localStorage.setItem('simple_tweet_registered_user', JSON.stringify(this.userData)) 
```

这里，我们使用本地存储 API 的`setItem`方法向本地存储添加一个条目，以便稍后我们可以使用它来检查用户是否注册。

然后我们还需要将整个`userData`对象存储为一个字符串。为此，我们使用了`JSON.stringify`方法，该方法将把对象转换成可以保存在 localStorage 中的 JSON 字符串。

我们的 JS 文件现在如下:

```
 let app = new Vue({
    el: '#app',
    data: {
        userData: {},
        usersID: 0,
        name: "",
        email: "",
        password: "",
        max_length: 25,
        max_pass_length: 16,
        error: "",
    },  
    methods: {
        registerAccount(){
            if (this.name !== ""  && this.email !== "" && this.password !== "" ) {
                this.userData.id = ++this.usersID,
                this.userData.name = this.name,
                this.userData.email = this.email,
                this.userData.password = this.password

            } else {
                this.error = "Complete all the form fields"
            }

        /* Add registration data to the local storage */
        localStorage.setItem('simple_tweet_registered', true)
        /* Add the whole userData object as JSON string */
        localStorage.setItem('simple_tweet_registered_user', JSON.stringify(this.userData))

        /* Clear the registration inputs */
        this.name = "";
        this.email = "";
        this.password = "";
        }

    }

}); 
```

现在，当用户访问我们的应用程序页面时，我们需要检查浏览器的本地存储，看看是否有一个名为`simple_tweet_registered`的键。如果有，我们可以假设用户已经注册，我们可以显示下一部分，tweet box。否则，我们出示登记表。

我们将通过在数据对象中创建一个`registered: false`属性来实现，并使用它来显示或隐藏注册表单。

```
 data: {
    userData: {},
    usersID: 0,
    name: "",
    email: "",
    password: "",
    max_length: 25,
    max_pass_length: 16,
    error: "",
    registered: false,      
} 
```

用指令`v-if="!registered"`将表单包裹在 div 周围，如下所示:

```
<div class="register" v-if="!registered">
    // here goes the form
</div>

<div v-else> Tweetbox </div> 
```

我们最终的 HTML 文件现在看起来像这样:

```
 <div class="card">
        <i class="fab fa-twitter fa-lg fa-fw"></i>
        <!-- Register an account -->
        <div class="register" v-if="!registered">
            <button form="register" type="submit">Register</button>
            <h2>Create your account</h2>
            <form id="register" v-on:submit.prevent="registerAccount">
                <div class="form_group">
                    <label for="name">Name
                        <span> {{ name.length + '/' + max_length }}</span>
                    </label>
                    <input type="text" v-model="name" :maxlength="max_length" required>
                </div>
                <div class="form_group">
                    <label for="email">Email
                        <span> {{ email.length + '/' + max_length }}</span>
                    </label>
                    <input type="email" v-model="email" :maxlength="max_length" required>
                </div>
                <div class="form_group">
                    <label for="password">Password
                        <span> {{ password.length + '/' + max_pass_length }}</span>
                    </label>
                    <input type="password" v-model="password" :maxlength="max_pass_length" required>
                </div>
            </form>

            <div v-if="error.length > 0"> {{error}}</div>
        </div>
        <!-- Add tweet -->
        <div class="tweetBox" v-else>
            <h2>Welcome username_here write your first Tweet</h2>
        </div>

    </div> 
```

现在，为了实现这一点，我们将使用我们创建的生命周期挂钩，它允许我们在创建 Vue 实例时注入代码。这是因为我们希望在挂载根元素之前检查这一点。

因此，让我们为 Vue 实例添加一个生命周期挂钩。我们将检查我们的键是否存在，如果存在，我们将把属性`registered`的值更新为`true`。

我们还在本地存储中存储了完整的`userData`对象，这样当页面被用户提交的细节刷新时，我们可以用它来重新填充`userData`对象。

```
created(){
    /* Check if the user is registered and set the registered to true */
    if(localStorage.getItem("simple_tweet_registered") === 'true'){
        this.registered = true;
    }
    // repopulate the userData object
     if(localStorage.getItem('simple_tweet_registered_user')) {
            this.userData = JSON.parse(localStorage.getItem('simple_tweet_registered_user'))
        }

} 
```

要将 JSON 字符串转换回对象，我们可以使用`JSON.parse`方法。

现在已经为下一个任务做好了准备——注册后向用户显示一个 tweet 表单。

到目前为止，我们的代码如下所示:

main.js 文件:

```
 let app = new Vue({
    el: '#app',
    data: {
        userData: {},
        usersID: 0,
        name: "",
        email: "",
        password: "",
        max_length: 25,
        max_pass_length: 16,
        error: "",
        registered: false,
    },

    methods: {
          registerAccount(){

              if (this.name.length > 0 && this.name.length <= this.max_length && this.email !== "" && this.password !== "" ) {

                    this.userData.id = ++this.usersID,
                    this.userData.name = this.name,
                    this.userData.email = this.email,
                    this.userData.password = this.password
                    this.registered=true;

              } else {
                  this.error = "Complete all the form fields"
              }

            /* Add registration data to the local storage */
            localStorage.setItem('simple_tweet_registered', true)
            /* Add the whole userData object as JSON string */
            localStorage.setItem('simple_tweet_registered_user', JSON.stringify(this.userData))

            /* Clear the registration inputs */
            this.name = "";
            this.email = "";
            this.password = "";
        }

    },
    created(){
        /* Check if the user is registered and set the registered to true */
        if(localStorage.getItem("simple_tweet_registered") === 'true'){
            this.registered = true;
        }
        // repopulate the userData object
        if(localStorage.getItem('simple_tweet_registered_user')) {
            this.userData = JSON.parse(localStorage.getItem('simple_tweet_registered_user'))
        }

    }

}); 
```

和`app`元素中的 HTML:

```
<div class="card">
    <i class="fab fa-twitter fa-lg fa-fw"></i>
    <!-- Register an account -->
    <div class="register" v-if="!registered">
        <button form="register" type="submit">Register</button>
        <h2>Create your account</h2>
        <form id="register" v-on:submit.prevent="registerAccount">
            <div class="form_group">
                <label for="name">Name
                    <span> {{ name.length + '/' + max_length }}</span>
                </label>
                <input type="text" v-model="name" :maxlength="max_length" required>
            </div>
            <div class="form_group">
                <label for="email">Email
                    <span> {{ email.length + '/' + max_length }}</span>
                </label>
                <input type="email" v-model="email" :maxlength="max_length" required>
            </div>
            <div class="form_group">
                <label for="password">Password
                    <span> {{ password.length + '/' + max_pass_length }}</span>
                </label>
                <input type="password" v-model="password" :maxlength="max_pass_length" required>
            </div>
        </form>

        <div v-if="error.length > 0"> {{error}}</div>
    </div>
    <!-- Add tweet -->
    <div class="tweetBox" v-else>
        <h2>Welcome {{ userData.name }} write your first Tweet</h2>

    </div>

</div> 
```

在 HTML 中，由于我们在 add tweet 部分使用了`v-else`,并使用本地存储来检索用户提交的数据，因此我们可以使用模板内表达式来获取用户名，以便输出欢迎消息。

在下一节中，我们将创建一个 tweet box 表单，以便用户在注册后可以写一条 tweet。

[https://www.youtube.com/embed/v1j_bDDd6jI?feature=oembed](https://www.youtube.com/embed/v1j_bDDd6jI?feature=oembed)

### 如何创建一个 tweet box 表单

现在是时候构建我们的 add tweet 表单了。我们在本文前面做了一些非常类似的事情，但是现在我们需要存储数据并使其持久化。这让我们即使在页面刷新时也能显示推文列表。

```
<div class="tweetBox" v-else>
    <h2>Welcome {{ userData.name }} write your first Tweet</h2>
    <form v-on:submit.prevent="sendTweet">
        <div class="form_group">
            <label for="tweet">
                Send your tweet
                <span> {{ tweetMsg.length + '/' + max_tweet }}</span>
            </label>
            <textarea name="tweet" id="tweet" cols="30" rows="10" v-model="tweetMsg" maxlength="200"></textarea>
        </div>
        <button type="submit">Tweet</button>
    </form>

</div> 
```

这对我们来说已经不是什么新鲜事了。在`tweetBox`元素中，我们添加了一个带有常用 v-on 指令的表单和一个需要在 methods 对象中定义的方法`sendTweet`。这将获取 tweet 并将其保存在某个地方，可能在数据对象的一个属性中。

在表单内部，有一个`textarea`，它有一个`v-model`指令，将它绑定到我们需要创建的一个`tweetMsg`属性。

最后，一个提交按钮。

我们在 tweet 标签中也有一个 span，向用户显示一个字符计数器，就像我们之前在注册表单中做的那样。

这里我们有一个新的属性`max_tweet`用于显示限制，`tweetMsg.length`用于显示当前插入的字符数。

如果你想回顾你所学的内容，你可以点击在 [YouTube 上观看视频。](https://youtu.be/xFwfrIciFt0)

### 创建一个 tweets box 表单- Vue

让我们转到 Vue 实例，添加属性和`sendTweet`方法。

我们的数据对象现在又多了三个属性，设置为`200`的`max_tweet`，绑定到`textarea`的`tweetMsg`，以及一个我们将用来存储用户发送的所有 tweets 的`tweets`数组。

```
data: {
    userData: {},
    usersID: 0,
    name: "",
    email: "",
    password: "",
    max_length: 25,
    max_pass_length: 16,
    max_tweet: 200, // max tweets lenght
    error: "",
    registered: false,
    tweetMsg: "", // current tweet
    tweets: [] // list of tweets
} 
```

在这些方法中，我们有一个新方法，当提交表单时，v-on 指令将调用这个新方法:

```
 sendTweet(){
    /* Store the tweet in the tweets property */
    this.tweets.unshift(
        {
            text: this.tweetMsg,
            date: new Date().toLocaleTimeString()
        }

    );
    /* Empty the tweetMsg property */
    this.tweetMsg = "";
    //console.log(this.tweets);

    /* Tranform the object into a string  */
    stringTweets = JSON.stringify(this.tweets)
    //console.log(stringTweets);

    /* Add to the local storage the stringified tweet object */
    localStorage.setItem('simple_tweet_tweets', stringTweets)
}, 
```

上面的代码做了四件事:

*   获取 tweets 数组并在其中添加一个对象，用文本和日期属性表示一条 tweet。我们将与`textarea`绑定的`tweetMsg`的值赋给 text 属性。对于日期，我们用`new Date().toLocaleTimeString()`方法创建一个新的日期对象。
*   我们清空`tweetMsg`的值
*   我们使用方法`JSON.stringify(this.tweets)`将 tweets 属性转换成一个字符串
*   然后我们将它添加到本地存储中。

我们最终的 main.js 文件现在看起来像这样:

```
 let app = new Vue({
    el: '#app',
    data: {
        userData: {},
        usersID: 0,
        name: "",
        email: "",
        password: "",
        max_length: 25,
        max_pass_length: 16,
        error: "",
        registered: false,
    },

    methods: {
          registerAccount(){

              if (this.name.length > 0 && this.name.length <= this.max_length && this.email !== "" && this.password !== "" ) {

                    this.userData.id = ++this.usersID,
                    this.userData.name = this.name,
                    this.userData.email = this.email,
                    this.userData.password = this.password
                    this.registered=true;

              } else {
                  this.error = "Complete all the form fields"
              }

            /* Add registration data to the local storage */
            localStorage.setItem('simple_tweet_registered', true)
            /* Add the whole userData object as JSON string */
            localStorage.setItem('simple_tweet_registered_user', JSON.stringify(this.userData))

            /* Clear the registration inputs */
            this.name = "";
            this.email = "";
            this.password = "";
        },
        sendTweet(){
            /* Store the tweet in the tweets property */
            this.tweets.unshift(
                {
                    text: this.tweetMsg,
                    date: new Date().toLocaleTimeString()
                }

            );
            /* Empty the tweetMsg property */
            this.tweetMsg = "";
            //console.log(this.tweets);

            /* Tranform the object into a string  */
            stringTweets = JSON.stringify(this.tweets)
            //console.log(stringTweets);

            /* Add to the local storage the stringified tweet object */
            localStorage.setItem('simple_tweet_tweets', stringTweets)
        },

    },
    created(){
        /* Check if the user is registered and set the registered to true */
        if(localStorage.getItem("simple_tweet_registered") === 'true'){
            this.registered = true;
        }
        // repopulate the userData object
        if(localStorage.getItem('simple_tweet_registered_user')) {
            this.userData = JSON.parse(localStorage.getItem('simple_tweet_registered_user'))
        }

    }

}); 
```

现在我们已经完成了这一部分，我们可以显示一个 tweets 列表，并在页面刷新时进行处理，本地存储中有我们的 tweets 对象。我们需要对其进行解析，并将其内容添加到`tweets`属性中以查看列表。

接下来，我们将学习如何使用 v-for 指令显示 tweets 列表。

[https://www.youtube.com/embed/xFwfrIciFt0?feature=oembed](https://www.youtube.com/embed/xFwfrIciFt0?feature=oembed)

### 如何显示推文列表

在我们的根元素中，添加以下代码:

```
 <!-- Show all tweets -->
    <div class="card_tweets">
        <section class="tweets" v-if="tweets.length > 0">
            <h2>Tweets</h2>
            <div class="tweetMsg" v-for="(tweet, index) in tweets">
                <p>
                    {{tweet.text}}
                </p>

                <div class="tweetDate">
                    <i class="fas fa-calendar-alt fa-sm fa-fw"></i>{{tweet.date}}
                </div>

            </div>

        </section>
        <div v-else>No tweets to show</div>
    </div> 
```

这里我们用一个类`card_tweets`将所有东西包装在一个 div 中。然后我们在子节中使用 v-if 指令来检查在`tweets`数组`v-if="tweets.length > 0"`中是否有 tweets。

在这个部分中，我们可以使用一个`v-for="(tweet, index) in tweets"`指令来遍历 tweets 数组。之后，我们使用模板内表达式来显示 tweet 文本属性`{{tweet.text}}`和数据`{{tweet.date}}`。

在`section`之后，我们可以使用`v-else`指令来显示一条消息，以防 tweets 数组`<div v-else>No tweets to show</div>`中没有存储 tweets。完成了。

现在我们还需要做最后一件事，那就是想办法从列表中删除推文。

但是当用户刷新页面时，一切都没了。因此，在呈现根元素之前，我们需要再次使用`localStorage`来重新填充 tweets 数组。

在`created`生命周期挂钩中，我们现在将编写一些代码来解析 tweets，并将它们保存回`tweets`属性中:

```
/* Parse all tweets from the local storage  */
if(localStorage.getItem("simple_tweet_tweets")) {
    console.log("There is a list of tweets");
    this.tweets = JSON.parse(localStorage.getItem('simple_tweet_tweets'))
}else {
    console.log("No tweets here");
} 
```

这里我们使用`localStorage` API 首先检查是否有一个名为`simple_tweet_tweets`的键。如果是这样，我们使用`this.tweets`获取 tweets 属性，并将`localStorage`的内容分配给它。但是我们用`JSON.parse`将字符串解析回`JSON`，所以我们写`this.tweets = JSON.parse(localStorage.getItem('simple_tweet_tweets'))`。

现在一切正常。在我们刷新页面后，推文仍然存在。我们继续吧。下一步，我们将添加一个从列表中删除 tweets 的方法。

你可以点击这里或本节末尾的 [YouTube 上的视频来回顾你所学到的内容。](https://youtu.be/3DzBkUHH3bU)

### 如何删除推文

在包含 tweet 消息的 div 中，我们可以添加另一个 div 来显示链接和垃圾桶图标。这让用户点击它并删除该推文。

```
<div class="tweet_remove" @click="removeTweet(index)">
    <span class="remove">Delete this tweet <i class="fas fa-trash fa-xs fa-fw"></i></span>
</div> 
```

这里，我们简单地在 div 上使用了一个 v-on 短语法指令，并调用了一个方法`removeTweet(index)`，将元素索引传递给该方法，以便我们知道要删除什么。

现在让我们构建我们的`removeTweet`方法:

```
removeTweet(index){
    let removeIt = confirm("Are you sure you want to remove this tweet?")
    if(removeIt) {
        this.tweets.splice(index, 1);
        /* Remove the item also from the local storage */
        localStorage.simple_tweet_tweets = JSON.stringify(this.tweets)
    }
} 
```

这段代码非常简单。我们的方法接受一个索引，该索引表示 tweet 对象在调用该方法时从 v-for 指令获得的数组中的位置。

然后我们创建一个变量，要求用户确认他们想要删除这条推文。我们为此使用了`confirm`函数。

如果`removeIt`变量的值为真，那么我们执行代码并使用`this.tweets.splice(index, 1)`从使用其索引的数组中删除 tweet。

最后，我们通过使用`localStorage.simple_tweet_tweets = JSON.stringify(this.tweets)`为新数组赋值来更新`localStorage`值。

### 最终代码

我们的代码现在已经完成了。你可以在这里找到最终的代码:[[https://bitbucket.org/fbhood/simple-tweet-app/src/master/](https://bitbucket.org/fbhood/simple-tweet-app/src/master/)]。

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue 2 Hello World</title>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.1.1/css/all.css"
        integrity="sha384-O8whS3fhG2OnA5Kas0Y9l3cfpmYjapjI0E4theH4iuMD+pLhbf6JI0jIMfYcK3yZ" crossorigin="anonymous">
    <!-- Axios CDN -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/axios/0.21.0/axios.min.js"
        integrity="sha512-DZqqY3PiOvTP9HkjIWgjO6ouCbq+dxqWoJZ/Q+zPYNHmlnI2dQnbJ5bxAHpAMw+LXRm4D72EIRXzvcHQtE8/VQ=="
        crossorigin="anonymous"></script>
    <!-- development version, includes helpful console warnings -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.1.1/css/all.css"
        integrity="sha384-O8whS3fhG2OnA5Kas0Y9l3cfpmYjapjI0E4theH4iuMD+pLhbf6JI0jIMfYcK3yZ" crossorigin="anonymous">
    <link rel="stylesheet" href="style.css">
</head>

<body>
<div id="app">
    <div class="card">
        <i class="fab fa-twitter fa-lg fa-fw"></i>
        <!-- Register an account -->
        <div class="register" v-if="!registered">
            <button form="register" type="submit">Register</button>
            <h2>Create your account</h2>
            <form id="register" v-on:submit.prevent="registerAccount">
                <div class="form_group">
                    <label for="name">Name
                        <span> {{ name.length + '/' + max_length }}</span>
                    </label>
                    <input type="text" v-model="name" :maxlength="max_length" required>
                </div>
                <div class="form_group">
                    <label for="email">Email
                        <span> {{ email.length + '/' + max_length }}</span>
                    </label>
                    <input type="email" v-model="email" :maxlength="max_length" required>
                </div>
                <div class="form_group">
                    <label for="password">Password
                        <span> {{ password.length + '/' + max_pass_length }}</span>
                    </label>
                    <input type="password" v-model="password" :maxlength="max_pass_length" required>
                </div>
            </form>

            <div v-if="error.length > 0"> {{error}}</div>
        </div>
        <!-- Add tweet -->
        <div class="tweetBox" v-else>
            <h2>Welcome {{ userData.name }} write your first Tweet</h2>
            <form v-on:submit.prevent="sendTweet">
                <div class="form_group">
                    <label for="tweet">
                        Send your tweet
                        <span> {{ tweetMsg.length + '/' + max_tweet }}</span>
                    </label>
                    <textarea name="tweet" id="tweet" cols="30" rows="10" v-model="tweetMsg" maxlength="200"></textarea>
                </div>
                <button type="submit">Tweet</button>
            </form>

        </div>

    </div>
    <!-- Show all tweets -->
    <div class="card_tweets">
        <section class="tweets" v-if="tweets.length > 0">
            <h2>Tweets</h2>
            <div class="tweetMsg" v-for="(tweet, index) in tweets">
                <p>
                    {{tweet.text}}
                </p>

                <div class="tweetDate">
                    <i class="fas fa-calendar-alt fa-sm fa-fw"></i>{{tweet.date}}
                </div>
                <div class="tweet_remove" @click="removeTweet(index)">
                    <span class="remove">Delete this tweet <i class="fas fa-trash fa-xs fa-fw"></i></span>
                </div>

            </div>

        </section>
        <div v-else>No tweets to show</div>
    </div>
</div>
<script src="./main.js"></script>
</body>

</html> 
```

JavaScript 文件

```
 let app = new Vue({
    el: '#app',
    data: {
        userData: {},
        usersID: 0,
        name: "",
        email: "",
        password: "",
        max_length: 25,
        max_pass_length: 16,
        max_tweet: 200,
        error: "",
        registered: false,
        tweetMsg: "",
        tweets: []
    },

    methods: {
          registerAccount(){

              if (this.name.length > 0 && this.name.length <= this.max_length && this.email !== "" && this.password !== "" ) {

                    this.userData.id = ++this.usersID,
                    this.userData.name = this.name,
                    this.userData.email = this.email,
                    this.userData.password = this.password
                    this.registered=true;

              } else {
                  this.error = "Complete all the form fields"
              }

            /* Add registration data to the local storage */
            localStorage.setItem('simple_tweet_registered', true)
            /* Add the whole userData object as JSON string */
            localStorage.setItem('simple_tweet_registered_user', JSON.stringify(this.userData))

            /* Clear the registration inputs */
            this.name = "";
            this.email = "";
            this.password = "";
        }, 
        sendTweet(){
            this.tweets.unshift(
                {
                    text: this.tweetMsg,
                    date: new Date().toLocaleTimeString()
                }

            );
            this.tweetMsg = "";

            //console.log(this.tweets);
            stringTweets = JSON.stringify(this.tweets)
            //console.log(stringTweets);
            localStorage.setItem('simple_tweet_tweets', stringTweets)
        },
        removeTweet(index){
            let removeIt = confirm("Are you sure you want to remove this tweet?")
            if(removeIt) {
                this.tweets.splice(index, 1);
                /* Remove the item also from the local storage */
                localStorage.simple_tweet_tweets = JSON.stringify(this.tweets)
            }
        }
    },
    created(){
        /* Check if the user is registered and set the registered to true */
        if(localStorage.getItem("simple_tweet_registered") === 'true'){
            this.registered = true;
        }

        if(localStorage.getItem('simple_tweet_registered_user')) {
            this.userData = JSON.parse(localStorage.getItem('simple_tweet_registered_user'))
        }
        /* Parse all tweets from the local storage  */
        if(localStorage.getItem("simple_tweet_tweets")) {
            console.log("There is a list of tweets");
            this.tweets = JSON.parse(localStorage.getItem('simple_tweet_tweets'))

        }else {
            console.log("No tweets here");
        }
    }

}); 
```

我们已经准备好继续我们的 Vue 之旅。现在是时候学习组件了。

[https://www.youtube.com/embed/3DzBkUHH3bU?feature=oembed](https://www.youtube.com/embed/3DzBkUHH3bU?feature=oembed)

## Vue 组件基础

组件是表示页面上特定元素的可重用代码块。

每个网页和 web 或移动应用程序都可以分成组件。从主要部分开始，我们可以进一步把它们分成更小的部分，组成子组件。

每个组件都是可重用的，由专用的 HTML、CSS 和 JavaScript 代码组成。

我们可以使用组件来组织代码，构建易于维护的复杂布局。

看一个简单的网页，它通常由一个页眉、主要内容区域和一个页脚组成。但是这三块中的每一块都可以再分成更小的部分。

例如，一个标题可以有一个主导航菜单，一个二级菜单和一个英雄图像。主区域和页脚区域也是如此。

你可以点击在 [YouTube 上观看视频，或者点击本节末尾进行回顾。你也可以在这里](https://youtu.be/wrqjPka7puo)查看资源库[。](https://bitbucket.org/fbhood/how-to-vuejs/src/master/12-components-basics/)

要开始使用组件，我们首先需要学习如何注册它们，向它们传递数据，然后我们需要学习如何使用它们。以下是这些主题的一些很好的概述，供您参考:

*   注册一个组件([https://vuejs.org/v2/guide/components-registration.html](https://vuejs.org/v2/guide/components-registration.html))
*   如何使用道具([https://vuejs.org/v2/guide/components-props.html](https://vuejs.org/v2/guide/components-props.html))
*   如何使用老虎机([https://vuejs.org/v2/guide/components-slots.html](https://vuejs.org/v2/guide/components-slots.html))
*   数据对象如何在组件内部工作
*   子组件事件([https://vuejs.org/v2/guide/components-custom-events.html](https://vuejs.org/v2/guide/components-custom-events.html))
*   动态组件([https://vuejs.org/v2/guide/components-dynamic-async.html](https://vuejs.org/v2/guide/components-dynamic-async.html))

### 如何在 Vue 中注册组件

要注册一个组件，我们需要在`Vue()`对象上使用`component`方法。调用这个函数后，我们需要用一些特定于组件的标记来定义一个模板属性。

```
Vue.component('component-name', {
    // component properies here
}); 
```

每个组件至少需要有一个模板属性——没有它，一个组件就没有多大意义。

所以下一步是定义一个模板属性，并向它传递一个带有 HTML 标记的字符串:

```
Vue.component('test-component', {
    // component properies here
    template: `<p>I am a component</p>`

}); 
```

现在，我们可以在主 HTML 文件中多次使用我们的组件，方法是使用它的名称，因为它是一个标准的 HTML 标记。

```
<div id="app">
  <test-component></test-component>
</div> 
```

然而，我们的组件将总是呈现相同的内容，`I am a component`。让我们让它更有用，并按照我们的 tweets 示例，构建一个 tweet 消息组件。

```
 Vue.component('tweet-message', {

    template: `
       <div>
           <p> Tweet text goes here </p>
           <p> Date of the tweet goes here</p>
       </div>
    `
}); 
```

好了，现在我们有了组件的基础，我们需要向它传递数据。

这里需要注意的一点是，每个组件在模板属性中都需要一个根元素。因此，由于我们有两个段落，我们将它们包装在一个 div 中，该 div 将被视为组件的根元素。

在它里面，我们可以放任何我们想要的东西来构建我们的定制组件。

让我们继续下一步，将一些数据传递给组件。

### 如何在 Vue 中使用道具

现在，根据我们到目前为止所学的知识，我们希望将数据传递给我们的组件，就像我们之前通过小胡子语法在 Vue 实例和标记文件之间绑定数据一样。

然而，对于组件来说，事情会有所不同。我们使用 props 来创建组件与其模板之间的绑定。

属性可以被定义为一个数组或者一个对象。
当作为数组使用时，我们可以在数组内将属性指定为字符串，这些属性可以像我们通常做的那样在组件内使用。

当我们使用一个对象时，我们可以把 prop 作为键，把它的类型作为值。这将有助于确保将准确的数据类型传递给我们的组件。

让我们来看一个例子。

作为数组的道具示例:

```
Vue.component('tweet-message', {
    props: ['text', 'date']
    template: `
       <div>
           <p> {{text}} </p>
           <p> {{date}}</p>
       </div>
    `
}); 
```

使用 props 作为对象，其中键是属性，值是其类型:

```
Vue.component('tweet-message', {
    props: {
        text: String,
        date: String
    }
    template: `
       <div>
           <p> {{text}} </p>
           <p> {{date}}</p>
       </div>
    `
}); 
```

一旦我们定义了我们的属性，我们就可以将它们作为 HTML 属性使用，并将我们希望组件呈现到页面上的数据传递给它们。

例如，我们可以使用上面的组件来显示一堆使用我们新创建的组件的推文。

```
 <!-- Manually pass the data to the tweet message component -->
    <tweet-message text="This is a component" date="25/12/2020"></tweet-message>
    <tweet-message text="This another component" date="26/12/2020"></tweet-message>
    <tweet-message text="This another component" date="27/12/2020"></tweet-message>
    <!-- Pass a javascript expression to the date property of the tweet message component -->
    <tweet-message text="This another component" :date="new Date().toLocaleString()"></tweet-message> 
```

第一个例子将呈现我们在引号之间传递的字符串。但是
要呈现新的`Date()`实例的计算结果，我们需要使用 v-bind 指令，以便将其内容解释为 JavaScript 代码。

你可以在这里的文档中查看所有这些:[[https://vuejs.org/v2/guide/components-props.html](https://vuejs.org/v2/guide/components-props.html)。

### 组件内部的数据属性

到目前为止，我们已经看到可以通过在 Vue 实例上的`data`对象中定义属性来绑定数据。

当使用组件时，数据对象不能作为对象使用，只能作为函数使用。这个函数可以返回一个带有属性的对象。这将使每个组件的实例都是唯一的，并且独立于其他组件。

按照我们之前的例子，让我们添加几个 CSS 类到我们的组件。

首先，我们将编辑组件模板，并将 class 属性绑定到一个数据属性。然后我们将创建我们的数据对象。

```
Vue.component('tweet-message', {
    props: {
        text: String,
        date: String
    }
    template: `
       <div :class="tweetBoxWrapper">
           <p> {{text}} </p>
           <p :class="dateClass"> {{date}}</p>
       </div>
    `
}); 
```

现在我们的模板将寻找两个数据属性，`tweetBoxWrapper`和`dateClass`，稍后我们可以在 CSS 中使用它们来为元素添加一些样式。现在让我们添加数据函数。

```
Vue.component('tweet-message', {
    props: {
        text: String,
        date: String
    }
    template: `
       <div :class="tweetBoxWrapper">
           <p> {{text}} </p>
           <p :class="dateClass"> {{date}}</p>
       </div>
    `,
    data(){
        return {
            // Data properties go here
            tweetBoxWrapper: "tweet-message",
            dateClass: "tweet-date",
        }
    }
}); 
```

我们可以做的另一件事是定义一个数据属性，并在模板中使用它，例如，动态显示当前日期。我们可以定义一个`now`属性，并在模板中使用它，就像我们之前对`date`属性所做的那样:

```
 Vue.component('tweet-message', {
    props: {
        'text': String,

    },
     template: `
       <div :class="tweetBoxWrapper">
           <p> {{text}} </p>
           <p :class="dateClass">{{now}}</p>

       </div>

    `,
    data(){
        return {
            tweetBoxWrapper: "tweet-message",
            dateClass: "tweet-date",
            now: new Date().toDateString(), // 3 

        }
    }

}); 
```

在上面的例子中，我们同时使用了`props`和`data`。当我们使用组件 `<tweet-message text="This is a component"></tweet-message>`时，我们可以使用道具`text`作为属性。我们在`data`方法中返回的属性被绑定到模板，并将呈现我们在数据方法中指定的信息。

当在数据方法内部时，我们需要记住这里定义的属性可以使用关键字`this`访问。

因此，如果我们想将文本属性的值存储在数据对象中，我们可以像这样获取它:

```
Vue.component('tweet-message', {
    props: {
        'text': String,

    },
     template: `
       <div :class="tweetBoxWrapper">
           <p> {{text}} </p>
           <p :class="dateClass">{{now}}</p>

       </div>

    `,
    data(){
        return {
            tweetBoxWrapper: "tweet-message",
            dateClass: "tweet-date",
            now: new Date().toDateString(), 
            message: this.text
        }
    }

}); 
```

接下来，我们将学习老虎机。

### 如何使用插槽

有些情况下，我们不知道或者不想严格定义组件内部的内容。或者我们可能想让用户在使用我们的组件时决定它的内容。

在这种情况下，我们可以在声明组件的模板时使用插槽。

让我们想象一下，我们有另一个组件，我们想用它来将推文分成不同的部分。

```
Vue.component('tweet-section', {
    props: {
        'title': String,

    },
     template: `
        <div class="tweet_section">
            <h2>{{title}}</h2>
           <slot></slot>
       </div>  
    `    
}); 
```

我们的新组件就这么简单，一个带有类`tweet_section`的 div、一个绑定到道具的`h2`和一个插槽。插槽意味着在我们的组件中，我们可以放入任何我们想要的东西，比如嵌套其他元素甚至其他组件。

```
 <tweet-section title="Latest Tweets">
    <tweet-message text="This is my first tweet"></tweet-message>
    <tweet-message text="This is my second tweet"></tweet-message>
    <tweet-message text="This is my third tweet"></tweet-message>
    <tweet-message text="This is my fourth tweet"></tweet-message>

</tweet-section>
<tweet-section title="Most popular">
    <h3>Trendy in IT</h3>
    <tweet-message text="This is a very popular tweet"></tweet-message>
    <tweet-message text="This is another popular tweet"></tweet-message>
</tweet-section> 
```

我们在这里仅仅触及了表面，但是就我们所知，我们已经可以修改我们的`simple_twitter`应用程序来使用组件。在这个过程中，我们还将了解事件在组件内部是如何工作的。

[https://www.youtube.com/embed/wrqjPka7puo?feature=oembed](https://www.youtube.com/embed/wrqjPka7puo?feature=oembed)

## 如何用组件更新您的 Simple_twitter 项目

现在，我们对组件有了基本的了解，我们可以更新我们在前面的视频中构建的简单 Twitter 项目，并使用组件来使我们的代码变得更好。

我们需要做一些事情来实现这一点，并创建一个组件:

1.  我们需要决定我们想要构建什么组件
2.  我们需要从标记中提取代码，并将其放在模板属性中
3.  我们需要重构代码以使组件工作。

你可以点击
在 [YouTube 上观看教程，或者点击](https://youtu.be/HanHyGFC6Sc)查看知识库[。](https://bitbucket.org/fbhood/how-to-vuejs/src/master/13-simple-twitter-components/)

假设我们想要为 tweet 消息创建一个组件。

### 如何创建组件

让我们为 tweet 消息创建一个组件，如下所示:

```
Vue.component('tweet-message',{
    template: ``
}); 
```

### 如何移动 tweetMsg 元素

然后我们必须将`tweetMsg`元素移动到组件的`template`属性中:

```
 Vue.component('tweet-message',{
    template: `
    <div class="tweetMsg" v-for="(tweet, index) in tweets">
        <p>
            {{ tweet.text}}
        </p>
        <div class="tweetDate">
            <i class="fas fa-calendar fa-sm fa-fw"></i>{{ tweet.date }}
        </div>
        <div class="tweet_remove" @click="removeTweet(index)">
            <span class="remove">Delete this tweet <i class="fas fa-trash fa-sm fa-fw"></i></span>
        </div>
    </div>
    `
}); 
```

之后，我们需要更新模板，因为 v-for 指令现在没有用了。因此，我们将删除它，并在以后准备使用该组件时将其添加回去。

鉴于此时我们没有 v-for 指令，我们仍然希望使用 tweet 变量来获取 tweet，因此我们将把它作为`props`来传递。

```
Vue.component('tweet-message',{
    props: {
        'tweet': Object,
    },
    template: `
    <div class="tweetMsg">
        <p>
            {{ tweet.text}}
        </p>
        <div class="tweetDate">
            <i class="fas fa-calendar fa-sm fa-fw"></i>{{ tweet.date }}
        </div>
        <div class="tweet_remove" @click="removeTweet(index)">
            <span class="remove">Delete this tweet <i class="fas fa-trash fa-sm fa-fw"></i></span>
        </div>
    </div>
    `
}); 
```

### 如何发出自定义事件

还有一个事件侦听器需要更改，以让我们的应用程序按预期工作。

```
<div class="tweet_remove" @click="removeTweet(index)">
    <span class="remove">Delete this tweet <i class="fas fa-trash fa-sm fa-fw"></i></span>
</div> 
```

这里的代码`<div class="tweet_remove" @click="removeTweet(index)">`监听点击事件，因此用户可以通过点击删除一条推文。

这需要删除，我们需要用 Vue 实例的一个特殊方法`$emit()`来替换它。我们的组件实例将需要与父实例通信，并告诉它想要触发 remove tweet 方法。

为了解决这个问题，Vue 提供了一个自定义事件系统。它允许我们使用 v-on 指令不仅监听本地 DOM 事件，还监听组件级定义的自定义事件。

我们需要更新这行代码:

```
<div class="tweet_remove" @click="removeTweet(index)"> 
```

并且像这样改变它:

```
<div class="tweet_remove" @click="$emit('remove-tweet', 'index')"> 
```

让我们来分解一下:我们一直使用 v-on 指令的简写形式`@`。然后，我们使用 Vue `$emit`方法来定义一个定制事件，当我们单击这个元素时，我们的组件将发出这个事件。

对于`$emit`方法，我们传递两个参数，第一个是定制事件`remove-tweet`的名称，第二个是当我们使用`index`时我们想要传递给事件监听器的参数。这将是我们想要删除的元素的索引。

这样父实例就可以监听我们的事件，触发我们在主 Vue 实例中定义的`removeTweet`方法，并删除正确的 tweet。

### 把它们放在一起

我们最后的组件现在看起来像这样:

```
 Vue.component('tweet-message',{
    props: {
        'tweet': Object,
    },
    template: `
    <div class="tweetMsg">
        <p>
            {{tweet.text}}
        </p>

        <div class="tweetDate">
            <i class="fas fa-calendar-alt fa-sm fa-fw"></i>{{tweet.date}}
        </div>
        <div class="tweet_remove" @click="$emit('remove-tweet', 'index')">
            <span class="remove">Delete this tweet <i class="fas fa-trash fa-xs fa-fw"></i></span>
        </div>

    </div>
    `
}); 
```

我们将如下更改我们的 index.html 文件:

```
<!-- Show all tweets -->
<div class="card_tweets">
    <section class="tweets" v-if="tweets.length > 0">
        <h2>Tweets</h2>
        <tweet-message v-for="(tweet, index) in tweets"  v-bind:tweet="tweet" :key="index" @remove-tweet="removeTweet(index)"></tweet-message>
    </section>
    <div v-else>No tweets to show</div>
</div> 
```

现在我们已经完成了我们的第一个项目，让我们学习如何进行 API 请求，以及如何使用 GitHub API 来构建我们的最终作品集。

[https://www.youtube.com/embed/HanHyGFC6Sc?feature=oembed](https://www.youtube.com/embed/HanHyGFC6Sc?feature=oembed)

## 如何使用 Axios 执行 API 调用

对于我们的下一个项目，我已经使用 Figma 创建了一个简单但很好的设计，我们将使用它来启动我们的投资组合。

我们的投资组合将使用 GitHub 的 rest API 来拉动项目并填写设计。

你可以在这里
观看 [YouTube 上的视频，并在](https://youtu.be/XJEmPr89HA8) [BitBucket](https://bitbucket.org/fbhood/how-to-vuejs/src/master/14-axios/) 上查看知识库。

### 什么是 REST API？

引用维基百科:

> “REST API 是一种软件架构风格，使请求系统能够访问和操作 web 资源的文本表示。”

这意味着我们的 Vue 应用程序(请求系统)将从 GitHub 请求我们的存储库的文本表示，我们可以在以后使用并操作它来展示我们投资组合中的项目。

对于我们的最终项目，我们将使用一个名为 Axios 的库，它将帮助我们向 GitHub API 发出 HTTP 请求。

我们可以通过多种方式在项目中安装 Axios。对于我们的例子，我们将保持简单，使用 CDN。

还有其他方法可以用来安装 Axios。Axios 的官方文档可以在[这里](https://www.npmjs.com/package/axios)T2 获得，你可以在[文档这里](https://vuejs.org/v2/cookbook/using-axios-to-consume-apis.html#Base-Example)阅读如何使用 API。

### 如何通过 CDN 安装 Axios

因此，让我们开始通过 CDN 安装 Axios。我们将使用 UNPKG CDN 并在主 HTML 文件中插入一个脚本标签。

该 CDN 将始终提供最新版本的 Axios。或者，我们也可以指定不同的版本号。

让我们首先在一个 index.html 文件中插入以下脚本，我们将使用它向 GitHub API 发送第一个 HTTP 请求。

```
<script src="https://unpkg.com/axios/dist/axios.min.js"></script> 
```

我们最终的 HTML 文件将会是这样的:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VueJS / GitHub API</title>

</head>
<body>
    <div id="app"> </div>

    <!-- Axios latest version CDN -->
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>

    <!-- VueJS development version -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="./main.js"></script>
</body>
</html> 
```

上面的代码没什么新鲜的，还是一点一点来看吧。

我们有一个基本的 HTML 结构。在结束 body 标签之前，我们放置了两个脚本标签，一个用于 Axios，一个用于 VueJs。

在正文中，我们为 Vue 应用程序创建了一个根元素，我们称之为`#app`。

最后，在 body 标记的结尾之前，我们放置了一个新的 script 标记，它指向我们将要编写代码的文件 main.js 文件。

现在我们有了所有的构建模块来进行第一次 API 调用，并从 GitHub API 请求数据。

但在此之前，让我们快速了解一下什么是 HTTP 请求，以及我们可以发出什么样的请求。

### 什么是 HTTP 请求？

HTTP 代表超文本传输协议。它是一种应用层协议，设计用于两点之间的通信:

1.  web 客户端(浏览器)
2.  网络服务器

这个协议允许传输像 HTML 文件和。它定义了可用于对给定资源执行特定操作的动词，也称为方法。

我们将在项目中使用的方法或动词是`GET`方法，正如您可能已经猜到的那样，它用于从 web 服务器获取资源。

我们还有其他方法:

*   获取(检索数据)
*   POST(发送数据)
*   PUT(更新数据的整个表示)
*   修补(类似于上传，但用于部分更新数据)
*   删除(删除数据)

这些请求中的每一个都在资源上执行特定的操作，但是还有其他动词，如 HEAD、OPTIONS、CONNECT 和 TRACE。

我不会在这里详细讨论 HTTP，因为它超出了本指南的范围。但是如果你想了解更多，下面有一些与这个主题相关的文档页面的链接。

我建议你至少阅读以下内容:

*   [HTTP 简介](https://www.freecodecamp.org/news/how-the-internet-works/)
*   [HTTP 方法](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)

### 如何执行 GET 请求

我们将使用 get 方法来执行来自 GitHub API 的 GET 请求。我们想要请求的所有数据都是可公开访问的(用户的公共存储库)，因此我们不必对我们的应用程序进行认证。

但是未经身份验证的请求是有限的。对于本教程的范围来说，这完全没问题。如果您计划将它投入生产，那么您可能想看看如何进行认证请求并从 GitHub 获得 API 密钥。

GitHub 提供了关于其 Rest API 的清晰而深入的文档，包括您可以请求的资源列表及其端点。我们将使用“为用户列出存储库”可用资源[这里](https://docs.github.com/en/free-pro-team@latest/rest/reference/repos#list-repositories-for-a-user)。

让我们看看文档。我们注意到的第一件事是 GitHub 给了我们一个 GET 端点，我们可以在这里发送 HTTP 请求`/users/{username}/repos`。

占位符`{username}`需要替换为我们想要向其请求公共存储库列表的用户的实际用户名。

从文档中，我们还可以看到，我们可以使用其他参数来细化我们的请求。我们将在路径中使用`username`,它需要是一个字符串，如类型下的参数表中所述。

我们还可以使用`per_page`和`page`参数对结果进行分页。

让我们提出第一个请求，看看我们会得到什么。

在我们的 main.js 文件中，我们将创建一个新的 Vue 实例，并添加一个`mounted`生命周期钩子，在这里我们将使用 Axios 执行 HTTP 请求。

```
let app = new Vue({
    el:'#app',
    data:{
        projects: [],
        perPage: 20,
        page: 1
    },
    mounted(){

         axios
         .get(`https://api.github.com/users/fabiopacifici/repos?per_page=${this.perPage}&page=${this.page}`)
         .then(
            response => {
                console.log(response);
                this.projects = response.data;
            }
        )
        .catch(error=> {console.log(error);})
    }
}); 
```

让我们来分解这个代码。首先，我们创建了一个新的 Vue 实例。然后我们使用了`el`属性，并给它分配了一个根 HTML 元素。

然后我们定义了一个`data`对象和属性，稍后我们将使用它们来执行 HTTP 请求和处理响应。

在数据对象之后，我们定义了一个生命周期钩子，一旦根元素被挂载，它将用来运行我们的代码。

在`mounted`方法中，是时候使用 Axios 并执行 HTTP 请求了。

Axios 是一个基于 promise 的 HTTP 客户端。当我们使用 get 方法从 GitHub API 请求数据时，它将返回一个需要处理的承诺。

我们使用语法`axios.get()`来执行请求，然后在承诺上使用`.then()`方法来处理它的响应。

如果我们的请求失败了,`.catch()`方法将处理错误，在这种情况下，在控制台上显示错误消息。

承诺超出了本指南的范围，但是如果你想了解更多，你可以点击查看[这篇详细的文章。](https://www.freecodecamp.org/news/javascript-promise-tutorial-how-to-resolve-or-reject-promises-in-js/)

在`.get()`方法中，我们放入了包含查询字符串的 URL，该字符串使用`per_page`和`page`参数来提交我们的请求。在`.then()`方法中，我们处理响应。响应参数是 promise 给我们的，我们使用一个箭头函数来处理它。

```
response => {
                console.log(response);
                this.projects = response.data;
            } 
```

get 方法返回一个承诺。这里我们简单地用一个箭头函数处理了它的`response`，其中`response`是我们通过调用`axios.get()`获得的返回值。

我们将响应对象记录到控制台。然后我们将它的内容`response.data`分配给我们的`projects`属性，这样我们以后可以检索每个项目，并像往常一样用`v-for`指令将它们显示在页面上。

### 如何显示每个项目

现在是时候展示我们投资组合中的项目了。我们可以用 v-for 指令来实现。

在这种情况下，projects 属性包含一个对象数组。每个对象都有自己的属性，我们可以用它们来填充模板。

```
<div id='app'>

    <div v-for="project in projects">
        <h2 class="title">{{project.full_name}}</h2>

        <div class="author">
            <img width="50px" :src="project.owner.avatar_url" alt="me">
        </div>
        <div class="view">
            <a :href="project.html_url">View</a>
        </div>
    </div>

</div> 
```

这里我们使用 v-for 指令来遍历项目数组。
现在,`project`变量包含一个对象，表示 GitHub 帐户中的一个存储库。

查看响应对象，我们知道我们可以获取许多属性。所以我们选择了`full_name`，存储库的全称，`owner.avatar_url`，概要文件头像的 URL，`html_url`是我们存储库的实际 URL。这就是我们目前所需要的。

如果我们现在查看页面，我们将立即看到我们帐户中的所有存储库。

现在我们知道了如何用 Axios 发出 HTTP 请求并从 GitHub 获取数据，我们几乎准备好开始构建我们的投资组合了。

在下一节中，我们将会看到另一个名为 Vue-router 的 Vue 库，它将会在我们的最终项目中使用。

[https://www.youtube.com/embed/XJEmPr89HA8?feature=oembed](https://www.youtube.com/embed/XJEmPr89HA8?feature=oembed)

## 如何使用 VueRouter 处理路由

我们的投资组合肯定会有不止一个页面，所以我们需要一个系统，当用户点击导航栏中的某个特定页面的链接时，它能够理解将用户发送到哪里。

为此，Vue 有一个官方的路由包，可以帮助我们做到这一点，并建立一个单一的页面应用程序。

单页面应用程序是指当用户访问新页面时不会刷新页面的应用程序，这样用户体验会更加流畅。

至于 Vue 和 Axios，我们需要安装这个库，我们通过它的 CDN 来完成。但一如既往，也有其他方法取决于您的需求。我现在只想保持事情简单，所以让我们从将 CDN 脚本标记放入 HTML 文件开始，学习这个新库的基础知识。

你可以在这里
观看 [YouTube 上的教程，并在](https://youtu.be/T_avTRFAEAg) [BitBucket](https://bitbucket.org/fbhood/how-to-vuejs/src/master/15-routing/) 上查看知识库。

你也可以在这里看到 Vue 路由器文档[。](https://router.vuejs.org/installation.html#direct-download-cdn)

### 如何通过 CDN 安装 Vue 路由器

让我们以之前的 index.html 为例，VueJS CDN 将指向路由器`https://unpkg.com/vue-router@3.4.9/dist/vue-router.js`。

```
 <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VueJS / GitHub API</title>

</head>
<body>
    <div id="app"> 
        <div v-for="project in projects">
            <h2 class="title">{{project.full_name}}</h2>

            <div class="author">
                <img width="50px" :src="project.owner.avatar_url" alt="me">
            </div>
            <div class="view">
                <a :href="project.html_url">View</a>
            </div>
        </div>

    </div>
      <!-- Axios latest version CDN -->
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>

    <!-- VueJS development version -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

    <!-- Vue Router CDN -->
    <script src="https://unpkg.com/vue-router@3.4.9/dist/vue-router.js"></script>

    <!-- Main scrip file -->
    <script src="./main.js"></script>
</body>
</html> 
```

### 如何使用 Vue 路由器

现在，我们的应用程序可以访问路由器系统，我们可以为我们的应用程序添加一些路由。

我们可以使用库提供的`router-link`组件和它的`to`属性将链接指向特定页面。

```
<!-- Create a router link using the 'router-link' component and set the path using the 'to' attribute -->
<header>
    <nav>
        <router-link to="/">Home</router-link>
        <router-link to="/projects">Projects</router-link>
    </nav>
</header>

<!-- Render the component for the corresponding route -->
<router-view></router-view> 
```

我们还使用了路由器视图组件，它将为每条路由呈现一个特定的组件。

现在我们需要在 JavaScript 文件内部做一些事情来实现这一点。

让我们看看需要采取的步骤:

*   定义路线组件
*   定义路线
*   创建 Vue 路由器实例
*   创建并挂载 Vue 根实例。

首先，我们需要定义我们的组件，我们将使用这些组件来呈现页面的内容。

我们将创建两个组件，一个用于主页，一个用于项目页面。

为了简化步骤，我们将所有内容保存在同一个文件中，稍后进行重构。

让我们创建前两个基本组件，看看路由器是否工作:

```
// Create Route components
const Home = {template: '<div>My Portfolio</div>'} 
const Projects = {template: '<div> Projects </div>'} 
```

现在让我们按照剩余的步骤定义路由，创建 vue 路由器实例，并创建和挂载 Vue 根实例。

```
 // Define some routes
const routes = [
    {path: '/', component: Home},
    {path: '/projects', component: Projects}
];
// Create the router instance and pass the routes to it
const router = new VueRouter({
routes: routes
});
// Create and mount the root instance.

let app = new Vue({
    router 
}).$mount('#app'); 
```

就是这样。如果你访问主页，你会看到两个导航链接，网站内容也会相应改变。

让我们把这些放在一起，开始建立我们的投资组合。

在 Axios 部分的前一个示例中，我们从 GitHub API 请求用户的所有公共存储库，并将名称、用户头像和项目 URL 呈现到页面上。

让我们将一些逻辑移到使用路由的应用程序中。

这里我们需要做的主要更改是:

*   将 HTML 标记移动到项目组件的`template`属性中
*   将`data`属性移动到组件的`data`对象内部
*   将我们在挂载的钩子中编写的代码移动到我们的组件中。

最终的代码看起来像这样:

```
// Define route components

const Home = {template: '<div>My Portfolio</div>'} 
const Projects = {

    template: `<div> 
         <div v-for="project in projects">
            <h2 class="title">{{project.full_name}}</h2>

            <div class="author">
                <img width="50px" :src="project.owner.avatar_url" alt="me">
            </div>
            <div class="view">
                <a :href="project.html_url">View</a>
            </div>
        </div>
    </div>`,
    data(){
        return {
            projects: [],
            perPage: 20,
            page: 1
        }
    }, 
    mounted(){

         axios
         .get(`https://api.github.com/users/fabiopacifici/repos?per_page=${this.perPage}&page=${this.page}`)
         .then(
            response => {
                //console.log(response);
                this.projects = response.data;
            }
        )
        .catch(error=> {console.log(error);})
    }
} 

// Define some routes
const routes = [
    {path: '/', component: Home},
    {path: '/projects', component: Projects}
];
// Create the router instance and pass the routes to it
const router = new VueRouter({
routes: routes
});
// Create and mount the root instance.

let app = new Vue({
    router 
}).$mount('#app'); 
```

HTML 文件保持不变:

```
<div id='app'>
  <header>
            <nav>
                <router-link to="/">Home</router-link>
                <router-link to="/projects">Projects</router-link>
            </nav>
        </header>

        <router-view></router-view>
</div> 
```

现在我们有了一个基础，让我们改进它。我们将使用我用 Figma 制作的设计原型，并在我们的产品组合中添加一些功能，使它看起来很漂亮。

[https://www.youtube.com/embed/T_avTRFAEAg?feature=oembed](https://www.youtube.com/embed/T_avTRFAEAg?feature=oembed)

## 期末项目——如何用 VueJS、VueRouter、Axios、GitHub API 构建产品组合并部署到 Netlify

我们已经准备好构建我们的最终项目了！对于我们的 Vue-folio，我们将从上一节停止的地方开始。

我们将构建一个具有两条路径的单页面应用程序，一条用于主页，一条用于项目页面。

以下是构建模块:

*   Vuejs
*   路由器视图
*   阿克斯
*   GitHub rest API
*   投资组合设计

你可以点击这里在 [YouTube 上观看这个教程，点击这里](https://youtu.be/I6hQnWQU4rQ)在 [BitBucket 上查看知识库。](https://bitbucket.org/fbhood/how-to-vuejs/src/master/16-final-project-portfolio/)

### 项目结构

为了加快速度，我们将复制我们在上一节中编写的代码。

项目结构如下:

```
|-- index.html
|-- assets/
    |-- css/
        |-- style.css
    |-- js/
        |-- main.js
    |-- img/ 
```

### index.html 文件

index.html 文件与我们在上一节中看到的略有不同。这里，我们将只放置路由器视图组件，它负责显示与给定路由匹配的组件。

然后我们将把实际的`route-links`放在每个组件中，以确保我们得到了设计想要的结果。

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vuefolio</title>
    <link rel="preconnect" href="https://fonts.gstatic.com">
    <link href="https://fonts.googleapis.com/css2?family=Raleway:wght@100;300;400;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.1.1/css/all.css"
        integrity="sha384-O8whS3fhG2OnA5Kas0Y9l3cfpmYjapjI0E4theH4iuMD+pLhbf6JI0jIMfYcK3yZ" crossorigin="anonymous">
    <link rel="stylesheet" href="./assets/css/style.css">
</head>

<body>

    <div id="app">

        <!-- Render the component for the corresponding route -->
        <router-view></router-view>

    </div>
    <footer> © Developed by <a href="https://fabiopacifici.com">Fabio Pacific</a> </footer>
    <!-- Axios -->
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <!-- VueJS development version -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

    <!-- Vue Router -->
    <script src="https://unpkg.com/vue-router@2.0.0/dist/vue-router.js"></script>
    <!-- Main Js file -->
    <script src="./assets/js/main.js"></script>
</body>

</html> 
```

### style.css 文件

因为这不是一个 CSS 教程，所以对于 CSS 部分，如果你跟着做的话，你可以简单地从存储库文件中复制代码。

```
 /* Utility Classes */
    .d_none {
        display: none;
    }
    .d_flex {
        display: flex;
    }
    .container {
        max-width: 1170px;
        margin: auto;
    }
    a {
        color: white;
    } 
    a:hover {
        color:#DB5461;

    }
    .loading {
        font-size: 2rem;
    }
    /* END Utility Classes */
    /* Components */
    .bio__media {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        text-align: left;
    }
    .bio__media img {
        height: 120px;
    }
    .bio__media__text {
        padding: 1rem;
    }
    .bio__media__text h1{
        font-size: 36px;
        font-weight: 900;
        color: #DB5461;

    }
    .bio__media__text p {
        font-weight: 100;
        font-size: 16px;
        line-height: 1.5rem;

    }

    .card__custom {
        position: relative;
        display: flex;
        max-width: 400px;
        height: 300px;
        min-height: 300px;
        padding: 0.5rem;
        margin-bottom: 3rem;
        flex-grow: 1;
        flex-basis: calc(100% /2);
        align-items: center;
        justify-content: space-between;
    }
    .card__custom > .card__custom__text {
        max-width: calc((100% / 3) *2);
        text-align: right;
        height: 80%;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        overflow: hidden;

    }
    .card__custom__img {

        position: absolute;
        width: 70%;
        height: 100%;
        background-image: url(../img/cards_bg_img.svg);
        background-position: center;
        background-repeat: no-repeat;
        background-size: contain;
        display: inline-block;
        z-index: -1;
        left: 60%;
        transform: translateX(-50%);
        border-radius: 85px 0 100px 25px;

    }
    .card_custom__button a, .btn_load_more {
        background: #F1EDEE;
        border: 5px solid #3D5467;
        box-sizing: border-box;
        border-radius: 54px;
        padding: 0.5rem 1rem;
        font-weight: 900;
        color: #3D5467;
    }
    .card_custom__button a:hover, .btn_load_more:hover {
        cursor: pointer;
        background: #324555;
        color: white;
        border-color: #DB5461;
        transition: 1s;
    }
    .card__custom__text h3 {
        text-transform: uppercase;
        font-size: 1.5rem;
    }
    /* END Componenet */
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    body{
        font-family: 'Raleway', Arial, Helvetica, sans-serif;
        color: white;
        background: linear-gradient(116.82deg, #3D5467 0%, #1A232B 99.99%, #333333 100%);

    }
    a {
    text-decoration: none;   
    }

    /* Home Page */
    main#home {
        width: 100%;
        height: 100vh;
        min-height: 600px;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    #home > .about__me {
        text-align: center;
        width: 80%;
        line-height: 1.5rem;
    }
    #home > .about__me > h1 {
        margin: 20px 0 0;
        font-size: 36px;
        font-weight: 900;
        color: #DB5461;
    }
    #home > .about__me > h3 {
        font-size: 28px;
        font-weight: 500;

    }
    #home > .about__me > h1, #home > .about__me > h3  {
        font-style: normal;
        line-height: 42px;
        letter-spacing: 0.115em;

    }
    #home > .about__me p {
        font-weight: 100;
        font-size: 22px;
        padding: 2rem;
    }
    .skills_projects_link {
        position: relative;
    }
    .skills_projects_link > a {
        text-transform: uppercase;
        color: white;
        font-weight: 900;
        font-size: 18px;
        line-height: 21px;

    }
    .skills_projects_link > a:hover {
        color: #DB5461;
        transition: all 0.5s ease-in-out;

    }
    .skills_projects_link > a:hover::after {
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
        display: flex;
        margin: auto;
        text-align: center;
        content: "";
        width: 30px;
        height: 2px;
        background-color: #DB5461;
        transition: background-color 0.5s ease-in-out;

    }

    /* Header */
    #site_header {
        text-align: center;
        padding: 2rem 0;
        justify-content: space-between;
        align-items: center;
    }
    #site_header > h1 {
        text-transform: uppercase;
    }
    nav a {
            color: #e2e2e2;
        text-transform: uppercase;
        font-weight: 900;
    }
    nav a:hover {
        color: #DB5461;
    }
    /* Portfolio Page Section */

    #portfolio {
        margin-top: 4rem;
        display: flex;
        flex-wrap: wrap;
        align-items: center;
        justify-content: space-around;
    }
    .btn_load_more {

    }
    /* Skills */

    #skills_section {
        margin-top: 4rem;
        min-height: 300px;
        background-image: url(../img/skills_bg.svg);
        background-repeat: no-repeat;
        background-size: contain;
        background-position: top left;
    }
    #skills_section h2 {
        margin-left: 180px;
        font-size: 44px;
        color: #F1EDEE;
        line-height: 2rem;

    }
    #skills_section ul {
        list-style: none;
        margin: 20px 120px;
        display: flex;
        flex-wrap: wrap;

    }
    #skills_section ul  li {
        padding: 1rem;
        margin: 0.5rem;
        background-color: #DB5461;
        border: 5px solid #3D5467;
        border-radius: 35px;
    }

    .avatar {
        width: 30px;
        height: auto;
        border-radius: 50%;
            margin: 0 1rem;

    }

    .card__back {
        display: none;
    }
    .rotate__card {
        transform: rotate3d(360,0,0,180deg);
    }
    /* Site Footer */

    footer {
        text-align: center;
        padding: 2rem 0;
    }

    /* Media Query  */

    @media screen and (max-width: 475px) {
        .card {
            flex-basis: 100%;
            width: 100%;
        }
    } 
```

## Main.js 文件基本结构

在 main.js 文件中，我们有单页应用程序的核心。在这里，我们将定义需要为每个视图/页面、主页和项目组件呈现的路线组件。

然后，我们将定义两个路由，一个用于主页，一个用于项目页面，创建一个路由器实例，并将其传递给路由。最后，我们将创建一个新的 Vue 实例，并将路由器实例传递给它，并挂载根 HTML 元素。

让我们从路由组件开始。

### 如何为每个视图定义组件

主页组件相当简单。

```
// Homepage component
const Home = {
    template: 
    `<main id="home">
        <div class="about__me">
            <img src="./assets/img/avatar.svg" alt="">
            <h1>John Doe</h1>
            <h3>Python Expert</h3>
            <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. </p>

            <div class="skills_projects_link">
                <router-link to="/projects">Projects/Skills</router-link> 
            </div>
        </div>
    </main>`
} 
```

我们来分解一下。首先，我们创建一个 Home 常量来保存路由器组件对象。

在对象内部，我们唯一要放入的是带有一些标记的模板属性，以呈现我们的页面。这里要注意的主要事情是 router-link 组件，它将从主页指向/projects 路径。

接下来，让我们为项目页面创建一个路由组件。我们在这里有很多事情要做，所以现在让我们只添加一些样板代码-我们稍后会回来，一步一步地写出所有的逻辑。

```
const Projects = {
    template: 
    `<div>
        <h1>Projects</h1>
    </div>`,
    data() { 
        return {
                // Data object here
            }
    },
    methods: {
        // All methods here
    },
    mounted(){  
        // Lifecycle hook      

    }
} 
```

Projects route 组件有一个`template`属性，到目前为止它保存了一个基本的标记，但只显示了一个`h1`标题。

之后是返回一个空对象的组件的数据方法，然后是一个空的方法对象和一个空的生命周期钩子。

这就是我们目前所需要的，所以让我们继续定义其余的构建模块、路由、路由器和 Vue 实例。

### 如何定义路由、路由器和 Vue 实例

现在我们有两个组件要在主页上呈现，我们可以前进到下一步:

*   定义路线
*   创建路由器实例
*   创建并挂载 Vue 实例

首先，让我们定义我们的两条路线并链接组件。

```
 // Define routes
const routes = [
    {path: '/', component: Home},
    {path: '/projects', component: Projects},
]; 
```

在代码中，我们定义了一个名为 routes 的新常量。在其中，我们将两条路线定义为一个对象数组。

每个对象都有两个属性:

*   小路
*   成分

第一个对象是主页。它的路径将响应对我们网站基本 URL 的请求，如[https://fabiopacifici.com/](https://fabiopacifici.com/)。

然后，component 属性将该页面链接到我们在上一步中定义的名为`Home`的路由组件。

第二个对象用于项目页面。路径响应对`/projects`的请求，并链接到`Projects`路由组件。

现在我们有了路线:

```
 // create the router instance
const router = new VueRouter({
    routes
}) 
```

上面我们使用了 ES6 语法，它允许我们只输入保存路由的变量的名称，因为它等于我们需要使用的属性的名称。其实和写`routes: routes`是一样的。

现在，我们创建一个 Vue 实例。在其中注入路由器实例，最后挂载根元素。

```
 // create and mount the vue instance
const app = new Vue({
    router
}).$mount('#app'); 
```

搞定了。我们现在一切就绪，可以开始构建我们的投资组合并完成项目路线组件。

## 如何构建主项目路线组件

我们将开始处理数据对象。这里我们需要定义一些属性，一旦我们从 git hub API 获取数据，这些属性将保存我们所有的项目。

### 数据对象

为了简单起见，我有意将结果限制为 20 个。如果你觉得这还不够，你可以随意修改代码。

您可以通过增加将传递给查询字符串的 page 属性来实现结果的分页，或者通过增加`perPage`属性的值来返回每页更多的结果。

```
data() { 
    return {
        projects: [],
        projectsList: null,
        skills: [],
        projectsCount: 5,
        perPage: 20,
        page: 1,
        loading: true,
        errors: false,
        }
    }, 
```

正如我们在使用 Axios 从 GitHub REST API 获取数据的章节中所了解的，我们需要定义一些属性。

组件的数据函数返回一个带有`projects`属性的对象，我们将在其中存储从 GitHub 获取的所有项目。

然后我们添加一个`projectsList`属性，一次只保存几个项目。稍后我们将使用这个属性结合`projectsCount`属性实现一个非常简单的
load more 特性。

然后我们有一个`skills`属性，我们将在其中存储用于构建我们的项目的所有语言。

我们将使用`perPage: 20`和`page: 1`属性构建用于从 GitHub 获取数据的查询字符串。这将需要 20 个项目，
只返回结果的第一页，除非我们改变这些值。

最后，我们有一个`loading: true`属性，用于检查页面是否正在获取数据，还有一个`errors: false`属性，用于在我们无法连接到 GitHub 服务器时显示一条错误消息。

下一步，我们将开始研究使我们的应用程序工作所需的所有方法。

### 获取所有数据的方法

第一种方法是我们将用来从 GitHub 获取数据的方法。

该方法将使用 Axios 对 GitHub rest API 进行 Ajax 调用，并将响应存储在 Vue 实例的属性中:

```
 fetchData: function(){
            axios
            .get(`https://api.github.com/users/fbhood/repos?per_page=${this.perPage}&page=${this.page}`)
            .then(
                response => {

                    this.projects = response.data;
                    this.projects.forEach(project =>{
                        if (project.language !== null && ! this.skills.includes(project.language)) { 
                            this.skills.push(project.language)
                        };
                    });
                }
            )
            .catch(error=> {
                console.log(error);
                this.errors = true;
            })
            .finally(() => { 
                this.loading = false
                this.getProjects();
            })
        }, 
```

我们来分析一下。首先，我们定义了一个名为`fetchData: function(){}`的方法。这个方法使用 Axios 对
REST API 进行 API 调用。

在`.get()`方法中，我们也使用属性`perPage`和`page`作为查询字符串的一部分来构建 URL。

get 方法返回一个承诺，所以我们在承诺上使用了`.then()`方法来处理使用箭头函数`response => {}`的响应。

在 arrow 函数中，我们使用 `this.projects = response.data;`将响应数据存储在 Vue 实例的 projects 属性中。

接下来，我们使用一个`forEach`循环迭代每个项目，并使用下面的代码将存储库中使用的语言存储为技能:

```
this.projects.forEach(project =>{
    if (project.language !== null && ! this.skills.includes(project.language)) { 
        this.skills.push(project.language)
    };
}); 
```

我们链接了一个`.catch`方法来处理错误，以防我们无法连接到 rest API 并获取数据。我们将把错误记录到
控制台，并将`errors`属性的值更新为 true，这样我们就可以在以后向用户显示自定义的错误消息。

最后，我们链接了将在响应被处理后执行的`.finally()`方法。我们还更新了`loading`属性，并将其设置为 false，以便向用户显示结果。

在`finally`方法中，我们还可以调用一个方法(我们仍然需要创建这个方法),稍后我们将使用这个方法来分割结果。

让我们建造它。

### 获取项目方法

这个方法获取了我们实际存储在`projects`属性中的项目的一部分。我们可以使用`projectsList`属性来存储切片，然后实现一个方法，用 show more 按钮来增加切片。

```
 getProjects: function(){

    this.projectsList = this.projects.slice(0, this.projectsCount);
    return this.projectsList;

}, 
```

getProjects 方法使用数组切片方法和设置为 5 的属性`projectsCount`的
,获取存储在属性`projects`中的所有项目的一部分。所以它将只存储前五个结果并返回它们。

为了向`projectsList`属性添加另外五个项目，我们还需要一个方法，当用户单击 load more 按钮时可以调用这个方法。让我们创造它。

### 加载更多项目方法

load more 方法将首先检查`projects`数组的长度是否小于或等于`projectsList`数组的长度。然后，如果没有，它将把`projectsCount`属性的值增加 5，然后从`projects`属性中获取更大的份额。

```
 loadMore(){

    if(this.projectsList.length <= this.projects.length){
        this.projectsCount += 5;
        this.projectsList = this.projects.slice(0, this.projectsCount)
    }

} 
```

### 构建模板

在`Projects`组件的 template 属性中，我们可以从 header 部分开始。我们还将为页面导航放入两个`router-link`组件:

```
`<div>
        <header id="site_header" class="container d_flex">
            <div class="bio__media">
                <img src="./assets/img/avatar.svg" alt="">
                <div class="bio__media__text">
                    <h1>John Doe</h1>
                    <h3>Python Expert</h3>
                    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. </p>
                </div>
            </div>
            <nav>
                <router-link to='/'>Home</router-link>
                <router-link to="/projects">Project</router-link>
                <a href="https://">
                    <i class="fab fa-github fa-lg fa-fw"></i>
                </a>
            </nav>
        </header>

</div> 
```

接下来，我们可以继续处理模板并创建主要部分。我们将把下面的标记放在主模板 div 中，就在标题结束标记的下面。

让我们从放置主容器开始:

```
<main class="container">
    <!-- Show an error message if the REST API doensn't work -->
    <!-- Otherwise show  a section for our portfolio projects and skills section-->
</main> 
```

在容器内部，让我们使用 v-if-else 指令来显示错误消息或项目部分:

```
 <!-- Show Errors if the rest api doesn't work -->
    <div class="error" v-if="errors"> 
        Sorry! It seems we can't fetch data righ now 😥
    </div>
    <!-- Else show the portfolio section -->
    <section id="portfolio" v-else></section> 
```

为了让代码工作，我们使用了 v-if 指令，并向它传递了`errors`属性。如果我们从 GitHub 获取数据时出现错误，该属性将被设置为`true`，如果一切正常，该属性将被设置为`false`。所以 v-else 指令将呈现 portfolio 部分。

接下来，我们需要展示一个“加载...”我们获取数据时的消息。完成后，我们可以使用 v-for 指令遍历结果。所以就在投资组合部分，我们会写另一个 v-if-else 指令。

```
<section id="portfolio" v-else>
 <!-- Use a v-if directive to show the loading message -->
        <div class="loading" v-if="loading">😴 Loading ... </div>

        <!-- use a v-for directive to loop over the projectsList array -->
        <div v-for="project in projectsList" class="card__custom" v-else></div>

</section> 
```

这里我们使用 v-if 指令`<div class="loading" v-if="loading">😴 Loading ... </div>`来呈现加载消息。之后，`<div v-for="project in projectsList" class="card__custom" v-else></div>`
有两个指令，一个是 v-for 指令，我们用它来循环遍历`projectsList`属性，另一个是 v-else 指令，当我们完成从 GitHub 获取数据时，它会显示这个元素。

现在我们可以使用`project`变量在我们的标记中呈现所有项目细节:

```
<!-- use a v-for directive to loop over the projectsList array -->
<div v-for="project in projectsList" class="card__custom" v-else>
    <div class="card__custom__text">
        <div>
            <!-- Create a custom method to trim the project name so that it doesn't break the design -->
            <h3>{{project.name}}</h3>
            <!-- Create a custom trimmedText to trim the description -->
            <p>{{project.description}}</p>                        
        </div>

        <div class="meta__data d_flex">
            <div class="date">
                <h5>Updated at</h5>
                <div>{{new Date(project.updated_at).toDateString()}}</div>
            </div>
            <img class="avatar" :src="project.owner.avatar_url">

        </div>
    </div>
    <div class="card__custom__img"></div>
    <div class="card_custom__button">
        <a :href="project.html_url" target="_blank">
            Code
        </a>
    </div>

</div> 
```

为了呈现项目标题和描述，我们使用了属性`poject.name`和`project.description`。但是描述和标题会破坏我们的设计，除非我们在某个时候对它们进行修剪。

接下来，在具有类`date`的元素中，我们使用 `new Data().toDateString()`方法以可读格式呈现了项目数据。

为了呈现用户头像`<img class="avatar" :src="project.owner.avatar_url">`，我们使用了 v-bind di active 的快捷方式，这样我们就可以使用属性`project.owner.avatar_url`来获取头像 URL。

最后，为了呈现一个点击后将用户重定向到存储库页面的按钮，我们将`href`属性绑定到`project.html_url`属性`<a :href="project.html_url" target="_blank">Code</a>`。

我们的项目卡是完整的。我们需要做的下一件事是呈现一个 load more 按钮来显示更多的项目。

我们仍在`projects`部分内工作。就在项目卡之后，我们可以编写以下标记

```
<!-- Render a load more button -->
<div style="text-align: center; width:100%" v-if="!loading" >
    <div v-if="projectsList.length < projects.length">
        <button class="btn_load_more" v-on:click="loadMore()">Load More</button>
    </div>
    <div v-else>
        <a href="" target="_blank" rel="noopener noreferrer">Visit My GitHub</a>
    </div>

</div> 
```

v-if 指令首先检查 loading 属性是否设置为 false。
如果是这样，我们将使用另一个 v-if 指令来检查属性`projectsList`的长度是否小于属性`projects`的长度。

如果是这样，它将显示一个按钮，该按钮使用 v-on 指令来监听点击
`<button class="btn_load_more" v-on:click="loadMore()">Load More</button>`并触发一个`loadMore()`方法。否则，我们会显示 GitHub 帐户的链接。

在此之后，我们可以显示与所有项目相关的技能列表:

```
<!-- Show a skills section -->
<div id="skills_section">
    <h2>Development Skills</h2>
    <ul class="skills">
        <!-- Loop over the skills property -->
        <li v-for="skill in skills">{{skill}}</li>
    </ul>
</div> 
```

我们的标记是完整的，但我们需要改进我们的代码一点点，因为项目标题和它的描述打破了我们的设计！

让我们创建两个方法，一个用于修剪标题，一个用于描述文本。

### trimText 和 trimTitle 方法

`trimTitle`方法将所有的`-`和`_`替换为一个空格，并将字符数限制为 12。`trimText`方法只是减少了超过 100 个字符的描述的字符数。

```
trimTitle: function(text){
    let title = text.replaceAll("-", " ").replace("_", " ")
    if(title.length > 15) {
        return title.slice(0, 12) + ' ...'
    } return title;

},
trimText: function(text){
    //console.log(text.slice(0, 100));
    if(text.length > 100) {
        return text.slice(0, 100) + ' ...'
    } return text;
}, 
```

有了这两种方法，现在我们可以更新标记，并使用它们来确保没有东西破坏设计。

让我们更新这两行将被更改的内容:

```
<!-- Create a custom method to trim the project name so that it doesn't break the design -->
<h3>{{project.name}}</h3>
<!-- Create a custom trimmedText to trim the description -->
<p>{{project.description}}</p> 
```

对此:

```
<h3>{{trimedTitle(project.name)}}</h3>
<p>{{trimedText(project.description)}}</p> 
```

让我们把所有的东西放在一起。最终的标记如下:

```
<div>
    <header id="site_header" class="container d_flex">
        <div class="bio__media">
            <img src="./assets/img/avatar.svg" alt="">
            <div class="bio__media__text">
                <h1>John Doe</h1>
                <h3>Python Expert</h3>
                <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. </p>
            </div>
        </div>
        <nav>
            <router-link to='/'>Home</router-link>
            <router-link to="/projects">Project</router-link>
            <a href="https://">
                <i class="fab fa-github fa-lg fa-fw"></i>
            </a>
        </nav>
    </header>

        <main class="container">
        <div class="error" v-if="errors"> 
            Sorry! It seems we can't fetch data righ now 😥
        </div>

        <section id="portfolio" v-else>
            <div class="loading" v-if="loading">😴 Loading ... </div>
            <div class="projects" v-else>
                    <div v-for="project in projectsList" class="card__custom" >
                    <div class="card__custom__text">
                        <div>
                            <h3>{{trimedTitle(project.name)}}</h3>
                            <p>{{trimedText(project.description)}}</p>                        
                        </div>

                        <div class="meta__data d_flex">
                            <div class="date">
                                <h5>Updated at</h5>
                                <div>{{new Date(project.updated_at).toDateString()}}</div>
                            </div>
                            <img class="avatar" :src="project.owner.avatar_url">

                        </div>
                    </div>
                    <div class="card__custom__img"></div>
                    <div class="card_custom__button">
                        <a :href="project.html_url" target="_blank">
                            Code
                        </a>
                    </div>

                </div>

                <div style="text-align: center; width:100%" v-if="!loading" >
                    <div v-if="projectsList.length < projects.length">
                        <button class="btn_load_more" v-on:click="loadMore()">Load More</button>
                    </div>
                    <div v-else>
                        <a href="" target="_blank" rel="noopener noreferrer">Visit My GitHub</a>
                    </div>

                </div>

                <div id="skills_section">
                    <h2>Development Skills</h2>
                    <ul class="skills">
                        <li v-for="skill in skills">{{skill}}</li>
                    </ul>
                </div>
            </div>
        </section>  
    </main>
</div> 
```

还有最后一件事要做。由于从 GitHub 获取数据非常快，我们实际上看不到加载消息。让我们设置一个超时时间，延迟几秒钟，然后你就可以随意调整了。

### 安装的生命周期挂钩

```
 mounted(){  

        setTimeout(this.fetchData, 3000 );

    } 
```

在挂载的生命周期钩子中，我们使用了`setTimeout()`并调用了`fetchData`方法作为第一个参数。然后，对于第二个参数，我们指定该方法应该在 3000 毫秒(3 秒)后执行。

## 让我们一起看看我们的最终代码

Index.html 文件看起来如下:

```
 <!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vuefolio</title>
    <link rel="preconnect" href="https://fonts.gstatic.com">
    <link href="https://fonts.googleapis.com/css2?family=Raleway:wght@100;300;400;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.1.1/css/all.css"
        integrity="sha384-O8whS3fhG2OnA5Kas0Y9l3cfpmYjapjI0E4theH4iuMD+pLhbf6JI0jIMfYcK3yZ" crossorigin="anonymous">
    <link rel="stylesheet" href="./assets/css/style.css">
</head>

<body>

    <div id="app">

        <!-- Render the component for the corresponding route -->
        <router-view></router-view>

    </div>
    <footer> © Developed by <a href="https://fabiopacifici.com">Fabio Pacific</a> </footer>
    <!-- Axios -->
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
    <!-- VueJS development version -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

    <!-- Vue Router -->
    <script src="https://unpkg.com/vue-router@2.0.0/dist/vue-router.js"></script>
    <!-- Main Js file -->
    <script src="./assets/js/main.js"></script>
</body>

</html> 
```

这是 main.js 文件:

```
// Create route components
const Home = {
    template: 
    `<main id="home">
        <div class="about__me">
            <img src="./assets/img/avatar.svg" alt="">
            <h1>John Doe</h1>
            <h3>Python Expert</h3>
            <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. </p>

            <div class="skills_projects_link"><router-link to="/projects">Projects/Skills</router-link> </div>
        </div>
    </main>`
}
const Projects = {
    template: 
    `<div>
        <header id="site_header" class="container d_flex">
            <div class="bio__media">
                <img src="./assets/img/avatar.svg" alt="">
                <div class="bio__media__text">
                    <h1>John Doe</h1>
                    <h3>Python Expert</h3>
                    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. </p>
                </div>
            </div>
            <nav>
                <router-link to='/'>Home</router-link>
                <router-link to="/projects">Project</router-link>
                <a href="https://">
                    <i class="fab fa-github fa-lg fa-fw"></i>
                </a>
            </nav>
        </header>

         <main class="container">
            <div class="error" v-if="errors"> 
                Sorry! It seems we can't fetch data righ now 😥
            </div>

            <section id="portfolio" v-else>
                <div class="loading" v-if="loading">😴 Loading ... </div>
                <div class="projects" v-else>
                     <div v-for="project in projectsList" class="card__custom" >
                        <div class="card__custom__text">
                            <div>
                                <h3>{{trimedTitle(project.name)}}</h3>
                                <p>{{trimedText(project.description)}}</p>                        
                            </div>

                            <div class="meta__data d_flex">
                                <div class="date">
                                    <h5>Updated at</h5>
                                    <div>{{new Date(project.updated_at).toDateString()}}</div>
                                </div>
                                <img class="avatar" :src="project.owner.avatar_url">

                            </div>
                        </div>
                        <div class="card__custom__img"></div>
                        <div class="card_custom__button">
                            <a :href="project.html_url" target="_blank">
                                Code
                            </a>
                        </div>

                    </div>

                    <div style="text-align: center; width:100%" v-if="!loading" >
                        <div v-if="projectsList.length < projects.length">
                            <button class="btn_load_more" v-on:click="loadMore()">Load More</button>
                        </div>
                        <div v-else>
                            <a href="" target="_blank" rel="noopener noreferrer">Visit My GitHub</a>
                        </div>

                    </div>

                    <div id="skills_section">
                        <h2>Development Skills</h2>
                        <ul class="skills">
                            <li v-for="skill in skills">{{skill}}</li>
                        </ul>
                    </div>
                </div>
            </section>

        </main>
    </div>`,
data() { 
    return {
        data: [],
        projects: [],
        projectsList: null,
        skills: [],
        projectsCount: 5,
        perPage: 20,
        page: 1,
        loading: true,
        errors: false,
        }
    },
    methods: {
        trimedTitle: function(text){
            let title = text.replaceAll("-", " ").replace("_", " ")
            if(title.length > 15) {
                return title.slice(0, 12) + ' ...'
            } return title;

        },
        trimedText: function(text){
            //console.log(text.slice(0, 100));
            if(text === null) {
                return 'This project has no description yet!';
            } else if(text.length > 100) {
                return text.slice(0, 100) + ' ...'
            } 
            return text;

        },
        getProjects: function(){

            this.projectsList = this.projects.slice(0, this.projectsCount);
            return this.projectsList;

        },
        fetchData: function(){
            axios
            .get(`https://api.github.com/users/fbhood/repos?per_page=${this.perPage}&page=${this.page}`)
            .then(
                response => {
                    this.projects = response.data;
                    this.projects.forEach(project =>{
                        if (project.language !== null && ! this.skills.includes(project.language)) { 
                            this.skills.push(project.language)
                        };
                    });
                }
            )
            .catch(error=> {
                console.log(error);
                this.errors = true;
            })
            .finally(() => { 
                this.loading = false
                this.getProjects();
            })
        }, 
        loadMore(){

            if(this.projectsList.length <= this.projects.length){
                this.projectsCount += 5;
                this.projectsList = this.projects.slice(0, this.projectsCount)
            }

        }

    },
    mounted(){  

        setTimeout(this.fetchData, 3000 );

    }
}

// Define routes
const routes = [
    {path: '/', component: Home},
    {path: '/projects', component: Projects},
];

// create the router instance
const router = new VueRouter({
    routes
})

// create and mount the vue instance
const app = new Vue({
    router
}).$mount('#app'); 
```

在 CSS 方面，这是我们所拥有的:

```
 /* Utility Classes */
.d_none {
    display: none;
}
.d_flex {
    display: flex;
}
.container {
    max-width: 1170px;
    margin: auto;
}
a {
    color: white;
} 
a:hover {
    color:#DB5461;

}
.loading {
    font-size: 2rem;
}
/* END Utility Classes */
/* Components */
.bio__media {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    text-align: left;
}
.bio__media img {
    height: 120px;
}
.bio__media__text {
    padding: 1rem;
}
.bio__media__text h1{
    font-size: 36px;
    font-weight: 900;
    color: #DB5461;

}
.bio__media__text p {
    font-weight: 100;
    font-size: 16px;
    line-height: 1.5rem;

}

.card__custom {
    position: relative;
    display: flex;
    max-width: 400px;
    height: 300px;
    min-height: 300px;
    padding: 0.5rem;
    margin-bottom: 3rem;
    flex-grow: 1;
    flex-basis: calc(100% /2);
    align-items: center;
    justify-content: space-between;
}
.card__custom > .card__custom__text {
    max-width: calc((100% / 3) *2);
    text-align: right;
    height: 80%;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    overflow: hidden;

}
.card__custom__img {

    position: absolute;
    width: 70%;
    height: 100%;
    background-image: url(../img/cards_bg_img.svg);
    background-position: center;
    background-repeat: no-repeat;
    background-size: contain;
    display: inline-block;
    z-index: -1;
    left: 60%;
    transform: translateX(-50%);
    border-radius: 85px 0 100px 25px;

}
.card_custom__button a, .btn_load_more {
    background: #F1EDEE;
    border: 5px solid #3D5467;
    box-sizing: border-box;
    border-radius: 54px;
    padding: 0.5rem 1rem;
    font-weight: 900;
    color: #3D5467;
}
.card_custom__button a:hover, .btn_load_more:hover {
    cursor: pointer;
    background: #324555;
    color: white;
    border-color: #DB5461;
    transition: 1s;
}
.card__custom__text h3 {
    text-transform: uppercase;
    font-size: 1.5rem;
}
/* END Componenet */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body{
    font-family: 'Raleway', Arial, Helvetica, sans-serif;
    color: white;
    background: linear-gradient(116.82deg, #3D5467 0%, #1A232B 99.99%, #333333 100%);

}
a {
 text-decoration: none;   
}

/* Home Page */
main#home {
    width: 100%;
    height: 100vh;
    min-height: 600px;
    display: flex;
    justify-content: center;
    align-items: center;
}
#home > .about__me {
    text-align: center;
    width: 80%;
    line-height: 1.5rem;
}
#home > .about__me > h1 {
    margin: 20px 0 0;
    font-size: 36px;
    font-weight: 900;
    color: #DB5461;
}
#home > .about__me > h3 {
    font-size: 28px;
    font-weight: 500;

}
#home > .about__me > h1, #home > .about__me > h3  {
    font-style: normal;
    line-height: 42px;
    letter-spacing: 0.115em;

}
#home > .about__me p {
    font-weight: 100;
    font-size: 22px;
    padding: 2rem;
}
.skills_projects_link {
    position: relative;
}
.skills_projects_link > a {
    text-transform: uppercase;
    color: white;
    font-weight: 900;
    font-size: 18px;
    line-height: 21px;

}
.skills_projects_link > a:hover {
    color: #DB5461;
    transition: all 0.5s ease-in-out;

}
.skills_projects_link > a:hover::after {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    margin: auto;
    text-align: center;
    content: "";
    width: 30px;
    height: 2px;
    background-color: #DB5461;
    transition: background-color 0.5s ease-in-out;

}

/* Header */
#site_header {
    text-align: center;
    padding: 2rem 0;
    justify-content: space-between;
    align-items: center;
}
#site_header > h1 {
    text-transform: uppercase;
}
nav a {
        color: #e2e2e2;
    text-transform: uppercase;
    font-weight: 900;
}
nav a:hover {
    color: #DB5461;
}
/* Portfolio Page Section */

#portfolio {
    margin-top: 4rem;

}

#portfolio .projects {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: space-around;
}
/* Skills */

#skills_section {
    margin-top: 4rem;
    min-height: 300px;
    background-image: url(../img/skills_bg.svg);
    background-repeat: no-repeat;
    background-size: contain;
    background-position: top left;
}
#skills_section h2 {
    margin-left: 180px;
    font-size: 44px;
    color: #F1EDEE;
    line-height: 2rem;

}
#skills_section ul {
    list-style: none;
    margin: 20px 120px;
    display: flex;
    flex-wrap: wrap;

}
#skills_section ul  li {
    padding: 1rem;
    margin: 0.5rem;
    background-color: #DB5461;
    border: 5px solid #3D5467;
    border-radius: 35px;
}

.avatar {
    width: 30px;
    height: 30px;
    border-radius: 50%;
        margin: 0 1rem;

}

.card__back {
    display: none;
}
.rotate__card {
    transform: rotate3d(360,0,0,180deg);
}
/* Site Footer */

footer {
    text-align: center;
    padding: 2rem 0;
}

/* Media Query  */

@media screen and (max-width: 475px) {
    .card {
        flex-basis: 100%;
        width: 100%;
    }
} 
```

就是这样！我们准备将代码投入生产。

[https://www.youtube.com/embed/I6hQnWQU4rQ?feature=oembed](https://www.youtube.com/embed/I6hQnWQU4rQ?feature=oembed)

## 使用 BitBucket 和 Netlify 继续部署

最后一步是部署我们的项目，以便其他人可以看到它们。为此，我们将使用两个服务:

*   BitBucket，一个用于托管的基于 git 的源代码库(如果您愿意，可以使用 GitHub)
*   Netlify 是一家虚拟主机公司，为那些将源代码文件存储在 Git 版本控制系统中的网站提供主机服务。

你可以点击在 [YouTube 上观看这个最终视频。](https://youtu.be/BH5I68DzcYQ)

您还可以在 BitBucket 上签出最终存储库:

*   简单推特
*   [虚拟投资组合](https://bitbucket.org/fbhood/vue-folio/src)

### 创建项目文件夹并复制所有文件

首先，创建两个文件夹，每个项目一个:

*   视图-页面
*   然后复制相关文件夹中的所有项目文件。

### 初始化 git 存储库

接下来，我们需要在本地初始化 git 存储库。

```
cd vue-folio 
git init
git add .
git commit -m"Initial Commit" 
```

您需要在终端执行上面的命令，所以您需要在您的系统上安装 git。如果你不知道，你可以在这里阅读如何做到这一点。

使用第一个命令，我们导航到名为`vue-folio`的项目文件夹。然后，我们初始化 git 存储库，将所有文件添加到暂存区，并提交文件。

对两个项目文件夹重复上述步骤。

### 创建 BitBucket 或 GitHub 存储库

我假设你已经有了一个 GitHub o BitBucket 的账户。但是如果你没有，那就去创建一个。

按照视频中的步骤创建存储库，并将它们与您的本地存储库连接起来。

### 创建一个 Netlify 帐户并创建一个网站

你可以将 Netlify 的免费计划用于私人项目、爱好网站和实验。非常适合我们的教程。

按照视频中的步骤在那里部署您的项目。

[https://www.youtube.com/embed/BH5I68DzcYQ?feature=oembed](https://www.youtube.com/embed/BH5I68DzcYQ?feature=oembed)

## 下一步是什么？

在接下来的教程中，我将向你展示如何测试你的代码，升级到 Vue3，等等。

### 感谢您的阅读！

我希望你喜欢这个教程和附带的视频。如果是这样，请分享这篇文章，并点击视频上的“喜欢”按钮。您也可以通过点击铃声图标来启用通知，以了解我的下一个视频何时上线。

如果你有任何问题，请联系我。我回复所有的 YouTube 评论。

别忘了在这里订阅我的 YouTube 频道[。](https://youtube.com/channel/UCTuFYi0pTsR9tOaO4qjV_pQ)