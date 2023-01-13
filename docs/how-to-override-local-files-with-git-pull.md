# 如何用 Git Pull 覆盖本地文件

> 原文：<https://www.freecodecamp.org/news/how-to-override-local-files-with-git-pull/>

# 什么时候需要覆盖本地文件？

如果您觉得需要放弃所有本地更改，而只是用远程分支的副本来重置/覆盖所有内容，那么您应该遵循这个指南。

重要提示:如果您有任何局部更改，它们将会丢失。无论有没有`--hard`选项，任何没有被推送的本地提交都将丢失。

如果您有任何文件没有被 Git 跟踪(例如，上传的用户内容)，这些文件将不会受到影响。

## **覆盖工作流程:**

要覆盖本地文件，请执行以下操作:

```
git fetch --all
git reset --hard <remote>/<branch_name>
```

例如:

```
git fetch --all
git reset --hard origin/master
```

## **工作原理:**

`git fetch`从远程下载最新版本，无需尝试合并或重置任何内容。

然后 git 重置将主分支重置为您刚刚获取的内容。`--hard`选项改变工作树中的所有文件，以匹配`origin/master`中的文件。

## **附加信息:**

值得注意的是，可以通过从`master`创建一个分支或者在重置之前您想要处理的任何分支来维护当前的本地提交:

例如:

```
git checkout master
git branch new-branch-to-save-current-commits
git fetch --all
git reset --hard origin/master
```

在此之后，所有旧的提交都将保存在`new-branch-to-save-current-commits`中。然而，未提交的更改(即使是暂存的)将会丢失。一定要把你需要的东西藏起来并拿出来。

### 归属:

*本文基于一个堆栈溢出问题<a href = '[http://Stack Overflow . com/questions/1125968/force-git-to-overwrite-local-files-on-pull/8888015 # 8888015](http://stackoverflow.com/questions/1125968/force-git-to-overwrite-local-files-on-pull/8888015#8888015)' target = '*blank ' rel = ' no follow '>here _