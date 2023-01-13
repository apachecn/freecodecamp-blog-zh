# 首次体验缩放流星应用程序

> 原文：<https://www.freecodecamp.org/news/first-experiences-scaling-a-meteor-app-14a48e62a4af/>

埃利·斯坦博克

# 首次体验缩放流星应用程序

![1*CLGpRvTRx9l4dn-ky6bP5g](img/e854a67570bc5367e2569988f9faed85.png)

我最近经历了不得不扩展我的 [Meteor](https://www.meteor.com/) 应用程序的挑战和考验。这个项目已经在生产中运行了大约一年。今年夏天，随着数以千计的季前赛注册，这款应用变得更加流行。我的初始设置无法再处理负载，我面临着一个必须尽快解决的扩展问题。

这篇文章描述了我经历的过程和我在这个过程中学到的一些东西，我希望它能帮助其他人在未来面临类似的挑战。它将涵盖一些基础知识，比如什么是伸缩，以及负载平衡是如何工作的。它还将带您完成一些基本设置，并向您展示如何扩展您的 Meteor 应用程序。

今年夏天之前，我没有任何升级应用程序的经验，尽管我已经阅读了很多关于这个主题的资料，但当我不得不坐下来开始自己处理这些挑战时，我感到非常失落，不确定我是否能够解决这些问题。我还觉得我在网上读过的许多文章都假定了我还不具备的大量知识。

我希望这篇文章将有助于弥合一些差距。我远不是扩展 Meteor 应用程序的专家，但是我希望这篇文章能够为那些发现自己处于与我相似的情况的人提供价值。

### 背景

讨论中的应用程序的一点背景。这是一款英格兰超级联赛(足球)的选秀式梦幻足球游戏。大多数签约发生在赛季开始前的一个月。除了所有的签约，几乎所有的选秀都发生在这个月。选秀是当一个联盟中的所有用户在线选择他们的球队。用户一次选择一名足球运动员，整个过程是实时的，每个用户有 30 秒到 5 分钟的时间选择球员。

在高峰时期，该网站有超过 500 个并发用户在线，20 个草稿同时运行。如果服务器在选秀过程中变得没有反应，这完全破坏了用户的体验，玩家会被自动选中，而他并不一定打算选中，所以避免这种情况是非常重要的。

### 那么什么是缩放呢？

对许多人来说，这似乎是显而易见的，但不久前，我还不知道这个流行词是什么意思。

当你的应用起飞时，就会发生缩放。你的服务器只能处理这么多负载。一万个人同时想用你的 app 会怎么样？你的服务器将无法处理它，所以你要么需要更好的服务器，要么需要更多的服务器来运行应用程序。这个过程称为缩放。

有两种方法可以扩展应用。一种称为垂直缩放，另一种称为水平缩放。垂直扩展包括获得更强大的服务器。水平扩展包括从多个服务器为您的站点提供服务。

打个比方，如果你有一个商店，雇佣了一个工人，并希望能够服务更多的顾客，你可以得到一个更好、更快的工人(纵向扩展)，或者你可以雇佣更多的员工(横向扩展)。

对于 web 应用程序，垂直缩放通常更容易做到。在托管公司，如 [DigitalOcean](https://www.digitalocean.com/?refcode=2518a67f26c8) 或 [AWS](https://aws.amazon.com/?nc2=h_lg) (亚马逊网络服务)，你可以轻松地将你的虚拟专用服务器升级到更强的配置，增加 RAM、CPU 和存储。

这种方法的问题是，您可以让服务器变得多强大总是有一个极限。在某种程度上，你将不得不横向扩展。以上面的例子为例，你的企业只能靠一个员工走这么远。到了一定程度，你将不得不雇佣更多的员工。

说到 Meteor 应用程序，你必须在相当早的时候进行横向扩展。Meteor 应用程序实际上只是一个 NodeJS 应用程序，因此在单个进程中运行，这意味着它只能使用单个处理器。

我们可以通过同时运行应用程序的多个实例来利用多个处理器。这些处理器可以属于同一个服务器，也可以分布在多个不同的服务器上。运行一个应用的多个实例意味着在不同的 IP 或端口上运行同一个应用，并将负载分散到应用的不同实例上。该应用程序的所有实例仍然连接到同一个数据库，所有连接的客户端将立即接收任何数据库更新，而不管它们连接到的应用程序实例(假设您使用的是 MongoDB 的 Oplog 跟踪功能。否则更新可能需要几秒钟)。

无论我们是在一台多核服务器上运行应用的多个实例，还是在多台服务器上运行多个实例，我们所做的都是水平扩展，两种情况下的工作方式都是一样的。

每个平台都有自己的扩展挑战。这篇文章是关于扩展流星应用。在其他平台上，你可以在更长的时间内摆脱垂直缩放。StackOverflow 在 25 台服务器上运行，甚至可以用 5 台来凑合。这对于每台服务器来说都是一个很大的负载，并且是一个非常棒的垂直扩展的例子。

### 流星应用程序的不同部分

我们可以将我们的应用程序分成两个逻辑组件。第一部分是服务器，它处理用户的请求，将适当的数据发送回来，并执行任何需要执行的任务。第二部分是存储应用程序数据的数据库。

服务器与数据库交互，在一致的基础上查询和更新数据库。

我们不会过多讨论如何扩展 MongoDB。可能还需要一段时间才能达到必须扩展数据库的程度。如果你想了解更多关于这个话题的信息，[这个链接](http://highscalability.com/blog/2014/3/5/10-things-you-should-know-about-running-mongodb-at-scale.html)可能是一个很好的起点。

### 纵向扩展服务器之前要做的事情

在开始扩展之前，一个好的起点是确保你的应用尽可能高效地运行。推荐使用的工具是 [Kadira](https://kadira.io/) 来检查你的应用程序的性能，以及哪些方法和发布需要花费最多的时间来完成。基本计划是免费的，所以没有理由不使用它。使用[卡迪拉学院](https://kadira.io/academy)的文章来找出如何最好地优化你的应用程序，以及在哪里投入你的精力。一般来说，您希望优化最常运行并且需要很长时间才能完成的方法和发布。

对于良好的性能来说，一件绝对必要的事情是利用 MongoDB 索引。在《流星》中，你可以这样写:

> *帖子。_ ensure index({ userId:1 })；*

这在*帖子*集合中的*用户 Id* 字段上创建了一个索引。您也可以在 MongoDB 本身中创建索引。点击查看更多关于 MongoDB 指数[的信息。](http://docs.mongodb.org/manual/core/indexes/)

请参见本文的[部分，了解更多性能技巧。](http://blog.differential.com/scaling-meteor-to-20000-users-in-7-days/)

### 我什么时候需要开始担心伸缩性？

如果你的流星应用程序在任何时候都有超过 100 个用户在线，你可能要开始担心可伸缩性了。(当然你不知道你的 app 什么时候起飞。您可能会在一天之内从 5 个并发用户增加到 500 个，所以在这种情况发生之前做好准备是值得的。)

根据你的应用程序，你可以在一个 DigitalOcean droplet 上获得多达 500 个并发用户。我的应用程序很难处理 100–150 个并发连接，在这一点上它达到了 100%的 CPU。大多数 Meteor 应用程序的瓶颈似乎是 CPU 而不是 RAM，所以水平扩展是必须的。你可以添加所有你想要的内存，但它不会帮助你的应用程序。缺少 CPU 会使你的应用程序过载，你只能通过使用多个 CPU(或多个服务器)来获得更多的 CPU 能力。

下图来自 Kadira，展示了当你的应用程序承受太大压力时会发生什么。

![1*DLnj_FU_qp2TW6s5NADnuQ](img/7f020212705ad076ca91bfc010101027.png)

Kadira stats. 1 core, $10/month Digital Ocean droplet

在下午 7:28 左右，您可以看到发布的平均响应时间大约是 18 秒，方法的平均响应时间大约是 7 秒。这不是一个好的情况。高响应时间是由 CPU 中的一个大峰值引起的，该峰值在几分钟内达到 100%。RAM 不是问题，因为任何时候都只有大约 500MB 被使用，这里使用的 VPS 有 1GB RAM。

### 部署

有多种方法来部署你的流星应用。以下是人们在生产应用中常用的一些解决方案:

*   自托管在[数字海洋](https://www.digitalocean.com/?refcode=2518a67f26c8)或 AWS 上
*   [模数 io](https://modulus.io/meteor)

还有免费的 meteor.com 托管服务，但这应该只用于开发目的。MDG 也刚刚发布了 [Galaxy](https://www.meteor.com/why-meteor/pricing?gclid=Cj0KEQjwy92wBRCl7trx4PaIo8EBEiQASPhtC_SBf0TXyLL_MZpEx9RYTIyVoZT8-5zGVOaC_tDzUsoaAgBB8P8HAQ) ，但是目前的价格是每月 500 美元起。MDG 正致力于为 Galaxy 提供更便宜的计划以及免费计划，但在撰写本文时，这些都不是选项。

人们将数据库托管在另一个提供商处也很常见。比较流行的一个是 [Compose.io](https://www.compose.io/mongodb/) 。

> **更新:**你现在可以获得[银河](https://www.meteor.com/hosting)主机，每月 29 美元起。Compose.io 不再有免费计划。另一家名为[mlab](https://mlab.com/)的 Mongo 托管服务提供商提供。当你进入生产阶段时(即当开发已经结束，人们实际上正在使用你的应用程序时)，你不希望使用免费计划。

我自己的设置是 DigitalOcean + Compose.io。

如果你想了解其他设置，请参见 [Meteorpedia](http://meteorpedia.com/read/Category:Deployment) 上的部署部分。

我最初的设置是每月 10 美元的数字海洋服务。这为您提供了一个带 1GB RAM 的单核 VPS。你还可以获得 30GB 的存储空间，尽管你可能不会用到其中的大部分。

我使用 Compose.io 的每月 18 美元的 MongoDB 数据库托管弹性部署计划。你可以自己处理 MongoDB 的东西，但这只是我不想处理的额外工作。

为了进行部署，我使用了一个叫做 Meteor Up(简称 Mup)的工具。可以在 GitHub [这里](https://github.com/arunoda/meteor-up)看一下。

如果您在 Modulus.io 上部署，他们会为您处理很多扩展问题。如果你想运行你的应用程序的多个实例，你所要做的就是在模数网站上上下移动一个滑块。您运行的实例越多，成本就越高，但是您也可以随时缩减规模。

我没有使用部署模块，因为两年前我在设置它时遇到了一些问题。我想这些问题现在应该已经解决了。另一个问题是成本。部署在数字海洋上更便宜。缺点是它可能会占用你更多的时间。使用 DigitalOcean 的另一个好处是可以更好地控制你的服务器。

一篇比较 DO，Modulus.io 和 Heroku 的文章可以在[这里](http://joshowens.me/modulus-vs-heroku-vs-digital-ocean/)找到。

### 那么，你实际上如何扩展一个流星应用程序呢？

如上所述，我的应用程序部署在数字海洋使用流星了。有了这个设置，你就不必奢侈地使用滑块来加载更多的实例。一个标准的 *mup.json* 文件看起来像这样:

```
{   “servers”: [     {      “host”: “123.45.678.901”,      “username”: “root”,      “pem”: “~/.ssh/id_rsa”,      “env”: {}     },     {       “host”: “333.22.444.555”,       “username”: “root”,       “pem”: “~/.ssh/id_rsa”,       “env”: {}     }   ],   “setupMongo”: false,   “setupNode”: true,   “nodeVersion”: “0.10.40”,   “setupPhantom”: true,   “appName”: “myapp”,   “app”: “/Users/arunoda/Meteor/my-app”,   “env”: {      “PORT”: 80,      “ROOT_URL”: “http://myapp.com”,      “MONGO_URL”: “mongodb://arunoda:fd8dsjsfh7@hanso.mongohq.com:10023/MyApp",      "MONGO_OPLOG_URL": "mongodb://.....",      “MAIL_URL”: “smtp://postmaster%40myapp.mailgun.org:adj87sjhd7s@smtp.mailgun.org:587/”   },   “deployCheckWaitTime”: 15}
```

注意，我在 *servers* 块中列出了两台服务器。当您第一次开始时，您将只列出一台服务器。通过以下设置，我们将部署到 ip 地址 *123.45.678.901* 和 *333.22.444.555* 。部署之后，如果您在浏览器中访问这些 ip 地址中的任何一个，您会看到相同的内容，两台服务器将连接到同一个数据库。

### 负载平衡

因此，我们已经部署了我们的网站，但我们不希望用户访问随机 ip 地址的网站。我们希望他们访问我们的领域。假设我们的域名是 awesomedomain.com(依我拙见，比 example.com 好)。我们如何让我们的两台服务器分别处理一半的请求？

一种方法是使用一个叫做 [Nginx](http://nginx.org/) 的工具。来自[维基百科](https://en.wikipedia.org/wiki/Nginx):

> Nginx (发音为“engine x”)是一款专注于高并发性、高性能和低内存使用的 web 服务器。它还可以充当 HTTP、HTTPS、SMTP、POP3 和 IMAP 协议的反向代理服务器，以及负载平衡器和 HTTP 缓存。

我们将用它做三件事:

1.  作为一个反向代理
2.  作为负载平衡器
3.  对于 SSL 支持

这意味着我们将在我们的一台服务器的 80 端口上运行 Nginx。任何从网络进入服务器的流量都会被 Nginx 接收。Nginx 然后将流量转发到我们的 Meteor 应用程序的实例上，这些实例可以运行在同一服务器的不同端口上，也可以运行在不同的服务器上。Nginx 将尝试平衡发送到每个实例的请求数量。

Nginx 是一个反向代理，这意味着它将请求转发到其他地方，然后对用户做出响应。

成为负载平衡器意味着它将在应用程序的不同实例之间分割(平衡)负载。

我们还将使用 Nginx 为我们提供 SSL 支持。SSL 支持意味着用户和我们的服务器之间传输的任何数据都将被加密。如果没有 SSL 支持，我们的 Meteor 网站将无法在许多国家正常运行，人们也很容易劫持我们用户的账户或读取任何传输的数据。任何使用 HTTPS 的网站都支持 SSL。

SSL 不是本文的重点，但它是任何生产应用程序的必备工具，Nginx 可以为您处理 SSL 问题。要使用 Nginx 设置 SSL，请参见此处的。尽管你不需要使用 Nginx 来支持 SSL。流星了也可以处理它使用一种工具称为螺柱显示[在这里](https://github.com/arunoda/meteor-up#ssl-support)。

那么我们如何设置 Nginx 呢？

为此，我建议遵循本教程的第一步:

[**如何用 Nginx 在 Ubuntu 14.04 上部署一个 Meteor.js 应用| digital ocean**](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-meteor-js-application-on-ubuntu-14-04-with-nginx)
[*用 Nginx 和 MongoDB 在 Ubuntu 14.04 上部署一个 Meteor.js 应用。本教程向您展示了如何构建和部署…*www.digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-meteor-js-application-on-ubuntu-14-04-with-nginx)

不要费心去遵循教程的其余部分。流星会自动为你处理所有的事情。

本文还解释了如何使用 Nginx 来支持 SSL。

一旦所有这些都为你工作了，你应该让你的应用程序在某个域中运行。到目前为止，Nginx 实际上并没有为我们做多少事情(除了可能的 SSL 支持。)我们仍然没有使用我们的第二个流星实例，但我们现在要修复它。

要使用更多的服务器，您需要将以下代码添加到 Nginx 文件的顶部:

```
upstream myAppName {  ip_hash;               # for sticky sessions, more below  server 123.45.678.901:3000;  # server 1  server 333.22.444.555:3000;  # server 2}
```

现在，您的 Nginx 文件应该类似于以下内容:

```
upstream myAppName {  ip_hash;               # for sticky sessions, more below  server 123.45.678.901:3000;  # server 1  server 333.22.444.555:3000;  # server 2}server {  listen 80;  server_name www.myapp.com  # and all other "server" directives  location / {    # the "hostname" below must be same myAppName from upstream directive above    proxy_pass http://myAppName;    # and all other "location" directives  }}
```

…其中 *myAppName* 可能是类似于 example.com 或 app.example.com*的东西。*

以上代码改编自 [Meteorpedia](http://www.meteorpedia.com/read/nginx) 。

如果您现在使用以下命令重新加载 Nginx(如上面数字海洋文章中所述):

```
nginx -t # check everything is okaynginx -s reload
```

负载将在您的两个 Meteor 实例之间分配。

如果您想在将来添加更多实例，您所要做的就是在 Nginx 上游块中添加另一行，然后重启 Nginx。下面是一个使用 4 个 Meteor 实例的示例，其中 2 个实例运行在两个不同的服务器上:

```
upstream myAppName {  ip_hash;  server 10.0.0.1:3000;  # server 1, core 1  server 10.0.0.1:3001;  # server 1, core 2  server 10.0.0.2:3000;  # server 2, core 1  server 10.0.0.2:3001;  # server 2, core 2  # or whatever other appropriate combination}
```

删除实例只需要从上游块中删除行并重启 Nginx。

### 负载平衡算法

因此，如果您只是想让事情正常运行，那么遵循上述步骤应该是可行的，但如果您想理解得更深入一点，这里有一个我们刚才所做的基本解释。

我们使用 Nginx 作为负载平衡器。对于每个进来的请求，Nginx 必须决定将它发送到哪个服务器。同时，Nginx 必须尽可能快地完成工作，使用尽可能少的资源。Nginx 能够处理 10，000 个并发连接，简单的负载平衡算法是实现这一点的一部分。

一种用于决定将下一个请求发送到哪里的简单算法称为“循环法”在这个算法中，负载均衡器依次向每个服务器发送客户机请求，一旦到达服务器列表的末尾，就再次开始这个过程。三台服务器的结果是:1，2，3，1，2，3，1，2，…

还有许多其他算法可以用来决定将请求发送到哪个服务器，但是 Meteor 要求我们遵守的一个约束是，来自用户的每个请求都应该发送到同一个实例。这也称为“粘性会话”用户在整个会话期间都使用同一台服务器。

这意味着我们不能对我们的 Meteor 应用程序使用循环算法，它没有考虑用户首先连接到哪个服务器。相反，我们使用 *ip_hash* 算法，该算法在 [Nginx 文档](http://nginx.org/en/docs/http/load_balancing.html#nginx_load_balancing_with_ip_hash)中解释如下:

> 使用 ip-hash，客户机的 ip 地址被用作散列关键字，以确定应该为客户机的请求选择服务器组中的哪个服务器。这种方法确保来自同一客户端的请求总是被定向到同一服务器，除非该服务器不可用。

### 本地运行

你也可以尝试在本地运行你的应用的两个实例。为此，打开两个终端窗口，在标准端口(3000)上运行一个 Meteor 应用程序:

```
meteor
```

数据库现在将在端口 3001 上运行。

在单独的终端窗口中，通过运行以下命令，在连接到同一数据库的端口 4000 上运行该应用程序的另一个实例:

```
export MONGO_URL=mongodb://localhost:3001/meteor
```

```
meteor --port 4000
```

现在，您可以在您的网络浏览器中访问 *localhost:3000* 或 *localhost:4000* ，两者将对同一个数据库进行更改。

### 需要注意的事项

您的应用程序中可能有您只想运行一次的代码。例如，在我的梦幻足球游戏中，我只希望一个服务器更新比分。

另一个可能出现的问题是，某些操作可能相当昂贵。同样，在游戏中更新分数是一个相当长的过程，我不希望它干扰应用程序的基本功能。

那么如何解决这些问题呢？

处理这种情况有多种方法，但我只概述一下我的做法。

我在数字海洋上设置了一个新的 droplet，它不会从主网站接收任何流量。它可以通过其 IP 地址访问，但这个 droplet 不会从网站的公共 URL 接收任何流量。该实例负责运行所有后台任务和任何只应运行一次的任务。

为了使代码只针对应用程序的一个实例，我使用了环境变量。在您的本地环境中，您可以在终端窗口中导出变量，如下所示:

```
export ENV_VAR=valueOfOurVar
```

我们之前为 *MONGO_URL* 做过这个。

您可以使用以下方式访问 Meteor 应用程序中的环境变量:

```
process.env.ENV_VAR
```

在我的应用程序中，如果我只想让应用程序的一个实例运行某些代码，我可以这样做:

```
if (process.env.RUN_BACKGROUND_TASKS === ‘true’) {   // do some task}
```

我也可以使用纯微服务的方法，但是这会花费我更多的时间来设置，并且代码共享会更加困难。

使用 Meteor Up 时，您可以为特定实例设置环境变量，如下所示:

```
“servers”: [   {     “host”: “...”,     “username”: “...”,     “pem”: “...”,     “env”: {       “ENV_VAR”: “123”,       “RUN_BACKGROUND_TASKS”: “true”     }   } ],
```

当运行一个应用程序的多个实例时，也有一些包不能很好地工作。我遇到的一个属于这一类的包是 [mizzao:user-status](https://atmospherejs.com/mizzao/user-status) 。这个包告诉你在任何给定的时间哪些用户在线，当运行多个实例时[不能正常工作](https://github.com/mizzao/meteor-user-status/issues/70)。一个服务器将有一个开放的连接到一个特定的客户端，而其余的不会，这个包不知道如何处理这种情况。关闭的连接意味着用户脱机。一个变通办法是使用 [konecty:user-presence](https://atmospherejs.com/konecty/user-presence) 包。

您也不希望 cron 任务在多个实例上运行。为此你可以使用[渗滤液:同步-cron](https://atmospherejs.com/percolate/synced-cron) 包。我个人并没有使用它，但是它看起来是一个很好的解决方案，选择将所有的 cron 任务限制在一个特定的服务器上，但是它看起来是一个很好的解决方案。

### 其他解决方案

有许多方法可以实现本文中的目标。使用 Nginx 并不是唯一的解决方案。对于 SSL 支持，我们可以像 Meteor Up 那样使用 [stud](https://github.com/bumptech/stud) 来代替。对于负载均衡，我们可以使用一个名为 [HAProxy](http://www.haproxy.org/) 的工具，它的唯一目的是成为一个负载均衡器。

另一种方法是使用 [MeteorHacks Cluster](https://github.com/meteorhacks/cluster) 包。这是一个简单的流星包，你添加如下:

```
meteor add meteorhacks:cluster
```

要让它在您自己的机器上立即工作，请将以下环境变量导出到您的终端:

```
export CLUSTER_WORKERS_COUNT=2
```

如果您现在运行您的应用程序，您应该看到您的 Meteor 应用程序启动的两个实例。这里我们利用了集群包的多核支持。我们还可以使用集群在多个服务器之间分配负载。您应该查看[集群文档](https://github.com/meteorhacks/cluster)来进行设置。

用 MeteorHacks 集群好还是 Nginx 好？

我不知道这个问题的答案。我在生产中使用 MeteorHacks 集群时确实遇到了问题。该应用程序在低压力条件下工作正常，但在高负载下，该应用程序会随机崩溃，需要手动重启才能再次工作。GitHub 上有一个公开的问题[在这里](https://github.com/meteorhacks/cluster/issues/46)。如果你不使用多核支持，这应该不是一个问题。

### 我的最终设置

该数据库运行在 Compose.io 上，每 GB 使用空间的费用为每月 18 美元。

该应用目前运行在 8 个数字海洋水滴上:

($ 5/月 droplet 有 512MB 内存和 1 个内核，$ 10/月 droplet 有 1GB 内存和 1 个内核，$ 20/月 droplet 有 2GB 内存和 2 个内核。)

*   1 $ 5/月 droplet 负责后台任务，不处理任何用户请求。
*   1 个 20 美元/月的双核 droplet 运行 Nginx 和该应用的 2 个 Meteor 实例。
*   4 个 10 美元/月的液滴(每个液滴 1 个实例)
*   每月 2 美元 5 滴(每滴 1 个实例)

在 Ghost 的一个子域上运行的博客也有每月 5 美元的费用(这不是 Meteor)。

我还在开发一款手机应用，它将在其他 droplets 上运行，并为访问者提供不同的网站。这里的客户端视图/逻辑是不同的，服务器逻辑是相同的。

#### 选择哪些水滴？

所以你可能想知道为什么我的水滴大小不同。起初，我只使用 10 美元/月的水滴，但由于我的瓶颈是 CPU 而不是 RAM，我应该能够将所有 10 美元/月的水滴替换为 5 美元/月的水滴，因为每个水滴都具有相同的 CPU 能力。

我现在正在对此进行测试，看看效果如何。你需要注意的是，在使用 512MB 液滴时，不要耗尽内存。如果你使用[](https://atmospherejs.com/meteor/spiderable)*软件包，当搜索引擎索引你的网站时，你的服务器会崩溃，这将导致 CPU 达到 100%。你可以通过使用 [prerender.io](https://prerender.io/) 而不是 *spiderable* 和/或[添加交换空间](https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04)来解决这个问题。如果你使用 React，那么你可以为搜索引擎使用服务器端渲染，而不是 spiderable，如这里的[所解释的](https://kadira.io/blog/meteor/meteor-ssr-support-using-flow-router-and-react)。*

#### *自动缩放*

*如果能够在网站处于压力之下时自动加载更多的水滴，并在无人使用时将其移除，那就太好了。例如，我的网站在周末会被更多地使用。如果我能在流量大的时候自动发射更多的水滴，在流量少的时候把它们取下来，那就好了。*

*对于部署在 DigitalOcean 上的 Meteor 应用程序，我还没有看到一个简单的解决方案，但是利用 DigitalOcean API，这应该不需要太多的工作就可以实现。这将大大降低托管成本。*

*似乎很容易自动缩放模数，如这里的[所示](http://blog.modulus.io/new-modulus-multi-servo-auto-scaling)。你可以在 AWS [这里](http://allandequeiroz.com/2015/09/27/amazon-auto-scaling-and-meteor/)和[这里](http://fightingtheboss.github.io/)阅读自动缩放。*

#### *我会用不同的方式去做的事情*

*在开发过程的早期阶段，我会测试我的应用程序能否在两台服务器上运行良好。如果你以前从未做过，从 1 台服务器到 2 台服务器的跳跃是相当大的。从 2 台服务器到 10 台服务器的跳跃很小。*

#### *关于扩展 Meteor 应用程序的其他资源*

*   *[流星群:星团](https://github.com/meteorhacks/cluster)*
*   *[流星余迹](https://github.com/arunoda/meteor-up)*
*   *[气象百科](http://www.meteorpedia.com/read/Scaling_your_Meteor_App)*
*   *流星:流星有规模吗？*
*   *[探索流星:测量流星:实时应用的挑战](https://www.discovermeteor.com/blog/scaling-meteor-the-challenges-of-realtime-apps/)*
*   *[差异:7 天内将 Meteor 扩展到 20，000+用户](http://blog.differential.com/scaling-meteor-to-20000-users-in-7-days/)*

#### *后来增加的*

*我真的很喜欢下面的帖子。它有一些关于负载测试、更好的 Nginx 设置和使用 CDN 的关键信息:[https://medium . freecodecamp . com/minimum-valid-devo PS-919972 DFD 9 e 0](https://medium.freecodecamp.com/minimum-viable-devops-919972dfd9e0#.ym8w9jj8g)*