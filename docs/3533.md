# 如何使用 Bash 脚本管理从 AWS S3 存储桶下载和查看文件

> 原文：<https://www.freecodecamp.org/news/bash-script-download-view-from-s3-bucket/>

正如你在这篇文章中读到的，我最近的邮件服务器出了点问题，于是我决定将邮件管理外包给亚马逊的简单邮件服务(SES)。

该解决方案的问题是，我让 se 将新消息保存到 S3 存储桶中，而使用 AWS 管理控制台读取 S3 存储桶中的文件很快就会过时。

所以我决定编写一个 Bash 脚本来自动化下载、正确存储和查看新消息的过程。

虽然我写这个脚本是为了在我的 Ubuntu Linux 桌面上使用，但它并不需要太多的改动就可以通过 [Windows 子系统 for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) 在 macOS 或 Windows 10 系统上运行。

这是完整的剧本。在您花一些时间查看它之后，我将一步一步地向您介绍它。

```
#!/bin/bash
# Retrieve new messages from S3 and save to tmpemails/ directory:
aws s3 cp \
   --recursive \
   s3://bucket-name/ \
   /home/david/s3-emails/tmpemails/  \
   --profile myaccount

# Set location variables:
tmp_file_location=/home/david/s3-emails/tmpemails/*
base_location=/home/david/s3-emails/emails/

# Create new directory to store today's messages:
today=$(date +"%m_%d_%Y")
[[ -d ${base_location}/"$today" ]] || mkdir ${base_location}/"$today"

# Give the message files readable names:
for FILE in $tmp_file_location
do
   mv $FILE ${base_location}/${today}/email$(rand)
done

# Open new files in Gedit:
for NEWFILE in ${base_location}/${today}/*
do
   gedit $NEWFILE
done
```

The complete Bash script

我们将从下载当前驻留在我的 S3 桶中的任何消息的单个命令开始(顺便说一下，为了保护我的隐私，我已经更改了桶的名称和其他文件系统和认证细节)。

```
aws s3 cp \
   --recursive \
   s3://bucket-name/ \
   /home/david/s3-emails/tmpemails/  \
   --profile myaccount
```

当然，这只有在您已经为本地系统安装并配置了 AWS CLI 的情况下才有效。如果你还没有这样做，现在是时候了。

*cp* 命令代表“复制”， *-递归*告诉 CLI 甚至对多个对象应用操作， *s3://bucket-name* 指向我的 bucket(你的 bucket 名称显然会不同)，/home/david...line 是我希望消息复制到的绝对文件系统地址， *- profile* 参数告诉 CLI 我指的是我的多个 AWS 帐户中的哪一个。

下一节设置两个变量，这将使我在脚本的其余部分指定文件系统位置变得更加容易。

```
tmp_file_location=/home/david/s3-emails/tmpemails/*
base_location=/home/david/s3-emails/emails/
```

请注意 *tmp_file_location* 变量的值是如何以星号结尾的。那是因为我想引用目录中的*文件，而不是目录本身。*

我将在.../emails/ hierarchy，方便我以后查找邮件。这个新目录的名称将是当前日期。

```
today=$(date +"%m_%d_%Y")
[[ -d ${base_location}/"$today" ]] || mkdir ${base_location}/"$today"
```

我首先创建一个新的 shell 变量，名为今天的*，它将由*日期+"%m_%d_%Y"* 命令的输出填充。 *date* 本身输出完整的日期/时间戳，但是随后的( *"%m_%d_%Y"* )将输出编辑成更简单、更易读的格式。*

*然后，我直接使用该名称测试是否存在——这表明我在那天已经收到了电子邮件，因此，没有必要重新创建目录。如果这样的目录*不存在*(| |)，那么 *mkdir* 会为我创建它。如果不运行这个测试，您的命令可能会返回令人讨厌的错误消息。*

*由于 Amazon SES 给它放入我的 S3 桶中的每条消息都取了丑陋和不可读的名称，我现在将动态地重命名它们，同时将它们移到它们的新家(在我刚刚创建的过时目录中)。*

```
*`for FILE in $tmp_file_location
do
   mv $FILE ${base_location}/${today}/email$(rand)
done`*
```

**为...做...done* 循环将读取由 *$tmp_file_location* 变量表示的目录中的每个文件，然后将其移动到我刚刚创建的目录中(由 *$base_location* 变量表示，此外还有今天的$ *的当前值*)。*

*作为相同操作的一部分，我将赋予它新的名称，字符串“ *email* ”，后跟由 *rand* 命令生成的随机数。你可能需要安装一个随机数生成器:那将是*在 Ubuntu 上安装 rand* 的费用。*

*该脚本的早期版本创建了由更短的序列号区分的名称，这些序列号使用 *count=1 递增...count=$((count+1))* 逻辑内的*为*循环。只要我没有碰巧在同一天收到一批以上的消息，这种方式就很好。如果我这么做了，那么新消息将会覆盖当天目录中的旧文件。*

*我想从数学上来说，我的 *rand* 命令可能会给两个文件分配重叠的数字，但是，鉴于 *rand* 使用的默认范围是 1 到 32，576，我愿意冒这个险。*

*此时，新目录中应该有文件名为 email3039、email25343 等的文件。对于我收到的每一封新邮件。*

*在我自己的系统上运行 *tree* 命令显示，有五条消息保存到了我的 02_27_2020 目录中，还有一条保存到了 02_28_2020 目录中(这些文件是使用我的脚本的旧版本生成的，所以它们是按顺序编号的)。*

*当前在 *tmpemails -* 中没有文件，这是因为 mv 命令将文件移动到新的位置，不会留下任何东西。*

```
*`$ tree
.
├── emails
│   ├── 02_27_2020
│   │   ├── email1
│   │   ├── email2
│   │   ├── email3
│   │   ├── email4
│   │   ├── email5
│   └── 02_28_2020
│       └── email1
└── tmpemails`*
```

*脚本的最后一部分在我最喜欢的桌面文本编辑器(Gedit)中打开每封新邮件。它使用类似的*来...做...完成*循环，这次读取新目录中每个文件的名称(使用“ *today* 命令引用)，然后在 Gedit 中打开文件。请注意我添加到目录位置末尾的星号。*

```
*`for NEWFILE in ${base_location}/${today}/*
do
   gedit $NEWFILE
done`*
```

*还有一件事要做。如果我不清理我的 S3 桶，它会在我每次运行脚本时下载所有累积的消息。这将使管理变得越来越困难。*

*因此，在成功下载我的新邮件后，我运行这个简短的脚本来删除桶中的所有文件:*

```
*`#!/bin/bash
# Delete all existing emails 

aws s3 rm --recursive s3://bucket-name/ --profile myaccount`*
```