# Linux 中的 Tar 命令:用示例命令解释 Tar CVF 和 Tar XVF

> 原文：<https://www.freecodecamp.org/news/tar-command-linux-tar-cvf-tar-xvf/>

大多数人认为,`tar`是*磁带存档*的简称。问题中的“磁带”应该是 20 世纪 50 年代流行的所有磁存储驱动器。

这表明`tar`工具可能有点老，已经过了它的全盛时期。但事实是，这些年来，尽管 IT 世界发生了翻天覆地的变化，`tar`并没有失去它的力量和价值。

在本文中，基于我的 [Linux in Action book](https://www.amazon.com/gp/product/1617294934/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1617294934&linkCode=as2&tag=projemun-20&linkId=1a460c0cd9a39e01821133b90632cba8) 中的内容，我将向您展示 tar 归档创建、压缩和恢复的基础知识。让我们从头开始。

# 创建档案

这个例子将获取当前工作目录下的所有文件和目录，并构建一个归档文件，我巧妙地将其命名为`archivename.tar`。

这里，我在 tar 命令后使用了三个参数:

*   `c`告诉 tar 创建一个新的档案，
*   `v`将屏幕输出设置为详细，这样我就可以获得更新，并且
*   指向我要给存档的文件名。

`*`告诉`tar`递归地包含所有文件和本地目录。

```
$ tar cvf archivename.tar *
file1
file2
file3
directory1
directory1/morestuff
directory1/morestuff/file100
directory1/morestuff/file101 
```

tar 命令永远不会移动或删除您提供给它的任何原始目录和文件——它只会创建归档副本。

您还应该注意，使用点(。)代替前面命令中的星号(*)，甚至包括存档中的隐藏文件(文件名以点开头)。

如果您在自己的计算机上继续操作(您肯定应该这样做)，那么您会看到一个名为 archivename.tar 的新文件。的。tar 文件扩展名不是必需的，但是以尽可能多的方式清楚地表达文件的目的总是一个好主意。

提取您的归档文件以恢复文件很容易:只需使用`xvf`而不是`cvf`。在本例中，这将在当前位置保存原始文件和目录的新副本。

```
$ tar xvf archivename.tar
file1
file2
file3
directory1
directory1/morestuff
directory1/morestuff/file100
directory1/morestuff/file101 
```

当然，您也可以让 tar 使用`-C`参数将提取的文件发送到其他地方，后跟目标位置:

```
$ tar xvf archivename.tar -C /home/mydata/oldfiles/ 
```

您不会总是希望将目录树中的所有文件都包含在归档中。

假设您已经制作了一些视频，但是它们当前与各种图形、音频和文本文件(包含您的笔记)一起保存在目录中。您需要备份的唯一文件是使用. mp4 文件扩展名的最终视频剪辑。

下面是如何做到这一点:

```
$ tar cvf archivename.tar *.mp4 
```

太好了。但是那些视频文件太大了。使用压缩使归档文件变得更小不是很好吗？

不要再说了！只需使用`z` (zip)参数运行前面的命令。这将告诉 gzip 程序压缩档案。

如果您想遵循惯例，除了已经存在的`.tar`之外，您还可以添加一个`.gz`扩展。记住:清晰。

接下来是这样的:

```
$ tar czvf archivename.tar.gz *.mp4 
```

如果您在自己的. mp4 文件上尝试这样做，然后在包含新归档文件的目录上运行 ls -l，您可能会注意到,`.tar.gz`文件并不比`.tar`文件小多少，可能小 10%左右。那是怎么回事？

嗯，`.mp4`文件格式本身是压缩的，所以 gzip 做它的事情的空间很小。

因为 tar 完全了解它的 Linux 环境，所以可以使用它来选择位于当前工作目录之外的文件和目录。本示例添加了`/home/myuser/Videos/`目录中的所有`.mp4`文件:

```
$ tar czvf archivename.tar.gz /home/myuser/Videos/*.mp4 
```

因为归档文件可能会变得很大，所以有时将它们分解成多个较小的文件，将它们转移到新的位置，然后在另一端重新创建原始文件可能是有意义的。分割工具就是为此而设计的。

在这个例子中，`-b`告诉 Linux 将 archivename.tar.gz 文件分割成 1 GB 大小的部分。然后，该操作命名每个部分——archive name . tar . gz . partaa、archivename.tar.gz.partab、archivename.tar.gz.partac 等等:

```
$ split -b 1G archivename.tar.gz "archivename.tar.gz.part" 
```

另一方面，通过按顺序读取每个部分(cat archivename.tar.gz.part*)来重新创建归档，然后将输出重定向到名为 archivename.tar.gz 的新文件:

```
$ cat archivename.tar.gz.part* > archivename.tar.gz 
```

# 流式文件系统档案

这里是好东西开始的地方。我将向您展示如何创建一个正在运行的 Linux 安装的归档映像，并将其传输到一个远程存储位置——所有这一切都在一个命令中完成。命令如下:

```
# tar czvf - --one-file-system / /usr /var \
  --exclude=/home/andy/ | ssh username@10.0.3.141 \
  "cat > /home/username/workstation-backup-Apr-10.tar.gz" 
```

我不想马上解释所有这些，我将使用更小的例子来探索它，一次一个。

让我们创建一个名为 importantstuff 的目录的内容归档，其中包含非常重要的内容:

```
$ tar czvf - importantstuff/ | ssh username@10.0.3.141 "cat > /home/username/myfiles.tar.gz"
importantstuff/filename1
importantstuff/filename2
[...]
username@10.0.3.141's password: 
```

让我解释一下这个例子。我没有在命令参数后面输入档案名称(这是您到目前为止一直使用的方法)，而是使用了一个破折号(czvf -)。

仪表板将数据输出到标准输出。它允许您将归档文件名的详细信息推回到命令的末尾，并告诉 tar 期待归档的源内容。

然后，我将这个未命名的压缩档案通过管道(|)传输到一个远程服务器上的 ssh 登录中，在那里我被要求输入密码。

然后，用引号括起来的命令对归档数据流执行 cat，将数据流内容写入远程主机上我的主目录中名为 myfiles.tar.gz 的文件。

以这种方式生成归档文件的一个优点是避免了中间步骤的开销。甚至不需要在本地机器上临时保存存档的副本。想象一下，备份 128 GB 可用空间中的 110 GB。档案会放在哪里？

那只是一个文件目录。假设您需要将一个活动的 Linux 操作系统备份到一个 USB 驱动器上，这样您就可以将它移动到一台单独的机器上，并放到那台机器的主驱动器上。

假设在第二台机器上已经有了相同 Linux 版本的全新安装，下一次复制/粘贴操作将生成第一台机器的精确副本。

> 注意:这在没有安装 Linux 文件系统的目标驱动器上不起作用。为了处理这种情况，您需要使用`dd`。

下一个例子在 USB 驱动器上创建一个名为`/dev/sdc1`的压缩归档文件。

`--one-file-system`参数排除除当前文件系统之外的任何文件系统中的所有数据。这意味着像`/sys/`和`/dev/`这样的伪分区不会被添加到归档文件中。如果您想要包含其他分区(在本例中，您将对`/usr/`和`/var/`执行此操作)，那么应该显式添加它们。

最后，您可以使用`--exclude`参数从当前文件系统中排除数据:

```
# tar czvf /dev/sdc1/workstation-backup-Apr-10.tar.gz \
  --one-file-system \
  / /usr /var \
  --exclude=/home/andy/ 
```

现在让我们回到全服务命令示例。使用您已经学到的知识，归档文件系统的所有重要目录，并将归档文件复制到 USB 驱动器。现在你应该明白了:

```
# tar czvf - --one-file-system / /usr /var \
  --exclude=/home/andy/ | ssh username@10.0.3.141 \
  "cat > /home/username/workstation-backup-Apr-10.tar.gz" 
```

********在我的[bootstrap-it.com](https://bootstrap-it.com/)还有更多以书籍、课程和文章形式提供的管理知识。【T10********