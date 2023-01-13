# 如何忽略 npm 包中的文件

> 原文：<https://www.freecodecamp.org/news/how-to-ignore-files-from-your-npm-package-4724e6d9575d/>

您可以通过三种方式决定人们下载您的 npm 软件包时会获得哪些文件:

1.  用`.gitignore`文件
2.  用`.npmignore`文件
3.  用`files`属性

我们将查看每种方法，并讨论您应该(或不应该)使用哪些方法。

### 使用 gitignore 排除文件

首先，npm 将检查您的存储库中的`.gitignore`文件。如果有一个`.gitignore`文件，npm 将根据`.gitignore`文件中列出的内容忽略文件。

这是软件包作者阻止人们下载额外文件的最常见方式。

让我们看一个简单的例子。假设您有以下目录结构。

```
- project-name/   |- index.js   |- package.json   |- node_modules/
```

假设你不希望人们下载`node_modules`文件夹。您也不希望将`node_modules`保存在 Git 存储库中。

您要做的是创建一个`.gitignore`文件。

```
# .gitignore node_modules
```

在这种情况下，Git 和 npm 都会忽略`node_modules`文件夹。

### 使用 npmignore 将文件列入黑名单

第二种方法是用一个`.npmignore`文件将文件列入黑名单。`.npmignore`文件的工作方式与`.gitignore`文件相同。如果一个文件列在`.npmignore`文件中，该文件将被从包中排除。

**重要提示:**如果你有一个`.npmignore`文件，npm 会使用`.npmignore`文件。npm 将完全忽略`.gitignore`文件。(许多开发人员错误地认为 npm 会同时使用`.npmignore`和`.gitignore`文件。不要犯同样的错误！)

如果您想将文件从包中排除，但仍然将它们保留在 Git 存储库中，那么您可以使用这个方法。

让我们看另一个例子。假设您已经为您的包编写了测试，并将它们都放在一个`tests`文件夹中。这是您的目录结构:

```
- project-name/  |- index.js |- package.json |- node_modules/ |- tests/
```

您希望从您的 Git 存储库和包中排除`node_modules`。

您希望将`tests`包含在您的 Git 存储库中，但是将其从包中排除。

如果您选择了`npmignore`文件方法，您可以将这些写在您的`.gitignore`和`.npmignore`文件中:

```
# .gitignore node_modules
```

```
# .npmignore node_modules tests
```

### 使用文件属性将文件列入白名单

第三种方法是**将您希望成为**的**文件加入到`package.json`文件的**中，放在`files`属性下。

注:npm 将优先考虑这种方法，而不是上面提到的其他方法。这是限制其他人下载文件的最简单的方法。

这种方法非常简单。您需要的是在`package.json`文件中创建一个`files`属性。然后，提供您想要包含的文件列表。

这里有一个例子:

```
{   "files": [      "index.js"   ] }
```

注意:有些文件，像`package.json`，是[总是被](https://docs.npmjs.com/files/package.json)包含在一个包里。您不必将这些文件写在`files`属性中。

### 用哪种方法？

这三种方法都有效。挑一个你觉得最舒服的。对于简单的项目，`.gitignore`文件方法应该足够了。

如果您的项目更高级，您可能希望用`.npmignore`将文件列入黑名单，或者用`files`属性将文件列入白名单。选一个。两者都不需要。

### 快速提示

你可以使用`npm pack`来生成一个包。这个包包括其他人将得到的文件。

```
npm pack
```

试试看！

感谢阅读。这篇文章对你有帮助吗？如果是的话，我希望你能考虑分享它。你可能会帮助其他人。非常感谢！

本文原载于 [*我的博客*](https://zellwk.com/blog/ignoring-files-from-npm-package/) *。*
如果你想要更多的文章来帮助你成为一个更好的前端开发者，注册我的[简讯](https://zellwk.com/)。