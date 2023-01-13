# Git 签出解释:如何在 Git 中签出、更改或切换分支

> 原文：<https://www.freecodecamp.org/news/git-checkout-explained/>

`git checkout`命令在分支之间切换或恢复工作树文件。这个命令有许多不同的选项，这里不做介绍，但是你可以在 [Git 文档](https://git-scm.com/docs/git-checkout)中查看所有选项。

### **签出特定提交**

要签出特定提交，请运行以下命令:

```
git checkout specific-commit-id
```

我们可以通过运行以下命令来获取特定的提交 id:

```
git log
```

### **签出现有分支**

要签出现有分支，请运行命令:

```
git checkout BRANCH-NAME
```

通常，Git 不会让您签出另一个分支，除非您的工作目录是干净的，因为您会丢失任何未提交的工作目录更改。你有三个选择来处理你的改变:1)丢弃它们，2) [提交它们](https://guide.freecodecamp.org/git/git-commit/)，或者 3) [隐藏它们](https://guide.freecodecamp.org/git/git-stash/)。

### **签出新的分支**

要用一个命令创建和签出一个新分支，您可以使用:

```
git checkout -b NEW-BRANCH-NAME
```

这将自动将您切换到新的分支。

### **签出新分支或将分支重置为起始点**

下面的命令类似于检查一个新的分支，但是使用了`-B`(注意大写 B)标志和一个可选的`START-POINT`参数:

```
git checkout -B BRANCH-NAME START-POINT
```

如果`BRANCH-NAME`分支不存在，Git 将创建它并在`START-POINT`开始。如果`BRANCH-NAME`分支已经存在，那么 Git 将该分支重置为`START-POINT`。这相当于用`-f`运行`git branch`。

### **强制结账**

您可以通过使用`git checkout`命令传递`-f`或`--force`选项来强制 Git 切换分支，即使您有未分段的更改(换句话说，工作树的索引不同于`HEAD`)。基本上可以用来扔掉局部改动。

当您运行以下命令时，Git 将忽略未合并的条目:

```
git checkout -f BRANCH-NAME

# Alternative
git checkout --force BRANCH-NAME
```

### **撤销工作目录中的更改**

您可以使用`git checkout`命令撤销您对工作目录中的文件所做的更改。这将把文件恢复到`HEAD`中的版本:

```
git checkout -- FILE-NAME
```