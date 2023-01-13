# 如何保护您的 WebSocket 连接

> 原文：<https://www.freecodecamp.org/news/how-to-secure-your-websocket-connections-d0be0996c556/>

网络正以惊人的速度增长。越来越多的 web 应用是动态的、沉浸式的，并且不需要终端用户更新。出现了对 websockets 等低延迟通信技术的支持。Websockets 允许我们在连接到服务器的不同客户端之间实现实时通信。

许多人不知道如何保护他们的 websockets 免受一些非常常见的攻击。让我们看看它们是什么，你应该做些什么来保护你的网络套接字。

### #0:启用 CORS

WebSocket 没有内置 CORS。话虽如此，但这意味着任何网站都可以连接到任何其他网站的 websocket 连接，并且可以不受任何限制地进行通信！我不打算解释为什么会这样，但是一个快速的解决方法是验证 websocket 握手上的`Origin`头。

当然，攻击者可以伪造 Origin 头，但这没关系，因为攻击者需要在受害者的浏览器上伪造 Origin 头，而现代浏览器不允许 web 浏览器中的普通 javascript 更改 Origin 头。

此外，如果你真的使用 cookies 来认证用户，那么这对你来说并不是一个问题(更多关于第 4 点)

### #1:实施速率限制

限速很重要。没有它，客户端可以有意或无意地在您的服务器上执行 DoS 攻击。DoS 代表拒绝服务。DoS 意味着单个客户机使服务器过于繁忙，以至于服务器无法处理其他客户机。

在大多数情况下，这是攻击者蓄意破坏服务器的企图。有时糟糕的前端实现也会导致普通客户端拒绝服务。

我们将利用漏桶算法(这显然是网络实现的一种非常常见的算法)在我们的 websockets 上实现速率限制。

这个想法是，你有一个桶，它的底部有一个固定大小的孔。你开始往里面放水，水从底部的孔流出。现在，如果你把水放进桶里的速率，大于长时间从洞里流出的速率，在某个时刻，桶会变满，开始漏水。仅此而已。

现在让我们了解它与我们的 websocket 的关系:

1.  水是用户发送的 websocket 流量。
2.  水从洞里流下。这意味着服务器成功地处理了特定的 websocket 请求。
3.  还在桶里没有溢出来的水，基本上就是待定流量。服务器稍后将处理该流量。这也可能是突发流量(即，只要桶不泄漏，在非常短的时间内有太多的流量是可以的)
4.  溢出的水是服务器丢弃的流量(来自单个用户的流量太多)

这里的要点是，您必须检查您的 websocket 活动并确定这些数字。您将为每个用户分配一个存储桶。我们根据你的漏洞有多大(你的服务器处理一个 websocket 请求平均需要多少时间，比如把一个用户发送的消息保存到一个数据库中)来决定桶应该有多大(单个用户在一段固定时间内可以发送的流量)。

这是一个精简的实现，我在 [codedamn](https://codedamn.com) 使用它来实现 websockets 的漏桶算法。它在 NodeJS 中，但概念保持不变。

```
if(this.limitCounter >= Socket.limit) {
  if(this.burstCounter >= Socket.burst) {
     return 'Bucket is leaking'
  }
  ++this.burstCounter
  return setTimeout(() => {
  this.verify(callingMethod, ...args)
  setTimeout(_ => --this.burstCounter, Socket.burstTime)
  }, Socket.burstDelay)
}
++this.limitCounter
```

这里发生了什么？基本上，如果超过了限制和突发限制(这是常量设置)，websocket 连接就会断开。否则，在特定的延迟之后，我们将重置突发计数器。这又为另一次爆发留下了空间。

### #2:限制有效载荷大小

这应该作为服务器端 websocket 库中的一个特性来实现。如果没有，是时候换一个更好的了！您应该限制可以通过 websocket 发送的消息的最大长度。理论上没有限制。当然，获得巨大的有效负载很可能会挂起特定的套接字实例，并消耗掉比所需更多的系统资源。

例如，如果您使用 WS library for Node 在服务器上创建 websockets，您可以使用 [maxPayload 选项](https://github.com/websockets/ws/blob/master/doc/ws.md#new-websocketserveroptions-callback)来指定最大有效负载大小(以字节为单位)。如果有效负载大于这个值，库将自然地断开连接。

不要试图通过确定消息长度来自己实现这一点。我们不想先将整个消息读入系统 RAM。如果它比我们设定的限制大一个字节，就丢弃它。这只能由库来实现(库将消息作为字节流而不是固定字符串来处理)。

### #3:创建可靠的沟通协议

因为现在你在一个双工连接上，你可以向服务器发送任何东西。服务器可以将任何文本发送回客户端。你需要有一种方法在两者之间进行有效的交流。

如果您想要扩展网站的消息传递功能，就不能发送原始消息。我更喜欢使用 JSON，但是也有其他优化的方法来建立通信。然而，考虑到 JSON，下面是一般站点的基本消息传递模式:

```
Client to Server (or vice versa): { status: "ok"|"error", event: EVENT_NAME, data: <any arbitrary data> }
```

现在，在服务器上检查有效的事件和格式更加容易了。如果消息格式不同，请立即断开连接并记录用户的 IP 地址。除非有人手动激活 websocket 连接，否则格式不可能改变。如果您在 node 上，我推荐使用 [Joi 库](https://github.com/hapijs/joi)来进一步验证来自用户的输入数据。

### #4:在 WS 连接建立之前验证用户

如果您为经过身份验证的用户使用 websockets，那么只允许经过身份验证的用户建立成功的 websocket 连接是一个不错的主意。不要允许任何人建立连接，然后等待他们通过 websocket 本身进行身份验证。首先，建立一个 websocket 连接无论如何都有点贵。所以你不希望未经授权的人跳上你的网络插座，霸占可以被其他人使用的连接。

为此，当您在前端建立连接时，向 websocket 传递一些身份验证数据。可以是 X-Auth-Token: <some token="" assigned="" to="" this="" client="" on="" login="">这样的头。默认情况下，cookies 无论如何都会被传递。</some>

再一次，它实际上归结为你在服务器上用来实现 websockets 的库。但是如果你在 Node 上使用 WS，有这个 [verifyClient](https://github.com/websockets/ws/blob/master/doc/ws.md#new-websocketserveroptions-callback) 函数可以让你访问传递给 websocket 连接的 info 对象。(就像您有权访问 HTTP 请求请求对象一样。)

### #5:在 websockets 上使用 SSL

这是显而易见的，但还是要说。使用 wss://而不是 ws://。这为您的通信增加了一个安全层。使用类似 Nginx 的服务器来反向代理 websockets，并在其上启用 SSL。设置 Nginx 将是另一个完整的教程。我将把您需要用于 Nginx 的指令留给那些熟悉它的人。[更多信息请点击此处](http://nginx.org/en/docs/http/websocket.html)。

```
location /your-websocket-location/ {
    proxy_pass ​http://127.0.0.1:1337;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
}
```

这里假设您的 websocket 服务器正在监听端口 1337，并且您的用户以这种方式连接到您的 websocket:

```
const ws = new WebSocket('wss://yoursite.com/your-websocket-location')
```

### 有问题吗？

有什么问题或建议吗？问吧！