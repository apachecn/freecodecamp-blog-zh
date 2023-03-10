# 如何使用 AWS 云开发工具包定义云基础设施

> 原文：<https://www.freecodecamp.org/news/aws-cloud-development-kit-crash-course/>

与手动设置云基础设施相比，使用代码会更容易、更安全。AWS 云开发工具包(CDK)是一个开源软件开发框架，使用熟悉的编程语言定义您的云应用程序资源。

我们刚刚在 freeCodeCamp.org YouTube 频道上发布了一个速成班，将教你所有关于自动气象站 CDK 和如何使用它。

马特·马茨创立了这门课程。他是一名工程师，AWS 社区建设者，自称 CDK·范博伊。

在学习了 AWS CDK 的基础知识后，Matt 将带你快速体验 CDK 官方研讨会。然后，您将学习高级主题，如测试和最佳实践。

本课程包括以下几个部分:

*   CDK 速成班简介
*   我们将涵盖的内容
*   资源
*   CDK 基础知识
*   什么是 CDK 构造？
*   3 级构造示例
*   综合、资产、引导和部署
*   CDK 研讨会 Speedrun - Cloud9 准备
*   CDK 车间 Speedrun -新项目
*   CDK 车间 Speedrun -你好，CDK
*   CDK 研习班 Speedrun -编写结构
*   CDK 研讨会 Speedrun -使用构建库
*   CDK 车间速度运行-测试结构
*   高级 CDK
*   更多资源，谢谢！

观看 freeCodeCamp.org YouTube 频道的全部课程(1 小时观看)。

[https://www.youtube.com/embed/T-H4nJQyMig?feature=oembed](https://www.youtube.com/embed/T-H4nJQyMig?feature=oembed)

### 副本

(自动生成)

AWS cloud development kit 是一个开源工具，允许开发人员使用他们喜欢的编程语言为 AWS 编写基础设施代码，Matt marks 是 AWS 社区构建者，他将教您如何使用 AWS cloud development kit。

嘿，我是马特·马茨。

我是 AWS 社区建设者。

我在这里给你一个自由代码营的 CDK 速成班。

您来到这里可能是因为您厌倦了通过控制台手动配置资源。

或者像我一样，你讨厌写 yaml。

但是 CDK 最近也经常出现在新闻中。

12 月，第二版正式发布。

第二版是对第一版的巨大改进，因为只需要安装一个稳定的 NPM 库，而不是每个模块一个。

在其他改进中，CDK 也是 Vogel 博士重塑主题演讲中相当突出的一个特色。

CDK 的书发行了。

CDK 的书是由一群 AWS 英雄写的，它将比这个速成课程更深入，会有重叠，但我仍然强烈推荐这本书。

因此，我们将在本课程中讨论什么，我将从应用程序堆栈、结构的 CDK 定义的基础知识以及它们与云形成的关系开始。

然后我将在 workshop.com CDK 的 CDK 研讨会上做 30 分钟的快速跑，这个研讨会是由 AWS 提供的。

从那里，我们将进入一些高级主题，如测试和最佳实践。

本课程在 YouTube 上分成几个章节。

因此，如果有特别感兴趣的领域，请随意跳来跳去。

我还将在带有补充信息的描述中提供时间戳链接。

这将包括有用的文档、博客、链接或任何其他东西。

我将很快介绍一下 CDK 研讨会，但您应该亲自来做。

了解 CDK 的最好方法是使用它。

因为这是一个 YouTube 视频，如果你需要的话，请随意加快我的速度，放慢我的速度，或者重新观看。

如果你认为我在这个速成班中错过了什么，或者你有什么不明白的，请在评论中告诉我，或者在 Twitter 上关注我，联系我。

我很乐意帮忙。

这里是我想特别指出的一些其他资源。

Slack 上的 CDK 社区非常活跃。

这里到处都是友好、知识渊博、乐于助人的人。

《CDK》一书的作者在那里也非常活跃。

说到这个，你听说了吗，CDK 的书出版了。

除此之外，还有 CBK 日，这是一个与 CDK 有关的年度会议。

去年，我和 CDK 做了一个 15 分钟的简短谈话，内容是关于如何创建可验证的 jots。

我会在下面添加一个链接。

既然这样，我希望你来对地方了。

让我们转到基础部分。

CDK 代表云开发工具包。

它是由 AWS 提供的开源软件，使您能够编写命令式代码来生成声明性云形成模板。

它使您能够定义“如何”,以便您可以了解“是什么”,这意味着您可以更多地关注应用程序的架构，而不是 IAM 角色和策略声明的具体细节。

CDK 有 JavaScript、TypeScript、Python、Java、C sharp 版本，它在 go 的开发者预览版中。CDK 本身不基于 GS。

即使你使用其他语言，CDK 代码本身也不会通过 GS 来执行。

所以没有 J S 是必须的。

好了，CDK 产生了云。

但是云的形成是一种 AWS 服务，它提供 AWS 资源和堆栈。

AWS 资源包括 lambda 函数、s3、buckets、DynamoDB、表格，以及 AWS 提供的几乎所有东西。

CloudFormation 堆栈只是 AWS 资源的集合。

云对我来说，CloudFormation 使用 JSON 或 YAML 文件作为模板，描述部署应用程序所需的所有资源的预期状态。

堆栈实现并管理模板中概述的资源组，它允许一起管理这些资源的状态和依赖关系。

当您更新 CloudFormation 模板时，它会创建一个变更集。

变更集是变更的预览，这些变更将由堆栈操作执行，以创建更新或移除资源，从而使模板变得同步。

所以 CDK 生成云形成和云形成规定，AWS 资源。

CDK 的应用程序看起来像什么，它是如何关联的？CDK 的根源是 CDK 应用程序。

它是一个协调其中堆栈的生命周期的结构。

对于一个应用程序来说，没有 CloudFormation 的等价物。

在应用程序中，您可以有多个 CDK 堆栈，CDK 堆栈甚至可以有嵌套堆栈。

CDK 堆栈与云形成堆栈一对一等价，嵌套堆栈也与云形成堆栈一对一等价。

当你运行 CDK，synth 或 CDK 部署输出是这个应用程序结构的云形成模板。

CDK 应用程序是一个特殊的路由构造，它编排了堆栈和其中的资源的生命周期。

应用程序生命周期以自上而下的方式构建应用程序的树，然后执行树中构造的方法。

您通常不需要直接与构造的任何准备、验证或合成方法进行交互，但是它们可以被覆盖。

应用程序生命周期的最后一个阶段是部署阶段，在这个阶段，云形成工件被上传到云形成。

CDK 的部署单位称为堆栈。

在堆栈范围内定义的所有资源，无论是直接的还是间接的，都作为一个单元进行调配。CDK 堆栈与云架构堆栈具有相同的限制。

烟囱在云的形成中是强大的。

为了将一个模板部署到多个环境中，您需要使用云形成参数。

但是这些问题只能在部署过程中解决。

在 CloudFormation 中，如果您想基于参数有条件地包含一个资源，您必须使用 cloud formation 条件。

但是在 CDK，你不一定要使用这两者，你可以简单地使用一个 if 语句来检查一个值是否应该定义资源，这样会简单得多。

你可以使用 CDK 的云形成参数和条件。

但是他们被劝阻了，因为他们只在部署期间解决，这是在 CDK 合成之后很久。

我们听说过很多关于构造的东西。

但是它们是什么呢？有四个层次的结构。

零级构造只是基本资源。

所有更高的级别都继承自零级，没有特定的类型与之相关联。

一级结构是云信息资源的一对一表示。

在 CDK API 中，它们都以字母 C F n，we 作为前缀，这是 CloudFormation 的缩写形式。

我记得第一级是一对一的云编队。

二级构造不是一级构造的改进或扩展。

它们是由 CDK 团队提供的，提供帮助方法和合理的缺省值。

三级结构是结构的组合，可以是一级、二级和三级结构的混合。

通常情况下，您会与二级或三级结构进行交互。

CDK 的一些模块还没有二级结构。

但是几乎所有的一级构造都存在于相应的 CloudFormation API 中。

CDK 团队在实现对一级结构和更常用的二级结构的变更方面速度非常快。

对于第一级构造，让我们看看访问分析器模块。

在这种情况下，该模块没有任何可用的二级构造，因此只有一级 CFN 分析器构造。

如您所见，这是与 CloudFormation API 的一对一表示。

CFN 分析器构造具有与云形成 API 相同的属性。

在 CFN 分析器结构上也有新的辅助工具方法。

对于第二级构造，让我们看看 DynamoDB 表构造。

它没有 CFN 前缀，包括许多合理的默认值。

例如，除非您指定复制区域，否则计费模式默认为按请求付费。

如果指定复制区域，它将被调配；如果被调配，默认读写容量将被设置并默认为 5。表的默认删除策略将被保留。

该表有助手方法来创建全局或本地二级索引，并创建对表及其流的不同级别的访问。

对于第三级构造，CDK 不提供任何现成的特定内容。

这些往往是在个人组织或社区级别上创建的，并作为库提供。

第三级构造的一个例子是创建一个通知桶。

这个构造创建了一个 s3 bucket 和一个 SNS 主题。

然后，它添加一个来自 s3 存储桶的对象创建通知，以 SNS 主题为目标。

你所要做的就是在你的应用中添加一个新的通知桶。

它会自动提供所有这些资源。

社区中有几个很棒的资源，我也想说出来。

patterns.com CDK 是一个资源，使您能够通过社区提供的例子进行搜索，并通过其中使用的不同组件进行交叉引用。

AWS 还通过其 AWS 解决方案构造提供了 CDK 的另一个开源扩展。

这些提供了多服务的架构良好的模式，用于在代码中快速定义常用模式的解决方案。

这是一个可用的 NPM 库，你可以安装和使用开箱即用。

还有构造中枢。

construct hub 是一个发现和共享云应用程序设计模式和参考架构的中心目的地，这些设计模式和参考架构是为 CDK·CDK 和 CDK 设计的。

对于 TerraForm 和其他基于构造的工具，构造集线器从 NPM 注册中心拉，所有 CDK 构造支持多种语言，并被正确注释。

太好了。

所以我认为我们已经掌握了构造。

现在，让我们再深入一点。

我们如何生成云形成模板。

为了做到这一点，应用程序需要合成。

为此，我们可以运行 CDK。

由于它遍历应用程序树并在所有构造上调用 synthesize，因此最终会为 CloudFormation 资源生成唯一的 id，并生成相应的 EML 以及所需的任何资产。

好吧，那么什么是资产？资产是捆绑到 CDK 应用程序中的文件。

它们包括 lambda 处理程序代码、Docker 图像、层版本、文件、进入 s3、桶等。

它们可以代表应用程序需要的任何工件。

当您运行 CDK synth 或 deploy 时，这些会输出到您本地机器上的 CDK dot out 文件夹。

好吧，那么 CDK 怎么处理这些资产呢？它们是如何进入云层的？这就是自举发挥作用的地方。

引导创建一个 CDK 工具包云形成堆栈部署到您的帐户。

该帐户包括 s3 存储桶和 CDK 进行部署和上传资产所需的各种权限。

当您的应用程序有资产或您的 CloudFormation 模板变得大于 50 时，需要进行引导。

因此，这几乎总是必需的，你的筹码中几乎总是会有某种资产。

对于 CDK 版本，需要管理员访问权限来创建 CDK 工具包堆栈所需的角色，以便进行部署。

CDK 启动后，你不需要管理员权限。

完成引导后，我们可以继续部署了。

当您运行 CDK 部署时，应用程序被初始化或构建到应用程序树中。

一旦构建完成，应用程序将经历准备、验证和综合步骤。

所以每个构造调用它的准备方法，然后每个构造调用它的验证方法。

然后每个构造调用它的合成方法。

到目前为止，这是 CDK 发给我们的。

从那里，应用程序将模板和任何其他工件上传到云形成，云形成部署开始。

一旦移交完成，一切都在云形成的手中。

现在，我们已经定义了 CDK 的一些基本要素，让我们继续研讨会。

话止于 CDK 车间的平静。

你应该先做这件事，然后再回来。

完美。

你一回来，我们就在车间里进行 30 分钟的快速巡视。

我们将从创建一个九霄云外的环境开始本次研讨会。

为了做到这一点，我要从 CDK 管道车间偷一些说明，它将有一个屏幕，我注册了拥有完全访问权限的“九重天”。

这将用于启动 CDK 的帐户。

CDK 管道研讨会的第二版链接将在下面描述。

这是一个很棒的研讨会，如果你有时间的话，你应该参加。

首先，我们要去九重天控制台创建一个环境。

我就把它命名为 CDK 工作室，我们不需要描述。

然后我们选择一个实例类型，我用一个 T ^ 3 小实例做了这个研讨会。

但是 M 5 大号的可能会更好。

所有其他选项都可以使用默认值。

当 CLOUD NINE 加速时，我想指出的是，the T 小型实例和 N ^ 5 大型实例都不符合自由层条件。

所以可能会花你一些钱。

在 the T 小实例的最后，我遇到了一些内存错误。

但它仍然有效。

来创造“我是”的角色。

CDK 管道研讨会有一个深层链接，将带您进入角色创建页面。

从那里，我们确认它是 AWS 服务实体，并且为了方便，然后转到权限。

确保选择了管理员访问权限。

我们不需要新的标签，所以我们可以跳过它。

然后，我们将为其命名，我们将命名为“CDK 管理员”,并创建角色。

创建好角色后，我们可以返回到 CDK 管道研讨会，看到我们需要实际进入并将 roll 附加到 EC 实例。

有很深的联系。

但是既然我们已经命名了 EC 两个实例，我们实际上需要去 EC 控制台的其他地方，去实例。

然后选择正在运行的 cloud nine 实例。

转到操作、安全性和修改 Iam 角色。

在这里，我们将选择我们刚刚创建的 IAM 角色“CDK 管理员”,并保存它。

现在，如果我们去 CDK 研讨会、云 9 和更新，它将自动拥有我们需要的角色。

接下来，我们需要做一些云 9 内务和设置，我们将删除 AWS 管理临时凭据。

这些使我们无法使用我们附加到 cloud nine 的角色。例如，在 Cloud Nine 的右上角，转到“设置”,然后向下滚动到“AWS 设置”,关闭“AWS 管理临时凭据”。

为了确保凭据已被删除，我们将删除凭据文件夹。

接下来，我们将进行一些环境配置，我们需要使用 yum 安装 JQ。

然后我们将导出一些带有 AWS 帐户信息的环境变量。

为了检查以确保 AWS 区域设置正确，我们将执行这个测试命令。

最后，我们将导出客户 ID 和地区，并确保 AWS 配置正确。

现在，我们需要确保使用正确的 Iam 角色，Cloud Nine 易于实例化。

为此，我们将使用 STS get color identity 命令。

这里会失败，因为我们实际上将角色命名为 CDK 管理员。

因此，如果我们更新命令来检查 CDK 管理员，我们将得到这样的消息，即 im 角色是有效的。

在这一点上，我们可以切换回实际的 CDK 介绍研讨会。

我们将转到“前提条件”选项卡，开始检查以确保一切正常。

AWS CLI 已经安装，我们已经设置了 AWS 帐户和用户。

接下来，我们要确保我们在 CLOUD NINE 中安装了一个良好的节点版本，Cloud Nine 附带安装了节点。

所以我们只检查版本。

这里 16 版就可以了。

我们还想检查一下 CDK。

这是一个 CDK 工作室。

所以我们想确保我们确实升级了 CDK 的版本。

因此，如果我们做 NPM 我安装自动气象站 CDK。

从全球来看，我们会得到这个错误，因为他是 easytouse 怪异的九霄云外的实例，所以你实际上必须强制它。

在这种情况下，CDK 管道教程也经历了这一点。

现在我们已经强制安装了它，我们可以看到版本 2.3 已安装。

我们已经在使用 AWS cloud nine，我们刚刚检查了 CDK 版本。

前提条件到此结束。

接下来，我们可以开始实际的研讨会。

让我们开始吧，我们去 CDK。

下一步，我们需要做的第一件事是创建一个工作目录。

因此，我们将制作 CDK 车间目录，并在其中进行更改。

现在我们需要真正承认这个项目。

因此，我们将运行 CDK 和它的语言类型脚本示例应用程序。

这创建了一个 Git repo，NPM 为一个基本的 CDK 二号项目安装了所有的依赖项 CDK 二号是对 CDK 版本一个巨大的改进，因为它将所有稳定的模块烘焙成一个 NPM 依赖项 CDK 版本一对每个模块都有单独的 NPM 库，如 DynamoDB、lambda SNS 等。

在第二版中，你只需要一个，这就避免了很多依赖管理的麻烦。

既然 thug 项目已初始化为 CDK，workshop 将希望我们在后台运行 watch 命令，将类型脚本编译为 JavaScript。

因此，我们将创建一个新的终端，将目录更改为项目文件夹，然后在后台运行 NPM run watch 命令。

完成后，我们可以继续下一步，实际检查到目前为止已经初始化的项目结构。

让我们回到 Cloud Nine 环境，打开项目文件夹，开始查看一些文件。

让我们从 CDK 开始的地方开始，用 bin 斜线 CDK 工作坊打字稿文件。

该文件通过创建堆栈来实例化 CDK 应用程序。

也就是说，当你运行 CDK 中心的光盘工具包时，它就从这里开始生成云系模板。

CDK 堆栈与云形成堆栈是一对一的等效。

一个应用程序可以有多个堆栈，每个堆栈最终会在云形成中创建一个新的堆栈。

在我们的示例应用程序中，只有一个堆栈，即 CDK 工作室堆栈。

接下来让我们来看看。

示例堆栈非常简单。

它使用两个二级结构。

一个用于创建 SQ SQ，一个用于创建 SNS 主题，然后将队列订阅到主题。

就是这样。

研讨会对这些进行了类似的详细讨论。

但是让我们继续我们的第一个 CDK 合成器。

如前所述，当执行 CDK 应用程序时，它们会为应用程序中定义的每个堆栈生成一个 AWS CloudFormation 模板。

切换回九霄云外，让我们运行 CDK 赛斯，我要输出到一个模板点莫亚文件，使它更容易阅读。

现在，我想在这里提出两个重要的概念。

CDK 既是必要的，它描述了应用程序将如何建立。

但它也是一个项目，同样的投入，它会产生同样的产出。

看这里，我们在我的“九重天”实例中的 CDK 车间之间使用相同的代码。

云的形成也是如此。

例如，两者之间的资源哈希匹配。

这是因为我们使用了所有与 CDK 研讨会相同的投入。

如果我改变了主题或提示 ID，散列就会改变，我们就会得到别的东西。

这在以后的单元测试中很重要。

既然已经发了，那就部署吧。

为了做到这一点，我们需要引导帐户，让我们打开一个新的控制台窗口，并前往云的形成。

而这个账户，现在唯一还在运行的，应该是用于九重天的那个。

就在这里。

所以让我们回到九霄云外，运行引导程序。

这就是为什么我们需要创建一个具有管理员访问权限的 im 角色。

实际上，这是你引导账户时唯一需要管理员权限的时候。

为了让 CDK 部署应用程序，它需要将资产存储在某个地方。

Bootstrap 创建了自己的云形成模板，当我们回到云形成时就会看到。

这包括许多角色和策略，使 it 能够存储资产以供部署。

正如您在这里看到的，CDK 工具包堆栈是从引导过程中创建的。

现在，如果我们切换回 cloud nine，我们实际上可以运行 deploy 命令。

部署命令 resynthesize 是应用程序。

如果在人口普查中有任何 IAM 政策变化，它将显示这些变化，并询问您是否要继续。

在这里，因为我们要为主题订阅队列，所以需要一个 IAM 策略来实现这一点。

所以是的，我们确实希望这样。

所以我们来部署一下。

当 CloudFormation 运行时，如果我们刷新 CloudFormation，我们将看到 CDK 研讨会堆栈正在创建中。

我们可以在资源被创建的时候看到它们。

这里我们可以看到，队列和主题中有一个元数据条目，但是还没有实际的订阅或与之相关的策略。

现在这个箱子已经完成了，我们可以回到参考资料中，看到有一个队列策略和一个订阅。

嗯，那很有趣。

但是现在让我们弄脏她的手。

在这一点上，CDK 研讨会将带您经历几个步骤。

我们将通过删除 SNS 主题和 SQ sq 来清理堆栈。

然后我们将创建一个由 lambda 支持的简单 API 网关。

所以让我们开始吧。

我们将从堆栈中删除示例代码。

然后我们要做一个 diff，看看它显示了什么。

回到堆栈文件。

并删除队列主题和随导入一起添加描述的主题。

在 CDK 差异命令上。

CDK·迪夫所做的是重新合成云形成应用程序，并将山药从一个到另一个进行比较。

我们在这里可以看到，它正在破坏队列队列策略订阅和主题。

接下来，我们可以在部署时进行 CDK 部署，云形成堆栈唯一需要保留的是 CDK 元数据条目。

直到我们回到云的形成，我们可以通过刷新资源来检查它的进展。

他们现在完成了，让我们回到九霄云外，创建 lambda。

我们将创建一个名为 lambda 的文件夹，并将一个 hello.js 文件放入其中。

在里面，我们会放一些基本的 lambda 代码。

接下来，我们需要在堆栈中创建 lambda，我们将从 CDK 添加 lambda 导入，然后创建实际的 lambda 函数。

使用 CDK 创建 AWS 资源的最大好处之一是智能感知。

从 AWS lambda 导入后，我可以使用新的 lambda 点函数创建一个新函数，传入 lambda 的堆栈和 ID，并赋予它一些属性。

在这种情况下，我们需要使用 lambda.runtime.no，Gs 14 找到代码所在的运行时，我们使用来自资产的代码点，然后使用 lambda，将 lambda 文件夹作为资产上传到 s3。

然后他们是负责人。

在这种情况下，我们的处理程序和 Hello，Hello 文件，它被命名为 handler。

现在我们可以进行比较了。

这将向我们展示，它将为 lambda 和 lambda 函数本身创建一个 im 角色。

对我来说这看起来不错。

所以让我们来部署它。

我要把这部分加快一点。

我们可以看到它在云形成中的功能和作用。

所以让我们测试一下 lambda 本身。

在 lambda 控制台的代码源区域，我们可以通过单击 test 按钮来创建一个测试事件。

我们将选择 API 网关 AWS 代理，因为最终，我们将把它连接到 API 网关。

别忘了给它起个名字。

不喜欢空格。

然后点击创建。

我们再测试一次，它会调用 lambda。

现在我们可以看到 200 的状态。

身体有正确的反应。

所以让我们添加一个 API 网关。

首先，我们将从 CDK 导入 API 网关模块。

然后我们将创建一个 lambda REST API。

Lambda rest API 只是带有贪婪代理的 rest API，它将所有内容发送给定义的 Lambda。

我们将传入堆栈结构，并给 REST API 一个 ID。

然后我们将把我们的 hello 函数作为 API 处理程序告诉它。

现在我们可以做一个比较。

我还将通过转到 API 网关控制台来展示实际上还没有定义 API 网关。

这里没有 API。

所以我们来部署一下。

我要再加速一次。

但它所做的是自动创建一堆资源。

还有 API 网关本身对网关的权限，以及对它调用 lambda 等的权限。

现在完成了，我们可以看到在 CloudFormation 中我们从 3 个资源增加到了 15 个。

这都是用几行代码处理的。

如果我去刷新 lambda 页面，我们可以看到 API 网关作为触发器添加到 lambda 控制台。

类似地，我们可以看到 API 是在 API 网关控制台上定义的。

所以我们来测试一下。

我们将获取作为部署的一部分自动输出的 API URL，并向它发送一个 curl 请求。

很好。

让我们试试另一个。

非常好。

到目前为止，我们所做的只是使用了一些二级结构。

让我们做一个自己的三级建筑。

我们将创建一个拦截器 lambda，它写入 DynamoDB 表，然后调用任何目标下游 lambda。

让我们开始吧。

我们将在 lib 文件夹中创建一个新的计数器类型脚本文件，并将一些样板构造代码放在这里。

所有这些都是为了扩展构造和寻找道具。

接下来，让我们创建拦截器 lambda 和 lambda 文件夹，我们将创建点击计数器点 j，s。

我们将从车间复制代码。

这段代码使用 AWS SDK 中的两个模块与 Dynamo DB 交互，lambda 将使用 Dynamo DB 中的更新项来增加路径的命中计数器。

我们将使用 lamda 客户端来调用下游的 lambda，然后返回来自该 lambda 的带有表的响应，lambda 将通过环境变量传递给这个拦截器 lambda。

现在已经完成了，我们需要创建表和 lambda，我们将添加来自 CDK 的 DynamoDB 模块。

然后使用新的 DynamoDB 点表创建表，我们将传入堆栈结构并给表一个 ID。

而且 DynamoDB 还需要定义一个分区键。

因此，我们也将继续这样做。

接下来，我们将创建 lambda，这遵循与之前相同的模式，做 lambda 点函数，传递给它一个 ID，并设置运行时。

处理程序，在这种情况下是点击计数器点处理程序，然后我们需要在环境变量中传递来自资产 lambda 的代码点的代码。

我将对 lambda Id 进行调整，以便与研讨会保持一致。

现在我们要将 lamda 公开为该构造的只读属性。

所以将这一公共只读行添加到类中。

我们会把λ赋给这个属性。

现在这个构造本身很棒，但是它没有在任何地方被使用。

所以让我们解决这个问题。

我们将回到主车间。

并使用该构造。

这和二级结构的模式一样。

所以我们可以在堆栈中进行一次新的点击计数器传递，给它一个 ID。

在我们的例子中，这里的属性是我们想要调用的下游 lambda。

所以我们将下游设置为 Hello。

我将使用一个技巧从本地文件导入计数器。

但是现在我们需要让 API 使用这个 lambda。

因此，我们将移动下面的 API，并将 API 的处理程序从 hello 更改为 hello with counter，这就是我们公开处理程序的只读属性的原因，这样 API 实际上可以访问构造中的 lambda。

所以我们来部署一下。

通过一些编辑技巧，我们将再次跳过。

现在我们已经部署好了，我们可以测试它了。

CDK 研讨会揭示了 LTU 建筑的一些优点。

正如我们所看到的，这里的请求失败了。

为了了解为什么我们需要查看日志。

所以让我们切换到控制台中的 lambda 控制台。

单击监控选项卡，然后单击在云监控中查看日志。

如果我们转到最新的日志流，我们可以看到有一个调用错误，这是因为一个访问被拒绝异常。

如果你读得更深入一点，它基本上说 lambda 没有被授权在资源上执行 Dynamo DB 更新项目。

我们从未允许过。

所以让我们回到点击计数器文件。

我们将使用表 L 来构造 grant readwrite 数据的 helper 方法，以将正确的策略语句应用到 lamda。

车间还忘记了添加 lambda 的权限，以便能够调用下游的 lambda。

所以我们现在就继续添加。

现在，如果我们再次部署，向前跳过并测试功能，我们将获得成功的响应，因为一切都有正确的权限。

从这里，如果我们转到 Dynamo DB，并刷新表中的项目，我们可以看到正在跟踪正确的端点。

从 CDK 研讨会，我们将 npm 安装 CDK 迪纳摩表查看器库到我们的项目中。

安装好之后，我们可以切换到堆栈，从库中导入表查看器。

然后我们将创建它的一个新实例。

这个表查看器库是一个 L ^ 3 结构，因为它将多个 L ^ 2 结构组合在一起，就像我们在上一节中对计数器 L ^ 3 结构所做的那样。

table viewer 构造希望我们从另一个构造传入 DynamoDB 表，但是我们没有公开它。

因此，让我们回到我们的构造，添加表属性，并将表分配给该属性。

这样，我们可以将 hello with counter dot 表属性传递到我们的第三方构造中。

现在，如果我们部署并向前跳过，我们将看到表查看器构造创建了一个单独的 API 网关，并在输出中定义了自己的端点。

如果我们去那里，它将最终显示 DynamoDB 表中的内容。

测试构造是 CDK 最强大的功能之一。

CDK 研讨会将带你经历两种类型的测试，断言测试和验证测试。

对于第一个断言测试，我们将创建一个堆栈并使用我们的 hit counter 构造，然后验证是否创建了一个 DynamoDB 表。

所以让我们回到代码。

创建测试并删除旧的测试，现在我们可以运行它了。

并创建了一个 DynamoDB 表。

接下来，我们将继续检查λ。

CDK 断言库有一些有用的特性，比如 capture，它可以截取不同的属性作为模板合成的一部分。

在这种情况下，我们使用 CAPTCHA 来验证正确的下游函数和表名被传递到命中计数器 lambda 中。

当我们运行这个测试时，我们预计它会失败，因为我没有从合成输出中复制引用。

但让它失败实际上更容易找到它们。

因此，我们将获取正确的名称，并用正确的名称更新测试。

现在当我们运行它，它会通过。

接下来，让我们切换到测试驱动的开发思维模式。

假设我们希望确保我们的表使用服务器端加密，我们可以断言 DynamoDB 表资源启用了服务器端加密。

所以当我们运行测试时，它会失败。

从开发的角度来看，这意味着我们需要回到构造，并确保启用服务器端加密。

如果我们再做一次测试，会通过的。

因为我们实际上并不需要它，所以我将把它从构造中移除，并移除测试。

接下来，我们来谈谈验证测试。

假设作为构造的一部分，您希望确保 DynamoDB 表中使用相同数量的读取容量单元。

我们可以添加一个 read capacity 属性，传递到构造中。

在表中使用它。

然后在构造函数中，我们可以验证传入的值是合理的，比如在 5 到 20 之间。

如果不是，我们将抛出一个错误。

现在让我们添加一个测试，将一个超出范围的值传递给一个 3，并验证是否抛出了错误。

当我们运行测试时，它会通过，因为错误被抛出。

该错误将作为堆栈合成的一部分被抛出。

让我们看看，我们将回到我们的车间堆栈。

添加越界读取容量值并运行堆栈合成。

如你所见，验证测试非常有用。

在我的工作中，我使用它来帮助执行一些命名约定，并且在属性输入或默认值发生变化时记录或抛出不赞成使用的警告。

研讨会做得很好。

既然研讨会已经结束，我们可以继续讨论一些高级主题，比如方面和最佳实践。

在研讨会中，我们检查了细粒度的断言，检查了创建的表的数量，并确保加密已打开。

我们正在检查以确保 lamda 环境变量被传入或者是正确的。

但是如果您想使用 gests 匹配快照功能，您也可以使用快照测试来检查整个模板。

作为研讨会的一部分，我们还回顾了验证测试。

我们检查了传递到构造中的属性，以确保读取容量单位在特定的范围内。

在我的组织中，我们也使用它来帮助执行一些命名约定，以及在我们必须显著改变构造的情况下抛出一些不赞成的警告。

在这种情况下，警告不会阻止部署，但在进行合成部署时，它会显示在控制台中，开发人员可以在控制台中选择并修复它们。

但是，您并不仅限于将错误记录到控制台，当您发送时，您正在执行实际的代码。

因此，您也可以登录到外部服务，如跟踪部署的平台服务，或登录到云观察本身。

最后一种形式的测试是集成测试。

通过 CDK，您可以使用 AWS 定制资源来测试这些资源是否作为 CloudFormation 部署的一部分正确地协同工作。

如果出现问题，它会自动回滚云形成部署。

CDK 有一个提供者框架，用于与云形成定制资源接口。

这些定制资源使您能够编写定制的配置逻辑和模板，CloudFormation 会在您创建、更新或删除堆栈时运行。

我们可以利用这一点在整个堆栈中进行集成测试。

假设你有一辆活动巴士，一辆 lamda。

和 DynamoDB 表之间的交互。

您可以使用提供者框架来构建一个 lambda，在事件总线上发出一个测试事件，这是 CloudFormation 部署的所有部分。

然后自定义资源可以查询 DynamoDB 等待更改。

如果更改没有在设定的时间内发生，构建将失败，CloudFormation 将自动回滚更改。

马特·摩根有一篇关于这个的极好的博客文章，叫做用 AWS CDK 测试异步云。

CDK 的书也有这方面的一节。

CDK 的另一个高级主题是方面，方面在 CDK 是一个非常强大的工具。

有一种方法可以将操作应用于给定范围内所有构造。

该方面可以修改构造，比如通过添加标签，或者它可以验证某些东西或关于构造的状态，比如确保所有的桶都被加密。

在准备阶段，CDK 按照自上而下的顺序调用构造及其每个子对象的访问方法。

访问方法可以自由地改变构造中的任何东西。

在本例中，bucket 版本检查类实现了一个访问方法，该方法检查 s3 bucket 版本控制是否启用。

这里它抛出一个错误，但是您可以很容易地修改这个构造来启用版本控制。

堆栈应用到的所有内容，即堆栈中的每个构造，都由这个访问方法进行评估。

这就是为什么我们需要检查被评估节点的实例，以确保它是 CFN 桶。

这些节点最终都将成为正在讨论的资源的一级结构。

即使您在堆栈代码中使用了二级桶结构。

对于最佳实践，AWS 将其最佳实践建议分为四个不同的领域。

组织编码构建应用程序。

对于组织的最佳实践，AWS 建议让一个 CDK 专家团队帮助制定标准，并培训或指导开发人员使用 CDK。

这不一定是一个大团队。

它可以小到只有一两个人，或者如果你有一个大的组织，它可以是一个卓越的中心。

AWS 还推荐部署到多个 AWS 帐户的做法。

例如，拥有独立的生产、质量保证和开发账户。

您应该使用持续集成和持续部署工具，如 CDK 管道来部署超越开发帐户。

对于编码最佳实践，AWS 建议只在您真正需要的地方增加复杂性。

不要预先设计所有可能的场景。

所以保持简单，愚蠢的原则。

它还建议使用架构良好的框架 AWS CDK 应用程序是一种编纂和交付架构良好的最佳实践的机制。

当您遵循这些原则时，您可以创建和共享实现它们的组件。

之前我说过在一个项目中有多个应用是可能的，这是真的，但是不推荐。

最佳实践是不要这样做，每个回购只有一个应用程序。

在 CI CD 世界中，一个应用程序的变化可能会不必要地触发另一个应用程序的部署，即使它没有发生变化。

最后，CDK 代码和实现运行时逻辑的代码应该在同一个包中。

它们不需要存在于单独的存储库或包中。

对于构造最佳实践，您应该构建代码逻辑单元的构造。

如果你一直用 s3、buckets、API gateways 和 lambdas 来构建网站，那么就把它作为一个构造，然后在你的栈中使用这个构造。

在构造中使用环境变量查找是一种不好的做法。

环境变量查找应该发生在 CDK 应用程序的顶层，然后作为属性传递给堆栈和构造。

通过使用环境变量查找和构造，您失去了可移植性，并使您的构造更难重用。

如果您在合成期间避免网络查找，并在代码中对所有生产阶段进行建模，那么您可以在构建时在所有环境中一致地运行一整套单元测试。

请注意重构代码的方式，这可能会导致逻辑 ID 的更改。

逻辑 ID 源自您在实例化构造时指定的 ID，以及构造在应用程序树中的位置。

更改资源的逻辑思想会导致在下一次部署时用新的资源替换该资源，这几乎是您所不希望的。

构造是伟大的，但是它们不是为了强制遵从而构建的。

遵从性更适合使用服务控制策略和权限边界。

对于应用程序最佳实践，请在综合时做出决定。

不要使用云的形成条件和参数。

例如，CDK 常用的遍历列表并为每个列表实例化一个构造的做法，使用 CloudFormation 表达式是不可能的。

将 CloudFormation 视为实现细节，而不是语言目标。

你不是在编写 CloudFormation 模板，你是在编写恰好生成它们的代码。

如果您遗漏了资源名称，CDK 将为您生成它们。

并且它会以一种不会导致你以后重构代码时出现问题的方式来实现。

CDK 通过默认保留您创建的所有内容的策略，尽最大努力防止您丢失数据。

但是云手表价格昂贵。

浏览并设置您自己的保留策略。

CDK 也有助手方法，在销毁堆栈之前使用自定义资源清空 s3 存储桶，这非常有用。

考虑将有状态资源(如数据库)与无状态资源放在不同的堆栈中。

您可以为有状态堆栈打开终止保护，同时为无状态堆栈关闭一次。

有状态资源对构造重命名更敏感，这是将它们保存在单独的堆栈中的另一个原因。

它降低了以后重组应用程序的风险。

确定性是成功部署 CDK 的关键。

CDK 上下文是一种机制，用于记录来自合成的非确定性值的快照。

这允许将来的合成操作产生相同的精确模板。

不确定的值会导致使用类似于查找外部资源的构造。

这些来自查找值被缓存在 CDK 上下文 dot JSON 中。

grant 便利方法允许您大大简化 IAM 流程。

在 AWS 帐户中手动定义角色或使用预定义的角色会导致应用程序设计的巨大损失和灵活性。

服务控制策略和权限边界是预定义角色的更好替代方案。

另一个 CDK 最佳实践是为每个环境创建一个堆栈。

当您合成您的应用程序时，在 CDK dot out 文件夹中创建的云集合包含每个环境的单独模板。

你的整个构建是决定性的。

CDK 的大多数二级结构都有方便的方法来帮助创建指标，您可以轻松地使用它们来生成 CloudWatch 仪表板和警报。

最后，我还想介绍一些资源。

自由代码营有一篇关于如何使用 CDK 第二版创建三层无服务器应用程序的博文。

我的博客上有几篇与 CDK 相关的文章，但其中一篇特别展示了使用通用编程语言可以让你进一步扩展 CDK。

在我从 CDK 公开的 API 规范中。

在不部署第一篇文章的情况下，我将展示如何在合成过程中遍历 API 网关端点结构，以便在不部署的情况下首先输出一个开放的 API 规范。

最后，无服务器堆栈框架非常简洁。

它延伸了 CDK，促进了当地的 lamda 开发。

在进行开发时，它有一个非常简洁的观察控制台。

如果你已经学完了这一课，绝对值得一试，感谢观看。

谢了。

你可以在推特上关注我。

我的其他社交网站和博客都在 marks dot codes 上。

这是一个免费的代码营 CDK 速成班。

谢谢，再见。