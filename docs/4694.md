# 与 Nuxt.js、布尔玛和萨斯一起前进

> 原文：<https://www.freecodecamp.org/news/up-goind-with-nuxt-js-bulma-and-sass/>

**TL；DR:用这篇快速的文章克服 Nuxt.js、布尔玛和萨斯的诡计，帮助你在 10 分钟内开始开发你的下一个应用。**

大家好，❤️！几天前，我发现自己在努力让 **Nuxt.js** 、**布尔玛**和**萨斯**正常工作，我在谷歌上找到的信息并没有太大帮助。

我发现的大多数配置都不起作用，因为它们已经过时或者没有很好地解释如何去做。所以我在这个问题上深入研究了一下，并决定在不到 10 分钟的时间里写一篇文章来帮助你做同样的事情。

让我们找点乐子，在摸索一些实现这一目标所需的概念时，让自己的双手变脏。

## 1.脚手架 Nuxt.js

如今，为了快速上手 Nuxt.js，我们使用一个名为 **[create-nuxt-app](https://github.com/nuxt/create-nuxt-app)** 的搭建工具。请确保您的机器上安装了 **[npx](https://www.npmjs.com/package/npx)** 。

让我们打开一个终端并执行:`npx create-nuxt-app nuxt-bulma-sass`，其中`nuxt-bulma-sass`是本文中我们正在搭建的项目的名称。

**create-nuxt-app** 在创建脚手架之前会问你一些问题。出于本文的目的，我选择了以下设置:

![bulma2](img/1ca9256ed890b7f90830382c6cd02d3e.png)

create-nuxt-app init questions

因此，下一步是将目录更改为我们的项目文件夹:

`cd nuxt-bulma-sass`

并使用`yarn run dev`启动项目。(喜欢也可以用 npm)

此时，我们的项目正在运行:

![bulma3](img/1f38bccd91476adfd44c6e90c39e3425.png)

如果我们在 localhost:3000 上打开浏览器，我们将看到以下屏幕:

![Screen-Shot-2019-06-14-at-09.29.05](img/d79ea978e6f71b01a893d79ff7940c64.png)

localhost:3000 pages/index.vue

此时，我们在屏幕上看到了 pages/index.vue，这是默认情况下在项目中呈现的第一个页面。

让我们用下面的内容替换这个文件的内容:

![carbon1](img/458f7bc62eee2d2fd344aef849cff019.png)

如果我们在浏览器中检查我们的页面，我们会看到我们安装了 **bulma** ,因为部分是根据它格式化的。

![Screen-Shot-2019-06-14-at-09.45.03](img/ef076a298cab1ae7bcfaea6f78d85351.png)

简易柠檬榨汁机。

让我们添加一个类并选择一些颜色:

![carbon-2](img/568e332350cff6e8d5f24cfc6c447718.png)![Screen-Shot-2019-06-14-at-09.47.55](img/6ec9f88df029b7ededb3b402ab01cf7f.png)

如果我们想筑巢。 *hello-nuxt* 里面。*江户主题*？我们需要萨斯来做到这一点。

## 2.添加 Sass

因此，要将 Sass 添加到我们的项目中，我们需要停止正在运行的应用程序(Ctrl+c ),并执行以下操作:

`yarn add node-sass sass-loader --dev`

这是作为开发依赖项需要的两个包，以便能够在我们的样板文件中包含 Sass。

请注意，我们将它添加为开发依赖项，因为我们只在开发和构建时需要它。在那之后 **Sass** 被转化为 **CSS** ，我们不再需要它了。

先来先睹为快我的 package.json 给你检查一下:

![Screen-Shot-2019-06-14-at-09.57.38](img/8b99f79e1042cbbb817720c0cf5c0929.png)

package.json with sass added to the project

好了，❤️，现在我们可以嵌套我们想要的类了。

让我们再次运行样板文件:`yarn run dev`并做必要的调整。

![carbon--1-](img/622562e5ca654f0f4963fb849f5a4d95.png)![Screen-Shot-2019-06-14-at-10.05.23](img/ed8cfc16a1d210b28dff8087bb58370d.png)

噪音。我们今天已经做了很多了！去喝杯咖啡吧，☕，我在这里等你？

好了，让我们稍微抽象一下，创建这个文件 *~/assets/scss/main.scss* ，并在其中放置一些类和变量:

![carbon-1](img/43297a9fe225795d5cfc250e5123219c.png)

new ~/assets/scss/main.scss

![carbon--1--1](img/bc223d30a281def996526b9c36f4f7ea.png)

不错！起作用了！

现在我们有两个问题:

1.  我们需要将 main.scss 导入到我们的每个页面/组件中，这并不好。我们希望只进口一次，并在我们所有的
2.  我们不能使用 bulma sass 变量(尝试从。江户-主题类从**$江户**到**$初级**。我们希望有 bulma sass 变量，以便覆盖它们，并从那里创建新的主题。

因此...如果我们想使用[布尔玛萨斯变量](https://bulma.io/documentation/overview/colors/)呢？

![Screen-Shot-2019-06-14-at-10.13.31](img/b760b9291a2aafd6e2626122ecbaff32.png)

bulma sass variables (colors doc)

## 3.接下来是困难的部分，我花了一些时间才理解。

布尔玛被导入到 create-nuxt-app 支架中。当你这样做的时候，这是隐藏的。 **nuxt** 文件夹到你的 **nuxt-bulma-sass** 文件夹。

如果你看一下 App.js:

![nuxt2](img/56b33aa08b81c287556197e4277d1d2b.png)

当您启动 dev environment 时，您将看到从节点模块中导入了 bulma。

![nuxt-bulma](img/a8846acd1f1f003aaeb567dfe5cf1e04.png)

.nuxt/App.js

因此，如果我们想覆盖 bulma sass 变量，在启动 nuxt.js scaffold 时导入 bulma 是不合适的。

不要绝望，你没必要丢掉你的项目。演出必须继续？

## 4.正确使用布尔玛？

我们如何按照我们需要的方式将布尔玛放入样板文件中？

让我们从注释掉来自 **nuxt.config.js** 模块部分的@nuxtjs/bulma 开始(把它放在 package.json 上，因为它在那里做的是安装 bulma，这和做`yarn add bulma`是一样的，AFAIK)。

停止你的运行环境，重新做`yarn run dev`。

如果你看一看*。/nuxt/App* 你会看到它不再导入 bulma 了。

现在我们要做的是转到 main.scss 文件，并将其导入到文件的最后一行。

![carbon--4-](img/cf6972cc12fdf4453a52819504897653.png)

我还导入了 bulma/sass/utilities/_ all . sass,这样我们就有了带有颜色的 sass 变量。

![Screen-Shot-2019-06-14-at-11.12.24](img/af47bdda70d49666c17f109ccf7133bb.png)

/bulma/sass/utilities folder

当然，以后你可以通过只导入你所需要的来改进它。但那是另一篇文章的另一个故事了？

好吧，好吧，检查你的浏览器，看看它的工作。

![Screen-Shot-2019-06-14-at-11.19.16](img/da475377d564d2ed2f7272f919500cd9.png)

## 5.耶！是工作宝贝！

**现在最后一个问题！**我们不想每次需要使用它时都将其导入到我们的<风格>支架中。我们希望它在样板文件中的任何地方都是全局可用的。

解决方法是导入一个名为**[@ nuxtjs/style-resources](https://www.npmjs.com/package/@nuxtjs/style-resources)的包。**

这个包允许你在所有文件中共享变量，混合，函数。每个组件或页面的

只需停止您的开发环境，并执行以下操作:

注意:不要试图把它作为一个开发依赖项来安装，因为它不能正常工作。

另外，打开 nuxt.config.js 文件，将“@nuxtjs/style-resources”添加到模块的键/值中。

您还需要添加 *styleResources。*查完之后我的怎么样？

![carbon--5-](img/eb49d844af0ab309c1123c6a1e58482c.png)

再做一次`yarn run dev`...没有错误...但是...

不再导入 CSS 类。

**FML** ？？‍?☠️

## 6.最后一次调整

这里发生了什么事？

因此，从您导入并使用 *@nuxt/style-resources* 开始，您就不能再从 main.scss 导入实际的样式，因为它们不会存在于实际的构建中。

所以，要解决这个问题:

停止再次运行样板文件，打开 nuxt.config.js:

将 main.scss 路径添加到全局 css 数组，如下所示:

![carbon--6-](img/aa254f9e6f5271fac11be1a3f063ac4e.png)

这样，我们可以确保全局 css 样式也被导入到模板的范围内。

当然，从这一点开始，你可以为你 css 文件建立一个架构模式，创建独立的变量、函数和混合文件，并用一些额外的@imports 进行组合。

在 styleResources 对象中，您可以选择在样板文件中包含更多需要的文件。

同样，这超出了本文的范围，本文将向您展示如何消除 nuxt 及其生态系统在我们的应用流程中引入的这种微小的复杂性。

## 希望你喜欢它！❤️

## 坚强起来，坚持下去？

## 7.最后但并不是最不重要的

你可以克隆我的回购协议，玩它。

[https://github . com/evedes/nuxt-bulma-sass](https://github.com/evedes/nuxt-bulma-sass)

非常感谢 [@ruiposse](https://twitter.com/ruiposse) 审阅这篇文章，并指导我进入 vue 生态系统。❤️

## 8.文献学

01. [Nuxtjs.org](https://nuxtjs.org/)

02. [Nuxt 风格资源](https://github.com/nuxt-community/style-resources-module)

03. [Bulma.io](https://bulma.io/)

04.在谷歌的几个小时里，我感到沮丧，看到人们也对此感到沮丧？

* * *

嘿！我是 Edo，致力于 JavaScript 栈的前端工程师。如今，我主要与 React、Vue 和周围的所有生态系统一起工作。

如果你喜欢这篇文章，你可以在这里阅读更多内容。