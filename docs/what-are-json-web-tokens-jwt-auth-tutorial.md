# 什么是 JSON Web 令牌？JWT 认证教程

> 原文：<https://www.freecodecamp.org/news/what-are-json-web-tokens-jwt-auth-tutorial/>

大多数网络应用使用安全措施来确保用户数据的私密性。身份验证是安全性的一个关键部分，JSON Web 令牌(JWT)是实现身份验证的一种很好的方式。

## 那么什么是 JSON Web 令牌呢？

JWT 是一种标准，它定义了一种紧凑且独立的方式，以 JSON 对象的形式在客户机和服务器之间安全地传输信息。紧凑的大小使得令牌很容易通过 URL、POST 参数或在 HTTP 头中传输。此外，因为它们是自包含的，所以它们包含了关于用户的所有必要信息，所以数据库不需要被查询一次以上。

JWT 中的信息是可信的，因为它是使用秘密或公钥/私钥对进行数字签名的。

## 证明

JWT 主要用于身份验证。用户登录到应用程序后，应用程序将创建一个 JWT 并发送给用户。用户的后续请求将包括 JWT。令牌告诉服务器允许用户访问哪些路由、服务和资源。JWT 可以轻松地跨多个域使用，因此它们通常用于单点登录。

## 使用 JSON Web 令牌

Thomas Weibenfalk 制作了一个非常好的视频教程，解释了 JSON Web 令牌并演示了如何使用它们进行身份验证。本教程尽可能简单地教授 JWT Auth，不使用很多额外的库。

观看下面的教程或 freeCodeCamp.org YouTube 频道 (2 小时观看)。

[https://www.youtube.com/embed/x5gLL8-M9Fo?feature=oembed](https://www.youtube.com/embed/x5gLL8-M9Fo?feature=oembed)