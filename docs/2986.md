# Git 拉力——如何用 Git 覆盖本地更改

> 原文：<https://www.freecodecamp.org/news/git-pull-force-how-to-overwrite-local-changes-with-git/>

当你学习编码时，迟早你也会学到版本控制系统。虽然在这个领域有许多相互竞争的工具，但其中之一是业内几乎每个人都使用的事实上的标准。它如此受欢迎，以至于有些公司在他们的品牌中使用它的名字。当然，我们说的是 Git。

虽然 Git 是一个强大的工具，但它的威力却隐藏得很好。要真正精通 Git，您需要理解一些基本概念。好消息是，一旦你学会了它们，你就几乎不会遇到你无法逃脱的麻烦。

# 典型的工作流程

在典型的 Git 工作流中，您将使用一个本地存储库、一个远程存储库以及一个或多个分支。存储库存储了关于项目的所有信息，包括它的整个历史和所有分支。一个分支基本上是从一个空项目到当前状态的变更的集合。

克隆存储库后，您可以处理您的本地副本并引入新的更改。在您将本地更改推送到远程存储库之前，您的所有工作只能在您的机器上使用。

当您完成一项任务时，就该与远程存储库同步了。您希望拉动远程更改以跟上项目的进度，并且您希望推动本地更改以与其他人共享您的工作。

# 局部变化

当你和你团队的其他成员处理完全不同的文件时，一切都很好。无论发生什么，你们都不会踩到对方的脚。

然而，有时候你和你的队友同时在同一个地方引入变化。这通常是问题开始的地方。

你有没有执行过`git pull`只为了看可怕的`error: Your local changes to the following files would be overwritten by merge:`？每个人迟早都会遇到那个问题。

这里比较混乱的是，你什么都不想合并，就拉吧？实际上，拉远比你想象的要复杂一些。

# Git Pull 到底是怎么工作的？

拉不是单一的操作。它包括从远程服务器获取数据，然后将更改与本地存储库合并。如果需要，可以手动执行这两个操作:

```
git fetch
git merge origin/$CURRENT_BRANCH
```

`origin/$CURRENT_BRANCH`部分的意思是:

*   Git 将合并来自名为`origin`的远程存储库(您克隆的那个)的更改
*   已添加到`$CURRENT_BRANCH`
*   本地签出分支中不存在的

因为 Git 只在没有未提交的更改时执行合并，所以每次运行`git pull`时都有未提交的更改会给你带来麻烦。还好海贼王是有办法脱困的！

![image-167](img/23691e29e426b11e688cbc6b3dd5f92c.png)

Photo by [Sneaky Elbow](https://unsplash.com/@sneakyelbow?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

# 不同的方法

当您有未提交的本地更改，并且仍然希望从远程服务器获取新版本时，您的用例通常属于以下场景之一。要么:

*   您不关心本地更改并想要覆盖它们，
*   您非常关心这些更改，并且希望在远程更改后应用它们，
*   您想要下载远程修改，但尚未应用它们

每种方法都需要不同的解决方案。

### 你不关心当地的变化

在这种情况下，您只想删除所有未提交的本地更改。也许您修改了一个文件进行实验，但是您不再需要修改。你所关心的是跟上上游。

这意味着您在获取远程更改和合并它们之间增加了一个步骤。这一步会将分支重置为未修改的状态，从而允许`git merge`工作。

```
git fetch
git reset --hard HEAD
git merge origin/$CURRENT_BRANCH
```

如果您不想在每次运行该命令时都键入分支名称，Git 有一个指向上游分支的快捷方式:`@{u}`。上游分支是远程存储库中的分支，您可以向其推送数据或从中获取数据。

这就是上面的命令在使用快捷方式时的样子:

```
git fetch
git reset --hard HEAD
git merge '@{u}'
```

我们在示例中引用了快捷方式，以防止 shell 解释它。

### 你非常关心当地的变化

当您未提交的更改对您很重要时，有两种选择。您可以提交它们，然后执行`git pull`，或者您可以隐藏它们。

储存意味着暂时把变化放在一边，以后再拿出来。更准确地说，`git stash`创建一个在当前分支上不可见的 commit，但是 Git 仍然可以访问它。

要恢复上次保存的更改，您可以使用`git stash pop`命令。成功应用隐藏的更改后，该命令还会删除隐藏提交，因为不再需要它了。

工作流可能如下所示:

```
git fetch
git stash
git merge '@{u}'
git stash pop
```

默认情况下，来自存储库的更改将成为分阶段的。如果您想卸载它们，使用命令`git restore --staged`(如果使用比 2.25.0 新的 Git)。

### 您只想下载远程更改

最后一个场景与前面的略有不同。假设你正处于一个非常混乱的重构过程中。既不能丢失更改，也不能隐藏更改。然而，您仍然希望远程更改可以针对它们运行`git diff`。

您可能已经发现，下载远程更改根本不需要`git pull`！`git fetch`刚好够用。

需要注意的一点是，默认情况下，`git fetch`只会给你带来当前分支的变化。要从所有分支获得所有更改，使用`git fetch --all`。如果您想清理一些远程存储库中不再存在的分支，`git fetch --all --prune`将会进行清理！

![image-166](img/1dbd49063ea4833851bec8993b0b0885.png)

Photo by [Lenin Estrada](https://unsplash.com/@lenin33?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

# 一些自动化

你听说过 Git Config 吗？Git 在这个文件中存储了所有用户配置的设置。它驻留在您的主目录中:作为`~/.gitconfig`或`~/.config/git/config`。您可以编辑它来添加一些自定义别名，这些别名将被理解为 Git 命令。

例如，要有一个等同于`git diff --cached`的快捷方式(显示当前分支和暂存文件之间的差异)，您需要添加以下部分:

```
[alias]
  dc = diff --cached
```

之后，您可以随时运行`git dc`来查看更改。这样，我们可以设置一些与前面的用例相关的别名。

```
[alias]
  pull_force = !"git fetch --all; git reset --hard HEAD; git merge @{u}"
  pf = pull_force
  pull_stash = !"git fetch --all; git stash; git merge @{u}; git stash pop"
```

这样，运行`git pull_force`将覆盖本地更改，而`git pull_stash`将保留它们。

# 另一个 Git 拉力

好奇的头脑可能已经发现了`git pull --force`这种东西的存在。然而，这是一个与本文中呈现的完全不同的东西。

这听起来像是能帮助我们覆盖局部变化的东西。相反，它让我们从一个远程分支获取更改到另一个本地分支。`git pull --force`只修改取数部分的行为。因此相当于`git fetch --force`。

像`git push`，`git fetch`允许我们指定我们想要操作哪个本地和远程分支。`git fetch origin/feature-1:my-feature`将意味着来自远程存储库的`feature-1`分支中的变更将最终在本地分支`my-feature`上可见。当这样的操作修改现有历史时，如果没有显式的`--force`参数，Git 是不允许的。

就像`git push --force`允许覆盖远程分支一样，`git fetch --force`(或`git pull --force`)允许覆盖本地分支。它总是与作为参数提到的源和目标分支一起使用。使用`git --pull force`覆盖本地更改的另一种方法是`git pull --force "@{u}:HEAD"`。

# 结论

Git 的世界是广阔的。本文只讨论了存储库维护的一个方面:将远程更改合并到本地存储库中。甚至这个日常场景也需要我们稍微深入地研究一下这个版本控制工具的内部机制。

学习实际用例有助于您更好地理解 Git 是如何工作的。这反过来会让你在陷入困境时感到有力量。我们都会时不时地这么做。