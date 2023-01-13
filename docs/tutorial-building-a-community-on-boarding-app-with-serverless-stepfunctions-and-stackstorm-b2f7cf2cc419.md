# 教程:用无服务器、阶跃函数和堆栈风暴构建一个社区车载应用程序…

> 原文：<https://www.freecodecamp.org/news/tutorial-building-a-community-on-boarding-app-with-serverless-stepfunctions-and-stackstorm-b2f7cf2cc419/>

作者:德米特里·齐明

# 使用无服务器、步进功能和堆栈存储交换构建社区注册应用程序

使用[无服务器框架](https://serverless.com/framework/)和来自 [StackStorm Exchange](https://exchange.stackstorm.org) 开源目录的现成功能，在 AWS 上构建一个真实世界的无服务器应用程序。

第一集| [第二集](https://medium.com/@dzimine/building-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-2-b1efeb1b9bd6) | [第三集](https://medium.com/@dzimine/building-a-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-6efb9c102b0a) | [第四集](https://medium.com/@dzimine/building-a-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-7c5f0e93dd6)

请继续阅读，如果你是:

*   一个使用[无服务器框架](https://serverless.com/framework/)的无服务器开发人员，他想从 StackStorm Exchange 开源目录中检查现成可用的功能，
*   一个住在 AWS 的用户，错过了 T2 stack storm Exchange T3 的广泛集成。
*   任何人都有 2 个小时的时间来学习一些比 [Hello-world 示例](https://serverless.com/framework/docs/providers/aws/examples/hello-world/)更复杂、更真实的东西。

如果你只有 8 分钟的空闲时间，浏览一下文本和例子，再花 30 秒浏览一下 StackStorm Exchange ,看看有什么潜力，然后把这篇文章加入书签，当你需要的时候再回来看。

#### 介绍

当我[用 Python、StepFunctions 和 Web 前端](https://medium.com/@dzimine/exploring-serverless-with-python-stepfunctions-and-web-front-end-8e0bf7203d4b)探索无服务器时，我错过了一个可重用集成的目录。比如用于 Azure Logic 应用程序的 [200 连接器](https://docs.microsoft.com/en-us/azure/connectors/apis-list#popular-connectors)。或者[stack storm](https://exchange.stackstorm.org)的 130 集成包。

当我们需要连接 Slack、吉拉、Stripe 或 Nest 时，我们是否可以跳过对它们的 API 和身份验证内部函数的深入研究，直接获取一个现成的函数？

现在我们可以做到这一点:StackStorm 刚刚[宣布了一个无服务器框架的插件](https://stackstorm.com/2017/12/14/stackstorm-exchange-goes-serverless)，它将 StackStorm 交换的集成变成了 AWS Lambda 函数。

在本教程中，我将展示如何在从头开始构建一个无服务器社区应用程序的背景下使用插件和交换集成。让我们把这变成对话和乐趣。

我假设既不熟悉[无服务器框架](https://serverless.com/framework/)也不熟悉 StackStorm。但是你应该知道如何编码，并且聪明地弥补我将不可避免地犯的错误和遗漏。

我们会慢慢来，有令人痛苦的细节，因此它将是四集。

在这第一集中，我将设置好一切并部署我的第一个 StackStorm 交换动作。

在下一集，我们将添加更多的动作。

在第三集中，我将用 AWS StepFunction 将它们连接在一起。

在第四集，我们将添加网络前端的反思和总结。每集大约需要一个小时。

准备好了吗？让我们摇滚吧。

### 应用程序

我们将构建一个社区入门应用程序。实际上，从零开始重建我们在 StackStorm 经营的那个。这就像 [SlackIn](http://rauchg.com/slackin/) 采用多步可定制的入职工作流程。该应用程序提供了一个注册 web 表单，它通过 API Gateway 将新的用户信息传递给 StepFunction 工作流，该工作流执行入职步骤。

在我的例子中，步骤是 1)邀请用户 Slack 2)在 ActiveCampaign CRM 工具中创建联系人记录，3)将用户记录放入内部 DynamoDB 表中。这是它的样子:

![1*QdErjJE8X0jg3SfR7QyBVQ](img/38d2791850fa6fd2fc0d1c5f8a45ca71.png)

你可以在 github 上找到之前的实现[，或者](https://github.com/dzimine/slack-signup-serverless)[使用它并加入 Slack 上的 StackStorm 社区](https://stackstorm.com/community-signup)以获得关于 StackStorm Exchange 集成的问题。

#### 做好准备

首先，您需要 AWS 帐户、NodeJS、Docker 和无服务器框架。还有懈怠！因为我们的第一个行动是邀请用户放松。

1.  确定[节点。JS](https://nodejs.org/) 已安装，版本为`8.4.0`或更高。
2.  [安装无服务器框架](https://serverless.com/framework/docs/providers/aws/guide/installation/)，并按照本指南为其设置 AWS 凭证[。](https://serverless.com/framework/docs/providers/aws/guide/credentials/)
3.  [安装对接器](https://docs.docker.com/engine/installation/)。该插件将其用于构建环境，以使 lambdas 二进制文件与 AWS 执行环境兼容，无论您使用什么操作系统进行开发。有一种方法可以让它在没有 Docker 的情况下工作，但是不要冒险。
4.  懈怠！我们的行动将需要管理员访问，并将使用未记录的 Slack API(这里是文档，双关语)来邀请新用户。最简单的方法就是[创建一个新团队](https://slack.com/get-started)。需要 4 分钟。Slack 不会介意——他们会向风投展示增长。
5.  一旦创建了工作区，就该获取认证令牌了。去 api.slack.com/custom-integrations/legacy-tokens。不要害怕“遗留警告”:本教程将在他们之前转换遗留。照他们说的做，拿到你的令牌。

**专业提示:**使用[这个快速破解](https://github.com/StackStorm-Exchange/stackstorm-slack#obtaining-auth-token)来获得并使用你自己用户的认证令牌。快得多，适合玩和调试。但是请千万千万不要用于生产！

### 创建一个项目，添加第一个操作

试着确定至少有些东西是有效的。`sls`是`serverless`的简称，无服务器框架 CLI。现在把你的咖啡放在一边，是时候创建一个项目了。有些人喜欢使用无服务器自带的模板:`sls create --template`。我更喜欢从头开始:

```
mkdir slack-signup-serverless-stormlesscd slack-signup-serverless-stormless
```

```
npm init
```

```
# Once you answer questions, the project is set up.
```

接下来，安装插入[堆栈风暴交换](https://exchange.stackstorm.org)动作的`[serverless-plugin-stackstorm](https://github.com/StackStorm/serverless-plugin-stackstorm)`。

```
npm install --save-dev serverless-plugin-stackstorm
```

…并创建一个最小的`serverless.ym`文件，以便`sls`命令可以选择插件:

现在，**创建第一个动作**。我将使用来自 StackStorm Exchange 的久经考验的[松弛包。你说，哪个行动？好吧，StackStorm Exchange 还没有聪明到可以显示 pack 的行动列表，但是`sls stackstorm`会拯救我们。](https://exchange.stackstorm.org/#slack)

`sls stackstorm info --pack slack`

我的天啊。有这么多！它们是什么？我想我需要一个公关来打印动作描述。与此同时，`| grep admin`会给我们带来我们需要的:`slack.users.admin.invite`。让我们查询动作的参数:

```
$ sls stackstorm info --action slack.users.admin.inviteslack.users.admin.invite ...... Send an invitation to join a Slack OrgParameters  attempts [integer]  ......... description is missing  channels [string]  .......... Channels to auto join.  email [string] (required) ... Email address to send invitation to.  first_name [string]  ........ Users first name  set_active [boolean]  ....... Should the user be active.  token [string]  ............. Slack API token.Config  action_token [string]  ...... Slack Action token.  admin [object]  ............. Admin-action specific settings.  post_message_action [object]   Post message action specific settings.  sensor [object]  ............ Sensor specific settings.
```

厉害！我们可以看到只有一个必需的参数，`email`，但是我将添加`first_name`以保持对话性。令牌可以作为参数或配置来传递。如果我选择使用配置，我以前的部落知识暗示`admin [object]`只需要`admin_token`。这正是我在您设置可宽延工作空间时要求您记住的。

**专业提示:**当我们还在打磨插件以暴露所有配置细节时，你可以通过探索`config.schema.yaml`文件中的 StackStorm Exchange pack 配置模式来找到它。例如，这里是我们松弛包的 [config.example.yaml](https://github.com/StackStorm-Exchange/stackstorm-slack/blob/master/config.schema.yaml#L47-L78) 。

现在，我们已经拥有了创建任何无服务器项目的核心所需的一切:`serverless.yml`。它来了:

这是学习一点无服务器的好时机。快速浏览[核心概念](https://serverless.com/framework/docs/providers/aws/guide/intro/)并标记 [Serverless.yml 参考](https://serverless.com/framework/docs/providers/aws/guide/serverless.yml/)。

我在第 9:12 行添加了`events`部分，这样我们就可以通过 AWS API 网关端点调用 REST 调用函数。无服务器框架将完成所有的网关魔术。

注意，这个默认配置指示 API Gateway 在`body`键下传递带有 POST 主体的 REST POST 调用([细节在这里](https://serverless.com/framework/docs/providers/aws/events/apigateway#simple-http-endpoint))。当我们发布`{"first_name": "Santa", "email": "santa@mad.russian.xmas.com"}`时，传递给 Lambda 的事件是:

```
..."body": {    "first_name": "Santa",     "email": "santa@mad.russian.xmas.com"}
```

了解输入数据结构对于将它映射到动作输入参数非常重要。很直观:`input`代表 [AWS Lambda 编程模型](http://docs.aws.amazon.com/lambda/latest/dg/python-programming-model-handler-types.html)的`event`参数(BTW 应该叫它`event`？用 PR 投票！).

[Jinja](http://jinja.pocoo.org/) 用于映射输入；我们的 JavaScript 朋友不太熟悉这个常见的 Python 工具，他们发现在简单的情况下它足够直观；堆栈溢出充满了神奇的金贾技巧。

在`serverless.yml`的第 16:17 行，我将两个参数从输入体映射到期望的动作输入参数。或者，您也可以从操作结果形成一个函数输出。现在我将保持它的简单(第 20:22 行),并为以后保存更多的技巧。

为了保持配置的独立性，我创建了一个文件`env.yml`,并将我的配置参数放入其中:

```
# ./env.yml# WARNING: Don't commit to Github :)slack:  admin_token: "xoxs-111111111111-..."  organization: "my-team"
```

然后我在`serverless.yml`里是这样用的:`admin: ${file(env.yml):slack}`。注意这个语法是如何将对象从文件中的键放到`serverless.yml`中的键的。

好了，就这样吧！函数准备用`sls deploy`飞到 AWS。但是我认为**慢**。循序渐进。首先，我会在本地打包。

```
sls package
```

第一次需要很长时间，因为这是插件安装其运行时依赖项的时间。它从中心提取 Docker 图像。它安装了 StackStorm runners——知道如何运行 stack storm 交换包的代码。它从交换中取出`slack`包。它安装`slack`包 python 依赖项。它为我们做了很多工作，而且需要时间。好消息:这只是第一次。

哦，我说过你一定有关系吗？还是我们假设互联网连接是一种基本商品，就像呼吸空气和电力一样？至少在 FCC 废除网络中立之前？所以，是的，你需要互联网连接才能生活在没有服务器的世界里。

现在让我们在本地运行它。

```
sls stackstorm docker run --function InviteSlack --verbose \--data \'{"body": {"first_name": "Santa", "email": "santa@mad.russian.xmas.com"}}'
```

本地运行发生在容器中——您将看到`Spin Docker container to run a function` CLI 输出。这需要更长的时间，但是可以确保执行环境与 AWS lambda 非常匹配，所以安全总比遗憾好。

当我调试输入和输出参数转换时，我可能不想调用实际的函数，就像在松弛速率限制 API 的情况下一样。使用`--passthrough`参数，告诉插件进行预演并跳过动作调用。

现在我们真的准备好了。让我们将该功能部署到 AWS，并“无服务器”地运行它。

```
sls deploy
```

这需要一些时间——现在它是无服务器的(老实说，我们的包有点臃肿，耐心！插件开发者目前正忙于解决其他问题，我们将尽快优化)

**专业提示:**如果此时出现问题，很可能是您的 AWS 设置有问题。回到“准备工作，步骤 2”。阅读[无服务器安装](https://serverless.com/framework/docs/providers/aws/guide/installation/)文档。谷歌，栈溢出，无服务器 [Gitter 频道](https://gitter.im/serverless/serverless)或者[论坛](https://forum.serverless.com/)。

您可能很想知道它在 AWS 控制台中的样子。如果你内心的专业人士在说“不，你应该保持冷静，使用 CLI”，不要听。去放纵一下自己，打开浏览器，好好看看你的 Lambda。在那里，您还可以检查`sls`为您创建的 API 网关端点。

但是为了测试它，我们将回到终端。下面是如何用`sls`运行你的 Lambda:

```
sls invoke --function InviteSlack --log --data '{"body": {"first_name": "Santa", "email": "santa@mad.russian.xmas.com"}}'
```

最后，让我们发布到 API 端点。端点打印在`sls deploy`的末尾，您应该已经注意到了，但是如果您没有注意到也没关系:您总是可以通过键入`sls info`得到它。

爱你的人用它来发帖吧；务必设置`Content-Type=application/json`标题。我——我将用 [httpie](https://httpie.org/) 来展示一下，也就是人类的 CURL:

```
# DON'T copy-paste! Use YOUR endpoint!
```

```
http --json POST  https://YOUR-ENDPOINT.amazonaws.com/dev/invite \email=test@example.com first_name=Dmitri
```

怎么样了？一切正常，至少对我来说是这样。让我们启动另一个最有用的`sls`命令来检查 CloudWatch 日志:

```
sls logs --function InviteSlack
```

成功！以及第一集的结尾。现在够了:圣诞节到了，慢慢来，享受吧！

到目前为止，代码示例位于 Github 上的 [1-add-first-action](https://github.com/dzimine/slack-signup-serverless-stormless/tree/DZ/1-add-first-action) 。

[**第二集**](https://medium.com/@dzimine/building-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-2-b1efeb1b9bd6) : **增加更多动作**

希望这能帮助你学到一些新东西，发现一些有趣的东西，或者引发一些好的想法。请在这里的评论中分享你的想法，或者发微博给我[@ dz 亚胺](https://twitter.com/dzimine)。