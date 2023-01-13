# 如何将 beautiful 与 ESLint 和 stylelint 集成

> 原文：<https://www.freecodecamp.org/news/integrating-prettier-with-eslint-and-stylelint-99e74fede33f/>

作者:Abhishek Jain

# 如何将 beautiful 与 ESLint 和 stylelint 集成

#### 或者如何不再担心代码样式

![2NnxX8zdoJFw9uQM9ez2epHwX3Z26IQQjmt-](img/6ad3b839db3ce9349991eb7ff006ee5c.png)

Photo by [NordWood Themes](https://unsplash.com/photos/bJjsKbToY34?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/computer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

ESLint 和 [stylelint](https://github.com/stylelint/stylelint) 是非常神奇的工具，可以让你在团队中实施编码模式。这有很多好处，比如输出更好、更一致的代码，消除提交中无用的差异(换行符、缩进等)等等。

但是随着时间的推移，这对于团队的开发人员来说可能会是一个麻烦，他们发现手动添加分号、换行符、缩进等等只是为了符合 lint 规则，这是一个额外的精神负担。这就是像[beautiful](https://github.com/prettier/prettier)这样的代码格式化工具的用武之地。

可以根据一些特定的规则自动设置你的代码格式。如果你使用的是 VSCode，你甚至可以在每次点击 save 的时候格式化你的代码(我相信在其他编辑器中一定有设置这个的方法，但是我没有研究过。)

但是，您不希望创建一个新的更漂亮的配置文件，因为您已经在 ESLint 和 stylelint 配置文件中指定了所有与格式相关的规则。所以，我们需要一些魔法。✨

现在让我们一步一步地了解如何设置这一切，以及如何根据 lint 规则格式化所有现有代码。本指南假设您的项目已经用它们的`.eslintrc`和`.stylelintrc`文件设置了 ESLint 和 stylelint。

### 第 1 部分:格式化现有代码库

#### **第一步**

安装[beauty-eslint](https://github.com/prettier/prettier-eslint)，这是一个使用 beauty 后跟`eslint --fix`来格式化你的 JavaScript 的工具。`--fix`是一个 ESLint 特性，它试图自动为您修复一些问题。

```
npm install --save-dev prettier-eslint
```

这个工具从现有的*中推断出等价的更漂亮的配置选项。eslintrc* 文件。所以你不应该需要创造一个新的*。大多数情况下是 prettierrc* 文件。

#### **第二步**

安装[beauty-eslint-CLI](https://github.com/prettier/prettier-eslint-cli)。这是一个 CLI 工具，可以帮助你一次性运行所有的文件。

```
npm install --save-dev prettier-eslint-cli
```

#### **第三步**

安装[beauty-style lint](https://github.com/hugomrdias/prettier-stylelint)，这是一个用 beauty 后跟`stylelint —-fix`格式化你的 CSS/SCSS 的工具。像 ESLint 一样，`--fix`是一个 stylelint 特性，它试图自动为您修复一些问题。

```
npm install prettier-stylelint --save-dev
```

这个工具*也试图基于 stylelint 配置创建一个更漂亮的配置。*

请注意，不像 prettle-eslint，您不必为它的 CLI 安装另一个包，因为它已经包含在其中了。

#### **第四步**

在您的`package.json`中编写脚本，目标是您希望通过 beauty-eslint 和 beauty-style lint 运行的代码库中的现有文件。

```
"scripts": {
```

```
 "fix-code": "prettier-eslint --write 'src/**/*.{js,jsx}' ",
```

```
 "fix-styles": "prettier-stylelint --write 'src/**/*.{css,scss}' "
```

```
}
```

如你所见，我分别针对我现有的所有 JS 和 JSX 以及我现有的所有 CSS 和 SCSS。

`--write`标志为当前被格式化的文件就地写入改变。所以，要小心并且**确保你所有的现有文件都在源代码控制之下，并且没有未提交的变更**。

#### **第五步**

运行脚本！

```
npm run fix-codenpm run fix-styles
```

现在，您可以将所有这些新的更改作为一个大的提交来签入(如果您不想污染您自己的 git 历史，甚至可以来自一个临时的 git 用户。)

### **第 2 部分:设置 VSCode**

既然您现有的代码库已经格式化了，那么是时候确保所有正在编写的代码都被自动格式化了。

#### **第一步**

为 VSCode 安装更漂亮的、ESLint 和 stylelint 扩展:

[**beautiful-Code formatter-Visual Studio market place**](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
[*Visual Studio Code-VS Code plugin for beautiful/beautiful*marketplace.visualstudio.com](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)[**ESLint-Visual Studio market place**](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
[*Visual Studio Code-将 ESLint JavaScript 集成到 VS 代码中。*marketplace.visualstudio.com](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)[**stylelint-Visual Studio market place**](https://marketplace.visualstudio.com/items?itemName=shinnn.stylelint)
[*Visual Studio 代码扩展-现代 CSS/marketplace.visualstudio.com/少 linter*](https://marketplace.visualstudio.com/items?itemName=shinnn.stylelint)

#### **第二步**

配置一些 VSCode 设置:

`"prettier.eslintIntegration": true` —告诉更漂亮的使用更漂亮的-eslint 而不是更漂亮的

`"prettier.stylelintIntegration": true` —告诉更漂亮的使用更漂亮样式的 lint，而不是更漂亮

我们不需要 ESLint 直接为我们修改代码，因为不管怎样，beauty-ESLint 都会为我们运行`eslint --fix`。

`"editor.formatOnSave": true` —每次保存文件时使用上述选项运行得更漂亮，因此您永远不必手动调用 VSCode 的 format 命令。

此外，您可以将上面的 workplace 设置签入到源代码控制中，以便其他团队成员可以更容易地设置他们的编辑器。您可以通过在项目的根目录下创建一个`.vscode`文件夹，并将上述所有规则放在一个`settings.json`文件中来实现。

可选地，你可以告诉更漂亮地忽略格式化某些模式的文件。为此，只需在项目的根目录下添加一个`.prettierignore`文件，指定要忽略的路径。例如:

```
strings.jsonscripts/*
```

**就这样！再也不用担心代码样式了？**

本文并不打算成为一个详尽的指南，而是介绍使用这里提到的令人惊奇的工具的可能性。我建议打开每种工具的官方 GitHub 页面，以了解如何更有效地将这些工具用于您的特定工作流程。

如有任何帮助、建议等，请在下方写下评论。

#### *参考文献*

https://github . io/docs/en/
https://stylelint . io/user-guide/
https://sl link . org/
https://github . com/prettier/prettier-vscode
https://github . com/prettier/prettier-esline
https://github . com/prettier/esline-CLI
v=YIvjKId9m2c 足球俱乐部