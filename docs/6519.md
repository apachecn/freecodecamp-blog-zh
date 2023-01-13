# 网络安全快速入门

> 原文：<https://www.freecodecamp.org/news/a-quick-introduction-to-web-security-f90beaf4dd41/>

#### 一个关于 CORS、CSP、HSTS 和所有网络安全首字母缩略词的网络开发者入门！

了解 web 安全有很多原因，例如:

*   你是一个忧心忡忡的用户，担心你的个人数据被泄露
*   你是一名忧心忡忡的 web 开发人员，想要让他们的 web 应用程序更加安全
*   你是一名 web 开发人员，正在申请工作，如果面试官问你关于 web 安全的问题，你想做好准备

诸如此类。

这篇文章将解释一些常见的网络安全首字母缩略词，以一种容易理解但仍然准确的方式。

在此之前，让我们确保了解一些安全的核心概念。

### 安全的两个核心概念

#### 没有人是百分百安全的。

不存在 100%不受黑客攻击的概念。如果有人这样告诉你，那他们就错了。

#### **一层保护是不够的。**

你不能只说…

> 哦，因为我实施了 CSP，所以我是安全的。我可以从我的漏洞列表中划掉跨站点脚本，因为现在不可能了。

也许对某些人来说这是理所当然的，但是很容易发现自己以这种方式思考。我认为程序员很容易发现自己有这种想法的一个原因是因为太多的编码是非黑即白，0 或 1，真或假。安全不是那么简单的。

我们将从每个人在 web 开发之旅的早期都会遇到的问题开始。然后你在 StackOverflow 上找一堆答案告诉你怎么绕过它。

### 跨产地资源共享(CORS)

您遇到过类似这样的错误吗？

```
No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'null' is therefore not allowed access.
```

你肯定不是一个人。然后你用谷歌搜索，有人告诉你获得这个扩展，它会让你所有的问题消失！

> 很好，对吧？

CORS 是来保护你的，不是来伤害你的！

为了解释 CORS 如何帮助你，让我们先来谈谈 cookie，具体来说就是**认证 cookie**。身份验证 cookies 用于告诉服务器您已登录，并且它们会随着您对该服务器的任何请求自动发送。

假设你登录到脸书，他们使用认证 cookies。你点击`bit.ly/r43nugi`，它会将你重定向到`superevilwebsite.rocks`。`superevilwebsite.rocks`中的一个脚本向发送您的认证 cookie 的`facebook.com`发出一个客户端请求！

在一个没有 CORS 的世界里，他们可以在你不知道的情况下修改你的账户。当然，直到他们在你的时间线上发布`bit.ly/r43nugi`，你所有的朋友都点击它，然后他们在你所有朋友的时间线上发布`bit.ly/r43nugi`，然后这个循环继续在一个邪恶的广度优先的方案中进行，征服了所有的脸书用户，世界被`superevilwebsite.rocks`消耗。？

然而，在 CORS 的世界里，脸书只允许来源为`facebook.com`的请求编辑他们服务器上的数据。换句话说，他们会限制跨来源的资源共享。你可能会问…

> 那么，superevilwebsite.rocks 是否可以根据他们的请求更改 origin 标头，使其看起来像是来自 facebook.com？

他们可以尝试，但不会成功，因为浏览器会忽略它，并使用真正的起源。

> 好吧，但是如果 superevilwebsite.rocks 在服务器端发出请求呢？

在这种情况下，他们可以绕过 CORS，但他们不会赢，因为他们无法发送您的身份验证 cookie。该脚本需要在客户端执行才能访问您的客户端 cookies。

### 内容安全政策(CSP)

为了理解 CSP，我们首先需要谈谈网络上最常见的漏洞之一:XSS，它代表跨站脚本(yay——另一个缩写)。

XSS 就是某个邪恶的人将 JavaScript 注入到你的客户端代码中。你可能会想…

> 他们要做什么？把一种颜色从红色变成蓝色？

让我们假设有人已经成功地将 JavaScript 注入到您正在访问的网站的客户端代码中。

他们会做什么恶意的事？

*   他们可以伪装成您向另一个站点发出 HTTP 请求。
*   他们可以添加一个锚定标签，将你发送到一个看起来与你所在的网站一模一样的网站，但有一些稍微不同的恶意特征。
*   他们可以用内联 JavaScript 添加一个脚本标签。
*   他们可以添加一个脚本标签来获取远程 JavaScript 文件。
*   他们可以添加一个 iframe 来覆盖页面，看起来像网站的一部分，提示您输入密码。

可能性是无限的。

CSP 试图通过限制来防止这种情况发生:

*   iframe 中可以打开什么
*   可以加载哪些样式表
*   可以提出请求的地方等。

那么它是如何工作的呢？

当你点击一个链接或者在浏览器的地址栏中输入一个网址时，你的浏览器会发出一个 GET 请求。它最终到达服务器，服务器提供 HTML 和一些 HTTP 头。如果你想知道你会收到什么样的标题，在你的控制台上打开网络标签，然后访问一些网站。

您可能会看到如下所示的响应标头:

```
content-security-policy: default-src * data: blob:;script-src *.facebook.com *.fbcdn.net *.facebook.net *.google-analytics.com *.virtualearth.net *.google.com 127.0.0.1:* *.spotilocal.com:* 'unsafe-inline' 'unsafe-eval' *.atlassolutions.com blob: data: 'self';style-src data: blob: 'unsafe-inline' *;connect-src *.facebook.com facebook.com *.fbcdn.net *.facebook.net *.spotilocal.com:* wss://*.facebook.com:* https://fb.scanandcleanlocal.com:* *.atlassolutions.com attachment.fbsbx.com ws://localhost:* blob: *.cdninstagram.com 'self' chrome-extension://boadgeojelhgndaghljhdicfkmllpafd chrome-extension://dliochdbjfkdbacpmhlcpmleaejidimm;
```

那就是`facebook.com`的内容安全策略。让我们将它重新格式化，以便于阅读:

```
content-security-policy:
default-src * data: blob:;

script-src *.facebook.com *.fbcdn.net *.facebook.net *.google-analytics.com *.virtualearth.net *.google.com 127.0.0.1:* *.spotilocal.com:* 'unsafe-inline' 'unsafe-eval' *.atlassolutions.com blob: data: 'self';

style-src data: blob: 'unsafe-inline' *;

connect-src *.facebook.com facebook.com *.fbcdn.net *.facebook.net *.spotilocal.com:* wss://*.facebook.com:* https://fb.scanandcleanlocal.com:* *.atlassolutions.com attachment.fbsbx.com ws://localhost:* blob: *.cdninstagram.com 'self' chrome-extension://boadgeojelhgndaghljhdicfkmllpafd chrome-extension://dliochdbjfkdbacpmhlcpmleaejidimm;
```

现在，让我们分解指令。

*   `**default-src**`限制未明确列出的所有其他 CSP 指令。
*   `**script-src**` 限制可以加载的脚本。
*   `**style-src**`限制可以加载的样式表。
*   `**connect-src**`限制可以使用脚本接口加载的 URL，如 fetch、XHR、ajax 等。

请注意，除了上面显示的这四个指令之外，还有更多 CSP 指令。浏览器将读取 CSP 头，并将这些指令应用于所提供的 HTML 文件中的所有内容。如果指令设置得当，它们只允许必要的内容。

如果 CSP 报头不存在，那么一切正常，没有任何限制。无论你在哪里看到`*`，那都是一个通配符。你可以想象用任何东西代替`*`都会被允许。

### HTTPS 或 HTTP 安全

你肯定听说过 HTTPS。也许你听过一些人说…

> 如果我只是在网站上玩游戏，为什么我会关心使用 HTTPS 呢？

或者也许你已经听到了另一面…

> 如果你的网站没有 HTTPS，那你就是疯了。都 2018 年了！不要相信任何不这么说的人。

也许你听说过，如果你的网站不是 HTTPS 的，Chrome 会把它标记为不安全。

本质上，HTTPS 相当简单。HTTPS 是加密的，而 HTTP 不是。

那么，如果您不发送敏感数据，这又有什么关系呢？

准备好接受另一个首字母缩略词…MITM，它代表中间的人。

如果你在咖啡店使用没有密码的公共 Wi-Fi，有人很容易就能充当你的路由器，这样所有的请求和响应都会通过它们。如果你的数据没有加密，他们就可以为所欲为。他们可以在 HTML、CSS 或 JavaScript 到达你的浏览器之前对其进行编辑。根据我们对 XSS 的了解，你可以想象这会有多糟糕。

> 好吧，但是为什么我的电脑和服务器知道如何加密/解密，而这个 MITM 却不知道？

这就是 SSL(安全套接字层)和最近的 TLS(传输层安全性)的用武之地。TLS 在 1999 年取代 SSL 成为 HTTPS 使用的加密技术。TLS 是如何工作的超出了本文的范围。

### HTTP 严格传输安全(HSTS)

这个很简单。让我们再次以脸书的头球为例:

```
strict-transport-security: max-age=15552000; preload
```

*   `max-age`指定浏览器应记住多长时间，以强制用户使用 HTTPS 访问网站。
*   对我们的目的来说并不重要。这是一项由谷歌托管的服务，不属于 HSTS 规范。

此标题仅适用于您使用 HTTPS 访问网站的情况。如果您通过 HTTP 访问该站点，则该标题将被忽略。原因很简单，HTTP 太不安全了，不能信任。

让我们用脸书的例子来进一步说明这在实践中是如何有用的。你第一次访问`facebook.com`，你知道 HTTPS 比 HTTP 安全，所以你通过 HTTPS`https://facebook.com`访问它。当您的浏览器收到 HTML 时，它会收到上面的标题，告诉您的浏览器强制将您重定向到 HTTPS，以备将来的请求。一个月后，有人用 HTTP，`http://facebook.com`给你发了一个去脸书的链接，你点击了它。由于一个月比`max-age`指令规定的 15552000 秒短，您的浏览器会以 HTTPS 的身份发送请求，防止潜在的 MITM 攻击。

### 结束语

无论您处于 Web 开发之旅的哪个阶段，web 安全性都非常重要。你越多地接触它，你就会过得越好。安全应该对每个人都很重要，而不仅仅是那些在职位上明确提到安全的人！？