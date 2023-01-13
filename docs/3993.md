# 如何创建倒计时器

> 原文：<https://www.freecodecamp.org/news/how-to-create-a-countdown-timer/>

利用 JavaScript 的本地计时事件，构建一个简单的倒计时器很容易。你可以在[这篇文章](https://www.freecodecamp.org/news/p/50cdf5da-8359-4bd2-8718-d5bd7c0de03d/www.freecodecamp.org/news/javascript-timing-events-settimeout-and-setinterval/)中了解更多。

### 构建倒计时定时器

首先声明一个名为`startCountdown`的空函数，它将`seconds`作为参数:

```
function startCountdown(seconds) {

};
```

我们希望记录计时器启动后经过的秒数，因此使用`let`声明一个名为`counter`的变量，并将其设置为等于`seconds`:

```
function startCountdown(seconds) {
  let counter = seconds;
}
```

请记住，最佳做法是将计时事件函数保存到变量中。这使得以后更容易停止计时器。创建一个名为`interval`的变量，并将其设置为等于`setInterval()`:

```
function startCountdown(seconds) {
  let counter = seconds;

  const interval = setInterval();
}
```

可以直接给`setInterval`传递一个函数，所以我们给它传递一个空箭头函数作为第一个参数。此外，我们希望函数每秒运行一次，因此将 1000 作为第二个参数传递:

```
function startCountdown(seconds) {
  let counter = seconds;

  const interval = setInterval(() => {

  }, 1000);
}
```

现在我们传递给`setInterval`的函数将每秒运行一次。每次运行时，我们都希望在取消递增之前将`counter`的当前值记录到控制台:

```
function startCountdown(seconds) {
  let counter = seconds;

  const interval = setInterval(() => {
    console.log(counter);
    counter--;
  }, 1000);
}
```

现在如果你运行这个函数，你会看到它工作了，但是一旦`counter`小于 0:

```
startCountdown(5);

// Console Output // 
// 5
// 4
// 3
// 2
// 1
// 0 
// -1
// -2 
```

要解决这个问题，首先编写一个在`counter`小于 0 时执行的`if`语句:

```
function startCountdown(seconds) {
  let counter = seconds;

  const interval = setInterval(() => {
    console.log(counter);
    counter--;

    if (counter < 0 ) {

    }
  }, 1000);
}
```

然后在`if`语句中，用`clearInterval`清除`interval`,并记录一个报警音字符串到控制台:

```
function startCountdown(seconds) {
  let counter = seconds;

  const interval = setInterval(() => {
    console.log(counter);
    counter--;

    if (counter < 0 ) {
      clearInterval(interval);
      console.log('Ding!');
    }
  }, 1000);
}
```

### **执行**

现在，当您启动计时器时，您应该看到以下内容:

```
startCountdown(5);

// Console Output // 
// 5
// 4
// 3
// 2
// 1
// 0 
// Ding!
```

### **更多资源**

[JavaScript 计时事件:setTimeout 和 setInterval](https://www.freecodecamp.org/news/p/50cdf5da-8359-4bd2-8718-d5bd7c0de03d/www.freecodecamp.org/news/javascript-timing-events-settimeout-and-setinterval/)