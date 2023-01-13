# 在 Linux 中复制目录——如何在 Linux 和 Unix (MacOS)的命令行中复制文件夹

> 原文：<https://www.freecodecamp.org/news/copy-a-directory-in-linux-how-to-cp-a-folder-in-the-command-line-in-linux-and-unix-macos/>

要在基于 Unix 的操作系统(Linux 和 MacOS)中复制文件或目录，可以使用`cp`命令。

`cp`命令是一个相对简单的命令，但是它的行为会根据输入(文件和目录)和传递给它的选项而稍有变化。

要查看`cp`命令的文档或手册，请在您的终端上运行`man cp`:

```
$ man cp

NAME
     cp -- copy files

SYNOPSIS
     cp [OPTIONS] source_file target_file
     cp [OPTIONS] source_file ... target_directory

... 
```

该命令的基本形式是接受一个要复制的输入源(或多个源)(文件或目录)和一个要将文件或目录复制到的目标:

```
cp [OPTIONS] source_file target_file
```

## 如何将文件复制到当前目录

若要复制文件，请传递要复制的文件以及要将文件复制到的位置的路径。

如果您有一个名为`a.txt`的文件，并且您想要一个名为`b.txt`的文件的副本:

```
$ ls
a.txt

$ cp a.txt b.txt

$ ls
a.txt   b.txt
```

> 如果您不熟悉`ls`命令，`ls`“列出”一个目录的所有内容。

默认情况下，`cp`命令使用*你的* *当前目录*作为路径。

### 如何将文件复制到另一个目录

要将文件复制到与当前目录不同的目录，只需将另一个目录的路径作为目的地:

```
$ ls ../directory-1/

$ cp a.txt ../directory-1/

$ ls ../directory-1/
a.txt 
```

在发出`cp`命令后，先前为空的`directory-1`现在包含了文件`a.txt`。

默认情况下，复制的文件使用原始文件的名称，但是您也可以选择传递文件名:

```
$ cp a.txt ../directory-1/b.txt

$ ls ../directory-1/
b.txt
```

## 如何将多个文件复制到一个目录中

要一次复制多个文件，您可以传递多个输入源和一个目录作为目标:

```
$ ls ../directory-1/

$ cp first.txt second.txt ../directory-1/

$ ls ../directory-1/
first.txt       second.txt 
```

这里，两个输入源(`first.txt`和`second.txt`)都被复制到目录`directory-1`中。

> **注意:**当传递多个源时，最后一个参数必须是目录。

## 如何将一个目录复制到另一个目录

如果您尝试将一个目录作为输入源传递，您会得到以下错误:

```
$ cp directory-1 directory-2
cp: directory-1 is a directory (not copied).
```

要复制一个目录，您需要添加`-r`(或`-R`)标志——这是`--recursive`的简写:

```
$ ls directory-1
a.txt

$ cp -r directory-1 directory-2

$ ls
directory-1          directory-2

$ ls directory-2
a.txt
```

在这里，包含文件`a.txt`的`directory-1`被复制到一个名为`directory-2`的新目录中，该目录现在也包含文件`a.txt`。

### 如何复制整个目录和目录的内容

当您复制一个目录时，有一个有趣的边缘情况:如果目标目录已经存在，您可以通过从您的输入中添加或删除一个尾随的`/`来选择是复制**目录**的内容还是**整个目录**。

以下是来自`man`页面的`-R`选项的描述:

> 如果 source_file 指定了一个目录，那么 cp 会复制该目录以及在该点连接的整个子树。如果源文件以/结尾，则复制目录的内容，而不是目录本身。

如果您想将*目录中的内容*复制到另一个目录中，那么在您的输入中添加一个结尾`/`。

如果要将目录*的内容和目录文件夹本身*复制到另一个目录，不要添加尾随`/`:

```
$ ls
directory-1          directory-2

$ ls directory-2

$ cp -r directory-1 directory-2

$ ls directory-2
directory-1

$ ls directory-2/directory-1
a.txt
```

这里您可以看到，因为`directory-2`已经存在，并且输入源没有尾随的`/`,`directory-1`*和*目录本身的内容都被复制到了目的地。

## 如何防止用`cp`覆盖文件

默认情况下，`cp`命令会覆盖现有文件:

```
$ cat a.txt
A

$ cat directory-1/a.txt
B

$ cp a.txt directory-1/a.txt

$ cat directory-1/a.txt
A
```

> 如果您不熟悉`cat`或“concatenate”命令，它会打印文件的内容。

有两种方法可以防止这种情况。

### 交互式标志

要在即将发生覆盖时得到提示，您可以添加`-i`或`--interactive`标志:

```
$ cp -i a.txt directory-1/a.txt
overwrite directory-1/a.txt? (y/n [n])
```

### 禁止冲撞的旗帜

或者，为了防止在没有提示的情况下被覆盖，您可以添加`-n`或`--no-clobber`标志:

```
$ cat a.txt
A

$ cat directory-1/a.txt
B

$ cp -n a.txt directory-1/a.txt

$ cat directory-1/a.txt
B
```

这里你可以看到，由于有了`-n`标志，`directory-1/a.txt`的内容没有被覆盖。

## 其他选项

还有许多其他有用的选项可以传递给`cp`命令:比如`-v`表示“详细”输出，或者`-f`表示“强制”

我强烈建议您通读`man`页，了解所有其他有用的选项。

如果你喜欢这个教程，我也会在 Twitter 上谈论类似这样的话题[，并在](https://twitter.com/johnmosesman)[我的网站](https://johnmosesman.com/)上写下它们。