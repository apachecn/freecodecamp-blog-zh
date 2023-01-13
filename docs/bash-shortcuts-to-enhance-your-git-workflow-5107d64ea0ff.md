# 增强 Git 工作流的 Bash 快捷方式

> 原文：<https://www.freecodecamp.org/news/bash-shortcuts-to-enhance-your-git-workflow-5107d64ea0ff/>

巴迪·雷诺

# 增强 Git 工作流的 Bash 快捷方式

![x3ZGLLC2YwrQr7nzJ84PXWKnEUT70K-ksanI](img/f6ae8347803510bfd82f3b8ab2b26809.png)

Unrelated — I just like it. [Photo credit](https://unsplash.com/@peter_mc_greats)

你使用 Git 越多，你学习它的工作流程就越快。

以下是我和我的团队每天要做的一些工作:

1.  创建和命名分支
2.  对挤压提交进行计数
3.  将 master 更新到最新版本，然后将其重置到分支上

但是这些任务中的每一个都需要多个步骤。这让我想到:*一定有更好的方法来做这件事*。

谢天谢地，有更好的办法！通过学习一点 Bash，您可以创建 Git 别名，这将节省您大量的时间。

#### 首先:Git 的！旗

你见过以感叹号开头的 Git 别名吗？例如:

```
somealias = “!......some code”
```

根据 Git 的[文档](https://git-scm.com/docs/git-config#git-config-alias)，*如果别名扩展以感叹号为前缀，将被视为 shell 命令。*

嘿，太棒了！我们可以利用这一点，给我们的别名添加一些智能。让我们先尝试一个简单的例子，然后进一步增加复杂性。

在您喜欢的文本编辑器中打开您的`~/.gitconfig`文件，并添加以下别名:

```
hello = "!echo \"Hello World\""# (the backslashes are for escaping the quotes.)
```

现在，当您在终端中运行`$ git hello`时，您将得到`Hello World`作为输出。厉害！有了这些知识，让我们看一下上面概述的 3 个例子和我用来完成它们的别名。

#### 一致的分支名称

别名:

```
newb = "!f() { ticketnum=$1; branchName=$2; git checkout -b \"POD-${ticketnum}/${branchName}\"; }; f"
```

用法:

```
# Creates a new branch named POD-573/my-new-feature$ git newb 573 my-new-feature
```

键盘命令:

*   功能:`f(){}; f`
*   参数:`$1`，`$2`
*   字符串插值:`${ticketnum}`，`${branchName}`

在我的团队中，我们将分行名称放在前面，以匹配我们票务系统中的卡号。例如:“POD- 573/我的新功能”。这与票务系统中的提交挂钩一起工作，将事情联系在一起，所以我们坚持使用这个系统是很重要的。

#### 功能

在 bash 中，你可以这样写一个函数:`FunctionName(){}; FunctionName`。在函数声明之后写函数名是运行函数的关键。在我的别名中，为了简洁起见，我将函数名简称为`f`。

当 bash 运行`f`时，它将运行花括号`{}`之间输入的所有代码。在这种情况下，该功能正在运行`git checkout -b “MESSAGE”`。

#### 因素

参数是紧跟在命令后面的。例如，bash 中的 move 命令:

```
$ mv ./file.txt ./folder/file.txt
```

移动命令接收的第一个参数是`./file.txt`。这个参数在 bash 中自动赋给`$1`。

同样，`./folder/file.txt`被分配给`$2`。在 alias 函数中，您可以使用这些知识为这些参数分配更有意义的变量名。

```
# expanded for readability!f() { # much more meaningful! ticketnum=$1;  branchName=$2; git checkout -b \"POD-${ticketnum}/${branchName}\";};f
```

#### 字符串插值

要在 bash 中使用变量，只需在变量名前面加上一个美元符号`$`，比如:`$ticketnum`。在这种情况下，函数将变量插入到字符串中。

虽然 bash 确实允许用户直接使用变量进行插值，但我更喜欢使用许多其他编程语言用来进行字符串插值的语法`${variable}`。

使用上面的使用示例，当 bash 对`POD-${ticketnum}/${branchName}`求值时，会扩展到`POD-573/my-new-feature`。

#### 提交计数

别名:

```
count = "!f() { compareBranch=${1-master}; git rev-list --count HEAD ^$compareBranch; }; f"
```

用法:

```
# Branch has made 5 commits since branching from master.$ git count # returns 5# Pass in a branch name to check instead of master$ git count dev
```

键盘命令:

*   参数扩展:`${1-DEFAULT}`
*   盘点修订:`rev-list --count`

#### 参数扩展

类似于字符串插值，注意现在使用`${}`语法对`compareBranch`变量赋值。这允许您在没有参数传递给命令的情况下设置默认值。在别名中，如果没有向命令传递任何东西，`compareBranch=${1-master}`将使用 master 作为`compareBranch`。

```
# assumes master$ git count# compareBranch is set to dev$ git count dev
```

您可以查看 bash [文档](http://www.gnu.org/software/bash/manual/bashref.html#Shell-Parameter-Expansion)了解更多关于参数扩展的信息。

#### 清点修订

默认情况下，Git 的`rev-list`命令将返回与给定分支名称相关联的 sha。通过使用`--count`标志，它将返回该特定分支的提交总数。因为目标是从主节点(或其他分支)获取分支后的*新*提交的数量，所以需要用`^`操作符传入另一个分支名称。

```
$ git rev-list --count HEAD ^master
```

这个命令告诉 git *我想要可以从 HEAD(当前分支)访问但不能从 master 访问的提交数量。*

如果在从主服务器创建分支后提交了 5 次，该命令将返回 5。

#### 挤压 X 提交

别名:

```
squashbase = "!f() { branchName=${1-master}; commitCount=$(git count $branchName); git rebase -i HEAD~$commitCount; }; f"
```

用法:

```
# Get the number of commits to squash# and start an interactive rebase.$ git squashbase# pass in an optional branch name.$ git squashbase dev
```

键盘命令:

*   命令替换:`$(COMMAND)`

#### 命令替换

这个很好玩。该脚本再次使用可选的`branchName`来替换主脚本的分支，但是第二个参数使用了新的语法`$()`。这被称为“命令替换”当 Bash 看到括号中的命令时，它将计算该语句，并使用输出作为变量值。例如`x=$(echo “Hello”)`将评估为`x`接收`Hello`的值。

在这种情况下，别名回调到先前的`count`别名，以获得自主机以来的提交次数。假设当前分支自从从主分支以来已经提交了 5 次，运行`$ git squashbase`将评估为`$ git rebase -i HEAD~5`。这个命令用最近的 5 次提交启动一个交互式的 rebase，让您有机会清理您的提交。

#### 更新主数据库并重置分支

别名:

```
pullbase = "!f() { branchName=${1-master}; git checkout $branchName && git pull && git checkout - && git rebase -i $branchName; }; f"
```

用法:

```
# Checkout the branch, pull it, check out the previous branch and rebase.$ git pullbase$ git pullbase dev
```

键盘命令:

*   Git 的破折号快捷键:`-`
*   控制命令:`&&`

#### 破折号快捷键

这里不多解释了。基本上，这将为你节省大量的打字时间。破折号是对您当前签出的最后一个分支的引用——有点像电视遥控器上的 recall 按钮。

考虑下面的例子:

```
# currently on branch dev$ git checkout master # now on master branch$ git checkout - # back on dev branch.$ git checkout - # back on master branch.
```

#### 控制命令

bash 中有很多方法可以将操作链接在一起。使用双&符号`&&`是我的最爱之一。

此命令的好处是，如果前一个命令未能完成，它将停止处理。如果 git 签出了 master，但未能拉下 latest，它将停止，而不是向前推进，并将可能过时的更改重新存储到您的分支中。

然后，上述别名将执行以下步骤，如果任何步骤失败，则停止执行:

1.  签出主分支(或给定分支)。
2.  更新到最新版本。
3.  检查您之前所在的分支。
4.  交互式地将主分支转换为当前分支。

我的团队要求我们所有的拉请求在合并到 master 之前都是一次提交。所以我一天多次运行这个命令，自从我开始使用它以来，它节省了我很多时间。

您可以在 Bash 的[文档](http://www.gnu.org/software/bash/manual/bashref.html#Lists-of-Commands)中阅读其他控制命令。

### 等等，为什么要用化名？

您可能会问，“我不应该只编写 bash 概要脚本吗？”

技术上来说，你可以这么做。这里的优势是*上下文*。所有这些命令都由各种 Git 命令组成。bash 配置文件中的函数将在您键入它的任何地方运行。

通过将这些脚本创建为 Git 别名，可以确保命令只在 git repo 中运行。另外，您不必仅仅为了“命名空间”而在函数名前面加上`git-Function`或`gitFunction`。如果您打算这样做，Git 别名是更好的选择。

我希望你已经从我的快捷方式中找到了一些灵感，并学会了如何创建自己的快捷方式。

如果您对自己的 git 工作流做了一些喜欢的别名或其他改进，请在评论部分分享它们，这样我们都可以探索它们，并可能节省我们大量的时间和打字。