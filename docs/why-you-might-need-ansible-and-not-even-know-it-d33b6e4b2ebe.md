# 为什么你可能需要 Ansible 而自己却不知道

> 原文：<https://www.freecodecamp.org/news/why-you-might-need-ansible-and-not-even-know-it-d33b6e4b2ebe/>

#### 用 shell 脚本配置机器非常麻烦

是否要开始使用 Ansible？你已经在使用它了，但是遇到了挑战吗？即使你不属于*任何一类*，也不要停止阅读。我将向你展示为什么你可能真的**需要** Ansible，以及如何最好地利用它。

Ansible 的口号是“简单的 IT 自动化”。这是对它所做的非常准确的描述。在它最流行的操作模式(有几种)中，您描述您的机器的期望状态，Ansible 操纵它们来达到这个状态。此时，您可能会想“是的，但是我们已经有了相应的 shell 脚本。”不过，Ansible 提供了几个优于老式 shell 脚本的优点。

首先，描述理想状态的剧本是陈述性的，用 [YAML](http://yaml.org/) 写成。使用剧本意味着你不需要自己处理错误控制和条件检查。这也意味着如果状态已经满足，则不会采取任何动作(例如，如果已经安装了`nginx`包，则`apt-get`不会运行)。

但这只是故事的一部分。

Ansible 如此强大的另一个原因是模块的使用。而不是依赖很多第三方应用(`sed`、`grep`、`jq`、`useradd`、`parted`等)。)并解析它们的输出，您可以只关注状态本身。这意味着不管底层用户空间程序(`useradd`、`adduser`、Busybox、BSD 或 GNU 变体)，您都可以指定一个通用任务，如下所示:

```
– name: Create the operator user
  user:
    name: operator
    createhome: yes
    groups: wheel
    shell: /bin/sh
```

同样的，你可以将你的脚本封装到不同的库中，Ansible 包含了角色的概念。角色描述了机器的特定状态，以及可能的变量、配置文件或模板。它们被恰当地命名，例如，机器可以使用像`docker`、`nginx`和`python`这样的角色。然后，每一个都可以单独测试，并在所有项目中重用。它们还可以封装更抽象的概念，比如 OpenStack 的[可变强化](https://github.com/openstack/ansible-hardening)角色，这使得目标更难攻破。

另一件很酷的事是什么？要运行 Ansible，您只需要在目标机器上安装 Python 2.6+和一个开放的 SSH 连接。没必要再装别的了！这意味着你可能已经准备好马上开始使用 Ansible 了！通过运行 brew install ansible 或 pip install -user ansible 并跟随我来准备您的控制机器。

### 作为代码部署

**忘记位于项目根目录的自述文件。**它通常包含关于如何部署到试运行或生产的所有繁琐细节。对于 Ansible，文档在代码中。它是可测试的，可重用的，任何人都可以运行它，只要这个人能够访问目标机器。它还为持续部署(CD)管道提供了完美的基础。

确保您的应用程序始终可部署也有助于避免灾难。您只需编辑清单文件并对一组新的机器进行全新的部署，而不是弄清楚当您的服务器停机时该做什么。

### 确保您的团队为开发运维做好了准备

当部署是代码库的一部分时，它必须与代码库一起发展。确保你团队的所有成员都支持 DevOps，并且了解如何使用 Ansible。最好的方法是提供一个流动的环境，这样每个开发人员都可以在本地测试部署过程。

用测试应用程序的方式测试部署代码是非常重要的。可能与部署相关的应用程序的每个更改都需要反映在 Ansible 文件的更改中。例如，如果您添加必须复制到服务器的新文件，请确保有相应的任务。如果一个应用程序开始在一个特定的目录中登录，确保 Ansible 创建了那个目录并设置了适当的权限。

### 让它工作很容易，让它可维护却很难

Ansible 也有它的缺点。这些不一定与工具本身相关，但它们确实偶尔会出现。尽管 Ansible 有记录良好的最佳实践，但这些并不总能帮助你实现一个目标。当简单的解决方案已经足够时，这会导致复杂解决方案的产生。

### 首选一次性部署

尽管 Ansible advertises 的特性之一是[幂等性](https://stackoverflow.com/questions/1077412/what-is-an-idempotent-operation)，但是仍然很容易写出一个不能正确工作的剧本。例如，如何以幂等的方式更新服务？你不能，这是自相矛盾的，意味着你必须牺牲一件事来拯救另一件事(也就是一个更新)。

有两个概念可以帮助解决这个问题:[一次性基础设施](http://www.conigliaro.org/disposable-not-immutable-infrastructure/)和[不可变基础设施](https://www.oreilly.com/ideas/an-introduction-to-immutable-infrastructure)。从部署的角度来看，这两者非常相似。前者假设在成功部署后的任何时间点都可以轻松处理机器。它可能会在未来重新配置，但没有什么可以阻止你在任何时候把它拿下来。后者还要求机器在被供应后永远不要改变其配置。

两者都假设您的应用程序位于负载平衡器(或反向代理)之外。这种负载平衡器可以托管在外部或内部，独立于基础架构的其余部分。组成应用程序的服务在负载平衡器中注册。后端配置随着服务的变化而动态更新。如果你想自己托管负载平衡器， [confd](https://github.com/kelseyhightower/confd) 或 [consul-template](https://github.com/hashicorp/consul-template) 可以帮助动态重新配置。

### 使用和重用角色

像何时以及如何使用角色，或者哪些方面应该是可配置的这样的问题没有简单明了的答案。根据我的经验，最好考虑特定机器的各种用例，然后将这些用例封装在单独的角色中，这些角色不仅可以单独测试，还可以在其他项目中重用。由于更大的测试基数，这种重用还会带来更好的代码质量。

### 一系列的可能性

Ansible Galaxy 提供了许多我们可以使用的模块。这类似于 Docker Hub 或 NPM，在那里您可以搜索相关的部分并在您的项目中使用它们。它们都是遵循一个标准编写的，这意味着它们应该易于重用。不幸的是，情况并非总是如此。

在我的职业生涯中，我不止一次遇到一个宣称与 Debian 兼容的模块，但它只在 Ubuntu 上测试过。如果您可以选择自己的基本操作系统，这可能不是问题。如果您想利用现有的基础设施，这可能意味着在移植它时要做一些额外的工作。

版本锁定是另一个问题。我们都知道软件在进化，向后兼容性很少被观察到。当模块使用几个依赖项时，用一个精确的版本标签来描述每个依赖项是至关重要的。这样，我们可以避免在最新版本中安装一个与系统的其他部分不再兼容的包的问题。

当谈到固定版本时，不可避免地会提到比特腐烂。未使用的软件会衰减。引用的超链接可能会过时，版本可能会被删除，托管服务可能会停止运行。即使一个模块使用一个固定版本，如果没有根据需要定期测试和更新，它也可能变得不可用，这就引出了我们的下一个话题。

### 测试既困难又耗时

Ansible 通常操作系统服务。虽然可以在容器中测试它的一些特性(比如用 Docker)，但是这种方法通常会失败。Docker 不能测试所有的内核操作或 systemd 调用，因为它们不存在于它的作用域内。要正确测试可翻译剧本，您需要虚拟机。你注意到复数了吗？ **好，因为仅仅在一台虚拟机上测试是不够的。**

最基本的测试设置应该使用一个干净的 VM，运行剧本，检查结果，然后再次运行剧本来检查等幂问题。但这只能给你有限的信息。您仍然不知道剧本是否会在生产中实际部署。目标机器不一定是干净的虚拟机(除非您已经在使用可任意处理的基础设施)。

为了降低这种风险，拥有一个单独的虚拟机作为“长期内存”可能是一个好主意相比之下，这种虚拟机不会在每次测试部署后都被清理，而是允许随着时间的推移而累积变化。

### 摘要

记录代码的最佳方式是在代码本身。考虑这个简单的陈述会让我们得出一个合乎逻辑的结论——最好的部署文档是部署代码。有许多工具可以帮助实现这一点，Ansible 就是其中之一。就我个人而言，我更喜欢厨师 T1 或 T2 木偶 T3，但我还没有尝试过 T4 盐 T5 或 T6 栈 T7。

正如我在职业生涯中遇到的每一个工具一样，它也有它的缺点。预先了解它们应该可以帮助你避免我偶然发现的一些问题。希望从我的经历中学习能为你节省时间，减少你工作中的挫折。

> *如果你喜欢[我创造的](https://medium.com/@doomhammerng)考虑[订阅 Bit 更好。](http://eepurl.com/gcdRVb)这是一个社区时事通讯，推荐书籍、文章、工具，有时还有音乐。*

*最初发表于 2017 年 8 月 10 日[https://www.iamondemand.com](http://www.iamondemand.com/blog/might-need-ansible-not-even-know/)。*