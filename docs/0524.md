# 如何用 git-story 动画化 Git 提交历史

> 原文：<https://www.freecodecamp.org/news/animate-your-git-repo-with-git-story/>

对开发人员来说，可视化他们代码项目的各个方面通常是很有用的。对于 Git 这样的版本控制系统来说尤其如此，理解团队的工作流是必不可少的。

在 Git 中实现这一点的一种方法是绘制一个类似上面看到的图。

当[学习如何使用 Git](https://initialcommit.com/cluster/git-commands-git-cheat-sheets) 时，你可能会遇到这样的图片。

该图描述了 Git 存储库中的一个示例提交历史。

在 Git 中，提交历史被表示为 DAG，或**有向无环图**，这是一种网络图。每个提交都显示为一个圆圈，并且使用箭头将提交链接在一起。每个箭头都从子提交指向其直接父提交。

在大多数情况下，如果你想要这些图表中的一个，你需要自己画(手动或数字)——这需要时间和精力。此外，这些图形是静态的，在某些情况下，以视频格式显示提交进度会更有吸引力。

出于这些原因，我创建了 **[Git Story](https://initialcommit.com/tools/git-story)** ，它让您可以轻松地生成 mp4 视频，展示您的 Git 提交历史的布局和进展，所有这些都使用一个命令:`git-story`。

## 如何可视化 Git 的修订历史

在 Git 中，提交也称为修订，代表特定人在特定时间点所做的一组内容更改。

有几个附加组件可以帮助我们直观地理解我们的 Git 历史。

### 提交属性

在 Git 中提交时，可读性很重要。因此，最好留下一条清晰的[提交消息](https://initialcommit.com/blog/git-commit-messages-best-practices)来描述您的更改。这有助于其他查看提交历史的开发人员理解每个修订的目的。

提交 ID 是每个提交的唯一标识符，专门从每个提交的内容中生成。作为 Git 用户，您可能已经看到许多 Git 命令接受完整或部分提交 ID 作为参数。因此，在 Git 可视化中提供提交 ID 非常有用。

### 参考:分支、头和标签

在可视化 Git 历史时，了解自己所处的位置也很有用。Git refs(references 的缩写)帮助您理解提交在您的 Git 存储库中是如何组织的。

Refs 是 Git 附加到特定提交上的标签。分支名称、 [Git HEAD](https://initialcommit.com/blog/what-is-git-head) 和标签都是 Git 中 refs 的例子。您可以将 ref 视为指向特定提交的人类可读名称。

### Git 故事视频输出

**[Git Story](https://initialcommit.com/tools/git-story)** 为你解析所有这些信息——提交、关系和引用——并制作成动画视频`.mp4`。最好的部分是，您需要做的就是在终端中浏览到您的项目并运行命令`git-story`。

以下是 Git Story 生成的视频动画示例:

[https://www.youtube.com/embed/fI9D-c9wgPs?feature=oembed](https://www.youtube.com/embed/fI9D-c9wgPs?feature=oembed)

Git Story Example `.mp4` Video Output

## 为什么我用 Python 写 Git Story

我选择用 Python 编写 Git Story，因为有两个非常有用的库支持这个项目。

### GitPython

第一个叫做 [GitPython](https://gitpython.readthedocs.io/en/stable/intro.html) 。GitPython 是一个 Python 包，它通过一组方便的方法提供对来自 Git 存储库的数据的访问。这是 Git Story 如何从本地 Git repo 访问数据，以制作动画:

```
import git

repo = git.Repo(search_parent_directories=True)
```

这在 Python 程序的内存中创建了一个存储库对象，允许访问底层 Git 对象及其属性。可以按如下方式访问提交列表:

```
commits = list(repo.iter_commits(REF))

# This pulls a list of commits working backwards from REF
# REF can be a branch name, tag, HEAD, or commit ID
```

遍历提交列表允许访问每个提交的属性，这些属性将在下一步中用于生成动画。

### Manim

第二个依赖项叫做 [Manim](https://www.manim.community/) ，用于使用 Python API 生成动画数学视频。Manim 使得创建表示线条、形状、文本和方程的 Python 对象变得容易，并将这些对象放在动画场景中。

Git Story 使用 Manim 绘制圆圈、箭头、文本、 [Git 分支](https://initialcommit.com/blog/git-branches)名称和 refs，它们代表使用 GitPython 获得的 Git 历史的一部分。

下面是 Python 代码如何使用 Manim 为每次提交创建一个红圈:

```
circle = Circle(stroke_color=commitFill, fill_color=commitFill, fill_opacity=0.25)
```

下面是如何使用 Manim 来创建提交之间的箭头:

```
arrow = Arrow(start=RIGHT, end=LEFT, color=self.fontColor).next_to(circle, LEFT, buff=0)
```

最后，下面是如何将这些对象添加到动画场景中:

```
self.play(Create(circle), Create(arrow))
```

## 如何安装 Git Story

1.  为您的操作系统安装 [manim 和 manim 依赖项](https://www.manim.community/)
2.  安装 GitPython: `$ pip3 install gitpython`
3.  安装 git-story: `$ pip3 install git-story`

## 如何使用 Git Story

1.  打开命令行终端
2.  使用`cd`导航到 Git 项目的根文件夹
3.  执行命令:`git-story`

运行这个命令将从 Git repo 中最近的 8 次提交中生成一个 mp4 视频动画。视频文件将被放置在当前目录的路径`./git-story_media/videos`下。

## 如何定制 Git Story 的输出

Git Story 包括各种方法来修改您的 Git repo 在输出视频中的表示方式。这可以通过命令行选项和标志来实现。

要指定要制作动画的提交次数，请使用选项`--commits=X`，其中`X`是要显示的提交次数。

除了`HEAD`之外，你可能想从一个特定的提交开始制作动画。您可以使用`--commit-id=ref`选项来选择从哪个提交开始向后工作。除了`ref`，您可以用完整或部分提交 ID、分支名称或 [Git 标签](https://initialcommit.com/blog/git-tag)来代替。

您可以使用`--reverse`标志将提交从最新到最早动画化，也就是逆时间顺序，以更好地匹配 [Git 日志](https://initialcommit.com/blog/git-log)的输出。

测试时尝试使用`--low-quality`标志来加快动画生成时间。一旦你对动画的外观感到满意，移除标志并再次运行命令以获得最终的最佳质量版本。

如果你喜欢浅色而不是默认的深色，你可以指定`--light-mode`标志。

出于演示的目的，您可能希望在动画中添加介绍标题、徽标和结尾。您可以使用以下选项来实现这一点:

```
$ git-story --show-intro --title "My Git Repo" --show-outro --outro-top-text "My Git Repo" --outro-bottom-text "Thanks for watching!" --logo path/to/logo.png
```

使用`--media-dir=path/to/output`选项设置视频的输出路径。如果您不想在项目中创建额外的文件，这很有用。

最后，如果您的 Git 提交历史有多个分支，您可能想要测试一下`--invert-branches`标志。此标志将翻转分支解析的顺序，从而改变输出视频中的分支方向。有些动画设置了这个标志会看起来更好，有些不会。

下面是最后一个示例，展示了在输入提交范围内存在多个分支时生成的视频:

[https://www.youtube.com/embed/0uj5jRfOaZc?feature=oembed](https://www.youtube.com/embed/0uj5jRfOaZc?feature=oembed)

Git Story Example `.mp4` Video Output with Multiple Branches

## 摘要

Git Story 是我用 Python 编写的一个命令行工具，它使创建 Git 提交历史的视频动画变得容易。依赖项包括 Manim 和 GitPython。

输出视频显示了所需的一组提交及其关系，以及分支名称、`HEAD`提交和标签。

Git Story 有各种命令行标志和选项，允许您自定义动画。运行命令`git-story -h`获得完整的可用选项。

如有任何意见、问题或建议，请随时发送电子邮件至 [jacob@initialcommit.io](mailto:jacob@initialcommit.io) 给我。

在 [Git Story GitHub 页面](https://github.com/initialcommit-com/git-story)上，拉取请求非常受欢迎。

感谢阅读和快乐编码！