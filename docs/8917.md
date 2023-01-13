# RESTful 服务第三部分:HATEOAS 和 Richardson 成熟度模型

> 原文：<https://www.freecodecamp.org/news/restful-services-part-iii-hateoas-and-the-richardson-maturity-model-48d4e4c79b8d/>

由 sanchitgear

# RESTful 服务第三部分:HATEOAS 和 Richardson 成熟度模型

![fpEPt7dKK1eS59MHNDBJBCRhYRgBONbkULdT](img/d21455d0593804b6fb24d6105c392660.png)

在本系列的第一部分中，您学习了 HTTP 的基本知识。我们讨论了常见的 HTTP 结构，如标题、URL 和不同的可用状态代码。我们还研究了在构建面向资源的 web 服务时，这些结构是如何有用的。

在第二部分中，您了解了为了构建可伸缩的、高性能的 RESTful 系统，您需要遵守的不同约束。

这篇文章将为你提供第三个也是最后一个谜题。如前所述，REST 不是一个标准，而是一个抽象的概念。这使得准确量化一个服务的“RESTful”程度变得困难。

虽然上一部分讨论的约束在创建服务时很有帮助，但它们并不擅长解决这个问题。如果您选择严格遵循其中一个约束条件会怎么样？两个？三个？在什么时候，你的服务停止了部分的宁静，并进入了完全宁静的*的神奇之地？*

这正是理查森成熟度模型(RMM)帮你解决的问题。但是在我们深入 RMM 的本质之前，有最后一个主题将被证明对您理解 REST 是有用的。

### 仇恨的原则

**H**ypermedia**A**S**T**he**E**ngine**O**f**A**application**S**tate，简称 HATEOAS，建立在 REST 的一个约束之上(统一接口*)。我还在试图确定如何发音。我通常在讨厌和讨厌之间转换。随便选一个，或者想出自己的。不过反正我跑题了。*

*HATEOAS 背后的首要目标是为机器提供一种一致的方式来理解 API 并在事先没有任何信息的情况下导航它们，就像用户第一次访问网站一样。*

*假设你第一次访问 Medium 来写一篇文章。你会采取什么步骤？十有八九，你会访问 Medium 的主页，前往*故事*版块，开始撰写你的杰作。你并不真正关心*故事*版块所在的 URL。你没有记住它，但是你知道当你需要的时候你将能够到达那里。*

*或者想象你在亚马逊上订购一些东西。你进去，搜索不同的商品，把它们加入购物车，然后结账。作为用户，这些组件在系统中的位置对您来说无关紧要。如果需要访问购物车的 URL，很有可能您甚至都找不到。然而，你的经历并没有受到影响。*

*在这两种情况下，你只需要一条信息，那就是网站的入口点。从那时起，通过导航相关链接(又名*超媒体)，其他一切都是完全可以发现和使用的。这就是网络的工作方式，也是当今大多数用户的体验。**

*HATEOAS 也将这种可发现性的思想扩展到了 API 和 web 服务。如果给我一个单点访问服务，我就可以利用它所提供的一切，那会怎么样？幸运的是，这可以通过利用我们一直努力工作的数据的面向资源的性质来实现！*

*我们知道，因为我们的服务返回的所有东西本质上都是一种资源，所以我们的服务消费者只能用那种资源做少数事情。此外，每个动作都对应于我们系统中定义良好的 RESTful 路径(想想 GET、POST、PUT 等。).这意味着我们可以很容易地将所有与给定资源的潜在交互以可操作 URL 的形式嵌入到响应中。我们来看一个例子！*

*让我们回到我们之前在媒体上写故事的例子。想象一下，如果不是一个面向用户的网站，而是纯粹的网络服务。在 HATEOAS 模型下，我需要的导航服务的唯一信息是主机名:**medium.com***

*我通过向主机发出 GET 请求来开始我的交互。Medium 立即给出了它所能提供的所有资源的列表，以及在哪里可以找到它们。*

```
*`GET medium.com`*
```

```
*`links : [  {    rel : "bookmarks",    href : "/bookmarks"  },   {    rel : "stories",     href: "/stories"  }]`*
```

*在这个简化版的 Medium 中，我被告知提供了两种资源:*故事*和*书签。*我还知道这些资源在系统中的位置。*

*接下来，我需要想出如何创造一个新的故事。从我们之前的讨论中，我已经知道这将是一个 POST 请求，但是作为一个客户，我仍然不知道服务对这个请求期望什么样的数据。这正是选项请求派上用场的地方。所以让我们就这么做吧！*

```
*`OPTIONS medium.com/stories`*
```

```
*`Allow GET, POST{  name : "Stories",  description: "Ideas and opinions from around the world",   actions: [    {      POST: {        title: "string",        body: "string"      }    }  ]}`*
```

*啊哈！看起来我们需要名为 *title* 和 *body* 的参数来对应我们的新帖子。这给了我们所有需要的信息。我们现在可以开始向该服务发帖，并在该服务上创建新文章。*

*现在让我们说，按照这种方法，我降落在一个现有的故事。一个单独的故事会是什么样的？*

```
*`GET medium.com/stories/3`*
```

```
*`{  "id": 3,  "title": "An Introduction to Microservices",  "body": "...",  "created_at": "2016-10-25T20:52:12.804Z",  "links": [  {    "rel": "self",    "href": "/stories/1"  },   {    "rel": "author",    "href": "/authors/3"  },   {    "rel": "comments",    "href": "/stories/3/comments"  }],}`*
```

*现在，我不仅有了关于这个故事的信息，而且还有了一种获取作者和评论信息的方法。*

*当然，这是过分简单化了。还有大量其他事情需要处理，比如认证和授权。并且需要做大量的工作来设计可以如此容易地分解成资源的系统。但是理解 HATEOAS 背后的想法是有好处的。*

*这消除了开发人员维护服务文档的需要。客户可能需要知道的关于使用你的服务的一切都已经在那里了。*

*同样，作为客户，我不需要跟踪与这些资源相关的 URL。我寻找对应于资源的对象，并导航到它。如果变了，我不在乎。*

*考虑到这一点，现在让我们把注意力转移到理查森成熟度模型(RMM)上。*

### *理查森成熟度模型*

*如前所述，RMM 是一个帮助您评估服务 RESTful 程度的工具。这个分类系统——首先由 Leonard Richardson 描述——提供了一个从最终用户的角度考虑 web 服务的简洁方法，然后做出相应的判断。*

*理查森描述了宁静谱系中的四个不同层次。*

#### *0 级*

*这是 RESTful 服务的底线。此类服务遵循“一个 URL，一种方法”的原则。这意味着，该服务只向外界公开一个 URL，并且在该位置只接受一种类型的请求(通常是 POST)。*

*例如，这对于 SOAP 服务来说是典型的。对 SOAP 服务的典型请求如下所示:*

```
*`POST /Quotation HTTP/1.0Host: www.xyz.orgContent-Type: text/xml; charset=utf-8Content-Length: nnn<?xml version="1.0"?><SOAP-ENV:Envelope xmlns:SOAP-ENV="http://www.w3.org/2001/12/soap-envelope" SOAP-ENV:encodingStyle="http://www.w3.org/2001/12/soap-encoding" >   <SOAP-ENV:Body xmlns:m="http://www.xyz.org/quotations" >	      <m:GetQuotation>         <m:QuotationsName>MiscroSoft</m:QuotationsName>      </m:GetQuotation>		   </SOAP-ENV:Body>	</SOAP-ENV:Envelope>`*
```

*在请求的主体中描述了一切，包括动作(getQuotation)和请求的参数(Microsoft)。显然，该服务没有利用我们到目前为止讨论过的任何 REST 原则，更不用说在 HTTP 之上拥有一个完整的其他数据格式的成本了。*

#### *级别 1:资源*

*通往完全 RESTful 的下一步是引入基于资源的抽象。这相当于将应用程序分解成不同的特定于资源的 URL。Richardson 将其描述为“多个 URL，一种方法”的实现。*

*这里，系统中有几个不同的 URL 协同工作来提供所需的功能。但是每个都只接受一种类型的请求(同样，通常是 POST)。*

*例如，继续我们前面的例子，我们可以首先从我们的应用程序中检索所有公司的列表:*

```
*`POST /companies[  {    "name" : "Microsoft",    "id" : 3  },   {    "name" : "Apple",    "id" : 4  }]`*
```

*…然后获得一家公司的报价:*

```
*`POST /quotations/3`*
```

```
*`{  quotation: {}}`*
```

*这绝对比以前进步了。事实上，在 REST 流行起来之前，很多应用程序都是这样编写的。但是同样，我们没有利用 HTTP 的优势。我们可以做得更好！*

#### *第二级:动词*

*现在我们把动作动词的概念也加入进来。除了拥有定义良好的资源之外，可以在资源上执行的操作必须严格遵循 HTTP 约定。*

*GET 不能修改资源状态，POST 只能用于资源创建，等等。当然，它的特点是“许多 URL，许多动作”系统。*

*这就把我们带到了我们大多数人都熟悉并日常使用的服务上。这些也是我们通常认为是 RESTful 的服务。然而，为了达到梦寐以求的完全宁静的状态，服务还必须符合一个层次。*

#### *第三级:HATEOAS*

*这是大多数服务的不足之处。作为开发人员，您遇到的绝大多数 API 和 web 服务，或者您将要使用的 API 和 web 服务，很可能不遵循 HATEOAS 的原则。*

*大多数服务提供者仍然倾向于以传统方式记录他们的服务，为开发人员提供一个可用端点列表以及一些关于如何与该端点交互的信息。下面是 [Twitter REST API](https://dev.twitter.com/rest/public) 的例子。(有趣的是，PayPal API [强烈推荐超媒体控件](https://developer.paypal.com/docs/integration/direct/paypal-rest-payment-hateoas-links/)。)*

*这不一定是坏事。支持和反对使用 HATEOAS 都有一些很好的论据。虽然一方面它使 API 易于发现和使用，但这通常是以更多的开发时间和精力为代价的。*

*事实上，如果你需要做的只是对 API 进行一次调用，引入 HATEOAS 实际上可能会让你作为消费者的事情变得更加困难。*

### *结论*

*在一天结束的时候，这些措施并不是你必须遵守的。REST，连同它所有的限制，仅仅是构建 web 服务和应用程序时工具带中的一个工具。你完全可以从中选择最适合你需求的。*

*我希望你从这个系列中学到了很多有用的概念。如果您希望创建一个 RESTful web 服务来补充您的下一个项目，或者希望使用现有的 web 服务，那么您现在应该对它们背后的一些基本原理有了很好的理解。*

*以下是之前部分的链接，以防您错过:*

*RESTful 服务第一部分:HTTP 概要*

*RESTful 服务第二部分:约束和目标*

*请在评论中告诉我你对这篇文章的看法，或者通过 [LinkedIn](https://www.linkedin.com/in/sanchitgera) 联系我。*

***别忘了砸了？如果你喜欢这篇文章。***

*干杯，学习愉快！*

****更多资源****

*   *马丁·福勒的博客*
*   *[伦纳德·理查森的演讲](https://www.crummy.com/writing/speaking/2008-QCon/act3.html)*
*   *借用自[教程的 SOAP 示例点](https://www.tutorialspoint.com/soap/soap_examples.htm)*