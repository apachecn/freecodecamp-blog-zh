# 如何对 Node.js 和 npm 的 DotEnv 文件使用节点环境变量

> 原文：<https://www.freecodecamp.org/news/how-to-use-node-environment-variables-with-a-dotenv-file-for-node-js-and-npm/>

环境变量是在程序外部设置的变量，通常通过云提供商或操作系统设置。

在 Node 中，环境变量是安全、方便地配置不经常改变的东西的好方法，比如 URL、认证密钥和密码。

## 如何创建环境变量

Node 支持环境变量的开箱即用，并且可以通过`env`对象(这是`process`全局对象的一个属性)来访问。)

要看到这一点，您可以在节点 REPL 中创建自己的环境变量，方法是将一个变量直接追加到`process.env`对象。

例如，要创建一个环境变量来存储我的行李上的[组合，我可以像这样分配变量:`process.env.LUGGAGE_COMBO=“12345”`。](https://www.youtube.com/watch?v=a6iW-8xPw3k)

(题外话:按照惯例，环境变量通常都是大写的。)

虽然这是一个很好的实验，但你不会在应用程序中像这样使用节点 REPL。要在节点应用程序中创建环境变量，您可能需要使用像 DotEnv 这样的包。

## 如何使用 DotEnv

[DotEnv](https://www.npmjs.com/package/dotenv) 是一个轻量级的 npm 包，它自动将环境变量从`.env`文件加载到`process.env`对象中。

要使用 DotEnv，首先使用命令:`npm i dotenv`安装它。然后在你的 app 里，这样要求和配置包:`require('dotenv').config()`。

请注意，Create React App 等一些包已经包含 DotEnv，云提供商可能有不同的方式来设置所有环境变量。因此，在遵循本文中的任何建议之前，请务必查看您正在使用的任何软件包或提供程序的文档。

## 如何创建. env 文件

一旦安装并配置了 DotEnv，在文件结构的顶层创建一个名为`.env`的文件。这是你创建所有环境变量的地方，以 thr `NAME=value`格式编写。例如，您可以将端口变量设置为 3000，如下所示:`PORT=3000`。

您可以在`.env`文件中声明多个变量。例如，您可以像这样设置与数据库相关的环境变量:

```
DB_HOST=localhost
DB_USER=admin
DB_PASSWORD=password
```

不需要用引号将字符串括起来。DotEnv 会自动为您完成这项工作。

一旦你创建了这个文件，记住你不应该把它推到 GitHub，因为它可能包含敏感数据，如认证密钥和密码。将文件添加到。gitignore 以避免意外将其推至公共回购。

## 如何访问环境变量

访问你的变量非常简单！它们被附加到`process.env`对象，所以您可以使用模式`process.env.KEY`来访问它们。

如果您需要更改任何环境变量的值，您只需要修改`.env`文件。

## 包扎

环境变量将使你的代码更易维护，更安全。它们很容易用 Dotenv 设置，在 Node 中使用起来也很简单。

现在您已经知道了如何完成，您可以为您的节点应用程序创建自己的环境变量了。尽情享受吧！