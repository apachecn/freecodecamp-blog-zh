# 饭桶南瓜解释道

> 原文：<https://www.freecodecamp.org/news/git-squash-explained/>

## **什么是 Git 壁球？**

关于他们的拉请求，开发人员经常听到的一件事是“我觉得不错，请压缩并合并”。有趣的是没有像`git squash`这样的命令(除非你给它创建一个[别名](https://guide.freecodecamp.org/git/git-rebase))。

`squash` pull request 通常意味着将该请求中的所有提交压缩成一个(很少压缩成其他数字),以使其更加简洁、易读，并且不污染主分支的历史。为此，开发者需要使用 [Git Rebase](https://guide.freecodecamp.org/git/git-rebase) 命令的 ****交互模式**** 。

当你开发一些新的特性时，你经常会在你的历史中以几次间歇性的提交而告终——毕竟你是在增量开发。这可能只是一些打字错误或最终解决方案的步骤。大多数情况下，在代码的最终公共版本中包含所有这些提交是没有用的，因此将所有提交压缩到一个单一的最终版本中会更有好处。

因此，让我们假设在您想要作为拉请求的一部分合并的分支中有以下提交日志:

```
$ git log --pretty=oneline --abbrev-commit
30374054 Add Jupyter Notebook stub to Data Science Tools
8490f5fc Minor formatting and Punctuation changes
3233cb21 Prototype for Notebook page
```

很明显，我们更希望这里只有一个提交，因为知道我们开始写了什么以及稍后我们在那里修正了哪些打字错误是没有好处的。只有最后的结果才重要。

所以我们所做的是从当前的 ****头**** (提交 ****30374054**** )到提交 ****3233cb21**** 开始一个交互式的重置基会话，目的是将 ****3**** 最新提交合并成一个:

```
$ git rebase -i HEAD~3
```

这将打开一个类似如下的编辑器:

```
pick 3233cb21 Prototype for Notebook page
pick 8490f5fc Minor formatting and Punctuation changes
pick 30374054 Add Jupyter Notebook to Data Science Tools
# Rebase
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

和往常一样，Git 给了我们一个非常好的帮助消息，在那里你可以看到我们正在寻找的`squash`选项。

目前，交互式 rebase 的指令告诉`pick`每个指定的提交 ****和**** 保存相应的提交消息。那就是——不要改变任何东西。但是我们希望最终只有一个提交。

所以你可以在你的编辑器中简单地编辑文本，用`squash`(或者仅仅是`s`)替换`pick`，下一次我们想要删除的提交，保存/退出编辑器。可能是这样的:

```
s 3233cb21 Prototype for Notebook page
s 8490f5fc Minor formatting and Punctuation changes
pick 30374054 Add Jupyter Notebook to Data Science Tools
```

当您在保存此更改后关闭编辑器时，它会立即重新打开，并建议您选择并改写这些提交消息。大概是这样的:

```
# This is a combination of 3 commits.
# The first commit's message is:
Prototype for Notebook page

# This is the 2nd commit message:

Minor formatting and Punctuation changes

# This is the 3rd commit message:

Add Jupyter Notebook to Data Science Tools

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
```

此时，您可以删除所有不希望包含在最终提交版本中的消息。您也可以改写它们，或者从头开始编写提交消息。

请记住，新版本将包括所有不以`#`字符开头的行。再次保存并退出编辑器。

您的终端现在应该显示一条包含`Successfully rebased and updated <branch name>`的成功消息，git 日志应该显示一个简洁的历史记录，只有一次提交。所有中介提交都已完成，我们准备合并了！

### **关于本地和远程提交历史不匹配的警告**

如果您已经在远程存储库中发布了您的分支，那么这个操作有点危险——毕竟您是在修改提交历史。所以最好在做 ****推**** 之前，先在本地分支上做一下压扁操作。

有时，它已经被推了——你到底如何创建一个拉请求呢？在这种情况下，由于本地历史和远程存储库中的分支历史是不同的，所以您必须在完成压缩后使用 ****强制**** 对远程分支进行更改:

```
$ git push origin +my-branch-name
```

尽最大努力确保此时只有您在使用这个远程分支，否则当其他开发人员有历史不匹配时，您会让他们的日子更难过。但是，由于挤压 通常是在去除树枝之前对树枝进行的最后操作，所以这通常不是一个大问题。