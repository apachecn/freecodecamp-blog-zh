# 如何建立一个基本的 slackbot:初学者指南

> 原文：<https://www.freecodecamp.org/news/how-to-build-a-basic-slackbot-a-beginners-guide-6b40507db5c5/>

作者:维斯瓦·沙阿

****更新:代码和教程于 6 月 28 日更新，以反映 Slack API 的变化**。**

# 懒人机器人:为什么要用它们？

在我们进入这篇文章的教程部分之前，让我们看看为什么这是一个有价值的项目和工具。

Slack 是越来越受欢迎的团队交流工具。它已经发展到包括其他广泛使用的项目管理工具的插件，如 JIRA、谷歌驱动等。任何 slack 用户都知道——你在对话中做得越多越好。

slackbot 的常见用途从任务完成时的简单通知器(如测试构建，或您的午餐准备好了)到交互式的、基于按钮的机器人，它们按照用户的意愿执行命令。您可以构建轮询机制、对话机器人等等。

# 设置 python 编程环境

![1_p65lij8MGz7AUNRl9D3sNw](img/19eb290f8f9d4250f23cbc63bb608c0c.png)

如果你是 windows 用户，并且你以前没有使用过 python，你需要[安装](https://docs.python.org/3/using/windows.html)它。Linux/Mac 用户:Unix 自带 python！

一旦安装完毕，启动你的终端并输入`python`或`python3`(如果你有多个安装的话)以确保它工作并且存在。

还要检查你是否有一个好的代码文本编辑器: [sublime](https://www.sublimetext.com/3) 和 [atom](https://flight-manual.atom.io/getting-started/sections/installing-atom/) 是很好的选择。

可选:在虚拟环境中工作可能也很有用——当您有很多依赖项时，这是一个很好的实践。

```
pip install virtualenv
virtualenv tutorial
source tutorial/bin/activate
```

![1_QHX3UYi6NFRe_xteZ9qiYQ](img/fd2922d93280bc3a97f6d891f7f3f33c.png)

您还应该派生出教程 GitHub repo 并克隆到您的本地机器上，因为我们将使用该代码作为本教程的框架。

为此，进入[回购](https://github.com/vishwa35/slackbot-tutorial)，点击右上角的`Fork`。分叉的回购应该是****<your username>/slack bot-tutorial****。点击状态栏右边的绿色`Clone or download`按钮，复制网址。返回终端克隆存储库:

```
cd Desktop/
git clone https://github.com/yourusername/slackbot-tutorial.git
cd slackbot-tutorial/
sublime . (or open your text editor and open this directory)
```

# 松弛应用程序

创建 slackbot 有两种方法:独立的机器人，或者 Slack 应用程序。应用程序允许更广泛的功能，也是 Slack 推荐的创建机器人用户的途径。

前往[https://api.slack.com/apps](https://api.slack.com/apps)，点击右上角的`Create New App`。给它一个名称，并选择一个工作区，您可以在其中创建一个通道来测试您的 bot。您可以随时为另一个工作空间重新配置您的 bot，甚至可以将其发布到 Slack 应用程序目录。

我建议在您仅为开发目的而创建的工作空间中创建一个#test 通道，或者创建一个相对较少用户不介意您在那里测试某些东西的工作空间。

![1_aHE2Te70SwllhMxUE5Tgpg](img/dfa0291bbf08ef2e794be89500e9cb3e.png)

你要做的第一件事是获得机器人令牌。当你到达上面的页面，点击机器人。添加一些范围；这些决定了应用程序的机器人用户将拥有哪些权限。说干就干， [****聊天:写****](https://api.slack.com/scopes/chat:write) 和**[****im:写****](https://api.slack.com/scopes/im:write)**大概就够了。****

****![1_881iK1n-kUuZVXagvfS5qg](img/5bbe8d219f27604f6680a07ec0b0e200.png)****

****现在，要真正获得您的代币，您需要前往左侧栏的`OAuth & Permissions`。****

****在这里，您将能够`Install the App to the Workspace`并生成必要的令牌。根据经验， ****bot 令牌**** 以`xoxb-.`开头****

**你还会想要 s ****igning secret**** ，它位于基本信息> App 凭证下。**

## **充当你的机器人**

**现在，您拥有了进行 API 调用和充当 bot 所需的凭证。要对此进行测试，请启动一个终端并运行以下命令(使用正确的令牌和通道名):**

```
`curl -X POST \
     -H 'Authorization: Bearer xoxb-your-token' \
     -H 'Content-type: application/json;charset=utf-8' \
    --data '{"channel":"#test","text":"Hello, Slack!"}' \
https://slack.com/api/chat.postMessage`
```

**如果您进入 slack 工作空间中的通道，您现在应该会看到来自 bot 的消息！您刚刚发出了一个 HTTP POST 请求——要求服务器在某个地方发布一条消息。**

# **给机器人编程**

**我们希望通过编程来实现上述目标。有几种不同的方法可以设置 slackbot。我将介绍以下内容:**

*   **周期性地(按计划)被触发说某事**
*   **/斜杠命令**

**第二种需要运行服务器，而第一种不需要。**

## **预定消息**

**假设您想要定期向某个地方发送消息—可能是每周一早上。转到打开`slackbot-tutorial`的文本编辑器。**

**您应该会看到一个文件`scheduled.py`。看一看:`sendMessage`是一个向 slack 发出 API 调用并发布消息的函数。在底部，您将看到 main 方法:当您运行脚本时执行什么。在这里，您会看到一些需要注意的事项:**

*   **`SLACK_BOT_TOKEN`是从`os.environ['SLACK_BOT_TOKEN']`拉过来的——怎么拉？在你的终端中运行`export SLACK_BOT_TOKEN="xoxb-your-token"`来设置这个变量。**
*   **这里使用了一个调度器，并且有一个无限循环来检查调度器上的事件。默认情况下，我已经安排每分钟调用一次`sendMessage`函数。**

**为了测试这一点，返回到您所在的终端的`slackbot-tutorial`目录并运行**

```
`export SLACK_BOT_TOKEN="xoxb-your-token"
python scheduled.py`
```

**您应该会看到日志消息打印出来。确保您已经将代码中的`****channel=#test****`更改为您的测试通道名称(如果不同)并添加了您的 bot(在 slack 通道中，键入`/invite @botname`。让它运行几分钟，看看 Slack 上显示的消息！**

**当然，这是一个计划消息发送者的超级基本实现——实际上你可以用 slackbot `/remind #test “Hello, Slack!” every Monday at 9am`做到这一点。**

**这里的 ****真正的力量**** 是你可以用任何函数代替`sendMessage`，利用通过 API 与外部服务接口的力量，做数学等，然后构造一个消息来发布。**

## **斜线命令**

**这个需要更多一点的设置——回到你的[应用设置](http://api.slack.com/apps) >斜线命令。创建一个新的斜杠命令:例如`/test`。对于请求 URL，您需要部署这个 web 服务器(我使用 Heroku)，或者运行一个本地`ngrok`实例来测试它。后者将在本地运行，最适合测试。你可以`brew install ngrok`或者从[这里](https://ngrok.com/download)得到。**

**在 starter code repo 中，寻找`slashCommand.py`开始了解这个方法。要启动服务器，运行`python server.py`。放入 Slack 的请求 URL 将由您的`ngrok`实例和代码中的`@app.route`给出——类似于[http://a 1234 b5 CDE 6 f . ngrok . io](http://a8787d2fea3b.ngrok.io/)/****Slack/test****(粗体部分来自代码中定义的 route)。您应该能够在您的松弛工作空间中测试斜杠命令。从教程代码，尝试`/test`。**

# **走向**

**现在你有了一个非常基本的 slackbot，它要么根据命令运行，要么经常运行。创造性地使用它！想想你还能把这个骨架和什么联系起来，让它更有用。**

## **你的机器人可能做出反应的其他方式**

1.  **行动/响应可以由提及或某些短语触发。这需要运行服务器并在某处监听消息。**
2.  **你的机器人可能是会话式的，并可能有助于线程。查看一些 NLP，开始进行可理解的对话！Word2Vec + TensorFlow 或 Keras 可能是一个起点。DialogFlow 也很棒。**
3.  **把它和其他一些 API 连接起来。也许你希望能够与谷歌表互动，并运行一些计算。您可能希望根据某些操作向其他用户发送消息。整合[按钮](https://api.slack.com/docs/message-buttons)。也许你想触发基于其他东西的消息。**