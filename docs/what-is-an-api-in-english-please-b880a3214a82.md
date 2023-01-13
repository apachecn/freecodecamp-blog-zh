# 什么是 API？请用英语。

> 原文：<https://www.freecodecamp.org/news/what-is-an-api-in-english-please-b880a3214a82/>

彼得·加扎罗夫

在我学习软件开发之前，API 听起来就像一种啤酒。

今天，我经常使用这个词，事实上，我最近试图在酒吧订购 API。

酒保的回应是抛出 404:找不到资源。

我遇到很多人，无论是在技术领域还是其他领域工作的，他们对这个相当常见的术语的意思都有一个相当模糊或不正确的想法。

技术上，API 代表**应用编程接口**。在某个时候，大多数大公司都为他们的客户或内部使用构建了 API。

但是你怎么用简单的英语解释 API 呢？是否有比开发和商业中使用的含义更广泛的含义？首先，让我们回过头来看看网络本身是如何工作的。

## WWW 和远程服务器

当我想到网络时，我会想象一个由相互连接的服务器组成的大型网络。

互联网上的每一页都存储在远程服务器的某个地方。远程服务器并不神秘——它只是一台远程计算机的一部分，为处理请求进行了优化。

客观地说，你可以在你的笔记本电脑上安装一台服务器，能够为整个网站提供服务(事实上，工程师在向公众发布网站之前，用一台本地服务器来开发网站)。

当你在浏览器中输入[www.facebook.com](https://www.facebook.com/)时，一个请求就会发送到脸书的远程服务器。一旦浏览器收到响应，它就会解释代码并显示页面。

对于浏览器，也被称为客户端，脸书的服务器是一个 API。这意味着每次你访问网页时，你都要与一些远程服务器的 API 进行交互。

API 与远程服务器不同，它是服务器的一部分，接收请求并发送响应。

## API 作为服务客户的一种方式

你可能听说过公司将 API 包装成产品。例如，Weather Underground 出售其[天气数据 API](https://www.wunderground.com/weather/api) 的使用权。

**示例场景:**您的小企业网站有一个用于为客户预约的表单。您希望让您的客户能够自动创建一个包含该约会详细信息的 Google 日历事件。

API 使用:这个想法是让你的网站的服务器直接与谷歌的服务器对话，请求用给定的细节创建一个事件。然后，您的服务器将接收 Google 的响应，对其进行处理，并将相关信息发送回浏览器，比如给用户的确认消息。

或者，你的浏览器可以绕过你的服务器直接发送一个 API 请求到谷歌的服务器。

这个谷歌日历的 API 与其他远程服务器的 API 有什么不同？

**用专业术语**来说，区别就是请求和响应的格式。

为了呈现整个网页，您的浏览器期望得到包含表示代码的 *HTML、*响应，而 Google Calendar 的 API 调用只会返回数据——可能是像 *JSON* 这样的格式。

如果您网站的服务器发出 API 请求，那么您网站的服务器就是客户端(类似于当您使用浏览器导航到网站时，浏览器就是客户端)。

**从用户的角度来看，**API 允许他们在不离开网站的情况下完成操作。

大多数现代网站至少会使用一些第三方 API。

许多问题已经有了第三方的解决方案，无论是以库还是服务的形式。使用现有的解决方案通常更容易、更可靠。

对于开发团队来说，将他们的应用程序分成多个通过 API 相互通信的服务器并不罕见。为主应用服务器执行助手功能的服务器通常被称为 [*微服务*](http://microservices.io/patterns/microservices.html) *。*

总而言之，当一家公司向他们的客户提供 API 时，这仅仅意味着他们已经建立了一组返回纯数据响应的专用 URLs 这意味着**这些响应不会包含像网站**这样的图形用户界面中所期望的那种表示开销。

你能用你的浏览器提出这些要求吗？经常，是的。因为实际的 HTTP 传输是以文本形式进行的，所以您的浏览器总是会尽最大努力显示响应。

比如，你可以用浏览器直接访问 GitHub 的 API，甚至不需要访问令牌。以下是当你在浏览器中访问一个 GitHub 用户的 API 路由时得到的 JSON 响应([https://api.github.com/users/petrgazarov](https://api.github.com/users/petrgazarov)):

```
{  "login": "petrgazarov",  "id": 5581195,  "avatar_url": "https://avatars.githubusercontent.com/u/5581195?v=3",  "gravatar_id": "",  "url": "https://api.github.com/users/petrgazarov",  "html_url": "https://github.com/petrgazarov",  "followers_url": "https://api.github.com/users/petrgazarov/followers",  "following_url": "https://api.github.com/users/petrgazarov/following{/other_user}",  "gists_url": "https://api.github.com/users/petrgazarov/gists{/gist_id}",  "starred_url": "https://api.github.com/users/petrgazarov/starred{/owner}{/repo}",  "subscriptions_url": "https://api.github.com/users/petrgazarov/subscriptions",  "organizations_url": "https://api.github.com/users/petrgazarov/orgs",  "repos_url": "https://api.github.com/users/petrgazarov/repos",  "events_url": "https://api.github.com/users/petrgazarov/events{/privacy}",  "received_events_url": "https://api.github.com/users/petrgazarov/received_events",  "type": "User",  "site_admin": false,  "name": "Petr Gazarov",  "company": "PolicyGenius",  "blog": "http://petrgazarov.com/",  "location": "NYC",  "email": "petrgazarov@gmail.com",  "hireable": null,  "bio": null,  "public_repos": 23,  "public_gists": 0,  "followers": 7,  "following": 14,  "created_at": "2013-10-01T00:33:23Z",  "updated_at": "2016-08-02T05:44:01Z"}
```

浏览器似乎已经很好地显示了 JSON 响应。这样的 JSON 响应已经可以在您的代码中使用了。从这段文字中提取数据很容易。然后你可以对这些数据做任何你想做的事情。

## a 代表“应用”

最后，让我们再举几个 API 的例子。

“应用”可以指很多东西。以下是 API 环境中的一些例子:

1.  一款功能独特的软件。
2.  整个服务器，整个应用程序，或者只是应用程序的一小部分。

基本上，任何一个软件都可以与它的环境截然分开，都可以成为 API 中的“A ”,并且很可能也有某种 API。

假设您在代码中使用了第三方库。一旦整合到你的代码中，库就成为了你整个应用的一部分。作为一个独特的软件，这个库可能有一个 API，允许它与你的代码的其余部分进行交互。

这里有另一个例子:在**面向对象设计**中，代码被组织成对象。您的应用程序可能定义了数百个可以相互交互的对象。

每个对象都有一个 API——一组*公共*方法和属性，用于与应用程序中的其他对象进行交互。

一个对象也可能有内部逻辑，它是*私有的，*意味着它是对外部范围(而不是 API)隐藏的**。**

**从我们已经介绍的内容中，我希望您能够理解 API 的更广泛的含义以及这个术语目前更常见的用法。**

### **有趣的资源(我漏掉了但仍然很酷的东西):**

**[DNS(域名系统)上一个很棒的 youtube 视频](https://www.youtube.com/watch?v=72snZctFFtA&feature=youtu.be)**

**[HTTP 协议基础知识](https://simple.wikipedia.org/wiki/Hypertext_Transfer_Protocol)**

**[可汗学院关于面向对象设计原则的视频](https://www.khanacademy.org/computing/computer-programming/programming/object-oriented/p/object-types#)**