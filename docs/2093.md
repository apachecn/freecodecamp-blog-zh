# 什么是 Terraform？将地形和基础设施作为代码来学习

> 原文：<https://www.freecodecamp.org/news/what-is-terraform-learn-infrastructure-as-code/>

Terraform 是一个以代码形式帮助你管理各种云基础设施服务的工具。你将你的基础设施编码，所以它也被称为代码基础设施(IaC)。

云对越来越多的公司变得重要。这不仅有助于减少时间和成本，还能让客户专注于他们的核心业务。

## 为什么基础设施是代码？

随着云提供商数量的增加以及他们的服务变得更加灵活，管理您的云基础架构资源变得越来越重要。

Terraform 致力于基础设施即代码(IaC)的概念。简单地说，IaC 就是以代码的形式表示基础设施的能力。

让我们以给定云上的任何计算资源为例，比如 AWS 上的 EC2。从 AWS 请求 EC2 实例就是向 AWS 注册，提供一堆值，然后点击“启动”按钮。“资源”将在几分钟内准备好。

只要我们能向 AWS 提供这些价值，他们就会依赖云提供商。当然，这是传统的做法。

Terraform 提供了一种方法，以*配置*的形式获取这些凭证和输入，并处理它们以在目标云中创建资源。

这些配置以 Terraform 理解的语言描述资源。配置是您可以声明您的基础设施的期望状态的方式——基本上是“声明性”语法。

Terraform 使用云提供商 API 来创建资源。

## Terraform 的优势

Terraform 是来自 [Hashicorp](https://www.hashicorp.com/) 的产品，使用 [Hashicorp 配置语言](https://github.com/hashicorp/hcl) (HCL)语法来表示配置。

在下面的示例中，您可以看到 EC2 实例最简单的表示形式:

```
provider “aws” {
	region = “us-west-1”
}

resource “aws_instance” “myec2” {
	ami = “ami-12345qwert”
	instance_type = “t2.micro”
}
```

这个简单的例子足以让我们理解 Terraform 的能力。

代码包含两个模块——T0 和 T1。`provider`块让 Terraform 知道我们想在“us-west-1”地区使用`aws`提供者。

`resource`块让 Terraform 知道，在 AWS 提供的所有基础设施资源中，我们想要创建一个“instance”(EC2)类型的资源。

第一个参数在资源块中将此表示为“aws_instance”。第二个参数是我们命名的资源——在本例中是“myec2”。

资源块有两个参数，说明 AWS 机器映像和用于创建该资源的实例的类型。

在这里，我们已经设法以代码的形式表达了我们的基础结构。让我们来看看 IaC 的一些优点。

1.  由于基础设施的创建现在被压缩在配置/代码文件中，维护变得更加容易，因为我们现在可以利用 Git 这样的版本控制系统来协作和维护它。
2.  基础设施规划阶段所需的时间减少了，因为我们可以在**短时间**内编写配置。这些配置很容易被 Terraform 使用，从而在几分钟内创建云资源。
3.  对基础设施**的**变更**更容易**并且与应用程序代码变更相当。
4.  软件开发中应用程序管理生命周期的优势同样适用于基础设施。这使得**更加高效**。

## 地形特征

### 管弦乐编曲

在部署各种端到端服务时，Terraform 在创建云资源时充当编排流程的核心。

### 云不可知

由于 Terraform 支持包括 AWS、MS Azure 和 GCP 在内的大多数云，所以你不必太担心供应商锁定问题。Terraform registry 为所有受支持的云提供商提供文档。

用于在各种云上编码基础设施的语法模式是相同的，因此与特定于提供商的 API 相关的学习曲线是暂时搁置的，但不要忘记。

### 声明语法

Terraform 文件中表达的基础设施是声明性的，因此作为开发人员，我们不需要担心让 Terraform 理解创建资源所需的步骤。相反，我们需要做的只是让 Terraform 知道所需的状态，Terraform 会在内部处理这些步骤。

### 模块

Terraform 提供了帮助我们重用 Terraform 代码的模块。一个复杂的基础设施被分解成多个模块，每个模块都可以在不同的项目中重用。

将一个给定的 Terraform 配置转换成模块是非常容易的，Terraform 有其预建模块的生态系统。

### 状态管理

当 Terraform 创建和规划基础设施时，状态被维护。出于协作目的，这可以与其他团队成员共享。

Terraform 允许您远程管理状态，这有助于防止团队成员在试图重建基础架构时发生混乱。

### 准备金提取

Terraform 不是一个成熟的配置工具，但它有助于第一天的配置活动。Terraform 的*本地执行*和*远程执行*模块让你可以运行内嵌脚本。内联脚本有助于在成功创建资源后安装软件组件。

这在帮助 Chef、Ansible 和 Salt Stack 等配置管理工具安装各自的代理时特别有用。一旦安装成功，它们就可以发送一个“启动”信号。

### 开放源码

Terraform 可用作开源软件。它还有一个企业版。

## Terraform 工作流程[初始-计划-应用-销毁]

执行 Terraform 代码需要一些简单的步骤。这些步骤与云平台上的资源生命周期密切相关。

同样，这些步骤是与云无关的，这意味着相同的步骤/命令对任何给定云提供商上的**创建、更新和销毁**资源都有效。

**注意:**这篇博客文章不包括 Terraform 的安装步骤，我假设您的系统上已经安装了 Terraform CLI。

### 运行`init`命令

一旦我们准备好配置文件，我们需要运行的第一个命令是`terraform init`。Terraform 二进制安装不包括对所有云提供商的一次性支持。

相反，基于提供者，在 Terraform 执行代码之前，适当的**插件被下载**。在我们的例子中，运行`terraform init`将下载`aws`提供者插件。这个命令帮助*初始化*给定地形目录的后端。

### 生成执行计划

命令`terraform plan`帮助**生成执行计划**。根据您提供的配置，Terraform 会生成一个执行计划。在这个阶段，Terraform 在语法错误、API 认证、状态验证等方面执行**可行性检查**。

`plan`在实际执行之前，突出显示 Terraform 脚本中的任何修复。如果成功，它输出基础设施中潜在变化的**摘要。你应该在*应用*之前运行这个，因为它让你在修改基础设施之前意识到任何风险。**

### `Apply`变化

`terraform apply`帮助**执行对基础设施**的任何变更。您应该在运行`terraform apply`之前运行`plan`命令，因为计划会创建一个在应用阶段被引用的计划文件。

但是，如果直接执行`terraform apply`，将自动创建一个新的计划文件。

### `Destroy`资源

最后，`terraform destroy`帮助销毁属于当前配置/状态的任何资源。

## Terraform 在行动

好了，这篇文章的理论已经足够了。让我们通过在 AWS 上实际创建 EC2 的一个实例来实现我们到目前为止所学的内容。

首先，如果您还没有安装 Terraform CLI，请安装它。安装非常简单，你可以在这里找到你选择的操作系统的步骤。

在系统上创建一个目录/文件夹，并创建第一个 Terraform 文件。命名为`main.tf`。理想情况下，我们可以给它起任何名字，只要它有扩展名`.tf`。

Terraform CLI 可识别保存在特定目录中的所有扩展名为`.tf`的文件，以便执行。

将上述代码粘贴到该文件中并保存。**请注意**您需要根据您的首选地区使用正确的 AMI。

由于这将是我们第一次执行 Terraform 代码，我们需要*在这个目录中初始化* Terraform。运行`terraform init`会安装需要的`aws`插件。

启动一个终端，导航到我们的 Terraform 文件所在的目录，然后运行下面的命令。

`terraform init`

这将生成如下输出。输出非常清晰，我们可以看到`aws`插件是用 3.22.0 版安装的。

```
Initializing the backend...
Initializing provider plugins...
- Reusing previous version of hashicorp/aws from the dependency lock file
- Installing hashicorp/aws v3.22.0...
- Installed hashicorp/aws v3.22.0 (signed by HashiCorp)
Terraform has been successfully initialized!
You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.
If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary. 
```

接下来，我们将运行`terraform plan`命令来查看一个试验性的执行计划。这也验证了任何语法或引用错误。命令检查在 T2 文件中声明的资源的可行性。在同一个终端中运行这个程序。

`terraform plan`

如果一切正常，将生成以下输出。

```
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_instance.example will be created
  + resource "aws_instance" "myec2" {
      + ami                          = "ami-12345qwert"
      + arn                          = (known after apply)
      + associate_public_ip_address  = (known after apply)
. . .
Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run. 
```

`plan`命令指示将创建哪些资源。在我们的例子中，它计划用给定的配置创建一个`myec2`实例。它显示了用于创建实例的 AMI ID。

此外，它还表明其他属性如`arn`和`associate_public_ip_address`是在`apply`之后知道的，也就是实例将被创建的时间。

这里的底线表示最后一组更改——添加 1 个资源，没有任何更改或破坏。

所以，现在一切看起来都很好。让我们继续应用配置。在终端中运行下面的命令并观察输出。

`terraform apply`

一旦确认，在 AWS 上完成 EC2 实例的创建需要几秒钟，这由`terraform apply`生成的输出来指示。

如下面的输出所示，在我的例子中，创建一个 EC2 实例花了 51 秒，并且实例的 ID 也是可用的。

通过登录 AWS 控制台并搜索具有以下 ID 的 EC2 实例来验证这一点。如果一切顺利，你应该能找到它。

```
Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_instance.myec2: Creating...
aws_instance.myec2: Still creating... [10s elapsed]
aws_instance.myec2: Still creating... [20s elapsed]
aws_instance.myec2: Still creating... [30s elapsed]
aws_instance.myec2: Still creating... [40s elapsed]
aws_instance.myec2: Still creating... [50s elapsed]
aws_instance.myec2: Creation complete after 51s [id=i-04ef3120a0006a153]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed. 
```

因此，我们已经成功地使用 IaC 在 AWS 上定义/声明和创建了我们的虚拟机配置。

如果我们不再需要这个虚拟机，我们可以使用相同的配置来销毁它。

请注意，如果我们对配置进行了任何更改，而没有打算应用这些更改，然后我们试图销毁这些更改，我们可能会遇到错误。

这是因为 Terraform 在状态文件中维护了配置和真实资源之间的关系。更改应用程序的配置会影响对齐，Terraform 会将其视为要创建的新资源。

Terraform states 是一个值得单独发布的主题，所以我们将在后面讨论。现在，要销毁 EC2 实例，请在终端中运行下面的命令。

`terraform destroy`

在破坏资源之前，Terraform 会通过提供计划输出来请求我们的确认。它表明运行 destroy 命令将删除 1 个资源，这正是我们所期望的。

```
Terraform destroy
Plan: 0 to add, 0 to change, 1 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

aws_instance.myec2: Destroying... [id=i-04ef3120a0006a153]
aws_instance.myec2: Still destroying... [id=i-04ef3120a0006a153, 10s elapsed]
aws_instance.myec2: Still destroying... [id=i-04ef3120a0006a153, 20s elapsed]
aws_instance.myec2: Still destroying... [id=i-04ef3120a0006a153, 30s elapsed]
aws_instance.myec2: Still destroying... [id=i-04ef3120a0006a153, 40s elapsed]
aws_instance.myec2: Still destroying... [id=i-04ef3120a0006a153, 50s elapsed]
aws_instance.myec2: Still destroying... [id=i-04ef3120a0006a153, 1m0s elapsed]
aws_instance.myec2: Destruction complete after 1m5s

Destroy complete! Resources: 1 destroyed. 
```

同样，破坏资源需要几秒钟的时间。Terraform 不会让你一直挂着，因为它以 10 秒的间隔更新状态。

一旦资源被破坏，它就确认完成了。请随意登录 AWS 控制台，验证资源是否已终止。

### 感谢阅读！

我希望你从这个基本介绍中理解 Terraform 是如何工作的。在以后的文章中，我会写更多的文章，涵盖更深层次的概念，比如状态、语法、CLI、后端等等。

如果你喜欢这些内容，请考虑订阅、关注和分享这篇博文！ [Let'sDoTech](https://letsdotech.dev/) ， [Instagram](https://www.instagram.com/letsdotech/) ， [Twitter](https://twitter.com/letsdotech_dev) ， [LinkedIn](https://www.linkedin.com/company/letsdotech) 。