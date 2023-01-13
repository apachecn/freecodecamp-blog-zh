# 用 ChatGPT AI 进行结对编程——GPT 3.5 理解 Bash 吗？

> 原文：<https://www.freecodecamp.org/news/pair-programming-with-the-chatgpt-ai-how-well-does-gpt-3-5-understand-bash/>

几周以来，我们一直听到很多关于人工智能和自然语言处理的说法，特别是关于 OpenAI 的 GPT 的最新版本。

最近发布的 GPT 3.5，特别是非常新的 ChatGPT 工具，无疑是一个巨大的飞跃。

你可能已经读到有人要求人工智能起草房地产合同和遗嘱，甚至生成编程代码。本文从一个稍微不同的角度来探讨这个工具。

如果你想用它来补充你的学习，你可以在这里观看这篇文章的视频版本:

[https://www.youtube.com/embed/GxJKuINwPSw?feature=oembed](https://www.youtube.com/embed/GxJKuINwPSw?feature=oembed)

## 尝试 ChatGPT 和 Bash 脚本

我决定看看 ChatGPT 如何处理 Linux Bash 脚本。但是(剧透预警！)当我研究该工具的建议时——好代码和坏代码——我开始意识到还有一个更强大的好处摆在我面前:有机会与一个非常有帮助的伙伴结对编程。

人工智能最终没能解决我的问题，但它失败的方式绝对令人着迷。看着人工智能寻找新的答案是绝对疯狂的。

### 第一次尝试——ChatGPT 尝试解决问题

让我们来看看这一切是如何发生的。这里，或多或少，是我提供给 AI 的初始提示:

> 给我写一个 Bash 脚本，提示输入 YouTube 视频的 URL，然后使用 yt-dlp 程序从该 URL 下载。然后，脚本应该将文件名更改为“myvideo.mp4”

几秒钟后，ChatGPT 告诉我一个脚本来了。下面是这段代码的样子:

```
#!/bin/bash

# Prompt user for YouTube video URL
read -p "Enter YouTube video URL: " url

# Download video from YouTube using yt-dlp
yt-dlp $url

# Get the name of the downloaded video file
yt-dlp $url

# Change the downloaded video filename to "myvideo.mkv"
mv video-filename.mp4 myvideo.mp4
```

人工智能显然知道 Bash 脚本是什么，以及如何呈现代码。它甚至很好地注释了每一行代码。AI 知道如何正确地提示用户输入，然后如何将输入字符串导入到一个名为`url`的变量中。

它还知道 yt-dlp 程序以及它是如何工作的，适当地向它提供所提供的 URL。然后，它至少尝试将下载的视频文件重命名为 myvideo.mp4。但是当它假设下载文件名是 video-filename.mp4 时，一切都崩溃了。哎呦。

无论如何，人工智能向我展示了如何从代码中创建脚本，然后运行它。

### 第二次尝试–chat GPT 自我纠正

我的第一个后续提示指出了这个明显的错误，几秒钟后，ChatGPT 承认这是错误的，并提出了改进。

> 你是对的，我提供的脚本不会像预期的那样工作，因为它假设下载的视频将被保存到名为`video-filename.mp4`的文件中。然而，这并不能保证。来解决这个问题...

这只是令人兴奋的人工智能是如何完全参与到这个过程中的。它记得它的第一个代码，倾听并理解我的抱怨，并思考解决方案。这是第二次尝试:

```
#!/bin/bash

# Prompt user for YouTube video URL
read -p "Enter YouTube video URL: " url

# Download video from YouTube using yt-dlp
yt-dlp $url

# Get the name of the downloaded video file
filename=$(basename $url)

# Change the downloaded video filename to "myvideo.mkv"
mv $filename.mp4 myvideo.mp4
```

好的，我可以看到这个解决方案看起来是正确的。是一个 Linux 工具，它可以去掉文件系统目录，只留下实际的文件名。

问题是变量`url`不包含本地文件名，而是一个互联网地址。这个肯定不行。

### 第三次尝试–ChatGPT 再次尝试

所以我又试了一次，解释了为什么这个工具不适合这项工作。ChatGPT 又做了一次尝试。

```
#!/bin/bash

# Prompt user for YouTube video URL
read -p "Enter YouTube video URL: " url

# Extract the filename from the URL
filename=$(basename $url)

# Download the video using the yt-dlp tool and specify the filename
yt-dlp -o $filename $url

# Change the downloaded video filename to "myvideo.mkv"
mv $filename.mp4 myvideo.mp4
```

这一次，它表明它已经熟悉了 yt-dlp 文档，建议我们使用`-o`参数给下载的视频起一个名字，现在`basename`应该对这个名字起作用。每次迭代，人工智能都会给我更新的代码，让我享受剪切和粘贴的乐趣。

当然，这也是行不通的。随着下一次尝试，我感觉这个人工智能有点绝望。

### 如果一开始你没有成功...至少你会学到一些东西

我将跳过接下来的一两个尝试，因为我们遇到了一个小问题:来自 YouTube 的视频并不总是 MP4，它们有时是 mkv 甚至 WEBMs。这种不确定性可能会扰乱我们正在处理的文件名，所以我让 ChatGPT 将这一点融入到代码中。

```
#!/bin/bash

# Prompt user for YouTube video URL
read -p "Enter YouTube video URL: " url

# Extract the filename from the URL
filename=$(basename $url)

# Download the video using the yt-dlp tool and specify the filename
yt-dlp -F mp4 -O $filename $url

# Change the downloaded video filename to "myvideo.mkv"
mv $filename.mp4 myvideo.mp4
```

当我报告说这也行不通时，人工智能开始感到困惑。首先它告诉我,`-F`选项期望一个有效的视频格式，但是“mp4”是无效的。然后它给我有效的选项，包括“mp4”。

当人工智能建议的一切都不起作用时，它放弃了，建议我确保 yt-dlp 确实安装在我的机器上，并告诉我如何做。

我觉得这是放弃的好时机。这是一个令人惊讶的好努力，即使它没有解决问题，它确实向我展示了一些有用的语法，并让我思考。

如果你感兴趣的话，顺便说一下，这是一个实际的工作脚本的样子。

```
#!/bin/bash

# Prompt user for YouTube video URL
read -p "Enter YouTube video URL: " url

# Download video from YouTube using yt-dlp
yt-dlp $url

# Get the name of the downloaded video file
original_filename=$(ls | awk '/mp4/ || /webm/ || /mkv/')

# Change the downloaded video filename to "myvideo.mkv"
mv "$original_filename" /home/ubuntu/vids/myvideo.mp4
```

前两行代码正是 ChatGPT 建议的方式。但是我随后通过列出当前目录中的所有文件并使用`awk`过滤仅包含 mp4、webm 或 mkv 的文件名来隔离文件名。(这是假设目录中一次不会有一个以上的视频文件。)

文件名将被写入`original_filename`变量。然后，我将使用该变量作为一个`move`命令的一部分来重命名文件`myvideo.mp4`，据我所知，无论实际发送的是什么格式，这都会起作用。

## 包扎

人工智能今天可能还没有为现实世界的编程任务做好准备，但我不想打赌它明天就能实现...或者明年的某个时候。

与此同时，无论何时当你面临一个具有挑战性的编程任务时，我绝对推荐使用 ChatGPT 这样的工具。

两个人的想法(几乎)总是比一个人好。

*除了我的[我的 YouTube 频道](https://www.youtube.com/@davidbclinton)(你可以订阅)，还有[我的网站](https://bootstrap-it.com)链接到各种可用的技术，如文章、书籍和课程。*