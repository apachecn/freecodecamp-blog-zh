# Python String to Int:如何在 Python 中将字符串转换成整数

> 原文：<https://www.freecodecamp.org/news/python-string-to-int-how-to-convert-a-string-to-an-integer-in-python/>

与许多其他编程语言不同，当您将整数连接成字符串时，Python 不会隐式地将整数(或浮点数)类型转换成字符串。

幸运的是，Python 有一个方便的内置函数`str()`，它会将传入的参数转换成字符串格式。

## Python 中把字符串转换成整数的错误方法

来自其他编程语言的程序员可能会尝试执行以下字符串连接，这将产生一个错误:

```
age = 18

string = "Hello, I am " + age + " years old"
```

你可以在 repl.it 上运行这段代码。

显示的错误是:

```
Traceback (most recent call last):
  File "python", line 3, in <module>
TypeError: must be str, not int
```

这里，`TypeError: must be str, not int`表示整数必须先转换成字符串，然后才能连接。

## Python 中把字符串转换成整数的正确方法

下面是一个简单的串联示例:

```
age = 18

print("Hello, I am " + str(age) + " years old")

# Output
# Hello, I am 18 years old
```

你可以在 repl.it 上运行这段代码。

下面是如何使用单个字符串打印`1 2 3 4 5 6 7 8 9 10`:

```
result = ""

for i in range(1, 11):
    result += str(i) + " "

print(result)

# Output
# 1 2 3 4 5 6 7 8 9 10
```

[你可以在 repl.it](https://repl.it/@christopher_tse/int-to-string-loop) 上运行代码。

### 下面是对上述代码工作原理的逐行解释:

1.  首先，将变量“result”赋给一个空字符串。
2.  for 循环用于迭代一系列数字。
3.  这个数字列表是使用 range 函数生成的。
4.  所以 range(1，11)将生成一个从 1 到 10 的数字列表。
5.  在每次 for 循环迭代中，这个“I”变量将取值从 1 到 10。
6.  在第一次迭代中，当变量 i=1 时，变量[result=result+str(i)+"(空格字符)"]，str(i)将整数值“I”转换为字符串值。
7.  由于 i=1，在第一次迭代中最终结果=1。
8.  同样的过程一直持续到 i=10，最后在最后一次迭代后，结果=1 2 3 4 5 6 7 8 9 10。
9.  因此，当我们在 for 循环后最终打印结果时，控制台上的输出是“1 2 3 4 5 6 7 8 9 10”。

我希望这对你有所帮助。快乐编码。