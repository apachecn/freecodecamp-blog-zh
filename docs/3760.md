# Git Pull 解释道

> 原文：<https://www.freecodecamp.org/news/git-pull-explained/>

`git pull`是一个 Git 命令，用于从远程更新存储库的本地版本。

它是 Git 提示网络交互的四个命令之一。默认情况下，`git pull`做两件事。

1.  更新当前本地工作分支(当前已签出的分支)
2.  更新所有其他分支的远程跟踪分支。

`git pull`获取(`git fetch`)新提交并将 [( `git merge` )](https://guide.freecodecamp.org/git/git-merge) 这些合并到您的本地分支。

该命令的语法如下:

```
# General format
git pull OPTIONS REPOSITORY REFSPEC

# Pull from specific branch
git pull REMOTE-NAME BRANCH-NAME
```

其中:

*   ****选项**** 为命令选项，如`--quiet`或`--verbose`。您可以在 [Git 文档](https://git-scm.com/docs/git-pull)中阅读更多关于不同选项的信息
*   ****仓库**** 是你的回购的 URL。例子:【https://github.com/freeCodeCamp/freeCodeCamp.git 
*   ****REFSPEC**** 指定要获取的参考和要更新的本地参考
*   ****远程名称**** 是您的远程存储库的名称。比如:*产地*。
*   ****【分店名称】**** 是您分店的名称。比如:*养成*。

****注****

如果您有未提交的更改，`git pull`命令的合并部分将会失败，您的本地分支将不会受到影响。

因此，在从远程存储库中提取新的提交之前，您应该总是在分支中提交您的更改。

****目录****

*   [使用`git pull`](https://guide.freecodecamp.org/git/git-pull/#using-git-pull)
*   [分布式版本控制](https://guide.freecodecamp.org/git/git-pull/#distributed-version-control)
*   [`git fetch` + `git merge`](https://guide.freecodecamp.org/git/git-pull/#git-fetch-plus-git-merge)
*   [`git pull`在 IDEs](https://guide.freecodecamp.org/git/git-pull/#git-pull-in-IDEs)

### **使用 git 拉取**

使用`git pull`从相应的远程存储库更新本地存储库。例:在`master`本地工作时，执行`git pull`更新`master`的本地副本，并更新其他远程跟踪分支。(下一节将详细介绍远程跟踪分支机构。)

但是，要让这个例子成立，需要记住几件事:

本地存储库有一个链接的远程存储库

*   通过执行`git remote -v`进行检查
*   如果有多个遥控器，`git pull`可能不够信息。你可能需要输入`git pull origin`或者`git pull upstream`。

您当前签出到的分支有相应的远程跟踪分支

*   通过执行`git status`进行检查。如果没有远程跟踪分支，Git 不知道从那里拉信息*。*

### **分布式版本控制**

Git 是一个 ****分布式版本控制系统**** (DVCS)。有了 DVCS，开发人员可以在不同的环境中同时处理同一个文件。在*将*代码上传到共享的远程存储库之后，其他开发人员可以*提取*更改后的代码。

#### **Git 中的网络交互**

Git 中只有四个命令提示网络交互。在收到信息请求之前，本地存储库并不知道远程存储库所做的更改。此外，在提交之前，远程存储库不知道本地更改。

这四个网络命令是:

*   `git clone`
*   `git fetch`
*   `git pull`
*   `git push`

#### **DVCS 分行**

当使用 Git 时，感觉好像到处都是相同代码的许多副本。每个分支上都有同一文件的不同版本。并且，在每个开发人员的计算机和远程计算机上有相同分支的不同副本。为了跟踪这一点，Git 使用了一种叫做 ****的远程跟踪分支**** 。

如果在 Git 存储库中执行`git branch --all`,远程跟踪分支会显示为红色。这些是遥控器上出现的代码的只读副本。(上一次将信息带到本地的网络交互是什么时候？请记住此信息上次更新的时间。远程跟踪分支中的信息反映了来自该交互的信息。)

有了 ****远程跟踪分支**** ，你可以在 Git 中工作在几个分支上，无需网络交互。每次执行`git pull`或`git fetch`命令时，更新 ****遥控跟踪分支**** 。

### **git 获取加 git 合并**

`git pull`是组合命令，等于`git fetch` + `git merge`。

#### **git fetch**

`git fetch`自行更新本地存储库中的所有远程跟踪分支。任何本地工作分支实际上都没有反映任何变化。

#### **git merge**

没有任何参数，`git merge`会将对应的远程跟踪分支合并到本地工作分支。

#### **git pull**

`git fetch`更新远程跟踪分支机构。`git merge`用相应的远程跟踪分支更新当前分支。使用`git pull`，你可以获得这两部分的更新。但是，这意味着如果您签出到`feature`分支并执行`git pull`，当您签出到`master`时，任何新的更新都不会被包括在内。每当您签出到另一个可能有新变化的分支时，执行`git pull`总是一个好主意。

### **git 拉进 IDEs**

其他 ide 中的通用语言可能不包含`pull`这个词。如果你在寻找单词`git pull`却没有看到它们，那就寻找单词`sync`。

### **将远程 PR(拉取请求)提取到本地 repo**

出于审查等目的，应将远程的 PRs 提取到本地 repo。您可以使用如下`git fetch`命令来实现这一点。

`git fetch origin pull/ID/head:BRANCHNAME`

id 是拉请求 ID，BRANCHNAME 是您想要创建的分支的名称。一旦创建了分支，您可以使用`git checkout`切换到该分支。

### **Git 上你可能喜欢的其他资源:**

*   [Git 合并和 Git rebase](https://www.freecodecamp.org/news/the-ultimate-guide-to-git-merge-and-git-rebase/)
*   [Git checkout](https://www.freecodecamp.org/news/git-checkout-file-from-another-branch/)
*   [去委员会](https://www.freecodecamp.org/news/git-commit-command-explained/)
*   [Git stash](https://www.freecodecamp.org/news/git-stash-explained/)
*   [去分支](https://www.freecodecamp.org/news/git-branch-explained-how-to-delete-checkout-create-and-rename-a-branch-in-git/)