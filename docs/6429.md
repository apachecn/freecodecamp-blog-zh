# 限制 JavaScript 中的并发操作

> 原文：<https://www.freecodecamp.org/news/how-to-limit-concurrent-operations-in-javascript-b57d7b80d573/>

通常，执行我们代码的机器资源有限。一次做所有的事情不仅会造成伤害，还会挂起我们的进程，让它完全停止响应。

当我们要抓取 100 个网站的时候，我们应该一次抓取比如 5 个，这样就不会占用所有的可用带宽。只要一个网站被抓取，下一个就准备好了。

一般来说，所有“重”操作都要及时布局。为了获得更好的性能和节省资源，不应该同时执行它们。

### 履行

如果你熟悉我之前关于[实现承诺](https://medium.freecodecamp.org/how-to-implement-promises-in-javascript-1ce2680a7f51)的帖子，那么你会注意到许多相似之处。

```
class Concurrently<T = any> {
  private tasksQueue: (() => Promise<T>)[] = [];
  private tasksActiveCount: number = 0;
  private tasksLimit: number;

  public constructor(tasksLimit: number) {
    if (tasksLimit < 0) {
      throw new Error('Limit cant be lower than 0.');
    }

    this.tasksLimit = tasksLimit;
  }

  private registerTask(handler) {
    this.tasksQueue = [...this.tasksQueue, handler];
    this.executeTasks();
  }

  private executeTasks() {
    while (this.tasksQueue.length && this.tasksActiveCount < this.tasksLimit) {
      const task = this.tasksQueue[0];
      this.tasksQueue = this.tasksQueue.slice(1);
      this.tasksActiveCount += 1;

      task()
        .then((result) => {
          this.tasksActiveCount -= 1;
          this.executeTasks();

          return result;
        })
        .catch((err) => {
          this.tasksActiveCount -= 1;
          this.executeTasks();

          throw err;
        });
    }
  }

  public task(handler: () => Promise<T>): Promise<T> {
    return new Promise((resolve, reject) =>
      this.registerTask(() =>
        handler()
          .then(resolve)
          .catch(reject),
      ),
    );
  }
}

export default Concurrently;
```

我们通过将给定的任务添加到我们的 *tasksQueue* 中来注册它，然后我们调用 *executeTasks* 。

现在，我们在极限允许的情况下执行尽可能多的任务——一个接一个。每次在我们的名为 *tasksActiveCount* 的计数器上加 1。

当执行的任务完成时，我们从 *tasksActiveCount* 中删除 1，并再次调用 *executeTasks* 。

下面我们可以看到一个如何工作的例子。

限制设置为 3。前两项任务需要很长时间来处理。我们可以看到第三个“插槽”不时被打开，允许队列中的下一个任务被执行。

总是有三个，不多也不少。

![1*MxACL9-7TXYJTpUTQdJIHQ](img/65f3059c197567751ec05c895991395c.png)

Executing heavy and light tasks with the limit of 3.

您可以在[库](https://github.com/maciejcieslar/concurrently)中看到代码。

非常感谢您的阅读！你能想到其他方法达到同样的效果吗？下面分享一下。

如果你有任何问题或意见，欢迎在下面的评论区提出，或者给我发[消息](https://www.mcieslar.com/contact)。

查看我的[社交媒体](https://www.maciejcieslar.com/about/)！

[加入我的简讯](http://eepurl.com/dAKhxb)！

*最初发布于 2018 年 8 月 28 日[www.mcieslar.com](https://www.mcieslar.com/limiting-concurrent-operations-in-javascript)。*