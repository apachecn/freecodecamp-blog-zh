# 你应该在 2021 年建立的 7 个 React 项目

> 原文：<https://www.freecodecamp.org/news/react-projects-you-should-build-this-year/>

React 是一个 JavaScript 库，非常适合创建令人印象深刻的应用程序。你可以用 React 做无数个项目，但这里有七个项目在我的清单上，它们将在 2021 年建成。

为什么我特别选择了这七个项目？我选择它们是因为它们相互依存。它们要求您了解类似的基本概念，如身份验证、使用 API 和数据库、使用 React 路由器将页面添加到您的应用程序，以及播放音频或视频等媒体。

此外，许多应用程序可以(并且经常)相互集成。社交媒体应用可以包括聊天应用，音乐或视频应用可以包括电子商务应用，等等。

换句话说，**构建这些项目中的任何一个**都会给你所需的技能和知识来构建列表中的其余应用，包括你自己的个人项目。

除了每个项目，我还提供了几个真实世界的例子，你可以用它们来寻找灵感，以及一些关于我可能会用什么工具来构建每个应用程序的想法。

> 如果你想了解如何为自己构建这些应用，[请查看我的课程系列](http://bit.ly/react-projects)，在那里你将学习如何在每个月底创建一个令人印象深刻的 React 项目。

## 1.实时聊天应用

**现实世界的例子**:懈怠、信使、不和谐、干脆聊天

几乎所有人都使用某种实时聊天应用，无论是 WhatsApp 或 Viber 这样的移动应用，还是 Slack 或 Discord 这样的生产力工具。它也可以是网站中聊天工具的一部分，客户可以直接与网站所有者交谈。

所有聊天应用程序都允许用户实时向他人发送消息，对消息做出反应，并显示用户何时在线或离线。

### 如何构建实时聊天应用程序:

*   使用 create-react-app 或 Next.js 构建您的项目。
*   使用 Firebase 或 GraphQL 订阅等服务为用户实时创建和获取消息。
*   使用 npm 表情包 emoji-mart 添加对消息的反应
*   使用 Firebase 工具部署到 web

## 2.社交媒体应用

真实世界的例子:脸书、推特、Instagram

你最熟悉的应用可能是社交媒体应用。在许多方面，它类似于一个聊天应用，但扩展到了一个更大的用户社区。

这些用户可以以不同的方式相互交流:他们可以相互关注以接收帖子，添加图像和视频等媒体以与其他人分享，并使用户能够与帖子互动，如对帖子的喜欢或评论。

### 如何构建社交媒体应用程序:

*   用 create-react-app 构建前端，用 Node API 构建后端
*   使用 Postgres 或 MongoDB 这样的数据库，以及 Prisma (Postgres)或 mongose(MongoDB)这样的 ORM
*   使用谷歌、脸书或 Twitter 的社交认证，使用 Passport 或更简单的服务，如 Auth0
*   将后端部署到 Heroku，前端部署到 Netlify

## 3.电子商务应用程序

**现实世界示例:** Shopify、Etsy、开发到店面

我们可以在网上购买数字或实体产品的店面随处可见。电子商务应用增加了用户在购物车中添加和删除商品、查看购物车、使用信用卡结账的功能，以及 Google Pay 和 Apple Pay 等其他支付选项。

如果你正在寻找灵感，去看看一些简单的店面，比如 Shopify 店面，而不是像亚马逊或沃尔玛这样的大型零售商。

### 如何构建电子商务应用程序:

*   使用 create-react-app 或 Next.js 创建应用程序
*   添加`stripe` NPM 套餐，再加上`use-shopping-cart`通过 Stripe Checkout 轻松处理直接付款
*   构建一个节点 API 来处理使用条带创建会话
*   将后端部署到 Heroku，前端部署到 Netlify(或者两者都部署在 Heroku 上)

## 4.视频分享应用

真实世界的例子: YouTube，抖音，Snapchat

视频分享应用可能是最广泛的类别，因为视频在如此多的不同应用中以许多不同的方式使用。

你有像 YouTube 这样的视频分享应用，它允许你搜索任何浏览器，寻找任何你认为是用户创作的视频。此外，tik tok 和 Snapchat 让我们能够观看来自其他用户的视频，这些视频以更短、更容易理解的格式录制，更注重喜欢和观点等互动。

### 如何构建视频分享应用程序:

*   用 create-react-app 创建 app，用 Node/Express 创建后端
*   使用 Cloudinary 将图像和视频上传到 Cloudinary API
*   使用 Postgres 或 MongoDB 这样的数据库，以及 Prisma (Postgres)或 mongose(MongoDB)这样的 ORM
*   将后端部署到 Heroku，前端部署到 Netlify(或者两者都部署在 Heroku 上)

## 5.博客/作品集应用

**现实世界的例子:**中型、开发到、散列节点

这个应用程序示例可能是最实用的。对你来说，建立一个博客或作品集应用程序最直接的实际选择是展示你的技能。它允许你炫耀你作为一个开发者能做什么，同时也允许你包含反映你所知道的内容的帖子和内容。

用 Gatsby 或 Nextjs(都是 React 框架)这样的工具制作这些应用程序现在比以往任何时候都容易。

### 如何构建博客或作品集应用程序:

*   用 Gatsby 或 Next.js 创建应用程序
*   通过一个特殊的 markdown transformer 插件，如`remark`，对博客文章使用 markdown
*   将站点部署到 Netlify 或 Vercel

## 6.论坛 App

真实世界的例子: Reddit，StackOverflow，freeCodeCamp 论坛

当我们想获得帮助时，论坛应用程序是我们去的地方，作为程序员，我们访问像 Reddit 和 Stack Overflow 这样的论坛来获得我们的编码问题的答案。

论坛还通过帖子、评论和反应结合了聊天和社交媒体应用的许多元素。论坛更像是一个更有组织的社交媒体应用程序，用户可以更容易地找到他们正在寻找的问题的答案。

### 如何构建论坛 app:

*   用 create-react-app 构建前端，用 Node API 构建后端
*   使用 Postgres 或 MongoDB 这样的数据库，以及 Prisma (Postgres)或 mongose(MongoDB)这样的 ORM
*   使用谷歌、脸书或 Twitter 的社交认证，使用 Passport 或更简单的服务，如 Auth0
*   将后端部署到 Heroku，前端部署到 Netlify

## 7.音乐流媒体应用

真实世界的例子: Spotify，Soundcloud，Pandora

正如 React 应用程序非常适合提供视频内容一样，它们也非常适合音乐等流媒体。

音乐应用程序与视频分享应用程序的结构类似，可能允许也可能不允许用户上传自己的音乐。它们允许用户听音乐，喜欢歌曲，评论它们，甚至可能购买音乐。

通过这种方式，流媒体音乐应用程序可以结合视频共享应用程序和电子商务应用程序的元素。

### 如何构建音乐流媒体应用程序:

*   用 create-react-app 创建 app，用 Node/Express 创建后端
*   使用 Cloudinary 将图像和视频上传到 Cloudinary API
*   使用 Postgres 或 MongoDB 这样的数据库，以及 Prisma (Postgres)或 mongose(MongoDB)这样的 ORM
*   将后端部署到 Heroku，前端部署到 Netlify(或者两者都部署在 Heroku 上)

## 想用 React 构建像 YouTube、Instagram 和 Twitter 这样的真实应用吗？以下是方法。

每个月底，我都会发布一门独家课程，从头到尾向你展示如何用 React 构建一个完整的应用克隆。

想在下一门课程结束时收到通知吗？ **[在这里加入](http://bit.ly/react-projects)的等候名单。**