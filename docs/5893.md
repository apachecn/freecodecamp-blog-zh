# 现在你不再害怕 GIT 了，下面是如何利用你所知道的东西

> 原文：<https://www.freecodecamp.org/news/now-that-youre-not-afraid-of-git-anymore-here-s-how-to-leverage-what-you-know-11e710c7f37b/>

本系列的第一部分介绍了 Git 的内部工作方式，并向您展示了如何不害怕使用 GIT。

既然我们理解了 Git 是如何工作的，那么让我们进入实质性的内容:如何在我们的项目中利用我们所知道的东西。

### 合并

合并*合并*你的代码。

还记得我们是如何遵循良好的 Git 实践，为我们正在开发的各种特性建立分支，而不是`master`上的所有东西吗？总有一天你会完成这个功能，并希望将它包含在你的`master`中。这就是`merge`的用武之地。你想**将**你的分公司并入主公司。

有两种合并:

#### 快进合并

回到我们上次的例子:

这就像将标签从`master`移动到`the-ending`一样简单。Git 毫不怀疑到底需要做什么——因为我们的“树”只有一个节点链表。

```
$ git branch
  master
* the-ending
$ git checkout master
Switched to branch 'master'
$ git merge the-ending
Updating a39b9fd..b300387
Fast-forward
 byeworld | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 byeworld
```

#### 非快进合并

这就是 Git 不知道该怎么办的那种合并。在基础分支上有一些变化，在我们想要合并的分支上有更多的变化，因此导致了可怕的*合并冲突*！

关于合并冲突，首先要知道:如果你不知道发生了什么:

```
git merge --abort
```

这会让你回到原来的状态，没有副作用。你刚刚中止了你要制造的混乱。

![HAWGpuc2gjD5wYOk9UbaPNorlFq4dXA5FRFU](img/58cb8ca5989c92c82b59eb9e8f7d76b3.png)

[You don’t want to be Brian?](http://www.quickmeme.com/p/3vzhql)

现在让我们一步一步地了解如何解决合并冲突。

```
$ git checkout -b the-middle
Switched to a new branch 'the-middle'
```

继续我们的风格，让我们通过一个例子来学习。我在分支`the-middle`上修改`helloworld`。

```
$ git diff
diff --git a/helloworld b/helloworld
index a042389..e702052 100644
--- a/helloworld
+++ b/helloworld
@@ -1 +1,3 @@
 hello world!
+
+Middle World
```

在`the-middle`添加并提交。

然后，我切换到`master`，在 master 上修改`helloworld`。我补充如下:

```
$ git diff --cached
diff --git a/helloworld b/helloworld
index a042389..ac7a733 100644
--- a/helloworld
+++ b/helloworld
@@ -1 +1,3 @@
 hello world!
+
+Master World
```

你明白我为什么要在这里做`git diff --cached`了吗？如果没有，下面问我！

现在，是时候合并了！

```
$ git merge the-middle
Auto-merging helloworld
CONFLICT (content): Merge conflict in helloworld
Automatic merge failed; fix conflicts and then commit the result.
```

当一个`merge`失败时，git 会这样做:它用 merge 修改文件，向您显示它无法决定的确切内容。

```
$ cat helloworld 
hello world!
```

```
$ cat helloworld 
hello world!
<<<<<<< HEAD
Master World
=======
Middle World
>>>>>>> the-middle
```

这有道理吗？`<<<<< HEAD`部分是我们的(基层分公司)`>>>>> the-middle part`是`theirs`(并入基层分公司的分公司)。

您可以简单地编辑文件，删除 git 添加的额外内容，并选择最终应该放入`helloworld`的内容。有一些工具和编辑器集成可以使这变得更容易，但我认为当你没有你最喜欢的编辑器时，知道它是如何工作的会给你更多的信心。

```
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)
Unmerged paths:
  (use "git add <file>..." to mark resolution)
both modified:   helloworld
```

我决定保留这两部分。

```
$ cat helloworld 
hello world!
Master World
Middle World
```

现在你知道了:

```
$ git add helloworld 
$ git commit -m "resolve merge conflict"
[master c747e68] resolve merge conflict
```

### Remotes

由于版本源代码控制的一个强大功能是在发生灾难时保存您的代码——远程设备在此提供帮助。远程是您的 git 存储库的外部托管副本。更准确地说，remote 是一个外部存储库(不一定与您拥有的代码相同)。就外部而言，它可能在您系统上的不同文件夹中，也可能在云中。

#### 克隆

克隆*将存储库从远程克隆*到您当前的工作目录中。这只是创建了一个`.git/`文件夹的副本，它为我们提供了填充工作目录所需的全部历史和文件。

```
git clone <repository-url>
```

如果你没有克隆，你可能没有遥控器。您可以创建这样的遥控器:

```
git remote add <name> <url>
```

### 推拉

推和拉是施加在`remote`上的动作。

按下*按钮将*您的更改推至遥控器。因此，我们从对象存储中发送`Index`和相应的`Objects`！

```
git push <name of remote> <name of branch>
```

拉*从遥控器拉*代码。与之前完全一样，我们从对象存储中复制了`Index`和相应的`Objects`！

```
git pull origin master
```

`origin`是遥控器的默认名称。由于`master`是默认的分支，您可以看到这个命令是如何转化成我们随处可见的简单名称:`git pull origin master`。现在你更清楚了。

### 重置

重置*将你的代码库重置为之前的版本。复位带有 3 个标志:*

`--soft`、`--hard`和`--mixed`。

`reset`的美妙之处在于能够改变历史。假设你用一个`commit`犯了一个错误，现在你的`git log`被这样的行为搞得一团糟:

`Bugfix`

`Final BugFix`

`Final Final BugFix`

`God why isn't this working last try bug fix`

如果您想保持您的`master`历史干净，您需要清理这个提交日志。

如果你在没有挤压的地方发送一个拉请求，他们也会期望一个干净的提交历史！

这就是`reset`的用武之地:你可以`reset`所有的提交，并将它们转换成一个单独的提交:`got stuff done!`

(请不要将此作为您的提交信息，请遵循最佳实践！)

回到我们的例子，这是我所做的。

```
$ git log
commit 959781ec78c970d4797c5e938ec154de44d0151b (HEAD -> master)
Author: Neil Kakkar
Date:   Mon Nov 5 07:32:55 2018 +0000
God why isn't this working last final BugFix
commit affa90c0db78999d22c326fdbd6c1d5057228822
Author: Neil Kakkar
Date:   Mon Nov 5 07:32:19 2018 +0000
Final Final BugFix
commit 2e9570cffc0a8206132d75c402d68351eda450bd
Author: Neil Kakkar
Date:   Mon Nov 5 07:31:49 2018 +0000
Final BugFix
commit 4560fc0ec6305d0b7bcfb4be1901438fd126d6d1
Author: Neil Kakkar
Date:   Mon Nov 5 07:31:21 2018 +0000
BugFix
commit c747e6891af419119fd817dc69a2e122084aedae
Merge: 3d01508 fb8b2fc
Author: Neil Kakkar
Date:   Tue Oct 23 07:44:09 2018 +0100
resolve merge conflict
```

既然 bug 已经修复，我想在推送到`master`之前清理一下我的历史。这也很好——比如说，当我后来意识到我引入了另一个 bug，需要恢复到以前的版本时。在这种情况下，`c747e689`没有最好的提交消息来理解这一点。

```
$ git reset c747e6891af419119fd817dc69a2e122084aedae
$ git log
commit c747e6891af419119fd817dc69a2e122084aedae (HEAD -> master)
Merge: 3d01508 fb8b2fc
Author: Neil Kakkar
Date:   Tue Oct 23 07:44:09 2018 +0100
resolve merge conflict
```

好了，都整理好了吗？

```
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
      clean.txt
nothing added to commit but untracked files present (use "git add" to track)
```

`clean.txt`是我提交的用于修复 bug 的文件。现在，我要做的就是:

```
$ git add clean.txt 
$ git commit -m "fix bug: Unable to clean folder"
[master d8487ca] fix bug: Unable to clean folder
 1 file changed, 4 insertions(+)
 create mode 100644 clean.txt
$ git log
commit d8487ca8b9acfa9666bdf2c6b7fa27b3971bd957 (HEAD -> master)
Author: Neil Kakkar
Date:   Mon Nov 5 07:41:41 2018 +0000
fix bug: Unable to clean folder
commit c747e6891af419119fd817dc69a2e122084aedae
Merge: 3d01508 fb8b2fc
Author: Neil Kakkar
Date:   Tue Oct 23 07:44:09 2018 +0100
resolve merge conflict
```

好了，搞定了。你现在能猜出，使用来自`log`、`reset`命令语法和你的技术感觉的线索来找出它是如何在幕后工作的吗？

在指定的提交处切断提交树。该分支的所有标签(如果在前面)都被移回到指定的提交。但是现有的文件会保留在对象存储中吗？你现在知道怎么检查了吧，Ace。

这些文件也将从暂存区中删除。现在，如果您有许多不想添加的未跟踪/修改的文件，这可能会成为一个问题。

你是怎么做到的？

你能找到我在这一节开头留下的线索吗？

行为标志！

`--soft`保存所有文件。

```
$ git reset --soft c747e6891af419119fd817dc69a2e122084aedae
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
new file:   clean.txt
```

`--mixed`是默认值:也从临时区域删除所有文件。

`--hard`是铁杆。从对象存储和目录中删除文件。极其小心地使用。我的 bug 修复程序泡汤了。不见了。

```
$ git reset --hard c747e6891af419119fd817dc69a2e122084aedae
HEAD is now at c747e68 resolve merge conflict
$ git status
On branch master
nothing to commit, working tree clean
```

*嗯，不完全是。Git 太神奇了。你听说过元元数据吗？存储库中发生的事情的冗余日志？对，当然 git 留着！

```
$ git reflog
c747e68 (HEAD -> master) HEAD@{0}: reset: moving to c747e6891af419119fd817dc69a2e122084aedae
efc6d21 HEAD@{1}: commit: soft reset
c747e68 (HEAD -> master) HEAD@{2}: reset: moving to c747e6891af419119fd817dc69a2e122084aedae
d8487ca HEAD@{3}: commit: fix bug: Unable to clean folder
c747e68 (HEAD -> master) HEAD@{4}: reset: moving to c747e6891af419119fd817dc69a2e122084aedae
959781e HEAD@{5}: commit: God why isn't this working last final BugFix
affa90c HEAD@{6}: commit: Final Final BugFix
2e9570c HEAD@{7}: commit: Final BugFix
4560fc0 HEAD@{8}: commit: BugFix
c747e68 (HEAD -> master) HEAD@{9}: commit (merge): resolve merge conflict
3d01508 HEAD@{10}: commit: add Master World
b300387 (the-ending) HEAD@{11}: checkout: moving from the-middle to master
fb8b2fc (the-middle) HEAD@{12}: commit: add Middle World
b300387 (the-ending) HEAD@{13}: checkout: moving from master to the-middle
b300387 (the-ending) HEAD@{14}: checkout: moving from the-middle to master
b300387 (the-ending) HEAD@{15}: checkout: moving from master to the-middle
b300387 (the-ending) HEAD@{16}: merge the-ending: Fast-forward
a39b9fd HEAD@{17}: checkout: moving from the-ending to master
b300387 (the-ending) HEAD@{18}: checkout: moving from master to the-ending
a39b9fd HEAD@{19}: checkout: moving from the-ending to master
b300387 (the-ending) HEAD@{20}: commit: add byeworld
a39b9fd HEAD@{21}: checkout: moving from master to the-ending
a39b9fd HEAD@{22}: commit (initial): Add helloworld
```

这是从上一篇文章的例子开始的所有内容。这是否意味着，如果我犯了一个可怕的错误，我可以恢复事情吗？

```
$ git checkout d8487ca
Note: checking out 'd8487ca'.
You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.
If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:
git checkout -b <new-branch-name>
HEAD is now at d8487ca... fix bug: Unable to clean folder

$ ls
byeworld clean.txt  helloworld
```

这就是了。

恭喜你，你现在是一个饭桶忍者——学徒。

你还有什么想知道的吗？你对 Git 有什么困惑吗？下面让我知道！我会试着用我学的方法解释它！

喜欢这个吗？不要再错过任何帖子了——订阅我的邮件列表吧！