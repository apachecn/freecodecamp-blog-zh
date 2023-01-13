# 如何使用 Vue.js、Vuex、Vuetify 和 Firebase 构建 SPA:使用 Vue 路由器

> 原文：<https://www.freecodecamp.org/news/how-to-build-an-spa-using-vue-js-vuex-vuetify-and-firebase-using-vue-router-fc5bd065fe18/>

#### 第 2 部分:了解如何在您的 SPA 中使用 Vue 路由器

![E1pvqQ1jnZas610gycuziZk7Tu8KaK6uygz8](img/a305b625bdbd0ec3e0b778154f748a02.png)

Meal Prep application

了解如何使用 Vue.js、Vuex、Vue Router 和 Firebase 创建送餐网站。

这是关于构建 Vue 应用程序的四篇系列文章中的第二篇。以下是所有零件的清单:

[第 1 部分:安装 Vue 并使用 Vue 化和 Vue 路由器构建 SPA](https://medium.freecodecamp.org/how-to-build-a-single-page-application-using-vue-js-vuex-vuetify-and-firebase-838b40721a07)

[第二部分:使用 Vue 路由器](https://medium.com/p/fc5bd065fe18)

[第三部分:使用 Vuex 和访问 API](https://medium.com/p/f8036aa464ad)

[第 4 部分:使用 Firebase 进行认证](https://medium.com/p/d9932d1e4365)

### 概述

在本系列的第一部分中，我们使用 Vue CLI 创建了 Vue 应用程序。此外，我们在应用程序中添加了 Vuetify。我使用 Vuetify 来设计应用程序。我还将利用它提供的许多 UI 组件。

安装好所有东西后，我们设计了应用程序的主页。

### 使用 Vue 路由器

Vue 路由器为我们的应用程序提供导航。当您点击*登录*按钮时，它会将您重定向到登录页面。当你点击*菜单*按钮时，它会将你重定向到显示可用膳食计划的页面。

`router.js` 文件包含路由配置。打开那个文件。在该文件中，您将看到两条路线。当您点击`‘/’` route 时显示 Home.vue 组件。另一个在您点击路线“about”时显示 about.vue 组件。

我们需要为应用程序中的每个页面创建路线。我们的应用程序将需要以下路线:

*   /
*   /菜单
*   /登录
*   /加入

当我们使用 Vue CLI 创建应用程序时，我们选择安装 Vue 路由器。默认情况下，这会为主页的“/”和关于页面的“/about”创建路线。在第 4 部分中，我们将使用 about 页面显示用户已经订购的所有食谱。

我们需要向 routes 数组中添加三条新路由。添加这些新路线后，我们的`router.js`文件看起来是这样的:

```
import Vue from 'vue';import Router from 'vue-router';import Home from './views/Home.vue';
```

```
Vue.use(Router);
```

```
export default new Router({    mode: 'history',    base: process.env.BASE_URL,    routes: [        {            path: '/',            name: 'home',            component: Home        },        {            path: '/about',            name: 'about',            component: () =&gt; import('./views/About.vue')        },        {            path: '/menu',            name: 'menu',            component: () => import('./views/Menu.vue')        },        {            path: '/sign-in',            name: 'signin',            component: () =&gt; import('./views/Signin.vue')        },        {            path: '/join',            name: 'join',            component: () => import('./views/Join.vue')        }    ]});
```

#### 视图与组件

在第一课中，我们创建了几个新的 Vue 组件。我将这些组件放在组件文件夹中。对于这三个新组件，我们不会在 components 文件夹中创建它们。相反，我们将把它们放在视图文件夹中。原因是使用类似于`/menu`的 URL 点击的任何内容都属于 views 文件夹。其他的都应该在 components 文件夹中。

#### 创建新视图

我们需要为三条新路线中的每一条创建新视图。在“视图”文件夹中，创建以下三个文件:

*   菜单。视图
*   Signin，视野
*   join . view-关连

在每个文件中添加一个带有<v-layout>的<v-container>。在布局里面有一个带有页面名称的</v-container></v-layout>

# 标签。

下面是`Menu.vue`文件:

```
<template>    <v-container fluid>        <v-layout>            <h1>Menu Page</h1>        </v-layout>    </v-container></template>
```

```
<script>export default {    name: 'Menu'};</script>
```

```
<style scoped></style>
```

下面是`signin.vue`文件:

```
<template>    <v-container fluid>        <v-layout>            <h1>Signin Page</h1>        </v-layout>    </v-container></template>
```

```
<script>export default {    name: 'Signin'};</script>
```

```
<style scoped></style>
```

下面是`Join.vue`文件:

```
<template>    <v-container fluid>        <v-layout>            <h1>Join Page</h1>        </v-layout>    </v-container></template>
```

```
<script>export default {    name: 'Join'};</script>
```

```
<style scoped></style>
```

#### 使菜单项可点击

在我们的<v-toolbar>菜单中，用户可以点击四个项目。它们是:</v-toolbar>

*   菜单
*   轮廓
*   签到
*   加入

我们想配置这些，以便当用户点击它们时，它会把它们带到适当的页面。打开 AppNavigation.vue 文件。在<v-toolbar>部分找到菜单的<v-btn>。我们所需要的就是="/menu "。我们将对所有四个条目都这样做，但是要确保我们在 router.js 文件中指定了正确的路由。</v-btn></v-toolbar>

我们没有返回主页的菜单选项。我们可以通过将应用程序名称重定向到主页来解决这个问题。但是标题不是按钮，加`to="/menu"`不行。Vue 路由器提供了用`<router-link to=”`/>包围链接的选项。我们将为我们的应用程序标题这样做。

下面是我的 AppNavigation 现在的样子:

```
<template>    <span>        <v-navigation-drawer app v-model="drawer" class="brown lighten-2" dark disable-resize-watcher>            <v-list>                <template v-for="(item, index) in items">                    <v-list-tile :key="index">                        <v-list-tile-content>                            {{item.title}}                        </v-list-tile-content>                    <;/v-list-tile>                    <v-divider :key="`divider-${index}`"></v-divider>                </template>            </v-list>        </v-navigation-drawer>        <v-toolbar app color="brown darken-4" dark>            <v-toolbar-side-icon class="hidden-md-and-up" @click="drawer = !drawer"></v-toolbar-side-icon>            <v-spacer class="hidden-md-and-up"></v-spacer>            <router-link to="/"&gt;                <v-toolbar-title to="/">{{appTitle}}</v-toolbar-title>;            </router-link>            <v-btn flat class="hidden-sm-and-down" to="/menu">Menu</v-btn>            <v-spacer class="hidden-sm-and-down"></v-spacer>            <v-btn flat class="hidden-sm-and-down" to="/sign-in">SIGN IN</v-btn>            <v-btn color="brown lighten-3" class="hidden-sm-and-down" to="/join">JOIN</v-btn>        </v-toolbar>    </span></template>
```

```
<script>export default {    name: 'AppNavigation',    data() {        return {            appTitle: 'Meal Prep',            drawer: false,            items: [                { title: 'Menu' },                { title: 'Profile' },                 { title: 'Sign In' },                { title: 'Join' }            ]        };    }};</script>
```

```
<style scoped></style>
```

当我们这样做时，菜单中的应用程序标题有一点小问题。它从白色文本变成了带下划线的蓝色文本。这是锚标记的默认样式。我们可以通过添加以下样式来解决这个问题:

```
a {    color: white;    text-decoration: none;}
```

现在我们又回到了从前。如果您点击菜单上的所有项目，它们会将您重定向到相应的页面。我们对 About.vue 文件只有一点小问题。这个文件以不同的方式显示内容。为了保持一致性，请将 About.vue 文件更新为:

```
<template>    <v-container fluid>        <v-layout>            <h1>About Page</h1>        </v-layout>    </v-container></template>
```

```
<script>export default {    name: 'About'};</script>
```

```
<style scoped></style>
```

### 获取代码

尽管这是一个由 4 部分组成的系列，但你可以在我的 GitHub 帐户中获得[完成的代码。](https://github.com/ratracegrad/meal-prep)请帮帮我，**拿到代码后开始回购**。

### 摘要

在本系列的这一部分中，您已经学习了:

*   Vue 路由器如何工作
*   如何加载新路线
*   如何设置菜单以加载每个页面

### 下一步是什么

在本系列的下一部分，我们将介绍使用 Firebase 进行身份验证。Vuex 允许您在应用程序中提供“状态”。我们将注册一个配方 API。从这个 API 中，我们将获得菜单页面显示给用户的食谱。

如果你喜欢这篇文章，请为它鼓掌。如果你认为其他人会从这篇文章中受益，请与他们分享。

如果您有任何问题或发现代码有任何错误，请留下评论。如果你有其他话题想让我写，请留下评论。

#### 更多文章

这里有一些我写的其他文章，你可能想看看。

想要一份科技行业的工作？以下是求职者如何利用顶级在线市场获得工作的方法。
[*LinkedIn 是世界上最大的人才库，拥有 300 万个活跃的职位列表。让我向您展示如何利用…*medium.freecodecamp.org](https://medium.freecodecamp.org/want-a-job-in-tech-here-is-how-to-use-the-top-online-marketplace-for-job-seekers-to-get-that-job-878391456a2)[**JavaScript 中的实例化模式**](https://medium.com/dailyjs/instantiation-patterns-in-javascript-8fdcf69e8f9b)
[*实例化模式是在 JavaScript 中创建东西的方法。JavaScript 提供了四种不同的方法来创建…*medium.com](https://medium.com/dailyjs/instantiation-patterns-in-javascript-8fdcf69e8f9b)[**为开源做贡献并不难:我为 Node.js 项目做贡献的旅程**](https://medium.freecodecamp.org/contributing-to-open-source-is-not-hard-here-is-my-journey-to-contributing-to-nodejs-d10760e31194)
[*作为一名开发者，你应该考虑为开源软件做贡献。你的许多潜在雇主会看着……*medium.freecodecamp.org](https://medium.freecodecamp.org/contributing-to-open-source-is-not-hard-here-is-my-journey-to-contributing-to-nodejs-d10760e31194)

[**在 Twitter 上关注我！**](https://twitter.com/ratracegrad)