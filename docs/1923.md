# Python Decorators 如何用 Python 创建和使用 decorator，并附有示例

> 原文：<https://www.freecodecamp.org/news/python-decorators-explained-with-examples/>

Python decorators 允许您在不修改函数本身的情况下改变函数的行为。

在这篇文章中，我将向你展示如何创建和使用装饰器。您将会看到使用这个高级 Python 特性是多么容易。

在本文中，我将讨论以下主题:

*   在 Python 中何时使用装饰器
*   用于创建装饰器的构建块
*   如何创建 Python 装饰器
*   Python 装饰者的真实例子
*   Python 中的类装饰器

## 在 Python 中何时使用装饰器

当您需要在不修改函数本身的情况下改变函数的行为时，您将使用装饰器。当您想要添加日志记录、测试性能、执行缓存、验证权限等等时，这是一个很好的例子。

当您需要在多个函数上运行相同的代码时，也可以使用一个。这可以避免你写重复的代码。

## 下面是用于创建 Python 装饰器的构建块

为了更好地理解装饰者是如何工作的，你应该首先理解几个概念。

1.  函数是一个对象。因此，一个函数可以被赋给一个变量。可以从该变量访问该函数。

```
def my_function():

    print('I am a function.')

# Assign the function to a variable without parenthesis. We don't want to execute the function.

description = my_function
```

```
# Accessing the function from the variable I assigned it to.

print(description())

# Output

'I am a function.' 
```

2.一个函数可以嵌套在另一个函数中。

```
def outer_function():

    def inner_function():

        print('I came from the inner function.')

    # Executing the inner function inside the outer function.
    inner_function() 
```

```
outer_function()

# Output

I came from the inner function. 
```

请注意，`inner_function`在`outer_function`之外不可用。如果我试图在`outer_function`之外执行`inner_function`，我会收到一个 NameError 异常。

```
inner_function()

Traceback (most recent call last):
  File "/tmp/my_script.py", line 9, in <module>
    inner_function()
NameError: name 'inner_function' is not defined
```

3.因为一个函数可以嵌套在另一个函数中，所以它也可以被返回。

```
def outer_function():
    '''Assign task to student'''

    task = 'Read Python book chapter 3.'
    def inner_function():
        print(task)
    return inner_function

homework = outer_function() 
```

```
homework()

# Output

'Read Python book chapter 5.' 
```

4.一个函数可以作为参数传递给另一个函数。

```
def friendly_reminder(func):
    '''Reminder for husband'''

    func()
    print('Don\'t forget to bring your wallet!')

def action():

    print('I am going to the store buy you something nice.') 
```

```
# Calling the friendly_reminder function with the action function used as an argument.

friendly_reminder(action)

# Output

I am going to the store buy you something nice.
Don't forget to bring your wallet! 
```

## 如何创建 Python 装饰器

为了在 Python 中创建装饰函数，我创建了一个外部函数，它将一个函数作为参数。还有一个内部函数包裹着被修饰的函数。

下面是一个基本 Python 装饰器的语法:

```
def my_decorator_func(func):

    def wrapper_func():
        # Do something before the function.
        func()
        # Do something after the function.
    return wrapper_func 
```

要使用装饰器，您可以将它附加到一个函数上，如下面的代码所示。我们通过将装饰者的名字直接放在我们想要使用它的函数上面来使用装饰者。您用一个`@`符号作为装饰函数的前缀。

```
@my_decorator_func
def my_func():

    pass
```

这里有一个简单的例子。这个装饰器记录函数执行的日期和时间:

```
from datetime import datetime

def log_datetime(func):
    '''Log the date and time of a function'''

    def wrapper():
        print(f'Function: {func.__name__}\nRun on: {datetime.today().strftime("%Y-%m-%d %H:%M:%S")}')
        print(f'{"-"*30}')
        func()
    return wrapper

@log_datetime
def daily_backup():

    print('Daily backup job has finished.')   

daily_backup()

# Output

Daily backup job has finished.
Function: daily_backup
Run on: 2021-06-06 06:54:14
---------------------------
```

## 如何在 Python 中给 Decorators 添加参数

装饰者可以得到传递给他们的参数。为了给装饰者添加参数，我给内部函数添加了`*args` 和`****kwargs`。

*   `***args**`将接受任意类型的无限数量的参数，例如`10`、`True`或`'Brandon'`。
*   `****kwargs**`将接受无限数量的关键字参数，例如`count=99`、`is_authenticated=True`或`name='Brandon'`。

下面是一个带参数的装饰器:

```
def my_decorator_func(func):

    def wrapper_func(*args, **kwargs):
        # Do something before the function.
        func(*args, **kwargs)
        # Do something after the function.
    return wrapper_func

@my_decorator_func
def my_func(my_arg):
    '''Example docstring for function'''

    pass 
```

装饰者隐藏他们正在装饰的功能。如果我检查`__name__`或`__doc__`方法，我们会得到一个意外的结果。

```
print(my_func.__name__)
print(my_func.__doc__)

# Output

wrapper_func
None 
```

为了解决这个问题，我将使用`functools`。Functools 包装将使用修饰的函数属性更新装饰器。

```
from functools import wraps

def my_decorator_func(func):

    @wraps(func)
    def wrapper_func(*args, **kwargs):
        func(*args, **kwargs)
    return wrapper_func

@my_decorator_func
def my_func(my_args):
    '''Example docstring for function'''

    pass 
```

现在，我收到了预期的输出。

```
print(my_func.__name__)
print(my_func.__doc__)

# Output

my_func
Example docstring for function 
```

## Python 装饰器的实际例子

我已经创建了一个装饰器来测量一个函数的内存和速度。我们将使用装饰器来测试性能列表的生成，使用四种方法:范围、列表理解、附加和连接。

```
from functools import wraps
import tracemalloc
from time import perf_counter 

def measure_performance(func):
    '''Measure performance of a function'''

    @wraps(func)
    def wrapper(*args, **kwargs):
        tracemalloc.start()
        start_time = perf_counter()
        func(*args, **kwargs)
        current, peak = tracemalloc.get_traced_memory()
        finish_time = perf_counter()
        print(f'Function: {func.__name__}')
        print(f'Method: {func.__doc__}')
        print(f'Memory usage:\t\t {current / 10**6:.6f} MB \n'
              f'Peak memory usage:\t {peak / 10**6:.6f} MB ')
        print(f'Time elapsed is seconds: {finish_time - start_time:.6f}')
        print(f'{"-"*40}')
        tracemalloc.stop()
    return wrapper

@measure_performance
def make_list1():
    '''Range'''

    my_list = list(range(100000))

@measure_performance
def make_list2():
    '''List comprehension'''

    my_list = [l for l in range(100000)]

@measure_performance
def make_list3():
    '''Append'''

    my_list = []
    for item in range(100000):
        my_list.append(item)

@measure_performance
def make_list4():
    '''Concatenation'''

    my_list = []
    for item in range(100000):
        my_list = my_list + [item]

print(make_list1())
print(make_list2())
print(make_list3())
print(make_list4())

# Output

Function: make_list1
Method: Range
Memory usage:		        0.000072 MB 
Peak memory usage:	        3.693040 MB 
Time elapsed is seconds:    0.049359
----------------------------------------

Function: make_list2
Method: List comprehension
Memory usage:		        0.000856 MB 
Peak memory usage:	        3.618244 MB 
Time elapsed is seconds:    0.052338
----------------------------------------

Function: make_list3
Method: Append
Memory usage:		        0.000448 MB 
Peak memory usage:	        3.617692 MB 
Time elapsed is seconds:    0.060719
----------------------------------------

Function: make_list4
Method: Concatenation
Memory usage:		        0.000440 MB 
Peak memory usage:	        4.393292 MB 
Time elapsed is seconds:    61.649138
---------------------------------------- 
```

您也可以在类中使用 decorators。让我们看看如何在 Python 类中使用 decorators。

在这个例子中，注意没有涉及到`@`字符。使用`__call__`方法，当类的实例被创建时，装饰器被执行。

这个类跟踪查询 API 的函数运行的次数。一旦到达极限，装饰器就停止函数的执行。

```
import requests

class LimitQuery:

    def __init__(self, func):
        self.func = func
        self.count = 0

    def __call__(self, *args, **kwargs):
        self.limit = args[0]
        if self.count < self.limit:
            self.count += 1
            return self.func(*args, **kwargs)
        else:
            print(f'No queries left. All {self.count} queries used.')
            return

@LimitQuery
def get_coin_price(limit):
    '''View the Bitcoin Price Index (BPI)'''

    url = requests.get('https://api.coindesk.com/v1/bpi/currentprice.json')

    if url.status_code == 200:
        text = url.json()
        return f"${float(text['bpi']['USD']['rate_float']):.2f}"

print(get_coin_price(5))
print(get_coin_price(5))
print(get_coin_price(5))
print(get_coin_price(5))
print(get_coin_price(5))
print(get_coin_price(5))

# Output

$35968.25
$35896.55
$34368.14
$35962.27
$34058.26
No queries left. All 5 queries used. 
```

这个类将跟踪类的状态。

# 结论

在本文中，我讨论了如何将函数传递给变量、嵌套函数、返回函数以及将函数作为参数传递给另一个函数。

我还向您展示了如何创建和使用 Python decorators，以及一些真实的例子。现在，我希望你能够添加装饰到你的项目。

关注我的 [Github](https://github.com/brandon-wallace) | [开发到](https://dev.to/brandonwallace)