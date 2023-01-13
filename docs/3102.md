# 如何创建自己的 ESLint 配置包

> 原文：<https://www.freecodecamp.org/news/creating-your-own-eslint-config-package/>

ESLint 是一个强大的工具，可以帮助您执行一致的编码约定，并确保 JavaScript 代码库的质量。

编码约定有时很难决定，拥有一个自动化其实施的工具有助于避免不必要的讨论。确保质量也是一个受欢迎的特性:linters 是捕捉 bug 的优秀工具，比如那些与可变范围相关的 bug。

ESLint 的设计是完全可配置的，您可以选择启用/禁用每个规则，或者混合使用这些规则来满足您的需求。

考虑到这一点，JavaScript 社区和使用 JavaScript 的公司可以扩展原始的 ESLint 配置。npm 注册表里有[几个例子](https://www.npmjs.com/search?q=eslint-config):[eslint-config-Airbnb](https://www.npmjs.com/package/eslint-config-airbnb)是最知名的一个。

在您的日常工作中，您可能会组合多个配置，因为没有一个配置是万能的。这篇文章将展示如何创建你自己的配置库，给你集中所有规则定义的选项。

## 创建项目

首先，您需要创建一个新的文件夹和 npm 项目。[按照惯例](https://eslint.org/docs/developer-guide/shareable-configs)，模块名以`eslint-config-`开头，如`eslint-config-test`。

```
mkdir eslint-config-test
cd eslint-config-test
npm init 
```

您将拥有一个类似于以下代码片段的 package.json 文件:

```
{
  "name": "eslint-config-test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
} 
```

接下来，是时候添加您的 ESLint 依赖项了:

```
npm install -D eslint eslint-config-airbnb eslint-config-prettier eslint-plugin-import eslint-plugin-jsx eslint-plugin-prettier eslint-plugin-react eslint-plugin-react-hooks prettier 
```

包装会根据你的需要而改变。在这种情况下，我使用 React 代码库，并使用[漂亮的](https://prettier.io/)来格式化我的代码。[文档](https://eslint.org/docs/developer-guide/shareable-configs#publishing-a-shareable-config)提到如果你的可共享配置依赖于一个插件，你也应该把它指定为`peerDependency`。

接下来，我将使用我的配置创建一个`.eslintrc.js`——这类似于您已经在应用程序中做的事情:

```
module.exports = {
  extends: [
    'airbnb',
    'eslint:recommended',
    'plugin:import/errors',
    'plugin:react/recommended',
    'plugin:jsx-a11y/recommended',
    'plugin:prettier/recommended',
    'prettier/react',
  ],
  plugins: [
    'react-hooks',
  ],
  rules: {
  },
}; 
```

`rules`对象存储您想要覆盖的任何规则。在上面的片段中，`rules`是空的，但是可以随意检查[我的覆盖](https://github.com/leonardofaria/eslint-config-leozera/blob/master/.eslintrc.js#L14:L58)。在 Airbnb/JavaScript 库中，你可以[找到一个被社区覆盖的规则列表](https://github.com/airbnb/javascript/issues/1089)。

### 创建自定义规则

是时候用你的自定义规则创建一个`.prettierrc`了——这是一个棘手的部分，因为 Prettier 和 ESLint 可能会在一些规则上发生冲突:

```
{
  "tabWidth": 2
} 
```

值得一提的是,`.prettierrc`文件应该位于使用您的包的项目的根目录下。现在，我正在手动复制它。

下一步是在`index.js`文件中导出您的配置:

```
const eslintrc = require('./.eslintrc.js');

module.exports = eslintrc; 
```

在`index.js`文件中创建所有配置在技术上是可能的。但是如果你这样做，你不会得到配置对象(在这里插入你的 [Inception](https://www.imdb.com/title/tt1375666/) 笑话)。

### 你完了！

【to】瞧！这就是启动自己的配置包所需的全部内容。通过在 JavaScript 项目中运行以下内容，可以在本地测试配置包:

```
npm install /Users/leonardo/path/to/eslint-config-test 
```

请记住，您的配置包的依赖项也可能被安装。

如果一切正常，您可以发布到 npm 注册表:

```
npm publish 
```

## 完整示例

我有一个功能性的 GitHub 项目显示了这个帖子的设置: [eslint-config-leozera](https://github.com/leonardofaria/eslint-config-leozera) 。你也可以在下面看到:

[https://codesandbox.io/embed/github/leonardofaria/eslint-config-leozera/tree/master/?fontsize=14&theme=dark](https://codesandbox.io/embed/github/leonardofaria/eslint-config-leozera/tree/master/?fontsize=14&theme=dark)

## 关于项目的更多信息

*   [配置诚信通](https://eslint.org/docs/user-guide/configuring):正式诚信通单据。你知道，*阅读文件*
*   [如何发布你的第一个 NPM 套餐](https://medium.com/@bretcameron/how-to-publish-your-first-npm-package-b224296fc57b):引用帖子副标题——“创建 NPM 套餐你需要知道的一切。”
*   [eslint-config-wesbos](https://github.com/wesbos/eslint-config-wesbos) :由 [Wes Bos](https://www.wesbos.com/) 开发的一个项目，它在我做这项工作时帮助了我

也发布在[我的博客](https://bit.ly/2AKW42t)上。如果你喜欢这些内容，请在 [Twitter](https://twitter.com/leozera) 和 [GitHub](https://github.com/leonardofaria) 上关注我。苏珊·霍尔特·辛普森/Unsplash 的封面照片。