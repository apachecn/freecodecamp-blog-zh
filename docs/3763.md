# Git Stash 解释:如何在 Git 中临时存储本地更改

> 原文：<https://www.freecodecamp.org/news/git-stash-explained/>

Git 有一个称为 stash 的区域，您可以在这里临时存储更改的快照，而无需将它们提交到存储库。它独立于工作目录、临时区域或存储库。

当您对一个分支进行了更改，但还没有准备好提交，但是您需要切换到另一个分支时，这个功能非常有用。

### 隐藏变化

要在存储中保存您的更改，请运行以下命令:

```
git stash save "optional message for yourself"
```

这将保存您的更改，并将工作目录恢复到最近一次提交时的样子。隐藏的变更可以从存储库中的任何分支获得。

请注意，您想要隐藏的更改需要在被跟踪的文件上。如果您创建了一个新文件并试图隐藏您的更改，您可能会得到错误`No local changes to save`。

### 查看隐藏的更改

要查看您的存储中有什么，请运行以下命令:

```
git stash list
```

这将返回格式为`stash@{0}: BRANCH-STASHED-CHANGES-ARE-FOR: MESSAGE`的已保存快照列表。`stash@{0}`部分是藏匿点的名称，花括号中的数字(`{ }`)是藏匿点的索引。如果您隐藏了多个变更集，那么每一个都将有一个不同的索引。

如果您忘记了在存储中做了什么更改，您可以使用`git stash show NAME-OF-STASH`查看它们的摘要。如果您想查看典型的 diff 风格的补丁布局(用+和-表示逐行修改)，您可以包含`-p`(表示补丁)选项。这里有一个例子:

```
git stash show -p stash@{0}

# Example result:
diff --git a/PathToFile/fileA b/PathToFile/fileA
index 2417dd9..b2c9092 100644
--- a/PathToFile/fileA
+++ b/PathToFile/fileA
@@ -1,4 +1,4 @@
-What this line looks like on branch
+What this line looks like with stashed changes
```

### 检索隐藏的更改

要从存储中检索更改并将其应用到您所在的当前分支，您有两种选择:

1.  应用更改并在存储中留下一份副本
2.  `git stash pop STASH-NAME`应用更改并从存储中移除文件

应用更改时可能会有冲突。你可以解决类似于合并的冲突([详见`git merge`](https://www.freecodecamp.org/news/the-ultimate-guide-to-git-merge-and-git-rebase/))。

### 删除隐藏的更改

如果要删除隐藏的更改而不应用它们，请运行命令:

```
git stash drop STASH-NAME
```

要清除全部存储，请运行以下命令:

```
git stash clear
```