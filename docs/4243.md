# 如何使用 Ansible 管理您的 AWS 资源

> 原文：<https://www.freecodecamp.org/news/ansible-manage-aws/>

难道你不想简单地挥动一下魔杖，AWS 帐户中的资源层就会突然神奇地变成配置完美的生命，准备好满足你复杂的基础设施需求吗？

如果你已经有了 AWS 的经验，那么你就会知道当你手动提供服务时，在 Amazon 管理控制台中浏览一个又一个网页是多么的痛苦。甚至 AWS CLI——这是一个巨大的进步——也会增加复杂性和工作量。

这并不是说 AWS 本身没有解决他们自己的强大编排工具类的问题，包括 CloudFormation 和他们的弹性 Kubernetes 服务(我在 Pluralsight 的[我的“在 AWS 上使用 Docker”课程中详细讨论了这一点)。但是这两种选择都不太接近您现有的基础设施，或者使用尽可能熟悉的操作方式。](https://pluralsight.pxf.io/nZgKx)

如果您已经在使用 Ansible 进行内部操作，将它插入 AWS 帐户有时可能是将操作迁移到云的最快捷、最轻松的方式。

### 了解 Ansible/AWS 的优势

我的书“[使用 Ansible](https://www.amazon.com/gp/product/B07YK42ZH1/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B07YK42ZH1&linkCode=as2&tag=projemun-20&linkId=d90b5a553223444f00992afa4c8f8d16) 管理 AWS 资源”——本文摘自该书——旨在快速向您介绍如何应用 Ansible 的*声明式*方法来使用 AWS 资源。能够“声明”您想要的精确配置结果，然后通过让 Ansible 阅读剧本来产生它们，这是 Ansible 的魔杖。如果计划得当，执行复杂的分层 AWS 部署会变得非常简单。

在我们推出一个简单的“Hello World”Ansible 剧本之前，让我们首先确保您已经有了一个正确配置的工作环境，通过它 ansi ble 可以与您的 AWS 帐户中的所有新朋友进行通信。

### 准备本地环境

您可能已经知道，Ansible 是一个编排工具，它让您编写纯文本的*剧本*文件，这些文件*声明*您想要应用到目标服务器的软件配置文件和理想状态。这些服务器被称为主机，可以使用任何应用软件组合，在任何平台上运行，为您所能想象的任何数字工作负载提供服务。

在过去，当剧本在物理服务器上运行时，Ansible 会使用现有的 SSH 连接安全地登录到远程主机，并着手构建您的应用程序。但是对于 AWS 工作负载来说，这是行不通的。您看，因为您想要启动的 EC2 实例和其他基础设施还不存在，所以不可能有“现有的”SSH 连接。相反，Ansible 将使用 Boto 3——AWS 使用的软件开发工具包(或 SDK ),它允许 Python 代码与 AWS API 通信。

### 使用 AWS CLI 连接 Ansible

你不必知道所有这些是如何工作的，但它必须在那里，这样它才能工作。因此，您将安装 AWS 命令行界面(CLI)。我们不会在任何重要的事情上使用 CLI 本身，但是安装它会给我们提供我们需要的所有依赖项。你可以从 [AWS 文档页面](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)找到如何在你正在使用的最新版本的操作系统上实现这一点。

使用 Python 包管理器 PIP 是完成所有这些工作的一种流行方式。下面是如何在 Ubuntu 机器上安装 PIP 本身和 AWS CLI:

```
sudo apt update
sudo apt install python3-pip
pip3 install awscli
```

我应该注意到，在我写这篇文章的时候，Python 2 还活着...但仅仅是。因此，有时您的系统上可能仍然会安装不同的 Python 2 和 Python 3 版本。由于 Python 2 将很快被完全废弃，您可能不必担心用命令指定 python3 或 pip3:这应该是自动的。

一旦安装了 CLI，运行`aws configure`并输入您的 AWS 访问密钥 ID 和秘密访问密钥。

```
aws configure
cat .aws/credentials
```

您可以从 AWS 管理控制台的“您的安全凭据”页面获取密钥。以下是这些键的外观(不要有任何淘气的想法，这些都是无效的):

```
AccessKeyId: AKIALNZTQW6H3EFBRLHQ
SecretAccessKey: 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
```

请记住，颁发给 AWS 帐户根用户的一对密钥提供了对整个 AWS 帐户的完全访问权。任何拥有这些凭证的人都可以很快获得六位数甚至七位数的服务费，所以在使用和保存这些凭证时要非常小心。理想情况下，通过在 AWS Identify and Access Management(IAM)服务中创建一个具有有限权限的管理员用户，并使用向该用户颁发的密钥，可以更好地限制您的风险暴露。

无论如何，我为什么要这样做？填充我的 AWS 凭证文件的价值在于 Ansible 足够聪明来寻找它，如果系统环境中没有其他认证密钥可用，它将使用这些密钥。你很快就会看到那会多么方便。但是，您应该知道管理 Ansible 剧本的身份验证的其他方法，如使用 *ansible-vault* 或通过创建并调用 aws_keys.yml 文件。但是有一件事你绝对不应该做，那就是在剧本文件中硬编码密钥——特别是如果你打算把它们推到一个像 GitHub 这样的在线存储库中。我将快速测试 CLI，以确保我们可以正确地连接到 AWS。这个简单的命令将列出我在这个帐户中拥有的任何 S3 存储桶。

```
aws s3 ls
```

我们现在准备安装 ansible。为此我会选择 pip3。我可以同样容易地使用常规的 Ubuntu apt 库，但是它很可能会安装一个稍微旧一点的版本。根据您的网络连接，这将需要一两分钟，但我会跳过大部分。

```
$ pip3 install ansible 
```

我将通过运行 ansible - version 来确认它是否正确安装。这向我们展示了构建的版本，配置的 Ansible 模块将默认保存在文件系统中这两个位置中的任何一个，其他模块将在这里可用，最重要的是，Ansible 可执行文件位于我的用户主目录下的/local/bin/目录中。顺便说一下，我的用户叫 ubuntu。您还可以看到，我们使用的是最新版本的 Python 3。

```
$ ansible --version
ansible 2.8.5
  config file = None
  configured module search path = 
    ['/home/ubuntu/.ansible/plugins/modules', 
    '/usr/share/ansible/plugins/modules']
  ansible python module location = 
    /home/ubuntu/.local/lib/python3.6/site-packages/ansible
  executable location = /home/ubuntu/.local/bin/ansible
  python version = 3.6.8 (default, Aug 20 2019, 17:12:48) [GCC 8.3.0] 
```

再走一步。正如我前面提到的，Ansible 将使用 boto SDK 连接到 AWS。所以我们需要安装 boto 和 boto 3 包。这次我也会和皮普一起去。

```
$ pip3 install boto boto3 
```

一旦那一个被带上船，我们将准备好做一些真正的事情。这将在下一节开始。

## 用简单的剧本测试 Ansible

这将是一个非常简单的概念验证演示。我将创建几个文件，向您介绍语法，然后启动它。首先，我将使用任何纯文本编辑器创建一个 *hosts* 文件。通常，hosts 文件告诉 Ansible 在哪里可以找到您想要提供的远程服务器。但是因为，在 AWS 的情况下，将成为我们的主机的资源还不存在，我们将简单地将 Ansible 指向 localhost，boto 将在幕后处理连接。该文件的内容如下所示:

```
[local]
localhost 
```

接下来，我将创建一个剧本文件，我将其命名为 test-ansible.yml。当然，yml 扩展名表示该文件必须使用 YAML 标记语言语法进行格式化。正如您从我刚刚粘贴到下面的文件文本中看到的，它将以三个破折号开始，标记文件的开始，然后是一个缩进破折号，介绍一组定义。“主机”的值可以是一台或多台远程计算机，但是，正如我所说的，我们将让本地系统来解决这个问题。我们的联系也是如此。

下一部分包括我们希望 Ansible 执行的任务。这一个将使用 aws_s3 模块*在美国东部地区的亚马逊 s3 简单存储服务上创建一个新的存储桶。我必须给它取这个难看的名字，因为 S3 存储桶需要全局唯一的名字——如果您选择的名字与已经存在的无数个名字中的任何一个冲突，操作就会失败。*

```
---
  - name: Test s3
    hosts: local
    connection: local

    tasks:
      - name: Create new bucket
        aws_s3:
          bucket: testme817275b
          mode: create
          region: us-east-1 
```

我通过调用 ansible-playbook 命令来运行剧本，使用-i 来指定 hosts 文件，然后指向 test.yml 文件。Ansible 应该会在一两分钟内给我们一些反馈。如果我们成功了，您将看到“0”是“失败”的值，至少“1”是“好”的值。

```
$ ansible-playbook -i hosts test-ansible.yml
PLAY [Test s3] ******************************************************

TASK [Create new bucket] ********************************************

changed: [localhost]

PLAY RECAP **********************************************************
localhost: ok=1    changed=1    unreachable=0    failed=0   skipped=0
    rescued=0    ignored=0 
```

如果我再次检查我的桶列表，我应该看到新的:

```
$ aws s3 ls
2018-12-30 15:19:24 elasticbeanstalk-us-east-1-297972716276
2018-10-12 04:09:37 mysite548.com
2019-09-24 15:53:26 testme817275b 
```

这是对建立一个可行环境的非常简单的介绍。我们看到了与传统 Ansible 主机相比，Ansible 与 Amazon 的自动供应资源的使用方式将会有所不同。您将需要一套不同的身份验证和库存控制工具。我们完成了设置一个 Ansible 环境并将其连接到 AWS 的过程，然后运行一个简单的剧本。又短又甜。

这篇文章来自我的书“[使用 Ansible](https://www.amazon.com/gp/product/B07YK42ZH1/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B07YK42ZH1&linkCode=as2&tag=projemun-20&linkId=d90b5a553223444f00992afa4c8f8d16) 管理 AWS 资源”。在我的 bootstrap-it.com 的[网站上，有更多的技术好东西——以书籍、课程和文章的形式。](https://bootstrap-it.com)