# 升级到 MacOS Sierra 会破坏您的 SSH 密钥，并将您锁定在自己的服务器之外。

> 原文：<https://www.freecodecamp.org/news/upgrading-to-macos-sierra-will-break-your-ssh-keys-and-lock-you-out-of-your-own-servers-f413ac96139a/>

如果你有云服务器(AWS，Digital Ocean 等)，不要升级到 macOS Sierra。)先看这个帖子。它将引导您安全地更新到 Sierra 并更新您的 SSH 密钥。

像许多开发者一样，我收到了苹果公司的通知，让我安装新的 macOS Sierra。我连续几天点击“明天提醒我”。后来有一天晚上睡觉前，我终于屈服了。

当我醒来时，我再也无法访问自由代码营的服务器。我过了一会儿才意识到发生了什么事。幸运的是 [BerkeleyTrue](https://www.freecodecamp.org/news/upgrading-to-macos-sierra-will-break-your-ssh-keys-and-lock-you-out-of-your-own-servers-f413ac96139a/undefined) 还没有升级，可以添加我的新 SSH 密钥。

原来，苹果决定悄悄地强制每个人使用 2048 位 RSA 密钥，这对一些人来说是一种轻微的不便，对另一些人来说则是一种困惑的恐慌。

如果你想知道为什么 RSA 密钥比旧的 DSA 密钥更安全，那么[它们本来就不安全](http://security.stackexchange.com/a/5100)。但是 DSA 密钥通常只能是 1024 位，而 RSA 密钥可以更长，Sierra 默认的 2048 位 RSA 密钥就是这种情况。这些额外的比特使得这些新密钥更难破解。

让我们设置新的 2048 位 RSA SSH 密钥。

### 第一步:删除你的旧密钥并创建一个新的

首先，让我们检查并确定你确实需要一把新钥匙。

打开您的终端并键入:

```
ssh-keygen -l -f ~/.ssh/id_rsa.pub
```

如果提示符响应以“2048 SHA256”开头的字符串，那么您就完成了，不需要采取任何进一步的操作。

否则，通过运行以下命令创建一个新密钥:

```
ssh-keygen -t rsa
```

该提示应响应为:

```
Generating public/private rsa key pair.Enter file in which to save the key (/Users/freecodecamp/.ssh/id_rsa):
```

你只要按下回车键就可以把它保存在默认的地方。请注意，这将覆盖您的旧(损坏的)密钥。

```
Enter passphrase (empty for no passphrase):
```

您可以将此字段留空，或者添加一个密码以获得额外的安全性(并且需要更多的输入)。

然后你会得到一个很酷的随机“艺术”,它看起来总是像一棵圣诞树:

![1*NkRrhr4WF93hhtIKS2eIrg](img/4ac42feb4f978dbb4c5db82512862c93.png)

This was created for this article — it’s not the one I use.

现在，通过运行以下命令，确保您的密钥具有正确的访问权限:

```
sudo chmod 600 ~/.ssh/id_rsa
```

您可以通过运行以下命令来检查您的公钥的内容:

```
cat ~/.ssh/id_rsa.pub
```

它应该会返回如下内容:

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDijWK+s3ybgzEdaJ5LneNU11BsIyoNS51SV11Vi5auPJW9+Ji6OUSJ9OguZh4T019ULyFF/Qq66fhH9TvMzw80lTNoChgTRMpjs2+Qg75yTINKSde+Gv4TK6UvNw6EINORcTpb32Im9hgtdTj6WqJ/hCbSltv7IfFZU5ChV7SxTaoNZTa9M5H3N8YdQ/aGt3puh222Cq5DTjV8fRWaNVvjVQRe/huHAHEzEUr1T/eTlXtoFtGeC1z+pLfYllVzizoS7tyuUksfgqox1jJJMpaZ25V/R/p/MDUc936za/8zgB8OQFRBbrP6JvXXN99DLcvs9coz9vfb2GCVrhxi1aJ5 quincy@FreeCodeCamp
```

你需要把这个密钥放在你的服务器上。为了确保您复制了全部内容，我建议您可以通过运行以下命令将它直接复制到您的剪贴板:

```
pbcopy < ~/.ssh/id_rsa.pub
```

### 步骤 2:将新的公钥添加到服务器中

如果你可以在没有密钥的情况下 SSH 到你的服务器，那么如果你有密码的话，试着使用密码来访问。

否则，您需要请其他能够访问服务器的人来为您完成这项工作。

如果您已经禁用了对服务器的密码访问(出于安全原因，许多专家会建议这样做)，您也许可以[暂时重新启用密码访问](http://jeffreifman.com/2016/10/01/fix-macos-sierra-upgrade-breaking-ssh-keys/)。

一旦您获得了对您的服务器的 root 访问权限—假设它是一个 Linux 服务器—您只需要运行以下命令:

```
nano ~/.ssh/authorized_keys
```

这将使用大多数 Linux 发行版中包含的极简文本编辑器“nano”打开您的授权密钥文件。或者您可以使用 Vim。

然后粘贴前面的公共 SSH 密钥。按 control+o 保存更改，然后按 control+x 退出 nano。

断开与服务器的连接。现在，您可以尝试使用新的 SSH 密钥登录了。

### 第 3 步:SSH 进入您的服务器

在中对 SSH 运行以下命令，用服务器的登录名和 IP 地址替换 root@0.0.0.0:

```
ssh -i ~/.ssh/id_rsa root@0.0.0.0
```

您应该获得对服务器的正常 SSH 访问权限，而无需输入密码。

恭喜你！你又回到了昨天的状态，只不过现在苹果不会再烦你升级操作系统了。？

我只写编程和技术。如果你在推特上关注我，我不会浪费你的时间。？