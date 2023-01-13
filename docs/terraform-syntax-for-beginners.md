# 在 20 分钟内学会基本的地形语法

> 原文：<https://www.freecodecamp.org/news/terraform-syntax-for-beginners/>

在本文中，我将简要介绍 Terraform 的配置语法。

Terraform 的文档提供了对其语法最全面的了解。但是这篇文章应该作为一个简明的快速入门介绍，给新用户一个简单的概述。我们将把重点放在基本的结构上，而不要深入讨论。

为了帮助您学习语法，我们将通过一个示例，我将教您 Terraform 配置语言(称为 HCL-hashi corp 配置语言)最重要的部分。然后，我们将构建一些基础设施作为代码(IaC ),以查看它的运行情况。

在我们开始之前，您应该已经在本地系统上安装了 Terraform。您还应该能够访问 AWS 管理控制台，并且已经为 Terraform 设置和配置了一个 IAM 用户。

## Terraform 中的参数和块

下面的第一个示例创建了一个 EC2 实例:

```
provider “aws” {
    region = “us-west-1”
}

resource “aws_instance” “myec2” {
    ami = “ami-12345qwert”
    instance_type = “t2.micro”
} 
```

代码由两个用花括号({})括起来的块组成，每个块都定义了某些**参数**。

就像在大多数编程语言中一样，使用参数给变量赋值。在 Terraform 中，这些变量是与特定类型的块相关联的属性。

提供者“aws”块有一个参数—`region = “us-west-1”`，其中 region 是与该块相关联的属性，它被赋值为“us-west-1”。该值是字符串类型，因此用一对双引号( "" )括起来。

类似地，资源块有两个参数，用于设置相关属性的值。

Terraform 使用各种类型的**块**。基于类型，块表示并包含一组属性和功能。在给定的示例中，我们有一个 provider 类型的块和另一个 resource 类型的块，每个块都有一个标识符和一组输入标签。

提供者块接受一个输入标签——提供者的名称。在本例中为“aws”。它还告诉 Terraform 在初始化阶段安装 AWS provider 插件。

资源块有两个输入标签——资源的类型和资源的名称。在本例中，类型为“aws_instance ”,名称为“myec2”。接下来是用花括号括起来的块体。

## 如何在 AWS 上创建 EC2 实例

那么，我们如何开始将我们的基础设施表达为代码并加以利用呢？让我们以在 AWS 上创建一个简单的 EC2 实例为例。

首先创建一个您选择的目录，您将在其中放置创建 EC2 实例所需的所有配置代码。

默认情况下，Terraform 假设目录中所有扩展名为`.tf*`的文件都是配置的一部分，不管文件名是什么。在这个目录中创建一个名为`main.tf`的文件。

我们需要决定的第一件事是我们将使用哪些提供者。因为我们将在 AWS 上运行 EC2 实例，所以我们需要声明所需的提供者，如您在下面的代码片段中所见:

```
terraform {
 required_providers {
   aws = {
     source  = "hashicorp/aws"
     version = "~> 3.0"
   }
 }
}

provider "aws" {
 region = “us-west-1”
} 
```

我们已经声明了两个块——`terraform`和`provider`。`terraform`是最顶层的块，但也是可选的。指定这一点是一个很好的做法，尤其是在使用远程状态管理时。

`terraform`块有一个指定`required_providers`的嵌套块。我们需要`aws`提供商。required_providers 中的`aws`是一个映射，它指定了提供者的`source`和`version`。

接下来，我们有一个用于`aws`的 provider 块，它指定了所需的`region`。

一般来说，这是每个 Terraform 代码开始的方式。当然，可能会有一些变化，确定这一点的最好方法是参考 [Terraform 注册表](https://registry.terraform.io/)以了解 Terraform 的具体版本以及提供者插件本身。

为了当前的例子，我们参考了 [AWS 插件文档](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)。

Terraform registry 通过示例记录了各种云提供商的所有资源的使用情况。当您使用 Terraform 时，这是一个很好的参考。

### 平台提供商

在系统上安装 Terraform 是不够的。Terraform 基于插件架构工作，其中 Terraform 二进制文件构成核心，每个云提供商都是插件。

为了使配置工作，这些插件是在初始化阶段安装的。

提供者插件有自己的一套配置、资源类型和数据源。Terraform 注册表记录了给定提供商的所有详细信息。

### 地形资源

每个提供者都有一组资源。`resource`顾名思义，在配置语言中表示实际要创建的云资源。

提供商启用资源。在给定的示例中，`aws`是提供者，`aws_instance`是由 AWS 提供者提供的资源。

资源有它的属性。这些属性记录在 Terraform 注册表中。在所有属性中，有些是必需的。资源是由 Terraform 执行的确切构造。

继续这个例子，让我们通过将下面的代码附加到我们的 [`main.tf`](http://main.tf/) 文件中来定义一个 AWS EC2 实例资源。

```
resource "aws_instance" "demo" {
 ami = “ami-00831fc7c1e3ddc60”
 instance_type = “t2.micro”

 tags = {
   name = "Demo System"
 }
} 
```

我们从一个名为`“aws_instance”`的资源块开始，传递一个标签并将其命名为`“demo”`。标签是您选择的名称。

接下来，用花括号打开这个块，并指定资源`aws_instance`所需的属性。第一个属性是`ami`，它为 EC2 实例指定 Amazon 机器映像 ID。

第二个属性是`instance_type`，它指定了要创建的机器的大小。

我们也传递标签，这是一个可选的参数。作为标签，我们将`“name”`作为键传递，将`“Demo System”`作为值传递。就这样，我们已经定义了我们的资源。

我们现在在技术上已经准备好了配置。我们可以继续将 Terraform 初始化到这个目录中，以便它安装 AWS 的提供者插件。然后我们可以`plan`和`apply`这个配置。

保存文件，然后继续运行`terraform init`，看看它是否安装了 AWS provider 插件。一旦成功完成，运行`terraform plan`并观察输出。

让我们客观地看待一切。`providers`让 Terraform 知道需要安装哪些插件来执行配置。`resources`表示要创建的实际云资源。

通常，每个资源都有一个名称(`“aws_instance”`)。资源名称的第一部分是提供者标识符(`“aws”`)，由下划线分隔。

### 地形变量

Terraform 是一种声明性语言。在示例中，我们已经声明了虚拟机在所需云上的最终状态。

现在由 Terraform 来接受并执行这个配置，以创建虚拟资源。话虽如此，Terraform 让我们能够为其配置指定**输入变量**。

输入变量类似于给定函数的参数，就像任何编程语言一样。

当您必须在代码中的多个位置指定相同的值时，它们特别有用。随着项目规模的增长，使用变量更改可能在多个地方使用的某些值变得更加容易。

Terraform 支持基本类型的变量，如字符串、数字、布尔和几种复杂类型，如列表、集合、映射、对象和元组。

让我们在代码中定义一些变量，如下所示:

```
variable "region" {
 default = "us-west-1"
 description = "AWS Region"
}

variable "ami" {
 default = "ami-00831fc7c1e3ddc60"
 description = "Amazon Machine Image ID for Ubuntu Server 20.04"
}

variable "type" {
 default = "t2.micro"
 description = "Size of VM"
} 
```

如你所见，我们为`region`、`ami`和`type`引入了三个新变量。到目前为止，我们将在配置中使用它。使用`var.<variable name>`可以参考变量值。

Terraform 配置也给了我们返回值的能力。这些值被称为**输出值**。

当 Terraform 完成配置的执行时，它会使输出值可用。然后，它们可以用作其他接口的输入。

我们在代码中定义了一个输出变量`“instance_id”`。使用`“aws_instance.demo”`的属性参考设置该输出变量的值。类似地，我们可以引用配置中任何资源可用的其他输出变量。

下面是我们`main.tf`的更新代码。我们在这里使用了三个变量:

```
terraform {
 required_providers {
   aws = {
     source  = "hashicorp/aws"
     version = "~> 3.0"
   }
 }
}

provider "aws" {
 region = var.region
}

variable "region" {
 default = "us-west-1"
 description = "AWS Region"
}

variable "ami" {
 default = "ami-00831fc7c1e3ddc60"
 description = "Amazon Machine Image ID for Ubuntu Server 20.04"
}

variable "type" {
 default = "t2.micro"
 description = "Size of VM"
}

resource "aws_instance" "demo" {
 ami = var.ami
 instance_type = var.type

 tags = {
   name = "Demo System"
 }
}

output "instance_id" {
 instance = aws_instance.demo.id
} 
```

保存文件并运行`terraform plan`。注意，Terraform 这次注意到了输出变量。它表示输出已知`after apply`:

```
Plan: 1 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + instance_id = (known after apply) 
```

继续执行`terraform apply`，并检查输出。每次成功完成`apply`后，别忘了跑`terraform destroy`。

最后，Terraform 还支持**局部变量**，这些变量是函数和块在本地使用的临时值。

### 地形补给者

配置意味着在硬件或虚拟机准备就绪后，安装、更新和维护所需的软件。

一旦虚拟机准备就绪，Terraform 就可以触发软件供应流程，但这并不意味着它是一个全职供应工具。

您可以使用此工具，通过安装初始和基本的软件组件，使基础架构为管理做好准备。

有像 Salt Stack、Ansible、Chef 等工具，而且大部分都是基于代理的。Terraform 可以运行初始脚本来安装一些补丁更新、代理软件，甚至设置一些用户访问策略，以确保机器准备就绪。

Terraform 与通用供应器捆绑在一起，也支持特定于供应商的供应器。

## 如何在 Terraform 中管理代码

在我们继续之前，让我们首先将代码组织到多个文件中。一般来说，Terraform 代码库根据提供者、资源和变量分成多个文件。让我们创建如下三个文件:

1.  `variables.tf`–该文件包含所有声明的输入变量。在我们的例子中，我们有为`region`、`ami`和`type`定义的输入变量和一个输出变量`instance_id`。
2.  `provider.tf`–该文件包含您正在使用的提供程序的声明。在我们的例子中，我们有`terraform`和提供者`aws`块。
3.  `main.tf`–该文件包含要创建的实际资源的声明。

默认情况下，Terraform 假设放在特定目录中的所有代码都是同一配置的一部分。

所以从技术上来说，如果你把代码放在一个单独的文件中，或者把它分成多个文件和子目录，并没有太大的区别。从可维护性的角度来看，这样做很有意义。

### 地形中的元论元

在我们深入讨论这个问题之前，请确保当您在处理这些示例时，总是在每次运行`terraform apply`之后运行`terraform destroy`。

元参数是为资源提供的特殊构造。我们已经看到，资源块是 Terraform 创建的实际云资源。

通常，以满足特定需求的方式声明资源是很棘手的。

元参数在一些情况下很方便，比如在同一个云提供商但在不同的地区创建资源。当我们使用不同的名称创建多个相同的资源时，或者当我们必须在 Terraform 不能识别依赖本身的地方声明隐式依赖时，它们也很有用。

下面的部分描述了一些实际的元参数。

#### 提供者元参数

当在给定的 Terraform 配置中有多个提供者配置时，您将使用`provider`元参数。Terraform 自动将给定的资源映射到由资源标识符标识的默认提供者。

例如，`aws_instance`的默认提供者是`aws`。这个`aws`提供者当前被配置为在特定区域部署资源。

但是，如果我们想要为另一个区域提供另一个`aws`提供程序，或者使用不同的配置设置，我们可以编写另一个提供程序块。

尽管可以编写多个提供者配置，Terraform 默认为`aws`选择相同的提供者来创建资源。

这就是**别名**出现的原因。每个提供者配置都可以用一个别名来标记，这个别名的值用在资源块中的**提供者**元参数中，为相同的资源指定不同的提供者配置。

在我们的例子中，让我们复制`aws`提供者，并给它们适当的别名。在`provider.tf`文件中，带有别名的修改后的提供者应该如下所示:

```
provider "aws" {
  alias  = "aws_west"
  region = var.region_west
}

provider "aws" {
  alias  = "aws_east"
  region = var.region_east
} 
```

请注意，我们还修改了区域的变量，以表示两个不同的区域，西部和东部。对`variables.tf`文件进行如下相应的更改:

```
variable "region_west" {
  default     = "us-west-1"
  description = "AWS West Region"
}

variable "region_east" {
  default     = "us-east-1"
  description = "AWS East Region"
} 
```

我们需要做的最后一个更改是在`main.tf`文件中。现在，我们可以使用提供者元参数来指定特定提供者的别名。

我们可以通过在元参数中指定`<provider>.<alias>`来提及我们想要的提供者配置。参考下面修改后的`main.tf`文件:

```
resource "aws_instance" "demo" {
  provider      = aws.aws_west
  ami           = var.ami
  instance_type = var.type

  tags = {
    name = "Demo System"
  }
} 
```

通过运行`terraform validate`来验证最终配置，它应该显示为`Success!`

#### 生命周期元论点

`lifecycle`元参数指定了与 Terraform 管理的资源的`lifecycle`相关的设置。默认情况下，无论何时更改和应用配置，Terraform 都会按以下顺序运行:

1.  创造新的资源。
2.  销毁那些配置中不再存在的资源。
3.  更新那些不破坏就能更新的资源。
4.  销毁并重新创建无法即时更改的已更改资源。

如果您想改变这个默认行为，可以使用一个`lifecycle`元参数。这些元参数用在资源块中，类似于提供者元参数。

有三种生命周期元参数设置:

1.  `create_before_destroy`:当应用更改的配置时，当您希望避免基础架构意外丢失时使用。如果设置为 true，Terraform 将在销毁旧资源之前首先创建新资源。
2.  `prevent_destroy`:当设置为 true 时，任何试图在配置中破坏它的行为都会导致错误。这对于那些繁殖成本很高的资源来说是很有用的。
3.  `ignore_changes`:这是一个列表型元变元，以列表的形式指定某个资源的属性。在更新操作期间，通常会有这样一种情况，您希望防止外部因素导致的更改。在这种情况下，您需要声明一个属性列表，在没有被检查之前，这些属性不能被更改。

当我们在建立复杂的基础设施的过程中，元论据就派上了用场。

通过改变 Terraform 的默认行为，我们可以以`lifecycle`元参数的形式为已确认和最终确定的资源块提供一些保护。

在我们的例子中，我们不会使用任何生命周期元参数。

#### depends_on 元参数

一般来说，Terraform 在创建或修改资源时会意识到依赖关系，并自己处理顺序。

然而，在某些情况下，Terraform 无法检测到隐含的依赖关系，如果它看不到任何依赖关系，就继续并行创建资源。

例如，以 VPC 中包含的两个 EC2 实例的地形配置为例。

有了这个配置后，Terraform 自动知道它应该在启动 EC2 实例之前创建 VPC。

这是常识，Terraform 很清楚。在依赖关系不那么明显的情况下，`depends_on`元参数就可以解决这个问题。

它是一种列表类型的参数，接受配置中声明的资源标识符列表。

#### 计数元参数

想象一种情况，您想要创建多个相似的资源。默认情况下，Terraform 为单个资源块创建一个真实资源。

但是在多个资源的情况下，Terraform 提供了一个名为`count`的元自变量。顾名思义，`count`可以分配一个整数来代表多个资源。

在我们的例子中，让我们创建三个相似的 EC2 实例。在您的`main.tf`文件中，为资源`aws_instance.demo`添加一个属性计数，并为其赋值`3`。它应该看起来像下面这样:

```
resource "aws_instance" "demo" {
  count         = 3
  provider      = aws.aws_west
  ami           = var.ami
  instance_type = var.type

  tags = {
    name = "Demo System"
  }
} 
```

通过这样做，我们让 Terraform 知道我们需要用相同的配置创建三个 EC2 实例。

保存文件并执行`terraform validate`。它抛出一个错误，显示“`Missing resource instance key`”。

记得在我们的`variables.tf`文件中，我们提到了一个输出变量来输出所创建资源的`id`。因为我们要求 Terraform 创建三个实例，所以不太清楚它应该打印哪个 ID。

为了解决这个问题，我们使用了一个叫做`splat`表达式的特殊表达式。

这里的理想情况是对实例运行一个 for 循环，并打印出 ID 属性。splat 表达式是用更少的代码行完成相同任务的更好方法。

你需要做的就是在`variables.tf`文件中，用下面的代码替换输出值:

```
output "instance_id" {
  value = aws_instance.demo[*].id
} 
```

保存这个文件并运行`terraform validate`来查看是否一切正常。

一旦成功，继续运行`terraform plan`和`apply`并检查您在 us-west-1 地区的 AWS 管理控制台，即`aws_west`。也让我知道身份。

Splat 是其中的一种，我们将在接下来的章节中更好地了解表情。

#### for_each 元参数

`for_each`顾名思义，本质上是一个“for each”循环。`for_each`元变元用于创建/循环多个相似的云资源。是的，这听起来确实类似于`count`的元论点，但还是有区别的。

首先，`for_each`和`count`不能一起使用。

其次，你可以说这是`count`的加强版。计数元参数是一种数字类型。Terraform 只是创造了这么多资源。

但是，如果您希望在输出中创建这些带有一些定制的资源，或者如果您已经有了一个 map 或 list 类型的对象，您希望基于它来创建资源，那么可以使用`for_each`元参数。

如前所述，你可以分配`for_each` map 和 list 类型的值。映射是键值对的集合，而列表是值的集合(在本例中是字符串值)。

`for_each`自带特殊物件`each`。这是循环中的迭代器，可以用来引用`key`或`value`，或者在 list 的情况下只引用键。

让我们看一下我们的例子。我们希望为给定的地图创建 EC2 实例。映射被分配给`for_each`元参数，Terraform 为映射中的每个键值对创建一个 EC2 实例。

最后，我们使用`each`对象的`key`和`value`信息来设置`tag`中的 name 属性。

`main.tf`中的资源块现在看起来像这样:

```
resource "aws_instance" "demo" {
  for_each = {
    fruit = "apple"
    vehicle = "car"
    continent = "Europe"
  }
  provider      = aws.aws_west
  ami           = var.ami
  instance_type = var.type

  tags = {
    name = "${each.key}: ${each.value}"
  }
} 
```

执行`terraform validate`并观察输出。它为输出变量抛出一个错误—“`This object does not have an attribute named id`”。

这里有一个小提示:`splat`表达式适用于变量的列表类型。因为我们在设置元参数`for_each`时使用了 map，所以我们需要将返回值表达式改为 for each，如下所示:

```
output "instance_id" {
  //value = aws_instance.demo[*].id
  value = [for b in aws_instance.demo : b.id]
} 
```

再次执行`terraform validate`。如果成功，继续进行`apply`配置。检查 AWS 管理控制台中创建的计算机以及分配给它们的名称。

### 地形表达

表达式是使 Terraform 代码动态化的方法。

表达式有两种形式——简单的和复杂的。到目前为止，在我们的例子中，我们主要处理简单的表达式。

简单表达式是用作某个块的一部分的任何参数。写下一个赋有原始值的参数是一种表达形式。

在处理元参数时，我们在示例中使用了一个名为 splat ( `*`)的复杂表达式。

然而，您可以使用更复杂的表达式来使您的 Terraform 代码更加动态、易读和灵活。你可以在 [Terraform 文档](https://www.terraform.io/docs/configuration/expressions/index.html)中查看各种类型的表达式。

### 地形功能

Terraform 具有内置函数，可用于表达式。这些是在数字和字符串操作中有用的实用函数。

有一些函数可以处理文件系统、日期和时间、网络、类型转换等等。

函数和表达式使得编写一个真正动态的 IaC 变得非常容易。可以参考这里的函数列表[。](https://www.terraform.io/docs/configuration/functions.html)

## 结论

在这篇文章中，我们能够使用 Terraform 试验 AWS EC2 实例。我希望这能帮助你更好地使用 Terraform。

如果你喜欢这些内容，请考虑订阅、关注和分享这篇博文！ [Let'sDoTech](https://letsdotech.dev/) ， [Instagram](https://www.instagram.com/letsdotech/) ， [Twitter](https://twitter.com/letsdotech_dev) ， [LinkedIn](https://www.linkedin.com/company/letsdotech) 。