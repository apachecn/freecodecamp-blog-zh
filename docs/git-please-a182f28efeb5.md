# Git Please

> 原文：<https://www.freecodecamp.org/news/git-please-a182f28efeb5/>

巴迪·雷诺

# 如何在不急不躁的情况下用力

![1*PhSXjkYS9MTmoNwlPIdQpQ](img/bcd46b534af67b058378f6490bf31732.png)

Don’t be like Anakin.

随着开发团队规模的增长，有人进行强制推送并覆盖他人代码的可能性也在增加。

Git 中的强制推送是这样的:

```
$ git push --force origin master# `--force` can also  be written as `-f`
```

这个命令会导致各种问题。它基本上告诉 Git*我不关心 origin/master 中有什么。我所拥有的是正确的。覆盖它。*

那么，如果一个同事将变更提交到了一个分支，而您没有将它放入自己的回购中，会发生什么呢？它会被覆盖，您的同事可能不得不重做他们的工作(或者如果他们在本地还有提交的话，重新提交一两次)。

但是只要稍微改变一下如何使用`force`标志，就可以很容易地避免这种混乱。不要用`—-force`，用`--force-with-lease`。

```
$ git push --force-with-lease origin master
```

总结一下 git 的[文档](https://git-scm.com/docs/git-push#git-push---force-with-leaseltrefnamegt)，使用`force-with-lease`告诉 Git 检查远程 repo 是否与您试图推送的 repo 相同。如果不是，git 将抛出一个错误，而不是盲目地覆盖远程 repo。这将避免您意外覆盖您不打算覆盖的工作。

但是我讨厌输入`force-with-lease`——特别是因为我习惯于输入简写的`-f`来表示强迫。幸运的是，Git 允许您添加别名来加快速度。我喜欢认为我在问 Git 是否可以强制推送，所以我将`push —force-with-lease`化名为`git please`。

```
$ git please origin master
```

您可以在 git 中添加一个别名，方法是在终端中键入以下内容:

```
git config --global alias.please 'push --force-with-lease'
```

或者您可以打开您的`~/.gitconfig`文件并手动添加别名:

```
[alias]	co = checkout	ci = commit	please = push --force-with-lease
```

#### 总是有一个警告…

然而，用租借来欺骗武力是可能的。当您使用`git pull`从原点获取更新时，这是同时执行两个命令。Git 运行一个`fetch`来下拉所有变更的引用。然后，它运行一个`merge`，将您刚刚获取的更改合并到您当前的分支中。

如果你只是做了一个`fetch`来获得最新的更新，那么你只是更新了你的引用——而不是真正地将变化合并到你的工作副本中。然后，如果使用 lease 强制 push，Git 将查看这些引用，并认为本地副本与远程副本是最新的，而实际上还不是。这将欺骗 Git 用您的本地副本覆盖远程上的更改，而没有实际合并更改。

避免这个问题的最简单的方法是总是同时使用`git pull`到`fetch`和`merge`。我还没有遇到过需要手动操作`fetch`和`merge`的情况，所以我不能对那些情况发表意见。使用`pull`对我来说一直有效。

我希望你会发现`git please`很有帮助，因此，你永远也不需要从强制推行的噩梦中恢复过来。