# 如何使用 Node 创建真实的 Node CLI 应用程序

> 原文：<https://www.freecodecamp.org/news/how-to-create-a-real-world-node-cli-app-with-node-391b727bbed3/>

木材。木卫一

# 如何使用 Node 创建真实的 Node CLI 应用程序

![9ZjKi9cAzlM0d4FWrOmGLwpdnAyG2hCKpvlT](img/ee2dc2bf9d48b38411fdfabcf44d88ea.png)

命令行是一种用户界面，在 JavaScript 开发领域没有得到足够的重视。事实上，大多数开发工具都应该有一个供我们这样的书呆子使用的 CLI，用户体验应该与您精心创建的 web 应用程序相当。这包括漂亮的设计、有用的菜单、清晰的错误信息和输出、加载指示器和进度条等等。

当谈到用 Node 构建命令行界面时，现实世界中的教程并不多，因此这是超越基本的“hello world”CLI 应用程序的系列教程中的第一篇。我们将创建一个名为`outside-cli`的应用程序，它将为您提供任何地点的当前天气和 10 天预报。

![U7W2bAC42fKLJFVZ4YDEcjOfhEGR-68myb0T](img/fb0256c98ddb88f6ca62424b4e4873e3.png)

**注意**:有几个库可以帮助创建复杂的 CLI，例如 [oclif](https://github.com/oclif/oclif) 、 [yargs](https://github.com/yargs/yargs) 和 [commander](https://github.com/tj/commander.js) ，但是为了这个例子，我们将保持我们的依赖关系精简，这样你可以更好地理解事情是如何在幕后工作的。本教程假设您已经掌握了 JavaScript 和 Node 的基本知识。

### 入门指南

与所有 JavaScript 项目一样，创建 package.json 和一个条目文件是开始工作的最佳方式。我们可以保持简单——还不需要依赖。

```
{
  "name": "outside-cli",
  "version": "1.0.0",
  "license": "MIT",
  "scripts": {},
  "devDependencies": {},
  "dependencies": {}
}
```

package.json

```
module.exports = () => {
  console.log('Welcome to the outside!')
}
```

index.js

我们需要一种方法来调用我们新创建的应用程序，显示欢迎消息，并将其添加到系统路径中，以便可以从任何地方调用它。bin 文件是实现这一点的方法。

```
#!/usr/bin/env node
require('../')()
```

bin/outside

以前没见过`#!/usr/bin/env node`？它被称为 T2。它基本上告诉系统这不是一个 shell 脚本，它应该使用不同的解释器。

保持二进制文件的长度很重要，因为它的唯一目的是调用应用程序。我们所有的代码都应该存在于二进制文件之外，这样它就可以保持模块化和可测试性。如果我们将来想提供对我们库的编程访问，这也会有所帮助。

为了直接运行 bin 文件，我们需要给它正确的文件系统权限。如果你在 UNIX 上，这就像运行`chmod +x bin/outside`一样简单。如果你在 Windows 上，帮自己一个忙，使用 Linux 子系统。

接下来，我们将把二进制文件添加到 package.json 文件中。当用户把我们的包作为一个全局包安装时，这将自动把它放到用户的系统路径上(`npm install -g outside-cli`)。

```
{
  "name": "outside-cli",
  "version": "1.0.0",
  "license": "MIT",
  "bin": {
    "outside": "bin/outside"
  },
  "scripts": {},
  "devDependencies": {},
  "dependencies": {}
}
```

package.json

我们现在可以通过运行`./bin/outside`直接调用我们的 bin 文件。您应该会看到欢迎信息。在项目的根目录下运行`npm link`会将你的二进制文件符号链接到系统路径，通过运行`outside`可以从任何地方访问它。

运行 CLI 应用程序时，它由参数和命令组成。参数(或“标志”)是一个或两个连字符(如`-d`、`--debug`或`--env production`)的值，用于将选项传递给我们的应用程序。命令是所有其他没有标志的值。

与命令不同，参数不需要以任何特定的顺序指定。例如，我们可以运行`outside today Brooklyn`并假设第二个命令总是这个位置——但是如果我们将来想要添加更多的选项，运行`outside today --location Brooklyn`不是更好吗？

为了让我们的应用程序有用，我们需要解析这些命令和参数，并将它们转换成一个对象。我们可以随时跳入`process.argv`并尝试自己做，但是让我们安装我们的第一个依赖项[minimit](https://github.com/substack/minimist)来为我们处理这个问题。

```
npm install --save minimist
```

```
const minimist = require('minimist')

module.exports = () => {
  const args = minimist(process.argv.slice(2))
  console.log(args)
}
```

index.js

**注意**:我们用`.slice(2)`去掉前两个参数的原因是因为第一个参数总是解释器，后面跟着被解释的文件名。我们只关心之后的争论。

现在运行的`outside today`应该输出`{ _: ['today'] }`。如果你运行`outside today --location "Brooklyn, NY"`，它应该输出`{ _: ['today'], location: 'Brooklyn, NY' }`。当我们实际使用这个位置时，我们将更深入地讨论参数，但是现在这已经足够设置我们的第一个命令了。

### 参数语法

为了更好地理解参数语法是如何工作的，[你可以阅读这个](https://www.gnu.org/software/libc/manual/html_node/Argument-Syntax.html)。基本上，一个标志可以是单连字符或双连字符，它将接受命令中紧随其后的值，或者当没有值时等于 true。单连字符标志也可以为简写布尔值组合使用(`-a -b -c`或`-abc`会给出`{ a: true, b: true, c: true }`)。)

重要的是要记住，如果**值包含特殊字符或空格**，则必须加上引号。运行`--foo bar baz`会得到`{ : ['baz']，foo:' bar ' }`, but running`-foo " bar baz "`would give you`{ foo:' bar baz ' } `。_

最好将每个命令的代码分开，只在调用时才加载到内存中。这将加快启动速度，并防止加载不必要的模块。通过 minimist 给我们的主命令上的 switch 语句就足够简单了。使用这种设置，每个命令文件应该导出一个函数，在这种情况下，我们将参数传递给每个命令，以便以后使用。

```
const minimist = require('minimist')

module.exports = () => {
  const args = minimist(process.argv.slice(2))
  const cmd = args._[0]

  switch (cmd) {
    case 'today':
      require('./cmds/today')(args)
      break
    default:
      console.error(`"${cmd}" is not a valid command!`)
      break
  }
}
```

index.js

```
module.exports = (args) => {
  console.log('today is sunny')
}
```

cmds/today.js

现在如果你运行`outside today`，你会看到消息“今天是晴天”，如果你运行`outside foobar`，它会告诉你“foobar”不是一个有效的命令。我们仍然需要查询天气 API 来获得真实数据，但这是一个好的开始。

每个 CLI 中都应该有一些命令和参数:`help`、`--help`和`-h`，它们应该显示帮助菜单，还有`version`、`--version`和`-v`，它们应该输出当前的应用程序版本。如果没有指定命令，我们也应该默认一个主帮助菜单。

在我们当前的设置中，通过向 switch 语句添加两个 cases，为变量`cmd`添加一个默认值，并为帮助和版本参数标志实现一些 if 语句，可以很容易地实现这一点。Minimist 自动将参数解析为键/值，因此运行`outside --version`将使`args.version`等于真。

```
const minimist = require('minimist')

module.exports = () => {
  const args = minimist(process.argv.slice(2))

  let cmd = args._[0] || 'help'

  if (args.version || args.v) {
    cmd = 'version'
  }

  if (args.help || args.h) {
    cmd = 'help'
  }

  switch (cmd) {
    case 'today':
      require('./cmds/today')(args)
      break

    case 'version':
      require('./cmds/version')(args)
      break

    case 'help':
      require('./cmds/help')(args)
      break

    default:
      console.error(`"${cmd}" is not a valid command!`)
      break
  }
}
```

要实现我们的新命令，请遵循与`today`命令相同的格式。

```
const { version } = require('../package.json')

module.exports = (args) => {
  console.log(`v${version}`)
}
```

cmds/version.js

```
const menus = {
  main: `
    outside [command] <options>

    today .............. show weather for today
    version ............ show package version
    help ............... show help menu for a command`,

  today: `
    outside today <options>

    --location, -l ..... the location to use`,
}

module.exports = (args) => {
  const subCmd = args._[0] === 'help'
    ? args._[1]
    : args._[0]

  console.log(menus[subCmd] || menus.main)
}
```

cmds/help.js

现在，如果你运行`outside help today`或`outside today -h`，你应该会看到`today`命令的帮助菜单。运行`outside`或`outside -h`会显示主帮助菜单。

![G1BNtKcKV5D1dMaVgKfclg7mZ1IYF8WXps9w](img/2ba412d0a3ecc2686c1a99d3eed60c9f.png)

这个项目设置真的很棒，因为如果你需要添加一个新命令，你需要做的就是在`cmds`文件夹中创建一个新文件，将它添加到 switch 语句中，如果有帮助菜单的话，添加一个帮助菜单。

```
module.exports = (args) => {
  console.log('tomorrow is rainy')
}
```

cmds/forecast.js

```
// ...
    case 'forecast':
      require('./cmds/forecast')(args)
      break
// ...
```

index.js

```
const menus = {
  main: `
    outside [command] <options>

    today .............. show weather for today
    forecast ........... show 10-day weather forecast
    version ............ show package version
    help ............... show help menu for a command`,

  today: `
    outside today <options>

    --location, -l ..... the location to use`,

  forecast: `
    outside forecast <options>

    --location, -l ..... the location to use`,
}

// ...
```

cmds/help.js

有时一个命令可能需要很长时间才能运行。如果您从 API 获取数据、生成内容、将文件写入磁盘或任何其他需要几毫秒以上的过程，您希望给用户一些反馈，表明您的应用程序没有冻结，只是在努力工作。有时你可以测量你的操作进度，显示一个进度条是有意义的，但是其他时候更多的是可变的，显示一个装载指示器是有意义的。

对于我们的应用程序，我们不能测量我们的 API 请求的进度，所以我们将使用一个基本的微调器来显示正在发生的事情。为我们的网络请求和微调器再安装两个依赖项:

```
npm install --save axios ora
```

### 从 API 获取数据

现在让我们创建一个实用程序，向 Yahoo weather API 请求一个位置的当前条件和预测。

**注意**:雅虎 API 使用“YQL”语法，有点古怪——不要试图去理解，只是复制粘贴。这是我能找到的唯一一个不需要 API 键的天气 API。

```
const axios = require('axios')

module.exports = async (location) => {
  const results = await axios({
    method: 'get',
    url: 'https://query.yahooapis.com/v1/public/yql',
    params: {
      format: 'json',
      q: `select item from weather.forecast where woeid in
        (select woeid from geo.places(1) where text="${location}")`,
    },
  })

  return results.data.query.results.channel.item
}
```

utils/weather.js

```
const ora = require('ora')
const getWeather = require('../utils/weather')

module.exports = async (args) => {
  const spinner = ora().start()

  try {
    const location = args.location || args.l
    const weather = await getWeather(location)

    spinner.stop()

    console.log(`Current conditions in ${location}:`)
    console.log(`\t${weather.condition.temp}° ${weather.condition.text}`)
  } catch (err) {
    spinner.stop()

    console.error(err)
  }
}
```

cmds/today.js

现在如果你运行`outside today --location "Brooklyn, NY"`，你会看到一个快速的 spinner 发出请求，然后是当前的天气状况。

由于请求发生得如此之快，很难看到加载指示器。如果你想手动减慢它的速度，你可以把这一行加到你的天气工具函数的开头:`await new Promise(resolve => setTimeout(resolve, 500` 0)。

![0eoDghnUZOOvNti5wf3puX9oyxvK4RMjYG10](img/5ddda2286df2b5ca944c4e4106910e60.png)

太好了！现在让我们将代码复制到我们的`forecast`命令中，并稍微改变一下格式。

```
const ora = require('ora')
const getWeather = require('../utils/weather')

module.exports = async (args) => {
  const spinner = ora().start()

  try {
    const location = args.location || args.l
    const weather = await getWeather(location)

    spinner.stop()

    console.log(`Forecast for ${location}:`)
    weather.forecast.forEach(item =>
      console.log(`\t${item.date} - Low: ${item.low}° | High: ${item.high}° | ${item.text}`))
  } catch (err) {
    spinner.stop()

    console.error(err)
  }
}
```

cmds/forecast.js

你现在可以在运行`outside forecast --location "Brooklyn, NY"`时看到 10 天的天气预报。看起来不错！让我们再添加一个实用程序，如果命令中没有指定位置，它将根据我们的 IP 地址自动获取我们的位置。

```
const axios = require('axios')

module.exports = async () => {
  const results = await axios({
    method: 'get',
    url: 'https://api.ipdata.co',
  })

  const { city, region } = results.data
  return `${city}, ${region}`
}
```

utils/location.js

```
// ...
const getLocation = require('../utils/location')

module.exports = async (args) => {
  // ...
    const location = args.location || args.l || await getLocation()
    const weather = await getWeather(location)
  // ...
}
```

cmds/today.js & cmds/forecast.js

现在，如果你只是在没有位置的情况下运行`outside forecast`，你将会看到你当前位置的预测。

![oXE-zFoeG9LjeDVfp-n7vhSs3KZuC3tue3-m](img/f89fa9d395583dbacd1ae1234dcadc90.png)

### 错误处理

我没有深入讨论如何最好地处理错误的细节(这将在后面的教程中介绍)，但是要记住的最重要的事情是使用正确的退出代码。

如果您的 CLI 出现严重错误，您应该使用`process.exit(1)`退出。这让终端知道程序没有干净地退出，例如，这将从 CI 服务通知您。

让我们创建一个快速实用程序来完成这项工作，这样我们就可以在运行一个不存在的命令时获得正确的退出代码。

```
module.exports = (message, exit) => {
  console.error(message)
  exit && process.exit(1)
}
```

utils/error.js

```
// ...
const error = require('./utils/error')

module.exports = () => {
  // ...
    default:
      error(`"${cmd}" is not a valid command!`, true)
      break
  // ...
}
```

*index.js*

### 收尾工作

将我们的库发布出去的最后一步是将它发布给一个包管理器。因为我们的应用程序是用 JavaScript 编写的，所以发布到 NPM 是有意义的。让我们更详细地填写一下我们的`package.json`:

```
{
  "name": "outside-cli",
  "version": "1.0.0",
  "description": "A CLI app that gives you the weather forecast",
  "license": "MIT",
  "homepage": "https://github.com/timberio/outside-cli#readme",
  "repository": {
    "type": "git",
    "url": "git+https://github.com/timberio/outside-cli.git"
  },
  "engines": {
    "node": ">=8"
  },
  "keywords": [
    "weather",
    "forecast",
    "rain"
  ],
  "preferGlobal": true,
  "bin": {
    "outside": "bin/outside"
  },
  "scripts": {},
  "devDependencies": {},
  "dependencies": {
    "axios": "^0.18.0",
    "minimist": "^1.2.0",
    "ora": "^2.0.0"
  }
}
```

*   设置`engine`将确保任何安装我们应用程序的人都有 Node 的更新版本。由于我们使用的是异步/等待语法而没有编译，所以我们需要 Node 8.0 或更高版本。
*   如果用`npm install --save`而不是`npm install --global`安装，设置`preferGlobal`将警告用户。

就是这样！您现在可以运行`npm publish`，您的应用将可供下载。如果你想更进一步，在其他包管理器(如家酿)上发布，你可以看看 [pkg](https://github.com/zeit/pkg) 或 [nexe](https://github.com/nexe/nexe) ，它们可以帮助你将应用捆绑成一个独立的二进制文件。

### 摘要

这是我们在 [Timber](https://timber.io/) 的所有 CLI 应用程序遵循的结构，它有助于保持事物的组织性和模块化。

对于那些只浏览过本教程的人来说，本教程中的一些关键内容:

*   Bin 文件是任何 CLI 应用程序的入口点，应该只调用 main 函数
*   除非需要，否则不应该需要命令文件
*   始终包括`help`和`version`命令
*   保持命令文件简洁——它们的主要目的是调用函数和显示用户消息
*   总是显示某种活动指示器
*   使用正确的错误代码退出

我希望您现在对如何在 Node 中创建和组织 CLI 应用程序有了更好的理解。这是一系列教程的第一部分，所以稍后我们会更深入地讨论添加设计、ascii 艺术和颜色、接受用户输入、编写集成测试等等。你可以在 GitHub 上看到我们今天[写的所有源代码。](https://github.com/timberio/outside-cli)

*我们是一家基于云计算的伐木公司@ [木材](http://timber.io)。如果您试用我们的产品，我们会很高兴(它真的很棒！—您可以在这里创建一个免费帐户[，但这是我们要宣传我们产品的全部内容……你们来这里是为了了解如何在 Node 中构建 CLI 应用程序，希望本指南能帮助你们入门。](http://timber.io)*

*最初发表于 [timber.io](https://timber.io/blog/creating-a-real-world-cli-app-with-node) 。*