# ESLint:关于基本前端工具的基本事实

> 原文：<https://www.freecodecamp.org/news/the-essentials-eslint/>

最近，我越来越多地参与前端开发。我做得越多，我的思想和灵魂就越迷失在这个混乱的世界里。即使是一个简单的待办事项应用程序也很容易需要一堆工具——仅举几个例子，比如 [ESLint](https://eslint.org) 、 [Babel](https://babeljs.io/) 、[web pack](https://webpack.js.org/)——以及软件包。

幸运的是，有许多初学者工具包，所以我们不必从头开始做每件事。有了它们，一切都已经设置好了，所以我们可以马上开始编写第一行代码。它节省了重复、枯燥任务的时间，这对于有经验的开发人员来说非常有用。

然而，这种好处对初学者来说是有代价的。因为一切都是开箱即用的，这看起来像是魔术，他们可能不知道在引擎盖下到底发生了什么，这在某种程度上理解是很重要的。虽然学习曲线不像其他人那样陡峭——试着和你一直在学习和使用的一些工具比较，你会明白我的意思——在这个混乱的世界里，我们需要一个旅途的生存指南。

本系列将涵盖前端开发的基本工具以及我们需要了解的基本知识。这将允许我们统治工具，而不是被它们控制。

在这篇文章中，我们将关注每一个工具的开发者体验。因此，本系列的目标是作为生存指南，并给出每个工具的高层次概述，而不是作为文档。

将包括哪些内容:

*   埃斯林
*   巴比伦式的城市
*   网络包
*   流动
*   以打字打的文件
*   玩笑。

前言说够了，让我们从第一个工具开始:ESLint。

# 什么是 ESLint，为什么要用它？

ESLint，顾名思义，是 [ECMAScript](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 的 linter。棉绒的定义是:

> 轧棉后去除棉籽上的短纤维的机器。

虽然代码和棉籽没有任何关系，但无论代码还是棉籽，棉绒都将有助于使事情变得更干净和更一致。我们不希望看到这样的代码:

```
const count = 1;
const message  =  "Hello, ESLint"
    count += 1 
```

既难看又有错误。这就是 ESLint 介入的时候了。当我们运行代码时，ESLint 不会将错误转储到浏览器控制台，而是在我们键入代码时捕获它(实际上不会:我们需要编辑器或 IDE 扩展来实现这一点，这将在后面介绍)。

当然，这个错误并不难发现，但是如果每次我们将要犯错误的时候，有一个助手提醒我们，或者自动为我们纠正错误，那不是更好吗？虽然 ESLint 不能捕捉所有类型的错误，但它至少让我们节省了一些精力，因此我们可以将时间花在其他重要的、需要人类关注的事情上。

# ESLint 是如何工作的？

现在我们知道了什么是 ESLint 以及我们为什么需要它，让我们更深入一点，看看它是如何工作的。本质上，我们可以把它分成三大步骤。

## 句法分析程序

我们写的代码只不过是一系列字符。然而，这个序列不仅仅是随机的字符:它们需要遵循一套规则或惯例，这就是形成语言的语法。

对于一个人来说，从阅读文本或代码到从概念上理解它，我们几乎不需要付出什么努力。对于计算机来说，这要困难得多。例如:

```
const tool = 'ESLint' // 1
  const  tool  =  "ESLint" // 2 
```

当我们阅读上面的两行时，我们立即知道它们是相同的，可以读作“有一个名为`tool`的常数，其值为 ESLint”。对于一台不懂意思的电脑来说，这两条线看起来差别很大。因此，如果我们向 ESLint 输入原始代码，几乎不可能做任何事情。

当事情变得复杂和难以沟通时——想想我们如何让计算机理解我们在做什么——抽象可以是一种逃避。通过抽象一个东西，我们隐藏了所有不必要的细节，减少了噪音，并使每个人都在同一页面上，这简化了交流。在上面的例子中，一个空格或两个空格并不重要，单引号或双引号也不重要。

换句话说，这就是解析器的工作。它将原始代码转换为抽象语法树(AST ),这个 AST 被用作 lint 规则所基于的媒介。为了创建 AST，解析器还需要完成许多步骤——如果您有兴趣了解更多关于 AST 如何生成的知识，本教程有一个很好的概述。

## 规则

该过程的下一步是通过一系列规则运行 AST。规则是如何从 AST 中找出代码中潜在的现存问题的逻辑。这里的问题不一定是语法或语义错误，但也可能是文体错误。规则给出的输出将包含一些有用的信息供以后使用，比如代码行、位置和关于问题的信息性消息。

除了捕捉问题，如果可能的话，规则甚至可以自动纠正代码。比如在上面的代码中应用[无多空格](https://eslint.org/docs/rules/no-multi-spaces)时，会修剪掉所有多余的空格，让代码看起来干净一致。

```
 const  tool  =  "ESLint" // 2
// becomes
const tool = "ESLint" // 2 
```

在不同的场景中，规则可以在不同的级别使用——退出、仅警告或严格错误——并且有各种选项，这使我们可以控制如何使用规则。

## 结果

这个过程结束了。有了规则的输出，问题就在于我们如何以一种人类友好的方式显示它，这要感谢我们前面提到的所有有用的信息。然后从结果中，我们可以快速指出问题，它在哪里，并进行修复，或者可能没有。

# 综合

ESLint 可以作为一个独立的工具使用，具有强大的 CLI，但这是使用 ESLint 的基本方法。我们不希望每次需要 lint 代码时都键入命令，尤其是在开发环境中。这个问题的解决方案是将 ESLint 集成到我们的开发环境中，这样我们就可以在一个地方编写代码并查看 ESLint 发现的问题。

这种集成来自特定于 ide 或编辑器的扩展。这些扩展需要 ESLint 才能工作，因为它们在后台运行 ESLint——难怪我们仍然需要安装 ESLint，没有 ESLint 它们什么都不是。这个原则适用于我们日常使用的其他 IDE 或编辑器扩展。

还记得我们上面谈到的规则的输出吗？扩展将使用它在 IDE 或编辑器中显示。输出的显示方式取决于扩展的实现方式以及 IDE 或编辑器对其扩展的开放程度。一些扩展还利用了从规则中发布修正的能力，以便在保存时更改代码，如果我们启用了它的话。

# 配置

配置是赋予工具多功能性的主要动力。ESLint 与此并无不同，只是它拥有其他工具中最全面的配置。一般来说，我们需要一个文件或一个地方来放置配置，我们有几个选项。

所有这些都可以归结为两种主要的方式:要么我们为每个工具准备一个单独的配置文件，要么我们把它们都打包在`package.json`中。`.eslintrc.js`是 ESLint 将查找其配置的文件之一，也是具有最高优先级的文件。

关于配置，我们需要知道的下一件事是它的层次结构和级联行为。由于这些特性，我们不需要在项目的每个文件夹中都有一个配置文件。

如果文件夹中不存在配置文件，ESLint 只需查找文件夹的父文件夹，直到找不到为止。然后，它将在`~/.eslintrc`中回落到用户范围的默认配置。否则，配置文件将增加或覆盖上层的配置文件。

然而，这里有一个特殊的调整。如果我们在一个配置文件中指定`root: true`，查找将在该文件处停止，而不是像以前那样向上。此外，ESLint 将使用那个配置文件作为根配置，所有子配置都将基于这个文件。

这不仅仅局限于 ESLint——这些东西对于其他工具来说是常见的。先说 ESLint 具体配置。

## 句法分析程序

前面已经讨论了 ESLint 中解析器的作用。默认情况下，ESLint 使用 [Espree](https://github.com/eslint/espree) 作为它的解析器。如果我们分别使用 babel 或 typescript，我们可以将这个解析器更改为另一个兼容的解析器，如 [babel-eslint](https://www.npmjs.com/package/babel-eslint) 或[@ Typescript-eslint/parser](https://www.npmjs.com/package/@typescript-eslint/parser)。

为了配置解析器，我们使用`parserOptions`。在 Espree 支持的选项中，以下是一些我们经常使用并需要注意的选项:

*   `ecmaVersion`

我们需要为我们想要使用的功能指定合适的 ECMA 版本。比如说如果`emcaVersion: 5`，下面的代码会给出一些错误。

```
```javascript
let a = [1, 2, 3, 4] // error due to `let` keyword
var b = [...a, 5] // error due to spread syntax
``` 
```

解析器不能解析代码，因为关键字`let`和 spread 语法都是在 ES6 中引入的。将`emcaVersion`更改为 6 或更高将简单地解决错误。

*   `sourceType`

现在，我们大多把所有东西都写成模块，然后把它们捆绑在一起。所以这个选项，很多时候，应该是`module`。

我们可以使用的另一个值——也是默认值——是`script`。区别在于是否可以使用 [JS 模块](https://v8.dev/features/modules)，即使用`import`和`export`关键字。下一次我们得到这个错误消息`Parsing error: 'import' and 'export' may appear only with 'sourceType: module'`，我们知道去哪里找。

*   `ecmaFeatures.jsx`

可能还有我们想要使用的额外的 ES 特性，例如 [JSX](https://facebook.github.io/jsx/) 语法。我们使用`ecmaFeatures.jsx: true`来启用这个特性。注意，Espree 对 JSX 的支持和 React 对 JSX 的支持是不一样的。如果我们想要反应特定的 JSX，我们应该使用 [eslint-plugin-react](https://github.com/yannickcr/eslint-plugin-react) 以获得更好的结果。

如果我们使用另一个解析器，这些选项或多或少是相同的。有些选项可能更少，有些可能更多，但它们都在`parserOptions`下定义。

## 环境

这取决于代码在哪里运行:有不同的预定义全局变量。例如，我们在浏览器中有`window`、`document`。如果 [no-undef](https://eslint.org/docs/rules/no-undef) 规则被启用，而 ESLint 一直告诉我们`window`或`document`没有被定义，这将会令人恼火。

`env`选项可以提供帮助。通过指定环境列表，ESLint 将了解这些环境中的全局变量，并让我们一言不发地使用它们。

有一个特殊的环境我们需要注意，`es6`。它会隐式地将`parserOptions.ecmaVersion`设置为 6，并启用所有 ES6 特性，除了我们仍然需要单独使用`parserOptions.sourceType: "module"`的模块。

# 插件和可共享配置

在不同的项目中反复使用相同的规则配置可能会令人厌烦。幸运的是，我们可以重用一个配置，并且只在需要的时候用`extends`覆盖规则。我们称这种类型的配置为可共享配置，ESLint 已经为我们提供了两种配置:`eslint:recommended`和`eslint:all`。

按照惯例，ESLint 的可共享配置有`eslint-config`前缀，因此我们可以通过 NPM 使用 [`eslint-config`](https://www.npmjs.com/search?q=keywords:eslint-config) 关键字轻松找到它们。在数百个搜索结果中，有一些很受欢迎的，比如 [eslint-config-airbnb](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb) 或 [eslint-config-google](https://github.com/google/eslint-config-google) ，你能想到的都有。

从可能的错误、最佳实践、ES6 到风格问题，ESLint 有一堆现成的规则来满足不同的目的。此外，为了增强其能力，ESLint 有大量的第三方规则，由近千个插件提供。与可共享配置类似，ESLint 的插件以`eslint-plugin`为前缀，在 NPM 上可以使用 [`eslint-plugin`](https://www.npmjs.com/search?q=keywords:eslint-plugin) 关键字。

一个插件定义了一组新的规则，在大多数情况下，它会公开自己的便利配置。例如， [eslint-plugin-react](https://github.com/yannickcr/eslint-plugin-react) 给了我们两个可共享的配置，`eslint-plugin-react:recommended`和`eslint-plugin-react:all`，就像`eslint:recommended`和`eslint:all`一样。要使用其中一个，我们首先需要定义插件名，然后扩展配置。

```
{
  plugins: ["react"],
  extends: "plugin:react/recommended"
}

// Note that we need to prefix the config by `plugin:react` 
```

一个常见的问题是使用什么插件或配置。虽然这很大程度上取决于我们的需求，但我们可以使用 [Awesome ESLint](https://github.com/dustinspecker/awesome-eslint) 作为参考来找到有用的插件和配置。

# 较美丽

我们就要到了，我们就要到达终点了。最后但同样重要的是，我们将讨论一双流行的 ESLint，[更漂亮](https://github.com/prettier/prettier)。简而言之，Prettier 是一个固执己见的代码格式化程序。虽然 beauty 可以单独使用，但将其集成到 ESLint 中可以大大增强体验，而[ESLint-plugin-beauty](https://github.com/prettier/eslint-plugin-prettier)可以完成这项工作。

单独使用 prettle 和将 prettle 与 ESLint 一起使用的区别可以概括为代码格式化问题。与单独给出格式问题不同，用 ESLint 运行更漂亮将像对待其他问题一样对待格式问题。然而，这些问题总是可以修复的，这相当于格式化代码。

这就是`eslint-plugin-prettier`的工作方式。一般来说，它在后台运行得更漂亮，并比较运行前后的代码。最后，它将差异报告为单个 ESLint 问题。为了解决这些问题，插件只需使用来自 Prettier 的格式化代码。

为了进行这种集成，我们需要安装`prettier`和`eslint-plugin-prettier`。`eslint-plugin-prettier`还附带了`eslint-plugin-prettier:recommended`配置——它扩展了[eslint-config-beautiful](https://github.com/prettier/eslint-config-prettier)。因此我们也需要安装`eslint-config-prettier`来使用它。

```
{
  "plugins": ["prettier"],
  "extends": "plugin:prettier/recommended"
} 
```

# 结论

一般来说，代码 linters 或 formatters 已经成为软件开发中事实上的标准，特别是在前端开发中。

它的好处远远超出了它在技术上的作用，因为它帮助开发者专注于更重要的事情。由于将代码样式委托给机器，我们可以避免在代码评审中固执己见的风格，并将时间用于更有意义的代码评审。代码质量也会受益，我们会得到更一致、更不容易出错的代码。

*本文原载于[我的博客](https://blog.vinhis.me/2019/08/03/the-essentials-of-essential-frontend-tools-eslint.html)T3。*