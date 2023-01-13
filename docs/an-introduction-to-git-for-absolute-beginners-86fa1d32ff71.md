# 绝对初学者的 Git

> 原文：<https://www.freecodecamp.org/news/an-introduction-to-git-for-absolute-beginners-86fa1d32ff71/>

作者:沙赫赞

# 绝对初学者的 Git

![VQhi-KgyeBh6jegrDc2zaLOGxsBWq0Bw5dNq](img/059b296615da14d1e462293c426fa8a1.png)

如果您是编程界的新手，那么学习 Git 应该是您的首要任务。

Git 就是这样一个工具，您将在日常工作中遇到它。

#### 在这篇文章中你可以期待什么

在这篇文章中，我将概述 Git 以及如何开始使用它。

*   Git 是什么？
*   与 Git 相关的术语
*   使用命令行与 Git 交互

我保证尽可能用最简单的方式解释这些话题。

#### 所以让我们从理解什么是 Git 开始。

Git 是一个版本控制系统。

#### 现在，什么是版本控制系统(VCS)？

VCS 监控并跟踪对其监控的文件所做的所有更改。

它还允许多个开发人员共享同一组文件并协同工作，而不会与彼此的工作发生冲突。

它不仅跟踪哪些文件发生了更改，还跟踪

*   做了哪些改变？
*   谁做了这些改变？
*   当这些变化完成时。

为了与其他开发人员共享和协作，您需要访问基于 Git 的托管服务。

一些流行的 Git 托管服务提供商有:

*   [GitHub](https://github.com/)
*   [Bitbucket](https://bitbucket.org/)
*   [微软 Visual Studio 团队服务](https://visualstudio.microsoft.com/team-services/)

它们都提供类似的功能。

#### Git 中的存储库是什么？

一个 ***存储库*** 是一个文件夹，它的内容被 Git 跟踪。简单来说，它也被称为 ***回购*** 。

一个 repo 可能包含多个文件和子文件夹。通常，存储库中的文件包含源代码。

在每个回购内，都有一个 ***。git 文件夹*** 。这个文件夹包含 Git 跟踪对这个 repo 中的文件所做的所有更改所需的所有文件和文件夹。

如果我们删除这个。git 文件夹，Git 不会将该文件夹识别为 repo，也不会跟踪其内容。

存在于本地计算机上的回购被称为 ***本地储存库*** ，位于托管 Git 平台上的储存库被称为 ***远程储存库*** 。

#### 下载并安装 Git

下载和安装 Git 是一个相当简单的过程。

你可以从这里下载 Git。

一旦下载了 Git，[你可以参考这个指南来安装它](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)。

#### 初始化 Git 存储库

在我们继续使用 Git 跟踪我们的文件之前，我们需要为我们希望 Git 监控的文件夹初始化 Git。

简单地说，Git 将文件夹转换成存储库，这样它就可以跟踪它的内容。

为了将文件夹初始化到 Git 存储库中:

在基于 Windows 的系统上，我们需要 ***右击文件夹*** (我们希望被 Git 跟踪)，然后点击***“Git Bash Here”。这将打开一个类似窗口的命令提示符，允许我们使用 Git 命令与 Git 交互。***

> **注意:**每当我们想要与 Git 交互时，我们将通过这个 Git Bash 窗口使用 Git 命令进行交互。还要注意，对于基于 Windows 和 Unix 的系统，Git 命令没有什么不同。

在 Git Bash 窗口中，我们需要键入命令:

```
git init
```

此命令初始化文件夹。基本上，它将这个文件夹转换成 Git 存储库。

作为这个初始化过程的一部分，它还会在这个存储库中创建一个. git 文件夹(这是一个隐藏文件夹)。这包含 Git 跟踪对这个存储库所做的所有更改所需的所有文件。

但这只是一个普通的文件夹，就像系统中的其他文件夹一样。在 Git 术语中，更准确地说，我们仍然称之为存储库或本地存储库。

在基于 Unix 的系统上，我们只需导航到目录(您希望被 Git 跟踪的目录)，然后运行 **git init** 命令，就这样。这会将这个目录转换成 Git 存储库。

#### 储存库状态

在任何时候，如果我们想知道 Git 在存储库中跟踪了什么，我们可以通过输入下面的命令来实现:

```
git status
```

我们将在本文后面的某个地方更详细地查看这个命令。

现在只需要记住，如果我们想要查看 Git 在存储库中跟踪了什么，我们可以使用这个命令。

#### 跟踪存储库

即使我们已经将文件夹初始化为 Git 存储库，它的内容也不会被自动跟踪。我们需要指示 Git 监控它的内容。

为了做到这一点，我们使用了 **git add** 命令。该命令的语法如下所示:

```
git add file [file] [file..]
```

> 注意:方括号 *[]* 中的任何内容都是可选的。这适用于本文中列出的所有 Git 命令。

我们可以指定 Git 跟踪单个或多个文件。

如果我们希望 Git 监控存储库中的特定文件，我们可以通过指定我们想要跟踪的每个文件的单独文件名来实现。

如果我们想要跟踪属于特定文件类型的文件，我们可以通过指定其文件扩展名来实现，如下所示。这将跟踪所有以。txt 扩展名。

```
$ git add *.txt
```

如果我们希望 Git 跟踪存储库中的所有文件，语法如下所示。

```
$ git add .
```

假设我们的存储库中有以下文件:

![Z2s9Bni4O-19bASIGpay70eaDx-yNWHRK9Mi](img/b9f6d1a26abde084737ea4db24b2db7a.png)

正如你所看到的。git 文件夹已作为初始化过程的一部分创建。最初这个文件夹是隐藏的——我不得不改变文件夹的属性使它可见(只是为了向大家显示)。

这是 git init 命令执行后. git 文件夹的外观。

![kKznhad2RUFHV62YbjoWh6c-zvzIGoliSykk](img/647202ceff639094dd6775520a927477.png)

这就是。git 文件夹在对存储库做了一些事务后查看。

![UGbIpgCGjcID7R2xJNP4d8hdjx8f5ibGnlj3](img/4ba175ce85401f2756734821f880a8e4.png)

要检查 Git 当前正在跟踪哪些文件，我们可以使用 git status 命令:

```
$ git status
On branch master

No commits yet

Untracked files:
 (use “git add <file>…” to include in what will be committed)

HelloWorld.html
 Notes.txt
 README.md

nothing added to commit but untracked files present (use “git add” to track)
```

查看 **git status** 命令的输出，它表明 git 当前没有跟踪任何文件。

让我们继续添加这些文件，以便 Git 可以跟踪它们。

添加这些文件的命令如下所示:

```
$ git add HelloWorld.html Notes.txt
```

现在，让我们执行 git status 命令并检查其输出。

```
$ git status
On branch master

No commits yet

Changes to be committed:
 (use “git rm — cached <file>…” to unstage)

new file: HelloWorld.html
 new file: Notes.txt

Untracked files:
 (use “git add <file>…” to include in what will be committed)

README.md
```

如我们所见，我们在暂存区中有`HelloWorld.txt`和`Notes.txt`文件，它们正在等待提交。

`README.md`文件根本没有被跟踪，因为我们没有在之前执行的 git add 命令中包含这个文件。

当我们执行 git add 命令时，git 暂存了作为该命令输入的一部分指定的所有文件。

在我们提交这些文件之前，Git 不会开始跟踪这些文件。

#### 提交暂存文件

让我们通过键入如下所示的命令来提交这些暂存文件。

```
$ git commit -m ‘Initial Commit’
```

git commit 是用于提交任何暂存文件的命令，-m 用于指定此提交操作的注释。

如果我们想要查看已经执行的所有提交操作，我们可以通过键入 git log 命令来实现，如下所示。

```
$ git log

commit 8525b32ffcb92c731f5d04de7fe285a2d0ebe901 (HEAD -> master)

Author: shahzan <sxxxxxxn@gmail.com>

Date: Sun Apr 28 01:12:20 2019 +0100

Initial Commit
```

每当对 Git 跟踪的文件进行任何更改时，我们都需要重新存放这些文件并再次重新提交它们。在这些文件没有被重新登台和重新提交之前，它们将被 Git 跟踪。

我对 Notes.txt 文件做了一些小的修改，让我们通过执行 git status 命令来看看 Git 对这些修改有什么看法。

```
$ git status

On branch master

Changes not staged for commit:

(use “git add <file>…” to update what will be committed)

(use “git checkout — <file>…” to discard changes in working directory)

modified: Notes.txt

Untracked files:

(use “git add <file>…” to include in what will be committed)

README.md

no changes added to commit (use “git add” and/or “git commit -a”)
```

查看上面的输出块，可以清楚地看到文件`Notes.txt`已经被修改，并且这些更改没有提交。

我们使用相同的 git add 命令来重新存放文件。

```
$ git add Notes.txt

Shahzan@BlackBox MINGW64 /d/Medium Post Pics/Git/Source Code (master)

$ git status

On branch master

Changes to be committed:

(use “git reset HEAD <file>…” to unstage)

modified: Notes.txt

Untracked files:

(use “git add <file>…” to include in what will be committed)

README.md
```

正如您从上面的输出块中可以注意到的，文件已经被暂存，正在等待提交。

同样，我们使用同一个 git commit 命令来重新提交暂存文件。

```
$ git commit -m ‘Notes.txt file updated’

[master 184fcad] Notes.txt file updated

1 file changed, 3 insertions(+), 1 deletion(-)
```

让我们执行 git log 命令，看看提交是否成功。

```
$ git log

commit 184fcad4185296103cd9dba0da83520399a11383 (HEAD -> master)

Author: shahzans <shuaib.shahzan@gmail.com>

Date: Sun Apr 28 01:15:38 2019 +0100

Notes.txt file updated

commit 8525b32ffcb92c731f5d04de7fe285a2d0ebe901

Author: shahzans <shuaib.shahzan@gmail.com>

Date: Sun Apr 28 01:12:20 2019 +0100

Initial Commit
```

正如您可能注意到的，在上面的输出块中，显示了两个提交操作。

#### 忽略文件

在存储库中，可能有保存敏感数据或日志数据的文件，我们不希望在任何情况下被 Git 跟踪。

。gitignore 是一个文件，我们可以在其中指定所有不想让 Git 跟踪的文件。

```
$ touch .gitignore
```

创建这个文件的语法如上所示。

假设我不希望 Git 跟踪任何以。md 分机。

在添加*之前。md 到了。gitignore 文件，看看 git status 命令的输出，如下面的输出块所示。

```
$ git status

On branch master

Untracked files:

(use “git add <file>…” to include in what will be committed)

.gitignore

README.md

nothing added to commit but untracked files present (use “git add” to track)
```

正如您可能注意到的，我们将`.gitignore`和`README.md`显示为未跟踪的文件。

添加*后。md 到了。gitignore 文件，git 状态如下面的输出块所示。

你可能注意到了我们现在。gitignore 显示为未被跟踪的文件。

```
$ git status

On branch master

Untracked files:

(use “git add <file>…” to include in what will be committed)

.gitignore

nothing added to commit but untracked files present (use “git add” to track)
```

可以指定单个文件名，也可以在。gitignore 文件。

#### **总结会**

Git 是一个非常强大的工具，你可以用它做更多的事情，比如分支、合并、拉和推请求等等。

万一你有兴趣学习更多关于 Git 的知识，[我推荐你参加](http://bit.ly/git-complete)(会员链接)的课程。

### 在你说再见之前…

让我们保持联系，[点击这里输入您的电子邮件地址](https://forms.gle/3U1uBNEC4mDkSpMJ7)(如果上面的小工具没有出现在您的屏幕上，请使用此链接)。

非常感谢你花宝贵的时间阅读这篇文章。