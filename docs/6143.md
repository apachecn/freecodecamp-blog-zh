# Git 如何出错——以及应该怎么做。

> 原文：<https://www.freecodecamp.org/news/how-you-can-go-wrong-with-git-and-what-to-do-instead-d80eeeff1d95/>

我无法提交到远程存储库，让我执行强制推送。

让我在远程存储库上运行 rebase，使提交历史更加整洁。

让我修改一下远程存储库中的以前的提交。

上面提到的几点是在 Git 中应该避免做的事情。

在我之前的文章中，我介绍了 [Git 基础知识](https://medium.freecodecamp.org/what-is-git-and-how-to-use-it-c341b049ae61)和 [Git 修正和重置](https://medium.freecodecamp.org/how-to-become-a-git-expert-e7c38bf54826)。点击链接了解更多信息。

Git 有惊人的特性，对开发者很有帮助。但是在使用 Git 的时候还是会出错。在这里，我将提到一些在使用 Git 时应该避免的**事情，并且**将解释为什么**你应该避免它们。**

### 强制推送到远程存储库

![vxvEY0Py6Tt3Jsfqcw3H5BSMdGC2qpuA2TqD](img/98a43fa543b4f106a8041f188d32eb25.png)

Photo by [Tim Mossholder](https://unsplash.com/@timmossholder?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

假设两个开发人员在一个分支上工作。开发人员 2 是 Git 的初学者。

1.  开发人员 1 已经完成了他们的更改，并将代码推送到远程存储库。
2.  现在 Developer 2 已经完成了他们的变更，但是无法将代码推送到远程存储库。
3.  开发人员 2 进行了快速的 google 搜索，发现了 force push 命令并使用了它。命令是`git push -f`
4.  开发人员 1 检查远程存储库，却发现他们编写的代码已经完全消失了。

这是因为强制推送会覆盖远程存储库中的代码，因此远程存储库中的现有代码会丢失。

#### 处理这种情况的理想方式

开发人员 2 需要从远程存储库中提取最新的代码变更，并将代码变更重新存储到本地存储库中。一旦成功地完成了重定基础，开发人员 2 就可以将代码推送到远程存储库中。**这里我们讨论的是在同一个分行中从远程回购到本地回购的转基。**

**除非绝对必要，否则避免用力推动**。只有在别无选择的情况下才使用它。但是请记住，强制推送将覆盖远程存储库中的代码。

事实上，如果您使用的是像源代码树这样的用户界面，默认情况下强制推送是禁用的。您必须手动启用它才能使用它。

同样，如果使用正确的 [Git 工作流](https://medium.freecodecamp.org/how-to-use-git-efficiently-54320a236369)，每个开发人员将拥有他们自己的特性分支，这样的场景甚至不会发生。

### 尝试重新设置远程存储库的基础

![YYrwWwrW0Le9w89HoybfYPLmJ5JpmwwzABOG](img/ae953b70627925ef11105bacc336b80f.png)

“green, red, and white high voltage circuit breaker” by [Ben Hershey](https://unsplash.com/@benhershey?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

假设两个开发人员正在开发一个特性分支。

1.  开发人员 1 完成了一堆提交，并将其推送到远程特性分支。
2.  开发人员 2 将最新的变更从远程特性分支带入本地特性分支。
3.  开发人员 2 向本地特性分支添加了一堆提交。
4.  但是 Developer 2 也希望确保来自发布分支的最新变更被重新放入特性库中。因此，开发人员 2 将发布分支重新置于本地特性分支之上。**这是不同分行从远程到本地回购完成的转基**。
5.  现在，开发人员 2 试图将代码推送到远程特性分支。Git 不允许这样做，因为提交历史已经改变。所以开发者 2 会进行强制推送。
6.  现在，当开发人员 1 想要从远程特性分支获取最新的代码时，这是一项艰巨的工作，因为提交历史已经改变。因此，开发人员 1 需要处理大量的代码冲突——甚至是冗余的代码冲突，这些冲突已经被开发人员 2 处理了。

对远程存储库进行重新设置将会改变提交历史，并且当其他开发人员试图从远程存储库获取最新代码时，将会产生问题。

#### 处理这种情况的理想方式

处理这种情况的理想方式是总是只对本地存储库进行重新设置。本地存储库中的任何提交都不应被推送到远程存储库中。

如果任何提交已经被推送到远程特性分支，那么最好与发布分支进行合并，而不是重定基础，因为合并不会改变提交历史。

此外，如果使用正确的 Git 工作流，只有一个人会在一个特性分支上工作，这个问题甚至不会发生。

如果只有一个人在特性分支上工作，并且在远程特性分支上完成了 rebase，那么就没有问题了——没有其他开发人员从同一个远程特性分支中提取代码。但是最好避免重定远程存储库的基础。

Rebase 是一个非常强大的功能，但是要小心使用。

### 修改远程存储库中的提交

![gq1Wo6a6yVlzmaudBN6xO6Vs65fZ7EkiYukn](img/726f08d18d9fc6a57de7c9f4ad5a3c37.png)

“broken ceramic plate on floor” by [chuttersnap](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

假设两个开发人员正在开发一个特性分支。

1.  开发人员 1 完成了一个提交，并将其推送到远程功能分支。让我们称之为“旧提交”
2.  开发人员 2 将最新的代码从远程特性分支拉入本地特性分支
3.  开发人员 2 正在处理本地存储库中的代码，还没有将任何代码推送到远程存储库中。
4.  开发人员 1 意识到提交中存在错误，并在本地回购中修改提交。让我们称之为“提交新的”
5.  开发人员 1 试图将修改后的提交推送到远程功能分支。但是 Git 不允许这样做，因为提交历史发生了变化。所以开发者 1 做了一个强制推送。
6.  现在，当 Developer 2 想要从远程特性分支获取最新的代码时，Git 会注意到提交历史中的差异，并创建一个合并提交。当开发人员 2 检查本地回购中的提交历史时，开发人员 2 将注意到“提交新的”和“提交旧的”。这破坏了修改提交的全部意义。
7.  即使开发人员 2 从远程分支到本地分支进行了 rebase，“commit old”仍然会出现在开发人员 2 的本地分支中。因此它仍然是提交历史的一部分。

**修改提交会改变提交历史。因此，当其他开发人员试图从远程存储库中获取最新代码时，修改远程存储库中的提交将会造成混乱**

#### 处理这种情况的理想方式

最佳实践是只在本地存储库中修改提交。一旦提交到远程存储库中，最好不要做任何修改。

此外，如果使用正确的 Git 工作流，只有一个人会处理一个特性分支，这个问题甚至不会发生。在这种情况下，修改远程存储库不会产生任何问题，因为没有其他开发人员从同一个远程特性分支提取代码。

### 硬重置

![6JofEPRTTXtI2CQJvVYxKtSnV3NtdyL9TbBZ](img/8c6e2ad538c9bb7b07641b1be606513e.png)

“clear hour glass beside pink flowers” by [Nathan Dumlao](https://unsplash.com/@nate_dumlao?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

1.  使用`git reset --hard`完成硬复位
2.  还有其他类型的复位，如`--soft`和`--mixed`，它们没有硬复位危险

假设开发人员 1 正在处理一个特性分支，并在本地回购中完成了五次提交。

1.  此外，开发人员 1 目前正在处理两个尚未提交的文件。
2.  如果开发者 1 运行`git reset --hard <commit4hash>`，将会发生以下事情。
3.  功能分支中的最新提交现在将是提交 4，提交 5 将丢失。
4.  开发人员 1 正在处理的两个未提交的文件也丢失了

Commit5 仍然在 Git 内部，但是对它的引用丢失了。我们可以使用`git reflog`取回 commit5。但是，话虽如此，使用硬复位仍然非常危险。

在 Git 中使用重置命令时要非常小心。在某些情况下，您可能必须使用重置，但是在进行硬重置之前，请对情况进行全面评估。

### 如何知道使用 git 时的不良做法

![sa54At5X8hjGOHKzP2owlBAfmMuhkjIdpzOy](img/240993ab7289a66c985c0f5269e1f0b5.png)

“question mark neon signage” by [Emily Morter](https://unsplash.com/@emilymorter?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我上面提到的列表并没有涵盖所有内容。它只是列出了一些在使用 Git 时可能出错的地方。

那么，一般来说，您如何知道在使用 Git 时应该避免什么呢？

1.  在上面的列表中，您可能已经观察到了一件常见的事情，那就是当多个人在同一个分支工作时，问题就会发生。因此，使用正确的 Git 工作流将确保一次只有一个人在一个特性分支上工作。发布分支将由技术主管或高级开发人员处理。此工作流程可以防止一些重大问题的发生。
2.  你会观察到的另一个常见现象是到处使用力推。默认情况下，Git 确保您不能在远程存储库中进行任何破坏性的更改。但是强制推送覆盖了 Git 的默认行为。
3.  所以，当你处于可能需要用力的位置时，只把它作为最后的手段。也要评估是否有其他不使用强迫的方法来达到你的目的。
4.  任何改变远程存储库中提交历史的操作都是危险的。仅在本地存储库中更改提交历史记录。但是即使在本地存储库中，在使用硬重置时也要小心。
5.  在非常小的项目中，使用 Git 工作流可能是多余的。在这些项目中，多个开发人员将在同一个分支上工作。但是，在对远程存储库进行任何重大更改之前，最好先评估一下这是否会影响其他开发人员。

希望这篇文章给出了一些关于 Git 中可能出错的地方以及如何避免出错的想法。？

### 关于作者

我热爱技术，关注该领域的进步。我也喜欢用我的技术知识帮助别人。

请随时通过我的 LinkedIn 账户与我联系[https://www.linkedin.com/in/aditya1811/](https://www.linkedin.com/in/aditya1811/)

你也可以在推特上关注我[https://twitter.com/adityasridhar18](https://twitter.com/adityasridhar18)

我的网站:[https://adityasridhar.com/](https://adityasridhar.com/)

### 我的其他帖子

[Git 简介](https://medium.freecodecamp.org/what-is-git-and-how-to-use-it-c341b049ae61)

[如何高效使用 Git](https://medium.freecodecamp.org/how-to-use-git-efficiently-54320a236369)

[如何成为 git 专家](https://medium.freecodecamp.org/how-to-become-a-git-expert-e7c38bf54826)

最初发表于[adityasridhar.com](https://adityasridhar.com/posts/how-you-can-go-wrong-with-git)