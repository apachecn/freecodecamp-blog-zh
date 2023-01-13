# JavaScript 包管理器——NPM 和 Yarn 完全指南

> 原文：<https://www.freecodecamp.org/news/javascript-package-manager-npm-and-yarn/>

一个**包管理器**是开发人员用来自动查找、下载、安装、配置、升级和删除系统包的工具。

这篇文章将告诉你所有你需要开始与包装经理一样，NPM 和纱。

但是为什么我们在开发工作流程中需要一个包管理器呢？让我们找出答案。

## 为什么需要一个包管理器？

假设没有包管理器。在这种情况下，您必须手动执行以下操作:

*   为您的项目找到所有正确的包
*   验证软件包没有任何已知的漏洞
*   下载软件包
*   将它们安装在适当的位置
*   跟踪所有软件包的新更新
*   每当有新版本时，升级每个软件包
*   移除您不再需要的软件包

手动管理数十或数百个包是一件令人厌倦且耗时的工作。

因此，包管理器——如 [NPM](https://www.npmjs.com/) 、 [pNPM](https://pnpm.io/) 、[鲍尔](https://bower.io/)和[纱](https://yarnpkg.com/)——帮助自动化和消除手动管理所有包的繁琐过程。

请记住，包管理器不同于包注册表。那么，让我们找出主要的区别。

## 软件包管理器与软件包注册中心的区别是什么？

一个**软件包管理器**是开发者用来自动查找、下载、安装、配置、升级和卸载计算机软件包的工具。

NPM(节点包管理器)和 Yarn(另一个资源协商器)是两个常用的包管理器。

一个**包注册表**是数千个包(库、插件、框架或工具)的数据库(存储)。

换句话说，包注册中心是包发布和安装的地方。

[NPM 注册表](https://www.npmjs.com/)和 [GitHub 包](https://github.com/features/packages)是两种常用的包注册表。

所以，现在我们知道了什么是包管理器以及为什么需要它，我们可以讨论如何使用两个流行的——NPM 和纱。

请注意，有许多关于 NPM 和纱的争论——所以我们在这里将避免它们，因为最好的包管理器是最适合你的。

因此，本文将向您展示 NPM 和 Yarn 是如何工作的，而不是告诉您哪个包管理器是最好的。然后由你来决定你更喜欢哪一个。

或者，你可以选择让 NPM 负责一个特定的项目，让 Yarn 负责另一个项目——这取决于你认为哪个经理最适合这项工作。

所以，事不宜迟，让我们开始学习如何安装这两个管理器。

## 如何安装节点程序包管理器(NPM)

安装节点时，NPM 会自动安装。

因此，要在您的系统上安装 NPM，请访问 [NodeJS](https://nodejs.org/en/) 网站，获取 Node[的最新 LTS 或当前版本](https://tamalweb.com/which-nodejs-version)。

## 如何安装纱线

最好通过 NPM 安装纱线。所以，首先，从 [Node.js](https://nodejs.org/en/) 网站安装 NPM。

一旦你安装了 NPM，就像这样安装纱线:

```
npm install -g yarn
```

## 如何检查已安装的节点版本

要检查系统上安装的 Node.js 版本，请运行:

```
node -v
```

上面代码片段中的`-v`标志是`--version`的简写。

## 如何检查已安装的 NPM 版本

要检查系统上安装的 NPM 版本，请运行:

```
npm -v
```

## 如何检查安装的纱线版本

要检查系统上安装的 Yarn 版本，请运行:

```
yarn -v
```

## 如何升级节点程序包管理器

运行以下命令，更新至最新的 NPM 版本:

```
npm install npm@latest -g
```

## 如何升级节点

假设您希望升级 Node.js 安装。在这种情况下，您有两个选择:

### 选项 1:通过 NodeJS 网站升级

升级 NodeJS 安装的一个方法是从 [Node.js 网站](https://nodejs.org/en/)手动下载并安装最新版本。

### 选项 2:通过版本管理工具升级

另一种升级 NodeJS 安装的方法是使用一个[版本管理器](https://nodejs.org/en/download/package-manager/)，比如 [NVM](https://github.com/nvm-sh/nvm) 、 [n](https://github.com/tj/n) 或 [nvs](https://github.com/jasongin/nvs) 。

## 如何升级纱线

运行以下命令，更新至最新的 Yarn 版本:

```
yarn set version latest
```

所以，现在我们的计算机上有了 NPM(或 Yarn ),我们可以开始使用安装的管理器来查找、安装、配置和删除我们项目的包。

但是包到底是什么呢？让我们找出答案。

## 包到底是什么？

一个**包**是一个[目录](https://www.codesweetly.com/git-basic-introduction/#h-working-directory)(或者项目)，有一个`package.json`文件用来记录关于它的信息。

**注意:**你只能发布包(一个由`package.json`文件描述的项目)到 [NPM 注册表](https://docs.npmjs.com/cli/v6/using-npm/registry)。

## 如何安装软件包

有两种安装软件包的方法:本地安装或全局安装。

### 本地包安装

本地安装的包只能在安装它的项目中使用。

要在本地安装软件包，请执行以下操作:

1.  从命令行导航到项目的[根目录](https://www.codesweetly.com/web-tech-glossary/#h-root-directory)。
2.  使用下面的 NPM 或 Yarn 安装命令安装您的软件包(取决于您为项目选择的软件包管理器)。

**注意:**您必须在系统上安装 Node 和 NPM，下面的 NPM(和 Yarn)安装命令才能工作。您可以通过从 Node.js 网站安装最新的 LTS 或当前版本来获得这两者。

#### NPM 安装司令部

```
npm install package-name --save
```

注意，上面的`--save`命令指示 NPM 将`package-name`保存在`package.json`文件中，作为项目所依赖的包之一。

假设你希望安装一个软件包的精确版本。在这种情况下，在包名后添加一个`@[version-number]`,如下所示:

```
npm install package-name@4.14.1 --save
```

或者，如果您安装的软件包用于开发和测试目的，请使用:

```
npm install package-name --save-dev
```

上面的命令将使 NPM 下载三个项目到你的项目的根目录:一个`node_modules`文件夹，一个`package.json`文件，和一个`package-lock.json`文件。我们将在本文的后面详细讨论这些项目。

#### 纱线安装命令

```
yarn add package-name
```

假设你希望安装一个软件包的精确版本。在这种情况下，在包名后添加一个`@[version-number]`,如下所示:

```
yarn add package-name@4.14.1
```

或者，如果您安装的软件包用于开发和测试目的，请使用:

```
yarn add package-name --dev
```

上面的命令会让 Yarn 下载三个项目到你的项目根目录:一个`node_modules`文件夹，一个`package.json`文件，和一个`yarn.lock`文件。我们将在本文的后面详细讨论这些项目。

所以，现在我们知道了如何在本地安装软件包，我们可以讨论全局软件包安装了。

### 全局软件包安装

全局安装包是指可以在系统的任何地方使用的包。

要全局安装软件包，请在您的终端上运行以下代码:

```
npm install package-name -g
```

或者，你可以像这样使用纱线:

```
yarn global add package-name
```

请注意，您可以从系统上的任何位置运行上述命令。

### 本地与全局软件包安装

一般在本地安装包比较好。以下是本地安装和全局安装之间的一些差异。

#### 区别 1:安装位置

本地安装的软件包安装在您执行`npm install package-name`(或`yarn add package-name`)命令的目录中。

具体来说，您会在项目的`node_module`目录中找到项目的本地安装包。

相比之下，全局安装包安装在系统中的一个位置。确切位置取决于您的系统配置。

#### 差异 2:包版本

假设您在本地安装了您的软件包。然后，您可以使用同一个包的不同版本进行多个应用程序开发。

但是，当您全局安装时，您必须对所有应用程序使用相同的软件包版本。

#### 区别 3:更新

本地安装允许您选择希望升级到最新版本的项目包。这使得管理破坏与其他软件包兼容性的升级变得更加容易。

但是，升级一个全局安装的包会更新所有项目的包——如果升级破坏了与其他包的兼容性，这可能会导致维护上的噩梦。

#### 区别四:使用建议

全局安装最适合只在命令行上使用的包，尤其是当它们提供可跨项目重用的可执行命令时。

然而，本地安装最适合您打算在程序中使用的包——通过`import`语句或`require()`函数。

#### 差异 5:示例

[NPM](https://www.npmjs.com/) 、 [React Native CLI](https://reactnative.dev/docs/environment-setup) 、 [Gatsby CLI](https://www.gatsbyjs.com/docs/reference/gatsby-cli/) 、 [Grunt CLI](https://gruntjs.com/getting-started) 和 [Vue CLI](https://cli.vuejs.org/) 都是众所周知的全球包的例子。

本地包常见的例子有 [Webpack](https://webpack.js.org/) 、 [Lodash](https://lodash.com/) 、 [Jest](https://jestjs.io/) 和 [MomentJS](https://momentjs.com/) 。

**注:**

*   您可以[在命令行和您的项目中本地和全局安装](https://nodejs.org/en/blog/npm/npm-1-0-global-vs-local-installation/#when-you-can-t-choose)您想要使用的包。这类软件包的典型例子是 [ExpressJS](https://expressjs.com/) 和 [CoffeeScript](https://coffeescript.org/) 。
*   您的软件包管理器不执行已安装的软件包。NPM(和 Yarn)只安装包到`node_modules`目录。如果您指定了`--save`命令，那么您的经理会将关于这个包的详细信息添加到`package.json`文件中。
*   要执行(运行)任何[可执行的](https://helpdeskgeek.com/how-to/what-is-an-executable-file-how-to-create-one/)包，您必须自己显式地这样做。我们将在本文的后面部分讨论如何实现。

但是`node_modules`文件夹、`package.json`文件、`package-lock.json`文件、`yarn.lock`文件到底是什么呢？让我们找出答案。

## 什么是`node_modules`文件夹？

**node_modules** 目录是 NPM 为您的项目本地下载的所有包所在的文件夹。

## 什么是`package.json`文件？

一个 **package.json** 文件是一个 json 文档，包管理者——像 NPM 和 Yarn——用它来存储关于一个特定项目的信息。

换句话说，`package.json`文件是项目的元数据文件。

### `package.json`文件的优点

一个`package.json`文件:

*   可以将您的项目发布到 NPM 注册中心
*   让其他人更容易管理和安装您的软件包
*   帮助 NPM 轻松管理[模块](https://www.codesweetly.com/javascript-modules-tutorial/)的依赖关系
*   使您的包可复制并与其他开发人员共享

### 如何创建一个`package.json`文件

转到项目的根目录，通过运行以下命令初始化一个`package.json`文件的创建:

```
npm init
```

或者，如果您的包管理器是 Yarn，运行:

```
yarn init
```

一旦您执行了上面的初始化命令，您的包管理器将通过询问一些关于您的项目的问题来引导您创建`package.json`文件。

如果您希望跳过调查问卷，您可以创建一个默认的`package.json`文件。让我们看看怎么做。

### 如何创建默认的`package.json`文件

假设您希望跳过由`npm init`(或`yarn init`)命令提示的问卷。在这种情况下，转到项目的[根目录](https://www.codesweetly.com/web-tech-glossary/#h-root-directory)并运行:

```
npm init -y
```

或者，如果您的包管理器是 Yarn，运行:

```
yarn init -y
```

上面的命令将使用从当前目录中提取的[默认值来创建项目的`package.json`文件。](https://docs.npmjs.com/creating-a-package-json-file#default-values-extracted-from-the-current-directory)

**注:**`-y`旗是`--yes`的简写。

一旦您的包管理器完成了它的初始化过程，您的项目的`package.json`文件将包含一个具有一组属性的对象。

**这里有一个例子:**

```
{
  "name": "codesweetly-project",
  "version": "1.0.0",
  "main": "index.js"
}
```

您可以看到上面的`package.json`文件包含了`name`、`version`和`main`字段。下面我们来了解一下这些属性。

### `package.json`的字段

`package.json`的属性使您的项目对包管理人员和最终用户有用。

假设您希望将您的包发布到 NPM 注册中心。在这种情况下，您的`package.json`文件必须有`"name"`和`"version"`字段。

但是，如果您不打算发布您的包，在这种情况下，所有字段——包括`"name"`和`"version"`属性——都是可选的。

让我们了解一下`package.json`文件中常用的字段。

#### 名字

`"name"`字段是用于记录项目名称的属性。

`"name"`属性值必须是:

*   一个单词
*   小写字母
*   少于或等于 214 个字符

请注意，您可以用连字符和下划线将单词连接在一起。

**这里有一个例子:**

```
{
  "name": "code_sweetly-project"
}
```

#### 版本

`"version"`字段表示项目的当前版本号。

`"version"`属性必须是`major.minor.patch`格式的形式。它还必须遵循[语义版本指南](https://docs.npmjs.com/about-semantic-versioning)。

**这里有一个例子:**

```
{
  "version": "1.0.0"
}
```

#### 描述

`"description"`字段是包含项目目的简要描述的属性。

NPM 建议拥有一个`"description"`属性，让你的包裹更容易在 NPM 网站上找到。

您的描述将是人们运行`npm search`命令时显示的内容之一。

**这里有一个例子:**

```
{
  "description": "A brief description about this package (project)"
}
```

#### 主要的

`"main"`字段表示项目的[入口点](https://www.codesweetly.com/web-tech-glossary/#entry-point)。

换句话说，当有人运行`require()`函数时，Node 会将调用解析到`require(<package.json:main>)`。

**这里有一个例子:**

```
{
  "main": "./src/index.js"
}
```

#### 私人的

`"private"`域让包管理者知道他们是否应该将您的项目发布到 NPM 注册中心。

**这里有一个例子:**

```
{
  "private": true
}
```

如果您将 package.json 的`"private"`属性设置为`true`，包管理器将不会发布您的项目。

因此，设置属性是防止包意外发布的一个很好的方法。

#### 剧本

`"scripts"`字段定义了您希望在项目生命周期的不同时间运行的脚本命令。

**这里有一个例子:**

```
{
  "scripts": {
    "test": "jest",
    "dev": "webpack --mode development",
    "build": "webpack --mode production",
    "predeploy": "npm run build",
    "deploy": "gh-pages -d build" 
  }
}
```

上面的`"scripts"`字段包含五个属性，它们的值是我们希望我们的包管理器在我们调用属性的键时运行的命令。

例如，运行`npm run dev`将会执行`"webpack --mode development"`命令。

#### 关键词

`"keywords"`字段指定了一组关键字，可以帮助人们发现您的包。

**这里有一个例子:**

```
{
  "keywords": [
    "drag",
    "drop",
    "drag and drop",
    "dragndrop",
    "draggable" 
  ]
}
```

`"keywords"`属性是人们运行`npm search`命令时显示的信息的一部分。

#### 作者

`"author"`字段列出了项目作者的详细信息。

**这里有一个例子:**

```
{
  "author": "Oluwatobi Sofela <oluwatobiss@codesweetly.com> (https://www.codesweetly.com)"
}
```

您也可以将上面的代码片段写成:

```
{
  "author": {
    "name": "Oluwatobi Sofela",
    "email": "oluwatobiss@codesweetly.com",
    "url": "https://www.codesweetly.com"
  }
}
```

注意，`"email"`和`"url"`属性是可选的。

#### 属国

`"dependencies"`字段列出了一个项目在生产中依赖的所有包。

**这里有一个例子:**

```
{
  "dependencies": {
    "first-package": "^1.0.4",
    "second-package": "~2.1.3"
  }
}
```

因此，每当用户从 NPM 注册表安装您的项目时，dependencies 属性确保包管理器可以自动找到并安装列出的包。

请注意，您可以通过以下任一方式将包添加到`"dependencies"`字段:

*   手动添加您的项目在生产中所依赖的每个包的名称和语义版本。
*   在您的终端上运行`npm install package-name --save-prod`命令。或者`yarn add package-name`如果 Yarn 是你的包经理。

#### 开发依赖性

`"devDependencies"`字段列出了一个项目在生产中不需要的所有包——但是本地开发和测试需要这些包。

**这里有一个例子:**

```
{
  "devDependencies": {
    "first-dev-package": "^5.8.1",
    "second-dev-package": "3.2.2—4.0.0"
  }
}
```

请注意，`"devDependencies"`字段中列出的包将在项目的开发环境中可用，但不在其生产服务器上。

假设用户通过`npm install`(或`yarn add`)命令安装项目。在这种情况下，包管理器将找到所有列出的`devDependencies`并下载到项目的`node_modules`目录中。

请记住，您可以通过以下任一方式将包添加到`"devDependencies"`字段:

*   手动添加项目开发和测试所依赖的每个包的名称和语义版本。
*   在您的终端上运行`npm install package-name --save-dev`命令。或者`yarn add package-name --dev`如果 Yarn 是你的包经理。

#### 主页

`"homepage"`字段指定了项目主页的 URL。

**这里有一个例子:**

```
{
  "homepage": "https://codesweetly.com/package-json-file-explained"
}
```

所以，现在我们知道了什么是`package.json`文件，我们可以讨论一下`package-lock.json`。

## 什么是`package-lock.json`文件？

**package-lock.json** 文件是一个[文件](https://www.codesweetly.com/document-vs-data-vs-code/#h-what-is-a-document) NPM 用它来记录你已经本地安装到你的项目的`node_modules`目录中的所有包的精确版本。

一个`package-lock.json`文件让一个应用程序 100%可复制，就像你把它发布到 NPM 注册中心一样。

所以，假设一个用户克隆了你的应用并运行了`npm install`命令。在这种情况下，`package-lock.json`确保用户下载您用来开发应用程序的包的精确版本。

例如，假设一个用户克隆了一个包含*没有*的文件的应用程序，并且这个应用程序中使用的一个依赖项有了一个更新的版本。

假设`package.json`文件中依赖项的版本号有一个脱字符号(例如，`^2.6.2`)。在这种情况下，NPM 将安装该依赖项的最新次要版本，这可能会导致应用程序产生错误的结果。

然而，假设用户克隆了包含一个`package-lock.json`文件的应用程序。在这种情况下，NPM 将安装`package-lock.json`文件中记录的依赖项的精确版本——不管是否存在更新的版本。

因此，用户将总是以您发布到 NPM 注册中心的方式获得您的应用程序。

换句话说，NPM 使用`package-lock.json`文件将您的包的依赖项锁定到您用于项目开发的特定版本号。

**注意:**每当您运行`npm update`命令时，NPM 将更新记录在`package-lock.json`文件中的包。

## 什么是`yarn.lock`文件？

`yarn.lock`文件是一个文档，Yarn 使用它来记录您已经安装到项目的`node_modules`目录中的所有包的确切版本。

`yarn.lock`文件堪比 NPM 的[package-lock . JSON](#what-is-a-package-lock-json-file)lock file。

我们前面提到过，您的包管理器不执行已安装的包——您必须自己明确地执行。我们来讨论一下怎么做。

## 如何运行可执行包

有几种方法可以运行可执行包。以下是标准技术。

### 手动定位并执行包

运行可执行包的一种方法是在命令行中键入其本地路径，如下所示:

```
./node_modules/.bin/package-name
```

### 将包添加到 package.json 的`scripts`字段

执行包的另一种方法是首先将它添加到项目的 package.json 文件的`"scripts"`字段，如下所示:

```
{
  "name": "your_package",
  "version": "1.0.0",
  "scripts": {
    "desired-name": "name-of-package-to-execute"
  }
}
```

之后，您可以像这样运行包:

```
npm run desired-name
```

注意，上面的命令是`npm run-script desired-name`的简写。

或者，您可以用纱线执行包装，如下所示:

```
yarn run desired-name
```

**这里有一个例子:**

```
{
  "name": "codesweetly-app",
  "version": "1.0.0",
  "scripts": {
    "build": "webpack",
  }
}
```

上面的代码片段将 [webpack](https://www.codesweetly.com/javascript-module-bundler/) 添加到您的`package.json`的`"scripts"`字段中。所以，我们现在可以像这样在命令行上执行`webpack`:

```
npm run build
```

或者，如果您的包管理器是 Yarn，您可以像这样运行 webpack:

```
yarn run build
```

### 使用 NPX

运行可执行包的一个更快的方法是像这样使用 NPX:

```
npx package-name
```

有了 NPX，您不再需要将您的包添加到项目的`package.json`文件的`"scripts"`字段中。

NPX(节点包执行)是一个[节点包运行器](https://nodejs.dev/learn/the-npx-nodejs-package-runner)，它自动找到并执行指定的包。

**这里有一个例子:**

```
npx webpack
```

上面的命令会自动找到并执行 [webpack](https://www.codesweetly.com/javascript-module-bundler/) 。因此，我们不需要将`"build": "webpack"`属性添加到我们的`package.json`文件的`"scripts"`字段中。

**注意:**当您安装 Node 8.2/NPM 5.2.0 或更高版本时，会自动安装 NPX。

您还可以使用您喜欢的 Node.js 版本运行一些代码。让我们找出方法。

## 如何使用您喜欢的 Node.js 版本运行代码

您可以使用`@`字符和[节点 npm 包](https://www.npmjs.com/package/node)来指定您希望用来执行代码的 Node.js 版本。

**这里有一个例子:**

```
npx node@7 index.js
```

上面的代码片段告诉 NPX 用版本 7 的最新版本运行`index.js`。

使用`node@`命令有助于避免使用 Node.js 版本管理工具，如 [nvm](https://github.com/nvm-sh/nvm) 在节点版本之间切换。

假设您希望确认 NPX 将用来运行代码的节点版本。在这种情况下，运行:

```
npx node@7 -v
```

上面的代码片段将显示最新的版本 7 的节点版本，NPX 将使用它来运行您的代码—例如，`v7.10.1`。

## 如何检查过期的本地包

要确定您的项目包是否过期，请运行:

```
npm outdated
```

如果该命令没有输出任何内容，这意味着您的项目的所有包都是最新的。

否则，请参阅这篇 [npm 过时的文章](https://docs.npmjs.com/cli/v6/commands/npm-outdated),了解该命令输出的详细解释。

或者，你可以像这样使用纱线:

```
yarn outdated
```

**注意:**要检查特定包的过期状态，请在`outdated`关键字后添加包的名称，例如`npm outdated lodash`。

## 如何检查过期的全球软件包

要确认哪个全局包已过期，请运行:

```
npm outdated -g --depth=0
```

## 如何检查本地安装的软件包

以下是检查本地安装包的三种方法:

### 本地安装的软件包及其依赖项

```
npm list
```

或者像这样使用纱线:

```
yarn list
```

### 本地安装的软件包—没有它们的依赖项

```
npm list --depth=0
```

或者，

```
yarn list --depth=0
```

### 检查本地是否安装了特定的软件包

```
npm list package-name
```

## 如何检查全局安装的软件包

以下是检查全局安装包的三种方法:

### 全局安装的软件包及其依赖项

```
npm list -g
```

或者像这样使用纱线:

```
yarn list -g
```

### 全局安装的软件包—没有它们的依赖项

```
npm list -g --depth=0
```

或者，

```
yarn list -g --depth=0
```

### 检查是否全局安装了特定的软件包

```
npm list -g package-name
```

## 如何更新软件包

以下是如何更新 NPM 和纱包:

### 如何将特定软件包更新到其最新版本

```
npm update package-name
```

或者，对于用 Yarn 管理的项目，运行:

```
yarn upgrade package-name
```

### 如何更新项目的所有本地安装包

```
npm update
```

或者，

```
yarn upgrade
```

### 如何更新特定的全局安装包

您可以像这样更新全局安装的软件包:

```
npm update package-name -g
```

### 如何更新您系统的所有全局安装包

```
npm update -g
```

## 如何卸载软件包

以下是如何卸载包与 NPM 和纱线:

### 如何从特定项目中卸载包

首先，从命令行导航到项目的[根目录](https://www.codesweetly.com/web-tech-glossary/#h-root-directory)并运行:

```
npm uninstall package-name
```

**注:**

*   添加`-S`(或`--save`)标志来删除项目`package.json`文件的`dependencies`字段中对包的引用。
*   添加`-D`(或`--save-dev`)标志来删除项目`package.json`文件的`devDependencies`字段中对包的引用。

对于用 Yarn 管理的项目，运行:

```
yarn remove package-name
```

**注意:**`yarn remove`命令会自动更新项目的`package.json`和`yarn.lock`文件。

### 如何卸载全局软件包

```
npm uninstall package-name -g
```

请注意，最好不要从`node_modules`文件夹中手动移除包，因为这样做会影响依赖于它的其他*模块*。

但是 NodeJS 中的模块到底是什么呢？下面就来了解一下。

## node.js 中的模块到底是什么

NodeJS 中的一个**模块**是计算机可以通过 Node 的`require()`函数加载的`node_modules`文件夹中的任何文件。

**这里有一个例子:**

```
const myModule = require("./codesweetly.js");
```

假设计算机成功使用`require()`函数加载了`codesweetly.js`文件。在这种情况下，这意味着`codesweetly.js`是一个模块——分配给了`myModule`变量。

请记住，一个模块也可能是一个包——但不总是这样。

如果一个模块*没有`package.json`文件用来记录关于它的信息，那么这个模块*就不是*包。*

另外，请注意，对于可由`require()`函数加载的模块，该模块必须是以下之一:

*   一个包——其`package.json`文件包含一个`"main"`字段。
*   一个 JavaScript 文件。

## 如何将项目发布到 NPM 注册中心

NPM 是[公共包作者](https://www.npmjs.com/products)的免费注册。

因此，您可以使用它从您的计算机上发布任何具有`package.json`文件的项目(文件夹)。

以下是与全世界共享您的包所需的步骤。

### 第一步:登录或注册

前往 [NPM 网站](https://www.npmjs.com/)并登录(如果您还没有帐户，请注册)。

**注意:**确保在创建新账户后验证你的邮箱。否则，在发布您的包时，您会得到一个`403 Forbidden`错误。

### 第二步:登录

从命令行登录到您的 NPM 帐户，如下所示:

```
npm login
```

**注意:**您可以使用`npm whoami`命令来检查您当前是否登录。

### 第三步:发布你的包！

转到项目的根目录，并像这样发布它:

```
npm publish
```

确保您的包裹名称目前不在 NPM 上。否则，发布时会出现错误。

你可以使用`npm search`命令(或者 [NPM 网站](https://www.npmjs.com/)的搜索栏)来搜索你想要使用的名字是否已经在 NPM 上存在。

假设您的软件包的所有合适名称都已被占用。在这种情况下，NPM 允许您将项目发布为一个范围。

换句话说，您可以将您的包发布为用户名的一个子部分。下面来看看如何。

### 如何将您的包发布为您的用户名范围

打开您的`package.json`文件，在您的包名前面加上您的用户名。

**这里有一个例子:**

```
{
  "name": "@username/package-name",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT"
}
```

NPM 的默认设置假设一个限定了作用域的名称包是一个私有项目。因此，如果您使用`npm publish`命令共享一个限定了作用域的名称包，您将会得到一个错误。

因此，要将您的包发布为您的用户名的范围，请将`--access=public`标志添加到`npm publish`命令中:

```
npm publish --access=public
```

**注意:**您可以通过使用`npm init --scope=username`命令而不是`npm init`在初始化过程中使您的项目成为一个作用域包。

## 概观

本文讨论了什么是包管理器。我们还研究了两个流行的包装经理(NPM 和纱)是如何工作的。

感谢阅读！

### 这里有一个有用的资源:

我写了一本关于 React 的书！

*   这是初学者友好✔
*   它有✔的现场代码片段
*   它包含可扩展的项目✔
*   ✔有很多容易理解的例子

[React 解释清楚](https://amzn.to/30iVPIG)的书就是你理解 ReactJS 所需要的全部。

[![React Explained Clearly Book Now Available at Amazon](img/01a810717269c181d904ccbfc9224fa0.png)](https://amzn.to/30iVPIG)