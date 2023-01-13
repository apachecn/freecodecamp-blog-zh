# 如何制作 Chrome 扩展——浏览器插件开发教程

> 原文：<https://www.freecodecamp.org/news/chrome-extension-with-parcel-tailwind/>

构建一个 Chrome 扩展可能会让人不知所措。它不同于构建 web 应用程序，因为您不想在浏览器上增加太多的 JavaScript 开销，因为您的扩展将与您正在访问的网站一起运行。您通常也不会从今天的捆绑器和框架中获得捆绑和调试的好处。

当我决定构建一个 Chrome 扩展时，我发现关于构建一个的博客帖子和文章数量相当少。当您想使用 TailwindCSS 这样的新技术时，信息就更少了。

在本教程中，我们将了解如何使用 Parcel.js 构建一个 Chrome 扩展来进行绑定和查看，并使用 TailwindCSS 进行样式设置。我们还将讨论如何将你的样式从网站中分离出来，以避免 CSS 冲突——稍后会有更多的讨论。

有几种类型的 Chrome 扩展值得一提:

*   **内容脚本**:第一种 Chrome 扩展最常见。它在网页的上下文中运行，并可用于修改其内容。这就是我们将要创建的扩展类型。
*   **Popup** :基于 Popup 的扩展使用地址栏右边的扩展图标打开一个 Popup，可以包含你喜欢的任何 HTML 内容。
*   **选项 UI** :你猜对了！这是一个 UI，用于将选项定制为一个扩展。右键点击扩展图标并选择“选项”,或者从 Chrome 扩展列表`chrome://extensions`中导航到扩展的页面，就可以访问这个页面
*   DevTools 扩展:“一个 DevTools 扩展为 Chrome DevTools 增加了功能。它可以添加新的 UI 面板和侧边栏，与被检查的页面进行交互，获取有关网络请求的信息，等等。- [谷歌 Chrome 文档](https://developer.chrome.com/extensions/devtools)

在本教程中，我们将通过在网页上显示内容并与 DOM 交互来构建一个仅使用内容脚本的 Chrome 扩展。

## 如何使用 Parcel.js V2 捆绑你的 Chrome 扩展

Parcel.js 是一个零配置的 web 应用捆绑器。它可以使用任何类型的文件作为入口点。它使用简单，适用于任何类型的应用，包括 Chrome 扩展。

首先创建一个名为`demo-extension`的新文件夹(确保你已经安装了`yarn`或`npm`，我将在这篇文章中使用`yarn`):

`$ mkdir demo-extension && cd demo-extension && yarn init -y`

接下来，我们将把包作为本地依赖项安装:

`$ yarn add -D parcel@next`

现在创建一个名为`src`的新目录:

`$ mkdir src`

### 添加清单文件

每个浏览器扩展都需要一个清单文件。这是我们定义扩展的版本和元数据的地方，也是我们使用的脚本(内容、背景、弹出窗口等)的地方。等等)和许可(如果有的话)。

你可以在 Chrome 的文档中找到完整的模式:[https://developer.chrome.com/extensions/manifest](https://developer.chrome.com/extensions/manifest)

让我们继续在`src`中添加一个名为`manifest.json`的新文件，其内容如下:

```
{
  "name": "Demo extension",
  "description": "An extension built with Parcel and TailwindCSS.",
  "version": "1.0",
  "manifest_version": 2,
} 
```

现在，在我们深入了解 chrome 扩展如何工作以及你可以用它们做些什么之前，我们将设置 [TailwindCSS](https://tailwindcss.com/)

### 如何在 Chrome 扩展中使用 TailwindCSS

TailwindCSS 是一个 CSS 框架，它使用较低级的实用程序类来创建可重用但也可定制的可视 UI 组件。

顺风有两种安装方式，但最常用的方式是通过其 NPM 软件包:

`$ yarn add tailwindcss`

另外，继续添加`autoprefixer`和`postcss-import`:

`$ yarn add -D autoprefixer postcss-import`

您需要这些来为您的样式添加供应商前缀，并能够编写类似于`@import "tailwindcss/base"`的语法来直接从`node_modules`导入顺风文件。

现在我们已经安装好了，让我们在根目录下创建一个新文件，并将其命名为`postcss.config.js`。这是 PostCSS 的配置文件，目前它将包含以下几行:

```
module.exports = {
  plugins: [
    require("postcss-import"),
    require("tailwindcss"),
    require("autoprefixer"),
  ],
}; 
```

这里插件的顺序很重要！

就是这样！这就是开始在 Chrome 扩展中使用 TailwindCSS 所需的全部内容。

在`src`文件夹中创建一个名为`style.css`的文件，并包含顺风文件:

```
@import "tailwindcss/base";
@import "tailwindcss/utilities"; 
```

### 使用 PurgeCSS 移除未使用的 CSS

让我们通过启用 Tailwind 的清除功能来确保只导入我们使用的样式。

通过运行以下命令创建新的顺风配置文件:

`$ npx tailwindcss init`:这将创建一个新的`tailwind.config.js`文件。

要删除未使用的 CSS，我们将把源文件添加到 purge 字段，如下所示:

```
module.exports = {
  purge: [
    './src/**/*.js', 👈
  ],
  theme: {},
  variants: {},
  plugins: [],
} 
```

现在，我们的 CSS 将被清除，当您为生产而构建时，未使用的样式将被移除。

### 如何在你的 Chrome 扩展中启用热重装

在向我们的扩展添加一些内容之前，还有一件事:让我们启用热重载。

当你进行新的修改时，Chrome 不会重新加载源文件——你需要点击扩展页面上的“重新加载”按钮。幸运的是，有一个 NPM 软件包为我们做热重装。

`$ yarn add crx-hotreload`

为了使用它，我们将在我们的`src`文件夹中创建一个名为`background.js`的新文件，并导入`crx-hotreload`

```
import "crx-hotreload"; 
```

最后指向`manifest.json`中的`background.js`，这样它就可以被我们的扩展服务了(默认情况下，热重装在生产中是禁用的):

```
{
  "name": "Demo extension",
  "description": "An extension built with Parcel and TailwindCSS.",
  "version": "1.0",
  "manifest_version": 2,
  "background": { 👈
    "scripts": ["background.js"]
  },
} 
```

配置够了。让我们在扩展中构建一个小的脚本表单。

#### Chrome 扩展中的脚本类型

正如本文开头所提到的，在 Chrome extensions 中有几种你可以使用的脚本。

*内容脚本*是在被访问网页的上下文中运行的脚本。您可以运行任何常规 web 页面中可用的 JavaScript 代码(包括访问/操作 DOM)。

另一方面，后台脚本是您可以对浏览器事件做出反应的地方，它可以访问 Chrome 扩展 API。

### 添加内容脚本

在`src`文件夹下创建一个名为`content-script.js`的新文件。

让我们将这个 HTML 表单添加到新创建的文件中:

```
import cssText from "bundle-text:../dist/style.css";

const html =
`
<style>${cssText}</style>

<section id="popup" class="font-sans text-black z-50 w-full fixed top-0 right-0 shadow-xl new-event-form bg-white max-w-sm border-2 border-black p-5 rounded-lg border-b-6">
  <header class="flex mb-5 pl-1 items-center justify-between">
    <span class="text-2xl text-black font-extrabold">New event!</span>
  </header>
  <main class="event-name-input mb-6">
    <div class="mb-6">
      <label
        for="event-name"
        class="font-bold pl-1 block mb-1 text-black text-xl"
        >
      Event name
      </label>
      <div class="duration-400 flex bg-white border-black border-2 rounded-lg py-4 px-4 text-black text-xl focus-within:shadow-outline">
        <input
          id="event-name"
          name="event-name"
          type="text"
          placeholder="web.dev LIVE"
          class="font-medium w-full focus:outline-none"
          />
      </div>
    </div>
    </div>
    <div class="mb-6">
      <label
        for="event-date"
        class="font-bold pl-1 block mb-1 text-black text-xl"
        >
      Date
      </label>
      <div class="event-date-input duration-400 border-black flex bg-white border-2 rounded-lg py-4 px-4  text-xl focus-within:shadow-outline">
        <input
          id="event-date"
          name="event-date"
          type="date"
          class="font-medium w-full focus:outline-none"
          />
      </div>
    </div>
    <div class=" mb-8">
    <label
      for="event-time-input"
      class="font-bold pl-1 block mb-1  text-xl"
      >
    Time
    </label>
    <div class="inline-flex items-center">
      <input
        id="event-time-input"
        type="time"
        value="17:30"
        class="border-black mr-4 lowercase duration-400 w-auto bg-white text-xl border-2  rounded-lg px-4 py-4 focus:outline-none focus:shadow-outline"
        />
      <div class="inline-flex flex-col">
        <span class="text-xl font-bold">Casablanca</span>
        <span class="text-base font-normal">Africa</span>
      </div>
    </div>
  </main>
  <footer>
  <button 
    class="duration-400 bg-green-400 text-xl py-4 w-full rounded-lg border-2 border-b-6 leading-7 font-extrabold border-black focus:outline-none focus:shadow-outline"
    >
  Save
  </button>
  </footer
</section>
`

const shadowHost = document.createElement("div");
document.body.insertAdjacentElement("beforebegin", shadowHost);
const shadowRoot = shadowHost.attachShadow({ mode: 'open' });

shadowRoot.innerHTML = html 
```

设计一个浏览器扩展的样式并不像你想象的那么简单，因为你需要确保网站的样式不会受到你自己的样式的影响。

为了将它们分开，我们将使用一个叫做*阴影 DOM* 的东西。影子 DOM 是一种强大的样式封装技术。这意味着样式仅限于影子树。因此，不会向被访问的网页泄露任何内容。此外，外部样式不会覆盖影子 DOM 的内容，尽管 CSS 变量仍然可以访问。

一个*影子主机*是任何我们想要附加影子树的 DOM 元素。一个*阴影根*是从`attachShadow`返回的，它的内容是被渲染的。

当心，样式化影像树内容的唯一方法是通过内联样式。宗地 V2 有一个新的内置功能，您可以导入一个包的内容，并将其用作 JavaScript 文件中的编译文本。然后包裹会在包装时替换它。

我们对我们的`style.css`包就是这么做的。现在我们可以在构建时将 CSS 自动内联到影子 DOM。

现在我们必须告诉浏览器这个新文件，让我们继续，通过在清单中添加以下几行来包含它:

```
{
  "name": "Demo extension",
  "description": "An extension built with Parcel and TailwindCSS.",
  "version": "1.0",
  "manifest_version": 2,
  "background": {
    "scripts": ["background.js"]
  },
  👇
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content-script.js"],
    }
  ]
} 
```

为了服务于我们的扩展，我们将向我们的`package.json`添加一些脚本:

```
 "scripts": {
    "prebuild": "rm -rf dist .cache .parcel-cache",
    "build:tailwind": "tailwindcss build src/style.css -c ./tailwind.config.js -o dist/style.css",
    "watch": "NODE_ENV=development yarn build:tailwind && cp src/manifest.json dist/ && parcel watch --no-hmr src/{background.js,content-script.js}",
    "build": "NODE_ENV=production yarn build:tailwind && cp src/manifest.json dist/ && parcel build src/{background.js,content-script.js}",
  } 
```

最后，您可以运行`yarn watch`，转到`chrome://extensions`，并确保页面右上角的*开发者模式*已启用。点击“Load Unpacked”并选择`demo-extension`下的`dist`文件夹。

*   *如果您得到这个错误:`Error: Bundles must have unique filePaths`您可以简单地通过删除您的`package.json`* 中的`main`字段来修复它

## 如何将您的扩展发布到 Google Chrome 网络商店

在进一步深入之前，让我们添加一个新的 NPM 脚本，它将帮助我们按照 Chrome 的要求压缩扩展文件。

```
 "scripts": {
    "prebuild": "rm -rf dist .cache",
    "build:tailwind": "tailwindcss build src/style.css -c ./tailwind.config.js -o dist/style.css",
    "watch": "NODE_ENV=development yarn build:tailwind && cp src/manifest.json dist/ && parcel watch --no-hmr src/{background.js,content-script.js}",
    "build": "NODE_ENV=production yarn build:tailwind && cp src/manifest.json dist/ && parcel build src/{background.js,content-script.js}",
    "zip": "zip -r chrome-extension.zip ./dist" 👈
  } 
```

如果您尚未安装`zip`，请安装:

*   苹果电脑:`brew install zip`
*   Linux: `sudo apt install zip`
*   对于 Windows，您可以使用 powershell 命令`Compress-Archive`类似地:`powershell Compress-Archive -Path .\\dist\\ -Destination .\\chrome-extension.zip`

现在你所要做的就是前往 [Chrome 网络商店开发者仪表板](https://chrome.google.com/webstore/devconsole/register)设置你的账户并发布你的扩展？

*   *你可以在我的 GitHub 账号[这里](https://github.com/mrassili/extension-demo)* 找到这个教程的完整演示

## 结论

最终，Chrome 扩展与 web 应用并没有太大区别。今天，您学习了如何使用 web 开发中的最新技术和实践来开发扩展。

希望这篇教程能帮助你加快扩展开发的速度！

如果你觉得这很有帮助，请发微博告诉我，并通过 [@marouanerassili](https://twitter.com/marouanerassili) 关注我。

谢谢大家！