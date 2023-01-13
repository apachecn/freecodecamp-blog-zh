# 超级简单的初学者大口教程

> 原文：<https://www.freecodecamp.org/news/super-simple-gulp-tutorial-for-beginners-45141974bfe8/>

如今，使用构建工具是 web 开发工作流程中不可或缺的一部分。

Gulp 是目前最流行的构建工具之一——还有 Webpack。

但是学习吞咽有一个明确的学习曲线。最大的障碍之一是弄清楚进入它的看似数百个不同的部分。

最重要的是，您必须在命令行上做所有的事情，如果您没有使用过它，这可能会非常令人生畏。

本教程将带您了解 npm(节点包管理器)的基础知识，并为您的前端项目设置 Gulp。一旦你完成了，你将会更加自信的设置你的工作流程和使用命令行！

#### **所以大口喝有什么大不了的？**

吞咽可以节省大量时间。通过使用 Gulp，您可以让电脑处理繁琐的任务，例如:

*   将 Sass 文件编译为 CSS
*   连接(组合)多个 JavaScript 文件
*   缩小(压缩)你的 CSS 和 JavaScript 文件
*   以及当检测到文件改变时自动运行上述任务

Gulp 可以完成比我上面提到的更复杂的任务。然而，本教程将只关注 Gulp 的基础知识及其工作原理。

#### **快速概述我们将要做的事情**

以下是本教程将经历的步骤:

1.  在计算机上安装 Node.js 和 npm
2.  安装 Gulp 和项目所需的其他软件包
3.  配置 gulpfile.js 文件来运行您想要的任务
4.  让你的电脑为你工作！

如果您没有完全理解以上所有术语，请不要担心。我会一步一步解释一切。

现在让我们开始吧！

### 设置您的环境

#### **Node.js**

为了让 Gulp 在您的计算机上运行，您需要将 Node.js 安装到您的本地环境中。

Node.js 自诩为“JavaScript 运行时”，被认为是 JavaScript 的后端。Gulp 使用 Node 运行，因此可以理解，您需要在开始之前安装 Node。

可以从 [Node.js](https://nodejs.org/en/) 网站下载。当您安装 Node 时，它也会将 npm 安装到您的计算机上。

你会问，npm 是什么？

#### **Npm(节点包管理器)**

Npm 是一个不断更新的 JavaScript 插件(称为包)集合，由世界各地的开发者编写。Gulp 就是那些插件中的一个。您还需要几个，我们稍后会谈到。

npm 的美妙之处在于它允许您直接在命令行上安装软件包。这很棒，因为你不必手动去网站，下载并执行文件来安装。

下面是安装软件包的基本语法:

`npm install [Package Name]`

**Mac 用户注意:**
根据您的设置，您可能需要在开头添加“sudo”关键字，以便以 root 权限运行。

那么对于 MAC 来说如果会是什么样子:`sudo npm install [Package Name]`

看起来很简单，对吧？

#### **node _ modules 文件夹**

有一点需要注意:当您安装一个 npm 包时，npm 会创建一个名为 node_modules 的文件夹，并将所有的包文件存储在那里。

如果您曾经有过一个包含 node_modules 文件夹的项目，并且敢于查看它包含的内容，您可能会看到它有许多(我是说许多)嵌套的文件夹和文件。

为什么会这样？

这是因为 npm 软件包倾向于依赖其他 npm 软件包来运行它们的特定功能。这些其他的包被称为依赖项。

如果你正在编写一个插件，利用现有包的功能是有意义的。没有人想每次都重新发明轮子。

所以当你安装一个插件到你的 node_modules 文件夹时，这个插件将会安装额外的包到它自己的 node_modules 文件夹中。

以此类推，直到你在 wazoo 中有了嵌套的文件夹。

此时，您不必太担心弄乱 node_modules 文件夹——只是想简单解释一下那个疯狂的文件夹中发生了什么。

### 使用 package.json 跟踪包

npm 的另一个很酷的特性是它可以记住你为你的项目安装了哪些特定的包。

这是很重要的，以防你因为某种原因不得不重新安装所有的东西。

这也让其他开发人员的生活变得更加轻松，因为他们可以快速轻松地在自己的计算机上安装项目的所有包。

它是如何做到这一点的？

Npm 利用一个名为 package.json 的文件来跟踪您已经安装了哪些包以及包的版本。它还存储了项目的其他信息，如名称、作者和 Git 存储库。

#### **创建你的 package.json**

要初始化这个文件，您可以再次使用命令行。

首先，导航到您的项目文件夹，无论它位于您计算机上的什么位置。

然后输入以下命令:
`npm init`

Npm 随后会提示您输入有关该项目的信息。对于大多数选项，您可以按 enter 键并使用括号中的默认值。

完成后，npm 将在您的项目文件夹中生成 package.json 文件！如果您在编辑器中打开它，您应该会看到类似这样的内容:

```
{
  "name": "super-simple-gulp-file",
  "version": "1.0.0",
  "description": "Super simple Gulp file",
  "main": "gulpfile.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/thecodercoder/Super-Simple-Gulp-File.git"
  },
  "keywords": [
    "gulp"
  ],
  "author": "Jessica @thecodercoder",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/thecodercoder/Super-Simple-Gulp-File/issues"
  },
  "homepage": "https://github.com/thecodercoder/Super-Simple-Gulp-File#readme"
}
```

当然，对于你的项目，你会有你自己的名字和信息，而不是我这里有的。

在这一点上，我不会担心让所有的字段都正确。这个信息部分主要用于作为公共插件发布到 npm 的包。

现在，您**将**放入 package.json 文件的是运行 Gulp 所需的所有包的列表。

让我们看看如何将它们添加进去。

#### **安装软件包**

在上一节中，我们讨论了在命令行中键入:`npm install [Package Name]`来下载软件包并将其安装到 node_modules 文件夹中。

它将安装这个包，并自动将其作为一个依赖项保存到您的 package.json 文件中。

**注意:**在 npm 版本 5.0.0 之前，您必须添加标记“–save ”,以便 npm 将软件包添加为依赖项。在版本 5 及更高版本中，您不必再这样做了。

因此，如果我们想在包中安装 Gulp，我们可以输入:`npm install gulp`。

你的电脑可能需要一两分钟来安装与 Gulp 相关的所有东西。您可能会看到一些警告消息，但我不担心这些，除非安装失败。

现在，如果您打开 package.json 文件，您会在底部看到 Gulp 已经被添加为一个依赖项:

```
"dependencies": { "gulp": "^3.9.1" }
```

随着您安装额外的 npm 软件包，此依赖项列表将会增长。

#### **吞咽所需的其他包裹**

最初，我们希望使用 Gulp 来运行诸如编译 SCSS/CSS 和 JavaScript 文件之类的任务。为此，我们将使用以下软件包:

*   [gulp-sass](https://www.npmjs.com/package/gulp-sass) —将您的 sass 文件编译成 CSS
*   [gulp-cssnano](https://www.npmjs.com/package/gulp-cssnano) —缩小你的 CSS 文件
*   [gulp-concat](https://www.npmjs.com/package/gulp-concat) —将多个 JavaScript 文件连接(组合)成一个大文件
*   [囫囵吞下](https://www.npmjs.com/package/gulp-uglify) —缩小你的 JavaScript 文件

就像之前一样，通过一个接一个地输入这些行来安装每个包。在进入下一行之前，您必须等待几秒钟，让每个程序安装完毕。

```
npm install gulp-sass 
npm install gulp-cssnano 
npm install gulp-concat 
npm install gulp-uglify
```

#### **Gulp-cli vs 全局 Gulp**

在过去，为了能够从命令行运行“gulp”，您必须在本地计算机上全局安装 Gulp，使用命令:

`npm install –global gulp`

然而，如果您有多个项目都需要不同版本的 Gulp，那么拥有一个全球版本的 Gulp 可能会带来问题。

目前的共识是建议在全球范围内安装一个不同的软件包，Gulp-cli，而不是 Gulp 本身。

这将允许您仍然运行“gulp”命令，但是您可以在不同的项目中使用不同版本的 Gulp。

下面是代码:

```
npm install --global gulp-cli
```

如果你感兴趣，你可以阅读更多关于这个[树屋线程](https://teamtreehouse.com/community/gulp-global-install-gulp-vs-gulpcli)的内容。

好了，一旦你所有的包都安装好了，你就有了你需要的所有工具。让我们继续设置我们的项目文件！

### 设置您的文件结构

在我们开始创建文件和文件夹之前，要知道有许多不同的方法来设置你的文件结构。您将使用的方法适用于基本项目，但是“正确的”设置将在很大程度上取决于您的特定需求。

这个基本方法将帮助你掌握所有运动部件的基本功能。然后，您可以在将来根据自己的喜好构建或更改设置！

下面是项目树的样子:

**根项目文件夹**

*   index.html
*   gulpfile.js
*   package.json
*   节点模块(文件夹)
*   应用程序(文件夹)
*   script.js
*   style.scss
*   dist (folder)

我们已经检查了 package.json 文件和 node_modules 文件夹。当然，index.html 的文件将会是你网站的主要文件。

在 gulpfile.js 文件中，我们将配置 Gulp 来运行我们在本文开始时讨论的所有任务。我们一会儿会谈到这一点。

但是现在我想提一下 app 和 dist 这两个文件夹，因为它们对 Gulp 工作流很重要。

#### **App 和 dist 文件夹**

在 app 文件夹中，我们有基本的 JavaScript 文件(script.js)和基本的 SCSS 文件(style.scss)。这些文件是您编写所有 JavaScript 和 CSS 代码的地方。

dist 文件夹的存在只是为了存储 Gulp 处理完的最终编译的 JavaScript 和 CSS 文件。您不应该在 dist 文件中进行任何更改，只能在 app 文件中进行更改。但是 dist 中的这些文件将被加载到 index.html，因为我们想在网站中使用编译后的文件。

同样，有很多方法可以设置项目文件和文件夹。要记住的最重要的一点是，你的结构是合理的，能让你最有效地工作。

现在让我们进入本教程的核心部分:配置 Gulp！

### 创建和配置您的 Gulpfile

Gulpfile 包含加载已安装的软件包和运行不同功能的代码。该代码执行两个基本功能:

1.  将已安装的软件包初始化为节点模块。
2.  创建和运行吞咽任务。

#### **初始化软件包**

为了利用您添加到项目中的 npm 包的所有特性，您需要将它们导出为 Node 中的模块，因此文件夹名为“node_modules”。

在 Gulpfile 的顶部，像这样添加模块:

```
var gulp = require('gulp'); 
var cssnano = require('gulp-cssnano'); 
var sass = require('gulp-sass'); 
var concat = require('gulp-concat'); 
var uglify = require('gulp-uglify');
```

既然已经添加了包，那么就可以在 Gulpfile 脚本中使用它们的函数和对象了。您还将使用 Node.js 的一些内置函数。

如果你想了解更多关于 npm 包和节点模块的信息，npm 网站有一个很好的解释[在这里](https://docs.npmjs.com/getting-started/packages)。

### **创建你的吞咽任务**

使用以下代码创建一个 Gulp 任务:

```
gulp.task('[Function Name]', function(){    
   // Do stuff here 
}
```

这允许您通过在命令行中键入`gulp [Function Name]`来运行 Gulp 任务。这很重要，因为您可以从其他 Gulp 任务中运行该命名函数。

具体来说，我们正在构建几个不同的 Gulp 任务，当您运行默认的 Gulp 任务时，这些任务将全部运行。

我们将使用的一些主要功能有:

*   `.task()` —创建一个任务，如上所述
*   `.src()` —标识您将在特定任务中编译的文件
*   `.pipe()` —向 Gulp 正在使用的节点流添加函数；你可以在同一个任务中实现多个函数(在 [florian.ec](https://florian.ec/articles/gulp-js-streams/) 上阅读关于这个主题的精彩文章)
*   `.dest()` —将结果文件写入特定位置
*   `.watch()` —识别文件以检测任何更改

如果你好奇，你可以在这里阅读更多关于 Gulp 的文档。

都准备好了吗？现在言归正传(提示木兰音乐)写那些任务！

我们希望 Gulp 运行以下任务:

*   Sass 任务编译 SCSS 到一个 CSS 文件和 minify
*   连接 JavaScript 文件和缩小/丑陋的 JavaScript 任务
*   观察任务以检测 SCSS 或 JavaScript 文件何时被更改，并重新运行任务
*   当您在命令行中键入`gulp`时，运行所有需要的任务的默认任务

#### **Sass 任务**

对于 sass 任务，首先我们想使用`task()`创建任务，我们将它命名为“Sass”

然后，我们添加任务将运行的每个组件。首先，我们将使用`src()`指定源文件为 app/scss/style.scss 文件。然后，我们将管道在额外的功能。

第一个运行`sass()`函数——使用 Gulpfile 顶部的 gulp-sass 模块，我们称之为“sass”。它会自动保存与 SCSS 文件同名的 CSS 文件，因此我们的文件将被命名为 style.css

第二个用`cssnano()`缩小 CSS 文件。最后一个将生成的 CSS 文件放在 dist/css 文件夹中。

以下是所有这些的代码:

```
gulp.task('sass', function(){    
    return gulp.src('app/style.scss')       
        .pipe(sass())       
        .pipe(cssnano())       
        .pipe(gulp.dest('dist/css')); 
});
```

为了测试，我在 style.scss 文件中放入了一些填充 SCSS:

```
div {    
    display: block; 
   	&.content {       
        position: relative;    
    } 
} 

.red { 
    color: red; 
}
```

如果您键入`gulp`和任务名称，您可以在命令行上运行每个单独的 Gulp 任务。所以为了测试 Sass 任务，我输入了`gulp sass`来检查它是否工作正常，并生成了 minified dist/style.css 文件。

如果一切正常，您应该在命令行中看到如下消息:

```
[15:04:53] Starting 'sass'... [15:04:53] Finished 'sass' after 121 ms
```

检查 dist 文件夹显示确实有一个 style.css 文件，打开它显示正确缩小的 css:

```
div{display:block}div.content{position:relative}.red{color:red}
```

好了，我们的 Sass 任务现在完成了。转到 JavaScript！

#### 任务

JS Gulp 任务类似于 Sass 任务，但是有一些不同的元素。

首先，我们将创建任务并将其命名为“js”，然后我们将识别源文件。在`src()`函数中，你可以用几种不同的方式识别多个文件。

一种是利用通配符`(*)`告诉 Gulp 使用所有扩展名为`*.js`的文件，如下所示:

```
gulp.src('app/*.js')
```

但是，这将按字母顺序编译文件，如果您在加载其他脚本文件之前加载依赖于其他脚本的脚本，这可能会导致错误。

如果没有太多脚本文件，可以通过手动指定每个 JavaScript 文件来控制顺序。

通过使用方括号，`src()`函数可以将一组值作为参数:

```
gulp.src(['app/script.js', 'app/script2.js'])
```

如果你有很多 JavaScript 文件，你可以确保首先加载依赖项，将它们保存在一个单独的子文件夹中，比如“app/js/plugins”。然后将其他 JavaScript 文件保存在父“app/js”文件夹中。

然后，您可以使用通配符符号加载所有 lib(库)脚本，后面是常规脚本:

```
gulp.src(['app/js/lib/*.js', 'app/js/script/*.js'])
```

根据您拥有的 JavaScript 文件的数量和类型，您的方法会有所不同。

一旦你设置了源文件，你就可以用管道输入剩下的函数。第一种方法是将文件连接成一个大的 JavaScript 文件。`concat()`函数需要一个带有结果文件名称的参数。

然后，您将修改 JavaScript 文件，并将其保存在目标位置。

以下是 JS 任务的完整代码:

```
gulp.task('js', function(){    
    return gulp.src(['app/js/plugins/*.js', 'app/js/*.js'])          
        .pipe(concat('all.js'))       
        .pipe(uglify())       
        .pipe(gulp.dest('dist')); 
});
```

就像 Sass 任务一样，您可以通过在命令行中键入`gulp js`来测试 JS 任务是否工作。

```
[14:38:31] Starting 'js'... [14:38:31] Finished 'js' after 36 ms
```

现在我们已经完成了两个主要的 worker Gulp 任务，我们可以继续观察任务了。

#### **观看任务**

监视任务将监视您告诉它的文件是否有任何更改。一旦检测到变化，它将运行您指定的任务，然后继续观察变化。

我们将创建两个观察函数，一个观察 SCSS 文件，另一个观察 JavaScript 文件。

`watch()`函数有两个参数:源位置，以及检测到变化时运行的任务。

Sass 监视功能将监视 app 文件夹中的任何 SCSS 文件，如果检测到更改，则运行 Sass 任务。

该函数将如下所示:

```
gulp.watch('app/*.scss', ['sass']);
```

对于 JS Watch 函数，我们必须利用一个真正有用的节点特性，称为“globbing”Globbing 是指使用“**”符号作为文件夹和子文件夹的一种通配符。对于 JavaScript 文件，我们需要它，因为我们在 app/js 文件夹中有一个 JavaScript 文件，在 app/js/plugins 文件夹中有一个 JavaScript 文件。

下面是这个函数的样子:

```
gulp.watch('app/js/**/*.js', ['js']);
```

glob(“* *”)的工作方式是它将在 app/js 文件夹中的任何地方查找 JavaScript 文件。它将直接在文件夹中或任何后续的子文件夹中查找，如插件文件夹。Globbing 非常方便，因此您不必在`watch()`函数中将每个子文件夹指定为单独的源。

下面是完整的监视任务:

```
gulp.task('watch', function(){       
	gulp.watch('app/*.scss', ['sass']);          
    gulp.watch('app/js/**/*.js', ['js']); 
});
```

现在我们差不多完成了！最后创建的任务是默认的 Gulp 任务。

#### **默认大口任务**

默认的 Gulp 任务是您在命令行中键入`gulp`时想要运行的任务。当您创建任务时，您必须将其称为“默认”，以便 Gulp 能够识别您想要运行的任务。

我们想要做的是运行一次 Sass 和 JS 任务，然后运行监视任务，以便在文件发生更改时重新运行任务。

```
gulp.task('default', ['sass', 'js', 'watch']);
```

您可以创建其他任务来运行您的构建，只是不要重用“默认”名称。例如，假设您想让 CSS 和 JavaScript 文件在默认情况下保持不变，但是您确实想在生产中缩小它们。

您可以创建单独的 Gulp 任务来缩小您的 CSS 和 JavaScript 文件，称为“minifyCSS”和“minifyJS”然后，您不会将这些任务添加到默认的 Gulp 任务中，但是您可以创建一个名为“prod”的新 Gulp 任务，它拥有默认任务的所有内容，也拥有您的 minify 任务。

#### **您的 index.html 中的参考资料**

一旦你让你的吞咽过程工作，确保你的 index.html 文件引用所有正确的 CSS 和 JavaScript 文件。

对于我在这里给你的例子，你会想要在你的<头>中添加一个对`dist/style.css`的 CSS 引用:

```
<link rel="stylesheet" href="dist/style.css">
```

并在你的<主体>中添加一个引用`dist/all.js`的脚本标签:

```
<script src="dist/all.js"><;/script>
```

### 最后

恭喜你通过了！我希望这个基本的吞咽教程对你有所帮助。

就像我在开头提到的，这只是一个非常简单的 npm 和 Gulp 基础教程。

大多数开发者会在他们的 Gulpfile 中添加许多额外的任务。如果你有兴趣看另一篇关于这些更高级主题的文章，请告诉我！

最后，你可以在我的 GitHub 账号[这里](https://github.com/thecodercoder/Super-Simple-Gulp-File)查看本教程的所有代码。

我希望这篇文章对你有帮助！请在下面的评论中告诉我你的想法。

#### 想要更多吗？

*   在我的博客 c[oder-coder.com 上阅读更多教程。](https://coder-coder.com)
*   点击此处获取关于新文章的电子邮件。
*   加入 25，000 多名其他人——在 Instagram 上关注@th [ecodercoder。](https://www.instagram.com/thecodercoder/)
*   在我的 YouTube 频道上查看编码教程。

*本文最初发表于[Coder-Coder.com](https://coder-coder.com/gulp-tutorial-beginners/)。*