# 如何使用你的？bashrc 文件用一半的击键做两倍的事

> 原文：<https://www.freecodecamp.org/news/how-to-do-twice-as-much-with-half-the-keystrokes-using-bashrc/>

在我最近关于用 Bash 脚本设置 Ubuntu 的文章中，我简单提到了`.bashrc`的魔力。这并没有真正做到公平，所以这里有一个快速的帖子，提供了更多关于 Bash 配置文件可以做什么的细节。

我目前的配置极大地改善了我的工作流程，并且节省了超过 50%的击键次数。让我们看一些别名、函数和提示配置的例子，它们可以通过帮助我们更有效地减少按键次数来改进我们的工作流程。

# Bash 别名

一个写得漂亮的`.bashrc`可以节省大量的击键次数。通过使用 [bash 别名](https://www.gnu.org/software/bash/manual/html_node/Aliases.html)，或者扩展为更大命令的字符串，我们可以在字面上利用这一点。举个说明性的例子，下面是一个在终端中复制文件的 Bash 别名:

```
# Always copy contents of directories (r)ecursively and explain (v) what was done
alias cp='cp -rv'
```

`alias`命令定义了我们将要输入的字符串，以及该字符串将要扩展到的内容。我们可以覆盖现有的命令，如上面的`cp`。就其本身而言，`cp`命令只会复制文件，而不会复制目录，并且会悄无声息地成功。有了这个别名，我们不需要记住传递这两个标志，也不需要通过`cd`或`ls`我们复制的文件的位置来确认它在那里！现在，只需按下这两个键(T5 和 T6)就可以完成所有的工作。

下面再来几个`.bashrc`别名，用常用函数传递标志。

```
# List contents with colors for file types, (A)lmost all hidden files (without . and ..), in (C)olumns, with class indicators (F)
alias ls='ls --color=auto -ACF'
# List contents with colors for file types, (a)ll hidden entries (including . and ..), use (l)ong listing format, with class indicators (F)
alias ll='ls --color=auto -alF'

# Explain (v) what was done when moving a file
alias mv='mv -v'
# Create any non-existent (p)arent directories and explain (v) what was done
alias mkdir='mkdir -pv'
# Always try to (c)ontinue getting a partially-downloaded file
alias wget='wget -c'
```

当我们想避免输入长命令时，别名也很方便。以下是我在 Python 环境中使用的一些例子:

```
alias pym='python3 manage.py'
alias mkenv='python3 -m venv env'
alias startenv='source env/bin/activate && which python3'
alias stopenv='deactivate'
```

关于 Bash 别名如何节省时间的更多灵感，我强烈推荐本文中的例子。

# Bash 函数

上述别名的一个缺点是它们相当静态——它们总是扩展到所声明的文本。对于接受参数的 Bash 别名，我们需要创建一个函数。我们可以这样做:

```
# Show contents of the directory after changing to it
function cd () {
    builtin cd "$1"
    ls -ACF
}
```

我已经数不清我输入了多少次`cd`然后再输入`ls`来查看我现在所在的目录的内容。设置好这个函数后，这一切只需要这两个字母就可以完成！该函数将第一个参数`$1`作为目录更改的位置，然后将该目录的内容打印在带有文件类型指示符的格式良好的列中。为了让 Bash 允许我们覆盖这个默认命令，`builtin`部分是必需的。

Bash 函数在下载或升级软件时也非常有用。我以前每隔几周至少花几分钟下载静态站点生成器 Hugo 的新扩展版本，这要感谢它们出色的发货频率。有了一个函数，我只需要传入版本，几秒钟就升级了。

```
# Hugo install or upgrade
function gethugo () {
    wget -q -P tmp/ https://github.com/gohugoio/hugo/releases/download/v"$@"/hugo_extended_"$@"_Linux-64bit.tar.gz
    tar xf tmp/hugo_extended_"$@"_Linux-64bit.tar.gz -C tmp/
    sudo mv -f tmp/hugo /usr/local/bin/
    rm -rf tmp/
    hugo version
}
```

`$@`符号简单地接受所有给定的参数，替换它在函数中的位置。要运行上述函数并下载 Hugo 版本 0.57.2，我们使用命令`gethugo 0.57.2`。

我也为 Golang 准备了一个:

```
function getgolang () {
    sudo rm -rf /usr/local/go
    wget -q -P tmp/ https://dl.google.com/go/go"$@".linux-amd64.tar.gz
    sudo tar -C /usr/local -xzf tmp/go"$@".linux-amd64.tar.gz
    rm -rf tmp/
    go version
}
```

或者一个将 GitLab 的远程源 URL 添加到当前存储库的函数怎么样？

```
function glab () {
    git remote set-url origin --add git@gitlab.com:"$@"/"${PWD##*/}".git
    git remote -v
}
```

使用`glab username`，我们可以用 GitLab.com 上的`username`为当前的 Git 存储库创建一个新的`origin` URL。推到一个新的远程 URL [会自动创建一个新的私有 GitLab 库](https://victoria.dev/verbose/how-to-write-bash-one-liners-for-cloning-and-managing-github-and-gitlab-repositories/#a-bash-one-liner-to-create-and-push-many-repositories-on-gitlab)，所以这是一个创建备份的有用快捷方式！

Bash 函数实际上只受限于脚本编写的可能性，实际上这种可能性很少。如果我们经常需要在终端中输入几行代码，我们可以为它创建一个 Bash 函数！

# Bash 提示符

除了目录内容，查看我们所在目录的完整路径也很有用。Bash 提示符可以向我们显示这个路径，以及其他有用的信息，比如我们当前的 Git 分支。为了提高可读性，我们可以为提示的每个部分定义颜色。下面是我们如何在`.bashrc`中设置提示来实现这一点:

```
# Colour codes are cumbersome, so let's name them
txtcyn='\[\e[0;96m\]' # Cyan
txtpur='\[\e[0;35m\]' # Purple
txtwht='\[\e[0;37m\]' # White
txtrst='\[\e[0m\]'    # Text Reset

# Which (C)olour for what part of the prompt?
pathC="${txtcyn}"
gitC="${txtpur}"
pointerC="${txtwht}"
normalC="${txtrst}"

# Get the name of our branch and put parenthesis around it
gitBranch() {
    git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}

# Build the prompt
export PS1="${pathC}\w ${gitC}\$(gitBranch) ${pointerC}\${normalC} "
```

结果:

```
~/github/myrepo (master) $
```

命名颜色有助于轻松识别一种颜色的开始和结束位置，以及下一种颜色的开始位置。我们在终端中看到的提示是由跟在`export PS1`后面的字符串定义的，提示的每个组成部分都设置了一个[转义序列](https://www.tldp.org/HOWTO/Bash-Prompt-HOWTO/bash-prompt-escape-sequences.html)。让我们来分解一下:

*   `\w`显示当前工作目录、
*   `\$(gitBranch)`调用上面定义的`gitBranch`函数，该函数显示当前 Git 分支，
*   如果您是普通用户或处于普通用户模式，将显示“$ ”,如果您是超级用户，将显示“#”。

Bash 转义序列的完整列表可以帮助我们显示更多的信息，甚至包括时间和日期！Bash 提示是高度可定制和个性化的，所以可以随意设置。

这里有几个选项，可以将信息放在前面和中心，帮助我们更有效地工作。

## 对于厌恶拖延的人来说

用户名和当前时间(秒)，24 小时制 HH:MM:SS 格式:

```
export PS1="${userC}\u ${normalC}at \t >"
```

```
user at 09:35:55 > 
```

## 对于那些总是想知道自己立场的人

完整的文件路径在单独的一行，用户名:

```
export PS1="${pathC}\w${normalC}\n\u:"
```

```
~/github/myrepo
user: 
```

## 对于极简主义者

```
export PS1=">"
```

```
> 
```

我们可以用基本的转义序列构建许多实用的提示；一旦我们开始集成函数和提示，就像在 Git 分支示例中一样，事情就会变得非常复杂。这种复杂程度对你的工作效率是一种增加还是一种损害，只有你自己才能确定！

许多花哨的 Bash 提示都可以通过快速搜索获得。我故意没有在这里提供样本，因为，如果你能像我一样对这些东西感到兴奋，可能需要几个小时才能回到你开始阅读这篇文章之前所做的事情，我的良心不允许这样。？

我们希望现在已经在投入的时间和从 Bash 配置文件中获得的有用性之间取得了良好的平衡！我希望你好好利用你新恢复的击键能力。