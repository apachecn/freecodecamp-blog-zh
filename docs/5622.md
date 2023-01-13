# 如何使用 Docker 在 AWS 上部署 Mongo:新手权威指南

> 原文：<https://www.freecodecamp.org/news/how-to-deploy-mongo-on-aws-using-docker-the-definitive-guide-for-first-timers-3738f3babd48/>

### 你为什么需要这个？

#### 因为 JS + Python + Mongo =全数据开发

我是流星开发者。更准确地说，我用的是 Vulcan.js，[，但那是另外一个故事了](https://medium.com/dailyjs/write-less-code-ship-more-apps-how-vulcan-js-makes-me-an-efficient-developer-71c829c76417)。Meteor 是一个成熟的 Node.js 框架，非常适合 SaaS、实时应用、原型、管理软件和许多其他用例。

默认情况下，它与 MongoDB 一起工作。MongoDB 是一个 NoSQL 数据库管理系统(DBMS)。它将文档存储为 JSON，其外壳使用 JavaScript，这也是 Node.js 开发人员如此青睐它的原因。

有一个叫做 [Meteor Up](http://meteor-up.com/) 的工具，可以在生产服务器上自动部署 Meteor 应用程序和相关的 Mongo 数据库。整洁实际上是一个轻描淡写，*它是令人敬畏的。*

我也是一名 Python 开发者。Python 对于 web 抓取和数据科学来说非常优秀。它既易于使用，又适合高性能。

有时候，我喜欢同时使用 Python 和 Meteor 来提取和处理数据，并创建用户界面。我称之为“全面数据开发”，因为它从原始数据源到最终用户界面。

![FTRA0udkWF2CTm79qN6y82lab05vx3Sy-f9S](img/ff52e6ef6e870d35a96c640611c1a671.png)

The sacred three of the full data developer: Meteor, Mongo, Python

**但是有一个问题:** Meteor Up 目前不暴露 Mongo 数据库，只有本地的 Meteor app 可以连接。所以我不能把我的 Python 服务器插入到我的 Meteor 管理的数据库中:(

付费服务将通过提供托管数据库的 URL 来完成这项工作，如下所示:

```
mongo://username:password@somedomain.com:27017 
```

但是，当你可以花几个小时在 AWS 上使用 Docker 建立自己的 Mongo 数据库，并在这个过程中学习大量有用的技巧时，为什么还要花钱呢？如果有人，比如说我，先做这件事，然后写一篇冗长的教程来让你轻松一些，那就更好了！

*重要提示:*如果你要花很多时间来完成这篇冗长的教程，那也没关系。不要放弃！结果是值得的，因为掌握 Docker 和 AWS 是雇主非常欣赏的两项技能，在现实生活中非常有用。如果你有任何问题，请发表评论，我会尽我所能回答所有的问题。

![g0v26UG99TkEksA8pBDgkjcEzSH9LnLxWe9a](img/0b29f5c8e43edf56d150cea0f6b1a84a.png)

### 1 —让我们发现 Docker

#### 我们的第一个容器

我邀请你在这里阅读 [Docker 的官方安装文档并安装，只需要几分钟。那么，我们来玩一点。在您的终端中运行以下命令并观察结果:](https://docs.docker.com/get-started/)

```
docker run --name my-lame-db -d mongo
docker ps
```

![xCHkKOqT4f-VdvcBzbllxKAdAvqgcO-1uIDr](img/df767c737e70f609d1a1cb12610b9abd.png)

就这么简单，我们只是创建了一个运行 Mongo 的隔离容器！

你可以在 GitHub 上的这个文件夹中的某个地方[访问官方的 Mongo image Docker 文件，以更好地了解这里发生了什么。根据](https://github.com/docker-library/mongo) [Dockerfile](https://github.com/docker-library/mongo/blob/master/4.1/Dockerfile) 的最后几行，我们的 db 默认在端口“27017”上可用:

```
EXPOSE 27017 CMD ["mongod"] 
```

但是 Mongo 将自己暴露在容器内部的这个端口上。但是容器是隔离的，所以只有容器内部的程序才能和 Mongo 对话。我们的数据库已经启动并运行，但是却被困在它的容器中:/

那是蹩脚的！让我们释放它！

#### 打开集装箱，标出港口

如果想从容器外部访问 Mongo，就必须映射暴露的端口和机器的一个端口。`-p`选项专门用于此:

```
docker run -p 27017:27017 --name my-local-db -d mongo 
```

如果你有一个节点服务器，你可以写`docker run -p 80:3000 my-node-app`作为例子。因此，运行在端口 3000 上的服务器可以通过 HTTP(端口 80)访问。首先是容器端口，然后是图像端口。

让我们试着在浏览器中访问我们的数据库，只是为了好玩:

![xfsKKgSd0F9AGSpXXwrciUTUH2rLwsPrWnPj](img/509f4c743319d134daa107800c91b27c.png)

Nice! Our DB is running! You can try `docker stop my-awesome-db` and `docker start my-awesome-db` commands. You’ll see this address becoming not available and then available again.

不出所料，您无法通过浏览器连接到您的 db。但是这个拒绝消息来自 Mongo，这是个好兆头。让我们使用官方 CLI 工具再试一次:

```
mongo localhost:27017 #or just "mongo", as this is the default uri
```

您可以访问您的数据库外壳，我们正在取得进展！

但是…我希望我的数据库在远程服务器上，而不是在我的孤立机器上本地运行。

![7FeX2tR6yc47JAmWRjm6LFt4Y94Mf3iwUE7J](img/51c61941c0a1a1c1458d63845b151ef2.png)

### 2-从终端访问 AWS

我们选择 AWS 作为我们的云提供商是因为它分布广泛，但请记住这只是一个例子。AWS 为新用户提供一个服务器实例 12 个月的免费托管，所以你不需要付费来学习这个教程。如果你选择另一个主机服务，步骤是相似的。

第一步是使用 IAM 服务(身份和访问管理)创建对 Amazon Web Services 的编程访问。这个键将被 Docker Machine(见下文)用来做一些操作，比如为你创建一个 AWS EC2 实例。

设置这个超出了我们教程的范围。因此，我邀请你阅读 Vishal Kumar 的这篇文章的第一部分。遵循前 8 个步骤，直到您获得 AWS 凭证。文章的其余部分也很有趣，但在这一点上有点超前，并且针对平均堆栈。让我们暂时只关注 Mongo。

此时，你应该有你的证书。

```
[default] 
aws_access_key_id = [access key from the downloaded credential file] 
aws_secret_access_key = [secret access key from the downloaded credential file]
```

保证他们的安全！如果您丢失了密钥对，您将不得不重新创建一个……而且您肯定也不希望任何人发现它们！

![HK4A0Xh2ZoWhu74aTjMdBPjNyPOu9w5J5TXN](img/28962df8b0caa3b7887cfebfc0a3ce3f.png)

### 3—使用 Docker Machine 将 Docker 放在云中

#### 再也不需要 AWS 控制台

Docker Machine 是一个管理托管容器的不同机器(本地机器、云服务器)的工具。因此得名，Docker… Machine。您可以为多个云提供商设置它，它与 AWS 配合得非常好。

官方文档给出了 AWS 所需的所有信息。我只是跳过了 VPC 的部分，这对于现在来说有点太高级了，但是剩下的部分非常有用。

#### 创建 EC2 实例

您将最终编写一个类似于下面的命令。

```
docker-machine create \
--driver amazonec2 \
--amazonec2-access-key ***** --amazonec2-secret-key **** \
--amazonec2-region ***** \
--amazonec2-open-port 27017 my-awesome-server
```

我们来分解一下。

*   我使用 EC2 驱动程序，因为我是 AWS 用户。
*   我以内联方式传递凭证，因为我有多个帐户要管理
*   我经过我通常托管应用程序的地区(对我来说是“欧盟-西方-3”)，
*   不要忘记打开端口 27017 ( `--amazonec2-open-port 27017`)，否则即使您的容器设置正确，AWS 也会阻塞连接。

如果没有正确指定区域，您可能很难在 AWS 控制台上找到您的实例。我仍然不明白为什么你不能在这个界面中轻松地显示所有的区域，但这就是它的工作方式。

您还应该能够通过设置本地机器 AWS 凭证，或者将它们存储为环境变量，以某种方式消除访问密钥和秘密密钥。我个人更喜欢在命令行中使用它们，因为我可能会用环境变量替换它们。

![mZheBUCZ7JyCgxueh060ejYwNKpetat87YQA](img/4b5526a1e7bc6691388d234ef10bbb7d.png)

**此时，您可以检查 AWS 控制台，查看您的实例设置是否符合预期！**

![sKiD1G-AvQe43Ch54y1FXOqT2opVHCGh3wJh](img/79782c539eec6e3b8db81818ffd3eb6e.png)

### 4 —在您的实例上运行 Mongo 容器

#### 启动正确的机器

现在棘手的部分，我花了一段时间才弄对。Docker Machine 的作用是管理你的远程机器，启动实例并在其上安装 Docker。仅此而已。不是 Docker。

所以还是要用 Docker。但是 Docker 怎么知道要连接哪台机器呢？

试试这个，看看结果:

```
docker-machine env my-awesome-server 
```

它将显示一个小的 shell 脚本来设置环境变量。

![YjV8Su28WDjQfeUJOwvqBUj7j-bURkNq5eYb](img/d2029bad507a7773b6f16ec200cf02a7.png)

它还告诉您运行以下命令:

```
eval $(docker-machine env my-awesome-server)
```

这将只是在您的 shell 中运行显示的脚本。当这些 env 变量被设置时，你的机器就变得“活跃”了。

键入`docker-machine active`检查是否列出了正确的机器。

![3ySinraesoaVe-SxlGzHhCjfuueAdzkvRCqv](img/e750acf7f4b46aa5af35f5d7edcc151a.png)

\o/

现在输入`docker info`，你应该会看到`Name`与你的应用匹配。**魔法！** Docker“连接”到活动机器，由 Docker Machine 配置。

我们现在可以运行与前面相同的命令:

```
docker run -p 27017:27017 --name my-awesome-db -d mongo .
```

它将不会在本地机器上创建数据库，而是在远程服务器上创建。就这么简单！

运行以下命令获取您的计算机 IP:

```
docker-machine ip my-awesome-server
```

然后打开地址`http://<ip-given-by-docker-machine>` :27017:你应该会得到一个漂亮的错误信息，告诉你你正在尝试通过 HTTPS 访问 MongoDB:成功了！！！

跑`mongo <ip-given-by-docker-machine>` :27017 … **然后…你就进去了！恭喜你，你刚刚在一个生产服务器**上设置了你的 Docker 容器。

现在，我们肯定不希望全世界都访问我们的数据库，所以下一步是设置身份验证。

![vSa6Azoo6UzMD5zXyH-OuwJLxU0w3Dr0sXKW](img/c78e1ee2f788e39aef04ac7de7c3c689.png)

### 5 —设置身份验证—双容器解决方案

抱歉，您已经可以删除刚刚创建的容器了(练习:我让您弄清楚停止和删除容器的命令)。这只是一个例子。遗憾的是，您将无法用它设置 auth。

保持专注，因为这个过程比你想象的要复杂一些:

*   我们将创建第一个容器，我们将其命名为“容器#1”，不带 auth。
*   我们将设置这个容器，使数据保存在服务器磁盘上(这是一个重要的步骤)，创建和管理，并删除容器。
*   我们将创建第二个容器，即带有 auth 的容器#2。管理员用户凭据仍然有效，因为它们保存在磁盘上。

可能有我还不知道的更简单的解决方案，例如通过在容器创建期间提供管理员用户凭证，所以如果你是 Docker/Mongo 超级英雄，请随意评论！并且在文章结尾的附件中也提供了一个容器解决方案。

#### 在容器之间共享数据

在这一点上，你应该想“容器是隔离的，那么容器#1 和容器#2 怎么能共享同一个管理员用户呢”？你是对的。解决方案在于将文件存储在服务器上。

还记得我们映射端口时，让容器的开放端口映射 Mongo 的端口吗？我们将对文件系统应用相同的逻辑:我们可以映射容器上的文件夹和服务器上的文件夹。

让我们为我们的命令添加一个选项，我们称之为容器#1 的最后一个命令**:**

```
docker run \
-d \
-p 27017:27017 \
--name my-awesome-db \
-v ~/dataMongo:/data/db mongo \
mongod
```

聪明！**现在，当我们的 Mongo 容器在`data/db`中读/写它们的数据时，它们也可以被主机服务器**在`~/dataMongo`文件夹中使用(首先是主机文件夹，然后是容器文件夹)。

#### 创建管理员用户

这一切都在标题中:您现在将连接到您新创建的 Mongo 实例并创建 admin 用户。

```
docker-machine ip my-awesome-server
mongo <resulting-ip>
```

现在，您应该连接到您的数据库 shell 了。您只需要创建一个超级管理员用户:

```
db.createUser(
  {
    user: "admin",
    pwd: "yourpassword",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ],
    passwordDigestor : "server"
  }
)
```

可以看看[官方文档](https://docs.mongodb.com/manual/tutorial/enable-authentication/)、[本文](https://ianlondon.github.io/blog/mongodb-auth/)和[本栈溢出线程](https://stackoverflow.com/questions/34559557/how-to-enable-authentication-on-mongodb-through-docker/46645243#comment93826825_46645243)了解更多信息。

另外，[看这个帖子](https://stackoverflow.com/questions/23943651/mongodb-admin-user-not-authorized)关于`root`角色和`userAdminAnyDatabase`角色之间的区别，前者是一个真正的超级管理员，后者是一种“用户创建者”。

#### 删除容器并激活授权

我们可以删除容器#1，它的作用只是让我们访问我们服务器上的 Mongo shell，但是它缺乏认证。

```
docker stop my-awesome-db
docker rm my-awesome-db
```

然后，我们可以创建最终的容器，并激活身份验证。因为它也将连接到服务器文件系统，所以使用容器#1 创建的数据仍然可用，包括我们的 admin 用户。

我们只需在初始命令中添加`--auth`选项，它告诉 Mongo……嗯，启用认证。你猜对了。

**容器#2 的最终命令:**

```
docker run \
-d \
-p 27017:27017 \
--name my-awesome-db \
-v ~/dataMongo:/data/db mongo \
--auth
mongod
```

现在再次连接到您的 Mongo 实例并运行:

```
db.createCollection('IAMAHACKER')
```

您将得到一条预期的好的错误消息！

![vPDNkLmy6ItmfpXUyiQp7TkMYU4FrqcBnGC3](img/58e087fa8a4c4a722ae6353bf7a8b799.png)

现在，请在通过身份验证后重试:

```
mongo $(docker-machine ip my-awesome-server):27017 -u admin -p yourpassword
```

如果成功了，你就大功告成了！恭喜你；)

### 现在怎么办？

您的 Mongo 数据库在云中运行，安全地隔离在 Docker 容器中，并通过用户名/密码身份验证来保护。

接下来的步骤是将您的应用程序插入到这个数据库中。请记住，您可以使用命令`docker-machine ip my-awesome-server`访问您的服务器 IP。您肯定需要创建额外的用户来管理您的数据库。

此外，您可能不希望任何人连接到您的数据库 shell，即使设置了身份验证。AWS 将允许您将一些 IP 列入白名单，对应于您的应用程序和您自己的计算机，以便只有可信的来源可以连接到您的数据库。

在本文下面，您会发现一个术语表、最终脚本、一个设置身份验证的单一容器解决方案，以及一些额外的技巧。

我希望这篇教程对你有所帮助！如果是这样，别忘了鼓掌；)并请查看下面的附件…

[![HbDwt-Vv483gfb6SdD1uCZQ8YyifxJYOE6AY](img/78fd81b8f71db8d99aef156db5c1950a.png)](https://twitter.com/LBKE_FR)

### 附件 1 —最终脚本

```
# Launch an EC2 instance
docker-machine create \
--driver amazonec2 \
--amazonec2-access-key ***** --amazonec2-secret-key **** \
--amazonec2-region ***** \
--amazonec2-open-port 27017 my-awesome-server
# Activate the instance
eval $(docker-machine env my-awesome-server)
# Create container 1
docker run \
-d \
-p 27017:27017 \
--name my-awesome-db \
-v ~/dataMongo:/data/db mongo \
mongod
# Connect to your DB
mongo $(docker-machine ip my-awesome-server):27017
# IN THE MONGO SHELL create a super admin
db.createUser(
  {
    user: "admin",
    pwd: "yourpassword",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ],
    passwordDigestor : "server"
  }
)
# QUIT THE MONGO SHELL
# Delete the container and recreate one with auth
docker stop my-awesome-db
docker rm my-awesome-db
# Run the final container
docker run \
-d \
-p 27017:27017 \
--name my-awesome-db \
-v ~/dataMongo:/data/db mongo \
--auth \
mongod
# IF CA FAILS WITH PERMISSION DENIED ERROR
# see https://github.com/kubernetes/minikube/issues/3083
# sudo vim /var/lib/snapd/apparmor/profiles/snap.docker.docker
# Add following line inside file (e.g alongside with other "owner" lines): 
# owner @{HOME}/.docker/machine/machines/** r,
# sudo apparmor_parser -r /var/lib/snapd/apparmor/profiles/snap.docker.docker
```

### 附件 2——我希望我不必自学的巧妙技巧

*   如果你在 devops，你最终会得到一个 Docker 命令的肌肉记忆。否则，如果您一年只为生产设置一次项目，请记下所有内容。例如，你可以写一篇中等水平的文章。说真的，我已经损失了几个小时的工作，因为我没有写下我的进展。接下来的一周，我什么都忘记了。Docker 不是无足轻重的。
*   我在使用 Docker 机器的时候遇到了一个权限问题，解决方案在这个帖子的评论里:[https://github.com/kubernetes/minikube/issues/3083](https://github.com/kubernetes/minikube/issues/3083)
*   `docker ps -a`将打印所有集装箱，**包括未激活的集装箱，**仅打印激活的集装箱。
*   图像不是 Dockerfile 文件。**docker file 是一个配置文件**。想象一艘船的蓝图。**图像是一个构建的 Docker 文件。**想象一艘尚未出海的船。**容器是你正在运行的图像。想象一艘漫游 SEAS 的船。**

[**Docker 文件、Docker 映像和 Docker 容器的区别**](https://nickjanetakis.com/blog/differences-between-a-dockerfile-docker-image-and-docker-container)
[*快速跳转:这一切都始于 Docker 文件|如果你构建了它，它们将(通常)在随意的对话中运行你可能……*nickjanetakis.com](https://nickjanetakis.com/blog/differences-between-a-dockerfile-docker-image-and-docker-container)

*   例如，将拉一个 Mongo Docker 图像，已经建成。但是如何显示关联的 Dockerfile 呢？你不能。想查的话可以找相关的 GitHub 库(如果有的话)。这正是已安装的 NPM 软件包和它的代码之间的关系。`npm install`安装一个构建版本(一个“映像”)，但不一定下载代码(这里是一个“Dockerfile”)，它通常位于 GitHub 上，但也可能是私有的。

[**如何查看图像中的 Dockerfile？**](https://forums.docker.com/t/how-can-i-view-the-dockerfile-in-an-image/5687/3)
[*嗨，看着资源库里的一些图片(比如这张:https://hub.docker.com/r/filippobosi/mfi/)我愿意…*forums.docker.com](https://forums.docker.com/t/how-can-i-view-the-dockerfile-in-an-image/5687/3)

*   在 EC2 上，您的实例 IP 可以在重启时改变！您必须使用`docker-machine regenerate-certs my-awesome-server`重新生成证书以保持与实例的连接

### 附录 3—设置认证—一个容器解决方案

还有一个“一个容器”的解决方案。它不太有指导意义，但是更快，并且只需要一个容器。

注意:如果您已经使用 2 容器方法设置了 auth，那么您已经完成了。如果您仍然想尝试 1 容器方法，您需要在 EC2 上创建一个新实例，或者删除服务器的`~/dataMongo` 文件夹。

您仍然需要与主机共享您的文件夹，否则您的数据会被绑定到容器，这是不应该发生的！容器应该很容易删除而不会丢失任何数据，因此文档必须存储在其他地方。

这个想法是连接到您的服务器并从那里访问 Mongo，而不是从您的本地机器访问 Mongo。这是一个非常微妙的区别，但这让我们省了一步。

如果你在连接到你的服务器的同时连接到 Mongo，你将有更多的权限，即使已经设置了 auth，你也可以设置一个 admin 用户，因为 Mongo 认为你是一个“本地”用户。换句话说，既然你能够连接到运行 Mongo 的机器，也就是你的 AWS 实例，Mongo 已经认为你是一个“安全的”用户，因为黑客首先不应该出现在那里。

仅运行容器#2 的**最终命令**

```
docker run \
-d \
-p 27017:27017 \
--name my-awesome-db \
-v ~/dataMongo:/data/db mongo \
--auth
mongod
```

您可以随时使用`docker-machine ssh`ssh 您的服务器:

```
docker-machine ssh my-awesome-server
```

运行`mongo`将打开 Mongo shell。在那里，即使设置了 auth，您也可以添加管理员用户，就像我们之前所做的那样。

### 附件 4—术语表

一点 devops 人喃喃自语。

AWS: Amazon Web Services，一个著名的廉价云服务集合。

**AWS EC2:** 用于托管服务器实例的服务。那是你托管你的 API 或者你的整个网站的地方。注意，可能有更适合数据库托管的服务，但是因为 EC2 是必须知道的，所以它是我作为初学者的选择。

Docker: 这是一个……嗯，一个程序？简而言之，这是一堆帮助你在容器中运行程序的东西，与服务器上运行的其他程序隔离，而不需要像虚拟机那样耗费大量资源。容器也有助于管理服务(API、数据库等)。)每天:终止/重启它，在一行中创建新实例…

容器:这将是我的简化定义，想象一个没有操作系统的虚拟机。它提供了隔离，同时不会消耗太多的计算资源。

Dockerfile: 这是 docker 的配置文件，它定义了运行你的应用程序所需的所有程序/文件/命令/选项。

Docker Machine: 一个CLI，把你的 Docker 容器推到你想要的地方，在云中或者在你自己的服务器上。

我是法国公司 le brun Burel Knowledge Engineering(LBKE)的联合创始人——[https://www . LBKE . fr](https://www.lbke.fr)

*总是乐于谈论代码、机器学习、创新创业！*