# Linux 中的符号链接教程——如何创建和删除符号链接

> 原文：<https://www.freecodecamp.org/news/symlink-tutorial-in-linux-how-to-create-and-remove-a-symbolic-link/>

符号链接(也称为符号链接)是 Linux 中的一种文件类型，它指向计算机上的另一个文件或文件夹。符号链接类似于 Windows 中的快捷方式。

一些人将符号链接称为“软链接”——Linux/UNIX 系统中的一种链接——与“硬链接”相对。

## 软链接和硬链接的区别

软链接类似于快捷方式，可以指向任何文件系统中的另一个文件或目录。

硬链接也是文件和文件夹的快捷方式，但是不能为不同文件系统中的文件夹或文件创建硬链接。

让我们看看创建和删除符号链接的步骤。我们还将了解什么是断开的链接，以及如何删除它们。

## 如何创建符号链接

创建符号链接的语法是:

```
ln -s <path to the file/folder to be linked> <the path of the link to be created> 
```

`ln`是链接命令。`-s`标志指定链接应该是软的。`-s`也可以输入为`-symbolic`。

默认情况下，`ln`命令创建硬链接。下一个参数是您想要链接的`path to the file (or folder)`。(即要为其创建快捷方式的文件或文件夹。)

而最后一个参数就是`path to link`本身(快捷键)。

## 如何为文件创建符号链接–命令示例

```
ln -s /home/james/transactions.txt trans.txt 
```

运行该命令后，您将能够使用`trans.txt`访问`/home/james/transactions.txt`。对`trans.txt`的任何修改也将反映在原始文件中。

请注意，上面的命令将在您的当前目录中创建链接文件`trans.txt`。您也可以在文件夹中创建链接文件，链接方式如下:

```
ln -s /home/james/transactions.txt my-stuffs/trans.txt 
```

在你当前的目录中必须有一个名为“my-stuff”的目录——如果没有，这个命令将抛出一个错误。

## 如何为文件夹创建符号链接–命令示例

与上面类似，我们将使用:

```
ln -s /home/james james 
```

这将创建一个名为“james”的符号链接文件夹，其中将包含`/home/james`的内容。对此链接文件夹的任何更改也会影响原始文件夹。

## 如何删除符号链接

在您想要删除符号链接之前，您可能想要确认文件或文件夹是符号链接，这样您就不会篡改您的文件。

一种方法是:

```
ls -l <path-to-assumed-symlink> 
```

在您的终端上运行此命令将显示文件的属性。在结果中，如果第一个字符是一个小写字母 L ('l ')，这意味着该文件/文件夹是一个符号链接。

您还会在末尾看到一个箭头(->)，指示 simlink 指向的文件/文件夹。

有两种方法可以删除符号链接:

### 如何使用 Unlink 删除符号链接

语法是:

```
unlink <path-to-symlink> 
```

如果该过程成功，将删除符号链接。

即使符号链接是文件夹的形式，也不要附加“/”，因为 Linux 会假设它是一个目录，而`unlink`不能删除目录。

### 如何使用 rm 删除符号链接

正如我们所见，符号链接只是指向原始文件或文件夹的另一个文件或文件夹。要删除这种关系，您可以删除链接的文件。

因此，语法是:

```
rm <path-to-symlink> 
```

例如:

```
rm trans.txt
rm james 
```

注意，尝试做`rm james/`会导致一个错误，因为 Linux 会假设‘James/’是一个目录，这需要其他选项，比如`r`和`f`。但这不是我们想要的。符号链接可能是一个文件夹，但我们只关心它的名称。

与`unlink`相比，`rm`的主要好处是你可以一次删除多个符号链接，就像你可以删除文件一样。

## 如何找到并删除断开的链接

当符号链接指向的文件或文件夹更改路径或被删除时，就会出现断开的链接。

例如，如果“transactions.txt”从`/home/james`移动到`/home/james/personal`，则“trans.txt”链接会断开。每次尝试访问该文件都会导致“没有这样的文件或目录”错误。这是因为链接本身没有内容。

当您发现断开的链接时，您可以轻松地删除该文件。找到断开的符号链接的一个简单方法是:

```
find /home/james -xtype l 
```

这将列出`james`目录中所有断开的符号链接——从文件到目录再到子目录。

传递`-delete`选项会像这样删除它们:

```
find /home/james -xtype l -delete 
```

## 包扎

符号链接是 Linux 和 UNIX 系统一个有趣的特性。

您可以创建易于访问的符号链接来引用不方便访问的文件或文件夹。通过一些实践，您将在直观的层面上理解这些是如何工作的，并且它们将使您在管理文件系统时更加高效。