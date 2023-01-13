# 如何在本地和远程删除 Git 分支

> 原文：<https://www.freecodecamp.org/news/how-to-delete-a-git-branch-both-locally-and-remotely/>

在大多数情况下，删除一个 Git 分支很简单。在本文中，您将了解如何在本地和远程删除 Git 分支。

### TL；灾难恢复版本

```
// delete branch locally
git branch -d localBranchName

// delete branch remotely
git push origin --delete remoteBranchName 
```

## 何时删除分支

Git repo 有不同的分支是很常见的。在将新代码与主代码库隔离的同时，它们是处理不同特性和修复的好方法。

回购通常有一个主代码库的`main`分支，开发人员创建其他分支来处理不同的功能。

一旦某个特性的工作完成，通常建议删除该分支。

## 本地删除分支

Git 不会让您删除当前所在的分支，所以您必须确保签出您没有删除的分支。例如:`git checkout main`

删除带有`git branch -d <branch>`的分支。

例如:`git branch -d fix/authentication`

只有当分支已经被推送并与远程分支合并时，`-d`选项才会删除分支。如果您想强制删除分支，请使用`-D`,即使它还没有被推送或合并。

该分支现在已在本地删除。

## 远程删除分支

下面是远程删除分支的命令:`git push <remote> --delete <branch>`。

例如:`git push origin --delete fix/authentication`

该分支现在被远程删除。

您也可以使用这个较短的命令远程删除一个分支:`git push <remote> :<branch>`

例如:`git push origin :fix/authentication`

如果您得到下面的错误，这可能意味着其他人已经删除了该分支。

```
error: unable to push to unqualified destination: remoteBranchName The destination refspec neither matches an existing ref on the remote nor begins with refs/, and we are unable to guess a prefix based on the source ref. error: failed to push some refs to 'git@repository_name' 
```

尝试使用以下方法同步您的分支列表:

```
git fetch -p 
```

`-p`旗的意思是“修剪”。提取后，远程服务器上不再存在的分支将被删除。

如果你觉得这个教程有帮助，我们的非营利组织有超过 8000 个这样的实用教程。全部免费，没有广告。告诉你的朋友。😉