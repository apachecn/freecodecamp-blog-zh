# Webtask 后端即服务:快速无服务器教程

> 原文：<https://www.freecodecamp.org/news/webtask-backend-as-a-service-quick-serverless-tutorial/>

### 查尔斯·韦莱

今天， ****无服务器**** 这个词在几十个开发人员圈子里嗡嗡作响。

已经有一段时间了。

我一直想退出我的代码编辑器，来谈谈这里的趋势。尤其是几个月前，我发现了 Webtask。

因此，在错误修复、支持和新特性之间，我终于花了一些时间坐下来写点东西。

在这篇文章中，我将稍微谈一下无服务器架构和后端即服务工具。但是我主要关注的是提供一个简单而有意义的无服务器电子商务教程。我将使用我们自己的 HTML/JS 购物车平台& Webtask 来完成这项工作。

## 什么是无服务器体系结构，为什么要关注它？

像[“jam stack”](https://snipcart.com/blog/jamstack-clients-static-site-cms)、**、**无服务器架构**、**是一种新的网络发展趋势，顺应了我们所处的“前端崛起”范式。通俗地说，就是在云中运行服务器端代码，不用担心 web 服务器、路由等问题。它很大程度上依赖于 ****后端即服务(Baas)**** 第三方比如 Webtask，或者是 block 上最受欢迎的 kid:[AWS Lambda](http://docs.aws.amazon.com/lambda/latest/dg/welcome.html)。有了这些 BaaS 服务，您只需编写代码，让他们负责所有底层基础设施。从规模上来说，这种方法非常棒:这些服务提供的抽象层可以很好地处理网站上的流量高峰。

如你所知，我们是 Snipcart 的 jam stack(JavaScript，API&标记)的忠实粉丝。这是我提到的“前端崛起”范式的另一种生动表达。我们已经写了关于如何使用静态站点生成器运行电子商务的完整教程，比如 [Jekyll](https://snipcart.com/blog/static-site-e-commerce-part-2-integrating-snipcart-with-jekyll) 或 [Hugo](https://snipcart.com/blog/hugo-tutorial-static-site-ecommerce) ，甚至使用像 Contentful 这样的 [API 优先的 CMS。从成本到技术的转变，我相信这种方法将会在未来几年深刻影响企业处理 web 开发的方式。](https://www.contentful.com/blog/2016/02/10/snipcart-middleman-contentful/)

然而，我知道它有其局限性:一个现代的静态网站是 ****原始内容**** ，这意味着如果没有 **一些** 服务器端代码，使用 webhooks 等动态特性是不可能的。这就是 Webtask 的用武之地。

## Webtask:目标任务的后端即服务(或 FaaS)

Webtask 是由 [Auth0](https://auth0.com/) 精心制作的一个简洁的服务，这些好人对在线认证世界做出了重大贡献。作为功能即服务，它基本上消除了为简单的移动或单页应用程序配置后端的需要。通常[与 AWS Lambda](http://thenewstack.io/often-choose-webtask-lambda/) 相比，它允许开发者编写通过 HTTP 调用执行的服务器端逻辑&功能。因此，对于那些宁愿关注前端也不愿配置后端的开发人员来说，它是最好的后端即服务工具之一。

现在让我们看看它对于我们将在这个无服务器教程中探索的用例有多完美。

## 无服务器电子商务教程:webhooks &定制运费

在 Snipcart，我相信我们最强大的功能之一是 Webhooks 的发布 API。简单地说，它让你完全控制你的电子商务网站如何处理运输。

然而，利用这个特性需要运行服务器端代码。所以，如果你想使用一个带有静态站点生成器的 JAMstack，你就完了。然而，多亏了 Webtask，你不是！在这个无服务器教程中，我们将使用它直接通过他们的平台托管我们需要的电子商务运输功能。

## 我们简单的无服务器电子商务用例

现在，让我们假设我们有一个运行 Snipcart 的静态电子商务站点。

假设我们想提供三种特殊运费:

*   一个给我们魁北克老家的顾客
*   一个给美国客户
*   一个用于所有其他国际客户

## 1.如何创建 Webtask 函数

首先，让我解释一下我们的运输 API 是如何工作的。Snipcart 将所有订单细节发送到您可以在您的商家仪表板中指定的 URL。使用这些数据，您可以编写代码将可用的运输费率返回给您的最终客户。

****首先在****[](https://webtask.io/)******上创建一个帐户，完成后，按照他们的步骤通过**** `****npm****` ****安装 Webtask CLI。******

**我们现在将创建一个名为`shipping_task.js`的文件。它将包含解析从 Snipcart 接收的订单细节并返回可用运费所需的所有代码。**

**让我们从导出一个 Webtask 能理解的模块开始。**

```
`module.exports = function (context, cb) {
    cb(null, context.body);
}`
```

**第一个参数 context 包含 Snipcart 将发送给应用程序的数据。Webtask 负责解析 JSON，您可以通过 context.body 访问所有事件细节和顺序。**

**使用上面的代码，我们的任务将返回它收到的请求体；很没用。；)**

**现在，假设我们想为魁北克的客户提供免费送货服务。**

```
`module.exports = function (context, cb) {
    var orderDetails = context.body.content;
    var rates = [];

    var address = orderDetails.shippingAddress || order.billingAddress;

    if (address.country == "CA" && address.province == "QC") {
      rates.push({
        cost: 0,
        description: "Free shipping for Québec residents!"
      });
    }

    cb(null, { rates: rates });
}`
```

**那不是很好吗？但是，有了这个代码，魁北克以外的客户将没有任何运输选项。因此，如果订单不符合我们的条件，我们将确保退回标准运费:**

```
`if (rates.length === 0) {
  rates.push({
    cost: 20,
    description: "Standard shipping"
  });  
}`
```

**您可以在下面看到带有一些附加条件的完整代码:**

```
`module.exports = function (context, cb) {
    console.log(context.body);

    var orderDetails = context.body.content;

    var rates = [];

    var address = orderDetails.shippingAddress || order.billingAddress;

    if (address.country == "CA" && address.province == "QC") {
      rates.push({
        cost: 0,
        description: "Free shipping for Québec residents!"
      });
    }

    if (address.country == "CA" && address.province != "QC") {
      rates.push({
        cost: 10,
        description: "Shipping to Canada"
      });
    }

    if (address.country == "US") {
      rates.push({
        cost: 15,
        description: "Shipping to US"
      });
    }

    if (rates.length === 0) {
      rates.push({
        cost: 20,
        description: "Standard shipping"
      });
    }

    cb(null, { rates: rates });
}`
```

**然后，Webtask 将生成一个 URL 你应该在你的终端里看到它。**

**在配置 Webhooks Shipping API 时，只需使用这个 URL。**

#### **2.保护无服务器组件**

**按照我们目前的设置，任何人都可以向这个 API 发送请求，对吗？这不是我们想要的。因此，我们将确保该函数只处理来自 Snipcart 的请求。我们将使用我们的请求验证 API 来做到这一点。**

**首先，我们需要向任务发送一个 Snipcart secret API 密匙，以便调用请求验证 API。我们不想通过代码直接公开它，所以我们将使用 Webtask 具有的`secrets`特性。它允许我们向任务传递秘密参数，这些参数将被加密并可通过`context`对象访问。**

**创建任务时，我添加了`--secret`开关:**

```
`wt create shipping_task.js — secret snipcartApiKey=MY_SECRET_API_KEY`
```

**然后，您将能够使用`context.secrets.snipcartApiKey`访问该值。我们还将使用`request`模块，所以我们需要在文件的开头使用它:**

```
`var request = require('request');` 
```

**当我们向你的 Webhooks 发出请求时，我们总是在请求头中包含一个请求令牌。标题名为`X-Snipcart-RequestToken`。我们将再次通过我们的`context`对象访问它:**

```
`var requestToken = context.headers['x-snipcart-requesttoken'];`
```

**请注意，在 Webtask 中，所有的标题都是小写的。**

**下面是我们将用来向 API 发送请求的选项:**

```
`var requestToken = context.headers['x-snipcart-requesttoken'];
var secretApiKey = context.secrets.snipcartApiKey;

var requestOptions = {
  url: 'https://app.snipcart.com/api/requestvalidation/' + requestToken,
  headers: {
    "Accept": "application/json"
  },
  auth: {
    user: secretApiKey
  }
};`
```

**我们将运行这个请求，如果成功，只执行回调中的代码。**

```
`request(requestOptions, function(error, response, body) {
  if (response.statusCode === 200) {
      // Return rates
  } else {
    // Return an error when the request does not come from Snipcart.    
    cb("Only Snipcart can call this code!");
  }
});`
```

**所以我的整个函数现在看起来像这样:**

```
`var request = require('request');

module.exports = function (context, cb) {
  var requestToken = context.headers['x-snipcart-requesttoken'];
  var secretApiKey = context.secrets.snipcartApiKey;

  var requestOptions = {
    url: 'https://app.snipcart.com/api/requestvalidation/' + requestToken,
    headers: {
      "Accept": "application/json"
    },
    auth: {
      user: secretApiKey
    }
  };

  request(requestOptions, function(error, response, body) {
    if (response.statusCode === 200) {
      var orderDetails = context.body.content;
      var rates = [];

      var address = orderDetails.shippingAddress || order.billingAddress;

      if (address.country == "CA" && address.province == "QC") {
        rates.push({
          cost: 0,
          description: "Free shipping for Québec residents!"
        });
      }

      if (address.country == "CA" && address.province != "QC") {
        rates.push({
          cost: 10,
          description: "Shipping to Canada"
        });
      }

      if (address.country == "US") {
        rates.push({
          cost: 15,
          description: "Shipping to US"
        });
      }

      if (rates.length === 0) {
        rates.push({
          cost: 20,
          description: "Standard shipping"
        });
      }

      cb(null, { rates: rates });
    }
    else {
      cb("Only Snipcart can call this code!");
    }
  });
}`
```

#### **Webtask 上的结束语&无服务器方法**

**为运行 Snipcart 的静态站点设计这个小小的 Webtask 函数花了我不到两个小时。当然，我们可以专注于其他电子商务功能:处理 webhooks 以在 Snipcart 和外部会计系统之间建立桥梁，自动化数字商品交付，等等。**

**我真的相信有很多令人兴奋的无服务器用例开发者应该尝试用 Auth0 的 Webtask 来处理。任何与 API 的集成，或者委派更多长时间运行/消耗 CPU 的任务都是非常有意义的！**

**无服务器方法正在加速对我们作为开发人员的工作产生相当大的影响。正如我们现在看到的 RESTful APIs，许多服务将开始依赖他人开发或托管的功能。不久的将来，将会有越来越多的微服务托管在 Webtask & AWS Lambda 这样的环境中。尤其是加上像 Vue&jam stack 这样的[框架的兴起。](https://snipcart.com/blog/vue-js-seo-prerender-example)**

**所以我真诚地希望这篇文章能激励开发人员在不同的无服务器项目中利用 Webtask 的 FaaS 力量，不管是不是电子商务项目。当然，我非常有兴趣看看这样的设置。如果你有类似的东西要分享，或者你对这种方法有疑问，你可以发邮件到 geeks@snipcart.com 给我们。**

**喜欢这篇文章吗？花点时间？并 [*分享到 Twitter 上*](https://twitter.com/home?status=Using%20%40auth0%27s%20%40webtaskio%20FaaS%20on%20a%20%23serverless%20e-commerce%20tutorial%20http%3A//bit.ly/2e5vm8d%20%23webtask) *！***

***原载于* [*Snipcart 博客*](https://snipcart.com/blog/webtask-baas-serverless-tutorial) *和* [*本报快讯*](https://us5.list-manage.com/subscribe?u=c019ca88eb8179b7ffc41b12c&id=3e16e05ea2) *。***