# 在 Sass 和 JS 文件的工作流程中使用 Gulp 4

> 原文：<https://www.freecodecamp.org/news/gulp-4-walk-through-with-example-code-c3c018eab306/>

*本文最初发表于[Coder-Coder.com](https://coder-coder.com/gulp-4-walk-through/)。*

本教程将一步一步地解释如何在你的工作流程中设置 Gulp 4，以及如何从 Gulp 3 迁移到 Gulp 4 语法。

> 需要快速完成吞咽 3 gulpfile.js 使用 Gulp 4？[点击这里](#migrating)直接进入文章的那个部分。

1.  通过运行`npm install gulp-cli -g`在命令行上安装 gulp-cli。
2.  运行`npm install gulp`安装 Gulp。
3.  为您的 Gulp 工作流安装其他 npm 包。
4.  在项目根目录下创建一个`gulpfile.js`文件。
5.  将 npm 包作为模块导入 gulpfile。
6.  将您的任务添加到 gulpfile 中，以编译您的 SCSS/JS 文件并运行观察任务。
7.  运行`gulp`命令运行所有任务。

### 什么是 Gulp，它有什么作用？

Gulp 是一个工具，它将在你的 web 开发工作流程中为你运行各种任务。它可能被称为捆绑器、构建工具或任务运行器。它们的意思大致相同。

类似的工具有 Webpack 和 Grunt(尽管 Grunt 很快就过时了)。

下面是我们将让 Gulp 为我们做的事情:

1.  将我们的 Sass 文件编译成 CSS
2.  向我们的 CSS 添加供应商前缀
3.  缩小我们最终的 CSS 文件
4.  连接(即合并)我们的 JS 文件
5.  丑化我们最终的 JS 文件
6.  将最终的 CSS/JS 文件移动到我们的`/dist`文件夹中。

很酷，对吧？！

所以它的工作方式是，你所有的设置和任务都存储在一个`gulpfile.js`文件中。然后在命令行上运行 Gulp。

最棒的是，一旦你建立了 gulpfile，你可以很容易地在其他项目中重用它。所以这是一个巨大的时间节省！

让我们继续在你的电脑上安装和设置 Gulp。

### 安装 Gulp，使用一个工作演示项目

在运行 Gulp 之前，您需要安装一些东西:

*   如果还没有的话安装 [Node.js](https://nodejs.org/en/) 。
*   通过运行`npm install --global gulp-cli`安装 [Gulp 命令行实用程序](https://www.npmjs.com/package/gulp-cli)。

一旦你使用了 Gulp，看看我构建的一个使用 Gulp 的演示项目！这是一个前端样板项目，旨在为我快速入门任何简单的前端网站。

(在本教程的其余部分，我还有大量的代码片段，所以你也可以参考它们！)

**设置前端样板工程:**

*   克隆或下载这个项目的 [Git repo](https://github.com/thecodercoder/frontend-boilerplate) 到你的电脑上。
*   打开项目，在根项目文件夹中，转到命令行并运行`npm install`。这将安装在`package.json`文件中列出的 npm 包，尤其是 Gulp 4。

现在您应该已经设置好了项目文件，并且安装了我们将用来运行 Gulp 任务的所有 npm 包。

由于 repo 中的文件已经准备好了，如果您在命令行中键入`gulp`，您应该会看到如下输出:

```
> gulp [22:29:48] Using gulpfile ~\Documents\GitHub\frontend-boilerplate\gulpfile.js [22:29:48] Starting 'default'... [22:29:48] Starting 'scssTask'... [22:29:48] Starting 'jsTask'... [22:29:48] Finished 'jsTask' after 340 ms [22:29:48] Finished 'scssTask' after 347 ms [22:29:48] Starting 'watchTask'...
```

你已经跑了一大口！

### 运行 Gulp 时，项目中发生了什么？

好了，你成功地完成了这个项目！现在让我们更详细地了解一下这个项目的内容以及它是如何工作的。

首先，这里有一个我们项目的文件结构的快速纲要:

*   **index.html**—你的主 HTML 文件
*   **package.json** —包含运行`npm install`时要安装的所有 npm 包。
*   **gulpfile.js** —包含配置并运行所有 Gulp 任务
*   **/app** —工作文件夹，您将在这里编辑 SCSS/JS 文件
*   **/dist** — Gulp 将在这里输出文件，不要编辑这些文件

在您的工作流程中，您将编辑 HTML、SCSS 和 JS 文件。然后 Gulp 将检测任何变化并编译所有内容。然后，您将从您的`index.html`文件中的`/dist`文件夹加载您的最终 CSS/JS 文件。

现在我们已经运行了 Gulp，让我们更仔细地看看 Gulp 是如何工作的。

### gulpfile.js 里面到底有什么？

下面是对 gulpfile 每个部分的深入解释，以及它们的作用:

#### 步骤 1:初始化 npm 模块

在`gulpfile.js`的最顶端，我们有一大堆常量，它们使用`require()`函数导入我们之前安装的 npm 包。

以下是我们的产品包的作用:

大口包装:

*   `gulp` —运行 gulpfile.js 中的所有内容。我们只导出将要使用的特定 gulp 函数，如`src`、`dest`、`watch`等。这允许我们不使用`gulp`直接调用那些函数，例如我们可以只输入`src()`而不是`gulp.src()`。

CSS 包:

*   `gulp-sourcemaps` —将 CSS 样式映射回浏览器开发工具中的原始 SCSS 文件
*   `gulp-sass` —将 SCSS 编译成 CSS
*   `gulp-postcss` —运行 autoprefixer 和 cssnano(见下文)
*   `autoprefixer` —向 CSS 添加供应商前缀
*   `cssnano` —缩小 CSS

> *如果你以前用过 Gulp，你可能会奇怪为什么我用直接的`autoprefixer`和`cssnano`，而不是`gulp-autoprefixer`和`gulp-cssnano`。出于某种原因，使用它们可以工作，但是会阻止 sourcemaps 工作。点击阅读更多关于这个问题的信息[。](https://github.com/gulp-sourcemaps/gulp-sourcemaps/issues/60)*

JS 包:

*   `gulp-concat` —将多个 JS 文件连接成一个文件
*   `gulp-uglify` —缩小 JS

现在我们已经添加了模块，让我们将它们投入使用吧！

#### 步骤 2:使用模块运行您的任务

Gulp 中的“任务”基本上是执行特定目的的功能。

我们正在创建一些实用程序任务来编译我们的 SCSS 和 JS 文件，并观察这些文件的任何变化。然后，当我们在命令行中键入`gulp`时，这些实用程序任务将在我们默认的 Gulp 任务中运行。

#### **为文件路径创建常量**

不过，在编写任务之前，我们有几行代码将文件路径保存为常量。这是可选的，但我喜欢使用路径变量，这样我们就不必每次都手动重新键入它们:

这段代码创建了`scssPath`和`jsPath`常量，我们将使用它们来告诉 Gulp 文件在哪里被找到。

#### **Sass 任务**

下面是我们的 Sass 任务的代码:

```
function scssTask(){        return src(files.scssPath)        .pipe(sourcemaps.init())        .pipe(sass())        .pipe(postcss([ autoprefixer(), cssnano() ]))        .pipe(sourcemaps.write('.'))        .pipe(dest('dist')    );}
```

我们的 Sass 任务叫做`scssTask()`，正在做几件事情。在 Gulp 中，你可以在第一个函数之后使用 Gulp 函数`pipe()`来链接多个函数。

在我们的任务中，Gulp 首先运行`src()`来加载 SCSS 文件的源目录。它使用了我们之前创建的常量`files.scssPath`。

然后在`src()`之后，我们将所有其他东西都加入到构建过程中。你可以把它想象成在现有管道上增加越来越多的管道段。

它运行的功能有:

*   `sourcemaps.init()` — sourcemaps 需要在`src()`之后首先添加
*   将所有的 SCSS 文件编译成一个 CSS 文件
*   `postcss()`运行另外两个插件:
*   - `autoprefixer()`向 CSS 添加供应商前缀
*   - `cssnano()`缩小 CSS 文件
*   `sourcemaps.write()`在同一目录下创建 sourcemaps 文件。
*   `dest()`告诉 Gulp 将最终的 CSS 文件和 CSS sourcemaps 文件放到`/dist`文件夹中。

#### **JS 任务**

以下是我们的 JavaScript 任务的代码:

```
function jsTask(){    return src([files.jsPath])        .pipe(concat('all.js'))        .pipe(uglify())        .pipe(dest('dist')    );}
```

我们的 JavaScript 任务叫做`jsTask()`，它也运行多个函数:

*   `src()`从`files.jsPath`加载 JS 文件。
*   `concat()`将所有 JS 文件连接成一个 JS 文件。
*   `uglify()`丑化/缩小 JS 文件。
*   `dest()`将最终的 JS 文件移动到`/dist`文件夹中。

#### **观看任务**

`watch()`功能是 Gulp 的一个超级好用的部分。当您运行它时，它将持续运行，等待检测您让它监视的文件的任何更改。当它检测到变化时，它会运行你告诉它的任何数量的任务。

这很棒，因为这样你就不必在每次代码更改后都手动运行`gulp`。

下面是我们监视任务的代码:

```
function watchTask(){    watch(        [files.scssPath, files.jsPath],        parallel(scssTask, jsTask)    );}
```

`watch()`函数有三个参数，但我们只用了两个:

*   globs 表示文件路径字符串，
*   选项(未使用)，以及
*   任务，意味着我们想要运行哪些任务。

我们让我们的观察任务做的是观察我们的`scssPath`和`jsPath`目录中的文件。然后，如果其中任何一个发生变化，同时运行`scssTask`和`jsTask`任务。

既然我们已经设置了实用程序任务，我们需要设置主 Gulp 任务。

#### **默认任务**

这是主 Gulp 任务，如果你在命令行输入`gulp`，它将自动运行。

```
exports.default = series( parallel(scssTask, jsTask), watchTask);
```

当你使用`gulp`命令时，Gulp 会自动在你的`gulpfile.js`中搜索一个`default`任务。因此，您必须导出默认任务才能使其工作。

我们的默认任务将执行以下操作:

*   使用`parallel()`同时运行`scssTask`和`jsTask`
*   然后运行`watchTask`

您还会注意到，我们将所有这些任务都放在了`series()`函数中。

**这是 Gulp 4 在处理任务方面的主要新变化之一——你需要将所有任务(甚至是单个任务)包装在`series()`或`parallel()`中。**

### 注册任务的注意事项:Gulp 4 中有什么变化

如果你一直使用`tasks()`函数来运行一切，你可能已经注意到现在有所不同。

我们没有使用`gulp.task()`来包含所有的任务函数，而是创建了像`scssTask()`和`watchTask()`这样的实际函数。

这是遵循 Gulp 官方的创建任务指南。

Gulp 团队建议使用`exports`而不是`tasks()`:

> *“以前用`task()`把你的功能注册成任务。虽然该 API 仍然可用，但导出应该是主要的注册机制，除非在导出不起作用的边缘情况下。”【[大口文档](https://gulpjs.com/docs/en/getting-started/creating-tasks)】*

因此，虽然他们仍然允许您使用`task()`函数，但更流行的做法是为每个任务创建 JS 函数，然后导出它们。

如果可以的话，我建议更新你的语法来反映这一点，因为 Gulp 可能会在某个时候反对`task()`。

### 从 Gulp 3 迁移过来的问题？

如果你一直在使用 Gulp 3，而你想要的只是让这个该死的东西和 Gulp 4 一起工作，那你真幸运！

以前，在 Gulp 3 中，您可以简单地在一个数组中列出一个或多个函数。然而，在 Gulp 4 中，这样做而不将它们包装在`series()`或`parallel()`中会抛出一个错误。

类似于:

`AssertionError [ERR_ASSERTION]: Task function must be specified`

Gulp 4 引入了两个运行任务的新函数:`series()`和`parallel()`。它让您可以选择同时运行多个任务，或者一个接一个地运行。

因此，为了快速修复错误，将所有任务放入一个`series()`或`parallel()`函数中。

**(旧)吞咽 3 语法中的任务**

在 Gulp 3 中，您可能会这样运行任务:

`gulp.task('default', ['sass', 'js', 'watch']);`

`gulp.watch('app/scss/*.scss', ['sass']);`

**任务一饮而尽 4 语法**

将这些任务放入 series()函数中，如下所示:

`gulp.task('default', gulp.series('sass', 'js', 'watch'));`

`gulp.watch('app/scss/*.scss', gulp.series('sass'));`

这将通过尽可能小的改变来修复任务函数错误！？

### **项目文件下载**

我在这里展示的所有代码都来自我为前端样板文件准备的 GitHub 存储库。它是任何简单的前端网站项目的快速入门工具包。

欢迎您查看、定制并将其用于您自己的项目！

点击这里查看 GitHub 库。

### **在关闭时**

我希望你已经发现这个如何让 Gulp 4 运行的演练很有帮助！

如果你喜欢这篇文章或者有任何问题，请在下面留下你的评论。？

**想要更多？**

？在我的博客 c[oder-coder.com 上阅读更多教程。](https://coder-coder.com)
？在这里注册以获得关于新文章的电子邮件。T5？加入 24，000 多名其他人——关注 Instagram 上的@ [编码器。](https://www.instagram.com/thecodercoder/)
？在我的 YouTube 频道上查看编码教程。