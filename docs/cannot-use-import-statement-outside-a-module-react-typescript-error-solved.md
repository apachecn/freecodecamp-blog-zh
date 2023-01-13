# 无法在模块外使用 import 语句[React TypeScript 错误已解决]

> 原文：<https://www.freecodecamp.org/news/cannot-use-import-statement-outside-a-module-react-typescript-error-solved/>

构建 web 应用程序时，您可能会遇到`SyntaxError: Cannot use import statement outside a module`错误。

在后端使用 JavaScript 或 TypeScript 时，可能会引发此错误。因此，您可能在客户端使用 React、Vue 等等，仍然会遇到这个错误。

在客户端使用 JavaScript 时，也可能会遇到此错误。

在本文中，您将了解如何在使用带有[节点](https://www.freecodecamp.org/news/node-js-server-side-javascript-what-is-node-used-for/)的 TypeScript 或 JavaScript 时修复`SyntaxError: Cannot use import statement outside a module`错误。

您还将学习如何在客户端使用 JavaScript 时修复错误。

## 如何修复 TypeScript `SyntaxError: Cannot use import statement outside a module`错误

在本节中，我们将使用 Express 使用一个基本的节点服务器。

请注意，如果您为您的节点应用程序使用最新版本的 TypeScript， **tsconfig.json** 文件具有默认规则，可以防止引发`SyntaxError: Cannot use import statement outside a module`错误。

因此，如果您:

*   安装最新版本的 TypeScript，并使用默认的 **tsconfig.json** 文件，该文件是在使用最新版本运行`tsc init`时生成的。
*   为节点正确设置 TypeScript 并安装必要的软件包。

但是让我们假设你没有使用最新的 **tsconfig.json** 文件配置。

这里有一个快速服务器，它监听端口 3000 并记录“Hello World！”到控制台:

```
import express from "express"

const app = express()

app.listen("3000", (): void => {
    console.log("Hello World!")
    // SyntaxError: Cannot use import statement outside a module
})
```

上面的代码看起来应该可以完美运行，但是引发了`SyntaxError: Cannot use import statement outside a module`。

这是因为我们使用了`import`关键字来导入一个模块:`import express from "express"`。

要解决这个问题，请转到 **tsconfig.json** 文件，并滚动到模块部分。

在 modules 部分，您应该会看到这样一个特定的规则:

```
/* Modules */
"module": "esnext" 
```

要解决此问题，请将值“esnext”更改为“commonjs”。

那就是:

```
/* Modules */
"module": "commonjs"
```

## 如何修复 JavaScript `SyntaxError: Cannot use import statement outside a module`错误

使用 vanilla JS 时修复`SyntaxError: Cannot use import statement outside a module`错误与 TypeScript 略有不同。

这是我们的服务器:

```
import express from "express";

const app = express();

app.listen(3000, () => {
    console.log("Hello World!");
    // SyntaxError: Cannot use import statement outside a module
}); 
```

出于同样的原因，我们得到了`SyntaxError: Cannot use import statement outside a module`错误——我们使用了`import`关键字来导入一个模块。

要解决这个问题，请转到 **package.json** 文件并添加`"type": "module",`。那就是:

```
{
  "name": "js",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "type": "module",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.2"
  }
} 
```

现在您可以使用`import`关键字而不会出现错误。

要在客户端(没有任何框架)使用 JavaScript 时修复这个错误，只需将属性`type="module"`添加到要作为模块导入的文件的脚本标签中。那就是:

```
<script type="module" src="./add.js"></script>
```

## 摘要

在本文中，我们讨论了 TypeScript 和 JavaScript 中的`SyntaxError: Cannot use import statement outside a module`错误。

这个错误主要发生在使用`import`关键字导入 Node.js 中的模块时，或者在`script`标签中省略了`type="module"`属性时。

我们看到了在使用 TypeScript 和 JavaScript 时出现错误的代码示例以及如何修复它们。

编码快乐！