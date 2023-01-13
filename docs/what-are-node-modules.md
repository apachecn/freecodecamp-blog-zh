# 什么是节点模块，如何使用？

> 原文：<https://www.freecodecamp.org/news/what-are-node-modules/>

每个 Node.js 应用程序都有模块。这些模块构成了应用程序构建模块的一部分。它们帮助开发人员更快地工作，并编写更结构化的代码。

在本教程中，您将学习什么是节点模块。您还将了解三种类型的节点模块。我们将讨论在您的应用中使用它们的正确方法。

## JavaScript 中的模块是什么？

简单地说，模块是一段可重用的 JavaScript 代码。它可以是一个`.js`文件或包含`.js`文件的目录。您可以导出这些文件的内容，并在其他文件中使用它们。

模块帮助开发者在编程中坚持 DRY(不要重复自己)原则。它们还有助于将复杂的逻辑分解成小的、简单的、易于管理的块。

## 节点模块的类型

作为 Node.js 开发人员，您将使用三种主要类型的节点模块。它们包括以下内容。

*   内置模块
*   本地模块
*   第三方模块

### 内置模块

Node.js 自带了一些现成的模块。安装 Node.js 时可以使用这些模块。内置节点模块的一些常见示例如下:

*   超文本传送协议（Hyper Text Transport Protocol 的缩写）
*   全球资源定位器(Uniform Resource Locator)
*   小路
*   满量程
*   操作系统（Operating System）

您可以按照下面的语法使用内置模块。

```
const someVariable = require('nameOfModule')
```

你用`require`函数加载模块。您需要将正在加载的模块的名称作为参数传递给`require`函数。

**注意:**模块名必须用引号括起来。此外，使用`const`来声明变量可以确保在调用它时不会覆盖该值。

您还需要将来自`require`函数的返回值保存在`someVariable`中。您可以随意命名该变量。但通常，你会看到程序员给变量取和模块名一样的名字(见下面的例子)。

```
const http = require('http') 

server = http.createServer((req, res) => { 
    res.writeHead(200, {'Content-Type': 'text/plain'}) 
    res.end('Hello World!')
})

server.listen(3000)
```

您使用`require`函数来加载内置的`http`模块。然后，将返回值保存在一个名为`http`的变量中。

从`http`模块返回的值是一个对象。因为已经使用`require`函数加载了它，所以现在可以在代码中使用它。例如，调用`.createServer`属性来创建一个服务器。

### 本地模块

当您使用 Node.js 时，您会创建本地模块，并在程序中加载和使用这些模块。让我们看看如何做到这一点。

创建一个简单的`sayHello`模块。它将一个`userName`作为参数，并输出“hello”和用户名。

```
function sayHello(userName) {
	console.log(`Hello ${userName}!`)
}

module.exports = sayHello
```

首先，您需要创建函数。然后使用语法`module.exports`将其导出。不过，它不一定是一个函数。您的模块可以导出对象、数组或任何数据类型。

#### 如何加载本地模块

您可以加载本地模块，并在其他文件中使用它们。为此，您可以像处理内置模块一样使用`require`函数。

但是对于自定义函数，您需要提供文件的路径作为参数。在这种情况下，路径是`'./sayHello`'(它引用了`sayHello.js`文件)。

```
const sayHello = require('./sayHello')
sayHello("Maria") // Hello Maria!
```

一旦加载了模块，就可以在代码中引用它。

### 第三方模块

在 Node.js 中使用模块的一个很酷的地方是，您可以与他人共享它们。节点包管理器(NPM)使这成为可能。当你安装 Node.js 时，NPM 会随之而来。

有了 NPM，你可以通过[NPM 注册中心共享你的模块包。](https://www.npmjs.com/)您也可以使用其他人共享的包。

#### 如何使用第三方软件包

要在您的应用程序中使用第三方包，您首先需要安装它。您可以运行下面的命令来安装软件包。

```
npm install <name-of-package>
```

例如，有一个名为`capitalize`的包。它的功能就像是将一个单词的第一个字母大写。

运行以下命令将安装资本化软件包:

```
npm install capitalize
```

要使用已安装的包，需要用`require`函数加载。

```
const capitalize = require('capitalize)
```

然后你可以在你的代码中使用它，例如:

```
const capitalize = require('capitalize')
console.log(capitalize("hello")) // Hello
```

这是一个简单的例子。但是有一些软件包可以执行更复杂的任务，并且可以节省您大量的时间。

例如，您可以使用 Express.js 包，它是一个 Node.js 框架。它使得构建应用程序更快更简单。要了解更多关于 NPM 的信息，请阅读这篇关于节点包管理器的文章。

## 结论

在本文中，您了解了什么是节点模块以及三种类型的节点模块。您还了解了如何在应用程序中使用不同的类型。

感谢阅读。还有快乐编码！