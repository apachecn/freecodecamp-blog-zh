# 如何使用 React 头盔——使用案例示例

> 原文：<https://www.freecodecamp.org/news/react-helmet-examples/>

由于单页面应用程序(spa)的性质，在不使用助手库的情况下修改 React 应用程序中的元数据可能会很棘手。幸运的是，这个库已经存在了——它叫做“反应头盔”。

利用头盔包含元数据可以大大简化 React 应用程序 SEO 和社交媒体友好的过程。

头盔让我们可以像使用标准 HTML 语法一样将元数据插入到标签中。

在本文中，我们将介绍以下步骤:

1.  如何安装导入 React 头盔库？
2.  客户端和服务器端渲染的基本用法(CSR 与 SSR)。
3.  更先进的使用头盔设置一个搜索引擎优化组件。

为了理解这些主题，您应该具备 React 库的基础知识。

## React 头盔安装和设置

如果你已经熟悉使用 React 和 Node，安装头盔应该是轻而易举的事。

然而，在演示之前，重要的是要注意标准的 **react-helmet** 库现在被认为是过时的。相反，你应该使用**react-头盔-异步**。

这是因为 react-helmet 导致了一些错误，导致了内存泄漏和数据完整性差。可以说，当 React 开发者提到头盔时，他们几乎总是指的是 **react-helmet-async** 。

现在开始安装。只需在终端中导航到您的项目目录，并使用您选择的包管理器安装 react-helmet-async。下面是 yarn 和 npm 的语法:

```
yarn add react-helmet-async
npm i react-helmet-async
```

安装完成后，您可以继续导入和使用头盔组件库。

## 反应头盔的基本概念和用法

我们将从 **react-helmet-async** 导入的两个组件被称为**头盔**和**头盔提供者**。

1.  HelmetProvider 将包装整个应用程序组件，以创建上下文并防止内存泄漏。因此，这个组件只需要导入到根 **App** 组件中。
2.  **头盔**将被导入到任何你想要实现 meta 标签的页面组件中。把 **<头盔>** 想象成问题页面的 **<头部>** 标签。

我们将从客户端渲染(CSR)和服务器端渲染(SSR)的基本用法开始。让我们先来看看在一个基本的 CSR 实施中事情是如何运作的:

```
import React from 'react';
import { HelmetProvider } from 'react-helmet-async';
import NavBar from './NavBar';
import Landing from `./Landing;
export default function App() {
return (
<HelmetProvider>
<NavBar />
<Landing />
</HelmetProvider>
)}
```

如你所见，在 **App** 组件中，我们只从 **react-helmet-async** 中导入了`HelmetProvider`组件。很简单。

SSR 的实现非常相似，只是有一点小小的不同。让我们来看一看，看看你是否能发现不同之处:

```
import React from 'react';
import { HelmetProvider } from 'react-helmet-async';
import NavBar from './NavBar';
import Landing from `./Landing;
export default function App() {
const helmetContext = {};
return (
<HelmetProvider context={helmetContext}>
<NavBar />
<Landing />
</HelmetProvider>
)}
```

如果您注意到添加的 **helmetContext** 变量被作为道具传递给我们的 **HelmetProvider** ，您就发现了它！

使用最流行的状态管理系统(如 Redux)可以找到这种范式，它有助于确保上下文不会超出应用程序当前实例的范围。

现在，让我们假设以下页面组件是您的 React 应用程序的登录页面:

```
import React from 'react';
import { Helmet } from 'react-helmet-async';
export default function Landing() {
return (
<div>
<Helmet>
<title>Learning React Helmet!</title>
<meta name='description' content='Beginner friendly page for learning React Helmet.' />
</Helmet>
<h1>Cool Landing Page!</h1>
</div>
)
}
```

快速浏览一下登录页面组件，可以看到我们导入了**头盔**组件，并使用它向页面添加了*标题*和*描述*元数据。

我们只需在头盔组件中添加 HTML 等价的 meta 标签，将它添加到 **<头>** HTML 标签的工作由我们来处理。

厉害！我们现在正在创建一个 SEO 友好的 React 应用程序。

## 用 React 头盔创建一个 SEO 组件

元数据不仅仅与谷歌搜索结果有关。我们还希望引用我们网站的社交媒体帖子显示为很酷的预览卡。

说到元数据和元标签，有大量不同的变体需要记住。脸书使用 **og** (开放图)标签，Twitter 使用自己的 **twitter** 变种，等等。

## 如何使用组件进行抽象

用道具创建 React 组件的一个很酷的地方是，你可以在组件中重用道具。

利用这些知识，您可以创建一个名为 SEO 的组件，该组件抽象出常用元数据标签的用法，使您不必在每次构建 SEO 友好的应用程序时跟踪每个标签变体。

一个简化了添加脸书和 Twitter 标签的 SEO 组件示例如下所示:

```
import React from 'react';
import { Helmet } from 'react-helmet-async';
export default function SEO({title, description, name, type}) {
return (
<Helmet>
{ /* Standard metadata tags */ }
<title>{title}</title>
<meta name='description' content={description} />
{ /* End standard metadata tags */ }
{ /* Facebook tags */ }
<meta property="og:type" content={type} />
<meta property="og:title" content={title} />
<meta property="og:description" content={description} />
{ /* End Facebook tags */ }
{ /* Twitter tags */ }
<meta name="twitter:creator" content={name} />}
<meta name="twitter:card" content={type} />
<meta name="twitter:title" content={title} />
<meta name="twitter:description" content={description} />
{ /* End Twitter tags */ }
</Helmet>
)
}
```

如上所示，我们的组件接受四个属性:标题、描述、名称和类型。使用这四个道具，我们能够在九个不同类型的元标签上分配值！

下面是我们如何在**登录**页面组件中实现该组件的示例:

```
import React from 'react';
import { SEO } from './SEO’;
export default function Landing() {
return (
<div>
<SEO
title=’Learning React Helmet!’
description=’Beginner friendly page for learning React Helmet.'
name=’Company name.’
type=’article’ />
<h1>Cool Landing Page!</h1>
</div>
)
}
```

因此，我们所要做的就是传入我们的四个道具，我们的定制 SEO 组件处理创建多种不同类型的元数据标签的所有繁重工作。很好。

这个例子远不是元标签的详尽列表。如果你想为你的站点包含所有相关的 meta 标签，想象一下这个组件会有多有用并不需要太多的想象力。

## 结论

在这篇文章中，我们回顾了为什么 React 头盔是 React 应用程序的有用补充。您不仅学习了基本的设置和用法，还学习了更高级的实现，该实现有助于抽象掉元数据标记中涉及的大量重复工作。

希望你现在有足够的信心通过实现 React 头盔异步库来增强你的 React SEO 和社交媒体性能。祝你好运，编码快乐！

更多关于如何建立 JavaScript 网站以获得搜索引擎成功的信息，请查看[ohmycrawl.com](http://ohmycrawl.com/)。