# Git Push 命令解释了

> 原文：<https://www.freecodecamp.org/news/the-git-push-command-explained/>

`git push`命令允许您将提交从本地 Git 存储库中的本地分支发送(或*推送*)到远程存储库中。

为了能够推送到您的远程存储库，您必须确保 ****您对本地存储库的所有更改都被提交**** 。

该命令的语法如下:

```
git push <repo name> <branch name>
```

这个命令有许多不同的选项，你可以在 [Git 文档](https://git-scm.com/docs/git-push#_options_a_id_options_a)或者运行`git push --help`中了解更多。

### **推送至特定的远程存储库和分支**

为了推送代码，您必须首先将一个存储库克隆到您的本地机器。

```
# Once a repo is cloned, you'll be working inside of the default branch (the default is `master`)
git clone https://github.com/<git-user>/<repo-name> && cd <repo-name>
# make changes and stage your files (repeat the `git add` command for each file, or use `git add .` to stage all)
git add <filename>
# now commit your code
git commit -m "added some changes to my repo!"
# push changes in `master` branch to github
git push origin master
```

要了解有关分支机构的更多信息，请查看以下链接:

*   [git checkout](https://github.com/renington/guides/blob/master/src/pages/git/git-checkout/index.md)
*   [去分支](https://github.com/renington/guides/blob/master/src/pages/git/git-branch/index.md)

### **推送到特定的远程存储库和其中的所有分支**

如果您想要将您的所有更改推送到远程存储库和其中的所有分支，您可以使用:

```
git push --all <REMOTE-NAME>
```

其中:

*   `--all`是一个标志，表示您想要将所有分支推送到远程存储库
*   `REMOTE-NAME`是您要推送至的远程存储库的名称

### **用力参数**推送到特定的分支

如果您想忽略对 Github 上的 Git 存储库所做的本地更改(大多数开发人员都会这样做，以便对开发服务器进行热修复)，那么您可以使用—force 命令来忽略这些更改。

```
git push --force <REMOTE-NAME> <BRANCH-NAME>
```

其中:

*   `REMOTE-NAME`是您要将更改推送到的远程存储库的名称
*   `BRANCH-NAME`是您要将更改推送到的远程分支的名称

### **推送忽略 Git 的预推送挂钩**

默认情况下，`git push`将触发`--verify`切换。这意味着 git 将执行任何已经配置好的客户端预推送脚本。如果预推送脚本失败，那么 git 推送也会失败。(预推送挂钩有利于做一些事情，比如检查提交消息是否符合公司标准，运行单元测试等等)。有时，您可能希望忽略这种默认行为，例如，在您希望将您的更改推送到一个特性分支以供另一个贡献者提取的场景中，但是您的正在进行的更改正在破坏单元测试。要忽略钩子，只需输入 push 命令并添加标志`--no-verify`

```
git push --no-verify
```

### **更多信息:**

*   [Git 文档-推送](https://git-scm.com/docs/git-push)
*   [Git 挂钩](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)