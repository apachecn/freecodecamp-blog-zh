# 如何在 Git 中重命名本地和远程分支

> 原文：<https://www.freecodecamp.org/news/how-to-rename-local-and-remote-git-branch/>

您曾经需要重命名 Git 分支吗？如果是这样，本文将帮助您解决这个问题。

Git 使得在本地和远程重命名 git 分支变得简单。我们来看看解决方案。

## 为什么需要在 Git 中重命名分支？

Git 分支是项目开发中不同的阶段。它们为您提供了一种与您的主分支一起工作的方式，同时保持它没有混乱和未完成的代码。

在匆忙中，您可能会命名一个不能准确描述代码的分支，或者您可能想要重命名它。在这些情况下，大多数人都希望更改分行名称。

## 如何重命名本地 git 分支

如果您已经位于要重命名的本地分支中，则可以使用此命令。

```
git branch -m <new_name>
```

如果您在不同的分支上，并且想要重命名它，请使用下面的命令:

```
git branch -m <old_name> <new_name>
```

使用以下命令确定您当前的分支名称:

```
git status 
```

## 如何重命名远程 git 分支

如果要重命名已经推送到远程存储库的分支，请使用以下命令:

```
git push origin -u <new_name>
```

现在你需要删除旧的名字。为此，请使用以下命令:

```
git push origin --delete <old_name>
```

要检查您刚才所做的更改，请登录到您的客户端门户，并检查您刚刚更改的存储库。

就这么定了！不是这么简单吗？🥳

我也定期在我的时事通讯上写作。你可以直接在这里注册。 ****👇👇****

[https://thelearners.substack.com/embed](https://thelearners.substack.com/embed)