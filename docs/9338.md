# 用 NodeJS 编写命令行应用程序

> 原文：<https://www.freecodecamp.org/news/writing-command-line-applications-in-nodejs-2cf8327eee2/>

彼得·本杰明

# 用 NodeJS 编写命令行应用程序

![1*90gS34fsYQD1Up9L6KHwmw](img/f0c3b0f4639b9a8c7dbd8230fc60e735.png)

使用正确的包，用 NodeJS 编写命令行应用程序轻而易举。

其中一个包让事情变得异常简单: [*指挥官*](https://www.npmjs.com/package/commander) *。*

让我们做好准备，并演示如何使用 Commander 在 NodeJS 中编写命令行界面(CLI)应用程序。我们的目标是编写一个 CLI 应用程序来列出文件和目录。

#### 设置

T2 我喜欢网络游戏。当涉及到开发环境设置时，它们抽象掉了许多令人头痛的问题。我个人使用 [Cloud9](http://c9.io) 的原因如下:

*   布局很直观
*   编辑器很漂亮并且易于使用
*   自由层资源最近已经增加到 1GB RAM 和 5GB 磁盘空间，这对一个相当大的 NodeJS 应用程序来说已经足够了。
*   不限数量的工作站
*   这是一个测试或试验新项目/想法的完美环境，不用担心会破坏你的环境

**Node/NPM 版**
写这篇文章的时候，Node 是 5.3.0 版本，NPM 是 ad 3 . 3 . 12 版本。

#### 初始化

我们首先初始化我们的项目，接受所有 NPM 默认设置，并安装 *commander* 包:

```
npm initnpm i --save commander
```

导致:

**注:**

*   您必须手动添加 ***bin*** ，这将告诉 NodeJS 您的 CLI 应用程序的名称以及应用程序的入口点。
*   确保不要使用系统中已经存在的命令名。

#### 索引. js

现在我们已经初始化了我们的项目，并指出我们的入口点是 index.js，让我们创建 index.js:

```
touch index.js
```

现在，对于实际的编码部分:

通常，当执行 NodeJS 文件时，我们通过在文件前加上前缀 *node* 来告诉系统使用合适的解释器。然而，我们希望能够从系统中的任何地方全局执行我们的 CLI 应用程序，而不必每次都指定节点解释器。

因此，我们的第一行是 [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) 表达式:

```
#!/usr/bin/env node
```

这不仅告诉我们的系统使用合适的解释器，还告诉我们的系统使用合适的解释器的*版本*。

从现在开始，我们编写纯 JavaScript 代码。由于我将编写符合 ES6 的代码，我将从字面表达式开始:

```
'use strict';
```

这告诉编译器使用更严格的 javascript 版本[ [1](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) ]，并使我们能够在 Cloud9 上编写 ES6 代码。

让我们从要求*指挥官*打包开始:

```
const program = require('commander');
```

现在，用 *commander* 编写 CLI 应用程序是简单的，文档也很棒，但是我纠结于一些概念，我将在这里尝试澄清一下。

CLI 应用程序似乎有两种设计。以 ***ls*** 和 ***git*** 为例。

用 ***ls*** ，你传递一个或多个标志:

*   ***ls -l***
*   ***ls -al***

用 ***git*** ，你传递子命令，但是你也有一些标志:

*   ***git commit-am<messa*ge>**
*   ***git 远程添加原点<repo-u*rl>**

我们将探索 Commander 赋予我们设计这两种命令行界面的灵活性。

#### 指挥官 API

*Commander* API 很简单，文档也很棒。

我们可以用三种基本方式编写程序:

**方法#1:仅标记命令行应用**

```
const program = require('commander');
```

```
program  .version('0.0.1')  .option('-o, --option','option description')  .option('-m, --more','we can have as many options as we want')  .option('-i, --input [optional]','optional user input')  .option('-I, --another-input <required>','required user input')  .parse(process.argv); // end with parse to parse through the input
```

**注:**

*   短指针和长指针选项在同一个字符串中(见上图中的粗体文本)
*   当用户传递时， **-o** 和 **-m** 将返回**布尔**值，因为我们没有指定任何*可选的*或*必需的*用户输入。
*   **-i** 和 **-I** 将捕获用户输入并将值传递给我们的 CLI 应用程序。
*   方括号中的任何值(例如[ ])都被认为是可选的。用户可能会也可能不会提供值。
*   尖括号中的任何值(如< >)都被认为是必需的。用户必须提供一个值。

上面的例子允许我们对我们的 CLI 应用程序实现一个仅标记的方法。用户应该像这样与我们的应用程序进行交互:

```
//Examples:$ cli-app -om -I hello$ cli-app --option -i optionalValue -I requiredValue
```

**方法#2:子命令和基于标志的命令行应用**

```
const program = require('commander');
```

```
program  .version('0.0.1')  .command('command <req> [optional]')  .description('command description')  .option('-o, --option','we can still have add'l options')  .action(function(req,optional){    console.log('.action() allows us to implement the command');    console.log('User passed %s', req);    if (optional) {      optional.forEach(function(opt){        console.log("User passed optional arguments: %s", opt);      });    }  });
```

```
program.parse(process.argv); // notice that we have to parse in a new statement.
```

**注:**

*   如果我们利用**。命令('命令…')。描述('描述…')** ，我们必须利用**。action()** 传递一个函数，并根据用户的输入执行我们的代码。(我指出这一点是因为有一种替代方法可以利用**。command()** 这是我们接下来要探讨的。)
*   如果我们利用**。命令(' command…')** ，我们不能再随便钉上**。像我们在前面的例子中所做的那样，在最后解析(process.argv)** 。我们必须在新语句中传递 **parse()**

用户应该像这样与我们的 CLI 应用程序进行交互:

```
//Example: $ cli-app command requiredValue -o
```

**方法#3:与上面的方法#2 相同，但是允许模块化代码**

最后，我们不必用所有的**来膨胀我们的 JavaScript 文件。命令()。描述()。动作()**逻辑。我们可以像这样模块化我们的 CLI 项目:

```
// file: ./cli-app/index.jsconst program = require('commander');
```

```
program.version('0.0.1').command('command <req> [optional]','command description').command('command2','command2 description').command('command3','command3 description').parse(process.argv);
```

**注:**

*   如果我们利用**。command('command '，' description')** 为了传递命令和描述，我们不能再有**。动作()。** *指挥官*将暗示我们有单独的文件，有特定的命名约定，在那里我们可以处理每个命令。命名约定为 **index-command.js** 、 **index-command2.js** 、 **index-command3.js** 。[参见 Github](https://github.com/tj/commander.js/tree/master/examples) 上的例子(具体来说: **pm** 、**pm-安装**、**pm-发布**文件)。
*   如果我们走这条路，我们可以直接加上**。parse()** 结尾。

#### 回到我们的项目场景…

既然我们已经介绍了基础知识，接下来就容易多了。我们可以深呼吸。

***叹息***

好了，好戏开始了。

回想一下我们的项目场景，我们想要编写一个 CLI 应用程序来列出文件和目录。所以让我们开始写代码吧。

我们希望让用户能够决定他们是否希望看到“所有”文件(包括隐藏的文件)和/或他们是否希望看到长列表格式(包括文件/文件夹的权限/许可)。

因此，我们从编写程序的基本外壳开始，以查看我们的增量进度(为了演示起见，我们将遵循**方法#2** 的签名) :

```
#!/usr/bin/env node'use strict';
```

```
const program = require('commander');
```

```
program  .version('')  .command('')  .description('')  .option('','')  .option('','')  .action('');
```

```
program.parse(process.argv);
```

让我们开始填补空白:

```
#!/usr/bin/env node'use strict';
```

```
const program = require('commander');
```

```
program  .version('0.0.1')  .command('list [directory]')  .description('List files and folders')  .option('-a, --all','List all files and folders')  .option('-l, --long','')  .action();
```

```
program.parse(process.argv);
```

**注:**

*   我们决定将我们的命令命名为 ***列表*** 。
*   **目录**参数是可选的，所以用户可以简单地忽略传递一个目录，在这种情况下我们将列出当前目录中的文件/文件夹。

因此，在我们的场景中，以下调用是有效的:

```
$ cli-app list $ cli-app list -al$ cli-app list --all$ cli-app list --long$ cli-app list /home/user -al
```

现在，让我们开始为我们的**编写代码。动作()**。

```
#!/usr/bin/env node'use strict';
```

```
const program = require('commander');
```

```
let listFunction = (directory,options) => {  //some code here}
```

```
program  .version('0.0.1')  ...  .action(listFunction);
```

```
program.parse(process.argv);
```

我们将在这里使用内置的 ***ls*** 命令来作弊，该命令在所有类 unix 操作系统中都可用。

```
#!/usr/bin/env node'use strict';
```

```
const program = require('commander'),      exec = require('child_process').exec;
```

```
let listFunction = (directory,options) => {const cmd = 'ls';let params = [];if (options.all) params.push('a');if (options.long) params.push('l');let fullCommand = params.length                   ? cmd + ' -' + params.join('')                  : cmdif (directory) fullCommand += ' ' + directory;
```

```
};
```

```
program  .version('0.0.1')  ...  .action(listFunction);
```

```
program.parse(process.argv);
```

让我们来谈谈这个代码的原因。

1.  首先，我们要求 ***子进程*** 库执行 shell 命令 ***** ( *** *这带来了很大的安全风险，我将在后面讨论*** )
2.  我们声明一个常量变量来保存我们命令的根
3.  我们声明一个数组来保存用户传递的任何参数(例如， ***-a*** ， ***-l*** )
4.  我们检查用户是否传递了简写( **-a** )或长写(——**all**)标志。如果是，那么 **options.all** 和/或 **options.long** 将评估为 ***true*** ，在这种情况下，我们将把各自的命令标志推送到我们的数组中。我们的目标是构建 shell 命令，稍后我们将把它传递给 **child_process** 。
5.  我们声明一个新变量来保存完整的 shell 命令。如果 param 数组包含任何标志，我们将这些标志相互连接并连接到根命令。否则，如果 param 数组为空，那么我们使用 root 命令。
6.  最后，我们检查用户是否传递了任何可选的**目录**值。如果是这样，我们将它连接到完整构造的命令。

现在，我们想在 shell 中执行完整构造的命令。 **Child_Process.exec()** 给了我们这样做的能力，而 [NodeJS docs](https://nodejs.org/api/child_process.html#child_process_child_process_exec_command_options_callback) 给了我们签名:

```
child_process.exec(command, callback(error, stdout, stderr){  //"error" will be returned if exec encountered an error.  //"stdout" will be returned if exec is successful and data is returned.  //"stderr" will be returned if the shell command encountered an error.})
```

所以，让我们用这个:

```
#!/usr/bin/env node'use strict';
```

```
const program = require('commander'),      exec = require('child_process').exec;
```

```
let listFunction = (directory,options) => {  const cmd = 'ls';  let params = [];  if (options.all) params.push('a');  if (options.long) params.push('l');  let fullCommand = params.length                   ? cmd + ' -' + params.join('')                  : cmd  if (directory) fullCommand += ' ' + directory;
```

```
 let execCallback = (error, stdout, stderr) => {    if (error) console.log("exec error: " + error);    if (stdout) console.log("Result: " + stdout);    if (stderr) console.log("shell error: " + stderr);  };
```

```
 exec(fullCommand, execCallback);
```

```
};
```

```
program  .version('0.0.1')  ...  .action(listFunction);
```

```
program.parse(process.argv);
```

就是这样！

下面是我的示例 CLI 应用程序的要点。

当然，我们可以添加一些细节，比如:

*   着色输出(下面我用的是*粉笔库)*
*   *现代的 CLI 应用程序足够智能，当用户调用程序而没有参数或自变量时，会显示帮助文本(很像*)所以我在最底部添加了这个功能。**

```
**`#!/usr/bin/env node'use strict';/** * Require dependencies * */const program = require('commander'),    chalk = require("chalk"),    exec = require('child_process').exec,    pkg = require('./package.json');/** * list function definition * */let list = (directory,options)  => {    const cmd = 'ls';    let params = [];        if (options.all) params.push("a");    if (options.long) params.push("l");    let parameterizedCommand = params.length                                 ? cmd + ' -' + params.join('')                                 : cmd ;    if (directory) parameterizedCommand += ' ' + directory ;        let output = (error, stdout, stderr) => {        if (error) console.log(chalk.red.bold.underline("exec error:") + error);        if (stdout) console.log(chalk.green.bold.underline("Result:") + stdout);        if (stderr) console.log(chalk.red("Error: ") + stderr);    };        exec(parameterizedCommand,output);    };program    .version(pkg.version)    .command('list [directory]')    .option('-a, --all', 'List all')    .option('-l, --long','Long list format')    .action(list);program.parse(process.argv);// if program was called with no arguments, show help.if (program.args.length === 0) program.help();`**
```

**最后，我们可以利用 NPM 来符号链接我们的 CLI 应用程序，这样我们就可以在我们的系统中全局使用它。简单地说，在终端中， *cd* 进入我们的 CLI 应用程序的根目录，并键入:**

```
**`npm link`**
```

### **最后的想法和考虑**

**这个项目中的代码绝不是最好的代码。我充分意识到总有改进的空间，所以欢迎反馈！**

**此外，我想指出我们的应用程序中的一个安全缺陷。我们的代码不 ***净化*** 或 ***验证*** 用户的输入。这违反了安全最佳实践。考虑以下情况，用户可能会传递不需要的输入:**

```
**`$ cli-app -al ; rm -rf /$ cli-app -al ; :(){ :|: & };:`**
```

**如果您想编写一些代码来处理这个问题，或者修复任何其他潜在的问题，请务必留下评论，向我们展示您的代码。**