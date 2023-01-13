# GitHub 搜索技巧——如何在 GitHub 上更有效地搜索问题、回复和其他信息

> 原文：<https://www.freecodecamp.org/news/github-search-tips/>

当我是开源贡献的初学者时，我最大的挑战之一是找到正确的项目/问题来工作。

在很长一段时间里，我依赖互联网上不同作者整理的资源(顺便说一句，这些资源很好)。但我一直想找到一种方法来解决这个问题——一种我可以搜索和跟踪适合我的技能的项目的方法。

让我们在一点上达成共识:与谷歌不同，搜索 GitHub 并不容易。但是作为一名开发人员，您可能每天都要与 GitHub 或 Gitlab 进行交互。

现在的问题不是你用这些版本控制系统做什么，而是你如何使用它们。就像掌握谷歌搜索技能对任何普通互联网用户来说都是必不可少的一样，我相信开发者学习如何有效地搜索 GitHub 也是必不可少的。

在这篇文章中，我们将看看你可以用来正确搜索 GitHub 的不同技术。您将了解如何搜索:

*   问题和拉取请求
*   仓库
*   用户
*   主题

还有更多。让我们开始吧。

## GitHub 搜索查询

为了在网上找到某样东西的详细信息，你需要掌握正确的搜索技巧。GitHub 也没有什么不同——要找到详细的信息，您可以利用常见的过滤、排序和搜索技术来轻松找到特定的问题，并提取给定项目的请求。

即使你在互联网上为不同的项目列出了多种资源，当你想自己搜索时，主要的问题就来了。你如何开始？应该使用哪些关键字来找到正确的结果？

大多数维护者倾向于给他们的项目贴上有问题的标签，这让贡献者更容易找到合适的项目。下面列出的一些技巧可能会在你使用 GitHub 时帮到你。

### 如何在 GitHub 上搜索问题和请求

寻找有助于项目的最常见方法之一是通过搜索问题和相关的减贫战略。这里有一些技巧，你可以用来轻松找到可靠的答案:

1.  **[是:问题是:开放标签:初学者](https://github.com/search?q=is%3Aissue+is%3Aopen+label%3Abeginner&type=issues)**——这个特定的查询将列出所有具有开放且标签为`beginner`的问题的项目。
2.  **[是:问题是:开放标签:简单](https://github.com/search?q=is%3Aissue+is%3Aopen+label%3Aeasy&type=issues)** -这将列出所有标记为`easy`的开放问题。
3.  **[是:问题是:开放标签:仅限首次投稿](https://github.com/search?q=is%3Aissue+is%3Aopen+label%3Afirst-timers-only&type=issues)**——这里列出了所有欢迎首次投稿的开放问题。
4.  **[is:issue is:open label:good-first-bug](https://github.com/search?q=is%3Aissue+is%3Aopen+label%3Agood-first-bug&type=issues)**——这列出了标有`good-first-bug`的开放问题的项目，以吸引贡献者参与其中。
5.  **[是:问题是:开放标签:“好的第一期”](https://github.com/search?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22&type=issues)**——这里会列出所有带标签`good first issue`的开放问题，意思是适合新手入门的地方。
6.  **[是:问题是:开放标签:启动者](https://github.com/search?q=is%3Aissue+is%3Aopen+label%3Astarter&type=issues)**——这列出了 GitHub 中所有被标记为`starte` r 的开放问题
7.  **[是:问题是:公开标签:待处理](https://github.com/search?q=is%3Aissue+is%3Aopen+label%3Aup-for-grabs&type=issues)** -如果你有必要的技能，这列出了准备处理的公开问题。
8.  **[否:项目类型:问题是:未决](https://github.com/search?q=no%3Aproject+type%3Aissue+is%3Aopen&type=issues)** -这将列出未分配给特定项目的所有未决问题。
9.  **[否:里程碑类型:问题是:打开](https://github.com/search?q=no%3Amilestone+type%3Aissue+is%3Aopen&type=issues)**——很多时候，项目都是用里程碑来跟踪的。但是如果您想查找未被跟踪的问题，这个搜索查询将为您列出那些项目。
10.  **[否:标签类型:问题是:打开](https://github.com/search?q=no%3Alabel+type%3Aissue+is%3Aopen&type=issues)** -这列出了所有未标记的打开问题。
11.  **[是:问题是:未决编号:受分配人](https://github.com/search?q=is%3Aissue+is%3Aopen+no%3Aassignee&type=issues)** -显示所有尚未分配给人员的未决问题。

### 如何搜索存储库

默认情况下，要进行搜索，您将在搜索栏中键入存储库名称，瞧！你会得到一些搜索结果。

但你找到你想要的回购协议的几率非常低。

让我们看看一些可以缩小搜索范围的方法:

#### 如何通过名称、描述/自述文件查找

当您根据自述文件的名称和描述进行搜索时，需要注意的一点是，您的搜索短语应该以`in`限定符开始。这使得在“内部”寻找你想要的东西成为可能。

**例子**

*   使用`in:name`。假设您正在寻找资源来学习更多关于数据科学的知识。在这种情况下，您可以使用命令`Data Science in:name`,它将列出存储库名称中包含 Data Science 的存储库。

*   使用`in:description`。如果您想要查找带有 ceratin 描述的存储库，例如描述中包含术语“freeCodeCamp”的存储库，我们的搜索将是:`freecodecamp in:description`

*   使用`in:readme`。您可以使用它在文件的自述文件中搜索某个短语。如果我们想找到 README 中包含 freecodecamp 一词的存储库，我们的搜索将是:`freecodecamp in:readme`。

*   使用`in:topic`。您可以使用它来查找某个短语或单词是否在主题中被标记。例如，要查找主题中列出 freecodecamp 的所有存储库，我们的搜索将是:`freecodecamp in:topic`

您还可以组合多个搜索查询来进一步缩小搜索范围。

#### 如何通过星星、叉子找到

您还可以根据项目有多少星和叉来搜索存储库。这让你更容易知道这个项目有多受欢迎。

**例题**

*   使用`stars:n`。如果您搜索一个包含 1000 颗星星的存储库，那么您的搜索查询将是`stars:1000`。这将列出包含 1000 颗恒星的存储库。

*   使用`forks:n`。这指定了存储库应该拥有的分支数量。如果您想要查找少于 100 个分支的存储库，您的搜索将是:`forks:<100`。

好的一面是，你可以随时使用关系运算符，比如`<`、`>`、`<=`、`>=`、&、`..`，来帮助你进一步缩小搜索范围。

#### 如何通过语言找到

通过 GitHub 搜索的另一个很酷的方法是通过语言。这有助于您筛选出特定语言的存储库。

**举例:**

*   使用`language:LANGUAGE`。例如，如果您想查找用 PHP 编写的存储库，您的搜索将是:`language:PHP`

#### 如何按组织名称查找

您还可以搜索由特定组织维护或创建的存储库/项目。为此，您需要以关键字`org:...`开始搜索，后跟组织名称。

例如，如果你搜索`org:freecodecamp`，它会列出匹配 freeCodeCamp 的库。

#### 如何按日期查找

如果你想要基于特定日期的搜索结果，你可以使用这些关键字之一进行搜索:`created`、`updated`、`merged`和`closed`。这些关键字应附有日期，格式为`YYYY-MM-DD`。

**举例:**

*   使用`keyword:YYYY-MM-DD`。举个例子，我们想要搜索 2022-10-01 之后创建的带有单词 freeCodeCamp 的所有存储库。那么我们的搜索将是:`freecodecamp created:>2022-10-01`

您还可以使用`<`、`>`、`>=`和`<=`来搜索指定日期之后、之前和当天的日期。要在一定范围内搜索，您可以使用`...`。

#### 如何通过许可证查找

当你在寻找一个可以参与的项目时，许可证是非常重要的。不同的许可证给予贡献者不同的权利去做什么和不做什么。

为了使您更容易找到具有正确许可证的项目，您需要对许可证有很好的理解。你可以在这里阅读更多关于他们的信息。

**举例:**

*   使用`license:LICENSE_KEYWORD`。这是搜索具有特定许可的项目的好方法。例如，要搜索拥有 MIT 许可证的项目，您可以使用`license:MIT`。

#### 如何通过可见度找到

您还可以根据存储库的可见性进行搜索。在这种情况下，您可以使用 public 或 private。这将分别匹配公共或私有存储库中的问题和 PRs。

**例子:**

*   使用`is:public`。这将显示公共存储库的列表。让我们举一个例子，我们想要搜索 freeCodCamp 拥有的所有公共存储库。那么我们的搜索会是:`is:public org:freecodecamp`。
*   使用`is:private`。该查询旨在列出给定搜索查询下的所有私有存储库。

## 结论

尽管我们在这里已经讨论了许多搜索查询，但是您总是可以通过组合多个参数来进一步缩小搜索范围。

如需更多资源和更多搜索参数，您可以随时参考 [GitHub 文档](https://docs.github.com/en/search-github/searching-on-github)或使用[高级 GitHub 搜索](https://github.com/search/advanced?)。这些方法总会派上用场，因为它们提供了更多的文件选择。

有大量的搜索参数可以让你在 GitHub 上的日常活动变得更容易。希望这将帮助你更容易和有效地使用这个平台。