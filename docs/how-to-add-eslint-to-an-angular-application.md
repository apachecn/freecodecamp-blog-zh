# 如何将 ESLint 添加到角度应用程序中

> 原文：<https://www.freecodecamp.org/news/how-to-add-eslint-to-an-angular-application/>

在本文中，我们将使用最新版本的 [Angular](https://angular.io/) 构建一个 web 应用程序。然后我们将添加 [ESLint](https://eslint.org/) ，它静态地分析 JavaScript 代码以发现任何问题。

## 先决条件

在开始之前，您需要安装和配置下面的工具(如果您还没有安装它们)来创建 Angular 应用程序:

*   Git 是一个分布式版本控制系统。我们将使用它来同步存储库。
*   [Node.js 和 npm](https://nodejs.org/) : Node.js 是基于 Google 的 V8 引擎的 JavaScript 代码运行时软件。npm 是 Node.js 的包管理器(节点包管理器)。我们将使用它们来构建和运行 Angular 应用程序并安装库。
*   Angular CLI : Angular CLI 是 Angular 的命令行实用工具。我们将使用它来创建角度应用程序的基础结构。
*   IDE(例如 [Visual Studio Code](https://code.visualstudio.com/) 或 [WebStorm](https://www.jetbrains.com/webstorm/) ):集成开发环境是一种具有图形界面的工具，有助于应用程序的开发。我们将使用它来开发角度应用程序。

## 入门指南

### 创建角度应用程序

Angular 是一个开发平台，用于使用 HTML、CSS 和 TypeScript (JavaScript)构建 web、移动和桌面应用程序。目前，Angular 处于版本 14，Google 是该项目的主要维护者。

首先，让我们使用带有路由文件和 SCSS 风格格式的`@angular/cli`来创建具有角度基础结构的应用程序。

```
ng new angular-eslint --routing true --style scss
CREATE angular-eslint/README.md (1067 bytes)
CREATE angular-eslint/.editorconfig (274 bytes)
CREATE angular-eslint/.gitignore (548 bytes)
CREATE angular-eslint/angular.json (3136 bytes)
CREATE angular-eslint/package.json (1045 bytes)
CREATE angular-eslint/tsconfig.json (863 bytes)
CREATE angular-eslint/.browserslistrc (600 bytes)
CREATE angular-eslint/karma.conf.js (1431 bytes)
CREATE angular-eslint/tsconfig.app.json (287 bytes)
CREATE angular-eslint/tsconfig.spec.json (333 bytes)
CREATE angular-eslint/.vscode/extensions.json (130 bytes)
CREATE angular-eslint/.vscode/launch.json (474 bytes)
CREATE angular-eslint/.vscode/tasks.json (938 bytes)
CREATE angular-eslint/src/favicon.ico (948 bytes)
CREATE angular-eslint/src/index.html (299 bytes)
CREATE angular-eslint/src/main.ts (372 bytes)
CREATE angular-eslint/src/polyfills.ts (2338 bytes)
CREATE angular-eslint/src/styles.scss (80 bytes)
CREATE angular-eslint/src/test.ts (749 bytes)
CREATE angular-eslint/src/assets/.gitkeep (0 bytes)
CREATE angular-eslint/src/environments/environment.prod.ts (51 bytes)
CREATE angular-eslint/src/environments/environment.ts (658 bytes)
CREATE angular-eslint/src/app/app-routing.module.ts (245 bytes)
CREATE angular-eslint/src/app/app.module.ts (393 bytes)
CREATE angular-eslint/src/app/app.component.scss (0 bytes)
CREATE angular-eslint/src/app/app.component.html (23364 bytes)
CREATE angular-eslint/src/app/app.component.spec.ts (1097 bytes)
CREATE angular-eslint/src/app/app.component.ts (219 bytes)
✔ Packages installed successfully.
    Successfully initialized git.
```

现在我们将安装库并添加 ESLint 设置。

```
ng add @angular-eslint/schematics
ℹ Using package manager: npm
✔ Found compatible package version: @angular-eslint/schematics@14.2.5.
✔ Package information loaded.

The package @angular-eslint/schematics@14.2.5 will be installed and executed.
Would you like to proceed? Yes
✔ Packages successfully installed.

    All @angular-eslint dependencies have been successfully installed 🎉

    Please see https://github.com/angular-eslint/angular-eslint for how to add ESLint configuration to your project.

    We detected that you have a single project in your workspace and no existing linter wired up, so we are configuring ESLint for you automatically.

    Please see https://github.com/angular-eslint/angular-eslint for more information.

CREATE .eslintrc.json (984 bytes)
UPDATE package.json (1511 bytes)
UPDATE angular.json (3447 bytes)
✔ Packages installed successfully.
```

接下来，我们将运行下面的命令来验证 ESLint 的安装和配置:

```
npm run lint

> angular-eslint@1.0.0 lint /home/rodrigokamada/angular-eslint
> ng lint

Linting "angular-eslint"...

All files pass linting.
```

一切都准备好了！消息“*所有文件通过林挺*”显示没有发现问题。

应用程序存储库在这里可用[。](https://github.com/rodrigokamada/angular-eslint.)

## 结论

这就是我们在本文中讨论的内容:

*   我们创建了一个角度应用程序。
*   我们添加了 ESLint 来分析和查找代码中的问题。

在部署到您的环境之前，您可以使用它来检查您的应用程序代码。

感谢您的阅读，我希望你喜欢这篇文章！

这篇教程用葡萄牙语发布在我的博客上。

每当我发布新文章时，为了保持更新，请在 [Twitter](https://twitter.com/rodrigokamada) 和 [LinkedIn](https://www.linkedin.com/in/rodrigokamada) 上关注我。