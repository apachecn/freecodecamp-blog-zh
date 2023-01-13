# 错误:无法将一些引用推送到–如何在 Git 中修复

> 原文：<https://www.freecodecamp.org/news/error-failed-to-push-some-refs-to-how-to-fix-in-git/>

当使用 Git 与其他开发人员合作时，您可能会遇到`error: failed to push some refs to [remote repo]`错误。

当您试图将本地更改推送到 GitHub，而本地存储库(repo)尚未使用远程存储库中的任何更改进行更新时，就会出现此错误。

所以 Git 试图告诉您，在推送您自己的更改之前，用远程中的当前更改来更新本地 repo。这是必要的，这样您就不会覆盖其他人所做的更改。

在接下来的章节中，我们将讨论两种可能的方法来修复这个错误。

## 如何修复 Git 中的`error: failed to push some refs to`错误

我们可以使用`git pull origin [branch]`或`git pull --rebase origin [branch]`命令来修复 Git 中的`error: failed to push some refs to [remote repo]`错误。在大多数情况下，后者可以修复错误。

让我们来看看如何使用上面的命令。

### 如何使用`git pull`修复 Git 中的`error: failed to push some refs to`错误

发送拉请求意味着“获取”对远程 repo 所做的新更改，并将它们与本地 repo 合并。

一旦合并完成，您就可以将您自己的代码更改推送到 GitHub。

在我们的例子中，我们试图通过发送一个 pull 请求来消除`error: failed to push some refs to [remote repo]`错误。

你可以这样做:

```
git pull origin main
```

如果您正在处理一个不同的分支，那么您必须用您的分支的名称替换上面例子中的`main`。

请记住，使用此命令同步您的远程和本地 repos 以消除错误时，有可能会失败。如果请求成功，那么继续运行下面的命令来推送您自己的更改:

```
git push -u origin main
```

如果错误仍然存在，您将会得到一个错误消息:`fatal: refusing to merge unrelated histories`。在这种情况下，使用下一节中的解决方案。

### 如何使用`git pull --rebase`修复 Git 中的`error: failed to push some refs to`错误

在本地分支在远程分支之后提交的情况下,`git pull --rebase`命令很有帮助。

要修复错误，请继续运行以下命令:

```
git pull --rebase origin main

git push -u origin main 
```

如果上面的第一个命令运行成功，您应该会得到一个响应:`Successfully rebased and updated refs/heads/main`。

第二个命令将本地 repo 的当前状态推送到远程分支。

## 摘要

在本文中，我们讨论了`error: failed to push some refs to [remote repo]`错误。

当您试图将本地更改推送到远程存储库，而不使用对远程存储库所做的新更改来更新本地存储库时，会出现此错误。

我们讨论了两个可以用来修复错误的命令:`git pull origin [branch]`和`git pull --rebase origin [branch]`命令。

我希望这有助于您修复错误。

编码快乐！