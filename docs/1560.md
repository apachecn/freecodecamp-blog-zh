# React 可访问性工具——如何构建更易访问的 React 应用

> 原文：<https://www.freecodecamp.org/news/react-accessibility-tools-build-accessible-react-apps/>

使网站或 web 应用程序具有可访问性可以改善残障人士和所有用户的用户体验。

由于开发人员要处理紧迫的截止日期和相互竞争的优先级，很容易意外地将未解决的可访问性问题发布到产品中。当使用 JavaScript 框架时，事情变得更加复杂，比如 React，它涉及到编写 JSX。

但幸运的是，您可以利用一些工具来发现或评估文本编辑器或浏览器中常见的可访问性问题。

本文将阐明这些现有的可访问性工具，以及如何使用它们来构建更易访问的 React 应用程序。

## 什么是网页可访问性？

如果一个网站或 web 应用程序不因为其残疾而禁止残疾人使用，则称其为可访问的。

拥有一个可访问的网站消除了障碍，并确保残疾人和非残疾人平等地访问网络内容和功能。

使您的网站对残疾人无障碍的好处将扩展到所有用户，包括非残疾人。

## 为什么您应该关注可访问性

我再怎么强调可及性的重要性也不为过。如果你没有从项目一开始就注意到这一点，那么如果你开始改造，将来你就有可能把可访问性变成一种负担，一种昂贵的负担。

从一开始，让你的网站具有可访问性就应该是你的项目不可或缺的一部分。这不应该是事后的想法。

我在下面强调了为什么你需要从一开始就关注可访问性:

### 遵循 SEO 最佳实践

一些基本的可访问性需求也是 SEO 的最佳实践，比如使用语义 HTML 元素，正确使用标题元素，以及向`img`标签添加描述性的`alt`属性。

### 提高所有用户的 UX

提高残疾人的可访问性将改善所有用户的体验。

例如，增加足够的对比度不仅有助于视力低下、色盲或认知障碍的人，也有助于在不同照明条件下工作的人。

类似地，添加一个带有适当文本的`alt`属性将有助于使用屏幕阅读器的人以及那些在图像加载失败或加载时间过长时使用慢速互联网连接的人。

### 这是应该做的事情

通过使你的网站易于访问，你正在做正确的事情。他们也有权访问你提供的服务，有些是你的客户。此外，如果因为你的网站无法访问残疾人而被指控歧视，这对你或你的企业都没有好处。这会损害你的品牌和声誉。

### 避免法律问题

最后，根据您居住和工作的地方，您可能会遇到法律上的可访问性要求。一些国家立法要求残疾人可以访问网站。

## 无障碍标准和指南

有几种不同的可访问性标准和指南。最著名和最广泛认可的标准是由[万维网联盟(W3C)](https://www.w3.org/Consortium/) 通过其[网络无障碍倡议(WAI)](https://www.w3.org/WAI/about/) 开发的。

我在下面的小节中强调了其中的一些标准和指南。

### [网页内容无障碍指南(WCAG) 2.1](https://www.w3.org/WAI/standards-guidelines/wcag/)

WCAG 是国际公认的网页内容无障碍标准之一。

它是由 W3C 通过一个参与性的过程开发的，得到了来自世界各地的许多个人和机构利益相关者的投入。

该标准解释了如何让残障人士更容易访问 web 内容。也已经 [ISO 认证](https://www.iso.org/standard/58625.html)。

根据 W3C 的说法，WCAG 主要是作为个人、组织和政府在网络内容可访问性方面的国际通行标准而创建的。

### [创作工具辅助功能指南(ATAG) 2.0](https://www.w3.org/WAI/standards-guidelines/atag/)

ATAG 是一套可访问性指南，您可以使用它来设计创作 web 内容的工具。

该指南有助于确保您制作的创作工具对残障人士来说是可访问的。反过来，这些工具应该帮助作者创建可访问的 web 内容。

### [用户代理可访问性指南(UAAG) 2.0](https://www.w3.org/WAI/standards-guidelines/uaag/)

UAAG 2.0 是 WCAG 的姐妹。这组指南详细说明了如何使浏览器、浏览器扩展、媒体播放器和其他用户代理对残障人士具有可访问性。

浏览器供应商和浏览器扩展制造商使用它来解决某些可访问性问题，如浏览器中的文本定制。

在下一节中，我们将重点介绍几个工具，它们可以帮助您标记 React 应用程序中的基本可访问性问题。

## React 应用程序的辅助工具

尽管您尽了最大努力，但还是很容易无意中把可访问性问题带到生产中。在这一节中，我们将介绍一些可以用来突出常见的可访问性问题的工具。

如果您正在处理紧迫的截止日期，可能很容易忽略某些可访问性特性。因此，在您的系统中安装一些可访问性工具会有所帮助，这些工具会通知您可能已经忽略的可访问性缺陷。

这绝不是可访问性工具的详尽列表。如果您认为还有其他有用的工具，但没有包括在这里，请通过 [Twitter](https://twitter.com/mjmawa) 联系我。我很乐意更新这篇文章。有人可能会发现它们也很有用。

尽管这些工具可以捕捉到一些常见的可访问性问题，您可以通过编程来测量，但是它们不会为您工作。从项目的构思开始，你就有责任做出深思熟虑的努力来开发更易访问和更具包容性的数字产品。

我将我们将要讨论的工具分为两类，即:

*   可以集成到 React 项目中的可访问性工具，这些工具是根据 React 开发的。
*   通用的可访问性审计工具，您可以用它来审计用 React 或不用 React 构建的站点。

在下面的小节中，我将重点介绍可以在 React 项目中使用的工具。它们是专门为与 React 或 JSX 一起使用而创建的。

### 为 React 构建的辅助工具

#### [eslint-plugin-jsx-a11y](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y)

您可以使用此工具解决 React 项目中 JSX 元素的林挺可访问性问题。您可以在文本编辑器中将其与 eslint for 林挺等工具结合使用，以实现您的项目的可访问性标准。

因为它是通过 npm 分发的，所以您可以通过在项目中运行以下命令来安装它:

```
# using npm as package manager

npm install eslint-plugin-jsx-a11y --save-dev

# using yarn as package manager

yarn add eslint-plugin-jsx-a11y --dev 
```

您使用`create-react-app`创建的任何 React 项目都已经配置了这个工具——但是它只有默认启用的可配置可访问性规则的一个子集。

您可以通过在项目中创建一个`.eslintrc`配置文件并向其中添加以下代码来启用附加规则。下面的代码激活推荐的规则:

```
 {
  "extends": ["react-app", "plugin:jsx-a11y/recommended"],
  "plugins": ["jsx-a11y"]
} 
```

如果您想标记自定义 React 项目中的可访问性问题，您需要安装`eslint`并将`"jsx-a11y"`添加到您的`.eslintrc`配置文件的插件字段中。

然后，它将标记可访问性问题，它可以通过编程识别这些问题，并根据您的配置在文本编辑器中向您发出警告。

```
 {  "plugins": [    "jsx-a11y"  ]} 
```

有关如何在自定义 React 项目中配置这个林挺工具的更多信息，请查看 GitHub 上的项目[自述文件](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y#readme)。

#### [斧可及性棉绒](https://marketplace.visualstudio.com/items?itemName=deque-systems.vscode-axe-linter)

Axe accessibility linter 是一个 Visual Studio 代码扩展，可以用于林挺 React、HTML、Vue 和 Markdown，解决一些常见的可访问性缺陷。

它检查`.js`、`.jsx`、`.ts`、`.tsx`、`.vue`、`.html`、`.htm`、`.md`和`.markdown`文件中的可访问性问题。

在安装后，你不需要配置就可以开始使用 axe accessibility linter 。你可以从 VS 代码市场安装它，它会自动启动林挺兼容的文件来检查可访问性缺陷，无需额外的配置。

关于 axe accessibility linter 使用的规则的完整列表，请查看 VS 代码市场上的扩展页面。

如果您愿意，也可以通过在项目的根目录下添加`axe-linter.yml`配置文件来打开和关闭一些规则，进而配置该工具。

您可以选择使用 WCAG 标准单独或按组禁用可访问性规则。在您的项目中使用这个特性将确保您的团队的所有成员都遵守相同的可访问性标准。

您可以将以下内容添加到您的`axe-linter.yml`文件中，以单独启用或禁用某些规则。要获得可配置规则的完整列表，请查看 VS 代码市场的 axe accessibility linter 扩展页面。

```
 # To enable/disable rules at individual level
rules:
  accessibility-rule: false # turn off rule
  another-accessibility-rule: true # turn on rule 
```

或者，您可以将以下内容添加到您的`axe-linter.yml`配置文件中，以使用特定的 WCAG 标准作为一个组来禁用规则。

要获得可配置 WCAG 标准的完整列表，请查看 VS 代码市场的 axe accessibility linter 扩展页面。

```
 # To enable/disable rules at group level based on WCAG standard

tags: 
  - wcag2a # Disable all rules for WCAG 2.0 level A
  - wcag21a # Disable all rules for WCAG 2.1 level A 
```

#### [轴心反应](https://www.npmjs.com/package/@axe-core/react)

这个可访问性测试工具是由 axe accessibility linter 背后的同一批人开发和维护的。

`axe-core-react`原名`react-axe`。您可以在开发中的 React 项目中运行它，每当您的组件更新时，可访问性缺陷就会在 Chrome DevTools 控制台中突出显示。

它确实可以帮助您在开发的早期发现一些可访问性问题。目前，`axe-core-react`与谷歌 chrome 配合使用效果最佳。与前两个不同，它测试呈现的 DOM 的可访问性，而不是您在 React 组件中编写的 JSX 元素。

```
 npm install @axe-core/react --save-dev 
```

安装完成后，您可以运行开发中的包。

下面的代码说明了如何使用最基本的配置在 React 应用程序中运行`axe-core-react`。你可以在 GitHub 上的包 [README](https://github.com/dequelabs/axe-core-npm/blob/develop/packages/react/README.md) 中读到更多的配置选项。

```
 const React = require('react');
const ReactDOM = require('react-dom');

// Make sure to run @axe-core/react in development

if (process.env.NODE_ENV !== 'production') {
  const axe = require('@axe-core/react');
  axe(React, ReactDOM, 1000);
}

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
); 
```

您可以在 React 应用程序中直接使用上述工具来捕获和修复常见的可访问性问题。

在下一节中，我们将看看其他一些辅助工具，它们与 React 没有直接关系，但是对于识别 React 应用程序中的基本辅助缺陷非常有用。

### 其他辅助工具

有很多工具可以用来检测浏览器中常见的可访问性问题。我在下面重点介绍了其中的几个工具。

#### [Axe DevTools 浏览器扩展](https://www.deque.com/axe/)

这是一个浏览器扩展，您可以使用它对您的网页进行简单的审计，以发现常见的可访问性问题。

在使用此浏览器扩展检查可访问性问题之前，您的应用需要托管在某个位置。它将易访问性缺陷分为严重、严重、中等和轻微。

#### [WAVE 评估工具浏览器扩展](https://wave.webaim.org/extension/)

这是 chrome 浏览器的另一个扩展，你可以用它来识别你的网站的可访问性问题。

就像 Axe DevTools chrome 浏览器扩展一样，这个扩展要求您在使用它来审计您的 web 应用程序的可访问性缺陷之前托管应用程序。

#### [谷歌在 Chrome DevTools 中的灯塔](https://developers.google.com/web/tools/lighthouse)

你可以使用谷歌的 Lighthouse Chrome DevTools 来审计你的网站的可访问性问题。它会生成一份报告，您可以用它来修复网站中的缺陷。

有一个无止境的通用 web 可访问性评估工具列表。你可以挑一个符合你需求的。

想要一个全面的列表，你可以查看 W3C 的[网页可访问性评估工具列表或者 a11y 项目](https://www.w3.org/WAI/ER/tools/)的[可访问性工具。](https://www.a11yproject.com/resources/#tools)

## 结论

在您的项目中使用 eslint-plugin-jsx-a11y、axe accessibility linter 和 axe-core-react 等工具，将有助于您使用 react 开发更易访问和更具包容性的产品。

尽管它们很方便，但是这里提到的工具只能标记一定比例的可访问性缺陷——主要是那些可以通过编程检测到的缺陷。

因此，在您的开发中整合自动化测试、手动测试和实际用户测试非常重要，因为自动化测试本身可能无法检测出项目中 50%的可访问性问题。