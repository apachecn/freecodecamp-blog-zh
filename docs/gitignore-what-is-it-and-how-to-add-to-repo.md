# Gitignore 解释:什么是 Gitignore，如何添加到你的回购中

> 原文：<https://www.freecodecamp.org/news/gitignore-what-is-it-and-how-to-add-to-repo/>

`.gitignore`文件是一个文本文件，它告诉 Git 在项目中忽略哪些文件或文件夹。

本地`.gitignore`文件通常放在项目的根目录下。您还可以创建一个全局`.gitignore`文件，该文件中的任何条目都将在您的所有 Git 存储库中被忽略。

要创建一个本地`.gitignore`文件，创建一个文本文件并将其命名为`.gitignore`(记住在开头要包含`.`)。然后根据需要编辑该文件。每一行都应该列出一个您希望 Git 忽略的附加文件或文件夹。

该文件中的条目也可以遵循匹配的模式。

*   `*`用作通配符匹配
*   `/`用于忽略相对于`.gitignore`文件的路径名
*   `#`用于给`.gitignore`文件添加注释

这是一个关于`.gitignore`文件的例子:

```
# Ignore Mac system files
.DS_store

# Ignore node_modules folder
node_modules

# Ignore all text files
*.txt

# Ignore files related to API keys
.env

# Ignore SASS config files
.sass-cache
```

添加或更改您的全局。gitignore 文件，运行以下命令:

```
git config --global core.excludesfile ~/.gitignore_global
```

这将创建文件`~/.gitignore_global`。现在，您可以像编辑本地`.gitignore`文件一样编辑该文件。你所有的 Git 库都会忽略全局`.gitignore`文件中列出的文件和文件夹。

### 如何从新 gitignore 中跟踪以前提交的文件

要取消跟踪单个文件，即停止跟踪文件但不将其从系统中删除，请使用:

`git rm --cached filename`

要取消跟踪`.gitignore`中的每个文件*:*

首先 ****提交**** 任何未完成的代码更改，然后运行:

`git rm -r --cached`

这将从索引(临时区域)中删除任何已更改的文件，然后运行:

`git add .`

提交它:

`git commit -m ".gitignore is now working"`

要取消`git rm --cached filename`，请使用`git add filename`

### **更多信息:**

*   Git 文件: [gitignore](https://git-scm.com/docs/gitignore)
*   忽略文件: [GitHub](https://help.github.com/articles/ignoring-files/)
*   有用的`.gitignore`模板: [GitHub](https://github.com/github/gitignore)