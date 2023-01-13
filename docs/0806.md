# module . exports–如何在 Node.js 和 JavaScript 中导出

> 原文：<https://www.freecodecamp.org/news/module-exports-how-to-export-in-node-js-and-javascript/>

在编程中，模块是具有一个或多个函数或值的程序组件。

这些值也可以在整个程序中共享，并且可以以不同的方式使用。

在本文中，我将向您展示如何通过在 Node.js 中导出和导入模块来共享函数和值。

## 为什么要导出模块？

您需要导出模块，以便可以在应用程序的其他部分使用它们。

模块可以服务于不同的目的。它们可以提供简单的工具来修改字符串。它们可以提供发出 API 请求的方法。或者它们甚至可以提供常量和原始值。

当您导出一个模块时，您可以将它导入到应用程序的其他部分并使用它。

Node.js 支持 [CommonJS 模块](https://nodejs.org/api/modules.html)和 [ECMAScript 模块](https://nodejs.org/api/esm.html)。

在本文的其余部分，我们将关注 CommonJS 模块，这是在 Node.js 中打包模块的原始方法。

如果你想了解更多关于 ES 模块(以及 CommonJS 模块)的知识，你可以[查看这篇深度指南](https://www.freecodecamp.org/news/modules-in-javascript/)。

## 如何导出节点中的模块

Node.js 已经导出了内置模块，包括 [fs](https://nodejs.dev/learn/the-nodejs-fs-module) 、 [path](https://nodejs.dev/learn/the-nodejs-path-module) 和 [http](https://nodejs.dev/learn/the-nodejs-http-module) 等等。但是您可以创建自己的模块。

Node.js 将节点项目中的每个文件视为一个可以从文件中导出值和函数的模块。

例如，假设您有一个包含以下代码的实用程序文件`utility.js`:

```
// utility.js

const replaceStr = (str, char, replacer) => {
  const regex = new RegExp(char, "g")
  const replaced = str.replace(regex, replacer)
  return replaced
} 
```

`utility.js`是一个模块，其他文件可以从中导入内容。但是`utility.js`目前不导出任何东西。

您可以通过检查每个文件中的全局`module`对象来验证这一点。当您在这个实用程序文件中打印`module`全局对象时，您有:

```
console.log(module)

// {
//   id: ".",
//   path: "...",
//   exports: {},
//   parent: null,
//   filename: "...",
//   loaded: false,
//   children: [],
//   paths: [
//     ...
//   ],
// } 
```

`module`对象有一个`exports`属性，如您所见，它是一个空对象。

所以任何从这个文件导入任何东西的尝试都会抛出一个错误。

`utility.js`文件有一个`replaceStr`方法，用一些其他字符替换字符串中的字符。我们可以从这个模块中导出这个函数供其他文件使用。

方法如下:

```
// utility.js

const replaceStr = (str, char, replacer) => {
  const regex = new RegExp(char, "g")
  const replaced = str.replace(regex, replacer)
  return replaced
}

module.exports = { replaceStr }
// or
exports.replaceStr = replaceStr 
```

现在，`replaceStr`可用于应用程序的其他部分。要使用它，您可以像这样导入它:

```
const { replaceStr } = require('./utility.js')

// then use the function anywhere 
```

## module.exports 与节点中的导出

您可以使用`module.exports`从模块中导出函数和值:

```
module.exports = { value1, function1 } 
```

或者使用`exports`:

```
exports.value1 = value1
exports.function1 = function1 
```

有什么区别？

这些方法非常相似。基本上，`exports`是作为`module.exports`的参考。为了更好地理解这一点，让我们通过使用两种导出值的方式来填充`exports`对象:

```
const value1 = 50
exports.value1 = value1

console.log(module)
// {
//   id: ".",
//   path: "...",
//   exports: { value1: 50 },
//   parent: null,
//   filename: "...",
//   loaded: false,
//   children: [],
//   paths: [
//     ...
//   ],
// }

const function1 = function() {
  console.log("I am a function")
}
module.exports = { function1, ...module.exports }

console.log(module)

// {
//   id: ".",
//   path: "...",
//   exports: { function1: [Function: function1] },
//   parent: null,
//   filename: "...",
//   loaded: false,
//   children: [],
//   paths: [
//     ...
//   ],
// } 
```

这里有两件事需要注意:

*   `exports`关键字是对`modules`对象中的`exports`对象的引用。通过执行`exports.value1 = value1`，它向`module.exports`对象添加了`value1`属性，正如您在第一个日志中看到的。
*   第二个日志不再包含`value1`导出。它只有使用`module.exports`导出的功能。为什么会这样呢？

`module.exports = ...`是将新对象重新分配给`exports`属性的一种方式。新对象只包含函数，所以`value1`不再被导出。

那么有什么区别呢？

仅用`exports`关键字导出值是从模块中导出值的一种快捷方式。您可以在顶部或底部使用这个关键字，它所做的只是填充`module.exports`对象。但是如果你在一个文件中使用`exports`,那就坚持在整个文件中使用它。

使用`module.exports`是显式指定模块导出的一种方式。并且这在一个文件中应该只存在一次。如果它存在两次，第二个声明会重新分配`module.exports`属性，模块只导出第二个声明声明的内容。

因此，作为前面代码的解决方案，您可以像这样导出:

```
// ...
exports.value1 = value1

// ...
exports.function1 = function1 
```

或者像这样:

```
// ...
module.exports = { value1, function1 } 
```

## 包裹

Node.js 项目中的每个文件都被视为一个模块，可以导出值供其他模块使用。

`module.exports`是 Node.js 文件中的一个对象，用于保存从该模块导出的值和函数。

在文件中声明一个`module.exports`对象指定了从该文件中导出的值。导出时，另一个模块可以用`require`全局方法导入这些值。