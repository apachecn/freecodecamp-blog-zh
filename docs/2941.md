# 谴责 JavaScript——如何让你的 js 等待

> 原文：<https://www.freecodecamp.org/news/debounce-javascript-tutorial-how-to-make-your-js-wait-up/>

去抖方法在被调用时不执行。相反，它们在执行之前会等待一段预定的时间。如果再次调用相同的方法，前一个方法将被取消，计时器将重新启动。

这是一个简短的视频演示，其中我制作了一个去抖方法:

[https://www.youtube.com/embed/NfYIiKRZTaU?feature=oembed](https://www.youtube.com/embed/NfYIiKRZTaU?feature=oembed)

这是视频教程的源代码:

[https://codepen.io/adeelibr/embed/preview/LYNPYmb?height=300&slug-hash=LYNPYmb&default-tabs=js,result&host=https://codepen.io](https://codepen.io/adeelibr/embed/preview/LYNPYmb?height=300&slug-hash=LYNPYmb&default-tabs=js,result&host=https://codepen.io)

现在让我们更详细地看看代码。

假设您有一个如下所示的按钮:

```
<button id="myBtn">Click me</button>
```

在你的 JS 文件中有这样的内容:

```
document.getElementById('myBtn').addEventListener('click', () => {
  console.log('clicked');
})
```

每次你点击你的按钮，你都会在你的控制台上看到一条消息，上面写着`clicked`。

让我们在这里为我们的`click`事件监听器添加一个去反跳方法:

```
document.getElementById('myBtn').addEventListener('click', debouce(() => {
  console.log('click');
}, 2000))
```

这里的去抖方法接受两个参数，`callback` & `wait`。`callback`是您想要执行的功能，而`wait`是您想要执行`callback`的可配置时间延迟。

这里我们的`callback`方法简单来说就是`console.log('click');`，`wait`就是`2000 milliseconds`。

给定这个去抖方法，它接受两个参数`callback` & `wait`，让我们定义`debounce`:

```
function debounce(callback, wait) {
  let timerId;
  return (...args) => {
    clearTimeout(timerId);
    timerId = setTimeout(() => {
      callback(...args);
    }, wait);
  };
}
```

函数`debounce`接受两个参数:回调(我们想要执行的函数)和`wait`周期(我们想要在多长时间延迟后执行回调)。

在函数内部，我们简单地返回一个函数，如下所示:

```
let timerId;
return (...args) => {
  clearTimeout(timerId);
  timerId = setTimeout(() => {
    callback(...args);
  }, wait);
};
```

这个函数的作用是在一段时间后调用我们的`callback`方法。并且如果在这段时间内再次调用相同的方法，则先前的功能被取消，并且定时器被重置并再次启动。

就是这样！你只需要知道什么是去抖。

这是另一个关于闭包的额外视频，因为我在我的`debounce`函数中使用了一个`closure`。

[https://www.youtube.com/embed/-Q7oXxxw0-c?feature=oembed](https://www.youtube.com/embed/-Q7oXxxw0-c?feature=oembed)

如果你能在去抖方法中找到闭包的用法，请在 twitter 上告诉我。

各位编码愉快。