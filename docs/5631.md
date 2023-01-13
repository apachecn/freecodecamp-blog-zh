# 宣布马特宏峰？Node.js API 服务器样板文件

> 原文：<https://www.freecodecamp.org/news/announcing-matterhorn-a-node-js-api-server-boilerplate-4994759f1bf6/>

开发商节日快乐？最近我发表了 M [atterhorn？，](https://github.com/Ethan-Arrowood/matterhorn)用 Node.js 和 TypeScript 构建的 API 样板工程。API 服务器使用 Fastify，这是一个快速且低开销的 web 框架。该项目附带了一个已配置的类型系统(TypeScript)、测试运行器(Jest)、linter (TSLint)，甚至还有一个 CI 管道(Azure DevOps)。

本文将给出项目的简要概述，以及对某些设计决策的见解。

[**Ethan-Arrowood/matterhorn**](https://github.com/Ethan-Arrowood/matterhorn)
[*一个基于 Node.js 和 TypeScript 构建的 API 样板工程？——伊森-阿罗伍德/马特霍恩*ithub.com](https://github.com/Ethan-Arrowood/matterhorn)

### 概观

> ？嘶！该概述部分与 G [itHub](https://github.com/Ethan-Arrowood/matterhorn#matterhorn-) 上的项目文档非常相似

按照以下步骤快速开始:

1.  ？派生存储库
2.  ？‍♀️把它克隆到你的电脑上
3.  ？□运行 n□□□□□□□
4.  ？编辑 s `rc/`中的任何文件
5.  ？观看应用程序神奇地重建和重新启动自己

✨:基本用户指南到此为止。现在让我们深入了解一些默认情况下可用的命令。以下所有命令都可以用`npm run <scri` pt >运行。这个项目利用 npm mo `dul` es op `n and` rimraf 来启用平台无关的 npm 脚本。

*   `build` —构建类型脚本文件并输出到`lib/`
*   `build:watch` —如果在`src/`中检测到更改，则自动重建文件
*   `clean` —递归删除`lib/`和`coverage/`目录
*   `clean:build` —递归删除`lib/`目录
*   `clean:coverage` —递归删除`coverage/`目录
*   `coverage` —运行测试套件并生成代码覆盖报告
*   `coverage:open` —运行`npm run coverage`，然后在浏览器中打开结果
*   `dev` —同时运行`build:watch`和`start:watch`
*   `lint` —运行`src/`目录下 TSLint 配置的 linter
*   `start` —从`lib/`运行应用程序。一定要先用`npm run build`！
*   `start:watch` —如果在`lib/`中检测到新的变化，则重新启动服务器
*   `test` —运行在`tests/`目录中定义的单元测试
*   `test:ci` —运行单元测试并为 CI 集成生成必要的文件

#### 命令行参数和环境变量

Matterhorn 实现了命令行参数和环境变量的示例用法。它使用`yargs-parser`来管理命令行参数。命令行参数通过 start 命令传入:`node lib/index.js <command line argumen` ts >。

作为示例，`--log`参数已启用。运行`npm run start`在没有任何命令行参数的情况下启动项目。这个命令是为了在生产中使用，所以默认情况下日志是禁用的(也就是说，我们不传递`—-log`参数)。

如果您使用这个命令在本地测试您的代码，并且想要查看日志输出，那么运行`npm run start —- -—log`。这将命令行参数通过 npm 传递到别名命令中。

环境变量的工作方式类似于命令行参数。根据您使用的终端和操作系统，可以通过多种方式进行设置。在 bash 终端中，您可以在使用上述任何脚本时指定环境变量，方法是在命令前添加赋值。

例如，这个项目启用了`PORT` 环境变量。在 bash 终端中运行`PORT=8080 npm run start`来运行端口 8080 上的 API。

### 设计决策

我构建这个项目是因为我发现自己经常为新的 Node.js 项目复制和粘贴配置文件。我喜欢`create-react-app`团队已经完成的工作，并设想 Matterhorn 会发展成一种类似的工具。接下来，我期待开发一个完整的 CLI 来帮助开发人员更快地开始使用 Node.js 和 TypeScript。

马特宏峰是一个固执己见的项目。构建和林挺系统是根据我的喜好配置的，但是很容易更改。例如，在`tslint.json`中，我将`"semicolon"`规则定义为`false`——为了在整个应用中强制使用分号，将其改为`true`。

此外，这个项目包含一个`azure-pipelines.yml`文件。这定义了 Azure DevOps 上的 CI(持续集成)管道，这是一个由微软提供的强大工具，使团队能够更智能地规划，更好地协作，并通过一组现代开发服务更快地交付。由于我使用该工具的经验，这是另一个固执己见的决定。还有许多其他很棒的 CI 选项，如 Travis CI 或 Circle CI，我希望将来能够支持它们。

### 希望你喜欢！

感谢您花时间阅读这篇文章并查看 Matterhorn？。该项目是开源的，我鼓励任何技能水平的开发人员来贡献。在 G [itHub](https://github.com/Ethan-Arrowood/matterhorn) 上查看一下，如果你想了解未来的更新以及我开发的其他东西，请在 T [witter 上关注我。](https://twitter.com/ArrowoodTech)

最美好的祝愿？~伊森·阿罗伍德