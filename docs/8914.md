# 我觉得自己像夏洛克，如果他是一个开发商

> 原文：<https://www.freecodecamp.org/news/why-i-feel-like-i-am-sherlock-at-my-software-job-4a303ebdaf63/>

作者:达拉·多希

# 我觉得自己像夏洛克，如果他是一个开发商

![1*-CNoGje_a2x2Nziq0pru-w](img/4dcee8fd645367e32b7c36e5a1eb57a0.png)

这是正常的一天。我的老板给了我一个我一无所知的问题。我应该尽快解决这个问题。

大型项目中的某个地方有一段代码一直在崩溃。对我来说，这就像神秘谋杀案一样令人兴奋。

对我来说幸运的是，调试和调查齐头并进。

欢迎来到犯罪现场！

有线索。一些明显的疑点。一些指纹。

但是没有明确的。

我追踪了通常的嫌疑人，但是他们没有给我任何线索。

> "没有什么比一个显而易见的事实更具有欺骗性了。"
> ―[―**亚瑟·柯南道尔**](https://www.goodreads.com/author/show/2448.Arthur_Conan_Doyle)T5[博斯科姆谷之谜](https://www.goodreads.com/work/quotes/1214700)

我向相当于华生医生的人求助:我的思想。

我设置了几个断点。我加几个手表。

我想了更多。

我一遍又一遍地回放犯罪现场，调查事实。

浏览堆栈跟踪，我得到了一个帮助我缩小搜索范围的闪光点。

当我进入一个函数，并添加一个特定的断点时，我感到一阵兴奋。

过了一会儿，我从全神贯注的状态中走出来，解决了这个问题。

你想知道是什么问题吗？

如果你懂一些基本的 C 语言，看看这段代码，看看是否能找到出错的线索:

```
FILE *fd;char *filename="models/";strcat(filename,"bullet");strcat(filename,".h3d");if( (fd = fopen(filename,"r"))==NULL ){    printf("\nFile or Directory not found");    return;}
```

好了，击鼓…问题的起因是这样的:

这是一个[分段故障](https://en.wikipedia.org/wiki/Segmentation_fault)。简单明了。

```
char *filename="models/"; // This is a string literal stored in read-only memory
```

```
When we use strcat to append to "filename", it is undefined behavior because we aren't allowed to write to that read-only memory.
```

那么我是怎么解决的呢？

我分配了一个足够大的内存缓冲区来存储文件的完整路径。

```
char filename[256]; // Alternatively you can allocate dynamically strcpy(filename, "models/");strcat(filename,"bullet");strcat(filename,".h3d");
```

现在你明白我是如何情不自禁地被夏洛克教育了吧。

敬请期待下一个谜团！

[**订阅我的中帖**](https://powered.by.rabbut.com/p/Ntce?c=0)
[*输入你的邮箱接收我的更新。*powered.by.rabbut.com](https://powered.by.rabbut.com/p/Ntce?c=0)