# Git 删除上次提交——如何在 Git 中撤销提交

> 原文：<https://www.freecodecamp.org/news/git-remove-last-commit-how-to-undo-a-commit-in-git/>

Git 是一个强大的工具，也是最流行的版本控制系统。这是开发人员和技术团队在项目中合作和工作的方式。

但是，当您意外地提交了一个文件，并意识到您不应该这样做，因为该文件包含一个错误，会发生什么呢？

没有必要担心，因为 Git 允许您撤销错误并返回到项目的早期版本。

Git 最有帮助的特性之一是能够撤销您对项目所做的更改。

在本文中，您将了解如何根据 Git 存储库的状态撤销 Git 中的更改。

以下是我们将要介绍的内容:

1.  [如何撤销本地未暂存的变更](#unstaged)
2.  [如何撤销本地阶段性变更](#staged)
3.  [如何撤销本地提交的更改](#local-committed)
4.  [如何撤销公共提交的更改](#public-committed)

## 如何撤销 Git 中的本地未暂存更改

假设您正在本地机器上工作。您在本地对文件进行了一些更改并保存了这些更改，但是您想放弃这些更改。

当您还没有准备好这些变化时，您还没有使用`git add`命令。

在这种情况下，你需要使用`git restore`命令。

具体来说，`git restore`命令看起来像这样:

```
git restore filename 
```

所以，假设你有一个`README.md`文件，你不小心写下并保存了一些你想丢弃的文本。

您可以首先使用`git status`命令来查看 Git 存储库的状态。

该命令将确认文件未转移(意味着您尚未使用`git add`)，并允许您查看您可能想要撤销的文件:

```
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a") 
```

以下是如何撤销对`README.md`文件的更改:

```
git restore README.md 
```

然后您可以再次使用`git status`来检查存储库的状态:

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean 
```

现在，您已经成功地放弃了最近的更改，并恢复到项目的最后一个提交版本。

## 如何撤销 Git 中的本地阶段性更改

当您使用`git add`命令时，文件被暂存。

因此，假设您在本地对`README.md`文件进行了一些更改，您使用了`git add`命令来暂存这些更改，然后您意识到文本包含一些错误。

首先，运行`git status`以确保您已经暂存了文件(意味着您使用了`git add`):

```
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   README.md 
```

从`git status`的输出可以看出，您可以使用下面的命令来撤销您的更改:

```
git restore --staged filename 
```

此命令将取消暂存文件的暂存，但会保留您的更改。

让我们再次运行`git status`:

```
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a") 
```

现在，要放弃您所做的更改并将文件恢复到其原始内容，请使用:

```
git restore README.md 
```

让我们最后一次运行`git status`:

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean 
```

现在，您所做的更改已经消失，文件恢复到当前提交的版本。

## 如何撤销 Git 中本地提交的更改

假设您对一个文件进行了更改，您用`git add`命令暂存了该文件，并用`git commit`命令提交了该文件。

这意味着提交只存在于本地，还没有被推送到远程存储库。

首先，使用`git status`检查您是否提交了文件:

```
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean 
```

接下来，如果您想撤销上一次本地提交，使用`git log`命令:

最近的提交将有一个提交散列(一长串数字和字符)和一个结尾的`(HEAD -> main)`-这是您想要撤销的提交。

倒数第二个提交在末尾有一个提交散列和一个`(origin/main)`——这是您想要保留的提交和您推送到远程存储库的提交。

之后，使用以下命令撤销提交:

```
git reset --soft HEAD~ 
```

现在，让我们再次使用`git log`。

您应该会看到提交散列，以及结尾的一个`(HEAD -> main, origin/main)`。

您所做的最后一次提交不再是存储库历史的一部分，并且已被删除。

上面的命令将文件的所有内容恢复到意外或错误提交之前的版本，并返回一次提交。

让我们再检查一下`git status`

```
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   README.md 
```

请记住，尽管`git reset --soft HEAD~`命令撤销了您最近的提交，但它保留了您所做的更改。

## 如何撤销 Git 中公共提交的更改

如果您对一个文件进行了更改，您暂存了文件`git add`，用`git command`提交了它，并用`git push`将它推送到一个远程存储库，但是随后意识到您不应该首先提交该文件，那会怎么样呢？

那你会怎么做？

首先，使用`git status`来检查 git 存储库的状态:

```
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean 
```

在上一节中，您看到了每个提交都有一个提交散列，它是一长串数字和字符。

要查看提交哈希的简短版本，请使用以下命令:

```
git log --oneline 
```

使用`git log`命令，您还可以检查想要撤销的提交。

假设您最近的提交有一个提交哈希`cc3bbf7`，后跟`(HEAD -> main, origin/main)`，以及一个提交消息，如“提交 README.md 文件”。

要撤消特定提交，请使用以下命令:

```
git revert cc3bbf7 --no-edit 
```

上面的命令将通过创建一个新的提交并将该文件恢复到其以前的状态来撤消更改，就像它从未更改过一样。

最后，使用`git push`将变更推送到远程分支。

一旦您这样做了，您将看到提交消息将与前一个消息相同，但是在它前面有单词`revert`，例如
'Revert "commit README.md file " '。

请记住，提交历史将分别显示两次提交:

```
Revert "commit README.md file"
@john-doe
john-doe committed 9 minutes ago

commit README.md file
@john-doe
john-doe committed 16 minutes ago 
```

## 结论

现在，您已经知道了如何撤销 Git 中的更改。

要了解关于 Git 的更多信息，请查看以下免费资源:

*   [Git 和 GitHub 初学者速成班](https://www.youtube.com/watch?v=RGOj5yH7evk)
*   [Git 专业教程-工具&用 Git 掌握版本控制的概念](https://www.youtube.com/watch?v=Uszj_k0DGsg)
*   [高级 Git 教程——交互式 Rebase、Cherry-Picking、Reflog、子模块等](https://www.youtube.com/watch?v=qsTthZi23VE)

感谢您的阅读和快乐编码:)