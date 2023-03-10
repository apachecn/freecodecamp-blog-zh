# 如何开始持续集成

> 原文：<https://www.freecodecamp.org/news/how-to-get-started-with-continuous-integration-7b2f8d87c914/>

开始持续集成所需要知道的一切:分支策略、测试自动化、工具和最佳实践。

### 目标是:快速安全地交付工作代码

持续集成的目标是向存储库的主要分支交付代码:

*   快速:从将新代码推送到存储库，到工作时将其合并到主分支，应该在几分钟内完成
*   安全地:我们如何知道新代码有效？持续集成是关于建立正确的检查来完全信任地自动合并代码

持续集成与工具有一点关系，与团队的心态和文化有很大关系。您希望您的开发过程能够促进新代码的快速集成，同时始终保持一个有效的主分支。这个工作的主分支将在未来实现连续交付或连续部署。但这是另一个帖子的内容。让我们首先关注持续集成。

实现持续集成有两大支柱:

#### 分成小块工作

想象一下，一个 5 人团队正在开发一个 SaaS 产品。他们每个人都开发了一个独立的新功能。每个功能的工作量大约为 1 或 2 周。这里有两条路可以走。

*   团队可以选择特色分支。每个开发人员将在一个“特性分支”上工作。一旦每个人都对自己的工作满意了，这些分支就会被合并到主分支中
*   团队可以使用分支(仍然),但是在每次推进时将他们的工作集成到主分支中。即使事情仍在进行中！主分支的任何最终用户或测试人员都不会看到正在进行的工作

你认为哪种方法最有效？

第一种方法将最终导致“不可预测的发布综合症”。长期存在的特性分支为每个开发者创造了一种虚假的安全感和舒适感。由于分支在很长一段时间内相互分离，因此无法衡量将它们全部合并的难度。在最好的情况下，会出现一些小的代码冲突，在最坏的情况下，基本的设计假设会受到挑战，事情将不得不返工…这是一种艰难的方式。

> 返工将在时间压力下完成，导致质量下降和技术债务的积累。这是一个恶性循环。

请看关于[为什么不应该使用特性分支](https://fire.ci/blog/why-you-should-not-use-feature-branches/)的帖子，了解更多细节。

第二种方法是我们实现持续集成所需要的。每个开发人员都在他们自己的分支上工作。不同的是:

> 每次推送时，变更都被合并到主分支，每个开发人员每天都要将他们的分支与最新的主分支版本同步几次。

这样，团队可以更快、更容易地解决冲突并调整设计假设。尽早发现 5 个小问题比在发布日之前发现 1 个大问题要好得多。查看下面的“特性切换”部分，了解如何将“进行中的工作”集成到主分支中。

#### 自动化检查带来了安全性

古代的软件开发过程是基于一个构建周期，然后是一个测试周期。这可能仍然符合上面描述的“特征分支”方法。如果我们每天集成和合并代码几十次，手工测试就没有意义了。那会花太长时间。我们需要自动检查来验证代码是否正常工作。我们需要一个 CI 工具，它将自动接受每个开发人员的推送并运行构建和测试。

测试的类型和内容应该是:

*   足够快，可以在几分钟内为开发人员提供反馈
*   足够彻底，可以完全放心地将代码合并到主分支

不幸的是，没有一种尺寸适合所有测试类型和内容。正确的平衡取决于您的项目。在 CI 阶段，不要运行大型且耗时的测试套件。这种测试提供了更好的安全性，但是它们是以延迟对开发者的反馈为代价的。这导致了纯粹浪费时间的上下文切换。

### 优化开发人员的时间并减少上下文切换

长时间的 CI 检查，我说的长时间是指超过 3 分钟，会给团队中的每个开发人员带来复合的时间浪费。让我们比较一下“好的”和“坏的”工作流。

“好的”工作流程:

*   你提交并推动你的代码
*   CI 构建和测试运行 1 到 3 分钟
*   在 1 到 3 分钟的时间里，你回顾手头的任务，在一些管理工具中提升状态，或者再次回顾你的代码
*   在 3 分钟内，你得到一个成功的状态:你可以继续下一部分的任务。如果你得到一个失败的构建:你可以马上修复这个问题

“糟糕”的工作流程:

*   你提交并推动你的代码
*   CI 构建和测试运行 15 分钟
*   在这 15 分钟里你做了什么？
*   你可以和团队一起喝杯咖啡。很公平，但是你一天能吃几个？
*   你可能会专注于下一个工作
*   15 分钟后，您会收到构建失败的通知。你需要切换回之前的任务，尝试解决问题…然后再进行 15 分钟的循环…
*   这时，你会想:我是应该再回到下一个任务上，还是只等 15 分钟，让自己平静下来，因为我已经真正完成了当前的任务…

糟糕的工作流程不仅仅是浪费时间。这也让开发者感到沮丧。高效的开发人员是快乐的开发人员。

> 你需要调整你的工具和工作流程，让你的开发人员满意。

### 工具

#### 分支

持续集成是指将来自不同开发人员分支的代码集成到配置管理系统中的一个公共分支。您可能正在使用 git。在 git 中，存储库中的默认主分支称为“master”。一些团队创建一个名为“开发”的分支，作为持续集成的主要分支。他们使用“主”来跟踪交付和部署(开发被合并到主)。

您可能已经有了一个主要分支，您的团队会将代码推进或合并到其中。坚持下去。

每个开发人员应该在他们自己的分支上工作。如果同时处理许多不同的主题，可以使用多个分支。尽管这充其量是工作“不专注”的表现。一旦代码的一致部分准备好了，就推你的库。如果成功的话，CI 检查将会开始并将您的代码合并到主分支中。如果检查失败，您仍然在自己的分支上，可以修复任何需要修复的问题并再次推送。

上面过程中重要的一个词是**你的代码**中一致的部分。你怎么知道它是一致的？简单。

> 如果你能很容易地想出一个好的提交信息，它就是一致的。

另一方面，如果你的提交信息需要 3 个项目符号和许多形容词和副词，这可能不好。将您的工作分成多个一致的提交。然后推码。一致的提交有助于代码审查，并使存储库历史更容易跟踪。

不要随便推，因为这是一天的结束！

#### 拉取请求

什么是拉取请求？拉请求是一个概念，在这个概念中，您要求团队将您的分支合并到主分支。接受您的请求应该通过由您的 CI 工具提供的状态，并且可能通过代码审查。最终由负责合并拉请求的人进行人工接受。

拉式请求诞生于开源项目。维护人员需要一种结构化的方法来评估贡献，然后再将它们合并进来。拉请求不是 Git 的一部分。但是，任何 Git 提供者都支持它们(GitHub、BitBucket、GitLab 等等)。

请注意，拉请求对于持续集成并不是强制性的。它们的主要好处是支持代码审查过程，这不能通过设计来自动化。

如果您使用拉式请求，同样的“小块工作”和“优化开发人员时间”原则也适用:

*   保持每个拉取请求的规模小，并且有一个明确的目的(这将使代码审查更容易)
*   快速进行 CI 检查

#### 自动检查

连续过程的核心是自动检查。它们确保在您的代码被合并后，主分支代码能够正常工作。如果失败，您的代码不会被合并。作为最低要求，代码应该编译或转换，或者你的技术栈正在做的任何事情，以便为运行时做好准备。

在编译之上，你应该运行自动化测试来确保软件正常工作。测试覆盖越好，您对合并到主分支的新代码就越有信心。不过要小心！更好的覆盖率意味着更多的测试和更长的执行时间。你需要找到正确的权衡。

当你根本没有测试或者需要减少一些长时间运行的测试时，你从哪里开始？关注对你的项目或产品至关重要的东西。

如果您正在构建一个 SaaS 应用程序，您应该检查用户是否可以注册或登录，并执行您的 SaaS 提供的最基本的操作。除非您正在开发 Salesforce 竞争对手，否则您应该能够在几分钟内运行您的测试，如果不是几秒钟的话。如果您正在构建一个繁重的数据处理后端:使用有限的数据集来练习不同的构建块。将大型数据集上的长时间运行排除在持续集成之外。合并代码后，可以触发长时间运行的测试。

### 专业提示

#### 功能切换

持续集成的关键概念是尽快将您的代码放到主分支中。甚至是正在进行的工作。甚至是那些不能完全工作或者你不想向测试人员或最终用户公开的特性。实现这一点的方法是使用特征切换。让您的新功能处于启用/禁用切换状态。切换可以是编译时布尔标志、环境变量或运行时事物。正确的方法取决于你想要达到的目标。

特性切换的第一个主要好处是，您可以将它们投入生产，并根据需要启用/禁用新特性。您可以使用更改的环境变量重新启动服务器，或者打开/关闭新的 UI 仪表板布局。通过这种方式，您可以充分灵活地推出该功能。或者在生产中导致意外问题时禁用它。或者允许最终用户选择加入或退出该功能(在 UI 切换的情况下)。

特性切换的第二个主要好处是，它们迫使你考虑你正在做的事情和现有代码之间的界限。这是一个很好的练习，无论如何，这也是你每次在现有系统中添加内容时应该开始做的。特征切换步骤使得该过程的这一步骤更加可见。

特性切换的唯一缺点是，您需要定期从环境和代码中清理它们。一旦一个特性经过实战测试并被用户采用，它就应该是默认的。切换的代码和旧版本的东西(如果有的话)应该被清理。不要落入“配置如开关”系统的陷阱。缺点是你将永远无法维护和测试所有的触发器组合，最终会得到一个脆弱的架构。

#### 将 CI 构建时间控制在 3 分钟以内

记住文章第一部分中的“好的”和“坏的”工作流。我们希望避免开发者的上下文切换。拿起你的手机，设置一个 3 分钟的定时器。当你只是在等待什么的时候，看看它有多长！3 分钟应该是你集中注意力，安全有效地从一项任务转移到另一项任务的绝对上限。

对于一些团队来说，3 分钟以内的构建可能看起来很疯狂，但是这绝对是可以实现的。它更多的是与你如何组织工作有关，而不是与你使用的工具有关。优化构建的方法有:

*   使用更多的构建能力:如果您的 CI 工具上没有足够的并发构建，并且构建被排队，那么开发人员会浪费时间
*   利用缓存:大多数技术栈要求您在运行新版本时安装和配置依赖项。当依赖关系没有改变时，您的 CI 工具应该能够缓存这些步骤，以优化构建时间
*   检查您的测试:检查您的测试是否针对时间进行了优化。去除超时和“安全的长时间”等待步骤。如果您有大量的测试套件要运行，请考虑将它们移动到一个单独的构建中，该构建在合并到主分支之后运行。它们将不再是持续集成保障的一部分，但是繁重的测试无论如何也不应该是
*   分割你的代码库:你必须把所有的东西都放在一个库中吗？你必须对所有东西都进行构建和运行测试吗，即使一些小的部分发生了变化？这里可能会有胜利。
*   有条件地运行测试:仅当某些目录发生变化时才运行测试。如果你的代码库组织良好，这将是一个巨大的胜利

> 对你的 CI 检查强加一个短时间限制的好处是，它要求你从根本上改进你的整个开发过程。

正如[吉米·罗恩](https://www.jimrohn.com/)所说:

> "成为百万富翁不是为了百万美元，而是为了达到这一目标你会得到什么"。

#### 虚拟合并:代码本身并不重要

大多数持续集成工具在您的分支上运行 CI 构建，以确定它是否可以被合并。但这不是我们感兴趣的。如果你知道自己在做什么，那么你刚刚推送的代码很有可能已经在工作了！您的 CI 工具应该验证与主分支合并的分支是否正常工作。

您的 CI 工具应该执行您的分支到主分支的本地合并，并运行构建和测试。如果主分支在此期间没有变化，您的分支可以自动合并。如果确实发生了变化，CI 检查应该再次运行，直到您的代码可以安全地合并。如果您的 CI 工具不支持这种工作流，请更换您的工具。

#### 邪恶的任务经理

有一种误解，认为能够在你的敏捷板或者像 JIRA 或类似的 bug 跟踪器中跟踪与任务相关的代码是很酷的。虽然这在理论上是一个很好的概念，但是对开发过程的影响肯定是不值得努力的。任务管理器提供了一个“特性和缺陷”的世界视图。代码的结构和分层方式非常不同。试图协调任务管理器中的一个项目和一组提交是没有意义的。如果你想知道为什么要写一段代码，你应该能够从代码上下文和注释中得到信息。

### 最后的想法

工具只是工具。设置工具可能需要 1 个小时。但是如果你用错了，你就不会得到预期的结果。

请记住我们为自己设定的持续集成目标:

*   快速安全地交付工作代码
*   优化开发人员的时间并减少上下文切换

真正的交易是改变你的思维模式，为你的项目或产品“持续交付价值”。

> 把你的软件开发过程想象成一个硬件生产设施。开发人员的代码代表移动的部分。主要分支是组装产品。

你越快地将不同的部分整合在一起，并检查工作情况，你就越接近最终的工作产品。

几个实际例子:

*   您正在开发一个新功能，并且必须更改一个其他人很可能会使用的低级组件。对公共组件部分进行专门提交，并使其已经合并。然后继续处理你的特征的其余部分。其他开发人员将能够立即将他们的工作基于您的更改。
*   您正在开发一个需要大量时间和代码的大型特性？使用功能切换。不要孤立地工作。永远不会。
*   您正在等待代码审查，但是没有人可以完成这项工作。如果你的代码通过了 CI 检查，那么就合并它，然后进行代码复查。如果这听起来像是打破流程，记住“完成总比完美好”。如果它在工作，它在主干道上提供的价值比停在路边几天要多。

**感谢阅读！如果你觉得这篇文章有用，请点击下面的按钮。这对我来说意义重大，也有助于其他人了解这个故事！**

*文章原载于 2019 年 4 月 9 日 [fire.ci](https://fire.ci/blog) 。*