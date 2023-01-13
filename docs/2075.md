# Git 切换分支——如何在 Git 中改变分支

> 原文：<https://www.freecodecamp.org/news/git-switch-branch/>

在 Git 中，切换分支是您需要经常做的事情。

为此，您可以使用`git checkout`命令。

## 如何在 Git 中创建新的分支

要在 Git 中创建新的分支，可以使用`git checkout`命令并传递带有名称的`-b`标志。

这将从当前分支创建一个新的分支。新分支的历史将从您“分支”的分支的当前位置开始

假设您目前在一个名为`master`的分支上:

```
(master)$ git checkout -b my-feature
Switched to a new branch 'my-feature'
(my-feature)$
```

这里你可以看到一个名为`my-feature`的新分支，它是从`master`分支出来的。

## 如何切换到 Git 中现有的分支

要切换到一个现有的分支，您可以再次使用`git checkout`(没有`-b`标志)并传递您想要切换到的分支的名称:

```
(my-feature)$ git checkout master
Switched to branch 'master'
(master)$ 
```

通过将`-`传递给`git checkout`,而不是传递分支名称，也可以方便地返回到您所在的上一个分支:

```
(my-feature)$ git checkout -
Switched to branch 'master'
(master)$ git checkout -
Switched to branch 'my-feature'
(my-feature)$
```

## 如何签出特定的提交

要检查或切换到特定的提交，您也可以使用`git checkout`并传递提交的 [SHA](https://en.wikipedia.org/wiki/Secure_Hash_Algorithms) 而不是分支名称。

毕竟，分支实际上只是 Git 历史中特定提交的指针和跟踪器。

### 如何找到提交的 SHA

找到提交的 SHA 的一种方法是查看 Git 日志。

您可以使用`git log`命令查看日志:

```
(my-feature)$ git log
commit 94ab1fe28727b7f8b683a0084e00a9ec808d6d39 (HEAD -> my-feature, master)
Author: John Mosesman <johnmosesman@gmail.com>
Date:   Mon Apr 12 10:31:11 2021 -0500

    This is the second commmit message.

commit 035a128d2e66eb9fe3032036b3415e60c728f692 (blah)
Author: John Mosesman <johnmosesman@gmail.com>
Date:   Mon Apr 12 10:31:05 2021 -0500

    This is the first commmit message. 
```

在每个提交的第一行，单词`commit`之后是一长串字符和数字:`94ab1fe28727...`

这叫做*。*阿沙是为每次提交生成的唯一标识符。

要检查一个特定的提交，您只需要将提交的 SHA 作为参数传递给`git checkout`:

```
(my-feature)$ git checkout 035a128d2e66eb9fe3032036b3415e60c728f692
Note: switching to '035a128d2e66eb9fe3032036b3415e60c728f692'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 035a128 a
((HEAD detached at 035a128))$ 
```

> **注意**:您通常只需要使用 SHA 的前几个字符，因为字符串的前四或五个字符在整个项目中很可能是唯一的。

## 什么是超脱的元首状态？

检查一个特定提交的结果会使你处于一种“分离的状态”

[来自文档:](http://git-scm.com/docs/git-checkout#_detached_head)

> [一个分离的头状态]仅仅意味着`HEAD`指向一个特定的提交，而不是指向一个指定的分支

基本上，`HEAD`(Git 的内部指针之一，跟踪你在 Git 历史中的位置)已经从已知的分支转移，因此从这一点开始的变化将在 Git 历史中形成一条新的路径。

Git 希望确保这是您想要的，所以它给了您一个“自由空间”来进行试验——如输出所示:

```
You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.
```

在这个位置上，你有两个选择:

*   尝试，然后通过返回到先前的分支来丢弃您的更改
*   从这里开始工作，并从这里开始一个新的分支

您可以使用`git switch -`命令撤销您所做的任何更改，并返回到之前的分支。

如果您想保留您的更改并从这里继续，您可以使用`git switch -c <new-branch-name>`到*从这里创建一个新的分支*。

## 结论

`git checkout`命令是一个有用的多用途命令。

您可以使用它来创建新的分支，检查分支，检查特定的提交，等等。

如果你喜欢这个教程，我也会在 Twitter 上谈论像这样的话题[，并在我的网站上写下关于它们的话题](https://twitter.com/johnmosesman)[。](https://johnmosesman.com/)