# 如何创建和发布 Vue 组件库

> 原文：<https://www.freecodecamp.org/news/how-to-create-and-publish-a-vue-component-library/>

组件库现在非常流行。它们使得在整个应用程序中保持一致的外观变得容易。

在我的职业生涯中，到目前为止，我已经使用了各种不同的库，但是使用一个库与确切地知道制作一个库需要什么是非常不同的。

我想更深入地了解组件库是如何构建的，我还想向您展示如何更好地理解。

为了创建这个组件库，我们将使用`vue-sfc-rollup` npm 包。当启动一个组件库时，这个包是一个非常有用的工具。

如果您有一个想要使用该实用程序的现有库，请参考他们提供的[文档](https://www.npmjs.com/package/vue-sfc-rollup)。

这个包为项目的启动创建了一组文件。正如他们的文档所解释的，它创建的文件包括以下内容(SFC 代表单个文件组件):

*   最小[汇总](https://rollupjs.org/)配置
*   对应的 package.json 文件，包含构建/开发脚本和依赖项
*   一个最小的 babel.config.js 和。用于传输的 browserslistrc 文件
*   rollup 在打包 SFC 时使用的包装程序
*   启动开发的示例 SFC
*   示例使用文件，可用于在开发期间加载/测试您的组件/库

该实用程序既支持单个文件组件，也支持组件库。请务必注意文档中的这句话:

> 在库模式中，还有一个“索引”,它将组件声明为库的一部分。

这意味着在安装过程中会产生一些额外的文件。

# 酷，让我们建图书馆吧

首先，您需要全局安装`vue-sfc-rollup`:

`npm install -g vue-sfc-rollup`

一旦全局安装完毕，从命令行进入您希望库项目所在的目录。进入该文件夹后，运行以下命令来初始化项目。

`sfc-init`

在提示中选择以下选项:

*   这是单个组件还是一个库？图书馆
*   您的库的 npm 名称是什么？(这需要在 npm 上是唯一的。我使用了*布莱恩组件库*
*   这个库是用 JavaScript 还是 TypeScript 编写的？JavaScript(如果你知道自己在做什么，请随意使用 TypeScript)
*   **输入保存库文件的位置:**这是您希望库拥有的文件夹名称。它将默认为您在上面给它的 npm 名称，所以您只需按回车键。

安装完成后，导航到您的文件夹并运行 npm 安装。

```
cd path/to/my-component-or-lib

npm install
```

完成后，您可以在您选择的编辑器中打开该文件夹。

如上所述，有一个为我们构建的样例 Vue 组件。你可以在`/src/lib-components`文件夹中找到它。要查看这个组件的样子，您可以运行`npm run serve`并导航到 [http://localhost:8080/](http://localhost:8080/)

现在让我们添加我们自己的定制组件。在`lib-components`文件夹中创建一个新的 Vue 文件。对于这个例子，我将模仿 freeCodeCamp 任务部分中使用的按钮，所以我将它命名为`FccButton.vue`

![Screen-Shot-2020-07-22-at-10.08.05-AM](img/2d9016d25f2e38c02071ab04472882fb.png)

This is the button component we'll build

您可以将此代码复制并粘贴到您的文件中:

```
<template>
  <button class="btn-cta">{{ text }}</button>
</template>

<script>
export default {
  name: "FccButton", // vue component name
  props: {
    text: {
      type: String,
      default: "Enter Button Text Here"
    }
  },
  data() {}
};
</script>

<style>
.btn-cta {
  background-color: #d0d0d5;
  border-width: 3px;
  border-color: #1b1b32;
  border-radius: 0;
  border-style: solid;
  color: #1b1b32;
  display: block;
  margin-bottom: 0;
  font-weight: normal;
  text-align: center;
  -ms-touch-action: manipulation;
  touch-action: manipulation;
  cursor: pointer;
  white-space: nowrap;
  padding: 6px 12px;
  font-size: 18px;
  line-height: 1.42857143;
}

.btn-cta:active:hover,
.btn-cta:focus,
.btn-cta:hover {
  background-color: #1b1b32;
  border-width: 3px;
  border-color: #000;
  background-image: none;
  color: #f5f6f7;
}
</style>
```

src/lib-components/FccButton.vue

你可以看到我们在顶部有一个基本的模板部分，里面有我们想要的类。它还使用用户将传入的文本。

在脚本标签中，我们有组件名和组件将接受的属性。在这种情况下，只有一个名为`text`的属性，默认为“运行测试”。

我们也有一些造型来给它我们想要的外观。

为了查看组件的外观，我们需要将它添加到位于`lib-components`文件夹中的`index.js`文件中。您的 index.js 文件应该如下所示:

```
/* eslint-disable import/prefer-default-export */
export { default as FccButton } from './FccButton';
```

src/lib-components/index.js

您还需要将组件导入到`dev`文件夹中的`serve.vue`文件中，如下所示:

```
<script>
import Vue from "vue";
import { FccButton } from "@/entry";

export default Vue.extend({
  name: "ServeDev",
  components: {
    FccButton
  }
});
</script>

<template>
  <div id="app">
    <FccButton />
  </div>
</template> 
```

/dev/serve.vue

您可能需要再次运行`npm run serve`才能看到该按钮，但是当您在浏览器中导航到 [http://localhost:8080/](http://localhost:8080/) 时，它应该是可见的。

因此，我们已经构建了我们想要的组件。对于您想要构建的任何其他组件，您将遵循相同的过程。只要确保您将它们导出到`/lib-components/index.js`文件中，以便从我们即将发布的 npm 包中获得它们。

# 发布到 NPM

现在我们已经准备好将库发布到 NPM，我们需要完成构建过程，以便将它打包并准备好。

在我们运行 build 命令之前，我建议将`package.json`文件中的版本号改为`0.0.1`,因为这是我们库的第一个发布事件。在我们发布正式的“第一个”版本之前，我们希望库中不止有一个组件。你可以在这里阅读更多关于语义版本[的内容。](https://docs.npmjs.com/about-semantic-versioning)

为此，我们运行`npm run build`。

正如文件所述:

> 运行构建脚本会在`dist`目录中生成 3 个编译后的文件，分别对应于 package.json 文件中列出的`main`、`module`和`unpkg`属性。生成这些文件后，您就可以开始工作了！

运行这个命令后，我们就可以发布到 NPM 了。在此之前，请确保您拥有一个 NPM 账户(如果需要，您可以在此处完成此操作)。

接下来，我们需要通过运行以下命令将您的帐户添加到您的终端:

`npm adduser`

### 了解 package.json

当我们发布到 npm 时，我们使用 package.json 文件来控制发布什么文件。如果您查看我们最初设置项目时创建的`package.json`文件，您会看到类似这样的内容:

```
"main": "dist/brian-component-lib.ssr.js",
"browser": "dist/brian-component-lib.esm.js",
"module": "dist/brian-component-lib.esm.js",
"unpkg": "dist/brian-component-lib.min.js",
"files": [
    "dist/*",
    "src/**/*.vue"
],
```

`files`下面的部分告诉 npm 发布我们的`dist`文件夹中的所有内容，以及我们的`src`文件夹中的任何`.vue`文件。你可以在你认为合适的时候更新它，但是在本教程中我们将保持原样。

因为我们没有对 package.json 文件做任何修改，所以我们准备发布。为此，我们只需运行以下命令:

`npm publish`

![hy](img/1dd48f2d8deb2d4a199bc7bd2ff18b28.png)

I'm so proud of you!

就是这样！恭喜你！您现在已经发布了一个 Vue 组件库。你可以去[npmjs.com](https://www.npmjs.com/)在注册表里找到。