# Linux cp 命令——如何在 Linux 中复制文件

> 原文：<https://www.freecodecamp.org/news/the-linux-cp-command-how-to-copy-files-in-linux/>

在电脑上工作时，有几种不同的方法可以拷贝和粘贴内容。

如果你花更多的时间在你的设备的用户界面上，你可能会用你的鼠标来做这件事。您可以通过右键单击文件并选择“复制”，然后转到不同的目录并选择“粘贴”来复制文件。

对于我的终端好友来说，也可以在不离开终端的情况下进行文件复制粘贴操作。在基于 Linux 的终端中，您可以使用`cp`命令来完成这个任务。

在本文中，我将解释什么是`cp`命令，并向您展示如何使用终端在 Linux 中复制和粘贴文件和目录。

## 什么是`cp`命令？

使用`cp`命令将文件从一个位置复制到另一个位置。该命令还可以复制目录(文件夹)。

该命令的语法是:

```
cp [...file/directory-sources] [destination] 
```

`[file/directory-sources]`指定要复制的文件或目录的来源。而`[destination]`参数指定了您想要将文件复制到的位置。

为了理解本文的其余部分，我将使用这个文件夹结构示例。假设一个名为**directory ya**的目录中有两个目录:**directory ya _ 1**和**directory ya _ 2**。这些子目录中有许多文件和子目录。

我还假设您目前在终端的**目录下**位置，如果您没有，请确保您是:

```
cd DirectoryA 
```

## 如何用`cp`命令复制文件

如果你想复制一个文件，比如说从**目录 yA_1** 到**目录 yA_2** 的 **README.txt** ，你可以像这样使用`cp`命令:

```
cp ./DirectoryA_1/README.txt ./DirectoryA_2
# ./DirectoryA_1/README.txt is the source file
# ./DirectoryA_2 is the destination 
```

如果你想从**目录 yA_1** 复制多个文件到**目录 yA_2** ，你可以像这样使用`cp`命令:

```
cp ./DirectoryA_1/README.txt ./DirectoryA_1/ANOTHER_FILE.txt ./DirectoryA_2 
```

如您所见，您将首先放置所有源文件，最后一个参数将是目标。

## 如何用`cp`命令复制目录

默认情况下，`cp`命令处理文件。因此，如果您尝试像这样复制一个目录:

```
cp ./DirectoryA_1/Folder/ ./DirectoryA_2 
```

您将收到一条错误消息，指出:

**。/directory ya _ 1/Folder/是一个目录**

要复制目录，您必须传递`-r`标志。该标志通知`cp`命令递归复制一个目录及其内容(可能是文件或其他子目录)。因此，对于前面的命令，您可以在目录源前添加标志，如下所示:

```
cp -r ./DirectoryA_1/Folder/ ./DirectoryA_2 
```

该命令将递归复制**中的**文件夹**目录。/directory ya _ 1/**以及**文件夹**目录下的所有文件和目录。

## 如何复制匹配全局模式的文件

glob 模式类似于 Regex，它允许您匹配多个文件名与特定模式相匹配的文件。在这里了解更多的区别: [Regex 与 Glob 模式](https://dillionmegida.com/p/regex-vs-glob-patterns/)。

例如，如果你想用**复制**目录 yA_1** 中的所有文件。txt** 扩展名，可以执行这个命令:

```
cp ./DirectoryA_1/*.txt ./DirectoryA_2 
```

`./DirectoryA_1/*.txt`匹配文件名中带有`.txt`扩展名的文件，`cp`命令可以将所有这些文件复制到目的地。

您可以查看 [glob 文档](https://linux.die.net/man/7/glob)以了解更多关于 globbing 模式和您可以使用的字符的信息。

现在您知道了如何直接从命令行复制文件(和目录)。感谢阅读！