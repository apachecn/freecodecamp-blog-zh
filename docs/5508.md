# 如何使用未发布的节点依赖关系

> 原文：<https://www.freecodecamp.org/news/working-with-unpublished-node-dependencies-f396ea1a363a/>

作者桑托什·雷迪

# 如何使用未发布的节点依赖关系

![8elaD68FlZDebmV8XZa8ciXKjFwBe6zbc4Ye](img/ed0b18c65155a680f12dac1c8df43351.png)

Photo by [JJ Ying](https://unsplash.com/photos/PDxYfXVlK2M?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/link?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果您是 Node.js 开发人员，那么您肯定会遇到想要使用另一个节点依赖项中未完成的特性的情况。

让我们对此稍加阐述。例如，您的整个项目在逻辑上分为 4 个 npm 模块。一个模块是主要模块，依赖于其他 3 个模块。在这种设置下，您可能需要修改子模块中的代码，并检查它是否与您的主节点模块配合良好。

最简单的方法是将模块发布到 npm。在主节点模块中使用新版本。这种方法的缺点是，如果您在子模块中犯了错误，您必须重新发布并相应地使用它们。但是，事情并不止于此。您必须重复这一过程，直到您的主节点模块稳定下来。头疼。对吗？我知道。

那么，我们如何解决这个问题呢？

#### 使用 npm 链接

使用这种方法，您可以处理任何节点依赖项，如果它们在本地机器的某个位置被检出的话。您所要做的就是在包的根文件夹中运行下面的命令，这是您的主节点模块的一个依赖项。

这是做什么的？如果您曾经从事过基于节点的项目，您会知道有一个 **node_modules** 文件夹，其中包含您安装的依赖项。类似地，依赖项也有一个全局文件夹。上述命令为运行该命令的包创建一个符号链接。您必须在您想要使用名称在`package.json`中的依赖代码的包中再次运行这个命令。

这样，您对依赖节点模块所做的任何更改都可以直接使用，而不必重新安装。使用下面的命令可以缩短上述两个步骤。

```
npm link <relative-path-to-the-dependency>
```

#### 从 github 获取源代码

现在，让我们讨论另一个用例，在这个用例中，你不是处理依赖关系的人，而是你的一个同事。而且他们不想发布代码，直到他们确定这个特性在某种程度上是完整的。

但是你需要这个人的代码来测试任何早期的集成问题。我假设你们都使用 Git 版本控制系统来管理代码。您可以获得您的同事已经推送到 git 的变更，在您的文件中有如下的存储库代码的链接。

package.json

```
'package-name': git@github.com:<repository-name>.git#<branch-name>
```

一旦将上述路径放入`package.json`文件，您需要运行一个干净的`npm install`来从 git 获取最新的代码。

希望你喜欢这篇文章。如果你喜欢它，请鼓掌并与他人分享。

如果你有另一种处理节点依赖的方法，请在下面评论。

*最初发表于[humbleposts.com](http://humbleposts.com/working-with-unpublished-node-dependencies)。*