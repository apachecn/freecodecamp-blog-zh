# 如何不再害怕 GIT

> 原文：<https://www.freecodecamp.org/news/how-not-to-be-afraid-of-git-anymore-fe1da7415286/>

#### 了解消除不确定性的机制

![2GCBfaz1YnEnmhZzKldeQe4hvhk0OnUXOOSU](img/762f5afb7a0d4c8239437f703c8c8d0a.png)

Been here before? ([web comic by XKCD](https://imgs.xkcd.com/comics/git.png))

#### Git 到底是什么？

"这是一个版本控制系统."

#### 我为什么需要它？

"去版本控制，傻瓜。"

好吧，好吧，我还没帮上什么忙。基本思想是这样的:随着项目变得太大，有太多的参与者，跟踪谁在什么时候做了什么变得不可能。有人引入了打破整个系统的改变吗？你怎么知道变化是什么？你怎么回到以前的样子？回到完整的仙境？

我会更进一步——比方说不是一个有很多贡献者的项目，只是一个你作为创建者、维护者和发布者的小项目:你为这个项目创建了一个新的特性，它引入了你后来发现的微妙的 bug。您不记得为了创建这个新特性，对现有代码库做了哪些更改。问题？

所有这些问题的答案就是版本控制！拥有你编码的所有东西的版本可以确保你知道从项目开始以来，谁做了修改，什么修改，在哪里修改！

现在，我邀请你停止把它想成一个黑盒子，打开它，找出等待你的宝藏。弄清楚 Git 是如何工作的，你就不会再有任何问题了。一旦你经历了这些，我保证你会意识到做上面 XKCD 漫画所说的事情是多么愚蠢。这正是版本控制试图阻止的。

### 怎么 Git？

我假设您知道 Git 中的基本命令，或者听说过它们，并且至少使用过一次。如果没有，这里有一个基本词汇可以帮助你开始。

**储藏室**:存放东西的地方。对于 Git，这意味着您的代码文件夹

**head:** 一个指向你正在处理的最新代码的“指针”

**添加:**请求 Git 跟踪文件的动作

**提交:**保存当前状态的动作——以便在需要时可以重新访问该状态

**远程:**非本地的存储库。可以在另一个文件夹或云中(例如:Github):帮助其他人轻松协作，因为他们不必从您的系统中获取副本，他们只需从云中获取即可。此外，确保你有一个备份，以防你打破了你的笔记本电脑

**pull:** 从远程获取更新代码的动作

**push:** 向遥控器发送更新代码的动作

**合并:**合并两个不同版本代码的动作

**状态:**显示关于当前存储库状态的信息

#### 去哪里 Git？

介绍由隐藏文件夹控制的魔法:`.git/`

在每个 git 存储库中，您都会看到类似这样的内容

```
$ tree .git/
.git/
├── HEAD
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   └── update.sample
├── info
│   └── exclude
├── objects
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags
8 directories, 14 files
```

这就是 Git 控制和管理整个项目的方式。我们将逐一讨论所有重要的部分。

Git 由 3 部分组成:对象存储、索引和工作目录。

#### 对象存储

Git 就是这样在内部存储所有东西的。对于项目中的每一个文件，Git 都会为该文件生成一个散列，并将该文件存储在该散列下。例如，如果我现在创建一个`helloworld`文件并执行`git add helloworld`(告诉 git 将名为`helloworld`的文件添加到 Git 对象存储中)，我会得到如下结果:

```
$ tree .git/
.git/
├── HEAD
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── objects
│   ├── a0
│   │   └── 423896973644771497bdc03eb99d5281615b51
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags
9 directories, 16 files
```

一个新的对象已经生成！对于那些感兴趣的人来说，Git 在内部使用了 [hash-object](https://git-scm.com/docs/git-hash-object) 命令，如下所示:

```
$ git hash-object helloworld
a0423896973644771497bdc03eb99d5281615b51
```

是的，它与我们在 objects 文件夹下看到的散列相同。为什么子目录中有 hash 的前两个字符？它使搜索速度更快。

然后，Git 创建一个名为上述散列的对象，压缩我们给定的文件并存储在那里。因此，你实际上也可以看到对象的内容！

```
$ git cat-file a0423896973644771497bdc03eb99d5281615b51 -p
hello world!
```

这些都在引擎盖下。你永远不会使用[猫档案](https://git-scm.com/docs/git-cat-file)在日常补充。您只需简单地`add`，让 Git 处理剩下的事情。

这是我们的第一个 Git 命令。

`**git add**` **创建散列，压缩文件，并将压缩后的对象添加到对象存储中。**

#### 工作目录

顾名思义，这就是你工作的地方。您创建和编辑的所有文件都在工作目录中。我创建了一个新文件`byeworld`并运行`git status`:

```
$ git status
On branch master
No commits yet
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
new file:   helloworld
Untracked files:
  (use "git add <file>..." to include in what will be committed)
byeworld
```

未跟踪的文件是工作目录中我们没有要求 git 管理的文件。

如果我们在工作目录中没有做任何事情，我们会得到下面的消息:

```
$ git status
On branch master
nothing to commit, working tree clean
```

我很确定你现在明白了。现在忽略分支并提交。关键是，工作树(目录)是干净的。

#### 索引

这是 Git 的核心。也被称为集结地。索引存储文件到对象存储中的对象的映射。这就是`commits`的用武之地。看到这一点的最好方法是测试它！

让我们提交我们添加的文件`helloworld`

```
$ git commit -m "Add helloworld"
[master (root-commit) a39b9fd] Add helloworld
 1 file changed, 1 insertion(+)
 create mode 100644 helloworld
```

回到我们的树:

```
$ tree .git/
.git/
├── COMMIT_EDITMSG
├── HEAD
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── logs
│   ├── HEAD
│   └── refs
│       └── heads
│           └── master
├── objects
│   ├── a0
│   │   └── 423896973644771497bdc03eb99d5281615b51
│   ├── a3
│   │   └── 9b9fdd624c35eee08a36077f411e009da68c2f
│   ├── fb
│   │   └── 26ca0289762a454db2ef783c322fedfc566d38
│   ├── info
│   └── pack
└── refs
    ├── heads
    │   └── master
    └── tags
14 directories, 22 files
```

啊，有意思！我们的对象库中有两个新对象，在日志和引用中还有一些我们不了解的东西。回到我们的朋友`cat-file`:

```
$ git cat-file a39b9fdd624c35eee08a36077f411e009da68c2f -p
tree fb26ca0289762a454db2ef783c322fedfc566d38
author = <=> 1537700068 +0100
committer = <=> 1537700068 +0100
Add helloworld
$ git cat-file fb26ca0289762a454db2ef783c322fedfc566d38 -p
100644 blob a0423896973644771497bdc03eb99d5281615b51 helloworld
```

正如您所猜测的，第一个对象是提交元数据:谁做了什么以及为什么，用一棵树表示。第二个对象是实际的树。如果你了解 unix 文件系统，你就会确切地知道这是什么。

Git 中的`tree`对应的是 Git 文件系统。所有的东西要么是树(目录)，要么是 blob(文件),每次提交时，Git 也会存储树信息，告诉自己:这就是工作目录此时应该呈现的样子。注意，树指向它包含的每个文件的特定对象(散列)。

是时候说说**分店**了！我们的第一次提交也给`.git/`添加了一些其他的东西。我们现在感兴趣的是`.git/refs/heads/master`:

```
$ cat .git/refs/heads/master 
a39b9fdd624c35eee08a36077f411e009da68c2f
```

以下是您需要了解的关于分行的信息:

> Git 中的分支是指向这些提交之一的轻量级可移动指针。Git 中默认的分支名称是 master。

呃，什么？我喜欢把分支看作代码中的分叉。你想做一些改变，但你不想打破东西。您决定有一个比提交日志更强的分界，这就是分支出现的地方。`master`是默认分支，也用作事实上的生产分支。因此，创建了上面的文件。正如您可以从文件内容中猜到的，它指向我们的第一次提交。因此，它是一个指向提交的指针。

让我们进一步探讨这个问题。比方说，我创建了一个新分支:

```
$ git branch the-ending
$ git branch
* master
  the-ending
```

我们有了，一个新的分支！正如您所猜测的，一个新的条目必须被添加到`.git/refs/heads/`中，因为没有额外的提交，它也应该指向我们的第一个提交，就像`master`一样。

```
$ cat .git/refs/heads/the-ending

a39b9fdd624c35eee08a36077f411e009da68c2f
```

是的，没错！现在，还记得`byeworld`？该文件仍然未被跟踪，所以无论您转移到哪个分支，该文件都将一直存在。比方说，我现在想切换到这个分支，所以我将`checkout`这个分支，就像[的烟火表演一样。](https://www.urbandictionary.com/define.php?term=smokeshow)

```
$ git checkout the-ending
Switched to branch 'the-ending'
$ git branch
  master
* the-ending
```

现在，在幕后，Git 将更改工作目录的所有内容，以匹配分支提交所指向的内容。目前来看，由于这和师父一模一样，所以看起来也一样。

我的`add`和`commit`是`byeworld`文件。

您希望在`objects`文件夹中更改什么？

您希望在`refs/heads`文件夹中更改什么？

在前进之前想好这一点。

```
$ tree .git/
.git/
├── COMMIT_EDITMSG
├── HEAD
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── logs
│   ├── HEAD
│   └── refs
│       └── heads
│           ├── master
│           └── the-ending
├── objects
│   ├── 0b
│   │   └── 17be9dbc34c5a5fbb0b94d57680968efd035ca
│   ├── a0
│   │   └── 423896973644771497bdc03eb99d5281615b51
│   ├── a3
│   │   └── 9b9fdd624c35eee08a36077f411e009da68c2f
│   ├── b3
│   │   └── 00387d818adbbd6e7cc14945fdf4c895de6376
│   ├── d1
│   │   └── 8affe001488123b496ceb34d8b13b120ab4cb6
│   ├── fb
│   │   └── 26ca0289762a454db2ef783c322fedfc566d38
│   ├── info
│   └── pack
└── refs
    ├── heads
    │   ├── master
    │   └── the-ending
    └── tags
17 directories, 27 files
```

3 个新对象— 1 个用于添加，2 个用于提交！有道理？你认为这些物品包含什么？

*   提交元数据
*   `add`对象内容
*   树描述

图的最后一部分是:这个提交元数据如何与前面的提交元数据一起工作。嗯，`cat-file`！

```
$ git cat-file 0b17be9dbc34c5a5fbb0b94d57680968efd035ca -p
100644 blob d18affe001488123b496ceb34d8b13b120ab4cb6 byeworld
100644 blob a0423896973644771497bdc03eb99d5281615b51 helloworld
$ git cat-file b300387d818adbbd6e7cc14945fdf4c895de6376 -p
tree 0b17be9dbc34c5a5fbb0b94d57680968efd035ca
parent a39b9fdd624c35eee08a36077f411e009da68c2f
author = <=> 1537770989 +0100
committer = <=> 1537770989 +0100
add byeworld
$ git cat-file d18affe001488123b496ceb34d8b13b120ab4cb6 -p
Bye world!
$ cat .git/refs/heads/the-ending 
b300387d818adbbd6e7cc14945fdf4c895de6376
```

你看到粗体字了吗？父指针！这正是你所想的——一个链表，将提交链接在一起！

你看到分支实现了吗？它指向一个提交，这是我们在签出后做的最后一个提交！当然，主服务器应该仍然指向 helloworld commit，对吗？

```
$ cat .git/refs/heads/master

a39b9fdd624c35eee08a36077f411e009da68c2f
```

好了，我们已经经历了很多，让我们总结到这里。

### TL；速度三角形定位法(dead reckoning)

Git 处理对象——您要求 Git 跟踪的文件的压缩版本。

每个对象都有一个 ID(Git 根据文件内容生成的散列)。

每当您`add`一个文件时，Git 都会向对象存储中添加一个新对象。**这就是为什么你不能在 Git** 中处理非常大的文件——每次你`add`改变时，它存储整个文件，而不是 diff(与普遍的看法相反)。

每次提交都会创建两个对象:

1.  **树**:树的 ID，它的作用就像一个 unix 目录:它指向其他树(目录)或 blobs(文件):这将基于当时存在的对象构建整个目录结构。斑点由`add`创建的当前对象表示。
2.  **提交元数据**:提交的 ID，提交者，代表提交、提交消息和父提交的树。形成将提交链接在一起的链表结构。

分支是指向提交元数据对象的指针，都存储在`.git/refs/heads`中

幕后了解到此为止！[在下一部分](https://medium.freecodecamp.org/now-that-youre-not-afraid-of-git-anymore-here-s-how-to-leverage-what-you-know-11e710c7f37b)中，我们将经历一些让人们做噩梦的 Git 行为:

`reset, merge, pull, push, fetch`以及它们如何在`.git/`中修改内部结构。

本系列的其他故事:

*   [如何不再害怕 Vim](https://medium.freecodecamp.org/how-not-to-be-afraid-of-vim-anymore-ec0b7264b0ae)
*   [如何不再害怕蟒蛇](https://medium.freecodecamp.org/how-not-to-be-afraid-of-python-anymore-b37b58871795)

喜欢这个吗？不要再错过任何帖子了——订阅我的邮件列表吧！