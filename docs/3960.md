# Git 提交命令解释

> 原文：<https://www.freecodecamp.org/news/git-commit-command-explained/>

`git commit`命令将保存所有暂存的变更，以及用户的简要描述，提交到本地存储库。

提交是 Git 使用的核心。您可以将提交视为项目的快照，在当前存储库中创建项目的新版本。提交的两个重要特征是:

*   您可以在以后调用提交的变更，或者将项目恢复到那个版本([参见 Git checkout](https://guide.freecodecamp.org/git/git-checkout) )
*   如果多个提交编辑项目的不同部分，即使提交的作者彼此不知道，它们也不会相互覆盖。这是使用 Git 胜过 Dropbox 或 Google Drive 等工具的好处之一。

## 选择

有许多选项可以包含在`git commit`中。然而，本指南将只涵盖两个最常见的选项。关于选项的详细列表，请参考 [Git 文档](https://git-scm.com/docs/git-commit)。

### -m 选项

与`git commit`一起使用的最常见的选项是`-m`选项。`-m`代表信息。调用`git commit`时，需要包含一条消息。该消息应该是对提交的更改的简短描述。消息应该在命令的末尾，并且必须用引号括起来`" "`。

如何使用`-m`选项的示例:

```
git commit -m "My message"
```

终端中的输出应该如下所示:

```
[master 13vc6b2] My message
 1 file changed, 1 insertion(+)
```

****注意:**** 如果`-m`没有包含在`git commit`命令中，系统会提示您在默认的文本编辑器中添加一条消息——参见下面的“使用详细提交消息”。

### -a 选项

另一个流行的选项是`-a`选项。`-a`代表所有。此选项会自动转移所有要提交的已修改文件。如果添加了新文件，则`-a`选项不会暂存这些新文件。只有 Git 存储库知道的文件才会被提交。

例如:

假设您有一个已经提交到存储库中的`README.md`文件。如果您对这个文件进行了更改，那么您可以在 commit 命令中使用`-a`选项，将更改暂存并添加到您的存储库中。但是，如果你还添加了一个名为`index.html`的新文件呢？`-a`选项不会存放`index.html`,因为它目前不存在于存储库中。当添加了新文件时，应该调用`git add`命令，以便在提交到存储库之前暂存文件。

如何使用`-a`选项的示例:

```
git commit -am “My new changes”
```

终端中的输出应该如下所示:

```
[master 22gc8v1] My new message
 1 file changed, 1 insertion(+)
```

## 使用详细的提交消息

虽然`git commit -m "commit message"`工作得很好，但是提供更详细、更系统的信息还是很有用的。

如果您在没有使用`-m`选项的情况下提交，git 将用一个新文件打开您的默认文本编辑器，该文件将包含提交中暂存的所有文件/更改的注释掉的列表。然后编写详细的提交消息(第一行将被视为主题行),提交将在保存/关闭文件时执行。

请记住:

*   作为标准做法，保持提交消息行长度小于 72 个字符
*   编写多行提交消息是完全可以的，甚至是值得推荐的
*   您还可以在提交消息中引用其他问题或拉请求。GitHub 为所有的拉取请求和问题分配了一个编号参考，因此，例如，如果您想参考拉取请求#788，只需在适当的主题行或正文中这样做即可

### ——修正选项

`--amend`选项允许您更改上次提交。假设您刚刚提交了日志，但在提交日志消息中犯了一个错误。您可以使用以下命令方便地修改最近的提交:

```
git commit --amend -m "an updated commit message"
```

如果您忘记在提交中包括文件:

```
git add FORGOTTEN-FILE-NAME
git commit --amend -m "an updated commit message"

# If you don't need to change the commit message, use the --no-edit option
git add FORGOTTEN-FILE-NAME
git commit --amend --no-edit
```

在您的日常开发过程中，过早提交一直在发生。很容易忘记暂存文件或如何正确格式化提交消息。标志是修复这些小错误的一种便捷方式。该命令将用命令中指定的更新消息替换旧的提交消息。

修改后的提交实际上是全新的提交，先前的提交将不再在当前分支上。当您与其他人一起工作时，如果最后一次提交已经被推送到存储库中，您应该尽量避免修改提交。

对于`--amend`，您可以使用的一个有用标志是`--author`,它使您能够更改上次提交的作者。想象一下，在 git 配置中，您还没有正确设置您的姓名或电子邮件，但是您已经提交了一个 commit。有了`--author`标志，你可以简单地改变它们，而不用重置最后一次提交。

```
git commit --amend --author="John Doe <johndoe@email.com>"
```

### -v 或 verbose 选项

使用`-v`或`--verbose`选项时，不使用`-m`选项。当您希望在默认编辑器中编辑 Git 提交消息，同时能够看到您对提交所做的更改时，`-v`选项会很有用。该命令打开您的默认文本编辑器，其中有一个提交消息模板*以及*您为该提交所做更改的副本。提交消息中不会包含更改，但是当您在提交消息中描述更改时，它们提供了一种引用更改的好方法。

## 如何将多个提交压缩成一个

这是`rebase`的一个很棒的功能，可以在`interactive`模式下使用。要将最后一个 *n 个*提交压缩成一个，运行以下命令:

```
git rebase -i HEAD~n
```

这将打开一个文本编辑器，内容如下:

```
pick commit_1
pick commit_2
pick commit_3
...
pick commit_n
# Bunch of comments
```

让第一次提交保持不变，将其余的`pick`更改为`squash`。保存并退出编辑器。

因此，如果您想要压缩最后三个提交，您将首先运行`git rebase -i HEAD~3`，然后您将想要编辑您的提交，如下所示:

```
pick dd661ba Commit 1
squash 71f5fee Commit 2
squash f4b4bf1 Commit 3
```

如果您在压缩提交之前已经推送到远程，那么您将不得不用`-f`标志再次推送到远程，否则 git 将向您抛出一个错误。

强烈建议您阅读打开的文件中的信息，因为您可以做很多事情。

### **更多信息:**

*   Git 文档:[提交](https://git-scm.com/docs/git-commit)