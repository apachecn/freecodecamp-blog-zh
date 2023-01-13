# 如何管理多个 SSH 密钥

> 原文：<https://www.freecodecamp.org/news/how-to-manage-multiple-ssh-keys/>

可以肯定地说，web 领域的大多数开发人员都在某个时候遇到过 SSH。SSH 是最常用的安全数据交换协议之一。您使用 SSH 连接到远程服务器，这也包括使用 Git 管理您的代码以及与远程存储库同步。

尽管每个设备有一个私钥-公钥对被认为是一个好的做法，但有时您需要使用多个密钥和/或您有非正统的密钥名。

您可能使用一个 SSH 密钥对来处理您公司的内部项目，但是您可能使用不同的密钥来访问一些公司客户的服务器。您甚至可以使用不同的密钥来访问您自己的私有服务器。

一旦需要使用第二个密钥，管理 SSH 密钥就会变得很麻烦。我希望这篇文章能对任何有 SSH 密钥管理问题的人有所帮助。

我假设读者对 Git 和 SSH 有基本的了解。整篇文章中的大多数例子都将使用 Git。当然，所有这些都适用于任何其他 SSH 通信。也就是说，其中包含了一些特定于 Git 的技巧。

系好安全带，我们走！

## 处理一个 SSH 密钥

首先，让我们先看看你的工作流程是什么样子，然后再考虑多个关键点。

您有一个存储在`~/.ssh/id_rsa`中的私钥和一个对应的公钥`~/.ssh/id_rsa.pub`。

让我们想象一下，您想要将代码更改推送到远程 Git 服务器，或者从远程 Git 服务器获取代码更改——比如说 GitHub，为什么不呢？为此，您首先必须将您的公钥添加到 GitHub 中。

我不会跳过这一步，找到如何做应该很容易。我还假设你的名字是史蒂夫，你正在从事一个绝密项目，该项目使用覆盆子馅饼来嗅探网络流量。

要开始您的工作，您必须使用 SSH 克隆一个 git 存储库:

```
git clone git@github.com:steve/raspberry-spy.git
```

这时 GitHub 会说:“哟，这是个私人仓库！我们需要使用我这里的公钥和你的私钥来加密流量。”

您已经将公钥添加到您在 GitHub 上的配置文件中，但是 SSH 必须以某种方式找出您对应的私钥所在的位置。

由于我们不知道在 SSH 进入`git@github.com`时应该使用哪个私有密钥，SSH 客户端试图在默认位置找到一个密钥，即`~/.ssh/id_rsa`——这是他的最佳猜测。如果该位置没有文件，您将得到一个错误:

```
Cloning into 'raspberry-spy'...
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

如果您在文件`~/.ssh/id_rsa`中存储了*一些*私钥，SSH 客户端将使用该私钥进行通信加密。如果该密钥有密码(理应如此)，系统会提示您输入密码，如下所示:

```
Enter passphrase for key '/Users/steve/.ssh/id_rsa':
```

如果您输入了正确的密码短语，并且该私钥确实与您附加到您的配置文件的公钥相对应，那么一切都会顺利进行，并且将会成功克隆存储库。

但是如果您用不同的方式命名您的密钥(例如`~/.ssh/_id_rsa`)？SSH 客户端将无法确定私钥存储在哪里。您将得到与之前相同的`Permission denied ...`错误。

如果要使用不同名称的私钥，必须手动添加:

```
ssh-add ~/.ssh/_id_rsa
```

输入密码短语后，您可以通过执行`ssh-add -l`来检查密钥是否被添加到了`ssh-agent` (SSH 客户端)。该命令将列出 SSH 客户端当前可用的所有密钥。

如果您现在尝试克隆存储库，将会成功。

## **到目前为止，一切顺利吗？**

如果你眼光敏锐，你可能会开始注意到一些潜在的问题。

首先，如果你重启你的电脑，`ssh-agent`将会重启，你将不得不用`ssh-add`重新添加你的非默认名称的键，输入密码和所有那些乏味的事情。

我们可以自动添加密钥或者指定在访问某些服务器时使用哪个密钥吗？

我们能不能以某种方式保存密码，这样我们就不用每次都输入密码了？要是有类似于*钥匙链*的东西来保存受密码保护的 SSH 密钥就好了。。

请放心，所有这些问题都有答案。

## **回车，宋承宪`config`**

事实证明， [SSH 配置文件](https://linux.die.net/man/5/ssh_config)是一个东西，一个可以帮助我们的东西。它是用于 SSH 通信的每个用户的配置文件。创建一个新文件:`~/.ssh/config`并打开进行编辑。

### **管理自定义命名的 SSH 密钥**

我们使用这个`config`文件要解决的第一件事是避免使用`ssh-add`添加自定义命名的 SSH 密钥。假设您的 SSH 密钥名为`~/.ssh/_id_rsa`，将以下内容添加到`config`文件中:

```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/_id_rsa
  IdentitiesOnly yes
```

现在通过执行`ssh-add -D`来确保`~/.ssh/_id_rsa`不在`ssh-agent`中。该命令将从当前激活的`ssh-agent`会话中删除所有密钥。每次您注销或重启时(或者如果您手动终止`ssh-agent`进程)，会话都会重置。我们可以通过执行上面提到的命令来“模拟”重启。

如果您现在尝试克隆您的 GitHub 库，它将与我们手动添加密钥一样(就像我们以前做的那样)。您将被要求输入密码:

```
git clone git@github.com:steve/raspberry-spy.git
Cloning into 'raspberry-spy'...
Enter passphrase for key '/Users/steve/.ssh/_id_rsa':
```

您会注意到，提示我们输入密码的密钥与我们在`config`文件中指定的密钥相同。输入正确的 SSH 密钥密码后，将成功克隆存储库。

注意:如果成功克隆后，您尝试`git pull`，系统将再次提示您输入密码。我们稍后会解决这个问题。

来自`config`的`Host github.com`和来自 URI `git@github.com:steve/raspberry-spy.git`的`github.com`匹配很重要。也可以把`config`改成`Host mygithub`，用 URI `git@mygithub:steve/raspberry-spy.git`克隆。

这打开了闸门。当你这么做的时候，你的大脑在飞速运转，想着你所有的关于 SSH 的麻烦是如何结束的。以下是一些有用的配置示例:

```
Host bitbucket-corporate
        HostName bitbucket.org
        User git
        IdentityFile ~/.ssh/id_rsa_corp
        IdentitiesOnly yes
```

现在你可以使用`git clone git@bitbucket-corporate:company/project.git`

```
Host bitbucket-personal
        HostName bitbucket.org
        User git
        IdentityFile ~/.ssh/id_rsa_personal
        IdentitiesOnly yes
```

现在你可以使用`git clone git@bitbucket-personal:steve/other-pi-project.git`

```
Host myserver
        HostName ssh.steve.com
        Port 1111
        IdentityFile ~/.ssh/id_rsa_personal
        IdentitiesOnly yes
        User steve
        IdentitiesOnly yes
```

现在您可以使用`ssh myserver`SSH 进入您的服务器。多酷啊。每次执行`ssh`命令时，不需要手动输入端口和用户名。

### 额外收获:每个存储库的设置

您还可以定义特定存储库应该使用哪个特定的键，覆盖 SSH `config`中的任何内容。具体的 SSH 命令可以通过设置`<project>/.git/config`中`core`下的`sshCommand`来定义。示例:

```
[core]
        sshCommand = ssh -i ~/.ssh/id_rsa_corp
```

这在 git 2.10 或更高版本中是可能的。您也可以使用此命令来避免手动编辑文件:

```
git config core.sshCommand 'ssh -i ~/.ssh/id_rsa_corp'
```

## 密码管理

最后一个难题是管理密码。我们希望避免每次启动 SSH 连接时都必须输入密码。为此，我们可以利用 MacOS 和各种 Linux 发行版附带的钥匙串管理软件。

通过将`-K`选项传递给`ssh-add`命令，开始将您的密钥添加到钥匙串中:

```
ssh-add -K ~/.ssh/id_rsa_whatever
```

现在，您可以在钥匙串中看到您的 SSH 密钥。在 MacOS 上，它看起来像这样:

![Keychain Access](img/6e3f66a38d4e23ba1ca1a5f81903c2e3.png "Keychain Access")

如果您通过`ssh-add -D`从`ssh-agent`移除密钥(如前所述，当您重启电脑时会发生这种情况)并尝试 SSH-ing，您将再次被提示输入密码。为什么？我们刚刚把钥匙添加到钥匙链上。如果您再次检查“钥匙串访问”，您会注意到您使用`ssh-add -K`添加的密钥仍在钥匙串中。很奇怪吧。

原来还有一个铁环要跳过。打开您的 SSH `config`文件并添加以下内容:

```
Host *
  AddKeysToAgent yes
  UseKeychain yes
```

现在，SSH 将在 keychain 中查找密钥，如果找到了，将不会提示您输入密码。密钥也将被添加到`ssh-agent`。在 MacOS 上，这将在 MacOS Sierra 10.12.2 或更高版本上运行。在 Linux 上，你可以使用类似于`gnome-keyring`的东西，即使没有对 SSH `config`的最后修改，它也可能工作。至于 Windows——谁知道呢，对吧？

我希望有人觉得这很有用。现在去配置你的 SSH `config`文件吧！

## 了解有关 SSH 的更多信息:

*   [SSH 密钥的终极指南](https://www.freecodecamp.org/news/the-ultimate-guide-to-ssh-setting-up-ssh-keys/)
*   [自上而下的 SSH 介绍](https://www.freecodecamp.org/news/a-top-down-introduction-to-ssh-965f4fadd32e/)