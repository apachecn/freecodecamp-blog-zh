# Git 签出远程分支教程

> 原文：<https://www.freecodecamp.org/news/git-checkout-remote-branch-tutorial/>

Git 是一个版本控制工具，允许您维护和查看应用程序的不同版本。当一个新的更新破坏了你的应用程序时，Git 允许你把这些修改恢复到以前的版本。

除了版本控制，Git 还允许您同时在多个环境中工作。这个上下文中的多个环境意味着**分支**。

## 为什么需要分支机构

当您使用 git 时，您将拥有一个主(也称为主)环境(分支)。这个特殊的分支保存着源代码，当您的应用程序准备好投入生产时，源代码就会被部署。

当您想要更新您的应用程序时，您还可以向该分支添加更多的提交(更改)。对于小的改动，这可能不是什么大事，但是对于大的改动，这样做就不理想了。这就是其他分支存在的原因。

要创建和使用新的分支，您可以在终端的项目目录中使用以下命令:

```
# create a new branch
git branch branch-name
# change environment to the new branch
git checkout branch-name 
```

在这个新的分支上，您可以创建新的变更。完成后，您可以将它们与主分支合并。

分支的另一个好处是它们允许多个开发人员同时处理同一个项目。如果您有多个开发人员在同一个主分支上工作，这可能是灾难性的。每个开发人员的代码之间有太多的变化，这通常会导致合并冲突。

使用 Git，您可以跳到另一个分支(另一个环境)并在那里进行更改，而工作在其他分支中继续进行。

## Git Checkout 远程分支是什么意思？

当您使用 Git 开始一个项目时，您会得到两个环境:本地主分支(存在于您的计算机中)和远程主分支(存在于 Git 支持的平台中，如 GitHub)。

您可以将更改从本地主分支推送到远程主分支，也可以从远程分支拉更改。

当您在本地创建一个分支时，它只存在于本地，直到它被推送到 GitHub，在那里它成为远程分支。这显示在以下示例中:

```
# create a new branch
git branch new-branch
# change environment to the new branch
git checkout new-branch
# create a change
touch new-file.js
# commit the change
git add .
git commit -m "add new file"
# push to a new branch
git push --set-upstream origin new-branch 
```

从上面的例子来看，`origin new-branch`成为远程分支。您可能已经注意到，我们创建了一个新的分支，并在推送到新的远程分支之前提交了一个变更。

但是，如果远程分支已经存在，并且我们希望将该分支及其所有更改都转移到我们的本地环境中，该怎么办呢？

那就是我们“Git Checkout 远程分支”的地方。

## 如何 Git 结帐远程分行

假设有一个由另一个开发人员创建的远程分支，您想要提取该分支。你可以这样做:

### 1.获取所有远程分支

```
git fetch origin 
```

这将从存储库中获取所有远程分支。`origin`是您要定位的远程名称。因此，如果你有一个`upstream`远程名称，你可以调用`git fetch upstream`。

### 2.列出可供签出的分支

要查看可供签出的分支，请运行以下命令:

```
git branch -a 
```

该命令的输出是可用于签出的分支列表。对于远程分支，您会发现它们带有前缀`remotes/origin`。

### 3.从远程分支提取更改

请注意，您不能直接在远程分支上进行更改。因此，您需要该分支的副本。假设您想要复制远程分支`fix-failing-tests`，您可以这样做:

```
git checkout -b fix-failing-tests origin/fix-failing-tests 
```

它的作用是:

*   它创建了一个名为`fix-failing-tests`的新分支
*   是那根树枝
*   它将变更从`origin/fix-failing-tests`拉至该分支

现在您有了远程分支的副本。此外，您可以将提交推送到该远程分支。例如，您可以像这样进行新的提交:

```
touch new-file.js
git add .
git commit -m "add new file"
git push 
```

这将把提交的更改推送到`origin/fix-failing-tests`。如果你注意到了，我们不必指定我们在哪里推动改变(像`git push origin fix-failing-tests`)。这是因为 git 自动设置本地分支来跟踪远程分支。

## 结论

Git 分支使得应用程序开发期间的协作变得非常容易。

有了分支，不同的开发人员可以很容易地同时处理应用程序的不同部分。

使用 checkout 远程分支，协作甚至变得更加无缝，因为开发人员还可以在他们的系统上本地复制远程分支，进行更改，并推送到远程分支。