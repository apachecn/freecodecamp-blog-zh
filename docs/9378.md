# 大口喝！我改进了我的工作流程！

> 原文：<https://www.freecodecamp.org/news/gulp-i-improved-my-workflow-354d31d25655/>

斯特凡诺·格拉西

# 大口喝！我改进了我的工作流程！

#### 又一次使用 Gulp.js 的亲身体验

![1*3vp5N6O1BBr79sdNtU6cQg](img/131edba4b82f8e677c3d46aeaedef132.png)

Jökulsárlón, Iceland by [Jeremy Goldberg](https://unsplash.com/jeremy)

除非你在过去的几年里一直生活在岩石下，否则前端开发人员可以使用的工具数量已经快速增长。我们现在拥有的是一系列致力于加速、自动化和提高工作流程质量的项目。

例如，想象一下:

*   将[SASS](https://www.npmjs.com/package/gulp-sass)/[LESS](https://www.npmjs.com/package/gulp-less)/[post CSS](https://www.npmjs.com/package/postcss)/[Stylus](https://www.npmjs.com/package/gulp-stylus)编译成一个缩小的 CSS，[根据您的需求定制](https://www.npmjs.com/package/gulp-uncss)，[自动前缀](https://www.npmjs.com/package/gulp-autoprefixer)并实时
*   [连接](https://www.npmjs.com/package/gulp-concat)和[缩小](https://www.npmjs.com/package/gulp-uglify)你的脚本
*   压缩从[模板](https://www.npmjs.com/package/gulp-file-include)和原子模块创建的 html 文件
*   [预览](http://www.browsersync.io/)在编译期间，从虚拟服务器上预览您的 webapp，自动刷新并同步到您的所有设备
*   测试您的网络性能
*   项目的自我更新风格- [指南](https://trulia.github.io/hologram/)
*   [压缩](https://www.npmjs.com/package/gulp-imagemin)图像并创建[精灵](https://www.npmjs.com/package/gulp.spritesmith)

几年前，这听起来更像是迪斯尼乐园的一个梦，但我们生活在未来，所以不要害怕！[咕哝](http://gruntjs.com/)、[含羞草](http://mimosa.io/)、[西兰花](http://broccolijs.com/)和[大口](http://gulpjs.com/)来救场！

每个系统都有自己的优点。我根据自己的需要选择了 Gulp，但在决定哪一个最适合你之前，请确保检查所有这些产品。

#### 所以……一饮而尽？那是什么？

[**gulpjs/gulpjs**](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md)
[*gulps—流媒体构建系统*github.com](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md)

正如其网站所言，Gulp 是一个“[流构建系统](http://gulpjs.com/)”，这意味着你可以设置自己的任务在管道上运行，监控文件夹的变化并重新运行。

而且是**超级简单的**。

![1*_PQJkvbZJNE_BjXpvIGCPQ](img/28ab30e6ab5e0581107a9b3788c05363.png)

That’s ingenious, if I understand it correctly.
It’s a Swiss watch.
Yeah, the beauty of this is its simplicity.

#### 吞下基本概念

让我们细细品味主要元素

**gulp.task**
又名你想要实现的动作。管理 CSS？生成文档？用 [orchestrator](https://github.com/robrich/orchestrator) 来定义它们，这个模块允许我们定义依赖性和最大并发性

```
gulp.task(‘somename’, function() { // Do stuff});
```

![1*MRor084bOQstofwPYVjaFQ](img/8cd5738dc207c244e30432392897d558.png)

**gulp.watch**
又名你想保持检查的文件夹的变化

```
gulp.watch(‘folder/*.ext’, [‘firstTask’, ‘secondTask’]);
```

每一个**流**都来源于匹配特定**团**(文件需要匹配的模式)的源

```
gulp.src(globs[, options])
```

一系列**管道**(动作)

```
.pipe(concat()),.pipe(minify())
```

和一个**目的地**

```
gulp.dest(path[, options])
```

操作起来，gulp 需要两个核心文件， **package.json** 和 **gulpfile.js.**
*(关于 gulp 的安装，遵循官方文档)*

#### Gulpfile.js

在 **gulpfile** 中，我们将声明我们将要使用哪些插件，我们想要运行的任务，我们将要监视哪些文件夹，等等…

#### Package.json

**package.json** 文件用于存储关于项目依赖关系的所有信息(包括 gulp！).

![1*p1_LFP4jZEH6b9asHPueyg](img/4006a64d9ca1668f2fe1da65b8a85563.png)

*   去**创造**它

```
$ npm init
```

系统会提示您为文件标题输入一些基本信息，如作者姓名、项目名称等。

*   为了**安装**一个插件并保存对 json 文件的依赖

```
$ npm install --save-dev yourPluginName
```

*   卸载插件并移除对 json 文件的依赖

```
$ npm uninstall --save-dev yourPluginName
```

*   如果你需要从编译好的 package.json 安装所有的依赖项

```
$ npm install
```

#### 设计机构

这是我用 Gulp 管理项目来组织文件夹的方法

#### 插件 FTW！

Gulp 有一个令人印象深刻的插件列表(在我写这篇文章的时候)

[**gulp.js 插件注册表**](http://gulpjs.com/plugins/)
[*发现 gulp.js 插件*gulpjs.com](http://gulpjs.com/plugins/)

**必须有**

*   这是一个惰性加载插件的程序，安装在你的项目中。你给它分配一个变量，并使用它来引用其他插件，而不是为每个其他插件重复需求声明。

```
var $ = require('gulp-load-plugins')();
```

```
// Example for gulp-concat.pipe($.concat('main.js'))
```

*   [browsersync](http://www.browsersync.io/docs/gulp/)
    连接到相同 URL(本地主机或局域网)的每台设备上的页面在任何变化时刷新
*   我最喜欢的性能测试工具
*   你在使用类似 Bootstrap 的 css 框架来制作登陆页面吗？
    你需要这个。

#### 什么？你会问，我如何更新 Gulp 插件？

```
$ npm install -g npm-check-updates
```

```
$ npm-check-updates -u
```

```
$ rm -fr node_modules
```

```
$ npm install
```

> 基本上，这将全局安装 **npm-check-updates** ，针对您的 package.json 运行它，并更新依赖版本。
> 然后你只需删除节点模块文件夹并重新安装。

> 来自:[https://stack overflow . com/questions/27024431/updating-gulp-plugins](https://stackoverflow.com/questions/27024431/updating-gulp-plugins)

注意:作为一般规则，也是最后的手段，我们最好用以下方法来清理 npm 缓存

```
$ npm cache clean
```

#### *就这些了，各位！谢谢你走到这一步！*

#### 我希望我让你有足够的兴趣去查看这篇文章中的一些链接。他们在那里是因为我想表达我对其他开发者为社区所做的伟大工作的支持。