# 如何使用 SFTP 与远程服务器安全传输文件

> 原文：<https://www.freecodecamp.org/news/how-to-use-sftp-to-securely-transfer-files-with-a-remote-server/>

本文是关于如何使用安全文件传输协议(SFTP)与服务器交换文件的快速教程。这对于编程很有用，因为它允许您在本地编码和测试，然后在完成后将您的工作发送到服务器。

### **测试 SSH**

如果您还没有，请测试您是否能够 SSH 到服务器。SFTP 使用安全外壳(SSH)协议，所以如果你不能 SSH，你可能也不能 SFTP。

```
ssh your_username@hostname_or_ip_address
```

### **开始 SFTP 会议**

这使用与 SSH 相同的语法，并打开一个会话，您可以在其中传输文件。

```
sftp your_username@hostname_or_ip_address
```

要列出有用的命令:

```
help
```

### **传送文件和文件夹**

要下载文件:

```
get <filename>
```

要下载文件夹及其内容，请使用“-r”标志(也适用于上传):

```
get -r <foldername>
```

要上传文件:

```
put <filename>
```

### **更改文件夹**

要更改本地文件夹:

```
lcd <path/to/folder>
```

要更改远程文件夹:

```
cd <path/to/folder>
```