# 为什么我改变了对代码质量的看法

> 原文：<https://www.freecodecamp.org/news/why-i-changed-the-way-i-think-about-code-quality-88c5d8d57e68/>

作者约翰·科布

# 为什么我改变了对代码质量的看法

![F19UjZTNIQOwYT-ZyMaq1PV0t3AKjd0oZI3a](img/a47a27389d112d761a6019fb9416ba8c.png)

80% of the code that makes up the Matrix is unnecessary bloat.

当你想到代码质量时，你会想到什么？

是一致性吗？通过 linter 规则和格式化程序在您的代码上实施一套标准和最佳实践？如何确保您的代码在构建过程中有自动运行的测试？那么拉请求和代码审查呢——保护您的主分支不被直接提交，并让同行审查您的代码？

它们是我想到的一些事情。自动化流程和人工检查。聪明高效。然而，尽管它们都很有用，但它们实际上只解决了问题的一半。

### 我们不能自动化所有的事情

自动化对于维护代码质量至关重要。用 linter 对你的语法进行静态分析和自动化测试应该是强制性的。但是我可以编写通过所有自动化过程的代码，而不保证它的实际质量。

代码是否遵循既定的模式？它是使用现有的模块，还是重复代码？所有东西的名字都很合理吗？代码在代码库中的位置正确吗？这种变化会有更广泛的、意想不到的影响吗？这段代码真的解决了它想要解决的问题吗？它还能用吗？

自动化过程还不能为你回答这些问题。如果你(或其他人)没有对你的代码提出这些问题，那么你可能没有交付*质量*的代码。这就是我们进行代码审查的原因。

#### 一个好的代码评审应该不仅仅是关于代码

当然，代码评审应该是关于代码的(毕竟它就在名字里)。但也应该是关于上面提出的更广泛的问题，以及关于最终产品。

我注意到开发人员有一种将代码评审视为敷衍了事的倾向。对修改后的代码进行初步检查。对任何明显错误的评论(或者只是挑一两个小毛病，让自己看起来很忙)。

五分钟，任务完成。*我看不错*。

但是，不满足任务需求的代码不是高质量的代码。在设备/浏览器中产生控制台错误或视觉错误的代码不是高质量代码。这些事情都不能在敷衍的代码审查中发现。除非你*实际运行代码*，否则你无法充分地审查代码。

我建议一个好的代码审查*应该*至少包括:

*   将分支拉到本地环境。
*   构建项目(并检查 linter 和测试是否全部通过)。
*   检查代码在目标浏览器/设备中是否运行无误。
*   检查已完成的工作是否符合任务的要求。

如果这些步骤中的任何一个有任何问题，拉取请求应该被拒绝。不要通过 Go。不要收 200 美元。不要合并到母版。

评审人员也应该将代码评审作为提问的机会。如果您不理解代码，那么您不应该批准拉取请求。不要假设作者知道的比你多——如果这对你没有意义，要求澄清。

对于代码的质量，评审者和作者有同等的责任。这是维护代码质量的一种基本心态。

全面的代码审查对确保代码质量大有帮助。但是，在打开拉取请求之前，您可以采取一些步骤。您可以做一些小事情，这将有助于提高代码的质量，并减少审查代码所需的工作量。

### 仔细检查你自己工作的完整性

我有一个令人讨厌的习惯。当我写完一个任务的最后几行代码时，我会在心里把这个任务看作是*完成*。

如果我能听到我脑子里那个不耐烦的声音，我会马上提交我的请求。但是该代码可能包含以下许多或全部内容:

*   错过的需求。
*   缺少测试用例。
*   多余的、未使用的或草稿代码。
*   代码注释不足。
*   某些浏览器/设备中的视觉缺陷。

如果你的代码中有任何一点是正确的，那么你的代码就是不完整的。如果这些东西中的任何一个出现在主分支中，那么你就降低了代码库的质量。

这里的要点是:代码质量从代码作者开始。你不应该依赖自动化任务、代码审查、质量保证或用户验收测试来发现你的错误。

对完整性的双重检查是保证代码质量的第一步。这是最容易迈出的一步，但也是最容易被忽视的一步。

只有当您确定代码已经完成时，才应该打开一个拉请求。

### 对你的分支机构进行自我评估

我总是惊讶于我能在自己的代码中找到如此多的问题——或者改进解决方案的机会。只有当我退后一步，孤立地看待我的变化时，问题和机会才变得对我可见。

在指派团队成员审核您的工作之前，您可以审核您的工作并应用您自己的反馈。您还可以利用这个机会在拉取请求上留下评论，以便向审阅者澄清任何事情。

花时间确保你的工作是完整的，纠正明显的错误，或者评估你的解决方案，将会提高你的代码质量。它还减少了审查它所需的工作量。

这也可能让你免于尴尬。我知道对我来说是的。

### 确保代码质量应该是每个开发任务的内在要求

您可能认为这种方法增加了任务的时间。你说得对，确实如此。但这并不是一件坏事。

效率很重要，但是懒惰和冷漠是有害的。冷漠导致臃肿、不一致的代码库。懒惰造成了越来越多的不良技术债务。我们不能被动的维护代码质量。这需要时间和努力。

改变围绕代码质量的文化可能很难。项目经理和产品负责人通常不关心代码质量——他们有自己的顾虑。为代码质量过程请求额外的时间有时会被忽视。然而，维护代码质量不应该被认为是额外的事情，它应该是每个任务的内在要求。

作为开发人员，如果我们不改变我们思考代码质量的方式，我们就不能指望其他人改变。

我在这里谈到的没有什么是特别开创性的，也不是规定性的。不是每个团队、工作场所或项目都是一样的，上面的一些可能对你不适用。

我确实相信在开发人员思考代码质量的方式和解决它的实际行动之间经常存在差距。如果你也发现了这一点，那么希望你能从这篇文章中学到一些东西——或者也许你已经采取了不同的方法来解决它。很想在评论里听到大家的建议。

感谢阅读！