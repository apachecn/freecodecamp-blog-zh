# 如果你是初学者，如何使用命令行设置 Gulp-sass

> 原文：<https://www.freecodecamp.org/news/how-to-set-up-gulp-sass-using-the-command-line-if-youre-a-beginner-17729f53249/>

西蒙·贝洛

我目前在一家科技公司实习，几天前我的老板向我挑战写一篇文章。所以我决定写一些关于吞咽的东西。设置它有时会令人沮丧，尤其是当你还是个新手的时候。我使用 Windows，搜索一篇能解决我的问题的文章就像让杰克用黑色拼出“减少”一样困难。

好吧，我想我有点忘乎所以了…我说够了，让我们开始吧！

这是我发表的第一篇文章，希望你会喜欢。:)

### 安装节点

首先，打开命令行，在电脑上安装 [node.js](https://nodejs.org) 。它附带了一个节点包管理器(npm ),可以用来安装 Gulp。安装完成后，运行`npm install gulp -g`即可安装 Gulp。`**-g**` 指示 **npm** 在你的电脑上全局安装 gulp(这意味着你可以在你电脑的任何地方使用 Gulp 命令。)

在我继续之前，我假设您熟悉命令行！

导航到您的项目目录并运行`npm init`。这将创建一个 package.json 文件，按 enter 键，它会将您需要的内容添加到
package . JSON 文件中。

是的，你可能想知道 package.json 文件是什么？一个 [package.json 文件](https://docs.nodejitsu.com/articles/getting-started/npm/what-is-the-file-package-json/)保存了与你的项目相关的各种元数据。该文件向npm 提供信息，并允许其识别项目以及处理项目的依赖关系。这也使得安装 Gulp 项目中使用的所有任务变得更加容易。

如果你还是不明白，你可能需要黛安更好地解释一下——我对黑人的问题/迷恋是什么？？

运行`npm-init`之后，键入`npm install gulp --save-dev` **，**这指示 **npm** 将 Gulp 安装到您的项目中。通过使用`--save-dev`，我们将 Gulp 作为一个开发依赖项存储在 package.json 中

### 创建 Gulpfile

既然已经安装了 Gulp，就可以开始安装第一个任务了。你不得不吞咽。在项目目录中创建一个名为 gulpfile.js 的新文件——可以使用任何文本编辑器来完成。从将下面的代码添加到 gulpfile 开始。

```
‘use strict’;
```

```
var gulp = require(‘gulp’);
```

### 设置任务

现在，您可以安装一个 **gulp 任务—** 在本例中，我们将安装 Gulp-sass。这个任务使得将 *Sass 转换成 CSS* 成为可能。仍然使用命令行，可以通过运行`**npm** install gulp-sass --save-dev`来安装 Gulp-sass。之后，在 gulpfile.js 中要求 Gulp-sass。

在你要求吞咽的线下划上`var sass = require(‘gulp-sass’);`。

### 构建您的项目

在你使用下面的代码之前，我还假设你知道如何构建一个 web 应用程序。这里我将使用常见 web 应用程序的结构。

![Qe0oi9tR9oQ1WTgIJxWTV0VTBnZEHAkHEE2s](img/cea282beb63546634b42d83dbde80876.png)

### 编译 sass/scss

最后一件事是指示 gulp 它需要转换什么文件，以及目标应该在哪里——输出文件将存储在哪里。

使用以下内容；

```
//compile gulp.task(‘sass’, function () { gulp.src(‘app/scss/app.scss’) .pipe(sass().on(‘error’, sass.logError)) .pipe(gulp.dest(‘app/css’)); });
```

**gulp.src** 中的文件将被转换，您也可以全选。使用`“app/scss/*.scss”`将 scss 文件放在一个目录中。这将选择您所有的。文件夹中的文件。

**gulp.dest** 为输出。输出将存储在 app 文件夹内的 CSS 文件夹中。

### 大口-手表-萨斯

Gulp 有一个监视语法，允许它监视源文件，然后“监视”对代码所做的更改。只需通过键入以下命令将语法添加到 gulpfile.js 中:

```
//compile and watch gulp.task(‘sass:watch’, function() { gulp.watch(‘app/scss/app.scss’, [‘sass’]);});
```

这几乎是你要做的所有事情！压力没那么大，是吗？

感谢阅读！