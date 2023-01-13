# 如何像专业人士一样测试 Django 信号

> 原文：<https://www.freecodecamp.org/news/how-to-testing-django-signals-like-a-pro-c7ed74279311/>

by Haki Benita

# 如何像专业人士一样测试 Django 信号

![vbRnViG6kVSwEcZLZbrWE-7QtLQhLvznb1xo](img/858820ffaf4e28e10b65af177b642683.png)

A signal receiver I’m glad I don’t have to test

为了更好的阅读体验，请在我的网站上查看[这篇文章。](https://hakibenita.com/how-to-test-django-signals-like-a-pro)

Django 信号对于去耦模块非常有用。它们允许一个低级 Django 应用程序发送事件给其他应用程序处理，而不会产生直接的依赖。

信号很容易建立，但是很难测试。因此，在本文中，我将一步一步地指导您实现一个用于测试 Django 信号的上下文管理器。

### 使用案例

假设您有一个具有收费功能的支付模块。(我[写了很多关于支付](https://medium.com/@hakibenita/working-with-apis-the-pythonic-way-484784ed1ce0#.ji6p00af1)的东西，所以我很了解这个用例。)一旦收费，您希望增加总收费计数器。

使用信号会是什么样子？

首先，定义信号:

```
# signals.py
```

```
from django.dispatch import Signal
```

```
charge_completed = Signal(providing_args=['total'])
```

然后在充电成功完成时发送信号:

```
# payment.py
```

```
from .signals import charge_completed
```

```
@classmethoddef process_charge(cls, total):
```

```
 # Process charge…
```

```
 if success:        charge_completed.send_robust(            sender=cls,            total=total,        )
```

一个不同的应用程序，如摘要应用程序，可以连接一个增加总费用计数器的处理器:

```
# summary.py
```

```
from django.dispatch import receiver
```

```
from .signals import charge_completed
```

```
@receiver(charge_completed)def increment_total_charges(sender, total, **kwargs):    total_charges += total
```

支付模块不必知道汇总模块或处理已完成费用的任何其他模块。**您可以添加多个收款人，而无需修改支付模块**。

例如，以下是接收器的良好候选:

*   更新交易状态。
*   向用户发送电子邮件通知。
*   更新信用卡的最后使用日期。

### 测试信号

既然您已经了解了基础知识，让我们为`process_charge`编写一个测试。当充电成功完成时，您需要确保发送带有正确参数的信号。

测试信号是否发送的最佳方法是连接到它:

```
# test.py
```

```
from django.test import TestCase
```

```
from .payment import chargefrom .signals import charge_completed
```

```
class TestCharge(TestCase):
```

```
 def test_should_send_signal_when_charge_succeeds(self):        self.signal_was_called = False        self.total = None
```

```
 def handler(sender, total, **kwargs):            self.signal_was_called = True            self.total = total
```

```
 charge_completed.connect(handler)
```

```
 charge(100)
```

```
 self.assertTrue(self.signal_was_called)        self.assertEqual(self.total, 100)
```

```
 charge_completed.disconnect(handler)
```

我们创建一个处理程序，连接到信号，执行函数并检查参数。

我们在处理程序中使用`self`来创建一个闭包。如果我们没有使用`self`,处理函数将会在它的局部范围内更新变量，我们将无法访问它们。我们稍后将再次讨论这个问题。

让我们给**添加一个测试，以确保如果充电失败**信号不会被调用:

```
def test_should_not_send_signal_when_charge_failed(self):    self.signal_was_called = False
```

```
 def handler(sender, total, **kwargs):        self.signal_was_called = True
```

```
 charge_completed.connect(handler)
```

```
 charge(-1)
```

```
 self.assertFalse(self.signal_was_called)
```

```
 charge_completed.disconnect(handler)
```

这是可行的，但是有很多样板文件！肯定有更好的办法。

### 进入上下文管理器

让我们来分解一下我们到目前为止所做的事情:

1.  把信号连接到某个处理器上。
2.  运行测试代码并保存传递给处理程序的参数。
3.  断开处理器与信号的连接。

这种模式听起来很熟悉…

让我们看看一个(文件)[打开上下文管理器](https://docs.python.org/3/library/functions.html#open)做什么:

1.  打开一个文件。
2.  处理文件。
3.  关闭文件。

以及[数据库事务上下文管理器](https://docs.djangoproject.com/en/1.10/topics/db/transactions/#controlling-transactions-explicitly):

1.  未结交易。
2.  执行一些操作。
3.  关闭事务(提交/回滚)。

看起来**上下文管理器也可以为信号工作**。

在开始之前，请考虑您希望如何使用上下文管理器来测试信号:

```
with CatchSignal(charge_completed) as signal_args:    charge(100)
```

```
self.assertEqual(signal_args.total, 100)
```

很好，让我们试一试:

```
class CatchSignal:    def __init__(self, signal):        self.signal = signal        self.signal_kwargs = {}
```

```
 def handler(sender, **kwargs):            self.signal_kwrags.update(kwargs)
```

```
 self.handler = handler
```

```
 def __enter__(self):        self.signal.connect(self.handler)        return self.signal_kwrags
```

```
 def __exit__(self, exc_type, exc_value, tb):        self.signal.disconnect(self.handler)
```

我们这里有什么:

*   您用想要“捕捉”的信号初始化了上下文。
*   上下文创建一个处理函数来保存信号发送的参数。
*   您通过在`self`上更新一个现有的对象(`signal_kwargs`)来创建闭包。
*   你把处理器和信号连接起来。
*   (通过测试)在`__enter__`和`__exit__`之间进行一些处理。
*   你切断了信号。

让我们使用上下文管理器来测试收费功能:

```
def test_should_send_signal_when_charge_succeeds(self):    with CatchSignal(charge_completed) as signal_args:        charge(100)    self.assertEqual(signal_args[‘total’], 100)
```

这样更好，但是阴性测试会是什么样的呢？

```
def test_should_not_send_signal_when_charge_failed(self):    with CatchSignal(signal) as signal_args:        charge(100)    self.assertEqual(signal_args, {})
```

牦牛，那不好。

让我们再来看看这个处理程序:

*   我们希望确保调用了处理函数。
*   我们想测试发送给处理函数的参数。

等等… **我已经知道这个功能了！**

### 输入模拟

让我们用一个 Mock 替换我们的处理程序:

```
from unittest import mock
```

```
class CatchSignal:    def __init__(self, signal):        self.signal = signal        self.handler = mock.Mock()
```

```
 def __enter__(self):        self.signal.connect(self.handler)        return self.handler
```

```
 def __exit__(self, exc_type, exc_value, tb):        self.signal.disconnect(self.handler)
```

测试是:

```
def test_should_send_signal_when_charge_succeeds(self):    with CatchSignal(charge_completed) as handler:        charge(100)    handler.assert_called_once_with(        total=100,        sender=mock.ANY,        signal=charge_completed,    )
```

```
def test_should_not_send_signal_when_charge_failed(self):    with CatchSignal(charge_completed) as handler:        charge(-1)        handler.assert_not_called()
```

**好多了！**

您将 mock 用在了它应该用的地方，并且您不需要担心范围和闭包。

既然你已经有了这个工作，你能把它做得更好吗？

### 输入上下文库

Python 有一个用于处理上下文管理器的实用模块，叫做 [contextlib](https://docs.python.org/3.6/library/contextlib.html) 。

让我们用`contextlib`改写我们的上下文:

```
from unittest import mockfrom contextlib import contextmanager
```

```
@contextmanagerdef catch_signal(signal):    """Catch django signal and return the mocked call."""    handler = mock.Mock()    signal.connect(handler)    yield handler    signal.disconnect(handler)
```

我更喜欢这种方法，因为它更容易遵循:

*   yield 清楚地表明了测试代码在哪里执行。
*   无需保存`self`上的对象，因为设置代码(进入和退出)在同一范围内。

就这样——4 行代码就可以解决所有问题！利润！