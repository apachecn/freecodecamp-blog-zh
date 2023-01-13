# 在 Redhat/Centos Linux 中安装和配置 FTP 服务器

> 原文：<https://www.freecodecamp.org/news/install-and-configure-ftp-server-in-redhat-centos-linux/>

FTP 代表文件传输协议。它由 Abhay Bhushan 撰写，并于 1971 年 4 月 16 日作为 RFC 114 出版。所有操作系统和浏览器都支持它。它建立在客户机-服务器体系结构上。

## **如何在 Redhat/Centos Linux 中安装和配置 FTP 服务器**

步骤 1:我们将使用本地主机来设置 ftp 服务器。

步骤 2:安装 vsftpd(非常安全的 FTP 守护进程)包。

`yum install -y vsftpd`

步骤 3:当系统开启时，启动 FTP 服务器。

`systemctl enable vsftpd.service`

步骤 4:检查 ftp 服务器的状态

`systemctl status vsftpd.service`

步骤 5:配置 vsftpd 包。我们将编辑`/etc/vsftpd/vsftpd.conf`。

`Change the line which contain anonymous_enable=NO to anonymous_enable=YES`
`This will give permit any one to access FTP server with authentication.`
`Change the following to YES`
`local_enable=YES`


第六步:启动 FTP 服务器
`systemctl start vsftpd.service`

第七步:安装 FTP 客户端
`yum install -y lftpd`

步骤 8:将 ftp 连接到本地主机
`lftp localhost`