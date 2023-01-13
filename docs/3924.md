# 语义用户界面指南

> 原文：<https://www.freecodecamp.org/news/semantic-ui-guide/>

## **什么是语义 UI？**

语义 UI 是一个前端开发框架，类似于为主题化设计的 bootstrap。它包含预先构建的语义组件，有助于使用人性化的 HTML 创建美观且响应迅速的布局。

根据语义 UI 网站，该框架利用简洁的 HTML、直观的 JavaScript 和简化的调试来使前端开发成为有趣和愉快的体验。

它还集成了 React、Angular、Meteor、Ember 和许多其他框架来帮助组织 UI 层和应用程序逻辑。

### 语义 UI 版本历史

2013 年 9 月，由杰克·卢基奇创作的第一个预发布版本出现在 github 上。

语义 UI `1.x`于 2014 年 11 月首次发布，对之前的预发布版本进行了突破性的更改。

语义 UI `2.x`于 2015 年 6 月首次发布，并引入了新的 UI、几个错误修复、增强和默认主题改进。

### 语义 UI 浏览器支持

当前版本`2.2.x`支持以下浏览器

*   最近两个版本 FF，Chrome，Safari Mac
*   IE 11+
*   安卓 4.4+，安卓 44+的 Chrome
*   iOS Safari 7+
*   微软 Edge 12+

## 如何安装语义 UI

安装语义用户界面有几种方法，下面是一些最简单的方法:

### 通过内容交付网络(CDN)

这对于初学者来说是最容易的。如下创建一个 HTML 文件

```
<!DOCTYPE html>
<html>
  <head>
    <title>Semantic UI</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.2.13/semantic.min.css">
    <!-- Add custom stylesheet here -->
  </head>
  <body>

    <!-- Write your html code here -->

    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/semantic-ui/2.2.13/semantic.min.js"></script>
  </body>
</html>
```

上面第 5 行的 CDN 链接将包括语义 UI 中所有可用的组件。如果你想安装一个特定的组件，[点击这里](https://cdnjs.com/libraries/semantic-ui)查看其各自的 CDN 链接。

### 使用构建工具

这将假设你使用安装了`node`和`npm`的 Ubuntu Linux 操作系统，对于其他操作系统[点击这里](https://semantic-ui.com/introduction/getting-started.html)

在您的项目目录中，使用 npm 全局安装 gulp

```
npm install -g gulp
```

安装语义用户界面

```
npm install semantic-ui --save
cd semantic/
gulp build
```

包括在 HTML 中

```
<link rel="stylesheet" type="text/css" href="semantic/dist/semantic.min.css">
<script src="https://code.jquery.com/jquery-3.1.1.min.js" integrity="sha256-hVVnYaiADRTO2PzUGmuLJr8BLUSjGIZsDYGmIJLv2b8=" crossorigin="anonymous"></script>
<script src="semantic/dist/semantic.min.js"></script>
```

通过 npm 更新

```
npm update
```

### 与其他框架集成

您可以将语义 UI 与其他前端开发框架集成，如 React、Angular、Ember 或 Meteor。[点击此处](https://semantic-ui.com/introduction/integrations.html)了解更多信息和集成说明。

### 有关语义用户界面的更多信息:

语义 UI 有完整的、组织良好的文档，可以让你很快上手并运行。下面的链接会对你的语义 UI 之旅有所帮助。

*   [语义 UI 网站](https://semantic-ui.com/)
*   [语义用户界面入门](https://semantic-ui.com/introduction/getting-started.html)
*   [语义 UI 模板示例](https://semantic-ui.com/usage/layout.html#pages)
*   [定制和创建语义 UI 主题](http://learnsemantic.com/)