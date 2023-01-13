# 角度命令行界面解释

> 原文：<https://www.freecodecamp.org/news/angular-command-line-interface-explained/>

Angular 与其命令行界面(CLI)密切相关。CLI 简化了 Angular 文件系统的生成。它在幕后处理大部分配置，因此开发人员可以开始编码。CLI 还具有较低的学习曲线，推荐给任何想要直接入门的新手。见鬼，即使有经验的 Angular 开发人员也依赖 CLI！

## 装置

Angular CLI 需要 [Node.js 和节点数据包管理器(NPM)](https://nodejs.org/en/) 。您可以使用终端命令`node -v; npm -v`来检查这些程序。安装完成后，打开一个终端，使用以下命令安装 Angular CLI:`npm install -g @angular/cli`。这可以在系统的任何地方执行。CLI 配置为全局使用，带有`-g`标志。

使用命令`ng -v`验证 CLI 是否存在。这会输出几行信息。其中一行表示安装的 CLI 的版本。

认识到`ng`是 CLI 的基本构件。你所有的命令都会以`ng`开头。是时候看看以`ng`为前缀的四个最常见的命令了。

## 键盘命令

*   ng 新
*   发球
*   ng 生成
*   ng 构建
*   ng 更新

每一项的关键术语都很能说明问题。合在一起，它们就构成了你用 Angular 落地跑步所需要的东西。当然还有很多。所有命令都在 [CLI 的 GitHub 文档¹中有概述。您可能会发现上面列出的命令涵盖了必要的基础。](https://github.com/angular/angular-cli/wiki#additional-commands)

### ng 新

`ng new`创建一个*新的* Angular 文件系统。这是一个超现实的过程。请导航到生成新的*应用程序所需的文件位置。如下键入这个命令，用您想要的任何东西替换`[name-of-app]`:`ng new [name-of-app]`。*

应该会出现文件夹`[name-of-app]`下的文件系统。随意探索内在的东西。尽量不要做任何改变。运行第一个 Angular 应用程序所需的所有东西都打包在这个生成的文件系统中。

### 发球

为了让应用程序运行，必须在`[name-of-app]`文件夹中执行`ng serve`命令。文件夹内的任何地方都可以。Angular CLI 必须识别出它位于使用`ng new`生成的环境中。只要满足这一个条件，它就会运行。去试试吧:`ng serve`。

默认情况下，应用程序运行在端口 4200 上。您可以在任何网络浏览器中导航至`localhost:4200`来查看 Angular 应用程序。Angular 适用于所有浏览器。除非您使用的是旧版本的 Internet Explorer，否则该应用程序会弹出。它显示了官方的 Angular 标志，旁边还有一个有用的链接列表。

好，应用程序运行。希望它能正常工作，但是你需要知道引擎盖下发生了什么。回头参考`[name-of-app]`文件系统。导航`[name-of-app] -> src -> app`。你在`localhost:4200`上看到的那些文件就在那里。

### ng 生成

`.component`文件定义了一个角度组件，包括其逻辑(`.ts`)、样式(`.css`)、布局(`.html`)和测试(`.spec.ts`)。`app.module.ts`尤为突出。这两组文件一起作为`component`和`module`工作。`component`和`module`都是角度示意图的两个独立示例。原理图对不同目的驱动的代码块进行分类*可使用`ng generate`生成*。

对于本文来说，理解一个`module`从一个底层组件树导出和导入资产。一个`component`只关心用户界面的一部分。那个单元的逻辑、风格、布局和测试都封装在不同的`.component`文件中。

对于`ng generate`，该命令可以为每个可用的[角度示意图²生成骨架。导航至`[name-of-app -> src -> app]`。尝试通过执行:`ng generate component [name-of-component]`生成一个新的`component`。把`[name-of-component]`换成你喜欢的任何东西。一个新文件`[name-of-component]`将会弹出，同时弹出的还有其必需的`component`文件。](https://github.com/angular/angular-cli/wiki/generate#available-schematics)

你可以看到`ng generate`加速了 Angular 的[样板代码](https://en.wikipedia.org/wiki/Boilerplate_code)。`ng generate`还连线东西。在 Angular 文件系统环境中创建的原理图与系统的根模块相连接。在这种情况下，那就会是`[name-of-app -> src -> app]`里面的`app.module.ts`文件。

### ng 构建

Angular 是一个前端工具。CLI 代表前端执行其操作。负责后端服务器的设置。这使得开发完全集中在前端。也就是说，将自己的后端连接到 Angular 应用程序也是可能的。

满足了这一需求。然后在文件系统中进行试验。导航至`[name-of-app] -> angular.json`。寻找这一行代码:`"outputPath": "dist/my-app"`。

这一行配置决定了`ng build`将结果转储到哪里。结果是整个 Angular 应用程序被编译成一个文件夹`dist/my-app`。在那个文件夹里面，有`index.html`。整个角度应用可以通过`index.html`运行。从这里不需要`ng serve`。有了这个文件，你可以很容易地连接你的后端。

试一试:`ng build`。同样，这必须在 Angular 文件系统中执行。基于`angular.json`中`“outputPath:”`的键值。将生成一个文件，其中原始应用程序被完全编译。如果保持`“outputPath:”`不变，编译后的应用程序将位于:`[name-of-app] -> dist -> [name-of-app]`。

### ng 更新

在 angular cli ng update 中，将所有 angular 和 npm 软件包自动更新为最新版本。

下面是可以与`ng update`一起使用的语法和选项。

`ng update [package]`

### 选择

*   试运行`--dry-run (alias: -d)`
    不做任何更改地运行。
*   force `--force`
    如果为 false，将会报错已安装的软件包是否与更新不兼容。
*   all `--all`
    是否更新 package.json 中的所有包。
*   接下来`--next`
    使用最大版本，包括 beta 和 RCs。
*   仅迁移`--migrate-only`
    仅执行一次迁移，不更新已安装的版本。
*   从`--from`
    版本开始迁移。仅在更新单个包时可用，并且仅在迁移时可用。
*   到要应用迁移的`--to`
    版本。仅在更新单个程序包时可用，并且仅在迁移时可用。需要指定从。默认为检测到的已安装版本。
*   注册表`--registry`
    要使用的 NPM 注册表。

这些命令涵盖了基础知识。Angular 的 CLI 非常方便，可以加速应用程序的生成、配置和扩展。它在完成所有这些工作的同时保持了灵活性，允许开发人员进行必要的更改。

如果您还没有，请查看`localhost:4200`上的链接。打开之前，不要忘记运行`ng serve`。对 CLI 有了更好的理解后，您现在可以进一步了解其最基本的命令所生成的内容。

## 更多信息:

*   [最佳角度示例](https://www.freecodecamp.org/news/the-best-angular-examples/)
*   [最佳角度和角度教程](https://www.freecodecamp.org/news/best-angular-tutorial-angularjs/)
*   [如何验证角反应形式](https://www.freecodecamp.org/news/how-to-validate-angular-reactive-forms/)