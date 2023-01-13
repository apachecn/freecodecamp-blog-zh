# 如何在没有单元测试的情况下设置持续集成

> 原文：<https://www.freecodecamp.org/news/continuous-integration-without-unit-tests/>

你认为持续集成不适合你是因为你没有自动化测试吗？或者根本没有单元测试？不是真的。测试很重要。但是持续集成不仅仅是测试，还有很多方面。让我们看看它们是什么。

## **1。构建代码库**

这是持续集成应该解决的最关键的问题。代码库的主要分支应该总是构建/编译。提这件事似乎很傻。

带一个 12 人的团队。假设 1 个错误提交进入主分支。每个人都拉。从这里开始，找出问题所在，并协调谁应该或将要修复它。这种混乱会让你的整个团队在大约 30 分钟内失去焦点。此外，它还会导致挫败感。

假设这种情况每周发生一次(每个人最终都会犯错)。30 分钟 x 12 个人就是每周损失 8 小时。

如果你同意，你也可以:

*   建立一个 CI 流程，防止错误的构建进入主分支
*   每周给一个开发人员一天假期

同样的结果，更快乐的团队:)

设置一个 CI 流程来确保您的代码库编译不到半天的时间。努力是值得的。

## **2。静态代码分析**

这几乎在每种语言中都是免费的，并且是一个运行在一组预定义规则下的一行程序:

*   Javascript: [eslint](https://eslint.org/) ， [tslint](https://palantir.github.io/tslint/)
*   Java: [sonarlint](https://www.sonarlint.org/)
*   Python: [pylint](https://www.pylint.org/)
*   Go: [golint](https://github.com/golang/lint)

设置静态代码分析(或林挺)大约需要 1 个小时。那么有什么好处呢？在您的主分支中，您有格式良好且“按常规”的代码。对于您的代码库来说，这是一个明显的质量提升。

如果这是你的问题中最小的一个，因为你的团队总是赶着完成最后期限，那么这样想吧。您的代码审查过程将会更快。代码结构、最佳实践等领域的任何内容。已由您的 CI 流程检查。不需要回顾或讨论。您的开发人员可以专注于代码评审的业务内容。

巨大的好处:开发人员自动学习代码约定。静态分析工具向您展示违反的规则，并解释为什么这样做是错误的。

约定的一个障碍是开发人员对诸如制表符和空格之类的事情过于教条。归根结底，好的惯例是所有人都遵守的。选择一套标准惯例，并与它们一起滚动。

## **3。文化变革**

持续集成不是技术问题。这是一个团队过程。您希望以小增量工作，并经常将代码集成到主分支中。参见[如何开始 CI](https://fire.ci/blog/how-to-get-started-with-continuous-integration/) 了解关于文化目标实际上是什么的更广泛的讨论。

在团队掌握了第一个关键要素之后，另一个转变应该会发生。人们会意识到以较小的增量工作效率更高。对基本错误的自动检查将增强您的信心，这样您就可以更快地合并代码。

因此，树枝的寿命会缩短。代码审查会更快。每个人都将使用几乎最新的代码。它将防止漂移和合并冲突，由于人们分开工作。请参见[为什么不应该使用功能分支](https://fire.ci/blog/why-you-should-not-use-feature-branches/)以获得完整的优势列表。

在一天结束时，CI 帮助我们的骄傲和自我。每个人都应该很高兴有一个工具来在他们的错误出现在这个世界之前抓住它们。

## **如何开始？**

下面是一个非常简单可行的开始流程。不管你的 git 提供者是什么，它都能工作:GitHub、Bitbucket、Gitlab、Azure DevOps 和所有其他的。

### **1。启用拉取请求(PR)流程**

锁定你的主要分支，避免直接推动。一切都应该通过公关。以下是关于如何为 [Github](https://help.github.com/en/github/administering-a-repository/enabling-branch-restrictions) 、 [Bitbucket](https://confluence.atlassian.com/bitbucketserver/using-branch-permissions-776639807.html#Usingbranchpermissions-Addbranchpermissionsforasinglerepository) 、 [GitLab](https://docs.gitlab.com/ee/user/project/protected_branches.html) 和 [Azure DevOps](https://docs.microsoft.com/en-us/azure/devops/repos/git/branch-policies?view=azure-devops) 做这件事的链接。

### **2。选择 CI 平台**

每个 git 提供者都允许您为 PRs 定义构建管道。构建将在 PR 被创建时运行，并且对于 PR 携带的分支的每个新的推送。完成你的 PR(合并你的分支)的先决条件将是一个成功的建立。

CI 大玩家有 CircleCI，Codeship，Travis CI。我当然推荐 [Fire CI](https://fire.ci/) 因为它是我搭建的平台。但是我并不认为它在每个用例中都比其他的好。

挑一个就开始吧。

### **3。定义一个二衬管构建**

我们要实现的最基本的构建是构建+静态代码分析。实现这一点需要 shell 中的 2 或 3 个命令。

所有 CI 平台都支持“配置即代码”。您在*中定义您的构建。yml 文件，平台会选择它。

例如，对于 Fire CI，您需要在 repo 的根目录下添加一个. fire.yml 文件，如下所示:

```
pipeline:   
  dockerfile: Dockerfile
```

然后添加一个名为“Dockerfile”的文件来构建您的应用程序。这里有几个简单的 docker 文件的例子。

任何基于纱线/npm 的技术，如反应/角度/Vue/节点:

```
FROM python:3 
WORKDIR /app  
COPY . . 
RUN yarn
RUN yarn lint 
RUN yarn build
```

Python:

```
FROM python:3
WORKDIR /app 
COPY . .
RUN pip install all_your_dependencies
RUN pylint all_your_python_files.py
```

去吧:

```
FROM golang:latest
WORKDIR /app
COPY . .
RUN go build -o main .
```

我还可以举出更多例子。这些例子很简单，可以用更多的命令来改进。但是你明白了:这很简单。

### **可选:启用代码审查**

既然每个代码贡献都来自 PR，那么代码评审就很容易做了。每个 git 提供者都有一个很棒的 UI 来呈现不同之处，并允许您对代码进行评论。

如果你是这个过程的新手，不要定义一组强制性的评审者，因为这会降低你的团队的速度。一定要开始一个尽最大努力审查彼此代码的过程。并以此为基础。

## 然后呢？

和所有事情一样，从大处着眼，从小处着手。拥有一个合适的 CI 流程会打开一个充满机会的世界。

### **测试**

一旦您有了基本的流程，添加您的第一个自动化测试就变得轻而易举了。然后是其他一些。低而持续的努力可以在你知道之前给你带来令人敬畏的测试覆盖率。

我建议你保持精益，不要花精力写测试。检查哪些地方经常出问题，哪些地方需要大量的人工测试。实现自动化。永远记住生产力。做一大堆测试并不值得。

### **其他津贴**

有许多工具可以集成到您的 CI 流程中。它们不是关键，但是相对于收益来说，付出的努力是值得的。

下面是几个例子。链接是针对 GitHub marketplace 的，但是其他 git 提供者也很容易集成。

*   依赖关系自动更新: [Depfu](https://github.com/marketplace/depfu) 向您建议依赖关系自动更新。通过这种方式，你可以保持与时俱进的小增量。这总是比一年一次的“让我们放弃一切”策略要好。
*   开源安全: [Snyk](https://github.com/marketplace/snyk) 警告您开源库中的安全威胁。
*   图像优化: [ImgBot](https://github.com/marketplace/snyk) 检测存储库中的大图像，并提交尺寸优化版本的 PR。与前端项目相关，但仍然不错。

外面还有很多。浏览市场，寻找能为你解决问题的东西。

不过要小心！抵制使用所有想到的东西的冲动。选择那些真正能提高生产力的。你不仔细考虑的自由度量标准或工具是有害的，因为人们不确定如何使用它们。

## **结论**

您不需要花哨的测试套件来开始持续集成。

事实上，两个小时的努力就能让你兴奋起来。这将为你团队的生产力带来良性循环。

你的团队和项目越大，收益越大。到 2020 年，没有理由不采用 CI 流程。

如果您需要帮助为您的团队建立 CI 流程，请随时[联系我](https://twitter.com/jpdelimat)。如果可以的话，我很乐意帮忙。

感谢阅读，祝你好运！

*最初发表在火词博客上。*