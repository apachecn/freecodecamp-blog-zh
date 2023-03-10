# 安全存储 API 密钥的最佳实践

> 原文：<https://www.freecodecamp.org/news/how-to-securely-store-api-keys-4ff3ea19ebda/>

布鲁诺·佩德罗

# 安全存储 API 密钥的最佳实践

![CBhWsbKbnXyzZ31h71RlfC-H6TcZ9IdF6dej](img/e72ced14d8d94c8b4d3805a3177df981.png)

Picture by [Jose Fontano](https://unsplash.com/@josenothose)

在过去，我见过许多人使用 Git 存储库来存储与他们的项目相关的敏感信息。

最近，我看到一些人宣布他们在自己的私有 GitHub 库上存储 API 密钥。我写这篇文章是因为人们应该理解在你的代码中存储 API 键的风险。

本文并不打算成为您在存储 API 键时可能遇到的问题的永久解决方案。取而代之的是我自己对问题的分析和我对如何修复的建议。

那么，在 Git 存储库中将敏感信息存储在代码附近到底有什么问题呢？

### 为什么不应该在 Git 存储库中存储 API 键

在一个`git`存储库中存储 API 密钥或任何其他敏感信息是要不惜一切代价避免的。即使存储库是私有的，您也不应该将其视为存储敏感信息的安全地方。

让我们先来看看为什么把 API 密匙存储在**公共** `git`仓库中不是一个好主意。

本质上，任何人都可以访问公共的`git`存储库。

换句话说，任何有互联网连接的人都可以访问公共`git`库的内容。不仅如此，他们还可以浏览存储库中的所有代码，甚至可能运行它。如果你把一个 API 密匙存储在一个公共存储库中，你就是在公开发布，这样任何人都可以看到它。

最近在 GitHub 上搜索 **client_secret** 发现有超过 30，000 个提交可能会暴露 API 密钥和秘密。在某些情况下，您只需复制并粘贴代码就可以立即访问 API。

这个问题变得如此重要，以至于一些公司投资资源来确保没有任何泄露的 API 密钥和秘密。

去年，Slack 开始搜索暴露的 API 令牌，并主动使其无效。此操作防止恶意访问 Slack 的帐户，但不可能找到所有泄漏的令牌。

所以，这发生在公共 Git 仓库上。那些**私人**的呢？为什么这是一个问题？

托管在 GitHub、GitLab 和 Bitbucket 等服务上的私有 Git 存储库面临着不同类型的风险。当您将第三方应用程序与提到的服务之一集成时，您可能会向这些第三方开放您的私有存储库。这些应用程序将能够访问您的私有存储库并读取其中包含的信息。

虽然这本身不会产生风险，但是想象一下如果这些应用程序中的一个变得容易受到攻击者的攻击。通过对这些第三方应用程序之一进行未经授权的访问，攻击者可能会访问您的敏感数据，包括 API 密钥和机密。

### 那么，API 密钥应该存储在哪里呢？

有许多安全存储 API 密钥和秘密的替代方法。其中一些允许您使用 Git 存储库并加密敏感数据。其他工具更加复杂，并且作为部署工作流的一部分解密敏感信息。让我们看看一些可用的解决方案。

#### `git-remote-gcrypt`

第一种解决方案让您加密整个 Git 存储库。git-remote-gcrypt 通过向 git 远程助手添加功能来实现这一点，这样就有了一个新的加密传输层。用户只需设置一个新的加密遥控器，并将代码输入其中。

如果您正在寻找一个更细粒度的解决方案，让您加密单个文件，请继续阅读。

#### `git-secret`

`[git-secret](http://git-secret.io/)`是一款在本地机器上运行的工具，在您将特定文件推送到存储库之前对其进行加密。在幕后，`git-secret`是一个 shell 脚本，它使用 GNU Privacy Guard ( [GPG](https://www.gnupg.org/) )来加密和解密可能包含敏感信息的文件。

#### `git-crypt`

另一种解决方法是`[git-crypt](https://www.agwa.name/projects/git-crypt/)`。它的操作方式与 git-secret 非常相似，但是有一些有趣的区别。

关于`git-crypt`首先要注意的是，它是一个二进制可执行文件，而不是像 git-secret 那样的 shell 脚本。作为一个二进制可执行文件意味着你首先要编译它，或者你需要为你的机器找到一个二进制发行版。

如果你用的是 Mac，你很幸运，因为[家酿](https://brew.sh/)提供了一个`git-crypt`现成的安装包。你所要做的就是在终端上运行 brew install `git-crypt`。

#### 暗箱

[黑盒](https://github.com/StackExchange/blackbox)是由[栈溢出](https://stackoverflow.com/)创建的工具。这是流行 Q & A 社区如栈溢出本身、服务器故障、超级用户背后的公司。BlackBox 是一个健壮的工具，因为它可以与 Git 以及 Mercurial 和 Subversion 等其他版本控制系统一起工作。

它还支持加密小字符串，而不仅仅是整个文件。它在使用 [Puppet](https://puppet.com/) 时会这样做，并使用 Puppet 的 [Hiera](https://docs.puppet.com/hiera/) ，这是一个用于配置数据的键值查找工具。

BlackBox 具有加密和解密单个字符串的能力，这使得它成为保护 API 密钥和秘密的一个很好的解决方案。

#### Heroku 配置和配置变量

如果你和 Heroku 一起工作，你不应该在你的 Git 仓库中存储任何敏感信息，比如 API 密匙和秘密。Heroku 提供了一个解决方案，让你[设置配置变量](https://devcenter.heroku.com/articles/config-vars)。

然后，通过访问相应的环境变量，您的应用程序可以在运行时访问这些配置变量的内容。即使这些值没有加密，这个解决方案也可以让您避免使用 Git 存储库来存储 API 密钥。

像 Heroku 这样的开源解决方案 Dokku 也提供了同样的功能。

#### 码头工人的秘密

最有可能的解决方案是[码头工人的秘密](https://docs.docker.com/engine/swarm/secrets/)。这个解决方案[是 Docker](https://blog.docker.com/2017/02/docker-secrets-management/) 在 2017 年 2 月推出的。从那以后，它开始流行起来。

Docker secrets 允许您定义加密的变量，并在运行时将它们提供给特定的服务。秘密在传输过程中和静止时都被加密。

这种方法使得 Docker secrets 成为以安全和加密的方式存储和使用 API 密钥和秘密的完美解决方案。

### 摘要

到目前为止，您应该已经意识到了在公共和私有 Git 存储库中存储敏感信息(如 API 密钥和秘密)的危险性。

理解您的存储库可能暴露的潜在方式是评估和减轻与信息泄漏相关的风险的关键。

本文还提出了几个不同的解决方案，让您可以加密 API 密钥和秘密，以便您可以安全地使用您的代码库。

我相信有更多的解决方案可以帮助你达到同样的效果。