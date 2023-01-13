# python Do While–循环示例

> 原文：<https://www.freecodecamp.org/news/python-do-while-loop-example/>

在所有现代编程语言中，循环都是一个有用且经常使用的特性。

如果你想自动化一个特定的重复任务或者防止你自己在程序中写重复的代码，使用循环是最好的选择。

循环是一组重复运行直到满足某个条件的指令。让我们进一步了解 Python 中的循环是如何工作的。

## Python 中的循环

Python 内置了两种类型的循环:

*   `for`循环
*   `while`循环

让我们集中讨论如何在 Python 中创建一个`while`循环以及它是如何工作的。

## Python 中的 while 循环是什么？

Python 中`while`循环的一般语法如下:

```
while condition:
    execute this code in the loop's body 
```

当条件为真时，while 循环将运行一段代码。它将继续执行所需的代码语句集，直到该条件不再为真。

while 循环总是在运行前首先检查条件。

如果条件评估为`True`，那么循环将运行循环体内的代码。

例如，只要`number`小于`10`，该循环就会运行:

```
number = 0
while number < 10:
    print(f"Number is {number}!")
    number = number + 1 
```

输出:

```
Number is 0!
Number is 1!
Number is 2!
Number is 3!
Number is 4!
Number is 5!
Number is 6!
Number is 7!
Number is 8!
Number is 9! 
```

这里，变量`number`最初被设置为`0`。

在运行任何代码之前，Python 会检查条件(`number < 10`)。它的计算结果为真，因此执行 print 语句并将`Number is 0!`打印到控制台。

`number`然后增加`1`。条件被重新评估，并且再次为真，因此整个过程重复，直到`number`等于`9`。

此时`Number is 9!`被打印并且`number`递增，但是现在`number`等于`10`，所以条件不再满足，因此循环终止。

如果不满足条件，`while`循环可能永远不会运行，如下例所示:

```
number = 50
while number < 10 :
    print(f"Number is {number}!") 
```

由于条件总是假的，循环体中的指令不会执行。

### 不要制造无限循环

正如你在上面的例子中看到的，`while`循环通常伴随着一个变量，这个变量的值在整个循环过程中是变化的。它最终决定了循环何时结束。

如果不添加这一行，将会创建一个无限循环。

`number`不会递增和更新。它将总是被设置并保持在`0`，因此条件`number < 10`将永远为真。这意味着循环将永远继续循环下去。

```
 # don't run this

number = 0
while number < 10:
    print(f"Number is {number}!") 
```

输出:

```
Number is 0!
Number is 0!
Number is 0!
Number is 0!
Number is 0!
Number is 0!
Number is 0!
... 
```

它无限运行。

这与这样做是一样的:

```
 #don't run this
while True:
    print("I am always true") 
```

如果你发现自己处在这样的情况下会怎样？

按`Control C`退出并结束循环。

## 什么是 do while 循环？

其他编程语言中的`do while`循环的一般语法如下所示:

```
do {
  loop block statement to be executed;
  }
while(condition); 
```

例如，C 语言中的 do while 循环如下所示:

```
#include <stdio.h>

int main(void)
 {
   int i = 10;
   do {
      printf("the value of i: %i\n", i);
      i++;
      }
  while( i < 20 );
 } 
```

do while 循环的独特之处在于，循环块中的代码将至少执行*一次*。

语句中的代码运行一次，然后仅在执行了代码的之后检查条件的*。*

所以代码先运行一次，然后检查条件。

如果检查的条件评估为真，则循环继续。

有些情况下，您希望代码至少运行一次，这就是 do while 循环派上用场的地方。

例如，当你编写一个接受用户输入的程序时，你可能只要求一个正数。代码将至少运行一次。如果用户提交的数字是负数，循环将继续运行。如果是正的，就会停止。

Python 没有像其他语言那样显式创建`do while`循环的内置功能。但是在 Python 中模拟一个`do while`循环是可能的。

## 如何在 Python 中模拟 do while 循环

要在 Python 中创建一个`do while`循环，您需要稍微修改一下`while`循环，以便获得与其他语言中的`do while`循环相似的行为。

作为到目前为止的复习，一个`do while`循环将至少运行一次。如果条件满足，那么它将再次运行。

另一方面，`while`循环至少不会运行一次，事实上可能永远不会运行。当且仅当满足条件时，它才运行。

假设我们有一个例子，我们希望一行代码至少运行一次。

```
secret_word = "python"
counter = 0

while True:
    word = input("Enter the secret word: ").lower()
    counter = counter + 1
    if word == secret_word:
        break
    if word != secret_word and counter > 7: 
        break 
```

代码将至少运行一次，要求用户输入。

用`True`总是保证至少运行一次，否则会产生一个无限循环。

如果用户输入正确的密码，循环终止。

如果用户输入错误的密码超过 7 次，则循环将完全退出。

`break`语句允许您控制`while`循环的流程，而不会以无限循环结束。

`break`将立即终止当前循环并从中脱离。

这就是你如何在 Python 中创建一个类似于`do while`循环的效果。

循环总是至少执行一次。如果不满足条件，它将继续循环，然后在满足条件时终止。

## 结论

您现在知道了如何在 Python 中创建一个`do while`循环。

如果你有兴趣了解更多关于 Python 的知识，你可以在 freeCodeCamp 的 YouTube 频道上观看 [12 Python 项目视频](https://www.youtube.com/watch?v=8ext9G7xspg&t=40s)。你将建立 12 个项目，它是面向初学者的。

freeCodeCamp 还有一个免费的 [Python 认证](https://www.freecodecamp.org/learn/scientific-computing-with-python/)来帮助你很好的理解和全面的了解这门语言的重要基础。

在课程结束时，您还将构建五个项目来实践您所学的内容。

感谢阅读，快乐学习！