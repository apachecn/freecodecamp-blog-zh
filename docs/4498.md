# 如何编写 Bash 一行程序来克隆和管理 GitHub 和 GitLab 库

> 原文：<https://www.freecodecamp.org/news/bash-one-liners-for-github-and-gitlab/>

没有什么比一行优雅的 Bash 自动化几个小时的乏味工作更让我满意的了。

作为最近用 Bash 脚本自动重新创建我的笔记本电脑的探索的一部分(帖子即将发布！)，我想找到一种方法，轻松地将 GitHub 托管的存储库克隆到一台新机器上。经过一番挖掘，我写了一行这样的代码。

然后，本着不把所有鸡蛋放在同一个篮子里的精神，我编写了另一个一行程序来自动创建和推送 GitLab 托管的备份。他们来了。

# 一个 Bash 一行程序来克隆你所有的 GitHub 库

警告:您需要一份想要克隆的 GitHub 库的列表。这样做的好处是，它给了你充分的权力，让你可以在你的机器上选择你想要的库，而不是全情投入。

通过使用 HTTPS 和你的 [15 分钟缓存凭证](https://help.github.com/en/articles/caching-your-github-password-in-git)，或者，我更喜欢的方法，通过[用 SSH](https://help.github.com/en/articles/connecting-to-github-with-ssh) 连接到 GitHub，你可以很容易地克隆 GitHub 库，而不需要每次都输入密码。为了简单起见，我假设我们使用后者，并且我们的 SSH 键已经设置好了。

给定文件`gh-repos.txt`中的 GitHub URLs 列表，如下所示:

```
git@github.com:username/first-repository.git
git@github.com:username/second-repository.git
git@github.com:username/third-repository.git
```

我们运行:

```
xargs -n1 git clone < gh-repos.txt
```

这会将列表中的所有存储库克隆到当前文件夹中。如果您替换适当的 URL，同样的一行程序也适用于 GitLab。

## 这是怎么回事？

这个一行程序有两个部分:右边是违反直觉的输入，左边是让事情发生的部分。我们可以让这些部分的顺序更直观(也许？)通过编写如下相同的命令:

```
<gh-repos.txt xargs -n1 git clone 
```

为了对我们输入的每一行运行一个命令，`gh-repos.txt`，我们使用`xargs -n1`。工具`xargs`从输入中读取条目，并执行它找到的任何命令(如果它没有找到任何命令，它就会`echo`)。默认情况下，它假定项目由空格分隔；新行也可以使我们的列表更容易阅读。标志`-n1`告诉`xargs`使用`1`参数，或者在我们的例子中，每个命令一行。我们用`git clone`构建命令，然后`xargs`为每一行执行该命令。哒哒。

# Bash 一行程序，用于在 GitLab 上创建和推送许多存储库

与 GitHub 不同，GitLab 让我们可以做这件漂亮的事情，我们不必先使用网站来创建一个新的存储库。我们可以从终端创建一个新的 GitLab 存储库。新创建的存储库默认设置为私有，所以如果我们想在 GitLab 上公开它，我们必须稍后手动完成。

GitLab 文档告诉我们使用`git push --set-upstream`创建一个新项目，但是我发现这对于使用 GitLab 作为备份并不方便。当我将来使用我的库时，我希望运行一个命令，不需要我做额外的工作就可以将信息推送到 GitHub *和* GitLab。

为了让这个 Bash 一行程序工作，我们还需要一个 GitLab 的存储库 URL 列表(还不存在)。我们可以通过复制我们的 GitHub 库列表，用 Vim 打开它，并进行[搜索和替换](https://vim.fandom.com/wiki/Search_and_replace)来轻松做到这一点:

```
cp gh-repos.txt gl-repos.txt
vim gl-repos.txt
:%s/\<github\>/gitlab/g
:wq
```

这产生了`gl-repos.txt`，它看起来像:

```
git@gitlab.com:username/first-repository.git
git@gitlab.com:username/second-repository.git
git@gitlab.com:username/third-repository.git
```

我们可以在 GitLab 上创建这些存储库，添加 URL 作为远程，并通过运行以下命令将我们的代码推送到新的存储库:

```
awk -F'\/|(\.git)' '{system("cd ~/FULL/PATH/" $2 " && git remote set-url origin --add " $0 " && git push")}' gl-repos.txt
```

等一下，我会解释的；现在，注意`~/FULL/PATH/`应该是包含 GitHub 库的目录的完整路径。

我们必须注意几个假设:

1.  本地机器上包含存储库的目录的名称与 URL 中存储库的名称相同(如果它是用上面的一行程序克隆的，就会出现这种情况)；
2.  每个存储库当前都被检出到您想要推送的分支，即。`master`。

可以扩展这个一行程序来处理这些假设，但是根据作者的拙见，在这种情况下，我们真的应该编写一个 Bash 脚本。

## 这是怎么回事？

我们的 Bash 一行程序使用`gl-repos.txt`文件中的每一行(或 URL)作为输入。使用`awk`，它分离出包含我们本地机器上的存储库的目录名，并使用这些信息来构建我们更大的命令。如果我们对`awk`的产量进行`print`，我们会看到:

```
cd ~/FULL/PATH/first-repository && git remote set-url origin --add git@gitlab.com:username/first-repository.git && git push
cd ~/FULL/PATH/second-repository && git remote set-url origin --add git@gitlab.com:username/second-repository.git && git push
cd ~/FULL/PATH/third-repository && git remote set-url origin --add git@gitlab.com:username/third-repository.git && git push
```

让我们看看如何构建这个命令。

### 使用`awk`拆分字符串

工具`awk`可以根据[字段分隔符](https://www.gnu.org/software/gawk/manual/html_node/Command-Line-Field-Separator.html)拆分输入。默认的分隔符是一个空白字符，但是我们可以通过传递`-F`标志来改变它。除了单个字符，我们还可以使用一个[正则表达式字段分隔符](https://www.gnu.org/software/gawk/manual/html_node/Regexp-Field-Splitting.html#Regexp-Field-Splitting)。由于我们的存储库 URL 有一个固定的格式，我们可以通过查询斜杠字符`/`和 URL 末尾`.git`之间的子字符串来获取存储库名称。

实现这一点的一种方法是使用我们的正则表达式`\/|(\.git)`:

*   `\/`是转义的`/`字符；
*   `|`表示“或”，告诉 awk 匹配任一表达式；
*   `(\.git)`是我们的 URL 末尾匹配“”的捕获组。git”，带有转义的`.`字符。这有点像作弊，因为”。git”并没有严格地分割任何东西(另一边什么也没有)，但它是我们去掉这一部分的一个简单方法。

一旦我们告诉了`awk`在哪里拆分，我们就可以用[字段操作符](https://www.gnu.org/software/gawk/manual/html_node/Fields.html#index-_0024-_0028dollar-sign_0029_002c-_0024-field-operator)来获取正确的子串。我们用一个`$`字符引用我们的字段，然后用字段的列号。在我们的例子中，我们想要第二个字段，`$2`。下面是所有子字符串的样子:

```
1: git@gitlab.com:username
2: first-repository
```

为了使用整个字符串，或者在我们的例子中，整个 URL，我们使用字段操作符`$0`。要编写命令，我们只需用字段操作符代替存储库名称和 URL。在我们建造它的时候用`print`运行它可以帮助我们确保所有的空间都是正确的。

```
awk -F'\/|(\.git)' '{print "cd ~/FULL/PATH/" $2 " && git remote set-url origin --add " $0 " && git push"}' gl-repos.txt
```

### 运行命令

我们在`system()`的括号内构建我们的命令。通过使用这个作为`awk`的输出，每个命令一建立和输出就运行。`system()`函数创建一个[子进程](https://en.wikipedia.org/wiki/Child_process)，它执行我们的命令，然后在命令完成后返回。简单地说，这让我们可以在每个存储库上一个接一个地执行 Git 命令，而不会中断主进程，在主进程中`awk`正在处理我们的输入文件。这是我们最后的命令。

```
awk -F'\/|(\.git)' '{system("cd ~/FULL/PATH/" $2 " && git remote set-url origin --add " $0 " && git push")}' gl-repos.txt
```

### 使用我们的备份

通过添加 GitLab URLs 作为远程，我们简化了推送至两个外部托管存储库的过程。如果我们在一个存储库目录中运行`git remote -v`,我们会看到:

```
origin  git@github.com:username/first-repository.git (fetch)
origin  git@github.com:username/first-repository.git (push)
origin  git@gitlab.com:username/first-repository.git (push)
```

现在，只需不带参数地运行`git push`就会将当前分支推送到两个远程存储库。

我们还应该注意到，`git pull`通常只会尝试从您最初克隆的远程存储库(在我们上面的例子中标为`(fetch)`的 URL)中提取。同时从多个 Git 仓库拉取数据是可能的，但是很复杂，超出了本文的范围。如果你好奇的话，这里有一个关于推拉多个遥控器的[解释来帮助你开始。关于遥控器](https://astrofloyd.wordpress.com/2015/05/05/git-pushing-to-and-pulling-from-multiple-remote-locations-remote-url-and-pushurl/)的 [Git 文档可能也有帮助。](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)

# 详细阐述 Bash 一行程序的简洁性

理解 Bash 一行程序后，它可能是有趣且方便的捷径。至少，意识到像`xargs`和`awk`这样的工具可以帮助自动化和减轻我们工作中的许多乏味。然而，也有一些缺点。

就易于理解、可维护和易于使用的工具而言，Bash 一行程序很糟糕。它们通常比使用`if`或`while`循环的 Bash 脚本更复杂，阅读起来也更复杂。很可能当我们写它们的时候，我们会在某个地方漏掉一个单引号或右括号；正如我希望这篇文章所展示的那样，他们也需要相当多的解释。那么为什么要使用它们呢？

想象一下，一步一步地阅读烘焙蛋糕的食谱。你了解方法和成分，并收集你的用品。然后，当你思考这个问题的时候，你开始意识到，如果你把所有的原料按照正确的顺序放入烤箱，一个蛋糕就会立刻变成现实。你试一下，它就起作用了！

那会很令人满意，不是吗？