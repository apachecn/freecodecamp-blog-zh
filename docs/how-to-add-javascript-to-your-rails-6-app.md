# 如何在你的 Rails 6 应用中添加 JavaScript

> 原文：<https://www.freecodecamp.org/news/how-to-add-javascript-to-your-rails-6-app/>

作为一名初级的全栈开发人员，我的主要关注点是后端。我想学习如何编程我的后端服务器来服务我的 web 应用程序。

但是在学习了后端的基础知识之后，我发现前端和后端一样重要。增加 web 应用程序生动性的一种方法是添加 JavaScript。

JavaScript 对于增加 web 应用程序的交互性至关重要。当然，现在已经远不止这些了。它是 web 浏览器运行的一种编程语言。你访问过的很多网站都在前端使用了一些 JavaScript 代码。

您可能已经开始使用 Ruby on Rails 构建您的 web 应用程序，并且想知道如何将 JavaScript 添加到您的代码中。在本文中，我将向您展示如何向 Rails 应用程序添加 JavaScript 代码。

## 为什么是 JavaScript？

您可能想知道为什么在 web 应用程序中首先需要 JavaScript。简而言之，如果你不包含任何 JS，那么你的 web 应用的用户体验就不会好。

假设您有一个用户填写的表单，并且您想要进行表单验证。如果用户提交表单时没有填写正确的值，表单的页面必须重新加载，到达服务器并再次显示给用户。这要花很多时间，而且可能会惹恼用户。

当然，有时您可以避开 HTML 表单验证，但这并不总是可能的。添加一个简单的脚本来检查用户输入并通知他们应该输入正确的信息要好得多。

当然，这并不意味着您可以忽略服务器端验证，但这是另一篇文章的内容。

另一个用例是异步加载文件，这可以在 JavaScript 中完成，而无需重新加载整个页面。你可能使用过一些网络应用程序，当你向下滚动时，它们会加载更多的图片和文章，而不必一次又一次地刷新或改变页面。

这些和其他用例是在应用程序中使用 JavaScript 的重要原因。事实上，对 JavaScript 的需求一直在增加。你甚至可以用它来写你的后台。

但是我们热爱 Rails，所以我们会坚持一段时间。

## 我们将在这里讨论的内容

在撰写本文时，该框架的最新版本是 Rails 6，它为您与 JavaScript 的交互方式带来了一些变化。

在 Rails 6 之前，我们有资产管道来管理 CSS 和 JavaScript 文件。但是从 Rails 6 开始，Webpacker 是 JavaScript 的默认编译器。CSS 仍然由资产管道管理，但是您也可以使用 Webpack 来编译 CSS。

在 Rails 5 中，您不会在相同的位置找到 JavaScript 文件夹。JavaScript 的目录结构已经更改为 **app/javascript/packs/** 文件夹。

在那个文件夹中你会找到 **application.js** 文件，它就像 **application.css** 文件一样。当您创建新的 Rails 应用程序时，它将默认导入到 **application.html.erb** 文件中。

所有视图都将使用 **application.html.erb** 文件。

## 添加将由所有视图使用的脚本

如果您希望您的 JavaScript 在所有视图或页面中都可用，您只需将它放在 **application.js** 文件中:

```
require("@rails/ujs").start()
require("turbolinks").start()
require("@rails/activestorage").start()
require("channels")

console.log('Hello from application.js')
```

app/javascript/packs/application.js

默认情况下，前四行在那里。我添加了一个`console.log`语句，向您展示 JavaScript 将随处可用。

您可以通过查看应用程序中的任何页面并打开开发人员控制台来测试这一点。

但是您可能并不总是希望 JavaScript 代码加载到每个页面上。相反，您可以使脚本仅在访问特定页面时可用。

## 添加将由特定文件使用的脚本

如果您希望您的 JavaScript 只对特定的视图可用，那么您可以使用 **javascript_pack_tag** 来导入那个特定的文件:

```
<h1 class='text-center'>I want my JS here only</h1>

<%= javascript_pack_tag 'my_js' %> 
```

app/views/posts/index.html.erb

```
console.log('Hello from My JS')
```

app/javascript/packs/my_js.js

请记住，您需要将文件添加到**包**目录中。 **javascript_pack_tag** 也应该引用您创建的 javascript 文件的名称。

现在，只有在浏览器中加载了包含 **javascript_pack_tag** 的特定视图时，脚本才会运行。

## 包扎

我希望这篇文章有助于消除您在将应用程序更新到 Rails 6 时或者刚刚开始使用 Rails 时可能会有的任何困惑。

如果你想了解更多，可以关注我的 Github。