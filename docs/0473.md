# 错误:src refspec 主文件与任何主文件都不匹配–如何在 Git 中修复

> 原文：<https://www.freecodecamp.org/news/error-src-refspec-master-does-not-match-any-how-to-fix-in-git/>

使用 Git 时，您可能会遇到一个错误，提示“src refspace master 不匹配任何内容”。

以下是错误的含义和解决方法。

## `src refspec master does not match any`在 Git 中是什么意思？

当您尝试触发从本地资料库到主资料库的推送时，可能会出现此错误，如下所示:

```
git push origin master 
```

出现此错误的原因有多种。

出现此错误最可能的原因是`master`分支不存在。

也许您克隆了一个新的存储库，默认的分支是`main`，所以当您试图推进它时，没有主分支。

您可以使用如下的`git branch -b`命令显示连接到本地存储库的远程分支:

```
git branch -b

# results
#  origin/main
#  origin/feat/authentication
#  origin/other branches ... 
```

有了上面的结果，可以看到没有`master`资源库(`origin/master`)。因此，当您尝试推送到该存储库时，您将得到“respec 错误”。

这个结果也适用于任何其他不存在的分支。比方说，我做了一些更改，并将其推送到一个不存在的远程`hello`分支:

```
git add .
git commit -m "new changes"
git push origin hello 
```

该命令将产生以下错误:

```
error: src refspec hello does not match any 
```

## 如何修复“src refspec master 不匹配任何”错误

现在您知道了`master`分支并不存在。这个错误的解决方案是创建一个本地和远程的`master`分支，您可以将提交推送到这个分支，或者将提交推送到一个现有的分支——可能是`main`。

您可以在 Git 托管的网站(如 GitHub)上创建一个远程`master`分支，或者您可以直接从您的终端这样做:

```
git checkout -b master

# add commit

git push origin master 
```

这些命令将在本地创建一个`master`分支。通过推送到`origin master`，还将远程创建`master`分支。

但是如果您不想创建一个`master`分支，您可以使用现有的默认分支(可能是`main`)来代替。

## 包扎

因此，如果您在尝试推送到 master 时得到了`Error: src refspec master does not match any`错误，最可行的原因是`master`分支不存在。