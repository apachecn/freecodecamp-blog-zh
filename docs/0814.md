# 如何使用 REST APIs 完全初学者指南

> 原文：<https://www.freecodecamp.org/news/how-to-use-rest-api/>

应用程序编程接口(API)是一个需要理解的重要编程概念。如果你花时间去学习更多关于这些接口的知识，它可以帮助你的任务变得更容易管理。

一种常见的 API 是 REST API。如果您曾经考虑过从另一个网站获取数据，比如 Twitter 或 GitHub，您可能已经使用过这种 API。

那么为什么理解一个 REST API 是有用的呢？它如何确保现代业务连接？

在构建或操作一个 API，尤其是 REST API 之前，您应该首先了解什么是 API。本文将带您了解 REST API 原则，以及它们是如何发展成为强大的应用程序的。

## API 是如何工作的，为什么需要它们？

API 代表一组定义和协议。您需要它们来进行应用程序开发和集成，因为它们促进了两个软件之间的数据交换，比如信息提供者(服务器)和用户。

API 指定从返回响应的生产者发出调用的客户机可用的内容。

程序使用 API 来通信、检索信息或执行功能。API 允许用户使用系统返回他们想要的结果。

简单来说，API 充当用户(客户端)和资源(服务器)之间的中介。

当用户发出 API 请求或访问在线商店时，他们希望得到快速响应。所以你需要[优化 Magento TTFB(到达第一个字节的时间)](https://onilab.com/blog/magento-ttfb-optimization/)或者使用其他最适合你的 CMS 的性能增强策略。

集成 API 的原因包括:

*   简化资源和信息共享
*   借助[认证控制谁可以访问什么，并定义权限](https://www.freecodecamp.org/news/authenticate-and-authorize-apis-in-dotnet5/)
*   安全和控制
*   不需要了解软件细节
*   服务之间的一致通信，即使它们使用不同的技术

## **REST API 概述**

![DUwmoHyRnoD1WovETSrQdSaIv8rh5WUVPxVjPN9_cvVokx7E4fZxzGyCY0_XMRA2cikjPkWIUDlXmtDqqGDX-KCzya5EVEEgxi8sEVwpVTeiHBNsqCULC-78QCE4dJ0_ieC1mQzn](img/d972da12dd092e73c182823e766720a2.png)

RESTful 指的是软件架构，代表“表述性状态转移”。您可能在标准化信息交换系统(web 服务)的使用时听说过它。

这些 web 服务利用一种无状态协议来使它们的在线资源的文本表示可供阅读和处理。客户端执行众所周知的基于 HTTP 协议的活动，如获取、更新和删除。

REST 最初是在 2000 年建立的，目标是通过对 API 实施特定的限制来提高性能、可伸缩性和简单性。

因为有机会覆盖各种设备和应用程序，所以它越来越受欢迎。下面你会发现使用 REST APIs 的一些目的。

![Jk2xFwUgtgRzOuJuSa9kiWPPe51CN0qLd2hXMJ3F2SyW6MM10Gzq2qIY36dDQQj6fPJPG7Axl3q431QumWwi3WtYyFC1FA5TcI1i7i5PeQOO38tpdSCgIF0dJktnVhoWvVjAwFOK](img/ae499d311d877acd0e14f716c1cc2261.png)

### 1.网络使用

REST 没有特定的客户端技术，因为它适合不同的项目，例如:

*   web 开发
*   iOS 应用程序
*   物联网设备
*   Windows Phone 应用

由于您不必拘泥于特定的客户端堆栈，因此可以为您的公司构建任何基础架构。

### 2.云中的应用

REST API 调用是云应用程序的理想选择，因为它们是无状态的。如果出现问题，您可以重新部署无状态组件，并且它们可以增长以管理流量变化。

### 3.云计算

到服务的 API 连接需要控制 URL 的解码方式。这就是为什么 REST 在云服务中变得更加有用。

得益于云计算和微服务，RESTful API 架构将成为未来的常态。

## REST APIs 是如何工作的？

数据(比如图像、视频和文本)体现了 REST 中的资源。一个客户端访问一个特定的 URL 并发送一个服务器请求来接收响应。

![HwYHNtAz8M84Tggswzk662nm_dyGUA77st12KGsiqw4rVBGqhJM2gQ5wgL2sL8ZhWmwOGsoEJx6Uqt7TdxU4Bkbg_uccr2UVTXtWsxnR495yZReGoY_reZEd9rq5_9vnjiaUUBs2](img/72078230776492b09cdc337afa4691c0.png)

### REST APIs 背后的概念

请求(您访问的 URL)包含四个部分，分别是:

*   **端点**，它是具有结构`root-endpoint/?`的 URL
*   具有五种可能类型(GET、POST、PUT、PATCH、DELETE)之一的**方法**
*   **头**，提供各种功能，包括认证和提供关于主体内容的信息(您可以使用`-H`或`--header`选项来发送 HTTP 头)
*   **数据(或主体)**，这是您通过`-d`或`--data`选项发送给服务器的 POST、PUT、PATCH 或 DELETE 请求。

HTTP 请求允许您操作数据库，例如:

*   发布创建记录的请求
*   从服务器读取或获取资源(文档或图像、其他资源的集合)的 GET 请求
*   更新记录的上传和修补请求
*   从服务器删除资源的删除请求

这些操作代表四种可能的操作，称为 CRUD:创建、读取、更新和删除。

![Quydyrq2Zw2Mh3uJj4G9LE40DhjJyWLjRCU9-hqs0uKt-hGCgoyGVP9eiU_6IBnb6GwxsILeu9kqjO5LQ6s7LBmHDtbksnqb13YtPoCKRq062zXi1Pz4wf0GAO27maHMlhamixAz](img/97447127d013a26dafb64199941dcc61.png)

服务器以下列格式之一向客户机发送数据:

*   [HTML](https://www.freecodecamp.org/news/html-best-practices/)
*   JSON(这是最常见的一种，因为它独立于计算机语言，并且可以被人类和机器访问)
*   XLT
*   服务器端编程语言（Professional Hypertext Preprocessor 的缩写）
*   计算机编程语言
*   纯文本

## 为什么要使用 REST API？

为什么你更喜欢 REST 而不是其他 API，比如 SOAP？原因有很多，比如可伸缩性、灵活性、可移植性和独立性。

![yJ2QDrpGbA-RpzwhXOXr1yl9aGTvVHXeiuyBvFxsMtE5KQu2wRmNLwlCX7cNGOlp1TjRK-P9VsBsFaGRNkxZw-QWvggxqXLYFtLg-THClHzB-5GJlMX6hGkY3DQnFh1YpzkHt2iE](img/ae12773a8b13ebac78eccd64f96e8ee3.png)

### 不依赖于项目结构

独立的客户端和服务器操作意味着开发人员不会被绑定到任何项目部分。由于自适应 REST APIs，他们可以开发每个方面，而不会影响另一个方面。

### 便携性和适应性

REST APIs 仅在来自其中一个请求的数据被成功交付时才起作用。它们允许您从一台服务器迁移到另一台服务器，并随时更新数据库。

### 未来扩大项目规模的机会

由于客户机和服务器独立工作，编码人员可以快速开发产品。

## **RESTful 建筑风格的特征**

开发人员必须考虑一些 API 的刚性结构，比如 SOAP 或 XML-RPC。但是 REST APIs 不一样。它们支持广泛的数据类型，并且实际上可以用任何编程语言编写。

六个 REST 架构约束是设计解决方案的原则，如下所示:

![XRsmwgFoTf1sCI3hZf6n5DxHXDqHclunxf6ocqxjUVgWPss5KHiz8wm4fXYzCJ9mkijpfwhGc-YzSO_R1fm9JtOej1T1SQJwngs-wK_Lz0DhUwI2LfCOQWsZvm88nVlkGkmBgV-E](img/a38462ee0ae9b94d2698c201d9e967a0.png)

### **1。统一界面(一致的用户界面)**

这个概念规定，对同一资源的所有 API 查询，不管它们来自哪里，都应该是相同的，也就是说，使用一种特定的语言。一个统一资源标识(URI)与相同的数据相关联，例如用户名或电子邮件地址。

另一个统一接口原则规定消息应该是自描述的。它们必须是服务器可以理解的，以确定如何处理它(例如，请求的类型、mime 类型等等)。

### **2。客户端-服务器分离**

REST 架构风格对客户机和服务器实现采取了一种特殊的方法。问题是，它们可以独立完成，并且不需要知道另一个。

例如，客户机只有所请求资源的统一资源标识(URI ),不能以任何其他方式与服务器程序通信。另一方面，服务器不应该影响客户端软件。所以它通过 HTTP 发送基本数据。

这是什么意思？您可以随时修改客户机代码，而不会影响服务器的操作。

服务器代码是一条船上的:改变服务器端不会影响客户端的操作。

只要每一方知道向另一方传递什么样的消息格式，就可以保持客户机和服务器程序的模块化和独立性。

通过将用户界面问题从数据存储限制中分离出来，我们能实现什么？我们改进了跨平台的接口灵活性，提高了可扩展性。

此外，每个组件都从分离中受益，因为它可以独立发展。REST 接口帮助不同的客户端:

*   访问相同的 REST 端点
*   执行相同的活动
*   收到类似的回应

### **3。客户端和服务器之间的无状态通信**

基于 REST 的系统是无状态的，这意味着客户机状态对于服务器来说是未知的，反之亦然。这种约束允许服务器和客户机理解任何发送的消息，即使它们没有看到前面的消息。

为了加强这种无状态的约束，您需要使用资源而不是命令。这些是网络的名词。它们的目的是描述您可能想要保留或与其他服务通信的任何对象。

您可以控制、更改和重用组件，而不会影响整个系统，因此这种约束的好处包括实现:

*   稳定性
*   速度
*   RESTful 应用的可伸缩性

请注意，每个请求都应该包含完成请求所需的所有信息。客户端应用程序必须保存会话状态，因为服务器应用程序不应该存储任何与客户端请求相关联的数据。

### **4。可缓存数据**

REST 要求尽可能缓存客户端或服务器端的资源。数据和响应缓存在当今世界至关重要，因为它可以带来更好的客户端性能。

它如何影响用户？管理良好的缓存可以减少或消除一些客户端-服务器交互。

由于减轻了服务器的负担，它还为服务器提供了更多的可伸缩性选项。缓存提高了页面加载速度，并允许您在没有互联网连接的情况下访问以前查看的内容。

### **5。分层系统架构**

![DBk2dcqnTMZdz-dBA0sFDUe5cQu71VxMqG8pW-ux4rqNvkVcsixRNR_ZyuY1z6UeWWZ5NRV11FPIv8XYK86EGr2G-Nnb7O_njC9PER6a5TdmfpZ2qmRTI7f9P--S7QU50cYwD9EC](img/8cf8c862fc8e5733bb57c244e851cc91.png)

RESTful 分层设计结构是讨论中的下一个约束。这一原则包括将具有特定功能的不同层分组。

REST API 层有它们的职责，并按照层次顺序排列。例如，一个层可能负责在服务器上存储数据，第二个层负责在另一个服务器上部署 API，第三个层负责在另一个服务器上验证请求。

这些层充当中介，阻止客户端和服务器应用程序之间的直接交互。因此，客户端不知道它们寻址的是哪个服务器或组件。

当每一层在将数据传输到下一层之前执行其功能时，这意味着什么？它提高了 API 的整体安全性和灵活性，因为添加、更改或删除 API 不会影响其他接口组件。

### **6。按需编码(非强制性)**

使用 REST APIs 最常见的场景是以 XML 或 JSON 的形式交付静态资源表示。

然而，这种架构风格允许用户下载并运行 Java 小程序或脚本(如 JavaScript)形式的代码。例如，客户端可以通过调用 API 来检索 UI 小部件的呈现代码。

## **使用 REST APIs 时你应该预料到的挑战**

当您理解了 REST API 设计和架构约束时，您应该知道在使用这种架构风格时会遇到的问题:

![FnzdrS-v1CIkyY6lWVBZymkIbLGDOQb4ZFAPqcJD6_EDL9QL1Xd3KGwd2SP24GfYO2CTwO4-9ra4a8Dc8gOvokndr3uO7Zt0-VOjQjR6bdcLrSH3SWK0vmAeg5mZlEavHkgpsIhh](img/5cbfcdb8653426a41a1fd1ca572bc801.png)

### 关于 REST 终点的协议

不管 URL 构造如何，API 都应该保持一致。但是随着可能的方法组合的增长，在大型代码库中保持一致性变得更加困难。

### 版本控制是 REST APIs 的一个特性

API 需要定期的[更新或版本控制](https://www.freecodecamp.org/news/how-to-version-a-rest-api/)来防止兼容性问题。但是，旧端点仍在运行，这增加了工作负载。

### 许多认证方法

您可以指定哪些资源可用于哪些用户类型。例如，您可以确定哪些第三方服务可以访问客户的电子邮件地址或其他敏感信息，以及它们可以对这些变量做什么。

但是现有的 20 种不同的授权方法会使您的初始 API 调用变得困难。这就是为什么开发人员由于最初的困难而不继续这个项目。

### REST API 安全漏洞

尽管 RESTful APIs 有一个分层结构，但是仍然存在一些安全问题。例如，如果应用程序由于缺乏加密而不够安全，它可能会暴露敏感数据。

或者，黑客可能每秒发送数千个 API 请求，导致 DDoS 攻击或 API 服务的其他滥用，使您的服务器崩溃。

### 过多的数据收集和请求

服务器可能会返回包含所有数据的请求，这可能是不必要的。或者您可能需要运行多个查询来获取所需的信息。

## **结束**

毫无疑问，API 被预测将在未来简化基于 web 的通信。他们的目的是允许任何网络应用程序进行交互和共享数据。

例如，他们帮助发展中的在线企业开发强大的创新系统。随着 API 架构的发展，它采用了更轻便、更灵活的变体，这对于移动应用和分散的网络来说至关重要。

因此，在本文中，您了解了使用 REST APIs 需要了解的基础知识。