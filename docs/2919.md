# 如何使用 Github 动作调用 Webhooks 并从你的 PRs 统治互联网

> 原文：<https://www.freecodecamp.org/news/how-to-use-github-actions-to-call-webhooks/>

Github Actions 是大家最喜欢的代码工具中的一个新特性。虽然它们需要一点时间来适应，但对于 CI(持续集成)和对您的拉取请求的其他检查来说，它们是非常强大的工具。

在这里，我将讨论如何使用 Github 动作来调用 webhooks。如你所知，一旦你可以调用一个网络钩子，互联网就是你的了。

## 为什么不是老式的网钩？

现在，你可能会说:“Github 已经有 webhooks 了，为什么还要麻烦动作呢？”答案很简单:*版本控制*。

如果您和其他人一起开发您的代码库，您希望能够跟踪配置的变更是如何发生的，以及谁对此负责。

有了基本的 Github 设置，你就不知道这些东西了——有人设置并配置了一个 webhook。也许有一天失败了，然后呢？

有了 Github Actions，你不必离开你的文本编辑器去查看当你推送你的代码时会发生什么。

另一个原因是，从 Github Actions 可以做很多事情，下面是一些想法。我最近的一个项目需要 webhooks，但是有运行测试、收集代码覆盖率、林挺等功能。

鉴于我们中有如此多的人每天都在使用 Github，熟悉一下这个新工具也无妨。

# 你的第一个行动

那么，我们开始吧。你需要做的第一件事是创建一个`.github > workflows`文件夹。在里面，我们会把我们的行动。你把这个文件叫做什么并不重要——GitHub 会把你放在这个特殊文件夹中的所有动作都提取出来。

以下是我的 webhook 文件的内容。我有一个“测试学究”API 端点，它检查我的 PR 文件，如果我没有编写任何测试，就会留下一个学究式的注释。

```
# This is a basic workflow that is manually triggered
name: Test reminder

# Controls when the action will run. Workflow runs when manually triggered using the UI or API.
on:
  # Trigger the workflow on push or pull request,
  # but only for the master branch
  pull_request:
    branches: [ main ]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  test_commentary:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
    # Runs a single command using the runners shell
    - name: Webhook
      uses: distributhor/workflow-webhook
      with:
        url: "http://amberisgreat.ngrok.io/api/test_pedant"
        json: '{ "repository": "${{github.event.repository.full_name}}", "number": "${{github.event.number}}", "created_at": "${{github.event.pull_request.created_at}}", "updated_at": "${{github.event.pull_request.updated_at}}" }' 
```

从上到下:

*   首先我们给这个动作起一个名字(`Test reminder`)。
*   然后我们指定希望它运行的时间(`on`)。不用选分支。如果您想让它用于所有的 PRs，只需执行`on: [pull_request]`。Github 有一个巨大的可以触发动作的[事件列表。](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#webhook-events)
*   `jobs`是工作列表。在我有`test_commentary`的地方，你可以放一把你工作的钥匙(它可以是任何东西)。你的工作可能在`ubuntu-latest`上运行，但是检查一下工作本身。一个作业可能有多个步骤。我的有一个。
*   给你的步伐一个`name`(这也可以是任何东西)
*   `uses`指代码存储的位置。(你愿意的话可以自己写动作！)
*   不同的动作需要不同的输入——这个动作使用`with`和输入键。

我正在使用来自“distributor”的 [webhook 函数。Github 还没有正式的 webhook 工作，但是当你读到这篇文章的时候，他们可能已经有了。](https://github.com/distributhor/workflow-webhook)

这个动作至少需要一个输入:`url`，这个动作要到达的端点。`json`是指你要发送的数据。

## Github 操作有效负载

在 Github Actions 文档中寻找这些信息有点棘手。

Github 动作的有效负载嵌套在`github.event`下。您还需要知道您从哪个动作推送数据——因为根据您引用的动作,`event`上有各种键可用(在有效负载文档中列为`action`)。

相关文件[此处](https://docs.github.com/en/developers/webhooks-and-events/webhook-events-and-payloads)。

我的库不会让我把整个`event`都塞进我的 web hook——这就是为什么你会看到键被特意拔出来。也许你写的动作会把整个`event`(然后你就可以跟我说说了！)

# Github 操作在...行为

这就是你需要的一切！只要你的 webhook 回复 200，你的动作就会得到一张绿色支票。

![Github Action running](img/7fb6940bee23512cad0bd6a8b4706692.png)

如果您的操作失败，您可以在“操作”选项卡中检查出了什么问题。我的失败是因为我的 ngrok 隧道不再运行了。

![Github Action failure](img/b8a519279473e191dcac821378810026.png)

# 奖金:如何获得和使用 ngrok

与这个特性无关，能够为外部服务开放一个本地托管的端口非常有用。

在这种情况下，我希望我的 Github 动作触发我的 localhost，这样我就可以测试我的 webhook 的有效负载和执行。当我将它投入生产时，我将用代码的生产版本替换`url`。

来救援了！Ngrok 是一种创建到本地主机的隧道的服务。也是免费的。

我的公司为我们中的一些人支付了保留网址的费用(因为网络到移动开发的事情)，但是对于你的目的来说，免费是很好的。

只需`brew install ngrok`(或者您使用的任何包管理器)，启动您的本地主机，提供 webhook 代码，然后`ngrok http <your-port>`。

现在，您已经创建了到本地主机的公共连接。Github 将能够访问它，您可以测试您的执行。请注意，免费隧道会在一段时间后过期，需要一个新的网址。

# 现在怎么办？

如果这给了你一些很酷的想法，告诉你如何改变你的 Github，你应该去看看 [Actions Marketplace](https://github.com/marketplace?type=actions) 。

在里面，你会发现一些非常聪明的人已经想出了各种各样的事情，当事件在你的 Github 中被触发时就会发生。在这里你可以找到自动换羽器、吉拉连接、部署工具等等。

快乐行动！