# Redux 应用程序最重要的 ESLint 规则

> 原文：<https://www.freecodecamp.org/news/the-most-important-eslint-rule-for-redux-applications-c10f6aeff61d/>

保罗·马修·贾沃斯基

**TL；**博士运行`yarn add --dev eslint-plugin-import`并将`"import/named": 2`添加到您的`.eslintrc`规则中，以防止意外地将不存在的常数导入到您的归约器中。

如果你正在使用 [React](https://facebook.github.io/react/) 和 [Redux](http://redux.js.org/) 开发一个应用程序，你的 reducers 可能看起来像这样:

这个例子非常简单。你只需要在顶部导入一个常量。

您的文件结构目前如下所示:

```
.├── actions|   ├── constants.js|   └── index.js...omitted for brevity...├── reducers|   ├── accountReducer.js|   └── index.js...omitted for brevity...├── indes.js└── index.html
```

但是随着你的代码库的增长，你的 reducers 会变得更加复杂。按类型组织文件可能不再有意义。所以你决定开始按特征或路线来组织事情:

```
.├── actions|   ├── constants.js|   └── index.js...omitted for brevity...├── reducers|   └── index.js├── routes|   ├── accounts|   |   ├── components|   |   ├── containers|   |   ├── module|   |   |   ├── actions.js|   |   |   ├── constants.js|   |   |   └── index.js (exports our reducer)|   |   ├── styles|   |   └── index.js|   └── index.js...omitted for brevity...├── indes.js└── index.html
```

厉害！现在，我们的主组件文件夹中不再有 100 个组件。事情变得更整洁，也更容易推理。

不过，你的重构有一个问题。突然，你的减速器中的这个`import`指向了一个不存在的路径:

```
import { RECEIVE_ACCOUNT_SUCCESS } from '../actions/constants';
```

您会立即得到一个关于该路径未被解析的错误，因此您更改了它:

```
import { RECEIVE_ACCOUNT_SUCCESS } from './constants';
```

酷毙了。但是，如果您实际上没有在新文件中定义该常量，那该怎么办呢？

现在，您将体验 Redux 应用程序中最令人沮丧的错误之一——将未定义的常量导入 reducer。你的测试会崩溃，你的应用会崩溃，你会一头撞向你的办公桌，直到你弄明白为止。

问题是这种类型的错误只是无声无息地失败了。ES6 导入不关心您导入的变量是否被定义。你永远不会看到一个错误。

### ESLint 来救援了！

避免这种错误的关键是安装`eslint-plugin-import`，然后在`.eslintrc`中设置一个简单的小规则:

```
"import/named": 2
```

就是这样！现在，当你试图将一个未定义的常量导入到你的一个缩减器中时，你会得到一个错误。

编辑:除非你正在扩展一个已经包含它的基本配置，否则你还需要将`"import"`添加到你的`.eslintrc`的插件部分。感谢[戴夫·麦克](https://www.freecodecamp.org/news/the-most-important-eslint-rule-for-redux-applications-c10f6aeff61d/undefined)指出这一点！

编码快乐！