# 解释了节点流程对象

> 原文：<https://www.freecodecamp.org/news/node-process-object-explained/>

Node.js 中的`process`对象是一个全局对象，可以在任何模块内部访问，而不需要它。Node.js 中提供的全局对象或属性非常少，`process`就是其中之一。它是 Node.js 生态系统中的一个重要组件，因为它提供了关于程序运行时的各种信息集。

为了探究这个问题，我们将使用它的一个名为`process.versions`的属性。这个属性告诉我们关于我们已经安装的 Node.js 版本的信息。它必须与`-p`标志一起使用。

```
$ node  -p "process.versions"

# output
{ http_parser: '2.8.0',
  node: '8.11.2',
  v8: '6.2.414.54',
  uv: '1.19.1',
  zlib: '1.2.11',
  ares: '1.10.1-DEV',
  modules: '57',
  nghttp2: '1.29.0',
  napi: '3',
  openssl: '1.0.2o',
  icu: '60.1',
  unicode: '10.0',
  cldr: '32.0',
  tz: '2017c' }
```

您可以检查的另一个属性是`process.release`，它与我们在安装 Node.js 时使用的命令`$ node --version`相同。但是这次的输出会更详细。

```
node -p "process.release"

# output
{ name: 'node',
  lts: 'Carbon',
  sourceUrl: 'https://nodejs.org/download/release/v8.11.2/node-v8.11.2.tar.gz',
  headersUrl: 'https://nodejs.org/download/release/v8.11.2/node-v8.11.2-headers.tar.gz' }
```

这些是一些不同的命令，我们可以在命令行中使用它们来访问其他模块无法提供的信息。

这个`process`对象是 EventEmitter 类的一个实例。它包含自己的预定义事件，如`exit`，可以用来知道 Node.js 中的程序何时完成执行。

运行下面的程序，您可以观察到结果出现了状态代码`0`。在 Node.js 中，该状态代码表示程序已经成功运行。

```
process.on('exit', code => {
	setTimeout(() => {
		console.log('Will not get displayed');
	}, 0);

	console.log('Exited with status code:', code);
});
console.log('Execution Completed');
```

上述程序的输出:

```
Execution Completed
Exited with status code: 0
```

`Process`还提供了各种属性进行交互。其中一些可以在节点应用程序中使用，以提供节点应用程序和任何命令行接口之间的通信网关。如果您正在使用 Node.js 构建命令行应用程序或实用程序，这将非常有用

*   一个可读的流
*   一个可写的流
*   process.stderr:用于识别错误的可编写的流

使用`argv`你总是可以访问命令行中传递的参数。`argv`是一个数组，第一个元素是节点本身，第二个元素是文件的绝对路径。从第三个元素开始，它可以有任意多的参数。

尝试下面的程序，更深入地了解如何使用这些不同的属性和功能。

```
process.stdout.write('Hello World!' + '\n');

process.argv.forEach(function(val, index, array) {
	console.log(index + ': ' + val);
});
```

如果您用下面的命令运行上面的代码，您将得到输出，并且打印出`argv`的前两个元素。

```
$ node test.js

# output
Hello World!
0: /usr/local/bin/node
1: /Users/amanhimself/Desktop/articles/nodejs-text-tuts/test.js
```