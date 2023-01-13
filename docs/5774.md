# 什么不能保存到 Git 存储库中

> 原文：<https://www.freecodecamp.org/news/what-not-to-save-into-a-git-repository-29779ee94b96/>

您不应该将这四种类型的文件提交到您的 Git 存储库中。

1.  不属于项目的文件
2.  自动生成的文件
3.  图书馆(视情况而定)
4.  资格证书

### 不属于项目的文件

像`.DS_Store`(用于 Mac OS)、`Thumds.db`(用于 Windows)、`.vscode`(用于代码编辑器)这样的文件与你的项目无关。

它们不应被检入。

### 自动生成的文件

这包括来自预处理程序的文件(比如 Sass 到 CSS)。你不用签入 CSS。你检查一下萨斯文件。

如果您使用像 Webpack 或 Rollup 这样的 JavaScript 编译器，您不会签入生成的 JavaScript 文件。你签入你写的代码。

### 图书馆

如果你不使用包管理器，你应该检查你的库。这是因为如果您想要下载库，您必须:

1.  谷歌图书馆
2.  访问网站
3.  找到链接
4.  下载库
5.  投入到你的项目中

这个过程很繁琐。如果您的代码需要库才能工作，您应该签入该库。

另一方面，如果你使用一个包管理器，你不应该签入一个库，因为你可以用一个简单的命令安装这个库，比如`npm install`。

### 资格证书

您不应该存储用户名、密码、API 密钥和 API 秘密等凭证。

如果其他人窃取了您的凭据，他们可以用它做一些令人讨厌的事情。因为一个朋友不小心暴露了我的亚马逊凭据，我几乎损失了 4 万到 6 万美元。幸运的是，这笔钱被免除了。

如果您不想像我一样陷入困境，那么就不要将您的凭证存储在 Git 存储库中。

感谢阅读。这篇文章对你有什么帮助吗？如果你有，[我希望你能考虑分享它](http://twitter.com/share?text=What%20not%20to%20save%20into%20a%20Git%20repository%20by%20@zellwk%20?%20&url=https://zellwk.com/blog/what-not-to-save-into-a-git-repo/&hashtags=)。你可能会帮助别人。谢谢大家！

本文最初发布在 *[我的博客](https://zellwk.com/blog/what-not-to-save-into-a-git-repo)。*
如果你想要更多的文章来帮助你成为更好的前端开发人员，请注册我的[时事通讯](https://zellwk.com/)。