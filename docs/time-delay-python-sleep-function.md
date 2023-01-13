# 如何使用 sleep()函数在 Python 中设置时间延迟

> 原文：<https://www.freecodecamp.org/news/time-delay-python-sleep-function/>

有时候你希望你的程序立即运行。但是也有一些时候你想延迟某些代码的执行。

这就是 Python 的`time`模块的用武之地。`time`是 Python 标准库的一部分，包含有用的`sleep()`函数，它可以在给定的秒数内暂停或中止程序:

```
import time

print('runs immediately')

for letter in 'hello, world!':
    time.sleep(2)  # sleep 2 seconds between each print
    print(letter)
```

**输出:**

```
runs immediately
h # each character printed after a two second delay
e
l
l
o
,

w
o
r
l
d
!
```

浮点数可以作为参数提供给`sleep()`以获得更精确的睡眠时间。例如，下面的代码将每个`print()`语句延迟半秒，即 500 毫秒:

```
import time

for letter in 'floats work, too':
  time.sleep(0.5) # adds a 500 ms delay
  print(letter)
```

**输出:**

```
f # each character printed after a 500 ms delay
l
o
a
t
s

w
o
r
k
,

t
o
o
```

有时你可能需要延迟已知的不同的时间增量。在这种情况下，您可以用一个`for`循环遍历一系列不同的延迟时间:

```
import time

for i in [.5, 1, 2, 3, 4]:
  time.sleep(i)
  print(f"Delayed for {i} seconds")
```

**输出:**

```
Delayed for 0.5 seconds
Delayed for 1 seconds
Delayed for 2 seconds
Delayed for 3 seconds
Delayed for 4 seconds
```

可以想象，使用`sleep()`函数可以做很多事情。现在，在你自己的程序中尝试一下吧——不需要考虑太久！

#### **更多信息:**

睡眠功能的时间模块[文档](https://docs.python.org/3/library/time.html#time.sleep)。

## 更多 Python 教程:

[最好的 Python 教程](https://www.freecodecamp.org/news/best-python-tutorial/)

[最好的 Python 代码示例](https://www.freecodecamp.org/news/python-example/)

[查克博士为大家带来的 Python](https://www.freecodecamp.org/news/python-for-everybody/)