# Git 备忘单——你应该知道的 50 个 Git 命令

> 原文：<https://www.freecodecamp.org/news/git-cheat-sheet/>

Git 是一个分布式版本控制系统，可以帮助开发人员在任何规模的项目上进行协作。

Linux 内核的开发者 Linus Torvalds 在 2005 年创建了 Git 来帮助控制 Linux 内核的开发。

## 什么是分布式版本控制系统？

分布式版本控制系统是一种帮助您跟踪对项目中的文件所做的更改的系统。

这个变更历史保存在您的本地机器上，如果出现问题，您可以轻松地恢复到项目的前一个版本。

Git 让协作变得简单。团队中的每个人都可以在他们的本地机器上保存他们正在工作的存储库的完整备份。然后，由于像 BitBucket、GitHub 或 GitLab 这样的外部服务器，他们可以安全地将存储库存储在一个地方。

这样，团队的不同成员可以在本地复制它，每个人都可以清楚地看到整个团队所做的所有更改。

Git 有许多不同的命令可以使用。我发现这 50 个是我最常用的(因此也是最有助于记忆的)。

所以我把它们写了下来，并认为与社区分享它们会很好。我希望你会觉得它们有用——请享用。

## 如何检查您的 Git 配置:

下面的命令返回关于 git 配置的信息列表，包括用户名和电子邮件:

```
git config -l 
```

## 如何设置您的 Git 用户名:

使用下面的命令，您可以配置您的用户名:

```
git config --global user.name "Fabio" 
```

## 如何设置你的 Git 用户邮箱:

此命令允许您设置将在提交中使用的用户电子邮件地址。

```
git config --global user.email "signups@fabiopacifici.com" 
```

## 如何在 Git 中缓存您的登录凭证:

您可以将登录凭据存储在缓存中，这样就不必每次都键入它们。只需使用以下命令:

```
git config --global credential.helper cache 
```

## 如何初始化 Git repo:

一切从这里开始。第一步是在项目根目录中本地初始化一个新的 Git repo。您可以使用下面的命令来完成此操作:

```
git init 
```

## 如何在 Git 中将文件添加到临时区域:

下面的命令将向临时区域添加一个文件。只需用您想要添加到暂存区域的文件的名称替换`filename_here`。

```
git add filename_here 
```

## 如何在 Git 中添加临时区域中的所有文件

如果您想将项目中的所有文件添加到临时区域，您可以使用通配符`.`,每个文件都会自动添加。

```
git add . 
```

## 如何在 Git 中只将某些文件添加到临时区域

使用下面命令中的星号，您可以在临时区域中添加所有以' fil '开头的文件。

```
git add fil* 
```

## 如何在 Git 中检查存储库的状态:

此命令将显示当前存储库的状态，包括暂存、未暂存和未跟踪的文件。

```
git status 
```

## 如何在 Git 的编辑器中提交更改:

该命令将在终端中打开一个文本编辑器，您可以在其中编写完整的提交消息。

提交消息由一个简短的更改摘要、一个空行和其后的更改的完整描述组成。

```
git commit 
```

## 如何在 Git 中用消息提交更改:

您可以在不打开编辑器的情况下添加提交消息。此命令只允许您为提交消息指定一个简短的摘要。

```
git commit -m "your commit message here" 
```

## 如何在 Git 中提交更改(并跳过临时区域):

通过使用-a 和-m 选项，您可以用一个命令添加和提交跟踪的文件。

```
git commit -a -m"your commit message here" 
```

## 如何在 Git 中查看您的提交历史:

此命令显示当前存储库的提交历史记录:

```
git log 
```

## 如何查看您的提交历史，包括 Git 中的更改:

此命令显示提交的历史记录，包括所有文件及其更改:

```
git log -p 
```

## 如何在 Git 中查看特定的提交:

该命令显示了一个特定的提交。

将 commit-id 替换为提交日志中 commit 一词后面的提交 id。

```
git show commit-id 
```

## 如何在 Git 中查看日志统计信息:

这个命令将使 Git 日志显示关于每次提交中的更改的一些统计信息，包括更改的行和文件名。

```
git log --stat 
```

## 如何在 Git 中使用“diff”提交更改之前查看更改:

您可以将文件作为参数传递，以便只查看特定文件的更改。
`git diff`默认只显示未暂存的变更。

我们可以用`--staged`标志调用 diff 来查看任何阶段性的变化。

```
git diff
git diff all_checks.py
git diff --staged 
```

## 如何使用“git add -p”查看更改:

此命令会打开一个提示，询问您是否要转移更改，并包括其他选项。

```
git add -p 
```

## 如何在 Git 中从当前工作树中移除被跟踪的文件:

此命令需要一条提交消息来解释文件被删除的原因。

```
git rm filename 
```

## 如何在 Git 中重命名文件:

该命令暂存更改，然后它期待一条提交消息。

```
git mv oldfile newfile 
```

## 如何忽略 Git 中的文件:

创建一个`.gitignore`文件并提交它。

## 如何恢复 Git 中未暂存的更改:

```
git checkout filename 
```

## 如何恢复 Git 中的阶段性变更:

您可以使用-p 选项标志来指定要重置的更改。

```
git reset HEAD filename
git reset HEAD -p 
```

## 如何在 Git 中修改最近的提交:

`git commit --amend`允许您修改和添加对最近提交的更改。

```
git commit --amend 
```

！！注意！！:用 amend 修复一个本地提交是很棒的，在修复之后你可以把它推到一个共享的存储库中。但是您应该避免修改已经公开的提交。

## 如何回滚 Git 中的最后一次提交:

将创建一个新的提交，它与给定提交中的所有内容相反。
我们可以通过使用 head 别名来恢复最新提交，如下所示:

```
git revert HEAD 
```

## 如何在 Git 中回滚旧的提交:

您可以使用旧提交的提交 id 来恢复旧提交。这将打开编辑器，您可以添加提交消息。

```
git revert comit_id_here 
```

## 如何在 Git 中创建新分支:

默认情况下，您有一个分支，即主分支。使用这个命令，您可以创建一个新的分支。Git 不会自动切换到它——您需要使用下一个命令手动切换。

```
git branch branch_name 
```

## 如何切换到 Git 中新创建的分支:

当您想要使用不同的或新创建的分支时，可以使用此命令:

```
git checkout branch_name 
```

## 如何在 Git 中列出分支:

您可以使用`git branch`命令查看所有创建的分支。它将显示所有分支的列表，并用星号标记当前分支，并以绿色突出显示。

```
git branch 
```

## 如何在 Git 中创建一个分支并立即切换到它:

在一个命令中，您可以立即创建并切换到一个新的分支。

```
git checkout -b branch_name 
```

## 如何在 Git 中删除分支:

当您处理完一个分支并将其合并后，您可以使用下面的命令将其删除:

```
git branch -d branch_name 
```

## 如何在 Git 中合并两个分支:

要将您当前所在分支的历史与`branch_name`合并，您需要使用下面的命令:

```
git merge branch_name 
```

## 如何在 Git 中将提交日志显示为图形:

我们可以使用`--graph`将提交日志显示为图形。同样，
`--oneline`将提交消息限制在一行中。

```
git log --graph --oneline 
```

## 如何将提交日志显示为 Git 中所有分支的图形:

与上面的命令相同，但适用于所有分支。

```
git log --graph --oneline --all 
```

## 如何中止 Git 中的冲突合并:

如果您想放弃合并并重新开始，可以运行以下命令:

```
git merge --abort 
```

## 如何在 Git 中添加远程存储库

该命令将一个远程存储库添加到您的本地存储库(只需用您的远程存储库 URL 替换`https://repo_here`)。

```
git add remote https://repo_here 
```

## 如何在 Git 中查看远程 URL:

您可以使用以下命令查看本地存储库的所有远程存储库:

```
git remote -v 
```

## 如何在 Git 中获得关于远程回购的更多信息:

只需用通过运行 git remote -v 命令获得的遥控器的名称来替换`origin`。

```
git remote show origin 
```

## 如何在 Git 中将更改推送到远程 repo:

当您的所有工作都准备好保存到远程存储库时，您可以使用下面的命令推送所有更改:

```
git push 
```

## 如何在 Git 中从远程 repo 提取变更:

如果其他团队成员正在使用您的存储库，您可以使用下面的命令检索对远程存储库所做的最新更改:

```
git pull 
```

## 如何检查 Git 正在跟踪的远程分支:

该命令显示 Git 正在为当前存储库跟踪的所有远程分支的名称:

```
git branch -r 
```

## 如何在 Git 中获取远程回购变更:

该命令将从远程 repo 下载更改，但不会在本地分支上执行合并(因为 git pull 会执行合并)。

```
git fetch 
```

## 如何在 Git 中检查远程 repo 的当前提交日志

一次又一次的提交，Git 建立了一个日志。您可以使用以下命令找到远程存储库日志:

```
git log origin/main 
```

## 如何在 Git 中将远程回购与本地回购合并:

如果远程存储库有您想要与本地存储库合并的更改，那么这个命令会为您完成:

```
git merge origin/main 
```

## 如何在不自动合并的情况下获取 Git 中远程分支的内容:

这允许您在不将任何内容合并到
本地分支的情况下更新远程。您可以调用 git merge 或 git checkout 来进行合并。

```
git remote update 
```

## 如何在 Git 中将一个新分支推送到远程 repo:

如果您想将一个分支推送到一个远程存储库，您可以使用下面的命令。只需记住添加-u 来创建上游分支:

```
git push -u origin branch_name 
```

## 如何在 Git 中删除远程分支:

如果不再需要远程分支，可以使用下面的命令将其删除:

```
git push --delete origin branch_name_here 
```

## 如何使用 Git rebase:

您可以使用`git rebase`将完成的工作从一个分支转移到另一个分支。

```
git rebase branch_name_here 
```

如果做得不好，Git Rebase 会变得非常混乱。在使用这个命令之前，我建议你在这里重新阅读官方文档

## 如何在 Git 中交互运行 rebase:

您可以使用-i 标志交互式地运行 git rebase。它将打开编辑器并显示一组您可以使用的命令。

```
git rebase -i master
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit 
```

## 如何在 Git 中强制推送请求:

该命令将强制推送请求。这对于拉请求分支来说通常没问题，因为其他人不应该克隆它们。但是这不是你想用公开回购做的事情。

```
git push -f 
```

## 结论

这些命令可以极大地提高您在 Git 中的工作效率。你不需要记住所有的内容——这就是为什么我写了这个备忘单。将此页加入书签以备将来参考，如果你愿意，也可以打印出来。

感谢阅读！对了，我是 Fabio，全栈 web 开发人员兼教师，Python IT 自动化认证专家。如果你觉得这个小抄有用，你肯定也会在我的 YouTube 频道上找到一些有趣的东西。可以在这里订阅[。](https://www.youtube.com/channel/UCTuFYi0pTsR9tOaO4qjV_pQ)