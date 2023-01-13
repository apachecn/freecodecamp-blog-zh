# 在 Linux-Bash 终端命令中重命名文件

> 原文：<https://www.freecodecamp.org/news/rename-file-linux-bash-command/>

无论您使用命令行还是 GUI，重命名文件都是非常常见的操作。

与 GUI(或图形用户界面)相比，CLI 尤其强大。这部分是因为您可以批量重命名文件，甚至可以安排脚本在某个时间点重命名文件。

在本教程中，您将看到如何使用内置的`mv`命令在 Linux 命令行中重命名文件。

## 如何使用 Linux 的`mv`命令

您可以使用内置的 Linux 命令`mv`来重命名文件。

`mv`命令遵循以下语法:

```
mv [options] source_file destination_file
```

以下是一些可以用`mv`命令派上用场的选项:

*   `-v`、`--verbose`:解释正在做的事情。
*   `-i`、`--interactive`:重命名文件前的提示。

假设你想把`index.html`改名为`web_page.html`。您使用`mv`命令如下:

```
zaira@Zaira:~/rename-files$ mv index.html web_page.html 
```

让我们列出这些文件，看看该文件是否已被重命名:

```
zaira@Zaira:~/rename-files$ ls
web_page.html
```

## 如何使用`mv`批量命名文件

让我们讨论一个脚本，在这个脚本中，您可以使用一个循环和`mv`命令来批量重命名文件。

这里我们有一个扩展名为`.js`的文件列表。

```
zaira@Zaira:~/rename-files$ ls -lrt
total 0
-rw-r--r-- 1 zaira zaira 0 Sep 30 00:24 index.js
-rw-r--r-- 1 zaira zaira 0 Sep 30 00:24 config.js
-rw-r--r-- 1 zaira zaira 0 Sep 30 00:24 blog.js
```

接下来，你想把它们转换成`.html`。

您可以使用下面的命令重命名文件夹中的所有文件:

```
for f in *.js; do mv -- "$f" "${f%.js}.html"; done
```

让我们分解这一长串，看看在引擎盖下发生了什么:

*   第一部分[ `for f in *.js` ]告诉`for`循环处理每个”。js”文件。
*   下一部分[ `do mv -- "$f" "${f%.js}.html` ]指定处理将做什么。它使用`mv`来重命名每个文件。新文件将以原始文件的名称命名，不包括`.js`部分。将添加一个新的扩展名`.html`。
*   最后一部分[ `done` ]只是在所有文件处理完毕后结束循环。

```
zaira@Zaira:~/rename-files$ ls -lrt
total 0
-rw-r--r-- 1 zaira zaira 0 Sep 30 00:24 index.html
-rw-r--r-- 1 zaira zaira 0 Sep 30 00:24 config.html
-rw-r--r-- 1 zaira zaira 0 Sep 30 00:24 blog.html
```

## 结论

如您所见，使用 CLI 重命名文件非常容易。当部署在脚本中时，它会非常强大。

你在这里学到的最喜欢的东西是什么？在 [Twitter](https://twitter.com/hira_zaira) 上告诉我！

你可以在这里阅读我的其他帖子[。](https://www.freecodecamp.org/news/author/zaira/)

[Freepik 上的故事集](https://www.freepik.com/free-vector/college-project-concept-illustration_29659818.htm)图片