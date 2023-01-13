# Python Sleep 函数——如何让 Python 在继续之前等待几秒钟，示例命令

> 原文：<https://www.freecodecamp.org/news/the-python-sleep-function-how-to-make-python-wait-a-few-seconds-before-continuing-with-example-commands/>

你可以使用 Python 的`sleep()`函数给你的代码添加一个时间延迟。

例如，如果您想在 API 调用之间暂停代码，这个函数非常方便。或者通过在单词或图形之间添加停顿来增强用户体验。

```
from time import sleep
sleep(2)   
print("hello world") 
```

当我运行上面的代码时，在打印“hello world”之前有大约两秒钟的延迟。

我经历了一个延迟，因为`sleep()`在规定的秒数内停止了调用线程的[执行(尽管准确的时间是大概的)。所以在上面的例子中，程序的执行暂停了大约两秒钟。](https://docs.python.org/3/library/time.html#time.sleep)

在本文中，您将学习如何让 Python 代码进入睡眠状态。

# 睡眠的细节

Python 的[时间模块](https://docs.python.org/3/library/time.html#time)包含了很多与时间相关的函数，其中一个就是`sleep()`。为了使用 sleep()，您需要导入它。

```
from time import sleep 
```

sleep()接受一个参数:秒。这是您希望代码延迟的时间量(以秒为单位)。

```
seconds = 2
sleep(seconds) 
```

## 在行动中睡觉

现在让我们以几种不同的方式使用`sleep()`。

在我从`time`模块导入睡眠后，将会打印两行文本。但是，每行打印之间会有大约两秒钟的延迟。

```
from time import sleep

print("Hello")
sleep(2)  
print("World") 
```

这是我运行代码时发生的情况:

**`"Hello"`** 这一行马上打印出来。

然后，大约有两秒钟的延迟。

**`"World"`** 这一行大约在第一行后两秒钟打印出来。

### 你可以很精确

通过向`sleep()`传递一个浮点数来确定您的时间延迟。

```
from time import sleep

print("Prints immediately.")
sleep(0.50)
print("Prints after a slight delay.") 
```

这是我运行代码时发生的情况:

**`"Prints immediately."`** 这一行立即打印出来。

然后，有大约 0.5 秒的延迟。

**`"Prints after a slight delay."`** 这一行先打印后约 0.5 秒。

## 创建时间戳

这里还有一个例子可以考虑。

在下面的代码中，我创建了五个时间戳。我使用`sleep()`在每个时间戳之间添加大约一秒的延迟。

```
import time

for i in range(5):
   current_time = time.localtime()
   timestamp = time.strftime("%I:%m:%S", current_time)
   time.sleep(1)
   print(timestamp) 
```

在这里，我导入了整个时间模块，这样我就可以从中访问多个函数，包括 sleep()。

```
import time 
```

然后，我创建一个 for 循环，它将迭代五次。

```
for i in range(5):
... 
```

在每次迭代中，我得到当前时间。

```
current_time = time.localtime() 
```

我使用时间模块中的另一个函数 **`strftime()`** 得到一个时间戳。

```
timestamp = time.strftime("%I:%m:%S", current_time) 
```

接下来是 sleep()函数，这将导致循环的每次迭代延迟。

```
time.sleep(1) 
```

当我运行程序时，在第一个时间戳打印出来之前，我等待了大约一秒钟。然后，我等待大约一秒钟来打印下一个时间戳，以此类推，直到循环终止。

输出如下所示:

```
04:08:37
04:08:38
04:08:39
04:08:40
04:08:41 
```

### sleep()和用户体验

使用`sleep()`的另一种方法是通过制造一些悬念来提升用户的体验。

```
from time import sleep

story_intro = ["It", "was", "a", "dark", "and", "stormy", "night", "..."]
for word in story_intro:
   print(word)
   sleep(1) 
```

在这里，我遍历了 **`story_intro`** 中的单词列表。为了增加悬念，我使用 sleep()函数在每个单词打印出来后延迟大约一秒钟。

```
time.sleep(1) 
```

尽管执行速度经常是我们考虑的首要问题，但有时放慢速度并在代码中添加停顿是值得的。当这种情况出现时，你知道该伸手去拿什么，以及该怎么做。

我写关于学习编程的文章，以及在 amymhaddad.com 的 T2 学习编程的最佳方法。我发关于编程、学习和生产力的微博: *[@amymhaddad](https://twitter.com/amymhaddad) 。*