# 我是如何用 PurgeCSS 减掉 250KB 的 CSS 重量的

> 原文：<https://www.freecodecamp.org/news/how-i-dropped-250kb-of-dead-css-weight-with-purgecss-28821049fb/>

莎拉·达扬

# 我是如何用 PurgeCSS 减掉 250KB 的 CSS 重量的

![a14ppBhQh6-40FQPvkEVjoB-K1RJpZU7iddB](img/c38a2221483843a3a97d47a2b7777aad.png)

Photo by [Lena L](https://unsplash.com/photos/ASEaTdVIvBQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/balloon-helium?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我是实用优先 CSS 的大力提倡者。多年来尝试了几种方法后，我发现这是迄今为止**最好的、最易维护和扩展的 CSS 编写方式**。

当我和我的同事 [Clément Denoix](https://github.com/clemfromspace) 构建 [api-search.io](https://www.api-search.io/) 时，我决定使用 [Tailwind CSS](https://tailwindcss.com/) 来设计它。Tailwind CSS 是一个主题无关的、完全可定制的、实用至上的库。

![MilXaM3nNEeiZFyTo-R1O4tkdjjh-spHRRGS](img/74ad5891177c69c8435fc7b09804c64d.png)

库的全部意义在于让你可以随意使用大量的工具。问题是，因为你通常只使用它的一个子集，**你最终会在你的最终版本**中有很多未使用的 CSS 规则。

在我的例子中，我不仅加载了整个 Tailwind CSS 库，还向一些模块添加了几个变体。这最终使得最终缩小的 CSS 文件重量**为 259 KB** (在 GZip 之前)。当你考虑到这个网站是一个简单的单页应用程序，设计非常简单时，这是非常沉重的。

您不希望在需要时手动加载每个实用程序。这将是一项漫长而繁琐的任务。一个更好的场景是在开发过程中让一切都由你支配，并且**自动删除你在构建步骤**中没有使用的东西。

在 JavaScript 中，我们称之为[摇树](https://developer.mozilla.org/en-US/docs/Glossary/Tree_shaking)。现在，多亏了 [PurgeCSS](https://www.purgecss.com/) ，**你可以用你的 CSS 代码库**做同样的事情。

PurgeCSS 分析您的内容文件和 CSS，然后将选择器匹配在一起。如果在内容中没有找到任何选择器，它就从 CSS 文件中删除它。

在大多数情况下，**这可以开箱即用**。然而，在让 PurgeCSS 发挥其魔力之前，任何网站都有一些地方可能需要更多的思考。

### 拆分我的 CSS

该项目包含三个主要的 CSS 文件:

*   名为 [normalize.css](https://github.com/necolas/normalize.css) 的 CSS 重置，包含在 Tailwind CSS 中。
*   我的 CSS 代码库中最重要的部分。
*   一些定制的 CSS，主要是用来设计我无法添加类的 [InstantSearch](https://community.algolia.com/react-instantsearch/) 组件。

PurgeCSS 无法检测到我需要保留诸如`.ais-Highlight`、**这样的选择器，因为使用它的组件只在运行时出现在 DOM 中**。`normalize.css`也是如此:我依赖它来重置浏览器样式，但是许多相关组件永远不会匹配，因为它们是用 JavaScript 生成的。

对于以`.ais-`开头的类，我们可以用[白名单](https://frontstuff.io/how-i-dropped-250-kb-of-dead-css-weight-with-purgecss#whitelisting-runtime-classes)将它们分类。现在当涉及到重置样式时，选择符就有点难追踪了。另外，`normalize.css`的大小是相当不重要的，也不会改变。所以在这种情况下，我决定完全忽略这个文件。因此，**我不得不在运行 PurgeCSS** 之前拆分样式。

我最初的 CSS 配置是这样的:

*   一个包含三个`@tailwind`指令的`tailwind.src.css`文件:`preflight`、`components`和`utilities`。
*   一个包含我自定义样式的`App.css`文件。
*   在开始或构建项目之前，`package.json`中的 npm 脚本用于构建 Tailwind CSS。每当这个脚本运行时，它都会在`src`中输出一个`tailwind.css`文件，该文件被加载到项目中。

`@tailwind preflight`指令加载`normalize.css`。我不想让 PurgeCSS 碰它，所以我把它移到一个单独的文件中。

```
// tailwind.src.css @tailwind components;
```

```
@tailwind utilities;/* normalize.src.css */ @tailwind preflight;
```

然后，我在`package.json`中修改了我现有的`tailwind`脚本，单独构建`normalize.src.css`。

```
{  "scripts": {    "tailwind": "npm run tailwind:normalize && npm run tailwind:css",    "tailwind:normalize": "tailwind build src/normalize.src.css -c tailwind.js -o src/normalize.css",    "tailwind:css": "tailwind build src/tailwind.src.css -c tailwind.js -o src/tailwind.css"  }}
```

最后，我在项目中加载了`normalize.css`。

```
// src/index.js
```

```
...import './normalize.css'import './tailwind.css'import App from './App'...
```

现在，我可以在`tailwind.css`上运行 PurgeCSS，而不用担心它可能会删除所需的规则集。

### 配置 PurgeCSS

PurgeCSS 有多种风格:命令行界面、JavaScript API、Webpack 的包装器、Gulp、Rollup 等等。

我们使用[创建 React 应用](https://github.com/facebook/create-react-app)来引导网站，所以 Webpack 来了[预配置和隐藏](https://github.com/facebook/create-react-app#get-started-immediately) [react-scripts](https://www.npmjs.com/package/react-scripts) 后面的。这意味着我不能访问 Webpack 配置文件，除非我运行`npm run eject`来取回它们并在项目中直接管理它们。

不需要自己管理 Webpack 有很多好处，所以退出不是一个选项。相反，我决定使用 PurgeCSS 的自定义配置文件和 npm 脚本。

我首先在项目的根目录下创建了一个`purgecss.config.js`:

```
module.exports = {  content: ['src/App.js'],  css: ['src/tailwind.css']}
```

*   属性获取一个文件数组进行分析，以匹配 CSS 选择器。
*   属性接受一组要清除的样式表。

然后，我编辑我的 npm 脚本来运行 PurgeCSS:

```
{  "scripts": {    "start": "npm run css && react-scripts start",    "build": "npm run css && react-scripts build",    "css": "npm run tailwind && npm run purgecss",    "purgecss": "purgecss -c purgecss.config.js -o src"  }}
```

*   我添加了一个`purgecss`脚本，它获取我的配置文件并在`src`中输出被清除的样式表。
*   我让这个脚本在我们每次开始或构建项目时运行。

Tailwind CSS 使用特殊字符，所以如果您开箱即用 PurgeCSS，它可能会删除必要的选择器。幸运的是，PurgeCSS 允许我们使用一个[自定义提取器](https://www.purgecss.com/extractors#creating-an-extractor)，它是一个列出文件中使用的选择器的函数。对于顺风，我需要创建一个[自定义一个](https://tailwindcss.com/docs/controlling-file-size/):

```
module.exports = {  ...  extractors: [    {      extractor: class {        static extract(content) {          return content.match(/[A-z0-9-:\/]+/g) || []        },        extensions: ['js']      }    }  ]}
```

### 将运行时类列入白名单

PurgeCSS 不能检测运行时生成的类，但是它允许你定义一个白名单。无论如何，您列入白名单的类都会保留在最终文件中。

该项目使用了 [React InstantSearch](https://community.algolia.com/react-instantsearch/) ，它生成的组件的类都以`ais-`开头。很方便，PurgeCSS 支持正则表达式形式的模式。

```
module.exports = {  ...  css: ['src/tailwind.css', 'src/App.css'],  whitelistPatterns: [/ais-.*/],  ...}
```

现在，如果我忘记从`App.css`中删除一个我不再使用的类，它将从最终版本中删除，但是我的即时搜索选择器将保持安全。

### 新版本，更轻的 CSS

有了这个新的配置，**我最终的 CSS 文件从 259 KB 变成了…9 KB！**这对于整个项目来说意义重大，尤其是因为许多国家的互联网速度缓慢且不稳定，越来越多的人在移动中使用手机浏览网页。

可访问性也是为了迎合低带宽连接的人。不尝试帮助用户使用较慢的互联网是不可接受的，尤其是如果你让他们下载的是死代码。

这值得花点时间来优化你的构建。？

*最初发表于 [frontstuff.io](https://frontstuff.io/how-i-dropped-250-kb-of-dead-css-weight-with-purgecss) 。*