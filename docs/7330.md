# 使用 AWS Lambda 计划松弛消息

> 原文：<https://www.freecodecamp.org/news/scheduling-slack-messages-using-aws-lambda-e56a8eb22818/>

迁移到无服务器会带来很多问题。如何完成一些非无服务器的任务，比如无服务器应用程序中的 cronjob？

假设您有一个小型 Slack 应用程序，它将黑客新闻中排名前五的故事发送到您的 Slack 频道。在某个时候，你决定关闭运行应用程序的服务器，但你仍然想接收故事。AWS Lambda 的无服务器似乎很酷。但是如何在特定的时间触发 AWS Lambda 函数呢？

![yxY9HN0WQ-feWbL9t3GwVrMZwqwQn6pfIqYo](img/a50440d8b5c494bdb362143d99cd77f4.png)

All diagrams are created using [SimpleDiagrams 4](https://www.simplediagrams.com) app

如果你不熟悉，无服务器是一种在云基础设施上部署和运行应用程序的方法，基于按使用付费，不需要租赁或购买服务器。要了解更多关于无服务器以及它如何与 AWS 一起工作的信息，请参见本指南。

您可以使用各种 AWS 服务触发 AWS Lambda 函数，例如用于 API 的 API Gateway 和用于文件的 S3。有关服务的完整列表，请参见此处的文档[。可用的触发器之一是](https://docs.aws.amazon.com/lambda/latest/dg/invoking-lambda-function.html) [AWS CloudWatch 事件](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/WhatIsCloudWatchEvents.html)。

等等，CloudWatch 不是做日志的吗？

嗯，确实是。但似乎 AWS 的某人是 Jekyll 博士和 Hyde 先生的超级粉丝，在某些情况下，同一个名字背后隐藏着几个不同的服务。

除了提供日志之外，Amazon CloudWatch 还有一些事件，这些事件提供了描述 AWS 资源变化的近乎实时的系统事件流。事件还可以使用 cron 或 rate 表达式安排自动操作。答对了。

### 应用流程

该应用程序将如何处理 CloudWatch 事件？

您需要使用 [cron 语法](https://en.wikipedia.org/wiki/Cron)或速率表达式(即 5 分钟)。然后，CloudWatch 事件在配置的时间间隔触发 AWS Lambda 功能。在你的 AWS Lambda 函数中，你从黑客新闻 API 获取前五篇文章，并使用[传入 Webhooks](https://api.slack.com/incoming-webhooks) 将它们发布到 Slack。

你可以在下图中看到流程。

![nRLfCXLIjaUsXISxyK-qQqFLbq6iJGLNySwJ](img/deca899d11af3b0c04a604b18b324453.png)

The flow of the serverless scheduled Slack messages

这听起来很简单吧？让我们看看它在实践中是如何工作的。

### 发送预定消息

在我们开始之前，为了能够跟随本教程，您需要有一个 AWS 帐户，并且需要安装 AWS CLI 和 Node.js (v6+)。你可以在这里获得 AWS CLI [。](https://aws.amazon.com/cli/)

您还需要配置一个 Slack Incoming Webhook。为此，请遵循本教程[。在本教程的最后，您将获得 webhook URL。请保存该 URL，因为您很快就会用到它。去吧，去吧，我会在这里等你⏳](https://api.slack.com/tutorials/slack-apps-hello-world)

好了，时间到了！让我们从有趣的部分开始。

首先，创建一个新文件夹，并在其中启动一个新的 Node.js 项目(可以使用`npm init -y`命令)。

因为，您需要发送一些 HTTP 请求，安装来自 NPM 的[最小请求承诺](https://www.npmjs.com/package/minimal-request-promise)模块作为依赖项。为此，请运行以下命令:

```
npm install minimal-request-promise --save
```

最小请求承诺是一个小的 Node.js 模块，它只是将本地 HTTP 和 HTTPS 模块包装到 JavaScript 承诺中。

现在依赖关系已经准备好了，让我们看看下一张图中我们将使用的项目结构。

![tth9sqtLCdMEiMc8Bh2u5PuiEiRom5N4N1eb](img/8f6abdfd2a625f64657026448f7b193e.png)

Folder structure of your project

尽管代码很简单，我们还是会把它分成几个小文件来简化测试(更多信息请参见[六角形架构介绍](http://alistair.cockburn.us/Hexagonal+architecture))。如上图所示，您的代码包含以下文件:

*   `index.js`-Lambda 函数的初始文件，调用其他两个文件并响应 CloudWatch 事件。
*   `src/get-top-hackernews-stories.js` -一个从黑客新闻中获得五个头条新闻的文件。
*   `src/send-slack-message.js` -格式化并发送 Slack 消息的文件。

让我们从初始文件开始。这个文件只需要另外两个文件并调用`getTopHackerNewsStories`和`sendSlackMessage`函数。当两个函数都准备好了，或者发生了错误，它就对触发器作出响应(CloudWatch 事件)。

您的`index.js`文件应该类似于下面的代码清单。

为了可读性，它不包含事件验证，而事件验证应该出现在产品代码中。

```
'use strict'
```

```
const getTopHackerNewsStories = require('./src/get-top-hackernews-stories')
const sendSlackMessage = require('./src/send-slack-message')
```

```
function scheduledSlackMessage(event, context, callback) {  getTopHackerNewsStories()    .then(stories => sendSlackMessage(stories))    .then(() => callback(null))    .catch(callback)}
```

```
exports.handler = scheduledSlackMessage
```

两个函数中的第一个`getTopHackerNewsStories`，向[黑客新闻 API](https://github.com/HackerNews/API) 发出 HTTP 请求(不需要认证)。当 API 返回一个故事 ID 列表时，您需要获得前五个 ID，并为每个 ID 发送一个 HTTP 请求，以获得故事的细节。最后，您需要解析响应体(因为最小请求承诺并没有在幕后这么做)并返回结果。

您的`get-top-hackernews-stories.js`文件应该类似于下一个代码清单。

```
'use strict'
```

```
const rp = require('minimal-request-promise')
```

```
function getTopNews() {  return rp.get('https://hacker-news.firebaseio.com/v0/topstories.json', {    'Content-Type': 'application/json'  })    .then(response => {      const storyIds = JSON.parse(response.body)
```

```
 return Promise.all(        storyIds.slice(0, 5)          .map(id => {            return rp.get(`https://hacker-news.firebaseio.com/v0/item/${id}.json`, {              'Content-Type': 'application/json'            })              .then(response => JSON.parse(response.body))          })      )    })}
```

```
module.exports = getTopNews
```

当您获得故事时，`sendSlackMessage`函数格式化消息并向 Slack Incoming Webhook URL 发送另一个 HTTP 请求，如下面的代码清单所示。

我们不会对传入的 Webhook URL 进行硬编码，而是将其作为 AWS Lambda 环境变量进行传递。要了解更多关于环境变量和在无服务器应用中共享秘密的其他方式，请参见本指南。

```
'use strict'
```

```
const rp = require('minimal-request-promise')
```

```
function sendSlackMessage(news, url = process.env.SlackWebhookUrl) {  const body = JSON.stringify({    text: 'Following posts are trending on Hacker News:',    attachments: news.map(item => ({      'author_name': `${item.score} points by ${item.by}`,      title: item.title,      'title_link': item.url    }))  })
```

```
 return rp.post(url, {    headers: {      'Content-Type': 'application/json'    },    body: body  })}
```

```
module.exports = sendSlackMessage
```

现在代码已经准备好了，让我们部署应用程序并安排消息。

### 部署、配置和测试应用程序

我们将使用 [Claudia.js](https://claudiajs.com) 将我们的功能部署到 AWS Lambda。在我们继续之前，请确保您按照[本教程](https://claudiajs.com/tutorials/installing.html)安装克劳迪娅并配置 AWS 访问凭证。

此外，您需要在项目文件夹中创建`env.json`文件，以定义 Slack Webhook URL。这个文件应该与下一个代码清单有相似的内容。确保用配置 Slack 应用程序时收到的 URL 替换通用 URL。

```
{  "SlackWebhookUrl": "https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX"}
```

现在一切都准备好了，在您的终端中运行以下命令来部署您的应用程序:

```
claudia create --region eu-central-1 --handler index.handler --timeout 10 --set-env-from-json env.json
```

在此命令中，您可以执行以下操作:

*   定义将部署 Lambda 函数的区域。有关支持地区的完整列表，请参见[文档](https://docs.aws.amazon.com/general/latest/gr/rande.html#lambda_region)。
*   定义处理程序文件，它是入口点文件的相对路径，但是带有一个`.handler`而不是`.js`扩展名。
*   设置*超时*，因为默认的 AWS Lambda 是 3 秒，但是你需要做一些 HTTP 请求。为了安全起见，请将超时时间增加到至少 10 秒。
*   从您准备的 JSON 文件中设置环境变量。

几秒钟后，您将收到一个 JSON 响应，如下例所示。您还会在项目文件夹中看到`claudia.json`文件。

```
{  "lambda": {    "role": "scheduled-slack-messages-executor",    "name": "scheduled-slack-messages",    "region": "eu-central-1"  }}
```

这意味着你的 AWS Lambda 函数已经准备好了。

下一步是创建一个 CloudWatch 事件。假设您希望每天在 CET 时间上午 10 点收到一条消息，因为您的 cron 运行在 GMT 时区。您的 cron 命令应该如下所示:`cron(0 9 * * ? *)`。

要设置每天上午 10 点的事件，请从终端运行以下命令:

```
aws events put-rule --name hackerNewsDigest --schedule-expression 'cron(0 9 * * ? *)'
```

这个命令将输出规则 ARN，您需要保存它，因为您很快就会用到它。

Amazon 资源名称(ARNs)是 AWS 资源的唯一标识符。点击阅读更多关于 ARNs 的文档[。](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html)

既然您的 CloudWatch 事件已经准备好了，您需要允许它触发一个 Lambda 函数。为此，请从终端运行以下命令:

```
aws lambda add-permission \  --statement-id 'hackernews-scheduled-messages' \  --action 'lambda:InvokeFunction' \  --principal 'events.amazonaws.com' \  --source-arn ruleArn \  --function-name functionName \  --region region
```

在该命令中:

*   `ruleArn`是运行前面的命令后收到的云观察事件规则的 ARN。
*   `functionName`是来自`claudia.json`文件的函数名。
*   `region`是您的`claudia.json`文件中的区域。

您的命令将返回一个 JSON 响应。在响应中找到**资源**并复制λ**ARN**。它应该如下所示:

*   *arn:AWS:lambda:eu-central-1:123456789012:function:scheduled-slack-messages*

最后，您需要通过从终端运行以下命令来设置触发器:

```
aws events put-targets --rule hackerNewsDigest --targets '[{ "Id": "1", "Arn": "your Lambda ARN" }]'
```

就这样，您计划的时差事件准备好了。第二天，在 CET 时间上午 10 点，您应该会收到如下图所示的消息。

如果您等不到上午 10 点，并且想早点看到结果，那么从您的终端运行`claudia test-lambda`命令。请确保首先导航到您的项目文件夹。

![eqh0Xc8b2BzNL4H6nLBcOq8yGSwOaySx4fyX](img/79604e033eed22be9bff2ea2f327c671.png)

Message received in Slack

更多类似的文章即将发表。如果你想了解我的最新文章，或者你有一个你喜欢阅读的话题，在 twitter 上关注并联系我-[twitter.com/slobodan_](https://twitter.com/slobodan_)。

*一如既往，非常感谢我的朋友[亚历山大·西蒙维奇](https://twitter.com/simalexan)对这篇文章的帮助和反馈。*

> 所有插图都是使用 [SimpleDiagrams4](https://www.simplediagrams.com) 应用程序创建的。

如果你想从总体上了解更多关于无服务器应用的知识，可以看看“Node 和 Claudia.js 的无服务器应用”，这是我和 Aleksandar Simovic 为 Manning Publications 写的书。

[**【无服务器应用】与 Node 和 Claudia.js**](https://www.manning.com/books/serverless-apps-with-node-and-claudiajs)
[*第一个流行语:无服务器计算。AWS Lambda。API 网关。Node.js .微服务。云托管功能…*www.manning.com](https://www.manning.com/books/serverless-apps-with-node-and-claudiajs)

这本书将教你如何使用 Node 和 Claudia.js 构建和调试真实世界的无服务器 API(包括数据库、认证和测试)。它还涵盖了将服务器上运行的现有应用程序迁移到无服务器应用程序，如何为 Facebook Messenger 和 SMS(使用 Twilio)构建聊天机器人，以及 Alexa 技能。