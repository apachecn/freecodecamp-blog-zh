# 当你已经有了巴别塔的时候，建立流动

> 原文：<https://www.freecodecamp.org/news/using-flow-with-babel-c04fdca8d14d/>

作者杰米·凯尔

![sJBI4JItWa9ejYexUmZ0R3n6qXgiZbS2qkng](img/109288eb405ff4fc3e709c9867dcaec4.png)

# 当你已经有了巴别塔的时候，建立流动

Flow 是 JavaScript 的静态类型检查器。它通过在您编写代码时提供反馈来提高您的工作效率。Flow 会实时向您发出警告，指出您何时犯了错误。如果你想知道更多，请查看[flowtype.org](https://flowtype.org/)。

Flow 没有试图建立自己完全独立的生态系统，而是与现有的 JavaScript 生态系统挂钩。使用 Babel 来编译您的代码是将 Flow 集成到项目中最简单的方法之一。

经过两年的努力，Babel 几乎可以在任何地方工作了，只要看看集成了所有你能想到的构建工具的设置页面就知道了。

如果您还没有安装 Babel，您可以使用该指南进行安装。你可能也想看看我的巴别塔手册。

![jnjy7OosWCrCZ9O8fqF3Bg8NbCPnggn804el](img/0e129982cddb86bd527ad962ac1e2865.png)

### 在巴别塔上建立流动

一旦你建立了巴别塔，开始心流就很容易了。首先让我们安装两个依赖项:

```
$ npm install --save-dev babel-plugin-transform-flow-strip-types$ npm install --global flow-bin
```

巴别塔插件是为了剥离流类型，以便您的程序运行。 **flow-bin** 是你如何从命令行使用 flow。

接下来，让我们将巴别塔插件添加到您的**中。babelrc** (或者你在哪里配置 Babel 选项):

```
{  presets: [...],  plugins: [..., "transform-flow-strip-types"]}
```

> **注意:**我假设你在本教程中使用的是 Babel 6。如果你还没有升级，建议你[升级](http://babeljs.io/blog/2015/10/31/setting-up-babel-6)。

最后，我们将在我们的目录中运行 **flow init** ，这将创建一个**。看起来应该是这样的 flowconfig** 文件:

```
[ignore]
```

```
[include]
```

```
[libs]
```

```
[options]
```

厉害！现在我们已经在您的项目中设置好了所有的流程。我们开始在一些文件上运行怎么样？

### 让流程运行起来

“流动”被设计成增量地引入到你的回购中。这意味着您不必一次将它添加到整个代码库中。相反，您可以一个文件一个文件地添加它。让我们从简单的事情开始，让你开始。

首先，尝试找到一个不太复杂或没有外部依赖性的文件。一些只有少量简单函数的东西。

```
import {getNumberFromString} from "string-math-lib";
```

```
export function multiplyStrings(a, b) {  return getNumberFromString(a) * getNumberFromString(b);}
```

为了让 Flow 在这个文件上运行，我们需要在顶部添加一个“flow pragma”注释，如下所示:

```
// @flow
```

```
import {getNumberFromString} from "string-math-lib";
```

```
export function multiplyStrings(a, b) {  return getNumberFromString(a) * getNumberFromString(b);}
```

文件顶部的这个小注释告诉 Flow“好的，我希望您对这个文件进行类型检查”。

现在我们需要第一次实际运行 Flow。为此，您需要切换回您的终端并运行以下命令:

```
$ flow
```

> **注意:**该命令是**流程状态**的别名。

这个命令所做的是启动一个流服务器，并向它询问您的 repo 的“状态”,这很可能会返回一些错误供您修复。

在新文件中最常见的错误是:

*   "缺少注释"
*   "找不到所需的模块"

这些错误与导入和导出相关。它们出现的原因是“心流”如何工作的结果。为了检查跨文件的类型，Flow 直接查看每个文件的导入和导出。

#### **“缺失注释”**

您将在 Flow 中看到这样的错误，因为它在某种程度上与文件的导出有关。除此之外，流不会抱怨缺少类型注释。

因此，为了解决这个问题，我们可以开始向您的文件中添加一些类型。有关如何操作的详细指南，请参见用户指南。你最终会得到这样的一些类型:

```
import {getNumberFromString} from "string-math-lib";
```

```
export function multiplyStrings(a: string, b: string): number {  return getNumberFromString(a) * getNumberFromString(b);}
```

继续运行 **flow** 当你添加你的类型时，看看你所做的效果。最终，您应该能够清除所有“丢失注释”的错误。

#### "找不到所需的模块"

每当您导入/请求无法使用节点的正常模块算法解决时，或者如果您用**忽略了它，您就会得到这些错误。流量配置**。

这可能是由很多事情引起的，也许你正在使用一个特殊的 webpack 解析器，也许你忘记安装什么了。不管是什么原因，Flow 需要能够找到您正在导入的模块，以正确地完成它的工作。关于如何解决这个问题，您有几个选择:

1.  **module.name_mapper —** 指定一个正则表达式来匹配模块名和替换模式。
2.  为缺少的模块创建库定义

我们将专注于为模块创建一个库定义，如果你需要使用 **module.name_mapper** 你可以在文档中看到更多关于它的信息[。](https://flowtype.org/docs/advanced-configuration.html#options)

#### 创建库定义

拥有库定义对于给你已经安装的没有类型的包赋予类型是很有用的。让我们为前面例子中的 **string-math-lib** 库设置一个。

首先在您的根目录(您放置**的目录)中创建一个**流类型的**目录。流量配置**。

> 您可以通过**的**【lib】**部分使用其他目录名。流量配置**。然而，使用**流式**将开箱即用。

现在我们将创建一个**flow-typed/string-math-lib . js**文件来保存我们的“libdef ”,并像这样开始:

```
declare module "string-math-lib" {  // ...}
```

接下来，我们只需要为该模块的导出编写定义。

```
declare module "string-math-lib" {  declare function getNumberFromString(str: string): number}
```

> **注意:**如果您需要记录“默认”或主出口，您可以通过**申报模块来完成。出口:**或**申报出口默认**

关于库的定义还有很多，所以你应该通读一下[文档](https://flowtype.org/docs/declarations.html)并阅读[这篇关于如何创建高质量库定义的博客文章](https://medium.com/@thejameskyle/flow-mapping-an-object-373d64c44592)。

#### 从流类型安装 libdef

![fnjErEoc73SGhKGGham9ABRSZpDjgRv9hk96](img/b753574dee3da33c5697a52e8c73ea48.png)

因为 libdef 会消耗很多时间，所以我们为各种包维护了一个官方的高质量 libdef 库，称为[流类型](https://github.com/flowtype/flow-typed)。

要开始使用流类型，请全局安装命令行界面(CLI ):

```
$ npm install --global flow-typed
```

现在你可以在[**flow-typed/definitions/NPM**](https://github.com/flowtype/flow-typed/tree/master/definitions/npm)**中查看你想要使用的包是否有现成的 libdef，如果有，你可以这样安装它:**

```
`$ flow-typed install chalk@1.0.0 --flowVersion 0.30`
```

**这告诉 Flow-type 当你运行 Flow **0.30** 时，你想在 **1.0.0** 版本安装 **chalk** 包。**

****流类型的** CLI 仍处于测试阶段，有许多改进计划，因此预计在不久的将来会有很大改进。**

**一定要贡献给流类型的 libdefs。这是一项社区工作，贡献的人越多，效果就越好。**

#### **您可能遇到的其他错误:**

**希望我们已经涵盖了你在开始心流时将会遇到的一切。但是，您也可能会遇到类似这样的错误:**

*   ****node_modules** 内的程序包报告错误**
*   **读取**节点模块**花费了很长时间**
*   ****node_modules** 中格式错误的 JSON 导致错误**

**对于这些类型的错误，有一些原因我不会在这篇文章中讨论(我正在为每个错误编写详细的文档)。现在，为了让自己继续前进，您可以**【忽略】**导致这些错误的文件。**

**这意味着将文件路径添加到您的**中的**【ignore】**部分。流程配置**是这样的:**

```
`[ignore]./node_modules/package-name/*./node_modules/other-package/tests/*.json`
```

```
`[include]`
```

```
`[libs]`
```

```
`[options]`
```

**通常有比这更好的选择，但这应该给你一个好的开始。**

> **有关如何更好地处理 node_modules 中的错误的一些示例，请参见这个关于 fbjs 的[堆栈溢出回答。](http://stackoverflow.com/questions/38225538/flow-type-checker-errors-in-node-modules/38264353#38264353)**

#### ****专业提示:检查你的覆盖情况****

**如果您想知道一个文件被 Flow 覆盖的有多好，您可以使用下面的命令来查看覆盖报告:**

```
`$ flow coverage path/to/file.js --color`
```

### **额外的资源和支持**

**这篇文章没有涉及到很多内容。所以这里有一些资源链接可以帮助你。**

*   **[流量网站](https://flowtype.org/)**
*   **[试流上线](https://flowtype.org/try/)**
*   **[流量 GitHub](https://github.com/facebook/flow)**
*   **[堆栈溢出#流类型](http://stackoverflow.com/questions/tagged/flowtype)**

**Flow 团队致力于确保每个人都有使用 Flow 的出色体验。如果这不是真的，我们希望听到您关于如何改进的意见。**

**在推特上关注詹姆斯·凯尔。在推特上关注[的流量。](https://twitter.com/flowtype)**