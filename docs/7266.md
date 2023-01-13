# 如何在 JavaScript 应用程序中处理特定于环境的设置

> 原文：<https://www.freecodecamp.org/news/environment-settings-in-javascript-apps-c5f9744282b6/>

今天，许多 web 应用程序都是使用 React、Angular、Vue、Ember 等构建的。这些现代客户端呈现的应用程序通常调用托管在独立服务器上的 web APIs。这就产生了一个问题:如何配置你的应用程序在每个环境中调用正确的 API URL？

例如，在开发过程中，您可以在 localhost:3000 本地托管 API。在生产中，API 可能被托管在 api.mycompany.com 的其他服务器上。所以你需要你的应用程序在开发中调用 localhost:3000，在生产中调用 api.mycompany.com。但是怎么做呢？

并且基本 URL 仅仅是可以根据环境改变的设置的一个例子。出于性能、安全性或日志记录的目的，您可以选择调整每个环境的其他设置。下面的一些方法也适用于这些一般的特定于环境的配置。但是为了简单起见，**，**这篇文章关注的是为每个环境配置基本 URL 的技术。

我在 Twitter 上发布了一个投票，有几个选项:

> ？Poll:你是如何配置你的客户端渲染应用程序在每个环境中调用不同的 API URLs 的？
> 
> 举例:
> Dev api 运行在本地主机:3002
> Prod api 在 https://t.co/8ZSpUMQi4m
> 
> — Cory House (@housecor) [March 14, 2018](https://twitter.com/housecor/status/973881714710908928?ref_src=twsrc%5Etfw)