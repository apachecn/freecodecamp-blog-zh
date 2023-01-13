# 网络工作者在行动:为什么他们是有用的，以及你应该如何使用他们

> 原文：<https://www.freecodecamp.org/news/web-workers-in-action-2c9ff33be266/>

饥饿的大脑

# 网络工作者在行动:为什么他们是有用的，以及你应该如何使用他们

![1*f1HRu4YeZDn3PAS81QjuNg](img/ff0d0232d1b1f7b2c9a26342c5234e34.png)

Photo by [Fabian Grohs](https://unsplash.com/photos/dC6Pb2JdAqs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/web?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Javascript 是单线程的，多个脚本不能同时执行。因此，如果我们执行任何繁重的计算任务，那么有时我们的页面会变得没有响应，用户在执行完成之前不能做任何其他事情。

例如:

```
average = (numbers) => {
    let startTime = new Date().getTime();
    let len = numbers,
        sum = 0,
        i;

    if (len === 0) {
        return 0;
    }

    for (i = 0; i < len; i++) {
        console.log('i :: ', i)
        sum += i;
    }

    let endTime = new Date().getTime();
    alert('Average - ', sum / len);
}

hello = () => {
    alert("Hello World !!");
}

/*
Paste the above code in browser dev tool console
and try to call average(10000) and hello one by one
*/
```

blocking code

在上面的例子中，如果您在 *hello* 方法之前调用 *average* ，那么您的页面将变得没有响应，并且您将无法点击 *Hello* ，直到 *average* 的执行完成。

可以看到，当首先以 10000 作为输入调用 *average* 时，用了~1.82 秒。在这段时间内，页面变得没有响应，您无法点击 hello 按钮。

### 异步编程

Javascript 给了开发者编写 ****异步代码**** 的方法。通过编写异步代码，您可以在应用程序中避免这种问题，因为它通过“调度”部分代码在事件循环中稍后执行，使您的应用程序 UI 能够做出响应。

异步编程的一个很好的例子是`XHR request`，在这种情况下，我们异步调用一个 API，在等待响应的同时，可以执行其他代码。但是这仅限于与 web APIs 相关的某些用例。

编写异步代码的另一种方式是使用`setTimeout` 方法。在某些情况下，您可以通过使用`setTimeout`来解除 UI 对长时间运行的计算的阻塞，从而获得良好的效果。例如，通过在单独的`setTimeout`调用中批处理复杂的计算。

例如:

```
average = (numbers) => {
    let startTime = new Date().getTime();
    var len = numbers,
        sum = 0,
        i;

    if (len === 0) {
        return 0;
    }

    let calculateSumAsync = (i) => {
        if (i < len) {
            // Put the next function call on the event loop.
            setTimeout(() => {
                sum += i;
                calculateSumAsync(i + 1);
            }, 0);
        } else {
            // The end of the array is reached so we're invoking the alert.
            let endTime = new Date().getTime();
            alert('Average - ', sum / len);
        }
    };

    calculateSumAsync(0);
};

hello = () => {
    alert('Hello World !!')
};
```

**async programming using setTimeout**

在本例中，您可以看到，在您点击*计算平均值*按钮后，您仍然可以点击 *Hello* 按钮(这又会显示一条警告消息)。这种编程方式肯定是无阻塞的，但是花费太多时间，并且在实际应用中不可行。

这里同样输入 10000，用了~60 秒，效率很低。

那么，我们如何有效地解决这类问题呢？

答案是**网络工作者。**

### 什么是网络工作者？

Javascript 中的 Web workers 是一种很好的方式来执行一些任务，这些任务非常费时费力，会占用主线程之外的一个线程。它们在后台运行，执行任务时不会干扰用户界面。

Web Workers 不是 JavaScript 的一部分，它们是可以通过 JavaScript 访问的浏览器特性。

Web workers 由一个构造函数 ****Worker()**** 创建，它运行一个命名的 JS 文件。

```
// create a dedicated web worker
const myWorker = new Worker('worker.js');
```

如果指定的文件存在，那么它将被异步下载，如果不存在，那么 worker 将静默失败，所以您的应用程序在 404 的情况下仍将工作。

在下一节中，我们将了解更多关于 web workers 的创建和工作的内容。

工作线程有自己的上下文，因此你只能访问工作线程内部的选定功能，如 web 套接字、索引数据库。

网络工作者有一些 ****限制****—

1.  你不能从一个工人内部直接操作 DOM。
2.  您不能使用 window 对象的一些默认方法和属性，因为 window 对象在工作线程中不可用。
3.  根据使用情况，可以通过******DedicatedWorkerGlobalScope 或 SharedWorkerGlobalScope******访问工作线程内部的上下文。

### 网络工作者的特征

有两种类型的网络工作者

1.  ****专用 web worker****——专用 worker 只能被调用它的脚本访问。
2.  ****共享 web worker****——一个共享 worker 可以被多个脚本访问——即使它们被不同的窗口、iframes 甚至 worker 访问。

让我们来讨论一下这两类网络工作者

#### 创建网络工作者

对于专用的和共享的网络工作者来说，创作是非常相似的。

****专门的网络工作者****

*   创建一个新的 worker 很简单，只需调用 worker 构造函数并传递想要作为 Worker 执行的脚本的路径。

```
// create a dedicated web worker
const myWorker = new Worker('worker.js');
```

main.js

****共享 web worker :****

*   创建一个新的共享 worker 与创建一个专用 worker 非常相似，但是使用了不同的构造函数名。

```
// creating a shared web worker
const mySharedWorker = new SharedWorker('worker.js');
```

shared-main.js

#### 主线程和工作线程之间的通信

主线程和工作线程之间的通信通过 **postMessage** 方法和 **onmessage** 事件处理程序进行。

****专用 web worker****
如果有专用 web worker，通信系统就简单了。你只需要使用 postMessage 方法，无论何时你想发送消息给工人。

```
(() => {
  // new worker
  let myWorker = new Worker('worker.js');

  // event handler to recieve message from worker
  myWorker.onmessage = (e) => {
    document.getElementById('time').innerHTML = `${e.data.time} seconds`;
  };

  let average = (numbers) => {
    // sending message to web worker with an argument
    myWorker.postMessage(numbers);
  }

  average(1000);
})();
```

main.js

在 web worker 中，当收到消息时，您可以通过编写如下的事件处理程序块进行响应:

```
onmessage = (e) => {
    let numbers = e.data;
    let startTime = new Date().getTime();
    let len = numbers,
        sum = 0,
        i;

    if (len === 0) {
        return 0;
    }

    for (i = 0; i < len; i++) {
        sum += i;
    }

    let endTime = new Date().getTime();
    postMessage({average: sum / len, time: ((endTime - startTime) / 1000)})
};
```

main-worker.js

`onmessage`处理程序允许在收到消息时运行一些代码。

这里我们计算数字的平均值，然后再次使用`postMessage()`，将结果发送回主线程。

正如你在 main.js 的第 6 行 **上看到的，我们已经在 worker 实例上使用了 onmessage 事件。因此，每当工作线程使用 postMessage 时，主线程中的 onmessage 就会被触发。**

*   ****共享 web worker****
    在共享 web worker 的情况下，通信系统几乎没有什么不同。由于多个脚本共享一个 worker，我们需要通过 worker 实例的 port 对象进行通信。对于敬业的员工，这是隐式完成的。每当您想要向工作人员发送消息时，都需要使用 postMessage 方法。

```
(() => {
  // new worker
  let myWorker = new Worker('worker.js');

  // event handler to recieve message from worker
  myWorker.onmessage = (e) => {
    document.getElementById('time').innerHTML = `${e.data.time} seconds`;
  };

  let average = (numbers) => {
    // sending message to web worker with an argument
    myWorker.postMessage(numbers);
  }

  average(1000);
```

main-shared.js

在 web worker(*main-shared-worker . js*)内部，情况有点复杂。首先，我们使用一个`onconnect`处理程序在连接到端口时触发代码( *line 2* )。
我们使用这个事件对象的`ports`属性来抓取端口，并将其存储在一个变量中( *line 4* )。
接下来，我们在端口上添加一个`message`处理程序来进行计算，并将结果返回给主线程(*第 7 行和第 25 行*)，如下所示:

```
onmessage = (e) => {
    let numbers = e.data;
    let startTime = new Date().getTime();
    let len = numbers,
        sum = 0,
        i;

    if (len === 0) {
        return 0;
    }

    for (i = 0; i < len; i++) {
        sum += i;
    }

    let endTime = new Date().getTime();
    postMessage({average: sum / len, time: ((endTime - startTime) / 1000)})
};
```

main-shared-worker.js

#### 网络工作者的终止

如果需要立即从主线程中终止一个正在运行的工作线程，可以通过调用工作线程的 *terminate* 方法来实现:

```
// terminating a web worker instance
myWorker.terminate();
```

工作线程被立即终止，没有机会完成其操作。

### web worker 的衍生

如果愿意，工蚁可以繁殖更多的工蚁。但是它们必须与父页面驻留在同一原点。

### 导入脚本

工作线程可以访问一个全局函数`importScripts()`，这个函数允许它们导入脚本。

```
importScripts();                         /* imports nothing */
importScripts('foo.js');                 /* imports just "foo.js" */
importScripts('foo.js', 'bar.js');       /* imports two scripts */
importScripts('//example.com/hello.js'); /* You can import scripts from other origins */
```

### 工作演示

我们已经讨论了一些实现异步编程的方法，这样我们的 UI 就不会因为繁重的计算任务而被阻塞。但是这些方法有一些限制。所以我们可以使用 web workers 来有效地解决这类问题。

> *点击[此处](https://bhushangoel.github.io/webworker-demo-1/)进行现场演示。*

在这里，您将看到 3 个部分:

1.  **阻塞代码** :
    当您点击*计算平均值*时，加载器不显示，一段时间后您会看到最终结果和花费的时间。这是因为一旦调用了*平均方法*，我也触发了 *showLoader* 方法。但是由于 JS 是单线程的，所以在 average 执行完毕之前，它不会执行 showLoader。因此，在这种情况下，您将永远看不到加载程序。
2.  **异步代码** :
    在这里，我试图通过使用 setTimeout 方法，并将每个函数的执行放入一个事件循环中，来实现相同的功能。在这种情况下，您将看到加载器，但是与上面定义的方法相比，响应需要时间。
3.  Web worker :
    这是一个使用 Web worker 的例子。在这种情况下，只要您单击“计算平均值”,您就会看到加载器，并且您将在与方法 1 相同的时间内获得相同数量的响应。

您可以在此访问相同[的源代码。](https://github.com/bhushangoel/webworker-demo-1/tree/master)

### 高级概念

有一些与 web workers 相关的高级概念。我们不会详细讨论它们，但了解它们是有好处的。

1.  **内容安全策略—**
    Web worker 拥有自己的独立于创建它们的文档的执行上下文，因此它们不受父线程/worker 的内容安全策略的控制。
    例外情况是工作脚本的来源是一个全局唯一标识符(例如，如果它的 URL 有一个数据或 blob 方案)。在这种情况下，员工继承文档或创建文档的员工的内容安全策略。
2.  **与工作线程之间传递数据** —
    主线程与工作线程之间传递的数据是 ****复制的**** 而不是共享的。对象在传递给工人时被序列化，随后在另一端被反序列化。页面和工作进程 ****不共享同一个实例**** ，所以最终结果是 ****的两端都创建了一个重复的**** 。
    浏览器实现了 [****结构化克隆****](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Structured_clone_algorithm) 算法来实现这一点。
3.  **嵌入 worker—**
    你也可以把 worker 的代码嵌入到一个网页(html)里面。为此，您需要添加一个不带 src 属性的脚本标记，并为其分配一个不可执行的 MIME 类型，如下所示:

```
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8" />
  <title>embedded worker</title>
  <!--script tag with un-identified MIME and w/o src attr -->
  <script type="text/js-worker">
    // This script WON'T be parsed by JS engines because its MIME type is text/js-worker.
    var myVar = 'Hello World!';

    // worker block
    function onmessage(e) {
      // worker code
    }
  </script>
</head>
<body></body>
</html>
```

在我们的应用程序中有很多使用 web workers 的用例。我刚刚讨论了一个小场景。希望这能帮助你理解网络工作者的概念。

#### [链接]

github Repo:[https://github.com/bhushangoel/webworker-demo-1](https://github.com/bhushangoel/webworker-demo-1)Web worker in action:[https://bhushangoel.github.io/webworker-demo-1/](https://bhushangoel.github.io/webworker-demo-1/)JS demo showcase:[https://bhushangoel.github.io/](https://bhushangoel.github.io/)

感谢您的阅读。

快乐学习:)

*最初发表于[www.thehungrybrain.com](https://www.thehungrybrain.com/home/web-workers)。*