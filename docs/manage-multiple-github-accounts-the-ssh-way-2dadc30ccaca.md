# 如何使用 SSH 密钥在一台机器上管理多个 GitHub 帐户

> 原文：<https://www.freecodecamp.org/news/manage-multiple-github-accounts-the-ssh-way-2dadc30ccaca/>

对于大多数开发人员来说，在同一台机器上管理多个 GitHub 帐户的需求有时会出现。每次我碰巧换了我的 Mac 或者需要用一个新的工作账户来推送信息时，我都会在网上搜索我已经做过六次的事情的操作方法。

我不记录过程的懒惰和不记得步骤的能力使我花了相当多的时间从网上收集零碎的东西，然后设法让它工作。

我相信你们中有很多人曾经去过那里，做过那件事，还有更多人只是在等待下一次同样的事情发生(包括我自己！).这种努力是为了帮助我们所有人。

#### 1.生成 SSH 密钥

在生成 SSH 密钥之前，我们可以检查是否有任何现有的 SSH 密钥:`ls -al ~/.ssh`这将列出所有现有的公钥和私钥对，如果有的话。

如果`~/.ssh/id_rsa`可用，我们可以重用它，或者我们可以首先通过运行以下命令生成默认`~/.ssh/id_rsa`的密钥:

```
ssh-keygen -t rsa
```

当询问保存密钥的位置时，按 enter 键接受默认位置。私钥和公钥`~/.ssh/id_rsa.pub`将在默认的 ssh 位置`~/.ssh/`创建。

让我们将这个默认密钥对用于我们的个人帐户。

对于工作帐户，我们将创建不同的 SSH 密钥。下面的代码将生成 SSH 密钥，并将带有标签*“email @ work _ mail . com”*的公钥保存到`~/.ssh/id_rsa_work_user1.pub`

```
$ ssh-keygen -t rsa -C "email@work_mail.com" -f "id_rsa_work_user1" 
```

我们创建了两个不同的密钥:

```
~/.ssh/id_rsa
~/.ssh/id_rsa_work_user1
```

#### 2.将新的 SSH 密钥添加到相应的 GitHub 帐户

我们已经准备好了 SSH 公钥，我们将要求我们的 GitHub 帐户信任我们创建的密钥。这是为了避免每次推送 Git 时都需要输入用户名和密码。

复制公钥`pbcopy < ~/.ssh/id_rsa.` pub，然后登录个人 GitHub 账户:

1.  转到`Settings`
2.  从左侧菜单中选择`SSH and GPG keys`。
3.  点击`New SSH key`，提供一个合适的标题，并将密钥粘贴到下面的框中
4.  点击`Add key`——就大功告成了！

> 对于工作帐户，使用相应的公钥(`pbcopy < ~/.ssh/id_rsa_work_user1.` pub)并在您的 GitHub 工作帐户中重复上述步骤。

#### 3 .向 ssh 代理注册新的 SSH 密钥

要使用这些密钥，我们必须在我们的机器上向 **ssh-agent** 注册它们。使用命令`eval "$(ssh-agent -s)"`确保 ssh-agent 正在运行。
向 ssh-agent 添加密钥，如下所示:

```
ssh-add ~/.ssh/id_rsa
ssh-add ~/.ssh/id_rsa_work_user1
```

让 ssh 代理为不同的 SSH 主机使用各自的 SSH 密钥。

这是至关重要的部分，我们有两种不同的方法:

使用 ssh 配置文件(步骤 4)，并且在 SSH 代理中一次只有一个活动的 SSH 密钥(步骤 5)。

### **4。创建 SSH 配置文件**

在这里，我们实际上是为不同的主机添加 SSH 配置规则，说明哪个身份文件用于哪个域。

SSH 配置文件将在 **~/处提供。ssh/config** 。如果存在就编辑它，否则我们可以直接创建它。

```
$ cd ~/.ssh/
$ touch config           // Creates the file if not exists
$ code config            // Opens the file in VS code, use any editor
```

在您的`~/.ssh/config`文件中为相关的 GitHub 帐户创建与下面类似的配置条目:

```
# Personal account, - the default config
Host github.com
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa

# Work account-1
Host github.com-work_user1    
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_work_user1
```

“ **work_user1** ”是工作账户的 GitHub 用户 id。

“**g*ithub . com-*work _ user 1**”是用于区分多个 Git 账户的符号。你也可以使用"**work _ user 1 . g*ithub . com "***符号。确保您使用的主机名符号是一致的。当您克隆存储库或为本地存储库设置远程源时，这一点很重要

上述配置要求 ssh-agent:

*   使用 **id_rsa** 作为的密钥任何使用 **@github.com** 的 Git URL
*   对任何使用 **@github.com-work_user1** 的 Git URL 使用 **id_rsa_work_user1** 密钥

### 5.ssh 代理中一次一个活动的 SSH 密钥

这种方法不需要 SSH 配置规则。相反，我们手动确保 ssh-agent 在任何 Git 操作时只附加了相关的密钥。

`ssh-add -l`将列出连接到 ssh-agent 的所有 SSH 密钥。将它们全部删除，并添加您将要使用的一个密钥。

如果是你要推送的个人 Git 账户:

```
$ ssh-add -D            //removes all ssh entries from the ssh-agent
$ ssh-add ~/.ssh/id_rsa                 // Adds the relevant ssh key
```

ssh-agent 现在有了与个人 GitHub 帐户映射的密钥，我们可以对个人存储库进行 Git 推送。

要推送至您的工作 GitHub 帐户-1，请通过删除现有密钥并添加与 GitHub 工作帐户映射的 ssh 密钥来更改与 ssh-agent 映射的 SSH 密钥。

```
$ ssh-add -D
$ ssh-add ~/.ssh/id_rsa_work_user1
```

ssh-agent 目前拥有映射到 work Github 帐户的密钥，您可以将 Git 推送到 work repository。不过，这需要一些手工操作。

#### 为本地存储库设置 git 远程 Url

一旦我们克隆/创建了本地 Git 存储库，确保 Git 配置用户名和电子邮件正是您想要的。GitHub 从提交描述附带的电子邮件 id 中识别任何提交的作者。

要列出本地 Git 目录中的配置名和电子邮件，请执行`*git config user.name*`和`*git config user.email*`。如果没有找到，就相应地更新。

```
git config user.name "User 1"   // Updates git config user name
git config user.email "user1@workMail.com"
```

### 6.克隆存储库时

注意:如果我们在本地已经有了存储库，第 7 步会有帮助。

现在配置已经就绪，我们可以继续克隆相应的存储库了。在克隆时，请注意我们使用了在 SSH 配置中使用的主机名。

可以使用 Git 提供的克隆命令克隆存储库:

```
git clone git@github.com:personal_account_name/repo_name.git
```

工作存储库需要使用以下命令进行更改:

```
git clone git@github.com-work_user1:work_user1/repo_name.git
```

这种改变取决于 SSH 配置中定义的主机名。@和:之间的字符串应该匹配我们在 SSH 配置文件中给出的内容。

### 7.对于本地现有的储存库

**如果我们已经克隆了存储库:**

列出存储库的 Git remote，`git remote -v`

检查 URL 是否匹配我们要使用的 GitHub 主机，否则更新远程源 URL。

```
git remote set-url origin git@github.com-worker_user1:worker_user1/repo_name.git
```

确保@和:之间的字符串匹配我们在 SSH 配置中给定的主机。

**如果您在本地创建新的存储库:**

在项目文件夹`git init`中初始化 Git。

在 GitHub 帐户中创建新的存储库，然后将其作为 Git remote 添加到本地存储库中。

```
git remote add origin git@github.com-work_user1:work_user1/repo_name.git 
```

确保@和:之间的字符串匹配我们在 SSH 配置中给定的主机。

将初始提交推送到 GitHub 存储库:

```
git add .
git commit -m "Initial commit"
git push -u origin master
```

我们在一起

使用正确的主机添加或更新本地 Git 目录的 Git remote 将负责选择正确的 SSH 密钥来验证我们与 GitHub 的身份。有了以上所有的东西，我们的`git operations`应该可以无缝地工作。