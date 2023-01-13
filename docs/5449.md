# 马特宏峰深度——项目方面的解释

> 原文：<https://www.freecodecamp.org/news/matterhorn-in-depth-project-aspects-explained-3348f569f30a/>

最近，我发表了一篇关于我的新项目[马特宏峰的](https://github.com/Ethan-Arrowood/matterhorn)[文章](https://medium.freecodecamp.org/announcing-matterhorn-a-node-js-api-server-boilerplate-4994759f1bf6)？，一个 Node.js API 服务器样板文件。它提供了一组固执己见的配置文件和一些基本的示例代码。这些帮助开发人员更快地使用 Node.js 和 TypeScript。

Matterhorn 的灵感来自 Create React App 和 Gatsby CLI 等项目。该项目的目标是消除使用编程工具(如类型系统、测试和林挺框架，甚至基本的持续集成)所需的入门障碍。

这篇博文将回顾马特宏峰的每个核心方面。我将讨论选择框架背后的细节和固执己见的决定。

### 运行时和类型系统

这个项目的核心是用 Node.js 构建的，node . js 是一个基于 Chrome 的 V8 JavaScript 引擎构建的 JavaScript 运行时。建议您使用 Node.js 的最新稳定版本来运行该项目。写这篇文章的时候，是 **11.7.0** 。

Node.js 由非阻塞事件循环驱动，这使它成为构建可伸缩网络应用程序的绝佳选择。关于 Node.js 的更多信息，请查看他们的网站。

许多 Node.js 项目都是用 JavaScript 编写的。然而，JavaScript 的类型系统 TypeScript 在 2018 年底出现了关注高峰。2019 年很多开发者都有兴趣学习 TypeScript。它在开源 JavaScript 项目中的应用越来越多。Matterhorn 最初的目的是帮助那些对使用 TypeScript 构建后端 Node.js 应用程序感兴趣的开发人员起步。因此，Matterhorn 本身是用 TypeScript 编写的。

作为一个类型系统，TypeScript 是非常全面的。虽然一开始可能有一个陡峭的学习曲线，但使用它的好处是至关重要的。它帮助你，开发者，写出更干净，更少错误的代码。一旦您熟悉了生态系统和配置流程，您编写新功能的速度将比使用原生 JavaScript 更快。像 [VSCode](https://code.visualstudio.com/) 这样的编辑器默认启用了 TypeScript。它提供了一套广泛的开发人员工具来进一步改善开发人员的体验。

### API 框架

虽然只使用 Node.js 编写 HTTP API 是可能的，但如果开发人员希望实现生态系统的可维护性、安全性和可伸缩性，他们应该使用 API 框架。说到 Node.js API 框架，有很多可供选择，比如 Express、Koa 和哈比神。但是有一个框架比其他框架更快更有弹性: [Fastify](https://www.fastify.io/) 。

![Q8r068DOB8vOuyi-G1DtNEQWMUW-so1bn6xN](img/7b532e2486e055184ffa9415f0a3dbf2.png)

Fastify logo from [https://www.fastify.io](https://www.fastify.io)

Fastify 是一个快速和低开销的 web 框架，用于 Node.js。它受哈比神和 Express 的启发，在基于插件的架构上运行。它有一个非常健康的开源社区，超过 90 个公共插件，从身份验证到数据库绑定，等等。此外，Fastify 维护着自己的一组 TypeScript 绑定，这些绑定是直接从 NPM 随模块一起提供的。

### 测试转轮和棉绒

用单元测试备份代码是当今编程生态系统中的标准。Matterhorn 附带 Jest，这是一个流行的 JavaScript 测试程序。它被配置为使用 TypeScript，甚至包含一些测试 Fastify API 的示例。注意 Fastify 的`inject`方法；这对于测试您的路由行为非常有用。

除了运行测试，Jest 还被配置为输出代码覆盖文档。虽然代码覆盖率不是编写单元测试时要考虑的最重要的指标，但它是有价值的，可以帮助您验证您至少覆盖了尽可能多的代码库。

在开源社区中，代码棉条是实施某种编程风格的流行选择。它们否定了风格代码评审的需要。它们可以帮助开发人员在运行代码之前发现代码中的错误。

Matterhorn 配备了 ESLint，这是 JavaScript 林挺的一个流行选择。该项目最初是与 TSLint 一起发布的。然而，这被换成了支持 ESLint，因为 TypeScript [正式宣布](https://github.com/Microsoft/TypeScript/issues/29288)计划直接支持 ESLint 项目。linter 的配置符合项目维护者的意见。它可以很容易地重新配置为您自己的风格指南。

### 连续累计

Matterhorn 的最后一个方面是包含了一个完全配置的持续集成管道。对于很多开发者来说，尤其是学习或者只是修修补补的人，这个特性可能没有太大用处。然而，对于那些试图开发一个完整的应用程序，并希望企业开发的稳定性，这个 CI 是为他们准备的。

管道建立在 Azure DevOps(以前称为 Visual Studio Team Services)上。Azure DevOps 对公共存储库是免费的，并且管道工具是广泛的。它可以通过编程( [Matterhorn](https://github.com/Ethan-Arrowood/matterhorn/blob/master/azure-pipelines.yml) )或通过可视化编辑器(在浏览器中)进行配置。你可以在这里查看 Matterhorn 的 CI 渠道。它在*主机*上自动构建拉请求更新和任何新的提交。

### 结论

感谢您花时间阅读马特宏峰的各个方面。在为本项目选择服务和实用模块时，需要考虑很多因素。该项目是开源的，有很大的改进空间，所以如果你想贡献，请查看下面。

[**Ethan-Arrowood/matterhorn**](https://github.com/Ethan-Arrowood/matterhorn)
[*一个基于 Node.js 和 TypeScript 构建的 API 样板工程？——伊森-阿罗伍德/马特霍恩*ithub.com](https://github.com/Ethan-Arrowood/matterhorn)