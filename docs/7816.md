# 使用无服务器、步进功能和堆栈存储交换构建社区注册应用程序——第…

> 原文：<https://www.freecodecamp.org/news/building-a-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-6efb9c102b0a/>

作者:德米特里·齐明

# 使用无服务器、步进功能和堆栈存储交换构建社区注册应用程序——第 3 集

使用[无服务器框架](https://serverless.com/framework/)和来自 [StackStorm Exchange](https://exchange.stackstorm.org) 开源目录的现成功能，在 AWS 上构建一个真实世界的无服务器应用程序。

[第一集](https://medium.com/@dzimine/tutorial-building-a-community-on-boarding-app-with-serverless-stepfunctions-and-stackstorm-b2f7cf2cc419) | [第二集](https://medium.com/@dzimine/building-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-2-b1efeb1b9bd6) |第三集| [第四集](https://medium.com/@dzimine/building-a-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-7c5f0e93dd6)

我们在第三集。快速回顾:

*   在[第一集](https://medium.com/@dzimine/tutorial-building-a-community-on-boarding-app-with-serverless-stepfunctions-and-stackstorm-b2f7cf2cc419)中，我描述了我们正在构建的应用程序，引导你建立开发环境和创建一个无服务器项目，并展示了如何用[无服务器框架](https://serverless.com/framework)从 [StackStorm Exchange](https://exchange.stackstorm.org) 动作构建你的第一个 Lambda 函数。
*   在[第二集](https://medium.com/@dzimine/building-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-2-b1efeb1b9bd6)中，我们添加了更多的动作:一个原生 Lambda 将用户信息记录到 DynamoDB，另一个来自 StackStorm Exchange 的动作调用 ActiveCampaign CRM 系统。您学习了更多的`serverless.yml`语法，并用 Lambda 函数实践了开发工作流。

在第三集中，我将展示如何使用 AWS StepFunction 将操作连接到工作流中。

你可以从 [GitHub](https://github.com/dzimine/slack-signup-serverless-stormless/tree/DZ/3-add-stepfunction) 获得这一集的最终代码。

### 接线功能和步进功能

既然我们的构建模块——Lambda 函数——都已经排列好了，是时候把它们串起来了。一个 [AWS StepFunction](https://aws.amazon.com/step-functions/) 将定义调用的顺序，维护注册工作流的状态，并在步骤之间传送数据。我就用无服务器冠军@horike37 的`[serverless-step-functions](https://github.com/horike37/serverless-step-functions)`插件，给他一颗心:

让我们忙起来。安装插件:

```
npm install --save-dev serverless-step-functions
```

将插件添加到`serverless.yml`文件中:

```
plugins:  - serverless-plugin-stackstorm  - serverless-step-functions
```

阶跃函数定义将需要我的`accountID`。因为这是我想留给自己的东西，所以我将它添加到了`env.yml`，现在看起来是这样的:

```
# ./env.yml# Don't commit to Github :)
```

```
slack:  admin_token: "xoxs-111111111111-..."  organization: "your-team"active_campaign:  url: "https://YOUR-COMPANY.api-us1.com"  api_key: "1234567a9012a12345z12aa2aaa..."private:  accountId: "000000000000"
```

回到`serverless.yml`，添加以下两个程序块:

```
# ./serverless.yml......custom:  private: ${file(env.yml):private}  stage: ${opt:stage, self:provider.stage}  region: ${opt:region, self:provider.region}
```

```
stepFunctions:  stateMachines:    signup:      events:        - http:            path: signup            method: POST            cors: true      definition: ${file(stepfunction.yml)}
```

在`custom`块中，我从`env.yml`中的`private`键分配了`private`对象。我还为`stage`和`region`定义了变量，以便从 CLI 选项(如果提供的话)中选择值，或者默认为当前的 AWS 设置。

`stepFunctions`块在这里定义——你已经猜到了——阶跃函数。我的叫`signup`。

这里的`events`部分所做的正是`events`部分在函数定义中所做的:它为从 AWS 外部调用 StepFunction 配置一个 API 网关端点。我们稍后将使用它从 Web 表单调用后端。

在`serverless.yml`中，`definition`可以写成 YAML，但是我更喜欢从一个单独的文件中包含它，保持逻辑与配置分离。这是:

StepFunction 定义是用亚马逊国家语言编写的。该规范很短，写得很好，值得一读。使用 YAML 而不是 JSON 是这个插件的一个不错的好处——它读起来更好并且允许评论。但是如果你想要 JSON——没问题，请自便。

*   `Resource`指 ARN 的λ函数。我使用我们之前定义的变量来构建 ARNs 匹配账户 ID、区域和阶段，函数名:`arn:aws:lambda:${self:custom.region}:${self:custom.private.accountId}:function:${self:service}-${self:custom.stage}-RecordDB`
*   `ResultPath`用于在步骤之间传递数据。默认情况下，阶跃函数在“需要知道”的基础上工作:下游步骤只接收直接上游步骤的输出。如果你觉得合乎逻辑，再想一想:如果只有 RecordDB 接收工作流输入，RecordAC 和 InviteSlack 将如何获取？RecordDB 可能只是返回“200 OK”，而不是`email`。改变函数的代码来返回它们的输入会使它们[不恰当地亲密](https://refactoring.guru/smells/inappropriate-intimacy)。诀窍是使用`ResultPath`将函数输出写在一个特定于函数的键下，比如`ResultPath: $results.RecordDB`。这将在下游 Lambda 步骤的步骤输出中保留初始工作流输入，同时追加每个 Lambda 的输出。像这样:

```
{  "body": {    "name": "Vasili Terkin",    "email": "dmitri.zimine+terkin@gmail.com",    "first_name": "Vasili",    "last_name": "Terkin"  },  "results": {    "RecordDB": {      "statusCode": 200    },    "RecordAC": ...    ...  }}
```

要完全掌握它，请阅读规范中的[“输入和输出”部分](https://states-language.net/spec.html#filters)。Ansd 用来自[玛西娅·维拉巴](https://www.freecodecamp.org/news/building-a-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-6efb9c102b0a/undefined)的《 [AWS 阶跃函数教程](https://foobar123.com/aws-step-functions-tutorial-76b9f5a7b9c8)》的视频自娱自乐。

**PRO TIP:** 我让工作流按顺序进行，以演示数据传递技巧。更合适的做法是并行运行所有三个步骤:这样更快、更有弹性:一个步骤的失败不会阻止其他函数调用。继续将阶跃函数改为并行。

就是这样。是时候试试了。部署、调用、检查日志。

你们这些 CURL 粉丝知道如何使用我们 StepFunction 的新 API 网关端点。如果你忘记了终点，`sls info`来救援。我再用 [httpie](https://httpie.org) 炫耀一下:

```
# DON'T COPY! Use YOUR ENDPOINT!
```

```
http POST https://YOUR.ENDPOINT.amazonaws.com/dev/signup \body:='{"email":"santa@mad.russian.xmas.com", "first_name":"Santa", "last_name": "Claus"}'
```

好的，`http`或`curl`，无论哪种方式，它都会返回 StepFunction 执行 ARN，这样我们就可以检查它，看看我们的 StepFunction 是如何执行的。我们如何检查它？恐怕你得打开浏览器，登录你的 AWS 控制台。如果您想先使用 AWS CLI，没关系，不要说我没有向您展示如何使用:

```
aws stepfunctions describe-execution --execution-arn arn:aws:states:us-east-1:00000000000:execution:SignupStepFunctionsStateMac-seo9CrijATLU:cbeda709-e530-11e7-86d3-49cbe4261318 --output json{    "status": "FAILED",     "startDate": 1513738340.18,     "name": "cbeda709-e530-11e7-86d3-49cbe4261318",     "executionArn": "arn:aws:states:us-east-1:00000000000:execution:SignupStepFunctionsStateMac-seo9CrijATLU:cbeda709-e530-11e7-86d3-49cbe4261318",     "stateMachineArn": "arn:aws:states:us-east-1:00000000000:stateMachine:SignupStepFunctionsStateMac-seo9CrijATLU",     "stopDate": 1513738370.481,     "input": "{\"body\":{\"email\":\"santa@mad.russian.xmas.com\",\"first_name\":\"Santa\",\"last_name\":\"Claus\"}}"}
```

这是由于 RecordAC 函数超时而导致执行失败的输出。你能从输出中推断出这一点吗？这里唯一有价值的信息是`FAILED`。不开玩笑！我必须说 AWS 没有给 StepFunction 应有的爱。不在 CLI 中。如果你认为我错过了什么，检查[CLI 文档](http://docs.aws.amazon.com/cli/latest/reference/stepfunctions/index.html)，找到它并告诉我。

最气人的是 CLI 没有告诉我哪一步失败了。他们让我逐个调用每个 Lambda 上的日志。幸运的是我只有 3 个函数，如果有更多呢？

或者，打开浏览器，跳到 AWS 控制台。

![1*0xq8ywxkBv-oIe6b-6H9ug](img/2b73337ce5f61f6c308006102ba24ac4.png)

即使在那里，调试一些 Lambda 故障，比如超时，也是很棘手的。StepFunction 执行“异常”报告说`"The cause could not be determined because Lambda did not return an error type."`去 lambda 日志看看那里发生了什么。

在那里我找到了我希望在 StepFunction 异常中看到的那一行:

```
2017-12-20T04:21:44.298Z 4230a73b-e53d-11e7-be6b-bff82b9b3572 Task timed out after 6.00 seconds
```

**PRO 提示:**调试时，用`sls invoke stepf`调用 StepFunction:它创建执行，等待完成，将输出打印到终端。三个 AWS CLI 命令合二为一。

```
sls invoke stepf --name signup \--data  '{"body": {"email":"santa@mad.russian.xmas.com", "first_name":"Santa", "last_name": "Clause"}}'
```

您的 StepFunction 执行可能工作得很好——我们已经调整了超时。我带您走上这条调试之路，是为了体验 StepFunction 故障排除，不可否认有点苦。好的一面是，一旦调试成功，阶跃函数就能像丰田汽车一样可靠地运行。

作为一名后端开发人员，我很想在这里称之为 done。但是为了让它成为一个“完整的例子”,我们还需要一样东西。**Web 前端。**

今天就到此为止，把网页部分和结论留给下一集也是最后一集。

[**第四集**](https://medium.com/@dzimine/building-a-community-sign-up-app-with-serverless-stepfunctions-and-stackstorm-exchange-episode-7c5f0e93dd6) **:增加 Web 前端，反思和总结**

希望这能帮助你学到一些新东西，发现一些有趣的东西，或者引发一些好的想法。请在这里的评论中分享你的想法，或者发微博给我[@ dz 亚胺](https://twitter.com/dzimine)。