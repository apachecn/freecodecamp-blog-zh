# 多线程 Python:滑过 I/O 瓶颈？

> 原文：<https://www.freecodecamp.org/news/multithreaded-python/>

### 如何利用 Python 中的并行性使您的软件速度提高几个数量级。

我最近开发了一个项目，我称之为 [Hydra](https://github.com/victoriadrake/hydra-link-checker) :用 Python 编写的多线程链接检查器。与我在研究中发现的许多 Python 网站爬虫不同，Hydra 只使用标准库，没有像 BeautifulSoup 这样的外部依赖。它旨在作为 CI/CD 流程的一部分运行，因此它的成功部分取决于速度。

Python 中的多线程是一个有点棘手的问题(不好意思),因为 Python 解释器实际上并不允许多线程同时执行。

Python 的[全局解释器锁](https://wiki.python.org/moin/GlobalInterpreterLock)，或 GIL，防止多个线程同时执行 Python 字节码。每个想要执行的线程必须首先等待当前正在执行的线程释放 GIL。GIL 几乎是低预算会议小组中的麦克风，除了没有人能大声说话的地方。

这具有防止[竞态条件](https://en.wikipedia.org/wiki/Race_condition)的优点。然而，它缺乏并行运行多个任务所带来的性能优势。(如果你想重温一下并发、并行和多线程，请参阅[并发、并行和圣诞老人的多线程](https://victoria.dev/blog/concurrency-parallelism-and-the-many-threads-of-santa-claus/)。)

虽然我更喜欢 Go，因为它提供了支持并发性的方便的一级原语(参见 [Goroutines](https://tour.golang.org/concurrency/1) )，但是这个项目的接受者更喜欢 Python。我把它当成了一个考验和探索的机会！

在 Python 中同时执行多个任务并非不可能；只是需要做一些额外的工作。对于 Hydra 来说，主要优势在于克服输入/输出(I/O)瓶颈。

为了让网页被检查，Hydra 需要到互联网上获取它们。与 CPU 单独执行的任务相比，通过网络执行的任务相对较慢。有多慢？

以下是在典型电脑上执行任务的大致时间:

|  | 工作 | 时间 |
| --- | --- | --- |
| 中央处理器 | 执行典型指令 | 1/1，000，000，000 秒= 1 纳秒 |
| 中央处理器 | 从 L1 高速缓存中提取 | 0.5 纳秒 |
| 中央处理器 | 分支预测失误 | 5 毫微秒 |
| 中央处理器 | 从 L2 高速缓存中提取 | 7 毫微秒 |
| 随机存取存储 | 互斥锁/解锁 | 25 毫微秒 |
| 随机存取存储 | 从主存储器提取 | 100 毫微秒 |
| 网络 | 通过 1Gbps 网络发送 2K 字节 | 20，000 纳秒 |
| 随机存取存储 | 从内存中顺序读取 1MB | 250，000 纳秒 |
| 唱片 | 从新磁盘位置提取(寻道) | 800 万纳秒(8 毫秒) |
| 唱片 | 从磁盘顺序读取 1MB | 20，000，000 纳秒(20 毫秒) |
| 网络 | 把我们的包裹寄到欧洲再寄回来 | 150，000，000 纳秒(150 毫秒) |

几年前，Peter Norvig 在《十年自学编程》中首次发表了这些数据。由于计算机及其组件每年都在变化，上面显示的确切数字并不是重点。这些数字有助于说明不同操作之间的数量级差异。

比较从主存中取数据和通过互联网发送一个简单的数据包的区别。虽然从人类的角度来看，这两种操作都发生在不到一眨眼的时间内，但您可以看到，通过互联网发送一个简单的数据包比从 RAM 中读取要慢 100 多万倍。在单线程程序中，这种差异会很快积累起来，形成麻烦的瓶颈。

在 Hydra 中，解析响应数据和将结果组装成报告的任务相对较快，因为这都发生在 CPU 上。程序执行最慢的部分是网络延迟，超过六个数量级。Hydra 不仅需要获取数据包，还需要获取整个网页！

提高 Hydra 性能的一个方法是找到一种方法，让页面获取任务在不阻塞主线程的情况下执行。

Python 有几个并行执行任务的选项:多个进程或多个线程。这些方法允许您避开 GIL，并以几种不同的方式加速执行。

## 多重过程

要使用多个进程执行并行任务，可以使用 Python 的 [`ProcessPoolExecutor`](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.ProcessPoolExecutor) 。来自 [`concurrent.futures`模块](https://docs.python.org/3/library/concurrent.futures.html)[`Executor`](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.Executor)的具体子类`ProcessPoolExecutor`使用由 [`multiprocessing`模块](https://docs.python.org/3/library/multiprocessing.html#module-multiprocessing)产生的进程池来避免 GIL。

该选项使用最大默认为机器上处理器数量的工作子进程。`multiprocessing`模块允许您跨进程最大限度地并行执行功能，这可以真正加速计算绑定(或 [CPU 绑定](https://en.wikipedia.org/wiki/CPU-bound))任务。

因为 Hydra 的主要瓶颈是 I/O，而不是 CPU 要完成的处理，所以使用多线程会更好。

## 多线程

顾名思义，Python 的 [`ThreadPoolExecutor`](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.ThreadPoolExecutor) 使用线程池来执行异步任务。也是 [`Executor`](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.Executor) 的子类，它使用定义数量的最大工作线程(根据公式`min(32, os.cpu_count() + 4)`，默认情况下至少五个)，并在启动新线程之前重用空闲线程，使其非常高效。

下面是 Hydra 的一个片段，其中的注释展示了 Hydra 如何使用`ThreadPoolExecutor`来实现并行多线程的好处:

```
# Create the Checker class
class Checker:
    # Queue of links to be checked
    TO_PROCESS = Queue()
    # Maximum workers to run
    THREADS = 100
    # Maximum seconds to wait for HTTP response
    TIMEOUT = 60

    def __init__(self, url):
        ...
        # Create the thread pool
        self.pool = futures.ThreadPoolExecutor(max_workers=self.THREADS)

def run(self):
    # Run until the TO_PROCESS queue is empty
    while True:
        try:
            target_url = self.TO_PROCESS.get(block=True, timeout=2)
            # If we haven't already checked this link
            if target_url["url"] not in self.visited:
                # Mark it as visited
                self.visited.add(target_url["url"])
                # Submit the link to the pool
                job = self.pool.submit(self.load_url, target_url, self.TIMEOUT)
                job.add_done_callback(self.handle_future)
        except Empty:
            return
        except Exception as e:
            print(e) 
```

你可以在 [Hydra 的 GitHub 库](https://github.com/victoriadrake/hydra-link-checker)中查看完整代码。

## 单线程到多线程

如果你想看完整的效果，我比较了一个原型单线程程序和多线程程序的运行时间。

```
time python3 slow-link-check.py https://victoria.dev

real    17m34.084s
user    11m40.761s
sys     0m5.436s

time python3 hydra.py https://victoria.dev

real    0m15.729s
user    0m11.071s
sys     0m2.526s 
```

阻塞 I/O 的单线程程序运行了大约 17 分钟。当我第一次运行多线程版本时，它在 1 分钟 13.358 秒内完成——经过一些分析和调优，它用了不到 16 秒。

同样，确切的时间并不意味着什么；它们会根据不同的因素而变化，比如被抓取的站点的大小，你的网络速度，以及你的程序在线程管理的开销和并行性的好处之间的平衡。

更重要的事情，也是我将随时看到的结果，是一个运行速度快几个数量级的程序。