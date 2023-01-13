# Git 标签解释:如何在 Git 中列出、创建、删除和显示标签

> 原文：<https://www.freecodecamp.org/news/git-tag-explained-how-to-add-remove/>

标记允许开发人员在项目开发过程中标记重要的检查点。例如，软件发布版本可以被标记。(例如:v1.3.2)它本质上允许你给一个提交一个特殊的名字(标签)。

要按字母顺序查看所有创建的标签:

```
git tag
```

要获得标签的更多信息:

```
git show v1.4
```

有两种类型的标签:

带注释的

```
git tag -a v1.2 -m "my version 1.4"
```

轻量级选手

```
git tag v1.2
```

它们的储存方式不同。这些在你当前的提交上创建标签。

如果您想要标记以前的提交，请指定您想要标记的提交 ID:

```
git tag -a v1.2 9fceb02
```

在签出提交并将提交推送到远程存储库时，可以使用标记名称来代替提交 id。

#### **更多信息:**

*   Git 文档:[文档](https://git-scm.com/docs/git-tag)
*   Git 标签章节:[书](https://git-scm.com/book/en/v2/Git-Basics-Tagging)

你可以用`git tag`命令列出一个项目中所有可用的标签(它们将按字母顺序出现):

```
$ git tag
v1.0
v2.0
v3.0
```

这种列出标签的方式非常适合小项目，但更大的项目可能会有数百个标签，因此在搜索历史中的重要点时，您可能需要过滤它们。您可以找到包含特定字符的标签，将`-l`添加到`git tag`命令中:

```
$ git tag -l "v2.0*"
v2.0.1
v2.0.2
v2.0.3
v2.0.4
```

## **创建标签**

您可以创建两种类型标签:带注释的和轻量级的。第一个是 GIT 数据库中的竞争对象:它们被校验和，请求一条消息(比如提交),并存储其他重要数据，比如姓名、电子邮件和日期。另一方面，轻量级标签不需要消息或存储其他数据，只是作为指向项目中特定点的指针。

### **创建一个带注释的标签**

要创建带注释的标签，将`-a tagname -m "tag message"`添加到`git tag`命令中:

```
$ git tag -a v4.0 -m "release version 4.0"
$ git tag
v1.0
v2.0
v3.0
v4.0
```

如您所见，`-a`指定您正在创建一个带注释的标记，后面是标记名，最后是`-m`，后面是要存储在 Git 数据库中的标记消息。

### **创建一个轻量级标签**

轻量级标记只包含提交校验和(不存储其他信息)。要创建一个标签，只需运行没有任何其他选项的`git tag`命令(名称末尾的-lw 字符用于表示轻量级标签，但是您可以随意标记它们):

```
$ git tag v4.1-lw
$ git tag
v1.0
v2.0
v3.0
v4.0
v4.1-lw
```

这次您没有指定消息或其他相关数据，所以标记只包含引用提交的校验和。

## **查看标签的数据**

您可以运行`git show`命令来查看存储在标签中的数据。对于带注释的标记，您将看到标记数据和提交数据:

```
$ git show v4.0
tag v4.0
Tagger: John Cash <john@cash.com>
Date:   Mon Sat 28 15:00:25 2017 -0700

release version 4.0

commit da43a5fss745av88d47839247990022a98419093
Author: John Cash <john@cash.com>
Date:   Fri Feb 20 20:30:05 2015 -0700

  finished details
```

如果您正在监视的标记是轻量级标记，您将只能看到引用的提交数据:

```
$ git show v1.4-lw
commit da43a5f7389adcb9201ab0a289c389ed022a910b
Author: John Cash <john@cash.com>
Date:   Fri Feb 20 20:30:05 2015 -0700

  finished details
```

## **标记旧提交**

您还可以使用 git 标记 commit 来标记过去的提交。为此，您需要在命令行中指定提交的校验和(或者至少是它的一部分)。

首先，运行 git log 来找出所需提交的校验和:

```
$ git log --pretty=oneline
ac2998acf289102dba00823821bee04276aad9ca added products section
d09034bdea0097726fd8383c0393faa0072829a7 refactorization
a029ac120245ab012bed1ca771349eb9cca01c0b modified styles
da43a5f7389adcb9201ab0a289c389ed022a910b finished details
0adb03ca013901c1e02174924486a08cea9293a2 small fix in search textarea styles
```

当您需要校验和时，将其添加到标记创建行的末尾:

```
$ git tag -a v3.5 a029ac
```

运行`git tag`，您将看到标签被正确添加:

```
$ git tag
v1.0
v2.0
v3.0
v3.5
v4.0
v4.1-lw
```

## **推送标签**

当您运行 Git push 命令时，默认情况下 git 不会推送标签。因此，要成功地将标签推送到服务器，您必须使用`git push origin`命令:

```
$ git push origin v4.0
Counting objects: 14, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (16/16), done.
Writing objects: 100% (18/18), 3.15 KiB | 0 bytes/s, done.
Total 18 (delta 4), reused 0 (delta 0)
To git@github.com:jcash/gitmanual.git
 * [new tag]         v4.0 -> v4.0
```

您也可以使用`--tags`选项通过`git push origin`命令一次添加多个标签:

```
$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 160 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:jcash/gitmanual.git
 * [new tag]         v4.0 -> v4.0
 * [new tag]         v4.1-lw -> v4.1-lw
```

## **检查标签**

您可以像平常一样使用`git checkout`来签出标签。但是你需要记住，这将导致一个*分离头*状态。

```
$ git checkout v0.0.3
Note: checking out 'v0.0.3'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.
```

## **删除标签**

您可能会发现想要删除某个标签的情况。对于这种情况，有一个非常有用的命令:

```
$ git tag --delete v0.0.2
$ git tag
v0.0.1
v0.0.3
v0.0.4
```

### **更多信息**

*   [Git 专业标签基础知识](https://git-scm.com/book/en/v2/Git-Basics-Tagging)
*   [Git 专业文档](https://git-scm.com/docs/git-tag)
*   [Git HowTo](https://githowto.com/tagging_versions)
*   [Git 提示:标签](http://alblue.bandlem.com/2011/04/git-tip-of-week-tags.html)
*   [创建标签](https://www.drupal.org/node/1066342)

### **来源**

Git 文档:[标签](https://git-scm.com/book/en/v2/Git-Basics-Tagging)