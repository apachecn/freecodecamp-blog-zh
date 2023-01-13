# 什么是校验和？如何在 Linux 中使用 cksum 命令检查文件是否被修改

> 原文：<https://www.freecodecamp.org/news/file-last-modified-in-inux-how-to-check-if-two-files-are-same/>

当您在命令行上处理文件时，您可能需要检查它们的修改时间和内容完整性。

Linux 有一个强大的命令行，允许您探索文件和文件系统的多个方面。

如果您需要检查文件是否被修改，您可以遵循以下两种方法:

## 如何通过检查修改时间来检查文件是否被修改

编辑文件时，其时间戳会发生变化以匹配修改时间。

我们可以使用长列表(`ls -l`)来查看文件的最后修改时间。

在下面的输出中，我们可以看到文件在`Jul 19 13:22`被修改。

```
zaira@Zaira:~$ ls -lrt | grep calculator.py
-rw-r--r--  1 zaira zaira  263 Jul 19 13:22 calculator.py
```

File modification time is `Jul 19 13:22`

## 如何通过检查文件大小来检查文件是否被修改

如果我们知道文件以前的大小，我们可以将其与当前的文件大小进行比较，以查看是否发生了更改。

我们可以使用长列表(`ls -l`)来查看文件大小。第 5 列显示了文件的大小，以字节为单位。

```
zaira@Zaira:~$ ls -lrt | grep calculator.py
-rw-r--r--  1 zaira zaira  263 Jul 19 13:22 calculator.py
```

File size is 263 bytes

上面提到的方法通常可以完成任务，但是有一种高级的方法可以使用散列来检查文件的完整性。该方法被称为“checksum ”,在 Linux 中对应的命令是`cksum`。

## Linux 中的校验和是什么？

有时数据在传输或存储过程中会被破坏。为了确保数据保持一致，我们可以使用校验和。

校验和是一种称为加密哈希函数的算法的结果。它应用于文件中的数据块。

在网络中，您可以使用校验和来比较发送方和接收方的哈希值。如果哈希值相同，这意味着您的文件副本是真实的，没有错误。

一些常用的加密散列函数包括 MD5 和 SHA-1。

接下来我们将看到如何在 Linux 中计算散列。

## 如何在 Linux 中使用`cksum`找到校验和

**`cksum`** 是在类似*nix 的操作系统中发现的命令，它为文件或数据流生成校验和值。

根据`cksum`手册页，该命令打印每个文件的 CRC(循环冗余校验)校验和以及字节数。

要了解更多关于 CRC 算法的信息，请参考[本页](https://www.tutorialspoint.com/what-is-algorithm-for-computing-the-crc)。

### `cksum`的语法

`cksum`命令将文件名作为参数，并生成它的哈希值。基本语法如下:

```
cksum [FILE]
```

### 如何使用`cksum`

假设我们有一个名为`calculator.py`的文件。我们可以这样计算它的校验和:

```
zaira@Zaira:~$ cksum calculator.py
1991291549 262 calculator.py
```

在输出中，我们得到三列:

*   第一列是哈希值。
*   第二个值是给定文件的数据量，以字节为单位。
*   第三列是文件名。

即使是轻微的修改也会改变哈希值。让我们通过一个例子来看看它是怎样的。

让我们修改我们的原始文件`calculator.py`,在末尾增加一行:

```
zaira@Zaira:~$ echo >> "this file is now changed" >> calculator.py
```

让我们再次计算校验和，看看哈希值是否发生了变化:

```
zaira@Zaira:~$ cksum calculator.py
331872555 263 calculator.py
```

第一列是哈希值，自从我们添加文本后，它已经发生了变化。

现在，我们知道文件已经更改，因为校验和哈希值不再相同。

我们可以使用相同的方法来比较不同机器上具有相同名称、大小和修改时间的文件，以确保两个文件是相同的。

## 结论

有些情况下，您需要跨系统比较文件，特别是当它们从一个位置转移到另一个位置时。我们可以结合使用这三种方法来验证我们的文件是否完好无损:

*   查看文件修改时间。
*   验证文件大小。
*   使用`cksum`生成并比较哈希值。

我希望这篇教程对你有所帮助。谢谢你一直读到最后。

你从这个教程中学到的最喜欢的东西是什么？在 [Twitter](https://twitter.com/hira_zaira) 上告诉我！

你也可以在这里阅读我的其他帖子[。](https://www.freecodecamp.org/news/author/zaira/)