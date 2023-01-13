# Git 秘密:你可能不知道的 7 个命令

> 原文：<https://www.freecodecamp.org/news/7-git-commands-you-might-not-know/>

在过去的几年中，Git 已经成为几乎每个开发人员知识堆栈中的默认部分。但是即使 Git 如此出名，也有许多 Git *命令*并不出名。

在这篇短文中，我将向您展示七个小命令，它们可以帮助您提高工作效率并精通 Git。让我们开始吧。

## 找出文件中的变化

保持领先是很困难的——尤其是当许多人在同一个代码库上工作的时候。

为了帮助您准确理解*如何*(以及*何时*和*由谁*更改了一个文件，您可以使用传统的`git log`命令——但是要稍微加点香料:

```
$ git log --since="3 weeks" -p index.html 
```

使用“-p”可以确保您看到实际的变化(而不仅仅是提交的元数据)。“- since”选项可以帮助你锁定最近的时间范围。

## 体面地撤销你的最后一次提交

有时我们认为一堆变更已经准备好提交了——但是在提交之后，我们立即发现我们太快了。

变更可能会丢失，我们可能会进入错误的分支，或者可能会发生许多其他的问题...

唯一可以确定的是:我们想要撤销最后一次提交，并将我们的更改放回我们的工作副本中！

我们可以使用带有一组特殊选项的`git reset`命令:

```
$ git reset --mixed HEAD~1 
```

“- mixed”选项确保被重置的提交中包含的更改不会被丢弃。相反，它们在工作副本中作为本地更改保存。

使用“HEAD~1”符号是指定“最近一次提交之前的提交”的一个很好的快捷方式，这正是我们想要撤销最后一次提交的原因。

请随意阅读[如何撤销最后一次提交](https://www.git-tower.com/learn/git/faq/undo-last-commit?utm_source=freecodecamp&utm_medium=guestpost&utm_campaign=7-little-know-git-commands)中关于这个主题的更多内容。

## 找出文件在另一个分支中的不同之处

对特性分支中的文件进行一些更改后，您可能希望将它与它在另一个分支上的外观进行比较。举个具体的例子，比如说...

*   您目前位于“功能/登录”分支，并且...
*   想了解这里的文件“myFile.txt”是如何...
*   与“开发”分支上的版本不同。

如果您提供以下参数，`git diff`命令可以做到这一点:

```
$ git diff develop -- myFile.txt 
```

然后，您将看到一个漂亮、清晰的差异，显示您的文件与其在另一个分支上的版本有多么不同。

## 使用“切换”而不是“结帐”

`git checkout`命令有许多不同的任务和含义。这就是为什么最近 Git 社区决定发布一个新命令:`git switch`。顾名思义，它是专门为切换分支的任务而制造的:

```
$ git switch develop 
```

如你所见，它的用法非常简单，类似于“git checkout”。但是与“checkout”命令相比，它的巨大优势在于“switch”没有一百万种其他含义和功能。

因为它是 Git 命令家族的一个新成员，所以您应该检查您的 Git 安装是否已经包含了它。

## 在两个分支之间来回切换

在某些情况下，您可能需要在两个分支之间反复切换。您可以简单地使用下面的快捷方式，而不是总是写出完整的分支名称:

```
$ git switch - 
```

使用破折号字符作为其唯一的参数，“git switch”命令将检查出*之前的*活动分支。如前所述，如果您必须在两个分支之间来回走很多次，这将非常方便。

## 使用“git restore”撤销本地更改

直到最近，当你想要撤销本地更改时，你必须使用某种形式的`git checkout`或`git reset`。

但是有了(相对较新的)`git restore`命令，我们现在有了一个专门为此目的而设计的命令。这使它不同于“结帐”和“重置”，因为这些命令有许多其他的用例。

这里有一个使用“git restore”可以做的最重要的事情的快速概述:

```
# Unstaging a file, but leaving its actual changes untouched
$ git restore --staged myFile.txt

# Discarding your local changes in a certain file
$ git restore myFile.txt

# Undoing all of the local changes in the working copy (be careful)
$ git restore . 
```

## 还原文件的历史版本

`git restore`命令提供了另一个非常有用的选项，名为"- source "。在该选项的帮助下，您可以轻松还原特定文件的任何以前版本:

```
$ git restore --source 6bcf266b index.html 
```

您只需提到您想要恢复的版本的修订散列(当然还有文件名)。

如果你需要帮助*找到*正确的版本，你可以使用[的桌面 Git 客户端，比如 Tower](https://www.git-tower.com/?utm_source=freecodecamp&utm_medium=guestpost&utm_campaign=7-little-know-git-commands) :有了像“文件历史”这样的功能，你可以很容易地只检查某个文件中发生的变化，然后选择一个版本进行恢复。

## 发现 Git 的力量

虽然 Git 现在已经非常出名，但是它的大部分功能仍然不为公众所知。确实，您可以通过提交、推送和拉取等少数命令来“生存”。但这就像只在一档驾驶法拉利！

如果您愿意深入一点，您会发现 Git 的一些更强大的特性。这些不仅有可能让你更有效率，而且最终成为更好的开发者！

如果你想了解更多关于 Git 的知识，我建议你看看下面的免费资源:

*   Git 的急救包:通过一系列非常简短的视频，学习如何在 Git 中撤销和恢复错误。
*   Git 备忘单:一个流行的备忘单，总是有最重要的命令在手边。

## 关于作者

Tobias Günther 是 [Tower](https://www.git-tower.com/?utm_source=freecodecamp&utm_medium=guestpost&utm_campaign=7-little-know-git-commands) 的首席执行官，这是一款流行的 Git 桌面客户端，帮助全球超过 100，000 名开发人员使用 Git 提高工作效率。