# Python If-Else 语句示例

> 原文：<https://www.freecodecamp.org/news/python-if-else-statement-example/>

If-Else 语句——也称为条件逻辑——是编程的基石。Python 拥有这些优势。

Python 为评估变量、变量状态以及是否满足特定条件提供了多个选项:

*   普通 if-else 语句
*   没有 else 部分的 if 语句
*   嵌套的 if-else 语句
*   else-if 或`elif`语句
*   并以 for-else 和 while-else 的形式循环 if-else 语句

我们将讨论所有这些，并解释非常有用的双等于运算符。

## 如何用 Python 写 if-else 语句？

如果你只是在寻找一个可以复制粘贴的例子，这里有一个非常简单的 Python 中的 if-else 语句:

```
if x < y:
	print("x is less than y")
else:
	print("x is equal to or greater than y")
```

请注意，Python 对空格敏感的事实确保了这些 if-else 语句易于阅读——即使它们嵌套了几层。

说完这些，我们再来谈谈条件逻辑，以及为什么 if-else 语句对 Python 和其他编程语言如此重要。

## 我们如何使用 if-else 语句？

If-else 语句是条件逻辑的一种形式。本质上，这意味着

1.  我们测试一个条件。例如，一个给定变量是否等于另一个给定变量。
2.  如果条件为真，我们执行下面的代码块。
3.  如果条件为假，我们执行不同的代码块。

这对于任何类型的编程都是至关重要的。没有某种条件逻辑，就不可能有图灵完全编程语言。在 Python 中，这意味着大量 if-else 语句。

## Python 中 if 语句和 if else 语句有什么不同？

所以从技术上来说，你不需要 if-else 语句的`else`部分。例如:

```
age = int(input("Enter your age: ")) 
if age >= 18: print("You are eligible to vote!") 
```

要了解这是如何工作的，下面是 Python REPL:

```
>>> age = int(input("Enter your age: "))
Enter your age: 20
>>> if age >= 18: print("You are eligible to vote!")
You are eligible to vote!

>>> age = int(input("Enter your age: "))
Enter your age: 17
>>> if age >= 18: print("You are eligible to vote!")
[nothing happens] 
```

如您所见，这有点像带有不可见的`else`的 if-else 语句。如果`else`部分在那里，并且不满足条件，它就像“OK”。那就继续吧。”

## if 语句的 Else 和 Elif 构造有什么区别？

如果想有更多的潜在条件，可以使用一个`elif`语句。

下面是一个示例`elif`语句:

```
if x > y:
    print("x is greater than y")
elif x < y:
    print("x is less than y")
else:
    print("x is equal to y") 
```

你会注意到`elif`操作符出现在最初的`if`和`else`操作符之间。

还要注意，您可以使用任意多的`elif`。

```
if condition1:
    statement1
elif condition2:
    statement2
elif condition3:
    statement3
elif condition4:
    statement4
elif condition5:
    statement5
else
	statement6
```

## Python 中的 for else 和 while else 是什么？

您可以通过使用`for else`或`while else`语句将条件逻辑与循环结合起来。

下面是一个点击`break`并退出的示例`for else`语句:

```
for i in range(10):
    print(i)
    if i == 5:
        break
else:
    print("This code will only execute if the for loop completes without hitting a break statement.")

# this will output:
0
1
2
3
4
5 
```

这里是从一个更高的数字开始的相同的`for if`语句，它将跳过`break`事件并结束。看一下代码及其输出:

```
for i in range(6,10):
    print(i)
    if i == 5:
        break
else:
    print("This code will only execute if the for loop completes without hitting a break statement.")

# this will output:
6
7
8
9
This code will only execute if the for loop completes without hitting a break statement. 
```

## Python 中可以有多个 if 语句吗？

绝对的。您可以拥有任意数量的嵌套 if 语句。不过，要小心。这可能导致所谓的“末日金字塔”。

下面是一个嵌套 if 语句的示例:

```
if x == 5:
	if y == 10:
	    print("x is 5 and y is 10")
	else:
	    print("x is 5 and y is something else")
else:
	print("x is something else") 
```

注意有两个 if-else 语句，但是其中一个嵌套在另一个中。这对于一两层来说是可以的，但是很快就会变得混乱:

```
if x == 1:
    if y == 2:
        if z == 3:
            print("x, y, and z are all equal to 1, 2, and 3, respectively")
        else:
            print("x and y are equal to 1 and 2, respectively, but z is not equal to 3")
    else:
        print("x is equal to 1 but y is not equal to 2")
else:
    print("x is not equal to 1") 
```

## if 语句中可以有 3 个条件吗？

是的。但是如果你这样做，为了清晰起见，在你的语句中使用一些`elif`操作符可能是有意义的。

下面是一个包含 3 个条件的 if 语句示例:

```
if (condition1):
    # execute code1
elif (condition2):
    # execute code2
elif (condition3):
    # execute code3 
```

## `==`在 Python 中是什么意思？

Python `==`操作符——也称为相等操作符——是一个比较操作符，如果两个操作数(`==`左右两边的变量或值)相等，则返回 True。否则将返回 False。

这是制作 if 语句和其他条件逻辑的一个非常常见的工具。

学会它。知道吧。爱死了。

## 我希望你学到了很多关于 if 语句的知识。

我确实重温了我的 Python 知识，写了这篇教程。我希望这对你有所帮助。

如果你想学习更多关于 Python 编程和一般技术的知识，试试 [freeCodeCamp 的核心编码课程](https://www.freecodecamp.org/learn)。它是免费的。