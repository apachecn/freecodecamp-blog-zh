# Git 签出——如何从另一个分支签出文件

> 原文：<https://www.freecodecamp.org/news/git-checkout-file-from-another-branch/>

当您在 Git 中处理存储库时，您可能需要从另一个分支中签出特定的文件。

幸运的是，Git 提供了许多可能的方法来快速完成这项任务。最简单的解决方案之一是使用指定文件作为参数的`git checkout`命令。

在本文中，我们将分析这个问题的不同解决方案，并介绍每个解决方案需要遵循的流程。

让我们开始吧😎。

## Git 结帐用例示例

您正在处理一个名为`feature/A`的分支，其中包含一个名为`utils.js`的文件。

您有另一个名为`feature/B`的分支，它有一个更新的`utils.js`文件。

您想要签出该文件，并将其从名为`feature/B`的分支带到名为`feature/A`的分支。

对于这项任务，有三种可能的解决方案。

### 解决方案 1:使用`git checkout`命令

`git checkout`命令提供了一种从另一个分支获取文件或文件夹的简单方法。

以下是从另一个分支签出文件的语法:

```
git checkout <other-branch-name> -- path/to/your/folder
```

以下是要遵循的流程:

1.签出到要复制文件的分支。

```
git checkout feature/A
```

2.一旦你在正确的分支上，复制文件。

```
git checkout feature/B -- utils.js
```

3.使用 [git status](https://timmousk.com/blog/git-status/) 命令确保文件已被复制。

4.提交并推送到远程。

使用 checkout 命令时，您还可以获得:

*   另一个分支的文件夹。
*   通过指定每个文件来创建多个文件。

此外，请注意，您可以从 stash 中获得一个文件/文件夹。

### 解决方案 2:使用`git restore`命令

另一种选择是将`git switch`命令与`git restore`命令一起使用。

如果您从未听说过这两个命令，那也没关系。它们相对较新。Git 在 2019 年的 2.23 版本中引入了它们。

这两个命令的目的是分割`git checkout`命令的职责，以便为用户简化。

`git restore`命令恢复工作树。

`git switch`命令切换分支。

下面是从另一个分支获取文件的过程:

1.切换到要签出文件的分支。

```
git switch feature/A
```

2.从另一个分支获取文件。

```
git restore --source feature/B -- utils.js
```

3.提交并推动变更。

### 解决方案 3:使用`git show`命令

最后，我们可以使用`git show`命令。

以下是要遵循的流程:

1.切换到工作分支。

```
git switch feature/A
```

2.从另一个分支获取文件。

```
git show feature/B:path/utils.js > path/utils.js
```

3.提交并推动变更。

**注意:**这次您需要指定从您的目录根目录开始的相对路径。

## 最后的想法

如您所见，从另一个分支获取文件并不是什么难事。

当我在日常生活中需要这样做时，我通常使用`git checkout`命令。

![6ii6ur](img/f12b584d0a1530350b7186c6b0006dc7.png)

如果您有兴趣了解更多关于 Git 或 web 开发技术(如 TypeScript)的信息，请访问我的博客。

感谢您阅读这篇文章。