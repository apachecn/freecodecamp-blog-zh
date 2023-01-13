# 如何使用 Docker 和 NGINX 以及 API 代理将 React 应用程序部署到生产环境中

> 原文：<https://www.freecodecamp.org/news/how-to-deploy-react-apps-to-production/>

这篇文章将帮助您学习如何将 React 应用程序部署到生产环境中。我们将使用 Docker 和 NGINX 来保护 API 密钥和代理请求，以防止跨源资源共享(CORS)违规。

你可以在最后的总结里找到**代码**和**视频**。

### 你将从这篇文章中学到什么

在每个项目的生命周期中，都到了发布的时候，但如何发布并不总是那么明显。生产环境不同于开发环境，用户不需要额外的步骤来运行它。大多数 web 应用程序都使用某种 API，并且通常托管在不同的服务器上。

在这种情况下，作为开发人员，我们需要解决跨源资源共享(CORS)问题。太多时候，我们最终会构建一个后端，尽管这并不是必需的。我认为开发人员应该保持他们的应用程序简单，并删除所有多余的部分。

在本文中，我将向您展示我是如何准备 React 应用程序以将其部署到生产环境中的。

我可以构建一个简单的 React 示例应用程序，但它不会很有帮助。所以我决定将我的应用程序与圣路易斯联邦储备银行提供的一个真正的 API 挂钩。API 需要访问密钥来检索数据，并且端点受到保护，不会受到跨域请求的影响，外部 web 应用程序无法直接使用数据。

**注意**:如果你的应用依赖于**服务器端渲染**，这是**而不是正确的**部署策略。你可以得到灵感，但你仍然需要某种后端。

## 先决条件

掌握一些如何构建 React 应用程序的基本知识至关重要。在按照本文中的说明进行操作之前，您还应该了解一些 Docker 的基础知识。

如果你错过了什么，不用担心！只需查看这篇令人惊叹的文章和 YouTube 上关于 FreeCodeCamp 的教程:

*   [由](https://www.freecodecamp.org/news/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b/) [@iam_preethi](https://twitter.com/iam_preethi) 撰写的关于容器、虚拟机和 Docker 的初学者友好介绍
*   [创建 React 应用速成班](https://www.freecodecamp.org/news/create-react-app-crash-course/)

## 如何构建示例 React 应用程序

我使用 *create-react-app* 引导了一个简单的 web 应用程序。该应用程序唯一的工作是显示一个代表美国 GDP 的折线图。

![fredapp](img/b5d56f71a2fa95f1f48726b3fe29c2d3.png)

该应用程序仅从以下 API 中检索数据:

```
https://api.stlouisfed.org/fred/series/observations?series_id=GDPCA&frequency=a&observation_start=1999-04-15&observation_end=2021-01-01&file_type=json&api_key=abcdefghijklmnopqrstuvwxyz123456
```

以下是参数:

*   `series_id` -系列的 id。`GDPCA`代表“实际 GDP”。
*   `frequency` -数据的聚合。`a`代表年度。
*   `observation_start` -观察期的开始。
*   `observation_end` -观察期的结束。
*   `file_type` -数据的格式。默认为`xml`。
*   `api_key` -从该 API 中检索任何数据所需的访问键。你可以在这里申请[一个。](https://fred.stlouisfed.org/docs/api/api_key.html)

您可以在[文档](https://fred.stlouisfed.org/docs/api/fred/series_observations.html)中找到更多详细信息。

生活并不总是完美的，API 设计也不理想。它要求开发人员将访问键和数据的预期输出作为 URL 参数传递。

将输出作为参数传递对我们来说不是问题，因为这只会增加一些噪音——但是泄漏的 API 键是问题。想象一下，如果有人拦截它们并滥用 API 来执行一些禁止的操作。我们不想冒这个险。

让我们暂时假设 API 键不是问题。尽管如此，还是不可能利用这个 API。FRED API 受到跨域请求的保护，因此如果我们尝试从外部域调用它，将会出现以下错误:

![frederror](img/5be142c0929afac15c520dbf146d59c2.png)

许多开发人员会建议构建中间件(后端)来代理对 API 的请求并过滤敏感数据。他们会说他们将来可能需要增加新的功能，在某种程度上，这是一种公平的方法。

但是我更喜欢用更 YAGNI 的方式来开发我的应用程序(你不会需要它的)。因此，我将避免构建后端，直到它是必要的——在我们的例子中，我根本不会构建它。

## 还是用 NGINX 吧！

我是 NGINX 的忠实粉丝，因为它带来了简单性。NGINX 拥有您准备生产级 web 服务器所需的所有功能，比如 HTTP2、压缩、TLS 和许多其他功能。

最重要的是，我们可以通过定义几行配置来实现这一切。只需看一下下面的片段:

```
...

http {
    ...

    server {
        ...

        location /api {
            set         $args   $args&&file_type=json&api_key=abcdefghijklmnopqrstuvwxyz123456;
            proxy_pass  https://api.stlouisfed.org/fred/series;
        }
    }
}
```

我只需要这 4 行代码来隐藏我们的 API 密匙并抑制 CORS 错误。字面意思！从现在开始，所有对`/api`的 HTTP 请求都将被代理到 FRED API，只有我们的应用程序能够使用这个 API。所有外部请求都将面临 CORS 错误。

> 为了摆脱混乱，我用`...`(三个点)替换了文件的所有默认内容。你可以在我的 **GitHub** 或者**视频**上找到完整版本(链接如下)。

这是我们终点的样子:

```
/api/observations?series_id=GDPCA&frequency=a&observation_start=1999-04-15&observation_end=2021-01-01
```

我们既不需要传递`api_key`也不需要传递`file_type`参数来检索数据。没有人能从 URL 中读取访问密钥，所以它是安全的。

![Screen-Shot-2021-03-14-at-12.49.55](img/5070774bbd09c47218ddb1d8e148a44c.png)

## 码头工人 loves nginx

在云中运行 NGINX 最方便的方法是使用 Docker。对于这一部分，我假设您知道 Docker 是什么(如果不知道，请阅读先决条件中链接的文章)。

我们只需要创建一个 Dockerfile 文件，包含以下内容:

```
FROM nginx

COPY container /
COPY build /usr/share/nginx/html
```

现在，运行 FRED 应用程序只需三个步骤:

1.  构建 React 应用程序。这个过程生成包含静态文件的`build/`目录。
2.  *建立 Docker 镜像*。它将创建一个可运行的 Docker 映像。
3.  *将 Docker 镜像*发布到某个存储库或者*在本地机器上运行*。

现在，让我们试着在我们的机器上运行它。

```
$ yarn install
$ yarn build
$ docker build -t msokola/fred-app:latest .
$ docker run -p 8081:80 -it msokola/fred-app:latest
```

`8081`是你机器上的一个端口。这意味着该应用程序将在以下 URL 下可用:`http://localhost:8081`。

在浏览器中打开该 URL 后，您应该会在终端中看到如下日志:

```
0.0.0.1 - - [11/Mar/2021:18:57:50 +0000] "GET / HTTP/1.1" 200 1556 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 11_2_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.192 Safari/537.36" "-"
...
0.0.0.1 - - [11/Mar/2021:18:57:51 +0000] "GET /api/observations?series_id=GDPCA&frequency=a&observation_start=1999-04-15&observation_end=2021-01-01 HTTP/1.1" 200 404 "http://localhost:8081/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 11_2_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.192 Safari/537.36" "-"
```

注意那些代表 HTTP 状态 OK 的`200`。如果您在 API 请求旁边看到一个`400`，这意味着您的 API 键有问题。`304`也很好(这意味着数据被缓存了)。

## 如何在 AWS 上部署容器

容器正在工作，所以我们可以部署它。在文章的这一部分，我将向您展示如何在 Amazon Web Services (AWS)中运行您的应用程序。

AWS 是最受欢迎的云平台之一。如果你想使用微软 Azure 或任何其他平台，步骤将是相似的，但命令的语法将有所不同。

**请注意:**我在 YouTube 上录制了一段视频，您可以观看我完成整个部署过程。如果您遇到困难或遇到任何问题，您可以检查我们在每个步骤中是否有相同的结果。如果你想看视频，[点击这里](https://youtu.be/bUSXeQ4H20g)或者你可以在下面的*摘要*中找到。

### 1.安装 AWS CLI 工具

在我们开始之前，您需要安装 [AWS CLI](https://aws.amazon.com/cli/) 工具，这样您就可以在您的云上调用命令。

AWS 为所有操作系统提供安装向导，所以我将跳过这一节。成功安装后，您必须通过键入以下命令登录:

```
$ aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-east-2
Default output format [None]: json
```

要生成访问密钥，您需要登录 AWS 控制台。在那里，点击您的用户名，并选择“*我的安全凭证*”。

![Screenshot-2021-03-16-at-22.27.53-1](img/5fdaa4c418060e0906b80e683b599c04.png)

### 2.创建新的弹性容器注册表(ECR)

设置好 CLI 工具后，我们需要创建一个空间来存储应用程序的可执行文件。我们使用 Docker，所以我们的可执行文件将是 Docker 映像，我们将在虚拟机上运行。

AWS 提供了一个专门的存储图像的服务，叫做弹性容器注册。以下命令将为我们创建一个:

```
aws ecr create-repository --repository-name react-to-aws --region us-east-2
```

以下是参数:

*   `ecr`—“弹性容器注册表”的首字母缩写。
*   我们注册中心的名称。请记住，我们以后会提到这个名字。
*   `region` -地区代码。您可以找到一个离您最近的区域来减少延迟。这是所有地区的[列表。](https://docs.aws.amazon.com/general/latest/gr/rande.html)

您可以在[文档](https://docs.aws.amazon.com/cli/latest/reference/ecr/create-repository.html)中找到更多详细信息。

这是预期的输出:

```
{
    "repository": {
        "repositoryArn": "arn:aws:ecr:us-east-2:1234567890:repository/react-to-aws2",
        "registryId": "1234567890",
        "repositoryName": "react-to-aws",
        "repositoryUri": "1234567890.dkr.ecr.us-east-2.amazonaws.com/react-to-aws2",
        "createdAt": "2021-03-16T22:50:23+04:00",
        "imageTagMutability": "MUTABLE",
        "imageScanningConfiguration": {
            "scanOnPush": false
        },
        "encryptionConfiguration": {
            "encryptionType": "AES256"
        }
    }
}
```

### 3.将 Docker 映像推送到云

在这一步中，我们将把 Docker 映像推送到云中。我们可以通过从 AWS 控制台复制 push 命令来实现。

让我们在浏览器中打开 *AWS 控制台*，在“*所有服务-容器*列表中点击*弹性容器注册表*。如果您没有更改您的地区，您可以[点击此处](https://us-east-2.console.aws.amazon.com/ecr/repositories?region=us-east-2)。您将看到您的存储库的完整列表:

![Screenshot-2021-03-16-at-23.00.24-1](img/eb53e0462dbb3182b67ae27d2d9ed10e.png)

现在你需要选择`react-to-aws`库，然后从菜单中选择*查看推送命令*(在上图中用红圈标出)。您将看到以下窗口:

![Screenshot-2021-03-16-at-23.08.49](img/fef3a17bc14776dead8df64a24dcc407.png)

您需要将所有命令从模态复制到您的终端。 ***不要*从**下面的片段中复制命令，因为它不起作用。

```
$ aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin 123456789.dkr.ecr.us-east-2.amazonaws.com
Login Succeeded

$ docker build -t react-to-aws .
[+] Building 0.6s (8/8) FINISHED
...

$ docker tag react-to-aws:latest 123465789.dkr.ecr.us-east-2.amazonaws.com/react-to-aws:latest

$ docker push 123456789.dkr.ecr.us-east-2.amazonaws.com/react-to-aws:latest
The push refers to repository [123456789.dkr.ecr.us-east-2.amazonaws.com/react-to-aws:latest]
...
latest: digest: sha256:3921262a91fd85d2fccab1d7dbe7adcff84f405a3dd9c0e510a20d744e6c3f74 size: 1988
```

现在你可以关闭模态，并点击储存库的名称(`react-to-aws`)来浏览可用图像的列表。您应该会看到以下屏幕:

![Screenshot-2021-03-16-at-23.17.56-1](img/a34f066f0b8f901f703a476c498c8151.png)

您的应用程序已经在存储库中，可以部署了！现在点击“*复制 URI”***并保存你的剪贴板内容**(粘贴到一些记事本或文本文件中)，因为我们将需要运行它！

### 4.配置应用程序

我们的映像在云中可用，所以现在我们需要配置它。

虚拟机不知道如何执行您的映像来确保它正常工作。我们必须定义一些指令，比如开放端口、环境变量等等。AWS 称之为任务定义。

打开 *AWS 控制台*，在*所有服务-容器*列表中点击*弹性容器服务(ECS)* 。如果您没有更改您的地区，您可以[点击此处](https://us-east-2.console.aws.amazon.com/ecs/home?region=us-east-2#/clusters)。

现在选择*任务定义*，点击*新建任务定义*，如下图所示:

![Screenshot-2021-03-17-at-00.07.54-1](img/660132b49a95f029938093f373863223.png)

我们有两个运行任务的选项:`FARGATE`和`EC2`。选择`FARGATE`、，点击*下一步*。

![Screenshot-2021-03-17-at-00.09.53](img/e9b351f3291caf1244035afc532fef9d.png)

在下一步中，您需要用以下值填写表单:

*   **任务定义名称** - `react-to-aws-task`。
*   **任务角色** - `none`。
*   **任务记忆(GB)** - `0.5GB`(最小)。
*   **任务 CPU (vCPU)** - `0.25 vCPU`(最小)。

一旦到达“*容器定义”*部分，点击*“添加容器”*:

![Screenshot-2021-03-17-at-00.18.19](img/401bb6ffdbadf46b3a51f5840560ba6d.png)

用以下值填写表单:

*   **集装箱名称** - `react-to-aws`。
*   **图片**——第四步中的 URI。你把它贴在什么地方了。
*   **内存限制(MiB)**-`Soft limit`-`128`。
*   **端口映射**-`80`-HTTP 端口。

其他选择与我们无关。现在点击*Add*按钮添加一个容器，点击 *Create* 完成任务定义。您应该看到以下屏幕，并点击“*查看任务定义*”。

![Screenshot-2021-03-17-at-00.24.56](img/b58b2e9a5b894716bef6b7e600a7c6fb.png)

### 5.让我们运行它！

最后，我们可以创建一个集群，这样我们就可以在云中运行我们的应用程序。您需要从左侧的菜单中选择“*集群*”，然后选择“*创建集群*”。如下图所示:

![Screenshot-2021-03-17-at-00.29.46](img/94c2acb5f950c02cc45be8d059c3df85.png)

现在我们有三个选项:`Networking only`、`EC2 Linux + Networking`和`EC2 Windows + Networking`。选择第一个- `Networking only`，点击*下一步*。您应该会看到以下屏幕:

![Screenshot-2021-03-17-at-00.34.35](img/2432030db6ef5d6cc3259aeea0989f02.png)

输入集群名称`react-to-aws`，点击*创建*按钮。您应该会看到一个成功的午餐状态。它看起来类似于任务定义创建后我们得到的屏幕。现在点击*查看集群*。

![Screenshot-2021-03-17-at-00.39.42-1](img/7803bbac9ad03e4e474416e092211f25.png)

现在你需要点击*任务*选项卡，然后点击*运行新任务*。恭喜你！您已经到达要填写的最后一张表格:)

![Screenshot-2021-03-17-at-00.41.10](img/355d2856b679941cb5410ac47652ff54.png)

用以下值填写表单:

*   **发射类型** - `FARGATE`。
*   **集群 VPC**——第一个。
*   **子网**——第一个。

**保持其他值为**，点击*运行任务*按钮。您应该会看到以下屏幕:

![Screenshot-2021-03-17-at-00.46.45-1](img/4040dbca14eeece525894cc02e31ef39.png)

我们需要等待大约一分钟，直到“*最后状态*变为正在运行。请注意，您需要点击*刷新*按钮来刷新列表。任务状态为“正在运行”后，单击任务名称。

![Screenshot-2021-03-17-at-00.50.52](img/e55332c4a6cee6fb47fffafd0fc176d3.png)

在“*网络”*部分你会找到你的容器的*公共 IP* 。你可以在你的浏览器中打开它，你会看到你的应用程序。

## 摘要

如果你刚刚开始你的职业生涯，你可能从来没有自己部署过应用程序。但是学习这种技能是有好处的，因为有一天你会需要这样做。

每个项目都需要面对用户，否则就没有成功的机会，也永远不会有回报。

> "停泊在港口的船只是安全的——但这不是建造船只的目的。"约翰·谢德

配置过程有点繁琐，但好消息是您只需要做一次。

完成所有配置后，接下来的部署将会更加简单。您只需要推送新的映像并重新启动任务来部署应用程序的新版本。

如果你有兴趣深入研究 AWS，FreeCodeCamp 提供了一个关于它的免费教程(大约 5 个小时)。

你可以在我的 **YouTube** 频道找到这个教程的**截屏(17 分钟)**。我在 YouTube 旅程的最开始- 至少每周我都会上传一个关于编程的视频。如果你看了我的截屏视频，订阅并点击“喜欢”按钮，那对我来说就是整个世界:)

[https://www.youtube.com/embed/bUSXeQ4H20g?feature=oembed](https://www.youtube.com/embed/bUSXeQ4H20g?feature=oembed)

**你可以在这个 GitHub 仓库里找到所有的代码**:【https://github.com/mateuszsokola/react-to-aws】T2

可以在 Twitter 上给我发 DM:[@ MSO kola](https://twitter.com/msokola)

那都是乡亲们！我希望你喜欢它，并有一个伟大的一天:)

![vidar-nordli-mathisen-xgP0GNl9Gzg-unsplash](img/506049dee66468c208269e67a392934c.png)

Photo by [Vidar Nordli-Mathisen](https://unsplash.com/@vidarnm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://www.freecodecamp.org/news/s/photos/ship-storm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)