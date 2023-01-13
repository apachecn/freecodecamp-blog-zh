# CSRF 保护问题以及如何修复它

> 原文：<https://www.freecodecamp.org/news/csrf-protection-problem-and-how-to-fix-it/>

一天，我在工作中制作一个专题。我在 JIRA 票证中创建了许多分支，所以我想在不同的选项卡中一次打开一堆 PRs(拉式请求)。

这是我通常的工作方式——我打开了很多标签页，这加快了速度，因为我不需要等待下一页的加载。

但是当我在 BitBucket 中创建了第一个 PR 并试图进入下一个页面时，我收到了一条关于无效 CSRF 令牌的错误消息。这是具有 CSRF 保护的 web 应用程序的常见问题。

因此，在本文中，您将了解什么是 CSRF 以及如何修复这个错误。

## 目录:

*   什么是 CSRF？
*   [标准 CSRF 保护](#standard-csrf-protection)
*   [代币的问题](#the-problem-with-tokens)
*   [跨选项卡通信解决方案](#cross-tab-communication-solution)
    *   [Sysend 库](#sysend-library)
    *   [广播频道](#broadcast-channel)
*   [结论](#conclusion)

## 什么是 CSRF？

CSRF 是**跨站请求伪造**的首字母缩写。它是攻击者通常用来进入您系统的攻击媒介。

通常防止 CSRF 的方法是发送由每个 HTTP 请求生成的唯一令牌。如果服务器上的令牌与请求中的令牌不匹配，您会向用户显示一个错误。

## 标准 CSRF 保护

这是使用令牌抵御 CSRF 的一种方法:

```
const inital_token = '...';

const secure_fetch = (token => {
    const CSRF_HEADER = 'X-CSRF-TOKEN';
    return (url) => {
        const response = await fetch(url, {
            method: 'POST',
            headers: {
              [CSRF_HEADER]: token
            }
        });
        response.then(res => {
           token = res.headers[CSRF_HEADER]
        });
        return response;
    };
})(inital_token); 
```

这段代码使用[获取 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 来发送和接收 [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) 报头中的安全令牌。在后台，您应该在页面加载时生成第一个初始令牌。在服务器上，对于每个 [AJAX](https://en.wikipedia.org/wiki/Ajax_(programming)) 请求，您应该检查令牌是否有效。

## 代币的问题是

除非您打开了不止一个标签，否则这样做很好。每个选项卡都可以向服务器发送请求，这将打破这个解决方案。高级用户可能无法按照他们想要的方式使用您的应用程序。

但是有一个简单的方法可以解决这个问题，那就是跨标签通信。

## 跨选项卡通信解决方案

### Sysend 库

你可以使用 [Sysend 库](https://github.com/jcubic/sysend.js)，这是我专门为此目的创建的一个开源解决方案。它简化了跨选项卡的通信。

如果您愿意，可以使用类似于[广播频道](https://developer.mozilla.org/en-US/docs/Web/API/BroadcastChannel)的原生浏览器 API 来做同样的事情。本文稍后将详细介绍如何做到这一点。

但是 **Sysend** 库将为不支持广播频道的浏览器工作。在 IE 中也可以用(它有一些 bug，这并不意外)。您可能还需要支持一些旧的移动浏览器。它还有一个更简单的 API。

这是一个最简单的例子:

```
let token;
sysend.on('token', new_token => {
    token = new_token;
});

// ...

sysend.broadcast('token', token);
```

Simple example of using base function of sysend library

这就是你如何使用这个库来修复 CSRF 保护:

```
const inital_token = '...';

const secure_fetch = (token => {
    const CSRF_HEADER = 'X-CSRF-TOKEN';
    const EVENT_NAME = 'csrf';
    sysend.on(EVENT_NAME, new_token => {
        // get new toke from different tab
        token = new_token;
    });
    return (url) => {
        const response = await fetch(url, {
            method: 'POST',
            headers: {
              [CSRF_HEADER]: token
            }
        });
        response.then(res => {
           token = res.headers[CSRF_HEADER];
           // send new toke to other tabs
           sysend.broadcast(EVENT_NAME, token); 
        });
        return response;
    };
})(inital_token); 
```

secure_fetch function with CSRF protection using sysend

在发送请求时，您只需从其他选项卡发送和接收一条消息。你的 CSRF 保护应用程序将在许多标签上工作。

仅此而已。这将允许高级用户在他们想要打开许多标签时使用你的具有 CSRF 保护的应用。

### 广播信道

下面是使用广播频道的最简单的例子:

```
const channel = new BroadcastChannel('my-connection');
channel.addEventListener('message', (e) => {
    console.log(e.data); // 'some message'
});
channel.postMessage('some message');
```

Basic usage of Broadcast Channel

因此，使用这个简单的 API，您可以做我们之前做过的事情:

```
const inital_token = '...';

const secure_fetch = (token => {
    const CSRF_HEADER = 'X-CSRF-TOKEN';
    const channel = new BroadcastChannel('csrf-protection');
    channel.addEventListener('message', (e) => {
        // get new toke from different tab
    	token = e.data;
    });
    return (url) => {
        const response = await fetch(url, {
            method: 'POST',
            headers: {
              [CSRF_HEADER]: token
            }
        });
        response.then(res => {
           token = res.headers[CSRF_HEADER];
           // send new token to other tabs
           channel.postMessage(token);
        });
        return response;
    };
})(inital_token);
```

secure_fetch function with CSRF protection using BroadcastChannel

从上面的例子可以看出，广播频道没有任何事件的名称空间。因此，如果您想要发送多种类型的事件，您需要创建事件类型。

这是一个使用广播信道做的比我们到目前为止讨论的 CSRF 保护修复更多的例子。

您可以同步应用程序的登录和注销。如果您登录到一个选项卡，您的其他选项卡也会让您登录。同理，可以在一些电商网站同步购物车。

```
const channel = new BroadcastChannel('my-connection');
const CSRF = 'app/csrf';
const LOGIN = 'app/login';
const LOGOUT = 'app/logout';
let token;
channel.addEventListener('message', (e) => {
    switch (e.data.type) {
        case CSRF:
            token = e.data.payload;
            break;
        case LOGIN:
            const { user } = e.data.payload;
            autologin(user);
            break;
        case LOGOUT:
            logout();
            break;
    }
});

channel.postMessage({type: 'login', payload: { user } });
```

Using Broadcast Channel with different type of messages

## 结论

如果你能保护你的应用免受攻击者的攻击，那就太好了。但是也要记住人们将如何使用你的应用程序，这样你就不会让它变得不必要的难用。这不仅适用于这个特殊的问题。

**Sysend** 库是在同一个浏览器中打开的选项卡之间进行通信的一种简单方式。它可以解决 CSRF 保护的主要问题。这个库有更多的特性，你可以查看它的 [GitHub repo](https://github.com/jcubic/sysend.js) 了解更多细节。

**广播频道**也没那么复杂。如果不需要支持老浏览器或者一些老的移动设备，可以使用这个 API。但是如果你需要支持旧的浏览器，或者想让你的代码更简单，你可以使用 sysend 库。

如果想看浏览器对广播频道的支持，可以看[我能不能用](https://caniuse.com/broadcastchannel)。