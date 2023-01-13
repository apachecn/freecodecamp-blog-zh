# Git 日志命令解释

> 原文：<https://www.freecodecamp.org/news/git-log-command/>

## **git log 是做什么的？**

`git log`命令显示存储库历史中的所有提交。

默认情况下，该命令显示每个提交的:

*   安全哈希算法(SHA)
*   作者
*   日期
*   提交消息

## 导航 Git 日志

Git 使用非终端分页器来分页查看提交历史。您可以使用以下命令导航它:

*   若要向下滚动一行，请使用 j 或↓
*   若要向上滚动一行，请使用 k 或↑
*   要向下滚动一页，请使用空格键或 Page Down 按钮
*   要向上滚动一页，请使用 b 或 Page Up 按钮
*   要退出日志，请使用 q

## Git 日志标志

您可以使用标志自定义由`git log`显示的信息。

### -单线

`git log --oneline`

`--oneline`标志使`git log`显示

*   每行一次提交
*   SHA 的前七个字符
*   提交消息

### 表示“稳定器”:thermostat | haemostat

`git log --stat`

`--stat`标志使`git log`显示

*   每次提交时修改的文件
*   添加或删除的行数
*   包含更改的文件和行的总数的摘要行

### - patch 或-p

`git log --patch`

或者，更简短的版本

`git log -p`

`--patch`标志使`git log`显示

*   你修改的文件
*   添加或删除的线条的位置
*   你所做的具体改变

## 按作者查看指定的提交次数

要查看作者对当前 repo 的指定数量的提交(可选地以修饰的格式)，可以使用下面的命令

`git log --pretty=format:"%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset" -n {NUMBER_OF_COMMITS} --author="{AUTHOR_NAME}" --all`

### 从特定提交开始

要在特定提交时启动`git log`,请添加 SHA:

`git log 7752b22`

这将显示使用 SHA 7752b22 的提交以及在该提交之前进行的所有提交。您可以将它与任何其他标志结合使用。

### 表示“以某方式写的东西”: autograph

`git log --graph`

`--graph`标志使您能够以图表形式查看您的`git log`。为了让事情变得有趣，你可以把这个命令和你从上面学到的`--oneline`选项结合起来。

`git log --graph --oneline`

输出将类似于，

```
* 64e6db0 Update index.md
* b592012 Update Python articles (#5030)
* ecbf9d3 Add latest version and remove duplicate link (#8860)
* 7e3934b Add hint for Compose React Components (#8705)
* 99b7758 Added more frameworks (#8842)
* c4e6a84 Add hint for "Create a Component with Composition" (#8704)
*   907b004 Merge branch 'master' of github.com:freeCodeCamp/guide
|\  
| * 275b6d1 Update index.md
* |   cb74308 Merge branch 'dogb3rt-patch-3'
|\ \  
| |/  
|/|   
| *   98015b6 fix merge conflicts after folder renaming
| |\  
|/ /  
| * fa83460 Update index.md
* | 6afb3b5 rename illegally formatted folder name (#8762)
* | 64b1fe4 CSS3: border-radius property (#8803)
```

使用这个命令的好处之一是，它使您能够了解提交是如何合并的以及 git 历史是如何创建的。

还有许多其他选项可以与`--graph`结合使用。其中有几个是`--decorate`和`--all`。一定要试一试这些。你可以参考[文档](https://git-scm.com/docs/git-log)获得更多有用的信息。

### 更多信息:

*   [Git 基础——查看提交历史](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)
*   [去日志](https://git-scm.com/docs/git-log)

## **Git 上的其他资源**

*   [Git Checkout](https://www.freecodecamp.org/news/git-checkout-explained/)
*   [去委员会](https://www.freecodecamp.org/news/git-commit-command-explained/)
*   [Git Stash](https://www.freecodecamp.org/news/git-stash-explained/)
*   [去分支](https://www.freecodecamp.org/news/git-delete-branch-how-to-remove-a-local-or-remote-branch/)