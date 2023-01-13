# AWS CLI 教程–如何安装、配置和使用 AWS CLI 来了解您的资源环境

> 原文：<https://www.freecodecamp.org/news/aws-cli-tutorial-install-configure-understand-resource-environment/>

*如何仅使用 AWS CLI 准确获取管理 AWS 帐户所需的帐户和环境信息*

安装 AWS CLI 实际上非常简单。最好的方法是阅读 [AWS 安装指南](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html),并按照操作系统的说明进行操作。

现在，他们正在向 CLI 的第 2 版推进，我看不出有什么理由不同意。我正在使用 Linux，所以这是我接下来要去的地方。

为了完成这项工作，我将把来自 Amazon 页面的 curl 命令粘贴到我的 Linux shell 中，它将下载这个包并把它写到一个本地 zip 文件中，然后我将解压这个文件。这将创建一个名为 aws 的新目录，其中包含一个安装脚本，我可以使用 sudo 运行该脚本以获得管理员权限。我将运行 aws - version 来确认一切正常。

```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
ls aws
sudo ./aws/install
aws --version 
```

下一步将需要快速访问一次管理控制台。你看，要向你的帐户验证 CLI，你需要一个有效的访问密钥。现在，CLI 有一个“create-access-key”命令，可以生成一个新的密钥，但这只有在我通过身份验证后才有可能。我相信你明白这个问题。

您可以从控制台上任何页面顶部的下拉帐户菜单中访问安全凭据页面。有了凭证，您就可以运行“aws configure”系统会提示您输入访问密钥 ID 和密钥本身。如果你愿意，你可以选择一个默认的 AWS 区域和输出格式。格式不会是一个问题，所以我会把它作为默认设置。

```
aws configure 
```

就是这样。为了确认一切正常，我将列出我账户中所有的 S3 股票。这样，我们就可以开始下一个片段的工作了。

```
aws s3 ls 
```

你可能已经知道 Amazon 的 CloudFormation 服务可以让你管理你的应用程序基础设施，把它组织到你的 AWS 账户资源栈中。

定义这些堆栈的 CloudFormation 模板可以在任何地方共享、编辑和启动，随时随地为您提供可预测和可靠的云应用环境。

您可能还知道，您可以通过 AWS 管理控制台管理您的 CloudFormation 堆栈，正如我在我的[新 Pluralsight 课程中所讨论的，使用 AWS CLI，使用命令行界面](https://pluralsight.pxf.io/EMPE2)创建和管理 AWS CloudFormation 堆栈。

如果您选择使用 AWS CLI——这是我强烈推荐的——您将需要一种方法来收集关于其他帐户资源的关键信息。但是，最初，如何通过 CLI 获得这些信息可能看起来不那么明显。

为了向您展示我的意思，让我们使用来自 AWS 文档样本的模板来试验一个更复杂的堆栈。

[应用框架模板集](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/sample-templates-appframeworks-us-east-1.html)包括一个用于自动扩展的 Linux 服务器的模板，该模板将预配有 Apache web 服务器和 PHP 脚本语言，以及一个到运行 MySQL 数据库引擎的 Multi-AZ RDS 数据库实例的连接。

您可以从 AWS 文档页面中单击 View，查看模板本身。在这里，您将看到参数部分定义了您的实例将启动的 VPC 和子网，以及 MySQL 数据库名称、用户和密码。

所有正确的服务都知道这些细节是非常重要的，因为，否则，它们将无法相互通信。我们必须找到一种方法来增加这些值。要开始工作，您可以简单地点击查看模板([，您可以在这里看到](https://s3.amazonaws.com/cloudformation-templates-us-east-1/LAMP_Multi_AZ.template)，并复制内容，将其粘贴到本地机器上的一个新的 JSON 文件中。

您可以使用 CLI 通过 create-stack 命令启动云形成堆栈。但是，该命令需要一些参数来传递重要信息。这个最小的例子向您展示了如何将 CloudFormation 指向您的 JSON 模板文件、分配给您的栈的名称以及有效的 SSH 密钥，以便我能够登录到它创建的实例。

```
aws cloudformation create-stack \
  --template-body file://lamp-as.json \
  --stack-name lamp \
  --parameters \
  ParameterKey=KeyName,ParameterValue=mykey 
```

问题是，如果您要对 JSON 文档中的模板运行该命令，它将会失败。这是因为，通过查看模板，您无疑会记得，有一些额外的参数需要满足。具体来说，我们需要引用一个 VPC 和两个子网，因为这是一个多可用性区域部署，所以它们需要位于不同的区域。

这将如何工作？这是 AWS CLI 的救援。需要 VPC 身份证吗？请记住，VPC 是 EC2 对象，您可以运行 AWS EC2 describe——VPC 和您需要的所有数据——包括 VPC ID——将神奇地出现。和子网？很明显，还是老样子。只需复制将出现的任意两个子网的子网 id，您就可以开始工作了。

```
aws ec2 describe-vpcs
aws ec2 describe-subnets 
```

现在让我们将所有这些信息放到新版本的 create-stack 命令中。你需要小心这一点，因为语法中有一些令人讨厌的陷阱。

```
aws cloudformation create-stack \
  --template-body file://lamp-as.json \
  --stack-name lamp-as \
  --parameters \
  ParameterKey=KeyName,ParameterValue=mykey \
  ParameterKey=VpcId,ParameterValue=vpc-1ffbc964 \
  ParameterKey=Subnets,ParameterValue=\'subnet-0e170b31,subnet-52d6117c\' \
  ParameterKey=DBUser,ParameterValue=myadmin \
  ParameterKey=DBPassword,ParameterValue=mypass23 
```

第一个新参数是 VPC ID。但是要确保大小写正确:在 Id 中使用大写的 D 会导致整个事情失败。我不知道为什么他们让事情变得如此难以忍受，但这就是我们所拥有的。

下一张更精致。因为我们需要两个子网，所以我们需要在一行中输入它们，用逗号分隔——但不能有空格。然而，我们还需要用单引号将字符串括起来。但是 CLI 不能像那样读取撇号，所以我们需要使用反斜线对它们进行转义。明白了吗？

我还将添加这两个数据库参数:DBUser 和我的超级秘密、超级神秘的 DBPassword。行得通吗？你打赌。但是不要告诉任何人，在我做对之前，我在没有你看的情况下尝试了多少次。记住:失败是你的朋友。

当我们的栈准备好并启动时(这可能需要半个小时)，运行 describe-stacks 会给我们提供我们的网站 URL。

```
aws cloudformation describe-stacks 
```

但这还不是故事的全部。我将使用另一个 aws ec2 命令——这次是 describe——instances——来获取作为这个堆栈的一部分启动的 ec2 实例的一些信息。这一个将过滤结果，将输出限制到那些当前正在运行的实例。

```
aws ec2 describe-instances \
  --filters Name=instance-state-name,Values=running \
  --query 'Reservations[*].Instances[*].{Instance:InstanceId,PublicIPAddress:PublicIpAddress}' 
```

我碰巧在这个区域没有运行其他实例，所以只有 CloudFormation 实例会显示出来。现在我使用- query 进一步过滤输出，只给出实例 id 和这些实例的公共 IP 地址。正如你所料，正好有两个在运行。

只是一个尝试——大部分都与云的形成有关——但是我想您已经了解了如何使用 AWS CLI 收集信息。

在我的[bootstrap-it.com](https://bootstrap-it.com)有更多关于管理的书籍、课程和文章。