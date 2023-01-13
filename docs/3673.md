# Git 责备用例子解释

> 原文：<https://www.freecodecamp.org/news/git-blame-explained-with-examples/>

使用`git blame`,你可以一行一行地看到谁修改了特定文件中的什么，如果你在团队中工作，而不是单独工作，这是很有用的。例如，如果一行代码让你想知道它为什么在那里，你可以使用`git blame`，你就会知道你必须问谁。

### **用途**

你这样用`git blame`:`git blame NAME_OF_THE_FILE`

例如:`git blame triple_welcome.rb`

您将看到如下输出:

```
0292b580 (Jane Doe      2018-06-18 00:17:23 -0500 1) 3.times do
e483daf0 (John Doe      2018-06-18 23:50:40 -0500 2)   print 'Welcome '
0292b580 (Jane Doe      2018-06-18 00:17:23 -0500 3) end
```

每一行都标注了 SHA、作者姓名和最后一次提交的日期。

### **别名饭桶怪**

一些程序员不喜欢“责备”这个词，因为“责备某人”会带来负面的含义。此外，该工具很少(如果有的话)用于责备某人，而是用于寻求建议或了解文件的历史。因此，有时人们会用别名将`git blame`改成听起来更好听的名字，如`git who`、`git history`或`git praise`。为此，您只需添加一个 git 别名，如下所示:

`git config --global alias.history blame`

你可以在这里找到更多关于别名 git 命令的信息。

### **利用 Git 的文本编辑器插件指责**

有一些插件可以用于各种利用`git blame`的文本编辑器。例如，为正在检测的当前生产线创建热图或添加内嵌信息。一个著名的例子是 VSCode 的[git lenes](https://gitlens.amod.io/)。