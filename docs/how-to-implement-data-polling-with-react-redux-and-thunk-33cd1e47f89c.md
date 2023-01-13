# 如何用 React、Redux 和 Thunk 实现数据轮询

> 原文：<https://www.freecodecamp.org/news/how-to-implement-data-polling-with-react-redux-and-thunk-33cd1e47f89c/>

瓦莱里·捷列先科

# 如何用 React、Redux 和 Thunk 实现数据轮询

![W54LHib-nPdzjQl-LNRTwfgZ61W9oXPv-K8R](img/00610dbdb1bb1ff42b11f14156b82d42.png)

Photo by [rawpixel](https://unsplash.com/@rawpixel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

### 介绍

在我之前的文章[如何用 Redux-Thunk、Redux-Saga、悬念&钩子](https://medium.freecodecamp.org/loading-data-in-react-redux-thunk-redux-saga-suspense-hooks-666b21da1569)加载数据中，我比较了从 API 加载数据的不同方式。在 web 应用程序中，数据经常需要更新，以便向用户显示相关信息。短轮询是实现这一点的方法之一。查看[这篇](https://codeburst.io/polling-vs-sse-vs-websocket-how-to-choose-the-right-one-1859e4e13bd9)文章了解更多详情…