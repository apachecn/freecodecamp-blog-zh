# 如何在 React 和 Vite 应用程序中导入 SVG

> 原文：<https://www.freecodecamp.org/news/how-to-import-svgs-in-react-and-vite/>

您在将 SVG 导入 React 应用程序时遇到困难了吗？这是许多开发人员面临的问题，尤其是在使用模块捆绑器设置新的 React 应用程序时。

在本文中，我将与您分享在 React 中导入 SVG 的不同方式，以及该过程如何在幕后工作。

让我们开始吧。

## 什么是 SVG？

SVG 是可缩放矢量图形的缩写，是一种用于在互联网上渲染二维(2D)图形的图像格式。

SVG 格式将图像存储为**向量**，这些向量是由基于几何和数学公式的点、线和曲线组成的图形。

因为它们基于数字和值，而不是像光栅图像那样的像素网格。png 和. jpg)，它们在缩放或调整大小时不会损失质量。

它们也非常适合创建响应迅速的网站，这些网站需要在各种不同的屏幕尺寸下看起来美观且功能良好。

总的来说，SVG 很棒，因为它们是可伸缩的、轻量级的、可定制的，并且在使用[内联](#2usingsvgsbyaddingdirectlyasjsx)时可以使用 CSS 来制作动画。

## 如何在 React 应用中导入 SVG

让我们来看看将 SVG 导入 React 应用程序时最常用的一些方法。

### 1.如何使用图像标签导入 SVG

使用 image 标签导入 SVG 是使用 SVG 最简单的方法之一。如果您使用 CRA(创建 React 应用程序)初始化您的应用程序，您可以在图像源属性中导入 SVG 文件，因为它立即支持它。

```
import YourSvg from "/path/to/image.svg";

const App = () => {
  return (
    <div className="App">
      <img src={YourSvg} alt="Your SVG" />
    </div>
  );
};
export default App; 
```

但是如果你没有使用 CRA，你必须在你使用的 bundler 中设置一个文件加载器系统(Webpack，package，Rollup，等等)。

例如，Webpack 有一个用于处理 SVG 的加载器，叫做[文件加载器](https://v4.webpack.js.org/loaders/file-loader/)。

要安装文件加载器，请添加以下命令:

```
npm install file-loader --save-dev 
```

接下来，将加载程序添加到`webpack.config.js`文件中:

```
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpe?g|gif)$/i,
        use: [
          {
            loader: "file-loader",
          },
        ],
      },
    ],
  },
}; 
```

现在，您可以导入 SVG 文件并使用它们:

```
import YourSvg from "/path/to/image.svg";

const App = () => {
  return (
    <div className="App">
      <img src={YourSvg} alt="Your SVG" />
    </div>
  );
};
export default App; 
```

注意:虽然这种方法很简单，但是它有一个缺点:与其他导入方法不同，您不能对在`img`元素中导入的 SVG 进行样式化。因此，它将适用于不需要定制的 SVG，如徽标。

### 2.如何通过直接添加为 JSX 来导入 SVG

JSX 支持`svg`标签，所以我们可以将 SVG 直接复制粘贴到 React 组件中。这种方法很简单，因为它可以帮助您充分利用 SVG，而无需使用捆绑器。

这种方法是可行的，因为 SVG 是 XML 格式的，就像 HTML 一样。所以，我们可以把它转换成 JSX 语法。也可以使用[编译器](https://transform.tools/html-to-jsx)来代替手动转换。

```
const App = () => {
  return (
    <div className="App">
      <svg

        className="ionicon"
        viewBox="0 0 512 512"
      >
        <path
          d="M160 136c0-30.62 4.51-61.61 16-88C99.57 81.27 48 159.32 48 248c0 119.29 96.71 216 216 216 88.68 0 166.73-51.57 200-128-26.39 11.49-57.38 16-88 16-119.29 0-216-96.71-216-216z"
          fill="none"
          stroke="currentColor"
          strokeLinecap="round"
          strokeLinejoin="round"
          strokeWidth={32}
        />
      </svg>
    </div>
  );
};
export default App; 
```

内联包含 SVG 的好处是我们可以访问它们不同的属性，这使得我们可以根据自己的需要来设计和定制它们。

需要记住的一点是，如果您的 SVG 文件很大，您的代码可能会变得复杂，从而降低可读性和生产率。如果是这种情况，请使用 png 或 jpeg 文件..

### 3.如何将 SVG 作为 React 组件导入

如果您使用 CRA，那么您有可能在某个时间点将 SVG 直接作为 React 组件导入和使用。

这种方法在文件加载器的帮助下是可行的，它的工作方式是将图片与 HTML 一起加载，而不是作为一个单独的文件。

```
import { ReactComponent as Logo } from "./logo.svg";

const App = () => {
  return (
    <div className="App">
      <Logo />
    </div>
  );
};

export default App; 
```

### 4.如何将 SVG 转换为 React 组件

这种方法类似于前面提到的方法。在这里，我们可以通过从类或函数组件内部返回 SVG 来将其转换为 React 组件。

为此，在文本编辑器中打开 SVG 文件，并将代码复制粘贴到一个新组件中:

```
export const ArrowUndo = () => {
  return (
    <svg

      className="ionicon"
      viewBox="0 0 512 512"
    >
      <path d="M245.09 327.74v-37.32c57.07 0 84.51 13.47 108.58 38.68 5.4 5.65 15 1.32 14.29-6.43-5.45-61.45-34.14-117.09-122.87-117.09v-37.32a8.32 8.32 0 00-14.05-6L146.58 242a8.2 8.2 0 000 11.94L231 333.71a8.32 8.32 0 0014.09-5.97z" />
      <path
        d="M256 64C150 64 64 150 64 256s86 192 192 192 192-86 192-192S362 64 256 64z"
        fill="none"
        stroke="currentColor"
        strokeMiterlimit={10}
        strokeWidth={32}
      />
    </svg>
  );
}; 
```

现在，您可以在另一个组件中导入和呈现 SVG 组件，如下所示:

```
import { ArrowUndo } from "./path/to/ArrowUndo.jsx";

export const Button = () => {
  return (
    <button>
      <ArrowUndo />
    </button>
  );
}; 
```

同样，这种方法只有在你的 React 应用有一个像 SVGR 的 [Webpack loader](https://www.npmjs.com/package/@svgr/webpack) 这样的加载器的情况下才有可能。

### 5.如何使用 SVGR 导入 SVG

SVGR 是一个获取原始 SVG 文件并将其转换成 React 组件的工具。它还拥有一个大型生态系统，支持 Create React App、Gatsby、Parcel、Rollup 和其他技术。

那么，我们如何设置它呢？

首先，通过运行下面的代码安装软件包:

```
# with npm
npm install --save-dev @svgr/webpack

# with yarn
yarn add --dev @svgr/webpack 
```

接下来，更新您的`webpack.config.js`:

```
module.exports = {
  module: {
    rules: [
      {
        test: /\.svg$/i,
        issuer: /\.[jt]sx?$/,
        use: ["@svgr/webpack"],
      },
    ],
  },
}; 
```

现在，您可以将 SVG 文件作为 React 组件导入:

```
import Logo from "./logo.svg";

const App = () => {
  return (
    <div className="App">
      <Logo />
    </div>
  );
};

export default App; 
```

### 6.如何使用 SVGR 的 Vite 插件导入 SVG

[`vite-plugin-svgr`](https://www.npmjs.com/package/vite-plugin-svgr) 是 Vite 的一个插件，它使用 svgr 将 SVG 转换成 React 组件。

您可以通过运行以下命令来安装它:

```
# with npm
npm i vite-plugin-svgr

# with yarn
yarn add vite-plugin-svgr 
```

接下来，将插件添加到您的应用程序的`vite.config.js`中:

```
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import svgr from "vite-plugin-svgr";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [svgr(), react()],
}); 
```

现在，您可以将 SVG 文件作为 [React 组件](#3importingsvgsasreactcomponents)导入:

```
import { ReactComponent as Logo } from "./logo.svg"; 
```

## 结论

就这样结束了！在本文中，我们介绍了如何使用特定包中的定制配置在 React 应用程序中导入 SVG，如何导入 React 组件，以及如何在 Vite 设置中使用它们。

当使用 Vite 时，我使用 vite svgr 插件，它可以完美地工作。您还可以尝试本文中讨论的其他方法。

我希望你觉得这篇文章很有见地。如果你有任何问题，请随时在 [Twitter](https://twitter.com/israelmitolu) 或 [LinkedIn](https://www.linkedin.com/in/israeloyetunji/) 上发消息。

感谢您的阅读，祝您编码愉快！

出发前，请查看以下资源:

*   [为什么你应该为 Vite 抛弃 Create-React-App](https://israelmitolu.hashnode.dev/why-you-should-ditch-create-react-app-for-vite)
*   [在 React 中使用 SVGs】](https://rossbulat.medium.com/working-with-svgs-in-react-d09d1602a219)
*   [开发人员的 Twitter 社区](https://twitter.com/i/communities/1532313139810906114)