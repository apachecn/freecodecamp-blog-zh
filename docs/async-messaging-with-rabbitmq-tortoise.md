# Node.js 中 RabbitMQ 和 Tortoise 的异步消息传递

> 原文：<https://www.freecodecamp.org/news/async-messaging-with-rabbitmq-tortoise/>

RabbitMQ 恰好是目前使用 AMQ 协议的最简单、性能最好的消息代理平台。在微服务架构中使用它可以转化为巨大的性能提升，以及可靠性的承诺。

在本指南中，我们将探索在 Node.js 中使用 RabbitMQ 的基础知识。

## **理论**

在最基本的层面上，理想情况下你会有两个不同的服务通过 Rabbit 相互交互——一个*发布者*和一个*订阅者*。

发布者通常将消息推送给 Rabbit，订阅者监听这些消息，并根据这些消息执行代码。

注意，它们可以同时存在——一个服务可以向 Rabbit 发布消息，也可以同时消费消息，这样就可以设计出真正强大的系统。

现在，发布者通常会将带有路由密钥的消息发布到一个叫做 T2 交换的地方。消费者监听同一个交换机上的*队列*，绑定到路由键。

从架构的角度来说，您的平台将使用一个 Rabbit exchange，不同种类的作业/服务将有它们自己的路由键和队列，以便发布-订阅有效地工作。

消息可以是字符串；它们也可以是本地对象——AMQP 客户机库负责将对象从一种语言转换成另一种语言。是的，这确实意味着服务可以用不同的语言编写，只要他们能够理解 AMQP。

## **入门**

我们将编造一个非常简单的例子，其中发布者脚本向 Rabbit 发布一条消息，包含一个 URL，而消费者脚本监听 Rabbit，获取发布的 URL，调用它并显示结果。你可以在 [Github](https://github.com/rudimk/freecodecamp-guides-rabbitmq-tortoise) 上找到完成的样品。

首先，让我们初始化一个 npm 项目:

`$ npm init`

你可以一直点击`Enter`并使用默认选项——或者你可以填充它们。

现在，让我们安装我们需要的包。我们将使用[乌龟](https://www.npmjs.com/package/tortoise)与 RabbitMQ 进行交互。我们还将使用 [node-cron](https://www.npmjs.com/package/node-cron) 来调度实际的消息发布。

`$ npm install --save tortoise node-cron`

现在你的`package.json`应该看起来很像这样:

```
{
  "name": "freecodecamp-guides-rabbitmq-tortoise",
  "version": "1.0.0",
  "description": "Sample code to accompany the FreeCodeCamp guide on async messaging with RabbitMQ and Tortoise.",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/rudimk/freecodecamp-guides-rabbitmq-tortoise.git"
  },
  "keywords": [
    "rabbitmq",
    "tortoise",
    "amqp"
  ],
  "author": "Rudraksh M.K.",
  "license": "MIT",
  "bugs": {
    "url": "https://github.com/rudimk/freecodecamp-guides-rabbitmq-tortoise/issues"
  },
  "homepage": "https://github.com/rudimk/freecodecamp-guides-rabbitmq-tortoise#readme",
  "dependencies": {
    "node-cron": "^1.2.1",
    "tortoise": "^1.0.1"
  }
}
```

现在我们都准备好了。让我们先创建一个发布者。

```
const Tortoise = require('tortoise')
const cron = require('node-cron')

const tortoise = new Tortoise(`amqp://rudimk:YouKnowWhat@$localhost:5672`)
```

在导入`tortoise`和`node-cron`之后，我们已经开始初始化到 RabbitMQ 的连接。接下来，让我们编写一个快速而肮脏的函数，向 Rabbit 发布消息:

```
function scheduleMessage(){
    let payload = {url: 'https://randomuser.me/api'}
    tortoise
    .exchange('random-user-exchange', 'direct', { durable:false })
    .publish('random-user-key', payload)
}
```

这很简单。我们已经定义了一个字典，其中包含一个到 [RandomUser.me](https://randomuser.me/) API 的 URL，然后这个 URL 被发布到 RabbitMQ 上的`random-user-exchange`交易所，带有`random-user-key`路由键。

如前所述，路由键决定了谁可以使用消息。现在，让我们编写一个调度规则，每 60 秒发布一次该消息。

```
cron.schedule('60 * * * * *', scheduleMessage)
```

我们的出版商准备好了！但是没有一个消费者去实际消费这些消息真的是不行！但是首先，我们需要一个库来调用这些消息中的 URL。我个人用`superagent` : `npm install --save superagent`。

现在，在`consumer.js`:

```
const Tortoise = require('tortoise')
const superagent = require('superagent')

const tortoise = new Tortoise(`amqp://rudimk:YouKnowWhat@$localhost:5672`)
```

接下来，让我们编写一个调用 URL 并显示结果的异步函数:

```
async function getURL(url){
	let response = await superagent.get(url)
	return response.body
}
```

编写代码以实际使用消息的时间:

```
tortoise
.queue('random-user-queue', { durable: false })
// Add as many bindings as needed 
.exchange('random-user-exchange', 'direct', 'random-user-key', { durable: false })
.prefetch(1)
.subscribe(function(msg, ack, nack) {
  // Handle 
  let payload = JSON.parse(msg)
  getURL(payload['url']).then((response) => {
    console.log('Job result: ', response)
  })
  ack() // or nack()
})
```

在这里，我们已经告诉`tortoise`去听`random-user-queue`，它被绑定到`random-user-exchange`上的`random-user-key`。一旦收到消息，就从`msg`中检索有效负载，并将其传递给`getURL`函数，该函数又从 RandomUser API 返回一个带有所需 JSON 响应的承诺。

## **结论**

使用 RabbitMQ 进行消息传递的简单性是无与伦比的，只需几行代码就可以非常容易地得出非常复杂的微服务模式。

最好的一点是消息传递背后的逻辑并没有跨语言改变——Crystal、Go、Python 或 Ruby 与 Rabbit 的工作方式都差不多。这意味着您可以用不同的语言编写服务，所有服务都可以毫不费力地相互通信，使您能够使用最适合的语言。