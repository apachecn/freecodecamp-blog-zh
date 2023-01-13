# Vue 测试工具和笑话:如何为 Vue 组件编写简单的单元测试

> 原文：<https://www.freecodecamp.org/news/simple-unit-tests-with-vue-test-utils-and-jest-c384d7abc321/>

作者:艾德·耶伯格

# Vue 测试工具和笑话:如何为 Vue 组件编写简单的单元测试

![1*KlrR7EWfaDgtcW5hJGFjHQ](img/9dda4272cbf061867f4ed2888aba3cb6.png)

在本教程中，我将向您展示如何测试 Vue 组件。

我们将使用 Jest 和 Vue 测试工具编写单元测试和快照测试。全部不带 Webpack。

本教程面向熟悉单元测试的用户。如果你是单元测试新手，可以看看我的文章[单元测试 Vue 组件新手篇](https://eddyerburgh.me/unit-test-vue-components-beginners)。

### 设置

我已经将[做成了一个简单的启动项目](https://github.com/eddyerburgh/vue-unit-test-starter/)。Git 将其克隆到一个目录中:

```
git clone https://github.com/eddyerburgh/vue-unit-test-starter.git
```

cd 并安装依赖项:

```
cd vue-unit-test-starter && npm install
```

安装完依赖项后，运行开发服务器:

```
npm run dev
```

现在我们可以回到代码了。

要说的一件事是化名。别名是使用速记符号导入文件的一种方式。而不是像下面这样冗长的导入语句:

```
import someModule from '../../../../../src/components/someModule'
```

您可以使用简写符号或别名。一个常见的别名是`@`符号，它解析为`src`目录:

```
import someModule from '@/components/someModule'
```

注意:您可以设置任何想要的别名，但是 vue-cli 项目使用`@`来引用`src`目录。

在 vue-cli 项目中， [Webpack 用于添加功能](https://github.com/eddyerburgh/jest-vue-starter/blob/master/build/webpack.base.conf.js#L25)。这很好，但是我们没有使用 Webpack 来运行我们的测试。我们需要另一种方法来解决别名。

这就是巴别尔的用武之地。有一个插件——babel-plugin-module-resolver——可以解析 babel 中的别名。你可以在`.babelrc`里看到。它只在测试环境中使用，因为当您运行开发或生产构建时，Webpack 会进行别名解析。

签出此文件:

好了，现在你已经有了这个项目的概况，是时候添加 Jest 了。

### 玩笑

Jest 是一个测试框架。这是最快的 Vue 单文件组件测试框架之一。

除了运行测试，Jest 还附带了一系列其他功能，比如模拟、代码覆盖和快照测试。

首先要做的是安装 Jest:

```
npm install --save-dev jest
```

为了测试 sfc，您需要在运行测试之前将它们编译成 JavaScript。如果您尝试运行一个未编译的 SFC，您将会得到一个语法错误。

Jest 不会开箱编译`.vue`文件。你需要告诉它编译它们。您可以通过向`package.json`添加一个`jest`字段来实现这一点。

将下面的代码添加到您的`package.json`中。

```
"jest": {    "moduleFileExtensions": [      "js",      "json",      "vue"    ],    "transform": {      "^.+\\.js$": "<rootDir>/node_modules/babel-jest",      ".*\\.(vue)$": "<rootDir>/node_modules/vue-jest"    }  }
```

您将看到一个`moduleFileExtensions`字段。这告诉 Jest 运行扩展名为`.vue`的文件，以及`.js`和。`json`。

还有一个`transform`字段。这告诉 Jest 如何在运行文件之前编译它们。匹配所有`.js`文件，用 babel-jest 编译。全部。vue 文件用 vue-jest 编译。

这些是为 Jest 构建的自定义转换。babel-jest 编译 JavaScript。vue-jest 获取`.vue`文件并将它们编译成 JavaScript。

您需要安装这两个软件包:

```
npm install --save-dev babel-jest vue-jest
```

好，酷，现在你应该添加一个冒烟测试，以确保一切正常。

在`src/components`中创建一个`__tests__`目录。添加一个`MessageToggle.spec.js`文件。所以完整的文件路径将是`src/components/__tests__/MessageToggle.spec.js`。

将下面的代码复制到文件中:

Jest 自动运行`__tests__`目录下的所有`.js`文件。它甚至添加了一个测试环境变量，所以您的测试脚本所做的就是运行 Jest。

在您的`package.json`的`scripts`字段中添加`unit`脚本:

```
"unit": "jest"
```

现在运行脚本:

```
npm run unit
```

太好了，第一次通过测试？。

现在您将使用 Vue 测试工具编写更复杂的测试。

### 有用的测试视图

Vue Test Utils 目前处于测试阶段，但你现在可以毫无问题地使用它。API 已经基本完成了。

安装它:

```
npm install --save-dev @vue/test-utils
```

现在您将使用 Vue 测试工具来替换`MessageToggle.spec.js`中的测试。

将下面的代码复制到`src/components/__tests__/MessageToggle.spec.js`

这里，我们可以使用`[mount](https://github.com/vuejs/vue-test-utils/blob/dev/docs/en/api/mount.md)`函数返回一个包装器对象。包装器包含一些帮助方法，如`text`，帮助断言组件。你可以在[的文档](https://github.com/vuejs/vue-test-utils/tree/dev/docs/en/api/wrapper)中看到完整的列表。

好了，让我们添加一个更复杂的测试，在`Messagetoggle`组件上执行一个动作。将下面的代码复制到`MessageToggle.spec.js`中:

这一次，我们点击`MessageToggle`中的一个按钮(`#toggle-message`)并检查`<`；p >标签文本已正确更改。

现在运行测试脚本:

```
npm run unit
```

呜，通过测试！？

Vue 测试工具抽象出了 Vue 内部构件。所以你需要做的就是学习 Vue Test Utils API。

现在，您将为列表组件编写一个测试。列表组件需要道具，幸运的是 Vue Test Utils 给了我们一个在挂载组件时传递道具的方法。

创建一个文件`/src/components/__tests__/List.spec.js`，并粘贴下面的代码

这次你会注意到我们使用了`shallow`函数。这与`mount`相同，除了它只渲染一层深度的组件。一般最好用浅的。

现在你已经写了一些单元测试，是时候看看快照测试了。

### 快照测试

Jest 有一个很棒的特性，叫做快照测试。

快照测试基本上将组件树的副本作为一个字符串，然后在每次运行测试时与它进行比较。如果呈现的组件 HTML 发生变化，测试就会失败。

让我们给`Messag.spec.js`添加一个快照测试。

您需要使用 vue-server-renderer 将组件呈现为一个字符串。返回的字符串不是很漂亮，所以您应该添加 jest-serializer-vue 来美化您的快照。

```
npm install --save-dev vue-server-renderer jest-serializer-vue
```

您还需要告诉 Jest 使用序列化程序。在您的`package.json`中的`jest`字段内添加一个`snapshotSerializers`字段:

```
"snapshotSerializers": [    "<rootDir>/node_modules/jest-serializer-vue"]
```

现在更新 List.spec.js 以包含快照测试:

这个测试浅层挂载组件，并使用 vue-server-renderer 将其呈现为一个 HTML 字符串。

现在运行您的测试:

```
npm run unit
```

您将看到一些关于保存快照的新输出。去`src/components/__tests__/__snapshots__/List.spec.js.snap`里看看:

```
// Jest Snapshot v1, https://goo.gl/fbAQLP
```

```
exports[`List.vue has same HTML structure 1`] = `<ul>    <li>        list item one    </li>    <li>        list item two    </li></ul>`;
```

酷，快照。？

现在，如果`List.vue`的标记发生变化，Jest 将警告您在运行测试时快照发生了变化。

### 结论

现在您已经用 Jest 和 Vue 测试工具设置了单元测试和快照测试。

我跳过了几个概念。如果你的项目不能正常工作，你可以看看 GitHub 上的[成品库。](https://github.com/eddyerburgh/jest-vue-example)

Jest 的[加载了更多的特性](https://facebook.github.io/jest/)以使测试更容易。

Vue Test Utils 还有更多方法— [查看文档](https://github.com/vuejs/vue-test-utils/tree/dev/docs/en)。

单元测试 Vue 组件从来没有这么简单过，所以出去写一些测试吧！

如果你从这篇文章中学到了什么，分享并给出一个？把消息传出去！