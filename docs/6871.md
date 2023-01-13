# 了解如何使用 Passport.js 处理 Node 的身份验证

> 原文：<https://www.freecodecamp.org/news/learn-how-to-handle-authentication-with-node-using-passport-js-4a56ed18e81e/>

作者安东尼奥·埃尔德尔雅克

# 了解如何使用 Passport.js 处理 Node 的身份验证

![1*OUk_mC8ojHhStMEURjbI8g](img/71c26492de560d6bdbab71f16f6ae4ee.png)

Photo by [Oskar Yildiz](https://unsplash.com/photos/cOkpTiJMGzA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/password-programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

支持我从它的原出处读起: [**原出处**](https://www.signet.hr/learn-how-to-handle-authentication-with-node-using-passport-js/)

在本文中，您将学习如何使用 **Passport.js.** 为您的节点服务器处理**认证**。本文**不涉及前端认证。**用这个来配置你的**后端认证**(为每个用户生成令牌&保护路由)。

记住**如果你在任何一步卡住了，可以参考这个 [GitHub repo](https://github.com/AntonioErdeljac/passport-tutorial)** 。

### 在这篇文章中，我将教你以下几点:

*   处理受保护的路线
*   处理 JWT 代币
*   处理未经授权的响应
*   创建基本 API
*   创建模型和模式

### 介绍

#### Passport.js 是什么？

Passport 是 [Node.js](https://nodejs.org/) 的认证中间件。由于它非常灵活和模块化，Passport 可以不引人注目地放入任何基于 [Express](https://expressjs.com/) 的 web 应用程序中。一套全面的策略支持使用[用户名和密码](http://www.passportjs.org/docs/username-password/)、[、](http://www.passportjs.org/docs/facebook/)、 [Twitter](http://www.passportjs.org/docs/twitter/) 以及 [more](http://www.passportjs.org/packages/) 进行身份验证。点击了解更多关于护照的信息[。](http://www.passportjs.org/)

### 辅导的

#### 从头开始创建我们的节点服务器

用这个“app . js”**文件创建一个新目录:**