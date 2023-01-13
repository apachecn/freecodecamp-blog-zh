# 快速浏览 Kubernetes 微服务中的通信

> 原文：<https://www.freecodecamp.org/news/communication-in-kubernetes-microservices-bf0a2af06551/>

亚当·汉森

# 快速浏览 Kubernetes 微服务中的通信

#### “服务”概念和 Node.js 示例

![1*JsNkOMxwSlxFNW44eFBdqg](img/389a9cba7dc369f7e968ecd629f8ec4a.png)

Chico Hailing a Cab

基于复杂性，一层微服务可能需要多种通信。Kubernetes 在管理服务、负载平衡和网络方面提供了丰富的特性。

这篇文章旨在提供一个简单的例子。如需深入了解，请参见[关于服务的官方文档](https://kubernetes.io/docs/concepts/services-networking/service/)。

### 服务

> Kubernetes `Service`是一个抽象，它定义了一个逻辑集合`Pods`和一个访问它们的策略——有时称为微服务。~ [Kubernetes Docs](https://kubernetes.io/docs/concepts/services-networking/service/)

正如所记录的，我们在公开服务和与服务通信方面有许多选择。我们来看看 Kubernetes DNS。关于 DNS 命名的细节可以在[这里](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#what-things-get-dns-names)找到。

### 简单的例子

考虑一组需要通过 HTTP 协议相互通信的微服务。例如，假设我们需要创建一个 cron 作业来从另一个 pod ping 一个健康检查端点，并记录响应状态代码。

我们有 Node.js Express app。

很公平，这个应用程序可以接收对“/healthcheck”的 HTTP GET 请求，并用 JSON 进行响应。

好了，现在让我们创建一个小型 cron 作业应用程序，在每天早上 8:00 执行对该端点的请求。

很好，很好…这里没什么异常。嗯，等一下——让我们仔细看看下面定义要获取的端点的那一行。

```
const apiUrl = 'http://api:3000/healthcheck';
```

在使用 Kubernetes DNS 时，与其他 pod 通信就是这么简单！我们上面的第一个应用程序(Express 应用程序)的服务配置可以像下面这样简单。

我们的 cron 应用程序可能与上面的非常相似。配置 pods 超出了本文的范围，但是您可以阅读如何利用[部署](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)来完成。

### 结论

Kubernetes 提供了大量的特性和文档，支持各种内部通信和对外公开的方式。我们可以简单地从 DNS 中获得显著的收益。