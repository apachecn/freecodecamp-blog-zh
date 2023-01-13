# 如何使用 child_process.spawn 将 Python/Ruby/PHP shell 脚本与 Node.js 集成

> 原文：<https://www.freecodecamp.org/news/how-to-integrate-a-python-ruby-php-shell-script-with-node-js-using-child-process-spawn-e26ca3268a11/>

有时从 Node.js 运行 Python/Ruby/PHP shell 脚本是必要的。这篇文章着眼于利用 child_process.spawn 封装 Node.js/JavaScript.调用的最佳实践

这里的目标是在 Node.js 和外壳之间有一个互操作层。如果系统的其他部分不是用 JavaScript 开发的，这是一个快速的解决方法。

我们将使用`spawn`而不是`exec`,因为我们正在讨论传递数据和潜在的大量数据。要了解`child_process.spawn`和`child_process.exec`的区别，请参见“[node . js child _ process 的 spawn 和 exec 的区别](https://www.hacksparrow.com/difference-between-spawn-and-exec-of-node-js-child_process.html)”。

它的长与短用于使用缓冲接口的少量数据(200k 以下)的`exec`和使用流接口的大量数据的`spawn`。

`spawn`对我们将要看的一些用例有更详细的语法。它更适合与 Ruby/Python/PHP 集成，因为我们可能会得到比几行文本更多的数据。

完整的例子[github.com/HugoDF/node-run-python](https://github.com/HugoDF/node-run-python)。

以下示例包含两个部分:

*   实际运行 shell 命令的部分，通常是一个名为`run`的函数，以及
*   一个实际调用它的 IIFE("立即调用的函数表达式"，`(async () => { await run() }`)()。这种生活是由 Async/await(s[ee Async JS:history，patterns and gotc](https://codewithhugo.com/async-js/#async-await) has)支持的一种很好的模式，但它只是出于说明的目的，因为它表示从应用程序的另一部分对 wrap`ed sp`awn 调用的调用。

### 调用一个 shell 命令并记录下来

在这种情况下使用`spawn`是多余的，因为 echo 只会返回传递给它的内容。

这个例子非常简单明了，展示了如何使用`child_process.spawn`来“解释”和读回数据。

`spawn`将要调用的可执行文件作为第一个参数，并可选地将该可执行文件的选项/参数数组作为第二个参数。

```
const { spawn } = require('child_process');
function run() {
  const process = spawn('echo', ['foo']);
  process.stdout.on(
    'data',
    (data) => console.log(data.toString())
  );
}
(() => {
  try {
    run()
    // process.exit(0)
  } catch (e) {
    console.error(e.stack);
    process.exit(1);
  }
})();
```

### 调用 Python 获取其版本

我们将快速展示如何用 Python 做类似于上面的事情。再次注意`--version`是如何在数组内部传递的。

我们还创建了一个很好的记录器来区分 stdout 和 stderr，并绑定到它们。由于 spawn 返回一个具有`stdout`和`stderr`事件发射器的实例，我们可以使用`.on('data', () => { /* our callback function */` }，将`logOutput`函数绑定到`'data'`事件。

另一个有趣的花絮是`python` `--version`输出版本给`stderr`。关于*NIX 可执行文件是否在成功/错误时使用退出代码、stderr 和 stdout 的不一致是我们在将 Python/Ruby/other 与 Node.js 集成时必须记住的一个怪癖。

```
const { spawn } = require('child_process')
const logOutput = (name) => (data) => console.log(`[${name}] ${data.toString()}`)
function run() {
  const process = spawn('python', ['--version']);
process.stdout.on(
    'data',
    logOutput('stdout')
  );
process.stderr.on(
    'data',
    logOutput('stderr')
  );
}
(() => {
  try {
    run()
    process.exit(0)
  } catch (e) {
    console.error(e.stack);
    process.exit(1);
  }
})();
```

输出:

```
$ node run.js
```

```
[stderr] Python 2.7.13
```

### 从节点调用 Python 脚本

我们现在将运行一个完全成熟的 Python 脚本(尽管它也可以是 Ruby、PHP、shell 等)。)来自 Node.js。

这是`script.py`，它只是登出`argv`(“参数向量”，即。`['path/to/executable', /* command line arguments ]`

```
import sys
print(sys.argv)
```

就像前面的例子一样，我们将使用第二个参数中的 Python 脚本路径(`./script.py`)调用 spawn with `python`。

这是以这种方式集成脚本的另一个问题。在本例中，脚本的路径基于调用`node`的工作目录。

当然，有一个解决方法，使用`path`模块和`__dirname`，例如，可以使用`require('path').resolve(__dirname, './other-script.py')`解析与调用`spawn`的 JavaScript 文件/节点模块位于同一位置的`other-script.py`。

```
const { spawn } = require('child_process')
const logOutput = (name) => (data) => console.log(`[${name}] ${data.toString()}`)
function run() {
  const process = spawn('python', ['./script.py']);
process.stdout.on(
    'data',
    logOutput('stdout')
  );
process.stderr.on(
    'data',
    logOutput('stderr')
  );
}
(() => {
  try {
    run()
    // process.exit(0)
  } catch (e) {
    console.error(e.stack);
    process.exit(1);
  }
})();
```

输出:

```
node run.js
\[stdout\] ['./script.py']
```

### 使用 child_process.spawn 从 Node.js 向 Python 脚本传递参数

集成的下一步是能够将数据从节点/JavaScript 代码传递到 Python 脚本。

为此，我们将使用 arguments 数组(第二个参数给`spawn`)传递更多的 shell 参数。

```
const { spawn } = require('child_process')
const logOutput = (name) => (data) => console.log(`[${name}] ${data.toString()}`)
function run() {
  const process = spawn('python', ['./script.py', 'my', 'args']);
  process.stdout.on(
    'data',
    logOutput('stdout')
  );
  process.stderr.on(
    'data',
    logOutput('stderr')
  );
}
(() => {
  try {
    run()
    // process.exit(0)
  } catch (e) {
    console.error(e.stack);
    process.exit(1);
  }
})();
```

除了第一个元素(它是脚本的路径)之外，我们的`script.py`也将注销`argv`。

```
import sys
print(sys.argv)[1:]
```

以下是输出结果:

```
node run.js
\[stdout\] ['my', 'args']
```

### 从 Node.js 读取 child_process.spawn 输出

能够将数据传递给 Python 脚本是件好事。我们仍然无法从 Python 脚本中以一种我们能够在 Node.js/JavaScript 应用程序中利用的格式取回数据。

对此的解决方案是将整个`spawn`调用函数包装成一个承诺。这让我们可以决定什么时候去`resolve`或者`reject`。

为了跟踪 Python 脚本的输出流，我们使用数组手动缓冲输出(一个用于`stdout`，另一个用于`stderr`)。

我们还使用`spawn().on('exit', (code, signal) => { /* probably call resolve() */` })为`'exit'`添加了一个监听器。这是我们将从 Python/Ruby/其他脚本中检查承诺值的地方`to reso`。

```
const { spawn } = require('child_process')
const logOutput = (name) => (data) => console.log(`[${name}] ${data}`)
function run() {
  return new Promise((resolve, reject) => {
    const process = spawn('python', ['./script.py', 'my', 'args']);
    const out = []
    process.stdout.on(
      'data',
      (data) => {
        out.push(data.toString());
        logOutput('stdout')(data);
      }
    );
    const err = []
    process.stderr.on(
      'data',
      (data) => {
        err.push(data.toString());
        logOutput('stderr')(data);
      }
    );
    process.on('exit', (code, signal) => {
      logOutput('exit')(`${code} (${signal})`)
      resolve(out);
    });
  });
}
(async () => {
  try {
    const output = await run()
    logOutput('main')(output)
    process.exit(0)
  } catch (e) {
    console.error(e.stack);
    process.exit(1);
  }
})();
```

输出:

```
node run.js
\[stdout\] ['my', 'args']
\[main\] ['my', 'args']
```

### 处理来自 child_process.spawn 的错误

接下来，我们需要在 Node.js/JavaScript 级别处理来自 Python/Ruby/shell 脚本的错误。

一个*NIX 可执行文件发出出错信号的主要方式是使用一个`1`退出代码。这就是为什么`.on('exit'`处理程序现在在决定用值来解决还是拒绝之前，会对`code === 0`进行检查。

```
const { spawn } = require('child_process')
const logOutput = (name) => (data) => console.log(`[${name}] ${data}`)
function run() {
  return new Promise((resolve, reject) => {
    const process = spawn('python', ['./script.py', 'my', 'args']);
const out = []
    process.stdout.on(
      'data',
      (data) => {
        out.push(data.toString());
        logOutput('stdout')(data);
      }
    );
const err = []
    process.stderr.on(
      'data',
      (data) => {
        err.push(data.toString());
        logOutput('stderr')(data);
      }
    );
process.on('exit', (code, signal) => {
      logOutput('exit')(`${code} (${signal})`)
      if (code === 0) {
        resolve(out);
      } else {
        reject(new Error(err.join('\n')))
      }
    });
  });
}
(async () => {
  try {
    const output = await run()
    logOutput('main')(output)
    process.exit(0)
  } catch (e) {
    console.error('Error during script execution ', e.stack);
    process.exit(1);
  }
})();
```

输出:

```
node run.js
[stderr] Traceback (most recent call last):
    File "./script.py", line 3, in <module>
    print(sy.argv)[1:]
NameError: name 'sy' is not defined
Error during script execution Error: Traceback (most recent call last):
    File "./script.py", line 3, in <module>
    print(sy.argv)[1:]
NameError: name 'sy' is not defined
at ChildProcess.process.on (/app/run.js:33:16)
    at ChildProcess.emit (events.js:182:13)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:240:12)
```

### 将结构化数据从 Python/Ruby 传递到 Node.js/JavaScript

Ruby/Python/PHP/shell 脚本和我们的 Node.js/JavaScript 应用层之间完全集成的最后一步是能够将结构化数据从脚本向上传递回 Node.js/JavaScript.

在 Python/Ruby/PHP 和 Node.js/JavaScript 中都可用的最简单的结构化数据格式是 JSON。

在 Python 脚本中，我们打印字典的`json.dumps()`输出，参见`script.py`:

```
import sys
import json
send_message_back = {
  'arguments': sys.argv[1:],
  'message': """Hello,
This is my message.
To the world"""
}
print(json.dumps(send_message_back))
```

在 Node 中，我们在`'exit'`处理程序中添加了一些 JSON 解析逻辑(使用`JSON.parse`)。

这里有一个问题，例如，如果`JSON.parse()`由于 JSON 格式错误而失败，我们需要传播这个错误。因此，try/catch 的`catch`子句`reject` -s 潜在的错误:`try { resolve(JSON.parse(out[0])) } catch(e) { reject(e) }`。

```
const { spawn } = require('child_process')
const logOutput = (name) => (message) => console.log(`[${name}] ${message}`)
function run() {
  return new Promise((resolve, reject) => {
    const process = spawn('python', ['./script.py', 'my', 'args']);
    const out = []
    process.stdout.on(
      'data',
      (data) => {
        out.push(data.toString());
        logOutput('stdout')(data);
      }
    );
    const err = []
    process.stderr.on(
      'data',
      (data) => {
        err.push(data.toString());
        logOutput('stderr')(data);
      }
    );
   process.on('exit', (code, signal) => {
      logOutput('exit')(`${code} (${signal})`)
      if (code !== 0) {
        reject(new Error(err.join('\n')))
        return
      }
      try {
        resolve(JSON.parse(out[0]));
      } catch(e) {
        reject(e);
      }
    });
  });
}
(async () => {
  try {
    const output = await run()
    logOutput('main')(output.message)
    process.exit(0)
  } catch (e) {
    console.error('Error during script execution ', e.stack);
    process.exit(1);
  }
})();
```

输出:

```
node run.js
[stdout] {"message": "Hello,\nThis is my message.\n\nTo the world", "arguments": ["my", "args"]}
[main] Hello,
This is my message.
To the world
```

就是这样！感谢阅读:)

我在[https://mentorcruise.com/mentor/HugoDiFrancesco/](https://mentorcruise.com/mentor/HugoDiFrancesco/)有辅导名额。如果你想让 Node.js/JavaScript/career 指导你，或者随时给我发推特[@雨果 __df](https://twitter.com/hugo__df)

多读一些我写的关于 codewithhugo.com 的文章