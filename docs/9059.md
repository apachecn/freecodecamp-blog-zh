# 使用 Webtask.io 创建一个简单的 REST API 端点

> 原文：<https://www.freecodecamp.org/news/create-a-simple-rest-api-endpoint-using-webtask-io-d9607fc00c17/>

作者:ismapro

# 使用 Webtask.io 创建一个简单的 REST API 端点

![1*NeGT5gT1GaOUWOC-ZSRlag](img/a1ccd5d0af4d6e1a5d2ca516ef888a2b.png)

Webtask.io 是 Auth0 提供的一项服务，它允许你通过 HTTP 调用在云中运行单段代码。

每个部署的部分将在沙箱下运行，但有一些限制:

*   有限的处理器时间
*   每个任务可用的库数量有限
*   有限存储

但是这些限制提供了一个环境，在这个环境中，您可以通过 *HTTP* 以简单且可伸缩的方式公开您的应用程序，而无需了解服务器管理或环境配置的本质。

还有许多其他可用的特性，如控制访问的令牌验证、机密数据和元数据。如果你想知道更多关于 Webtask 如何工作的信息，他们的文档中有一些例子。

#### 让我们开始编写基本的 REST API

要创建一个 webtask，您需要使用 *webtask-cli。这是一个命令行应用程序，允许你管理你的网络任务。*

所以首先在您的环境中安装它:

```
npm install wt-cli -g
```

然后初始化您的会话，使用此电子邮件登录流程:

```
wt init your_email@something.com
```

一旦你这样做，你应该会收到一个代码来激活您的帐户。

现在，您可以继续创建将成为我们的 webtask 的逻辑的文件。您可以随意命名它，但是请记住，它将是服务稍后提供的 URL 的一部分。我们给它起个名字吧:

```
basic-rest.js
```

让我们向它添加以下代码:

从命令行导航到保存文件的位置，然后运行以下命令:

```
wt create basic-rest.js
```

您将收到一个 URL，可用于检查您的 webtask，如下所示:

```
https://webtask.it.auth0.com/api/run/wt-myemail-gmail_com-0/basic-rest?webtask_no_cache=1
```

从浏览器导航到您的 url，您将看到应用程序的响应:

```
{"error":"GET method not implemented"}
```

这是我们期望从代码中得到的响应。现在，您可以向每个方法中添加任何您想要的逻辑。然后，您可以使用 postman 或 curl 测试其他方法(POST、DELETE、PUT)。

仅此而已。您已经部署了一项服务，无需任何额外的配置或管理。这项服务的伟大之处在于能够从外部 API 集成 webhooks，并使用其他后端与数据或查询进行交互。

有许多功能和选项我没有探索，但你可以查看他们的网页和更多关于他们的信息。

希望你喜欢，请在评论区告诉我你的想法。编码快乐！