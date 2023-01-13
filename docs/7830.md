# 使用无服务器、步进功能和堆栈存储交换构建社区注册应用程序—第 2 集

> 原文：<https://www.freecodecamp.org/news/building-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-2-b1efeb1b9bd6/>

作者:德米特里·齐明

# 使用无服务器、步进功能和堆栈存储交换构建社区注册应用程序—第 2 集

![1*-0KcbVF9M6t_UvLpObeNYw](img/76e6f04120218f943a5b29951af4f8d5.png)

使用[无服务器框架](https://serverless.com/framework/)和来自 [StackStorm Exchange](https://exchange.stackstorm.org) 开源目录的现成功能，在 AWS 上构建一个真实世界的无服务器应用程序。

[第一集](https://medium.com/@dzimine/tutorial-building-a-community-on-boarding-app-with-serverless-stepfunctions-and-stackstorm-b2f7cf2cc419) |第二集| [第三集](https://medium.com/@dzimine/building-a-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-6efb9c102b0a) | [第四集](https://medium.com/@dzimine/building-a-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-7c5f0e93dd6)

在第二集中，我将添加两个动作:在 ActiveCampaign 中创建一个联系人，在 Dynamo DB 中记录一个用户。这一集的完整代码在 Github 的 [2-add-more-actions 分支](https://github.com/dzimine/slack-signup-serverless-stormless/tree/DZ/2-add-more-actions)上。

### 添加更多操作

要继续下一步，您需要一个 [ActiveCampaign](https://www.activecampaign.com) 帐户。不用担心:只需要 2 分钟——我已经试过了——他们没有问我要信用卡或任何愚蠢的东西。只发邮件。进入后，进入“我的设置”，选择“开发者”。

注意“API 访问”部分的`URL`和`Key`字段。像这样把它们加入`env.yml`:

```
# Copy to env.yml and replace with your values.# Don't commit to Github :)slack:  admin_token: "xoxs-111111111111-111111111111-..."  organization: "your-team"active_campaign:  url: "https://YOUR-COMPANY.api-us1.com"  api_key: "1234567a9012a12345z12aa2..."
```

**专业提示:**如果你没有心情参加活动或注册，就用类似 [Mocky](https://www.mocky.io/) 或 [mockable.io](https://www.mocky.io/) 的东西模拟 API 端点，并相应地调整示例。为了加分，在您的`serverless.yml`中创建另一个具有 API 网关端点的无服务器函数，并使用它来模拟 ActiveCampaign API 调用。

现在我准备添加一个[活动](https://exchange.stackstorm.org/#activecampaign) lambda 函数。与[第一集](https://medium.com/@dzimine/tutorial-building-a-community-on-boarding-app-with-serverless-stepfunctions-and-stackstorm-b2f7cf2cc419)相同的程序:从在一个包中找到一个新动作`sls stackstorm info --pack activecampaign`开始(提示:你正在寻找的动作是`contact_sync`，它添加并更新了一个联系人)。用`sls stackstorm info --action activecampaign.contact_sync`检查动作:

一堆参数，但是除了配置之外，只有`email`是必需的。我也想用`first_name`和`last_name`。`serverless.yml`中的函数定义如下:

```
RecordAC:    timeout: 10    memorySize: 128    stackstorm:      action: activecampaign.contact_add      input:        email: "{{ input.body.email }}"        first_name: "{{ input.body.first_name }}"        last_name: "{{ input.body.last_name }}"      config: ${file(env.yml):activecampaign}
```

我不得不提高超时，因为 ActiveCampaign API 在圣诞节期间花费的时间比 Lambda 默认的 6 秒要长。但是我可以通过降低默认 512Mb 的内存使用来回收调用成本。这次我懒得把它暴露给 AWS API Gateway——我们可以方便地用`sls`测试它。

该功能已准备好飞往 AWS。我们可以用`sls deploy`立即完成，但让我们再慢一点，重复部署步骤，就像本教程的[第 1 集](https://medium.com/@dzimine/tutorial-building-a-community-on-boarding-app-with-serverless-stepfunctions-and-stackstorm-b2f7cf2cc419)中一样，将工作流程铭记在心:

1.  构建并打包包:

```
sls package
```

2.本地测试(注意我使用`--passthrough`只是为了测试转换，移除它来进行实际调用):

```
sls  stackstorm docker run --function RecordAC \--passthrough \--verbose --data '{"body":{"email":"santa@mad.russian.xmas.com", "first_name":"Santa", "last_name": "Claus"}}'
```

3.部署到 AWS:

```
sls deploy
```

4.使用`sls invoke`在 AWS 上测试:

```
sls invoke --function RecordAC --logs \ --data '{"body":{"email":"santa@mad.russian.xmas.com", "first_name":"Santa", "last_name": "Claus"}}'
```

5.检查日志:

```
sls logs --function  RecordAC
```

而且我查了一下，确实有效:圣诞老人出现在 ActiveCampaign 的联系人列表里。我怎么知道是我们的 Lambda？因为我不再相信没有我们的 Lambda 函数，圣诞老人真的会订阅社区。

**专业提示:**如果你发现了一个 bug 或者想要你正在使用的 [StackStorm Exchange](https://exchagne.stackstorm.org) 的软件包中的一个特性，你可以当场修复它。包是在`./~st2/packs`下克隆的。在那里找到您的操作，修改代码，并使用本地运行进行调试和验证。
将补丁推回交易所的巨大好处是:每个包都已经是 git 的克隆版，自然很容易对社区做出贡献。

让我们添加最后一个动作 RecordDB，它将联系信息写入 DynamoDB 表。我本可以使用 StackStorm Exchange 上的 [AWS 包中的`aws.dynamodb_put_item`动作——这个包被大量使用并且维护得很好。但是我决定让它成为原生 Lambda:它只有 30 行代码，没有额外的 Python 依赖，因为 Boto 库已经在 AWS Lambda 环境中了。](https://exchange.stackstorm.org/#aws)

该函数的代码进入`./record_db/handler.py`:

包含所有三个动作的最终 `serverless.yml`现在看起来像这样:

哇！文件翻了一倍。这个函数本身只有 2 行(35:36)，但是增加了很多有趣的内容。让我们回顾一下:

1.  第 9 行—为避免区域和环境之间的冲突而生成的表名。
2.  第 10:13 行—创建 IAM 角色，让函数访问 DynamoDB 表。
3.  第 39:56 行—定义表格的 AWS 云信息模板。

最后一点引出了一个重要的观察:[无服务器框架](https://www.freecodecamp.org/news/building-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-2-b1efeb1b9bd6/(https://serverless.com/framework))主要简化了无服务器的 FaaS 部分。但是因为无服务器不仅仅是 FaaS，您会发现自己编写了大量的云信息来提供其他服务。无服务器框架为特定于提供商的资源留出了空间。要在 AWS 上认真无服务器，掌握 CloudFormation。寻找“与提供商无关的无服务器”？仔细检查你的假设。

此外，我移动了`memorySize`和`timeout`以应用于所有函数(第 6:7 行)。API 网关端点从 InviteSlack 函数中消失了:它在第一集中充当了演示角色，但现在我们已经学会了直接测试 Lambda 函数。我们稍后将返回到 API Gateway 来调用最终的 StepFunction 端到端工作流。

让我们让 RecordDB 函数升上天空并运行起来。
部署、调用、日志。重复冲洗。

```
# Deploysls deploy ...
```

```
# Invokesls invoke --function RecordDB --logs --data '{"body":{"email":"santa@mad.russian.xmas.com", "first_name":"Santa", "last_name": "Claus"}}'...
```

```
# Check logs.sls logs --function RecordDB.
```

现在，这三个动作都在这里，等待用 StepFunction 连接成最终的工作流。但我只是被提醒不要成为吝啬鬼先生:现在是假期，让我们放松一下，把它推迟到下一集。在那之前，保持高昂的情绪！

本集的完整代码示例位于 Github 的 [2-add-more-actions](https://github.com/dzimine/slack-signup-serverless-stormless/tree/DZ/2-add-more-actions) 分支。

[**第三集**](https://medium.com/@dzimine/building-a-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-6efb9c102b0a) **:用 StepFunction 连接动作**

希望这能帮助你学到一些新东西，发现一些有趣的东西，或者引发一些好的想法。请在这里的评论中分享你的想法，或者发微博给我[@ dz 亚胺](https://twitter.com/dzimine)。