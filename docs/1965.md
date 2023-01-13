# 如何通过一个简单的示例项目从 Vue v.2 迁移到 Vue v.3

> 原文：<https://www.freecodecamp.org/news/migrate-from-vue2-to-vue3-with-example-project/>

## Vue.js 是什么？

js 是一个由尤雨溪编写的渐进式 JavaScript 前端框架。它是最强大、最容易学习的框架之一，每月下载量超过 950 万次。

2020 年 9 月，Vue 3 core 发布。新的 Vue.js 版本引入了一些很酷的新功能，但也有一些突破性的变化。

## 我为什么要迁移到 Vue3？

随着技术行业的发展，库、语言和框架也在发展。在每个版本中，错误被修复，新的特性被引入。通常，在任何主要版本中，您的工作流都会得到增强。新的功能可以让你有机会做以前单调乏味的事情。

Vue 3 还是比较新的。您不必迁移所有的项目，但是随着时间的推移，对版本 2 的支持可能会终止。因此，了解迁移项目需要采取的步骤是一个好主意。

在本指南中，我将带您了解完成迁移所需的基本步骤。我们将采用一个简单的项目，并将其迁移到 Vue 3。

我们将使用的项目是有意简单的，因此任何人都可以跟着做。您的项目越复杂，您就越需要仔细规划迁移。

## 介绍

新的 Vue.js 版本确实带来了一些突破性的变化和新功能。此外，像 Vue 路由器这样的流行库已经更新，以支持新的 Vue 版本。

如果你已经知道了 Vue 2，基本都差不多。但是在您将项目迁移到 Vue 3 之前，您需要考虑一些变化。

根据您想要迁移的项目的大小，确保考虑到新版本引入的所有变化，以便您的应用程序在迁移后仍然可以工作。

对于本教程，我将保持简单，并向您展示如何迁移当前使用 Vue 2 CDN 的 Vue.js 项目。

我从我为 freeCodeCamp 写的书中提取了这个项目，你可以在这里找到。

在那个项目中，我们使用了 Vue 路由器，所以我们也将在本文中看看 Vue 路由器的变化。

## 你需要遵循这篇文章

要继续下去，你需要一个 Vue.js 和 Vue 路由器的基本知识。如果你没有的话。那么我建议你先看看我在 [freeCodeCamp](https://www.freecodecamp.org/news/build-a-portfolio-with-vuejs/) 上的书。

你也可以在我的 YouTube 频道上免费找到完整的 8 小时课程播放列表。

## 我们将在本文中讨论的内容

本教程分为三个主要章节。首先，我们将看看 Vue.js v3.x 中的变化，然后快速概述 Vue Router v4.x。最后，我们将开始规划一个实际项目的迁移。

*   Vue v3.x 概述
    *   重大变化
*   Vue 路由器 v4.x 概述
    *   重大变化
*   投资组合项目迁移
    *   克隆回购
    *   更新 CDN 脚本
    *   更新 Vue 实例
    *   更新 Vue 路由器实例

如果你想继续下去，这里有这篇文章的视频版本:

[https://www.youtube.com/embed/5y8-fKSY_Lg?feature=oembed](https://www.youtube.com/embed/5y8-fKSY_Lg?feature=oembed)

观看视频将帮助您在阅读以下步骤的同时巩固您的学习。在这里，您可以找到项目的最终[存储库](https://bitbucket.org/fbhood/advanced-vuejs/src/master/)。

## Vue v3.x 概述

Vue 3 引入了一些新功能和一些突破性的变化。让我们看看这些变化将如何影响我们的应用程序，并在迁移之前考虑它们。

### Vue V3.x 重大变更

在 Vue 3 中，突破性的变化基本上分为七类:

*   全局 API
    (负责 Vue 的行为)——你很可能想看看这些变化。
*   模板指令
    (对垂直指令工作方式的改变)——你很可能想看看这些改变。
*   组件
    (组件工作方式的变化)——如果你的应用程序使用了组件，你很有可能想看看这些变化
*   Render 函数(允许您以编程方式创建 HTML 元素)
*   自定义元素(告诉 Vue 关于自定义 HTML 元素的创建)
*   微小的变化(这些可能不会影响到你，但是你仍然会想看看这些)
*   移除了 API(Vue 3 中不再提供的东西)

在所有的变化中，有一些是任何应用程序都会使用的，比如全局 API 和组件。所以，如果你想开始使用新的 Vue 版本，你需要考虑它们。

值得一提的是以下附加变化:

*   创建 Vue 应用程序和组件实例的方式已经改变(全局 API)
*   您应该始终将数据选项声明为一个函数(微小的变化)
*   在同一个元素上使用 v-if 和 v-for 时改变优先级(模板地址)
*   您应该为组件事件(组件)声明一个发出选项

要获得变更的完整列表，您可以前往[文档](https://v3.vuejs.org/guide/migration/introduction.html#breaking-changes)

现在让我们更详细地看一下这些变化。

### 如何在 Vue 3 中创建应用程序和组件实例

在 Vue 3 中，创建应用程序的方式已经改变。Vue 应用现在使用新的`.createApp()`方法来创建应用实例。

Vue 应用程序现在被认为是一个根组件，因此您定义其数据选项的方式也发生了变化。

HTML 根元素没有改变，所以在 index.html 文件中，您仍然会看到类似这样的内容:

```
<div id="app"></div> 
```

在 JavaScript 文件中，您需要理解一个重要的变化:您将不再使用`new Vue()`来创建新的应用程序实例，而是使用一个名为`createApp()`的新方法:

```
 // Vue 3 syntax

const app = Vue.createApp({
    // options object
})
app.mounth('#app') // Vue Instance - Root component

// Vue 2 syntax
const app = new Vue({
    // options object
    el: '#app'
}) 
```

### 如何在 Vue 3 中定义一个组件

要在 Vue 3 中定义一个组件，不再使用`Vue.component()`。相反，您现在使用应用程序根组件，如下所示:

```
/* Vue 3 syntax */
const app = Vue.createApp({
    // options here
})

app.component('componenet-name', {
    // component code here
})

/* Vue 2 syntax*/
Vue.component('component-name', {
    // component code here
}) 
```

### 如何使用 Vue 3 中的数据选项对象

假设主应用程序实例现在被认为是根组件，您不能再将数据属性指定为对象。相反，您需要将其定义为一个返回对象的函数，就像您通常在组件中所做的那样。

```
// Vue 3
const app = Vue.createApp({
    // options object
    data(){
        return {
            message: 'hi there'
        }
    }
})
app.mounth('#app') // Vue Instance - Root component

// Vue 2 syntax
const app = new Vue({
    // options object
    el: '#app'
    data: {
        message: 'hi there'
    }
}) 
```

### 更改 Vue 3 中 v-if/v-for 的优先级

在 Vue 2 中，如果在同一个元素上使用了两个指令，v-for 指令将优先于 v-if 指令。但是在 Vue 3 v-if 总是优先。

然而，同时使用这两个指令并不是一个好主意。请务必访问文档[此处](https://v3.vuejs.org/guide/migration/v-if-v-for.html#overview)以了解更多信息。

### 如何在 Vue 3 中的组件事件上使用 Emits 属性(重大更改/新功能)

类似于`props`属性，现在在 Vue 3 中也有一个`emits`属性，组件可以使用它来声明它可以向父组件发出的事件。

我强烈建议使用该属性来避免在需要重新发出本机事件的组件中两次发出事件，比如单击事件。

以下是官方文档中的一个示例:

```
<template>
  <div>
    <p>{{ text }}</p>
    <button v-on:click="$emit('accepted')">OK</button>
  </div>
</template>
<script>
  export default {
    props: ['text'],
    emits: ['accepted']
  }
</script> 
```

emits 属性也可以接受对象。

我不会深入探讨这个问题，但我保证，我迟早会在一个专门的视频系列中处理每个特性/变化。

## Vue 路由器 v4.x 概述

随着 Vue.js 的新发布，我们也有了新版本的 Vue 路由器。如果您想将项目迁移到新的 Vue 版本，新版本 v4.x 有一些突破性的变化，您需要考虑这些变化。

### Vue 路由器 V4 中断更改

两个突破性的变化尤其值得一提，因为它们是 Vue 路由器应用程序的基础。您需要了解它们，以便以后迁移您的应用程序。

*   Vue 路由器实例已更改
*   有一个新的历史选项

完整的变更列表可在[这里](https://next.router.vuejs.org/guide/migration/index.html)获得。

我们来深入的看看这两个变化。

### Vue 路由器 4 实例已更改

要创建一个新的 Vue 路由器实例，您不再需要使用 Vue Router 函数构造函数。

这里是 Vue 路由器 v.3x [文档](https://router.vuejs.org/guide/#javascript)这样你可以比较一下。

代码的变化如下:

```
// 3\. Create the router instance and pass the `routes` option
// You can pass in additional options here, but let's
// keep it simple for now.
const router = new VueRouter({
  routes // short for `routes: routes`
})

// 4\. Create and mount the root instance.
// Make sure to inject the router with the router option to make the
// whole app router-aware.
const app = new Vue({
  router
}).$mount('#app') 
```

对此:

```
// 3\. Create the router instance and pass the `routes` option
// You can pass in additional options here, but let's
// keep it simple for now.
const router = VueRouter.createRouter({
  // 4\. Provide the history implementation to use. We are using the hash history for simplicity here.
  history: VueRouter.createWebHashHistory(), // <-- this is a new property and it is mandatory!
  routes, // short for `routes: routes`
})

// 5\. Create and mount the root instance.
const app = Vue.createApp({})
// Make sure to _use_ the router instance to make the
// whole app router-aware.
app.use(router)

app.mount('#app') 
```

在上面的代码中，要创建一个新的 Vue 路由器实例，您现在必须使用 Vue router 对象并调用`createRouter()`方法。

另外，新的历史属性是强制性的-`history: VueRouter.createWebHashHistory()`。您必须定义它，否则您将得到一个控制台错误。

接下来，您将使用`createApp()`方法创建 Vue 实例，并使用变量`app`调用`.use()`方法。您将在前面的步骤中创建的路由器实例传递到那里。

最后，您可以使用`app.mount('#app')`在 app 实例上挂载根 DOM 元素。

更多细节可以阅读 Vue 路由器 v4.x [文档](https://next.router.vuejs.org/guide/#javascript)。

## 如何将一个投资组合项目从 Vue 2 迁移到 Vue 3

如果你愿意跟随，你可以在 YouTube 上的视频中看到如何做到这一点。

考虑到以上所有情况，并在仔细审查了重大变化后，让我们尝试升级我们的一个项目 my Vue course。我会用文件夹，这门课的最后一个项目。

我们需要:

*   克隆回购
*   更新 CDN 脚本
*   更新 Vue 实例
*   更新 Vue 路由器实例

要将我们的应用迁移到 Vue 3，我们肯定需要更新以下内容:

*   Vue 应用程序实例
*   vue-路由器实例
*   CDN 链接

让我们一步一步来。

### 克隆项目存储库

首先，确保在当前文件夹中克隆 repo:

```
git clone https://bitbucket.org/fbhood/vue-folio/src/master/ vue-folio 
```

由于我们的项目仍然使用 CDN，下一步是更新它的链接。

### 更新项目的 CDN

在我们的项目中，我们同时使用 Vue CDN 和 Vue 路由器 CDN，所以让我们更新它们。

打开 index.html 文件并替换它:

```
 <!-- VueJS 3 production version -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script>
    <!-- Vue Router -->
    <script src="https://unpkg.com/vue-router@2.0.0/dist/vue-router.js"></script> 
```

有了这个:

```
 <!-- VueJS 3 -->
    <script src="https://unpkg.com/vue@3"></script>

    <!-- Vue Router -->
    <script src="https://unpkg.com/vue-router@4"></script> 
```

### 更新代码

现在，如果您使用实时服务器打开项目并打开检查器，您会注意到应用程序没有显示，并且在控制台中有两个错误。两者似乎都与 Vue 路由器有关:

```
You are running a development build of Vue.
Make sure to use the production build (*.prod.js) when deploying for production.

Uncaught TypeError: window.Vue.use is not a function
    at vue-router.js:1832
    at vue-router.js:9
    at vue-router.js:10

Uncaught ReferenceError: VueRouter is not defined
    at main.js:185 
```

Vue 路由器？！为什么？

请记住，当 Vue 被重写时，它的库也必须更新它们的代码库。因此，不要忘记那些与 Vue-router 相关的重大变化，因为我们的应用程序使用了它。

让我们首先更新主 Vue 实例以使用新语法。然后，我们将看看我们需要做什么改变，使 Vue 路由器的工作。

从以下位置更新 main.js 文件中的代码:

```
// create and mount the Vue instance

const app = new Vue({
    router
}).$mount('#app') 
```

对此:

```
// create and mount the Vue instance

const app = Vue.createApp({
    router
})
app.mount('#app') 
```

### Vue 路由器 4 发生变化

上面我们看到了定义根 Vue 实例组件的新语法。但是现在，由于我们使用 Vue 路由器，我们也需要考虑它的[breaking 变化](https://next.router.vuejs.org/guide/migration/index.html)。

#### Vue 路由器的实例化方式已经改变

它的变化是这样的:

```
// create the router instance
const router = new VueRouter({
    routes
}) 
```

对此:

```
// create the router instance
const router = VueRouter.createRouter({
    // Provide the history implementation to use. We are using the hash history for simplicity here.
    history: VueRouter.createWebHashHistory(),
    routes, // short for `routes: routes`
}) 
```

上面的代码处理了两个主要变化:`new VueRouter()`已经被`VueRouter.createRouter()`取代，新的`history`选项现在取代了`mode`。

请访问 Vue Router 4 的[文档](https://next.router.vuejs.org/guide/#html)了解更多信息。

最后，让我们的应用程序知道我们正在使用 Vue 路由器。如果我们在 Vue 应用程序中注入了路由器实例，现在我们需要指示它使用 Vue 路由器，使用`.use()`方法这样做，并将路由器实例传递给它。

与此不同:

```
// create and mount the Vue instance

const app = Vue.createApp({
    router
})
app.mount('#app') 
```

对此:

```
// create and mount the Vue instance

const app = Vue.createApp({})
app.use(router)
app.mount('#app') 
```

现在你知道了！

## 结论

你的 Vue 应用程序有多复杂并不重要——如果你想迁移到一个新的主要版本，你仍然需要为它做计划，阅读发行说明，并检查所有的突破性变化，以确保你理解什么会被突破。

应用程序越复杂，您就应该越仔细地规划您的迁移。

对于我们的简单应用程序来说，这就是要做的全部工作。但并不总是这样。所以提前做好准备和计划。

如果你喜欢这个指南，请分享这篇文章，记得订阅我的 YouTube 频道。感谢阅读！