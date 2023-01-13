# 使用 Kubernetes 部署一个聊天网关(或者当技术工作正常时)

> 原文：<https://www.freecodecamp.org/news/using-kubernetes-to-deploy-a-chat-gateway-or-when-technology-works-like-its-supposed-to-a169a8cd69a3/>

作者:李泽楷

# 使用 Kubernetes 部署一个聊天网关(或者当技术工作正常时)

![1*ZsohwEQHHw_sh0KX37pdeg](img/a00ab3adece8c89144d32fcd4255fa91.png)

Photo by [Top Five Buzz Travel Magazine](https://unsplash.com/photos/XbWMjPgHsPw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/cloud?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

### TL；速度三角形定位法(dead reckoning)

这是一个关于当云技术像预期的那样工作时会发生什么的故事。在这种情况下，技术是 Docker 和 Kubernetes。

### 介绍

在 [Datawire](https://www.datawire.io) ，我们使用 [Gitter](http://gitter.im) ，这样我们开源工具的用户可以和我们(以及彼此)聊天。我们喜欢 Gitter，因为加入它的摩擦很小，尤其是如果你有 GitHub 或 Twitter 账户的话。然而，Gitter 的移动客户端并不出色，通知功能也不太好。

由于迁移一个用户社区是一项相当大的工作，我决定看看是否可以在 Slack(我们内部使用的)和我们的 Gitter chat 之间部署一座桥梁。稍微搜索了一下，我就找到了马特布里奇。

在本文的其余部分，我将介绍 Docker 和 Kubernetes 是如何让我的生活变得更简单的，以及为什么。我发现所需的牦牛毛量非常少……这让我真正意识到我们在云世界已经走了多远。

### Docker 很棒

我想在本地运行 Matterbridge 来调试配置。我遵循了为 Slack 和 Gitter 创建帐户的一般说明，然后将必要的认证令牌放入一个`matterbridge.toml`文件中。

我很高兴看到 Matterbridge 是如何作为 Docker 映像可用的，所以我能够将我的配置文件复制到 Docker 映像中来测试配置。我需要做的就是在使用`docker run`时指定我的配置文件:

```
docker run -ti -v /tmp/matterbridge.toml:/matterbridge.toml 42wim/matterbridge
```

我不得不这样做几次来调试我的配置，但这很快也很容易。Docker 镜像做了它应该做的事情:为 Matterbridge 提供一个经过测试的运行时环境，这样我就不用在笔记本电脑上调试它了。

### 在云端运行

下一步是将我的配置部署到云中。我们已经运行了一个生产 Kubernetes 集群，其前端是由[特使](https://www.datawire.io/envoyproxy/)提供支持的 [API 网关](https://www.getambassador.io)，所以我想将服务部署到它自己的名称空间中。

为了部署到 Kubernetes，我编写了一个简单的 Dockerfile 文件:

```
FROM 42wim/matterbridge:1.9.0ADD matterbridge.toml .CMD ["/bin/matterbridge"]
```

然后，我只需要一份库本内特的货单。

### 库伯内特斯

在我的 Kubernetes 清单中，我可以指定一系列关于服务的关键信息:

*   我的更新策略，`RollingUpdate`
*   要分配的最小和最大 CPU 和内存量
*   服务的副本数量

这是我的基本清单:

```
---apiVersion: apps/v1beta1kind: Deploymentmetadata:  name: {{build.name}}  namespace: {{service.namespace}}spec:  replicas: 1  strategy:    type: RollingUpdate  template:    metadata:      labels:        app: {{build.name}}    spec:      containers:      - name: {{build.name}}        image: {{build.images["Dockerfile"]}}        imagePullPolicy: IfNotPresent        resources:          requests:            memory: 0.1G            cpu: 0.1          limits:            memory: {{service.max_memory}}            cpu: {{service.max_cpu}} 
```

(注意，我使用 [Forge](https://forge.sh) 来模板化和管理服务，所以这是一个模板化的 Kubernetes 清单)。

有了这个清单，我就可以在 Kubernetes 提供服务了。

### Git 中我的 Matterbridge 服务

总之，我的服务包括:

*   A Dockerfile
*   (模板化的)Kubernetes 货单
*   一个`matterbridge.toml`配置文件

所有这些我都可以签入 Git 存储库。

### 前库伯内特世界

我能够轻松可靠地获得服务，这让我反思在 VM 时代我会如何做这件事。我不得不:

*   调配虚拟机(和/或创建自动扩展组)
*   通过一些 bash 脚本工具在 VM 上安装必要的运行时依赖项(或者使用 Ansible，或者构建一个定制的 AMI)
*   将我的配置复制到虚拟机

此外，如果我想让上面的内容重现，我必须使用 Terraform、Ansible、CloudFormation 或这些类型的工具之一。以下是 Terraform 文档中关于如何创建 EC2 实例的示例:

```
# Create a new instance of the latest Ubuntu 14.04 on an# t2.micro node with an AWS Tag naming it "HelloWorld"provider "aws" {  region = "us-west-2"}data "aws_ami" "ubuntu" {  most_recent = true  filter {    name   = "name"    values = ["ubuntu/images/hvm-ssd/ubuntu-trusty-14.04-amd64-server-*"]  }  filter {    name   = "virtualization-type"    values = ["hvm"]  }  owners = ["099720109477"] # Canonical}resource "aws_instance" "web" {  ami           = "${data.aws_ami.ubuntu.id}"  instance_type = "t2.micro"  tags {    Name = "HelloWorld"  }}
```

作为一名开发人员，上面的许多选项(区域、实例类型)都是我不太关心的东西。然而，如果我想提交基础设施即代码，这是当前的抽象。

### 摘要

Kubernetes 不仅仅是一个容器调度器。它确实为您提供了控制服务运行方式所需的原语，而不会让您陷入部署的细节中。我们肯定[支持](http://www.datawire.io/faster/developer-experience/)让一个小团队的运营人员管理 Kubernetes 集群，而开发团队的其他人只是通过 Kubernetes 原语管理他们的服务。

虽然在 Kubernetes-land 并不总是阳光和玫瑰，但在我部署 Matterbridge 的情况下，它是。你将不得不等待另一篇关于挑战的文章！