# 如何在 POSIX 系统中用 direnv 管理目录范围的变量

> 原文：<https://www.freecodecamp.org/news/how-to-manage-directory-scoped-envs-with-direnv-in-posix-systems/>

软件项目中的一个常见做法是将某些信息与使用它的代码库分开，但可以从代码库中访问这些信息。开发人员通常使用密码或私钥等秘密，或者特定于用户或上下文的信息片段来实现这一点。

但是管理环境变量可能是一件痛苦的事情。有很多解决方案可以减轻这种痛苦，甚至还有内置的解决方案，比如 [bash_profile](https://www.baeldung.com/linux/bashrc-vs-bash-profile-vs-profile) 。

我最近发现的一个特别方便的解决方案是 [direnv](https://github.com/direnv/direnv) 。它是一个外壳扩展，允许你定义目录范围内的环境变量。

在将扩展安装并挂接到您的 shell 之后，`direnv`将在您每次更改目录时执行，在相同或更高级的目录树中查找`.envrc`文件。然后，它将已定义的变量加载到当前环境，如果不再检测到相同的`.envrc`，则卸载它们。

注意，`direnv`将加载第一个检测到的`.envrc`文件，这意味着*环境将* **而不是** *从父目录*中的 `.envrc` *继承值。*

同样重要的是要记住，只有当您移动到受 `.envrc` *文件*影响的目录时，环境变量*才会加载到您的 shell 会话中。因此，如果您尝试运行一个脚本，加载在当前级别下的目录中定义的环境，这些变量将无法访问。*

## 如何安装`direnv`

这里有一个受支持系统的列表。很可能您的基于 UNIX 的系统的主要开源包管理器已经提供了它。

假设我们在 Debian 上，我们可以通过在终端中运行标准的外部包安装命令来安装`direnv`:

```
sudo apt-get install direnv 
```

## 如何设置`direnv`

安装好之后，你需要把`direnv`挂在你的外壳上。如果您正在使用 bash，您可以通过将这一行附加到 shell 启动配置文件的末尾来实现这一点:

```
echo 'eval "$(direnv hook bash)"' >> ~/.bashrc 
```

对于 ZShell 来说几乎是一样的:

```
echo 'eval "$(direnv hook zsh)"' >> ~/.zshrc 
```

Direnv 也支持鱼、TCSH 和精灵语。[这里是每个支持外壳](https://direnv.net/docs/hook.html)的挂钩说明。

## 如何使用`direnv`

现在，我们必须为我们想要确定环境变量范围的目录创建一个`.envrc`文件。

假设我们为目录`~/project`创建它。

```
echo export FOO='I love Linux!' >> ~/project/.envrc 
```

然后你会收到一个警告，当前的`.envrc`没有被读取。`direnv`将在每次检测到未被明确允许的更改时阻止加载`.envrc`。所以现在运行:

```
direnv allow ~/project 
```

瞧！，您现在有了一个目录范围的环境。

还记得我告诉过你'`direnv`会在每次检测到没有明确允许的改变时阻止加载`.envrc`？这并不仅限于新引入的更改，整个文件都将是未经授权的。所以当你这样做的时候:

```
echo export BAR='It is actually called GNU/Linux!' >> ~/project/.envrc 
```

您将不得不再次运行`direnv allow ~/project`，甚至访问`$FOO`。有点无聊，但偏向安全。

每次加载一个`.envrc`时，direnv 会输出一条消息，包括文件路径和加载变量的名称，所以你不必担心忘记你的设置。它还会告诉您环境何时被卸载。

### 感谢阅读！

就是这样！它非常简单，我希望你和我一样觉得方便。