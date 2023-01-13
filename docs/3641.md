# Git 分支解释:如何在 Git 中删除、检出、创建和重命名分支

> 原文：<https://www.freecodecamp.org/news/git-branch-explained-how-to-delete-checkout-create-and-rename-a-branch-in-git/>

## **去分支**

Git 的分支功能允许您创建项目的新分支来测试想法、隔离新特性或进行实验，而不会影响主项目。

****目录****

*   [查看分支机构](https://guide.freecodecamp.org/git/git-branch/#view-branches)
*   [结账分支](https://guide.freecodecamp.org/git/git-branch/#checkout-a-branch)
*   [创建新的分支](https://guide.freecodecamp.org/git/git-branch/#create-a-new-branch)
*   [重命名分支](https://guide.freecodecamp.org/git/git-branch/#rename-a-branch)
*   [删除一个分支](https://guide.freecodecamp.org/git/git-branch/#delete-a-branch)
*   [比较分支](https://guide.freecodecamp.org/git/git-branch/#compare-branches)
*   [帮助 Git 分支](https://guide.freecodecamp.org/git/git-branch/#help-with-git-branch)
*   [更多信息](https://guide.freecodecamp.org/git/git-branch/#more-information)

### **查看分支机构**

要查看 Git 存储库中的分支，请运行以下命令:

```
git branch
```

要查看远程跟踪分支和本地分支，请运行命令:

```
git branch -a
```

您当前所在的分支旁边会有一个星号(*)。

您可以在`git branch`中包含许多不同的选项来查看不同的信息。关于分支的更多细节，您可以使用`-v`(或者`-vv`，或者`--verbose`)选项。分支列表将包括每个分支名称旁边的`HEAD`的 SHA-1 值和提交主题行。

您可以使用`-a`(或者`--all`)选项来显示存储库的本地分支以及任何远程分支。如果您只想查看远程分支，请使用`-r`(或`--remotes`)选项。

### **结账分支**

要签出现有分支，请运行命令:

```
git checkout BRANCH-NAME
```

通常，Git 不会让您签出另一个分支，除非您的工作目录是干净的，因为您会丢失任何未提交的工作目录更改。您有三个选项来处理您的更改:

1.  扔掉它们(详见[Git check out](https://www.freecodecamp.org/news/git-checkout-explained/))或者
2.  提交它们(详见[Git commit](https://www.freecodecamp.org/news/git-commit-command-explained/))或
3.  藏起来(详见)Git stash。

### **创建新的分支**

要创建新分支，请运行命令:

```
git branch NEW-BRANCH-NAME
```

注意，这个命令只创建新的分支。你需要运行`git checkout NEW-BRANCH-NAME`来切换到它。

有一种快捷方式可以一次创建并签出一个新分支。可以通过`git checkout`传递`-b`选项(针对分支)。以下命令执行相同的操作:

```
# Two-step method
git branch NEW-BRANCH-NAME
git checkout NEW-BRANCH-NAME

# Shortcut
git checkout -b NEW-BRANCH-NAME
```

当您创建一个新分支时，它将包括来自父分支的所有提交。父分支是创建新分支时所在的分支。

### **重命名分支**

要重命名分支，请运行命令:

```
git branch -m OLD-BRANCH-NAME NEW-BRANCH-NAME

# Alternative
git branch --move OLD-BRANCH-NAME NEW-BRANCH-NAME
```

### **删除一个分支**

Git 不会让你删除当前所在的分支。您首先需要签出一个不同的分支，然后运行命令:

```
git branch -d BRANCH-TO-DELETE

# Alternative:
git branch --delete BRANCH-TO-DELETE
```

您切换到的分支会有所不同。如果您试图删除的分支中的更改没有完全合并到当前分支中，Git 将抛出一个错误。您可以覆盖它，并使用`-D`选项(注意大写字母)或使用`--force`选项和`-d`或`--delete`强制 Git 删除分支:

```
git branch -D BRANCH-TO-DELETE

# Alternatives
git branch -d --force BRANCH-TO-DELETE
git branch --delete --force BRANCH-TO-DELETE
```

### **比较分支**

您可以使用`git diff`命令比较分支:

```
git diff FIRST-BRANCH..SECOND-BRANCH
```

您将看到分支之间变化的彩色输出。对于所有已更改的行，`SECOND-BRANCH`版本将是以“+”开头的绿线，`FIRST-BRANCH`版本将是以“-”开头的红线。如果您不想让 Git 为每个变更显示两行，您可以使用`--color-words`选项。相反，Git 将用红色显示一行删除的文本，用绿色显示添加的文本。

如果您想要查看完全合并到您当前分支中的所有分支的列表(换句话说，您的当前分支包括列出的其他分支的所有变更)，运行命令`git branch --merged`。

### **帮助 Git 分支**

如果您忘记了如何使用某个选项，或者想要探索与`git branch`命令相关的其他功能，您可以运行以下任何命令:

```
git help branch
git branch --help
man git-branch
```

### **更多信息:**

*   [命令`git merge`命令](https://www.freecodecamp.org/news/the-ultimate-guide-to-git-merge-and-git-rebase/)
*   [命令`git checkout`命令](https://www.freecodecamp.org/news/git-checkout-explained/)
*   [命令`git commit`命令](https://www.freecodecamp.org/news/git-commit-command-explained/)
*   [命令`git stash`命令](https://www.freecodecamp.org/news/git-stash-explained/)
*   Git 文档:[分支](https://git-scm.com/docs/git-branch)