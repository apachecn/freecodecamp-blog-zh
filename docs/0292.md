# 如何为单页应用程序配置元数据

> 原文：<https://www.freecodecamp.org/news/configure-metadata-in-single-page-applications/>

## 为什么元数据很重要

元数据是任何现代 web 应用程序不可或缺的一部分，因为它与搜索引擎优化(SEO)有着内在的联系。

搜索引擎和它们各自的结果页面(SERPS)依靠元数据来正确地索引和显示每个站点的相关信息。

此外，meta 标签还可以在给定的社交媒体平台上正确显示您网站的内容，例如待售的文章或商品。

因此，了解元数据在现代 web 应用程序中是如何配置的至关重要。

单页应用程序(SPA)是一个非常流行的现代 web 应用程序实现。今天的大多数框架都以某种方式利用了它。在当今最流行的 SPA 框架中配置元数据将是本教程的重点。

## 单页应用程序和元数据

SPAs 的性质使得配置元数据的过程不如传统的多页面应用程序简单。我将尝试通过描述以下关键概念来阐明这个主题:

1.  温泉的结构。
2.  在 SPA 中修改元数据的问题。
3.  可用的元数据解决方案可能使用三种最流行的 SPA 框架:React、Svelte 和 Vue。

您应该对 HTML、元数据和三个 SPA 框架中的一个有基本的了解，以便理解我们将要讨论的概念。但是，我会保持初学者友好，所以不要担心！

## 单页应用程序如何工作

在潜水之前，你需要牢牢掌握什么是温泉。

顾名思义，单页面应用程序实际上由服务器发送的单个 HTML 页面组成。这个页面只是一个空的 HTML 外壳，看起来像这样:

```
<!DOCTYPE html>
	<html>
		<head>
		<title>Home | Demystifying SPA Metadata</title>
		<meta name="description" content="How to configure popular SPA 			frameworks to maintain quality site metadata."/>
		<link rel="stylesheet" href="./stylesheet.css" type="text/css" 			/>
		</head>
		<body>
			<script src="/bundle.min.js" type="text/javascript">					</script>
		</body>
	</html>
```

您可能想知道整个网站是如何从这个空的 HTML 外壳中派生出来的。

这是可能的，因为伴随 HTML 页面的将是为每个页面生成内容的大量客户端 JavaScript 代码。这段代码通过

## 在 SPA 中配置元数据的挑战

在前一节中，看一下 head 标记中的 HTML。以“meta”开头的各种标签是我们的元数据，还有 title 标签。

这并不是对元标记的详尽描述，因为还有更多的元标记被广泛使用。但是标题和描述对本教程来说很有用。

title 标签是非常重要的元数据，应该反映浏览器中当前页面的相关标题。现在，它非常适合主页。但是当用户导航到不同的页面时会发生什么呢？

元数据需要相应地改变，SPA 框架不会神奇地做到这一点。

您不能更改原始 HTML，因为每个页面都使用相同的 shell，因此会为每个页面反映相同的元数据。所以你需要一个聪明的编码策略。

## 用于元数据维护的 SPA 插件

SPA 框架非常注重将 HTML 注入 DOM，以便在屏幕上呈现内容。这意味着更新 body 标签是框架的中心焦点。因此，更新 head 标签往往是一个被忽略的特性。

对于 React 等许多 SPA 框架，开发人员社区已经收拾了残局，创建了简化元数据处理过程的库。

这将是本文剩余部分的重点——元数据库及其在最流行的 SPA 框架中的使用。

### 修改元数据标签的基本 JavaScript 代码

在深入这些元数据库之前，理解这一点至关重要:归根结底，它只是代码。因此，让我们来看一个可以修改标题标签和元描述的 JavaScript 代码的基本示例:

```
document.getElementsByTagName('meta')["description"].content = "New meta description!";

document.title = "New Title!";
```

除了这个基本代码示例之外，下面的库还会做很多额外的工作，但是揭开帷幕，看看真正发生的事情通常是非常简单的，这总是好的。

## react-Helmet–如何在 ReactJs 中配置元数据

React 是一个基于组件的库，用于构建可伸缩的 spa。它拥有各种优秀的特性，开发者可以利用这些特性来构建高性能的应用。元数据维护不在其中。

幸运的是，React 社区的开发人员推出了 react-helmet，这是一个组件库，它极大地简化了在标签中修改元数据的过程。

react-头盔现在被弃用，取而代之的是更强大的 react-头盔异步。我们不会深究其中的原因，只需知道当这些天提到 react-头盔时，大多数团队和开发人员实际上都在使用 react-头盔-异步。

下面是 react-helmet-async 代码的一个基本示例:

```
import React from 'react';
import ReactDOM from 'react-dom';
import { Helmet, HelmetProvider } from 'react-helmet-async';

const app = (
	<HelmetProvider>
	<App>
	<Helmet>
	<title>Home | Demystifying SPA Metadata</title>
	<meta name="description" content="How to configure popular SPA 			frameworks to maintain quality site metadata."/>
	</Helmet>
	<h1>Hello World</h1>
	</App>
	</HelmetProvider>
);
```

如您所见，实现非常简单。采取以下步骤:

1.  用 npm 或 yarn 安装 react-helmet-async。
2.  从 react-helmet-async 导入头盔和头盔提供者
3.  将整个应用程序包装在 HelmetProvider 组件中。
4.  在头盔组件中使用标准的 HTML 元标记。

完成这些步骤后，您现在可以在 React 应用程序的任何组件中使用头盔组件。

头盔是为了让事情变得简单。根据文件:

> “头盔采用普通 HTML 标签，输出普通 HTML 标签。这非常简单，对初学者也很友好。”

## 纤体元标签–如何在纤体中配置元数据

Svelte 是温泉区的一个新生事物，它正迅速获得大量的关注。简单来说就是用它的人爱它。用 Svelte 修改元数据是通过 svelte-meta-tags 组件库处理的。

有了这样一个向上移动的轨迹，熟悉如何在一个苗条的 SPA 中处理元数据是很重要的。

Svelte 是另一个声明性框架，它允许你直接在应用程序中编写“HTML like”代码，从而抽象了许多繁重的工作。

没有进入什么使苗条与众不同，为什么它是一个有趣的前景(值得一看！)，让我们深入研究一下元数据维护的相关代码:

```
<script>
import { MetaTags } from 'svelte-meta-tags';
</script>
<h1>
	Metadata in Svelte
</h1>
<MetaTags
	title='Home | Demystifying SPA Metadata'
	description='How to configure popular SPA frameworks to maintain 		quality site metadata.'
/>
```

使用超薄元标签的步骤如下:

1.  用 npm 或纱线安装细长的金属标签
2.  导入元标记组件。
3.  将每个所需的元数据属性设置为其各自的值。

类似于反应头盔的难度，MetaTags 组件对初学者非常友好，很容易上手。它支持所有现代元数据标签(完整列表，请查看文档)。

## vue-Meta–如何在 Vue.js 中配置元数据

Vue 比 Svelte 存在的时间稍长，但仍然比 React 年轻一岁左右。在过去的几年里，Vue 经历了一次流行的复苏，这也是我选择它作为三大 SPA 框架之一来回顾的原因。

像前两个框架一样，Vue 是声明性的和基于组件的。但是实现插件库略有不同。让我们来看看。

Vue 利用一个名为 **main.js** 的配置文件来初始化 Vue 应用。因为我们将在整个应用程序中使用 vue-meta 插件，所以这是我们想要导入插件的地方。

在 Vue 中，你用`Vue.use()`方法来做这件事，看起来会像这样:

```
Main.js
import Vue from 'vue';
import VueMeta from 'vue-meta';
import App from './App.vue';

Vue.use(VueMeta);
	new Vue({
	el: '#app',
	render: h => h(App)
});
```

现在我们已经导入了 VueMeta 组件，我们可以通过从任何 Vue 组件中导出一个名为 **metaInfo** 的属性来发送一些数据给它。

下面是一个使用 view-meta 插件的 Vue 登陆组件的例子:

```
Landing.vue
<template>
	<div>SPA Metadata</div>
</template>
<script>
	export default {
		name: 'landing',
		data () {
		return {}
		},
		metaInfo: {
		title: 'Home | Demystifying SPA Metadata',
		description: 'How to configure popular SPA frameworks to 				maintain quality site metadata.'
		}
		}
</script>
```

每个 Vue 组件导出一个对象供 Vue 应用程序使用，由于我们在 **main.js** 文件中导入了 VueMeta 插件，Vue 将寻找我们从 **landing** 组件中导出的 metaInfo 属性。

我们所要做的就是给它一些数据，我们的元标签就为我们生成了！

## SPA SEO 和元数据——总结

对于这三个 SPA 框架中的每一个，我们在本教程开始时看到了一些代码示例，它们将导致在 HTML 标记中找到元数据标记。

在 SPA 应用程序中修改元数据不像在多页面应用程序中那样简单，即使是动态的应用程序也是如此。理解这一概念将使生活更加轻松，同时利用当今可用的各种 SPA 框架。

元数据是任何现代 web 应用程序不可或缺的一部分。希望在读完这篇教程后，你可以自信地将这个概念应用到你的下一个 SPA 构建中。

有兴趣了解更多关于单页面应用程序元数据和搜索引擎优化的信息吗？阅读我们完整的 [SEO for SPA](https://www.ohmycrawl.com/spa-seo/) 指南，提升并充分理解它是如何工作的。