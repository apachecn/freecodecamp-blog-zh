# JavaScript 计时事件:setTimeout 和 setInterval

> 原文：<https://www.freecodecamp.org/news/javascript-timing-events-settimeout-and-setinterval/>

程序员使用定时事件来延迟某些代码的执行，或者以特定的间隔重复代码。

JavaScript 库中有两个本地函数用来完成这些任务:`setTimeout()`和`setInterval()`。

### 设置超时

`setTimeout()`用于将传递函数的执行延迟指定的时间。

传递给`setTimeout()`的有两个参数:您想要调用的函数，以及延迟函数执行的时间(以毫秒为单位)。

记住 1 秒有 1000 毫秒，所以 5000 毫秒等于 5 秒。

`setTimeout()`将在指定时间过后从第一个参数开始执行一次函数。

**举例:**

```
let timeoutID;

function delayTimer() {
  timeoutID = setTimeout(delayedFunction, 3000);
}

function delayedFunction() {
  alert(“Three seconds have elapsed.”);
}
```

当调用`delayTimer`函数时，它将运行`setTimeout`。经过 3 秒(3000 毫秒)后，它将执行`delayedFunction`，发出警报。

**设定间隔**

使用`setInterval()`指定一个在执行之间有时间延迟的重复功能。

同样，向`setInterval()`传递两个参数:您想要调用的函数，以及延迟函数每次调用的时间(以毫秒为单位)。

`setInterval()`将继续执行，直到被清除。

**举例:**

```
let intervalID;

function repeatEverySecond() {
  intervalID = setInterval(sendMessage, 1000);
}

function sendMessage() {
  console.log(“One second elapsed.”);
}
```

当你的代码调用函数`repeatEverySecond`时，它将运行`setInterval`。`setInterval`将每秒(1000 毫秒)运行功能`sendMessage`。

### clear 超时和 clearInterval

还有相应的原生函数来停止计时事件:`clearTimeout()`和`clearInterval()`。

您可能已经注意到，上面的每个定时器函数都保存在一个变量中。当`setTimeout`或`setInterval`函数运行时，它会被分配一个数字并保存到该变量中。请注意，JavaScript 在后台完成所有这些工作。

这个生成的数字对于定时器的每个实例都是唯一的。这个分配的数字也是当你想停止定时器时识别它们的方式。因此，您必须始终将计时器设置为变量。

为了代码的清晰，你应该总是匹配`clearTimeout()`到`setTimeout()`和`clearInterval()`到`setInterval()`。

要停止一个计时器，调用相应的 clear 函数，并向它传递与您希望停止的计时器匹配的计时器 ID 变量。`clearInterval()`和`clearTimeout()`的语法是相同的。

**举例:**

```
let timeoutID;

function delayTimer() {
  timeoutID = setTimeout(delayedFunction, 3000);
}

function delayedFunction() {
  alert(“Three seconds have elapsed.”);
}

function clearAlert() {
  clearTimeout(timeoutID);
}
```