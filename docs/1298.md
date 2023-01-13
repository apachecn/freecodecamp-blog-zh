# 如何使用 SCP 和 FTP 在 Linux 中的服务器之间传输文件

> 原文：<https://www.freecodecamp.org/news/how-to-transfer-files-between-servers-in-linux-using-scp-and-ftp/>

在机器之间传输文件是一项非常常见的操作任务，作为一名开发人员，您会一直这样做。

Linux 提供了许多传输文件的实用程序。在本教程中，我们将讨论`FTP`和`SCP`。许多自动化脚本也部署 FTP 或 SCP 来移动文件。

## 什么是 FTP？

FTP 是一种网络协议，用于通过网络交换文件。它使用端口 21。FTP 使您能够使用`ftp`命令访问远程系统以交换文件。

### FTP 语法

FTP 语法如下所示:

```
ftp host
```

这里，`host`可以是远程主机的主机名或 IP 地址。

### FTP 命令

FTP 命令类似于 Linux 命令。我们将讨论其中的一些。

| 命令 | 使用 |
| --- | --- |
| 打开 | 打开与另一台计算机的远程连接。 |
| 得到 | 将文件从远程系统复制到本地系统。 |
| 放 | 将文件从本地系统复制到远程系统的目录中。 |
| mget | 将多个文件从远程系统传输到本地系统的当前目录。 |
| mput | 将多个文件从本地系统传输到远程系统的目录中。 |
| 再见/辞职 | 准备退出 FTP 环境。 |
| 关闭 | 终止 FTP 连接。 |
| 美国信息交换标准码 | 启用 ASCII 文件传输模式 |
| 二进制的 | 启用二进制文件传输模式。 |

### 如何通过 FTP 传输文件

FTP 提供两种传输模式:ASCII 和二进制。

1.  ASCII 代表**美国信息交换标准码**。用于传输文本文件等普通文件。
2.  ****二进制模式**** :二进制模式用于传输图像等非文本文件。

默认传输模式是 ASCII。

#### 步骤 1–连接到 FTP

在下面的示例中，`hostA`是远程主机。系统将提示您输入用户名和密码。

```
$ ftp hostA
Connected to hostA.
220 hostA FTP server ready.
Name (hostA:user): user
331 Password required for user.
Password: password
230 User user logged in.
Remote system type is LINUX. 
```

一旦连接成功，您会注意到开头的`ftp>`符号。现在我们可以运行 FTP 命令了。

#### 步骤 2–选择文件传输模式

您可以根据文件类型选择模式(二进制或 ASCII)。

```
ftp> ascii
200 Type set to A.
```

#### 步骤 3–传输文件

我们使用`get`命令将文件`sample.txt`从远程 FTP 服务器传输到本地机器。

```
ftp> get sample.txt
200 PORT command successful.
150 Opening ASCII mode data connection for sample.txt (22 bytes).
226 Transfer complete.
local: sample.txt remote: sample.txt
22 bytes received in 0.012 seconds (1.54 Kbytes/s)
```

#### 第四步。结束会话

```
ftp> bye
221-You have transferred 22 bytes in 1 files.
221-Total traffic for this session was 126 bytes in 2 transfers. 221-Thank you for using the FTP service on hostA.
221 Goodbye.
```

### 如何通过 FTP 传输多个文件

批量传输文件有两个命令:`mget`和`mput`。

您使用`mget`下载文件，而使用`mput`上传文件。

```
ftp> mget sample_file.1 sample_file.2
```

To transfer files from remote host to local host.

```
ftp> mput sample_file.1 sample_file.2
```

To transfer files from local host to remote host.

我们刚刚学习的所有步骤都可以放在一个可执行文件中，并可以进行调度。你可以在这里找到自动化的代码[。](https://github.com/zairahira/FTP-Automation-Script)

## 什么是 SCP？

SCP 代表安全拷贝。它使用 SSH 和端口 22。通过 SCP 传输的数据是加密的，嗅探器无法访问。这使得 SCP 非常安全。

您可以使用 SCP 来:

*   将文件从本地机器传输到远程主机。
*   将文件从远程主机传输到本地机器。

### SCP 语法

让我们来探讨一下 SCP 的语法。

```
scp [FLAG] [user@]SOURCE_HOST:]/path/to/file1 [user@]DESTINATION_HOST:]/path/to/file2 
```

*   `[FLAG]`指定可以给予 SCP 的选项。以下是一些关于标志的详细信息:

| 旗 | 描述 |
| --- | --- |
| -r | 递归复制目录。 |
| 问 | 用于隐藏进度条和除错误之外的任何其他信息。 |
| -丙 | 用于在将数据发送到目的地时压缩数据。 |
| -P | 指定目标 SSH 端口。 |
| -p | 保留文件访问时间。 |

*   `[user@]SOURCE_HOST`是源机。
*   `[user@]DESTINATION_HOST:]`是目的机器。

**注意** : *要通过 SCP 传输文件，必须知道凭证，并且用户应该有写权限*。

### 如何通过`SCP`将文件从本地机器传输到远程主机

要将文件传输到远程主机，请使用以下命令:

```
scp source_file.txt remote_username@10.13.13.11:/path/to/remote/directory
```

在上面的命令中，`source_file.txt`是要复制的文件。`Remote_username`是远程主机`10.13.13.11`的用户名。在`:`之后，指定目的地路径。

样本输出:

```
remote_username@10.13.13.11's password:
source_file.txt                             100%    0     0.0KB/s   00:00 
```

文件`source_file.txt`现在将被放置在`/path/to/remote/directory`中。

要复制目录，使用如下所示的`-r`标志。

```
scp -r /local/directory remote_username@10.13.13.11:/path/to/remote/directory
```

### 如何通过`SCP`将文件从远程主机传输到本地机器

要将文件从远程主机传输到本地计算机，请使用以下命令:

```
scp remote_username@10.13.13.11:/remote/source_file.txt /path/to/local/directory
```

> 传输文件时要格外小心，因为 SCP **会覆盖**已经存在的文件。

## 包扎

在本教程中，您学习了如何通过命令行使用 FTP 和 SCP 传输文件和目录。

当自动化时，这些命令在数据仓库、ETL(提取、转换、加载)、报告、存档和批量文件处理中发挥更大的作用。一定要试试这些命令。我们在[推特](https://twitter.com/hira_zaira)上连线吧。