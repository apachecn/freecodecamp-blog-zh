# 这里是我上周使用的所有 Git 命令，以及它们的作用。

> 原文：<https://www.freecodecamp.org/news/git-cheat-sheet-and-best-practices-c6ce5321f52/>

作者:山姆·科尔科斯

# 这里是我上周使用的所有 Git 命令，以及它们的作用。

![bamlgwPuXXB-fnXeFEZmDDKpFyORz8ZacX23](img/9810861c3d2b939bde3417adc77160d5.png)

Image credit: [GitHub Octodex](https://octodex.github.com/)

像大多数新手一样，我开始在 StackOverflow 中搜索 Git 命令，然后复制粘贴答案，并没有真正理解他们做了什么。

![0qFQGxysX9XwKkft-6Sq0C4JyqfpRLzwvBkZ](img/1c446cd02cbc94a83b1d8ff70fa27063.png)

Image credit: [XKCD](https://xkcd.com/1597/)

我记得我当时在想，“如果有一个最常用的 Git 命令列表，并解释它们为什么有用，那该多好啊？”

几年后，我在这里编辑了这样一个列表，并列出一些即使是中级高级开发人员也会觉得有用的最佳实践。

为了保持实用性，我基于我在过去一周使用的实际 Git 命令列出了这个列表。

几乎每个开发者都使用 Git，最有可能的是 GitHub。但是一般的开发人员可能在 99%的时间里只使用这三个命令:

```
git add --all
git commit -am "<message>"
git push origin master
```

当你在一个人的团队、黑客马拉松或一次性应用上工作时，这一切都很好，但是当稳定性和维护开始成为优先事项时，清理提交、坚持分支策略和编写一致的提交消息变得很重要。

我将从常用命令列表开始，让新手更容易理解 Git 的功能，然后进入更高级的功能和最佳实践。

#### 常用命令

要在存储库(repo)中初始化 Git，只需键入以下命令。如果不初始化 Git，就不能在这个 repo 中运行任何其他 Git 命令。

```
git init
```

如果你正在使用 GitHub，并且你正在将代码推送到一个在线存储的 GitHub repo，你正在使用一个 ****远程**** repo。远程回购的默认名称(也称为别名)是 ****原点**** 。如果你从 Github 复制了一个项目，它已经有了一个 ****原点**** 。您可以使用命令 ****git remote -v**** 查看源代码，它将列出远程 repo URL。

如果您初始化了自己的 Git repo，并希望将其与 GitHub repo 相关联，那么您必须在 GitHub 上创建一个，复制提供的 URL，并使用命令****Git remote add origin<URL>**，用 GitHub 提供的 URL 替换“< URL >”。从那里，您可以添加、提交和推送您的远程存储库。**

当您需要更改远程存储库时，可以使用最后一种方法。假设您从其他人那里复制了一个 repo，并希望将远程存储库从原始所有者的帐户更改为您自己的 GitHub 帐户。遵循与****git remote add origin****相同的过程，除了使用 ****set-url**** 来改变远程回购。

```
git remote -v
git remote add origin <url>
git remote set-url origin <url>
```

复制回购最常见的方式是使用 ****git 克隆，**** 后跟回购的 URL。

请记住，远程存储库将链接到您从中克隆回购的帐户。因此，如果你克隆了一个属于别人的回购协议，你将无法推送到 GitHub，直到你使用上面的命令改变了 ****原点**** 。

```
git clone <url>
```

你会很快发现自己在使用分支。如果你不明白什么是分支，还有其他更深入的教程，你应该在继续之前阅读那些([这里有一个](https://docs.github.com/en/get-started/quickstart/github-flow))。

命令 ****git branch**** 列出了本地机器上的所有分支。如果要创建一个新的分支，可以用 ****git 分支<名称>**** ，用 ****<名称>**** 代表分支的名称，比如“master”。

****git check out<name>****命令切换到一个已存在的分支。您也可以使用****git check out-b<name>****命令来创建一个新的分支并立即切换到它。大多数人使用这个命令来代替单独的分支和签出命令。

```
git branch
git branch <name>
git checkout <name>
git checkout -b <name>
```

如果你对一个分支做了一系列的修改，我们称之为“开发”，并且你想把那个分支合并回你的**主分支，你使用 ****git 合并<分支>**** 命令。您需要 ****检出**** 主分支，然后运行****git merge develop****将开发合并到主分支中。**

```
`git merge <branch>`
```

**如果你和多人一起工作，你会发现自己处在一个位置，一个回购在 GitHub 上被更新，但是你没有本地的更改。如果是这种情况，您可以使用****git pull origin<branch>****从那个远程分支中提取最近的变更。**

```
`git pull origin <branch>`
```

**如果你想知道什么文件被修改了，什么被跟踪了，你可以使用 ****git status**** 。如果您想查看 **每个文件被修改了多少** ，您可以使用 ****git diff**** 来查看每个文件中被修改的行数。**

```
`git status
git diff --stat`
```

### **高级命令和最佳实践**

**很快，您就会希望您的提交看起来不错，并且保持一致。您可能还需要修改您的提交历史，以使您的提交更容易理解，或者恢复意外的重大更改。**

**使用 ****git log**** 命令可以查看提交历史。您将希望使用它来查看您的提交历史。**

**您的提交将带有消息和一个 ****散列**** ，它是一系列随机的数字和字母。一个示例哈希可能如下所示:****C3 d 882 aa a4 E3 D5 f 18 b 3890132670 FBE AC 912 f 7******

```
`git log`
```

**假设你推了一个东西，弄坏了你的 app。与其修复它并推出新的东西，不如退回一次提交，再试一次。**

**如果您想及时返回并从之前的提交中 ****签出**** 您的应用程序，您可以通过使用哈希作为分支名称来直接执行此操作。这将从当前版本中分离您的应用程序(因为您正在编辑历史记录，而不是当前版本)。**

```
`git checkout c3d88eaa1aa4e4d5f`
```

**然后，如果您从该历史分支进行了更改，并且想要再次推送，则必须进行强制推送。**

******注意:**** 强行推动是危险的，只有在万不得已的情况下才应该这样做。这将覆盖你的应用程序的历史，你将丢失之后的所有内容。**

```
`git push -f origin master`
```

**其他时候，把所有事情都放在一次提交中是不切实际的。也许你想在尝试一些有潜在风险的事情之前保存你的进展，或者也许你犯了一个错误并想避免在你的版本历史中有一个错误的尴尬。为此，我们有 ****git rebase**** 。**

**假设您在本地历史中有 4 次提交(没有推送到 GitHub ),您在其中来回移动。你的承诺看起来草率和优柔寡断。您可以使用 rebase 将所有这些提交合并成一个简单的提交。**

```
`git rebase -i HEAD~4`
```

**上面的命令将打开您的计算机的默认编辑器(它是 Vim，除非您已经将它设置为其他值)，其中有几个选项可以让您更改您的提交。它将类似于下面的代码:**

```
`pick 130deo9 oldest commit message
pick 4209fei second oldest commit message
pick 4390gne third oldest commit message
pick bmo0dne newest commit message`
```

**为了将这些结合起来，我们需要将“pick”选项改为“fixup”(如代码下面的文档所述)，以合并提交并丢弃提交消息。注意，在 vim 中，你需要按“ ****a**** 或者“ ****i**** ”才能编辑文本，要保存和退出，你需要键入 ****escape**** 键后接“ ****shift + z + z**** ”。别问我为什么，就是这样。**

```
`pick 130deo9 oldest commit message
fixup 4209fei second oldest commit message
fixup 4390gne third oldest commit message
fixup bmo0dne newest commit message`
```

**这将把您的所有提交合并到带有消息“最旧的提交消息”的提交中。**

**下一步是重命名提交消息。这完全是见仁见智的问题，但是只要你遵循一致的模式，你做的任何事情都是好的。我推荐使用 Google 为 Angular.js 发布的[提交指南。](https://github.com/angular/angular.js/blob/master/CONTRIBUTING.md#-git-commit-guidelines)**

**为了改变提交消息，使用 ****修改**** 标志。**

```
`git commit --amend`
```

**这也会打开 vim，文本编辑和保存规则同上。为了给出一个好的提交消息的例子，这里有一个遵循指南规则的例子:**

```
`feat: add stripe checkout button to payments page

- add stripe checkout button
- write tests for checkout`
```

**使用指南中列出的**类型的一个好处是，它使得编写变更日志更加容易。您还可以在引用问题的 ****页脚**** (同样，在指南中指定)中包含信息。****

******注意**** :如果你在一个项目上合作，你应该避免改变你的提交，并且将代码推送到 GitHub。如果你开始在人们的眼皮底下改变版本历史，你可能会因为难以跟踪的错误而使每个人的生活变得更加困难。**

**Git 有无数可能的命令，但是这些命令可能是您在编程的最初几年需要知道的唯一命令。**

****Sam Corcos 是 Sightline Maps 的首席开发人员和联合创始人，sight line Maps 是 3D 打印地形图最直观的平台，以及**[**learnphoenix . io**](http://learnphoenix.io/)**，这是一个使用 Phoenix 和 React 构建可扩展生产应用程序的中级高级教程网站。****