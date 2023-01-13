# Git 推送到远程分支——如何将本地分支推送到原点

> 原文：<https://www.freecodecamp.org/news/git-push-to-remote-branch-how-to-push-a-local-branch-to-origin/>

将本地分支推送到远程存储库的基本命令是`git push`。

这个命令有各种选项和参数可以传递给它，在本文中，您将了解到最常用的选项和参数。

## 如何将本地 Git 分支推到原点

如果您运行简单的命令`git push`，Git 将默认为您选择另外两个参数:推送的**远程存储库**和推送的**分支**。

该命令的一般形式如下:

```
$ git push <remote> <branch>
```

默认情况下，Git 选择`origin`作为远程节点，选择*当前分支*作为要推送的分支。

如果您当前的分支是`main`，命令`git push`将提供两个默认参数——有效地运行`git push origin main`。

在下面的例子中，`origin` remote 是一个 GitHub 存储库，当前分支是`main`:

```
(main)$ git remote -v 
origin  git@github.com:johnmosesman/burner-repo.git (fetch)
origin  git@github.com:johnmosesman/burner-repo.git (push)

(main)$ git push
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 16 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 274 bytes | 274.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:johnmosesman/burner-repo.git
   b7f661f..ab77dd6  main -> main 
```

从输出中您可以看到本地`main`分支被推送到远程`main`分支:

```
To github.com:johnmosesman/burner-repo.git
   b7f661f..ab77dd6  main -> main 
```

## 如何在 Git 中强制推送分支

通常，您将推送到一个分支并添加到它的提交历史中。

但是，有时候你需要强行**覆盖**一个分支的历史。

有几个原因你可能想这样做。

第一个原因是修复一个错误——尽管可能更好的方法是提交一个新的 commit [来恢复更改。](https://git-scm.com/docs/git-revert)

第二种也是更常见的场景是在类似于 **[重置](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase) —** 的动作之后，这改变了提交历史:

> 在内部，Git 通过创建新的提交并将它们应用于指定的基础来完成[一个基础]。理解这一点非常重要，即使分支看起来一样，它也是由全新的提交组成的。

重设基础创建了全新的提交。

这意味着，如果您尝试推送一个已在本地(而不是在远程)重设基础的分支，远程存储库将识别提交历史记录已更改，并阻止您推送，直到您解决差异:

```
(my-feature)$ git push
To github.com:johnmosesman/burner-repo.git
 ! [rejected]        my-feature -> my-feature (non-fast-forward)
error: failed to push some refs to 'git@github.com:johnmosesman/burner-repo.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details. 
```

您可以在这里执行`git pull`来合并差异，但是如果您*真的*想要覆盖远程存储库，您可以将`--force`标志添加到您的推送中:

```
(my-feature)$ git push --force origin my-feature
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 184 bytes | 184.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To github.com:johnmosesman/burner-repo.git
 + edb64e2...52f54da my-feature -> my-feature (forced update) 
```

(**注:**可以用`-f`代替`--force`作为速记。)

强制推动是一种破坏性的行为——只有在你确定这是你想要做的时候才使用它。

### 使用租约强制推送

有时候你可能想要强制推送——但是只有在没有其他人对分支做出贡献的情况下。

如果其他人对您的分支做出了贡献，并将他们的更改推送到远程，而您强制推送到远程，您将覆盖他们的更改。

为了防止这种情况，您可以使用`--force-with-lease`选项。

文档中的[:](https://git-scm.com/docs/git-push)

> - force-with-lease 单独使用，不指定细节，通过要求它们的当前值与我们为它们拥有的远程跟踪分支相同，将保护所有将要更新的远程引用。

基本上，你是在告诉 Git，只有当看起来和你最后一次看到它时一样，才强制更新这个分支*。*

如果您在您的分支上与其他人合作，最好避免使用`--force`或者至少使用`--force-with-lease`来防止丢失其他合作者所做的更改。

## 如何在 Git 上推送不同名称的分支

您通常会将本地分支推到同名的远程分支，但并不总是这样。

要推送不同名称的分支，只需要指定*你要推送的分支*和你要*到*推送的分支名称，中间用冒号(`:`)隔开。

例如，如果您想将一个名为`some-branch`的分支推送到`my-feature`:

```
(some-branch)$ git push origin some-branch:my-feature
Total 0 (delta 0), reused 0 (delta 0)
To github.com:johnmosesman/burner-repo.git
 + 728f0df...8bf04ea some-branch -> my-feature
```

### 如何将所有本地分支推送到远程

您不需要经常从本地推送所有分支，但是如果您需要，您可以添加`--all`标志:

```
(main)$ git branch
* main
  my-feature

(main)$ git push --all
...
To github.com:johnmosesman/burner-repo.git
   b7f661f..6e36148  main -> main
 * [new branch]      my-feature -> my-feature 
```

## 结论

`git push`命令是您将经常使用的一个命令，并且有大量的选项可以与它一起使用。我鼓励你[阅读文档](https://git-scm.com/docs/git-push)，寻找有用的选项和快捷方式。

如果你喜欢这个教程，我也会在 Twitter 上谈论像这样的话题[，并在我的网站上写下关于它们的话题](https://twitter.com/johnmosesman)[。](https://johnmosesman.com/)