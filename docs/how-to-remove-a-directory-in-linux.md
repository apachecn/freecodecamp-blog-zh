# 如何在 Linux 中删除目录–删除文件夹命令

> 原文：<https://www.freecodecamp.org/news/how-to-remove-a-directory-in-linux/>

如果你使用的是用户界面，你可以右键点击一个目录，然后选择“删除”或“移动到媒体夹”。但是如何在终端上做到这一点呢？我将在本文中解释这一点。

## 如何在 Linux 中删除目录

在 Linux 中有两种删除目录的方法:命令 **rm** 和 **rmdir** 。

TL；两个命令的 DR 都是 **rm** 删除可能包含文件、子目录等内容的目录， **rmdir** 只删除空目录。

此外，这两个命令都会永久删除目录(而不是将它们移动到回收站)，所以在使用它们时要小心。

让我们更详细地看一下这两个命令。

## 如何使用 Linux 的`rm`命令

在 Linux 中，使用`rm`命令删除文件和目录。对于目录，此命令可用于完全删除目录，也就是说，它删除一个目录以及该目录中的所有文件和子目录。

以下是该命令的语法:

```
rm [options] [files and/or directories] 
```

要删除一个文件，比如说`test.txt`，您可以使用不带选项的命令，如下所示:

```
rm test.txt 
```

对于目录，您必须提供一些标志选项。

### 如何删除包含内容的文件夹

对于包含内容的目录，您必须提供`-r`标志。如果不像这样使用这个标志:

```
rm test 
```

您将得到这个错误: **rm: test:是一个目录**

`-r`标志通知`rm`命令递归删除目录的内容(无论是文件还是子目录)。因此，您可以像这样删除一个目录:

```
rm -r test 
```

### 如何删除空文件夹

对于空文件夹，您仍然可以提供`-r`标志，但是专用的`-d`标志适用于这种情况。如果没有这个标志，你会得到同样的错误**RM:[文件夹]:是一个目录**。

要删除空目录，可以使用以下命令:

```
rm -d test 
```

建议对空目录使用`-d`标志，而不是`-r`标志，因为`-d`标志确保目录为空。

如果不是空的，你将得到错误**RM:test:Directory not empty**。因此，为了确保您正在执行正确的空目录操作，请使用`-d`标志。

## 如何使用 Linux 的`rmdir`命令

`rmdir`命令专门用于删除空目录。语法是:

```
rmdir [folders] 
```

它相当于带有`-d`标志的`rm`命令:`rm -d`。

当你在一个非空的目录上使用`rmdir`时，你会得到这个错误: **rmdir: [folder]:目录不是空的**。

要删除空目录，请使用不带选项的命令:

```
rmdir test 
```

`rmdir`命令也有`-p`标志，允许您删除树中的一个目录及其父目录。例如，如果您有这样的文件结构:

```
> Test
---> Test22 
```

在这种情况下， **Test** 是一个拥有 **Test2** 子目录的目录。如果你删除了 **Test2** 目录， **Test** 成为一个空目录。所以我们不做:

```
rmdir Test/Test2 Test
# deleting Test2 and then Test 
```

您可以像这样使用`-p`标志:

```
rmdir -p Test/Test2 
```

该命令将删除**测试 2** ，然后删除**测试**，树中的父级。但是如果两个目录都不为空，这个命令将抛出一个错误。

## 如何在 Linux 中删除与模式匹配的目录

您也可以将 **rm** 和 **rmdir** 用于 glob 模式。 [Globbing 类似于 Regex](https://dillionmegida.com/p/regex-vs-glob-patterns/) ，但是前者是用来在终端中匹配文件名的。

例如，如果您想删除目录 **test1** 、 **test2** 和 **test3** ，而不是运行:

```
rm -r test1 test2 test3

# or if they are empty

rmdir test1 test2 test3 
```

您可以像这样使用通配符 glob 模式:

```
rm -r test*

# or if they are empty

rmdir test* 
```

**星号*** 匹配“test”单词后的任何混合字符。您也可以应用其他 glob 模式。在 [globbing 文档中了解更多信息](https://linux.die.net/man/7/glob)

## 包裹

现在您知道了如何从命令行删除 Linux 中的目录。您了解了`rm`和`rmdir`命令以及何时使用它们。

编码快乐！