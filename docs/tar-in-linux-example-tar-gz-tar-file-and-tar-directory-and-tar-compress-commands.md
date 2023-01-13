# Linux 中的 Tar——Tar GZ、Tar 文件、Tar 目录和 Tar 压缩命令示例

> 原文：<https://www.freecodecamp.org/news/tar-in-linux-example-tar-gz-tar-file-and-tar-directory-and-tar-compress-commands/>

你想把一堆文件和目录合并成一个文件吗？Linux 中的`tar`命令就是您正在寻找的！

`tar`命令用于将一组文件压缩到一个档案中。该命令还用于提取、维护或修改 tar 归档文件。

Tar 存档将多个文件和/或目录组合成一个文件。Tar 存档不一定是压缩的，但是可以压缩。权限被保留，它支持许多压缩格式。

在这篇简短的文章中学习如何使用`tar`。

## 句法

`tar [options] [archive-file] [file or directory to be archived]`

**选项:**
**-c :** 创建归档文件
**-x :** 提取归档文件
**-f :** 创建具有给定文件名的归档文件
**-t :** 显示或列出归档文件
**-u :** 归档文件并添加到现有的归档文件
**-v :** 显示详细信息
**-A :**
**-j :** 使用 bzip2
**压缩 tar 文件-W :** 验证一个存档文件
**-r :** 更新或添加已经存在的文件或目录。 焦油文件

## 用法示例

**提取一个档案:**
`tar xfv archive.tar`
(选项:x =提取，f =文件，v =详细)

**用文件或文件夹创建一个档案:**
`tar cfv archive.tar file1 file2 file3`
(选项:c =创建)

**创建压缩档案:**
`tar cfzv archive.tar file1 file2 file3`
(选项:z =用 gzip 压缩)

**显示一个归档的所有文件:**
`tar tvf archive.tar`

**创建一个未压缩的存档文件。当前目录下的 txt 文件:**
`tar cfv archive.tar *.txt`

**从 gzip tar Archive Archive . tar . gz:**
`tar xvzf archive.tar.gz`

**使用 bzip2 创建一个压缩的 tar 存档文件:**
`tar cvfj archive.tar.tbz example.cpp`
(选项:j =使用 bzip2 压缩，文件较小，但花费的时间比`-z`长)

**通过将 todo.txt 文件添加到归档文件来更新现有的 tar 文件:**
`tar rvf archive.tar todo.txt`
(选项:r =添加文件)

**列出 tar 文件的内容:**
`tar tf file.tar`
(选项:t =显示，f =文件)

**创建当前目录的压缩档案，但排除某些目录:**
`tar --exclude='./folder' --exclude='./upload/folder2' cfzv archive.tar .`(“文件夹”和“文件夹 2”除外)