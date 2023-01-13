# 如何创建自我记录的 Makefile

> 原文：<https://www.freecodecamp.org/news/self-documenting-makefile/>

我最喜欢的完全不充分使用 Makefile 的方法？创建您可以签入的个性化、基于项目的存储库工作流命令别名。

Makefile 能提高您的 DevOps 并让开发人员满意吗？如果从事您项目的新开发人员不是从复制和粘贴您的自述文件中的命令开始的，那该有多棒？如果不是:

```
pip3 install pipenv
pipenv shell --python 3.8
pipenv install --dev
npm install
pre-commit install --install-hooks
# look up how to install Framework X...
# copy and paste from README...
npm run serve
```

…您只需键入:

`make start`

…然后开始工作？

## 有所作为

我每天都使用`make`来消除日常开发活动中的乏味，比如更新程序、安装依赖项和测试。

为了用 Makefile (GNU make)完成所有这些，我们使用了 [Makefile 规则](https://www.gnu.org/software/make/manual/make.html#Rules)和[方法](https://www.gnu.org/software/make/manual/make.html#Recipes)。POSIX 风格的 make 也有类似的相似之处，比如[目标规则](https://pubs.opengroup.org/onlinepubs/9699919799/utilities/make.html#tag_20_76_13_04)。这里有一篇关于 POSIX 兼容 Makefiles 的文章。

这里有一些我们可以`make`简化的事情的例子(抱歉):

```
update: ## Do apt upgrade and autoremove
    sudo apt update && sudo apt upgrade -y
    sudo apt autoremove -y

env:
    pip3 install pipenv
    pipenv shell --python 3.8

install: ## Install or update dependencies
    pipenv install --dev
    npm install
    pre-commit install --install-hooks

serve: ## Run the local development server
    hugo serve --enableGitInfo --disableFastRender --environment development

initial: update env install serve ## Install tools and start development server 
```

现在我们有了一些命令行别名，您可以签入。好主意！如果您想知道奇怪的`##`注释语法是怎么回事，它会变得更好。

## 自我记录的 Makefile

别名是很棒的，如果你不用不停地输入`cat Makefile`就能记住它们是什么和它们做什么。自然，您需要一个`help`命令:

```
.PHONY: help
help: ## Show this help
    @egrep -h '\s##\s' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-20s\033[0m %s\n", $1, $2}' 
```

通过一点命令行技巧，这个`egrep`命令获取`MAKEFILE_LIST`的输出，对其进行排序，并使用`awk`来查找遵循`##`模式的字符串。然后它打印出一个有用的格式化版本的注释。

我们将把它放在文件的顶部，这样它就是默认的目标。现在，要查看我们所有方便的快捷方式及其功能，我们只需运行`make`或`make help`:

```
help                 Show this help
initial              Install tools and start development server
install              Install or update dependencies
serve                Run the local development server
update               Do apt upgrade and autoremove 
```

现在，我们有了自己的个性化和特定于项目的 CLI 工具！

使用自文档化的 Makefile 来改进 DevOps 流程的可能性几乎是无限的。你可以用它来简化任何工作流程，并产生一些非常快乐的开发人员。