# 开源许可如何工作以及如何将它们添加到您的项目中

> 原文：<https://www.freecodecamp.org/news/how-open-source-licenses-work-and-how-to-add-them-to-your-projects-34310c3cf94/>

by Radu Raicea

最近，对于世界各地的开发人员来说，有一些令人兴奋的消息。脸书改变了他们开发的多个库的许可。他们从**BSD-3+专利**转到了**麻省理工**。

这看起来不错，但这意味着什么呢？不同的开源许可意味着什么？

本文将让您快速了解流行的许可证。它还会教你如何将它们应用到 GitHub 上的开源项目中。

### 当局

最流行的开源许可证有一个重要的共同点。开源倡议组织已经批准了它们。

OSI 成立于 1998 年，目标是促进开源软件。它创建了[开源定义](https://opensource.org/osd) (OSD)来定义开源软件的含义。

他们是这样描述自己的:

> 开放源码倡议(OSI)是一个全球性的非营利组织，旨在教育和宣传开放源码的好处，并在开放源码社区的不同支持者之间搭建桥梁。

### 许可证

大多数开源许可证包括以下声明:

1.  软件可以修改、商业使用和分发。
2.  软件可以私下修改和使用。
3.  软件中必须包含许可证和版权声明。
4.  软件作者不对软件提供任何担保，也不对任何事情负责。

我们将按照从最严格到最宽松的顺序(从用户的角度)讨论最流行的许可。

#### GNU 通用公共许可证，第 3 版(GPLv3)

GPLv3 是最严格的许可证之一。它为软件的作者提供了高度的保护。

*   无论何时发布软件，源代码都必须公开。
*   软件的修改必须在**相同许可**下发布。
*   对源代码**所做的更改必须被记录**。
*   如果在软件的创作中使用了专利材料，它就授予用户使用它的权利。如果用户起诉任何人使用专利材料，他们就失去了使用软件的权利。

[**GPLv2**](https://www.gnu.org/licenses/gpl-2.0.html) 也很受欢迎。与 GPLv3 的主要区别在于专利授权条款。

该条款在第三版中被加入，以防止公司向用户收取专利使用费。

使用 GPLv3 的热门项目有 [**Bash**](https://www.gnu.org/software/bash/) 和 [**GIMP**](https://www.gimp.org) 。 [**Linux**](https://github.com/torvalds/linux) 使用 GPLv2。

Ezequiel Foncubierta 指出了 GPL 许可证的重要之处:

> 您的源代码的许可证必须与您链接的开放源代码的许可证兼容。例如，如果你的代码是专有的，你就不能使用 GPL 许可下的库。这是人们容易犯更多错误的地方。

#### Apache 许可证 2.0

Apache License 2.0 为用户提供了更多的灵活性。

*   当发布软件时，源代码**不需要公开**。
*   对软件的修改可以在**任何许可**下发布。
*   对源代码**的更改必须**记录在案。
*   它提供与 GPLv3 相同的专利使用保护。
*   它明确禁止在项目中使用商标名称。

使用 Apache License 2.0 的热门项目有[**Android**](https://github.com/aosp-mirror/platform_system_core/blob/master/NOTICE)[**Apache**](https://httpd.apache.org)[**Swift**](https://github.com/apple/swift)。

#### 伯克利软件分发(BSD)

BSD 有两个主要版本: [2 条款](https://opensource.org/licenses/BSD-2-Clause)和 [3 条款](https://opensource.org/licenses/BSD-3-Clause)。它们都比 Apache License 2.0 为用户提供了更多的灵活性。

*   当发布软件时，源代码**不需要公开**。
*   对软件的修改可以在**任何许可**下发布。
*   对源代码**所做的更改可能会被** **而不是**记录下来。
*   它没有对专利的使用提供明确的立场。
*   许可证和版权声明必须包含在源代码的**编译版本**的文档中(而不是仅包含在源代码中)。
*   BSD 3 条款规定，未经许可，不得使用作者和贡献者的姓名来推广由该软件衍生的产品。

使用 BSD 许可证的热门项目有 [**Go**](https://github.com/golang/go) (3 条款) [**Pure.css**](https://github.com/yahoo/pure) (3 条款) [**Sentry**](https://github.com/getsentry/sentry) (3 条款)。

#### 带许可证

MIT 是最宽松的许可证之一。也是最受欢迎的一个。它对软件作者的保护非常低。

*   当发布软件时，源代码**不需要公开**。
*   对软件的修改可以在**任何许可**下发布。
*   对源代码**所做的更改可能会被** **而不是**记录下来。
*   它没有对专利的使用提供明确的立场。

使用 MIT 的热门项目有[**【angular . js】**](https://github.com/angular/angular.js)[**jQuery**](https://github.com/jquery/jquery)[**Rails**](https://github.com/rails/rails)[**Bootstrap**](https://github.com/twbs/bootstrap)等等。

脸书的 [**React.js**](https://github.com/facebook/react) 拥有截止到 9 月 25 日的 BSD-3+专利许可。它将 BSD-3 许可证与专利使用的额外条款结合起来。

简而言之，如果你起诉脸书或其任何子公司，你将失去使用 React(或同一许可下的任何其他软件)的权利。

React 现在是麻省理工学院许可的。你现在可以起诉脸书，但仍然可以使用 React。真是松了一口气！

### 将许可证应用于您的开源项目

许可你的项目很容易。您需要在您的存储库的根目录中添加一个`LICENSE`、`LICENSE.txt`或`LICENSE.md`。

GitHub 让这变得更加简单:

1.  在浏览器中打开您的 GitHub 资源库。
2.  在根目录下，点击`**Create new file**`。
3.  将文件命名为“许可证”。
4.  点击`**Choose a license template**`。
5.  选择其中一个许可证(本文中提到的所有许可证都在这里)。
6.  一旦选定，点击`**Review and submit**`。
7.  `**Commit**`文件。

### 总之…

*   限制性最强的许可证之一是 **GPL** 。
*   最宽松的许可证之一是麻省理工学院的。
*   其他流行的许可还有 **Apache License 2.0** 和 **BSD** 。
*   要在 GitHub 项目上应用许可证，您需要使用 GitHub 的许可证模板创建一个`LICENSE`文件。

**看看我的[解释](https://medium.freecodecamp.org/how-i-used-python-to-find-interesting-people-on-medium-be9261b924b0)我是如何使用 Python 在媒体上寻找有趣的人来关注的！**

更多更新，请在 Twitter 上关注我。