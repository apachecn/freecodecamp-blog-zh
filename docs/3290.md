# 如何将您的 Fork 与原始 Git 存储库同步

> 原文：<https://www.freecodecamp.org/news/how-to-sync-your-fork-with-the-original-git-repository/>

您正在为一个开源项目做贡献，并且您注意到您的 fork 与原始存储库不同步。你如何纠正这一点？

## TL；灾难恢复版本

```
# Add a new remote upstream repository
git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git

# Sync your fork
git fetch upstream
git checkout master
git merge upstream/master
```

## 添加新的远程上游储存库

你在笔记本电脑上克隆了你的叉子，并开始解决你的问题。

你知道你的叉子是个孤儿吗？如果您列出已配置的远程存储库，您将只看到您的分支作为源:

```
git remote -v
origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
```

没有父母的迹象！原始存储库在哪里？

我们需要配置此信息，通过添加新的远程上游存储库来恢复家族关系:

```
git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
```

你拯救了这个家庭！现在您可以看到原始存储库和 fork:

```
git remote -v
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
```

## 同步您的叉子

现在一切都准备好了。你可以只用两个命令来同步你的 fork。

确保您在项目的根目录中，并且在主分支中。否则，您可以签出到主分支机构:

```
git checkout master
Switched to branch 'master'
```

现在，您必须从原始存储库中获取变更:

```
git fetch upstream
remote: Enumerating objects: 16, done.
remote: Counting objects: 100% (16/16), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 7 (delta 5), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (7/7), 1.72 Kio | 160.00 Kio/s, done.
From https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY
   909ef5a..0b228a8  master     -> upstream/master
```

并合并主分支中的更改:

```
git merge upstream/master
Updating 909ef5a..0b228a8
Fast-forward
 node.js/WorkingWithItems/batch-get.js               | 51 ++++++++++++++++++++++++++------------------------
 node.js/WorkingWithItems/batch-write.js             | 95 +++++++++++++++++++++++++++++++++++++++++++++++----------------------------------------------
 node.js/WorkingWithItems/delete-item.js             | 37 ++++++++++++++++++------------------
 node.js/WorkingWithItems/get-item.js                | 31 +++++++++++++++++--------------
 node.js/WorkingWithItems/put-item-conditional.js    | 51 +++++++++++++++++++++++++-------------------------
 node.js/WorkingWithItems/put-item.js                | 49 ++++++++++++++++++++++++------------------------
 node.js/WorkingWithItems/transact-get.js            | 51 ++++++++++++++++++++++++++------------------------
 node.js/WorkingWithItems/transact-write.js          | 79 ++++++++++++++++++++++++++++++++++++++++-------------------------------------
 node.js/WorkingWithItems/update-item-conditional.js | 51 ++++++++++++++++++++++++++------------------------
 node.js/WorkingWithItems/update-item.js             | 47 ++++++++++++++++++++++++----------------------
 10 files changed, 282 insertions(+), 260 deletions(-)
```

就是这样！您的 fork 现在是最新的。

有什么问题吗？请随时在推特上联系我！