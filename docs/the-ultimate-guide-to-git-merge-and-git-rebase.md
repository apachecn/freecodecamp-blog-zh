# Git 合并和 Git Rebase 的最终指南

> 原文：<https://www.freecodecamp.org/news/the-ultimate-guide-to-git-merge-and-git-rebase/>

欢迎来到我们的`git merge`和`git rebase`命令终极指南。本教程将教你所有你需要知道的关于用 Git 组合多个分支的知识。

## Git Merge

`git merge`命令将把在一个单独的分支上对代码基础所做的任何更改合并到你的当前分支，作为一个新的提交。

命令语法如下:

```
git merge BRANCH-NAME
```

例如，如果您当前正在名为`dev`的分支中工作，并且想要合并名为`new-features`的分支中所做的任何新的变更，您可以发出以下命令:

```
git merge new-features
```

****注意:**** 如果您当前的分支上有任何未提交的变更，Git 将不会允许您进行合并，直到您当前分支中的所有变更都已提交。要处理这些更改，您可以:

创建一个新的分支并提交更改

```
git checkout -b new-branch-name
git add .
git commit -m "<your commit message>"
```

藏起来

```
git stash               # add them to the stash
git merge new-features  # do your merge
git stash pop           # get the changes back into your working tree
```

放弃所有更改

```
git reset --hard        # removes all pending changes
```

## Git Rebase

在 Git 中改变分支的基础是一种将整个分支移动到树中另一点的方法。最简单的例子是在树中向上移动一个分支。假设我们在 A 点有一个从主分支分出的分支:

```
 /o-----o---o--o-----o--------- branch
--o-o--A--o---o---o---o----o--o-o-o--- master
```

当您重设基础时，您可以像这样移动它:

```
 /o-----o---o--o-----o------ branch
--o-o--A--o---o---o---o----o--o-o-o master
```

要进行 rebase，确保您的主分支中的 rebase 中有您想要的所有提交。检查您想要重置的分支并键入`git rebase master`(其中 master 是您想要重置的分支)。

也可以基于不同的分支，例如，基于另一个分支的分支(我们称之为特性)基于主分支:

```
 /---o-o branch
           /---o-o-o-o---o--o------ feature
----o--o-o-A----o---o--o-o-o--o--o- master
```

在`git rebase master branch`或`git rebase master`之后，当您检查完分支后，您将得到:

```
 /---o-o-o-o---o--o------ feature
----o--o-o-A----o---o--o-o-o--o--o- master
                                  \---o-o branch
```

### 控制台中的 Git rebase interactive

要在控制台中使用带有提交列表的`git rebase`,您可以选择、编辑或删除 rebase:

*   输入`git rebase -i HEAD~5`,最后一个数字是您想要查看的最近一次提交的任意数量。
*   在 vim 中，按`esc`，然后按`i`开始编辑测试。
*   在左侧，您可以用以下命令之一覆盖`pick`。如果您想将一个提交压缩到前一个提交中并丢弃提交消息，请在提交的`pick`处输入`f`。
*   保存并退出文本编辑器。
*   当重设基础停止时，进行必要的调整，然后使用`git rebase --continue`直到重设基础成功。
*   如果它成功地重定基础，那么您需要使用`git push -f`强制推送您的更改，以将重定基础的版本添加到您的远程存储库中。
*   如果存在合并冲突，有很多方法可以解决，包括遵循[指南](https://help.github.com/enterprise/2.11/user/articles/resolving-a-merge-conflict-using-the-command-line/)中的建议。一种方法是在文本编辑器中打开文件，删除不需要的代码部分。然后使用`git add <file name>`后跟`git rebase --continue`。您可以通过输入`git rebase --skip`跳过冲突的提交，通过在控制台中运行`git rebase --abort`停止重置。

```
pick 452b159 <message for this commit>
pick 7fd4192 <message for this commit>
pick c1af3e5 <message for this commit>
pick 5f5e8d3 <message for this commit>
pick 5186a9f <message for this commit>

# Rebase 0617e63..5186a9f onto 0617e63 (30 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but stop to edit the commit message.
# e, edit = use commit, but stop to amend or add commit.
# s, squash = use commit, meld into previous commit and stop to edit the commit message.
# f, fixup = like "squash", but discard this commit's log message thus doesn't stop.
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom. 
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

## 合并冲突

合并冲突是指在不同的分支上进行提交，而这些分支以冲突的方式改变了同一行。如果发生这种情况，Git 将在类似如下的错误消息中不知道应该保留文件的哪个版本:

```
CONFLICT (content): Merge conflict in resumé.txt Automatic merge failed; fix conflicts and then commit the result.
```

如果您在代码编辑器中查看`resumé.txt`文件，您可以看到冲突发生的位置:

```
<<<<<<< HEAD
Address: 808 South Street
=======
Address: 505 North Street
>>>>>>> updated_address
```

Git 在文件中添加了一些额外的行:

*   `<<<<<<< HEAD`
*   `=======`
*   `>>>>>>> updated_address`

把`=======`想成冲突的分界线。`<<<<<<< HEAD`和`=======`之间的所有内容都是头 ref 指向的当前分支的内容。另一方面，`=======`和`>>>>>>> updated_address`之间的所有内容都是被合并的分支`updated_address`中的内容。

## Git Merge vs Git Rebase

`git merge`和`git rebase`都是非常有用的命令，一个不比另一个好。但是，这两个命令之间有一些非常重要的区别，您和您的团队应该加以考虑。

每当运行`git merge`时，就会创建一个额外的合并提交。每当您在本地存储库中工作时，过多的合并提交会使提交历史看起来混乱。避免合并提交的一种方法是使用`git rebase`来代替。

`git rebase`是一个非常强大的功能。话虽如此，但如果使用不当， ****也有风险**** 。`git rebase`改变提交历史，所以要小心使用。如果在远程存储库中进行基础变更，那么当其他开发人员试图从远程存储库中获取最新的代码变更时，就会产生很多问题。记住只在本地存储库中运行`git rebase`。

这就是你所需要知道的与他们中最好的人合并和重组。