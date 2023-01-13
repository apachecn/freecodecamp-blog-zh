# Git 樱桃采摘命令

> 原文：<https://www.freecodecamp.org/news/the-git-cherry-pick-command/>

## **Git Cherry Pick**

`git cherry-pick`命令应用一些现有提交引入的更改。本指南将尽可能多地解释这个特性，当然真正的 [Git 文档](https://git-scm.com/docs/git-cherry-pick)总是会派上用场的。

### **从主人那里取出一个现存的樱桃树枝**

在主分支的末端应用由提交引入的更改，并使用此更改创建新的提交。运行以下命令

```
git cherry-pick master
```

### **从不同的提交中检入变更**

要在所需的特定哈希值应用提交引入的更改，请运行以下命令

```
git cherry-pick {HASHVALUE}
```

这将把提交中引用的变更添加到您当前的存储库中

### **将某些提交从一个分支应用到另一个分支**

允许你从一个分支到另一个分支进行选择。假设你有两个分支`master`和`develop-1`。在分支`develop-1`中，您有 3 个提交，提交 id 分别为`commit-1`、`commit-2`和`commit-3`。在这里，您只能通过以下方式将`commit-2`应用到分支`master`:

```
git checkout master
git cherry-pick commit-2
```

如果您在这一点上遇到任何冲突，您必须修复它们并使用`git add`添加它们，然后您可以使用继续标志来应用精选。

```
git cherry-pick --continue
```

如果您希望中止中间的精选，您可以使用中止标志:

```
git cherry-pick --abort
```

## 关于 Git 命令的更多信息:

*   [每个开发者都应该知道的 10 个终端窍门](https://www.freecodecamp.org/news/10-important-git-commands-that-every-developer-should-know/)
*   [一个开发者一周内使用的所有 Git 命令](https://www.freecodecamp.org/news/git-cheat-sheet-and-best-practices-c6ce5321f52/)