# Git 重置和 Git 恢复的终极指南

> 原文：<https://www.freecodecamp.org/news/the-ultimate-guide-to-git-reset-and-git-revert/>

欢迎来到我们的`git reset`和`git revert`命令终极指南。本教程将教你在使用 Git 时修复常见错误和撤销错误提交的所有知识。

## 理解 Git 项目的三个部分

Git 项目有以下三个主要部分:

1.  Git 目录
2.  工作目录(或工作树)
3.  部队从一个战场转往另一个战场的集结地

****Git 目录**** (位于`YOUR-PROJECT-PATH/.git/`)是 Git 存储精确跟踪项目所需的一切的地方。这包括元数据和包含项目文件压缩版本的对象数据库。

****工作目录**** 是用户对项目进行本地修改的地方。工作目录从 Git 目录的对象数据库中提取项目文件，并将它们放在用户的本地机器上。

注: ****目录**** 又称 ****库**** 或简称回购。用户本地机器上的回购称为“本地回购”，而 git 服务器上的回购称为“远程回购”。

****暂存区**** 是一个文件(也称为“索引”、“暂存”或“缓存”)，它存储了关于下一次提交的内容的信息。提交是指您告诉 Git 保存这些分阶段的更改。Git 获取文件的快照，并将该快照永久存储在 Git 目录中。

通过三个部分，文件在任何给定时间都可以处于三种主要状态:修改、提交或暂存。每当你在你的工作目录中对一个文件做了改变，你就修改了这个文件。接下来，当您将它移动到暂存区时，它就是*暂存的*。最后，在提交之后是*提交*。

## 去重置

`git reset`命令允许您将当前磁头复位到指定状态。您可以重置特定文件以及整个分支的状态。如果您还没有将提交推送到 GitHub 或另一个远程存储库，这很有用。

### 重置一个或一组文件

以下命令允许您有选择地选择内容块，并对其进行还原或取消转移。

```
git reset (--patch | -p) [tree-ish] [--] [paths]
```

### 取消文件暂存

如果您使用`git add`将一个文件移动到暂存区域，但不再希望它成为提交的一部分，您可以使用`git reset`来取消该文件的暂存:

```
git reset HEAD FILE-TO-UNSTAGE
```

您所做的更改仍将存在于文件中，此命令只是将该文件从临时区域中删除。

### 将分支重置为以前的提交

以下命令将当前分支的 HEAD 重置为给定的`COMMIT`并更新索引。它基本上倒带你的分支的状态，然后你向前做的所有承诺都写在复位点之后的任何东西上。如果省略`MODE`，则默认为`--mixed`:

```
git reset MODE COMMIT
```

`MODE`的选项有:

*   `--soft`:不重置索引文件或工作树，但将 HEAD 重置为`commit`。将所有文件更改为“要提交的更改”
*   `--mixed`:重置索引，但不重置工作树，并报告未更新的内容
*   `--hard`:重置索引和工作树。自`commit`以来对工作树中被跟踪文件的任何更改都将被丢弃
*   `--merge`:重置索引，更新工作树中`commit`与 HEAD 不同的文件，但保留索引与工作树不同的文件
*   `--keep`:重置索引条目，更新工作树中与`commit`和 HEAD 不同的文件。如果`commit`和 HEAD 之间不同的文件有本地更改，重置被中止

### 关于硬复位的重要说明

将`--hard`选项与`git reset`一起使用时要非常小心，因为它会重置您的提交、暂存区和工作目录。如果这个选项使用不当，最终可能会丢失编写的代码。

## Git Revert

`git revert`和`git reset`命令都撤销先前的提交。但是如果您已经将提交推送到远程存储库，建议您不要使用`git reset`,因为它会重写提交的历史。这使得与其他开发人员一起使用一个存储库并维护提交的一致历史变得非常困难。

相反，最好使用`git revert`，它通过创建一个全新的提交来撤销前一次提交所做的更改，而不改变提交的历史。

### 还原一个或一组提交

以下命令允许您恢复以前提交的更改，并创建新的提交。

```
git revert [--[no-]edit] [-n] [-m parent-number] [-s] [-S[<keyid>]] <commit>…
git revert --continue
git revert --quit
git revert --abort
```

### 常见选项:

```
 -e
  --edit
```

*   这是默认选项，不需要显式设置。它会打开系统的默认文本编辑器，并允许您在提交恢复之前编辑新的提交消息。
*   该选项与`-e`相反，`git revert`不会打开文本编辑器。
*   该选项防止`git revert`撤销之前的提交并创建新的提交。`-n`不是创建一个新的提交，而是撤销前一次提交的更改，并将它们添加到暂存索引和工作目录中。

```
 --no-edit
```

```
-n
-no-commit
```

### 举例。

我们来设想以下情况:1。)您正在处理一个文件，您添加并提交您的更改。2.)然后，你做一些其他的事情，并进行更多的提交。3.)现在你意识到，三四次提交之前，你做了一些你想撤销的事情——你怎么能这样做呢？

您可能会想，只需使用`git reset`，但是这将删除您想要更改的提交之后的所有提交- `git revert`来拯救！让我们看一下这个例子:

```
mkdir learn_revert # Create a folder called `learn_revert`
cd learn_revert # `cd` into the folder `learn_revert`
git init # Initialize a git repository

touch first.txt # Create a file called `first.txt`
echo Start >> first.txt # Add the text "Start" to `first.txt`

git add . # Add the `first.txt` file
git commit -m "adding first" # Commit with the message "Adding first.txt"

echo WRONG > wrong.txt # Add the text "WRONG" to `wrong.txt`
git add . # Add the `wrong.txt` file
git commit -m "adding WRONG to wrong.txt" # Commit with the message "Adding WRONG to wrong.txt"

echo More >> first.txt # Add the text "More" to `first.txt`
git add . # Add the `first.txt` file
git commit -m "adding More to first.txt" # Commit with the message "Adding More to first.txt"

echo Even More >> first.txt # Add the text "Even More" to `first.txt`
git add . # Add the `first.txt` file
git commit -m "adding Even More to First.txt" # Commit with the message "Adding More to first.txt"

# OH NO! We want to undo the commit with the text "WRONG" - let's revert! Since this commit was 2 from where we are not we can use git revert HEAD~2 (or we can use git log and find the SHA of that commit)

git revert HEAD~2 # this will put us in a text editor where we can modify the commit message.

ls # wrong.txt is not there any more!
git log --oneline # note that the commit history hasn't been altered, we've just added a new commit reflecting the removal of the `wrong.txt`
```

这样你就离获得 Git 黑带又近了一步。