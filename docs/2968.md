# Git 二分法如何让调试变得更容易

> 原文：<https://www.freecodecamp.org/news/how-git-bisect-makes-debugging-easier/>

Git 二等分是一个神奇的工具，可以让调试变得轻而易举。但是很少有人主动使用。

在这篇简短的文章中，我将展示 *git 等分*如何轻松地指出您的错误所在。

但是首先，让我们谈谈...

### 增量调试

Delta 调试是完成许多调试步骤的过程，在每个步骤中，你的目标是消除一半的“问题”。你可以把它想象成调试的二分搜索法。或者正如安德烈亚斯·泽勒(这个术语的发明者)所说:

> Delta 调试自动化了科学的调试方法。科学方法的基本思想是建立一个假设，解释为什么某些东西不起作用。您测试这个假设，并根据测试结果改进或拒绝它。调试时，人们一直在这样做。手动。Delta 调试自动化了这个过程。

Git 二等分是我们如何用 Git 应用 delta 调试的。

让我们假设我们有一个错误，我们试图找到根本原因。在我们研究解决方案的每一步中，我们都要消除一半的解决方案空间。配置、代码、输入...任何事。下面举个例子让大家更清楚。

### Git 二等分示例

首先，我们需要初始化一个存储库来跟踪我们的工作。

```
mkdir test_git_bisect && cd test_git_bisect && git init 
```

假设我们要编写一个脚本，它获取一个纪元并将其转换为

```
datetime 
```

我们通过使用一个输入文件(名为 epochs.txt)来做到这一点，该文件中的*应该只包含 epochs。*

请注意，为了平稳地运行一个平分的 *git，我们需要有相当多的提交。*

我们将在这里使用的 python 脚本`parse_epochs.py`没什么特别的。

```
 from time import localtime, strftime

with open('epochs.txt', 'r') as handler:
    epochs = handler.readlines()
    for epoch in epochs:
        current_datetime = strftime('%Y-%m-%d %H:%M:%S', localtime(int(epoch)))
        print(current_datetime) 
```

让我们提交第一个更改:

`git add . && git commit -m "Created epoch parser"`

然后创建输入:

`for i in {1..100}; do   sleep 3;   date +%s >> epochs.txt; done`

这基本上是从我们开始脚本的时间(加 3 秒)到 5 分钟后的所有时期，步长为 3 秒。

我们再次提交变更:

`git add . && git commit -m "Generated the first version of input"`

如果我们现在运行初始脚本，我们将得到所有解析到日期的输入:

```
$ python3 parse_epochs.py
2020-07-21 16:08:39
2020-07-21 16:10:40
2020-07-21 16:10:43
2020-07-21 16:10:46
2020-07-21 16:10:49
2020-07-21 16:10:52
... 
```

现在让我们修改输入，使其出错:

```
echo "random string" >> epochs.txt 
```

并再次提交:

```
git add . && git commit -m "Added faulty input" 
```

出于熵的考虑，为了使这个例子更复杂，让我们添加更多的错误输入——提交。

```
echo "This is not an epoch" >> epochs.txt 
&& git add . && git commit -m "Added faulty input v2" 
```

```
echo "Stop this, the script will break" >> epochs.txt
&& git add . && git commit -m "Added faulty input v3" 
```

以下是我们创建的提交日志:

```
$ git log --pretty=format:"%h - %an, %ar : %s"
b811d35 - Periklis Gkolias, 2 minutes ago: Added faulty input v3
dbf75cd - Periklis Gkolias, 2 minutes ago: Added faulty input v2
cbfa2f5 - Periklis Gkolias, 8 minutes ago: Added faulty input
d02eae8 - Periklis Gkolias, 20 minutes ago: Generated first version of input
a969f3d - Periklis Gkolias, 26 minutes ago: Created epoch parser 
```

如果我们再次运行该脚本，它显然会失败，并出现以下错误:

```
Traceback (most recent call last):
  File "parse_epochs.py", line 6, in <module>
    current_datetime = strftime('%Y-%m-%d %H:%M:%S', localtime(int(epoch)))
ValueError: invalid literal for int() with base 10: 'random string\n' 
```

看起来我们需要 *git 平分*来解决这个问题。为此，我们需要开始调查:

`git bisect start`

并将一个提交标记为坏的(通常是最后一个)，将一个提交标记为好的。这将是我们生成输入时的第二次提交:

```
git bisect bad b811d35 && git bisect good d02eae8 
```

之后，git 二等分会将历史分为好的和坏的提交。您可以通过执行`git bisect visualize`来查看被认为是罪犯的提交，并且

```
git show 
```

要打印当前签出的文件，在我们的例子中是这个文件:

```
dbf75cd 
```

如果我们运行脚本，它仍然会失败。因此，我们将当前提交标记为坏提交:

`git bisect bad dbf75cd`

在这种情况下，值得一提的是 Git 的输出:

```
git bisect bad dbf75cd
Bisecting: 0 revisions left to test after this (roughly 0 steps)
[cbfa2f5f52b7e8a0c3a510a151ac7653377cfae1] Added faulty input 
```

Git 知道我们快到了。耶！

如果我们再次运行这个脚本，它当然会失败。如果我们把它标记为坏的，Git 会说:

```
git bisect bad cbfa2f5
cbfa2f5f52b7e8a0c3a510a151ac7653377cfae1 is the first bad commit 
```

到那时，你可以修复错误或者联系任何犯下错误的代码/输入/配置的人。以下是获取详细信息的方法:

```
$ git show -s --format='%an, %ae' cbfa2f5
Periklis Gkolias, myemail@domain.com 
```

## 结论

感谢您阅读这篇文章。欢迎分享您对这个伟大工具的想法。