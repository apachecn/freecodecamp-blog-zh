# Python 中的 If、Elif 和 Else 语句

> 原文：<https://www.freecodecamp.org/news/if-elif-else-statements/>

## **If Elif Else 语句**

`if` / `elif` / `else`结构是控制程序流程的一种常见方式，允许您根据某些数据的值执行特定的代码块。

### 如果语句

如果关键字`if`后面的条件评估为`true`，代码块将执行。请注意，在其他语言中，条件检查前后不使用括号。

```
if True:
  print('If block will execute!')
```

```
x = 5

if x > 4:
  print("The condition was true!") #this statement executes
```

### else 语句

您可以选择添加一个在条件为`false`时执行的`else`响应:

```
if not True:
  print('If statement will execute!')
else:
  print('Else statement will execute!')
```

或者你也可以看这个例子:

```
y = 3

if y > 4:
  print("I won't print!") #this statement does not execute
else:
  print("The condition wasn't true!") #this statement executes
```

*注意，`else`关键字后面没有条件——它捕捉条件为`false`* 的所有情况

### elif 语句

通过在初始的`if`语句后包含一个或多个`elif`检查，可以检查多个条件。请记住，只有一个条件会执行:

```
z = 7

if z > 8:
  print("I won't print!") #this statement does not execute
elif z > 5:
  print("I will!") #this statement will execute
elif z > 6:
  print("I also won't print!") #this statement does not execute
else:
  print("Neither will I!") #this statement does not execute
```

*注意:只有评估为`true`的第一个条件才会执行。*即使`z > 6`是`true`，在第一个真条件后`if/elif/else`程序块终止。这意味着只有当所有条件都不为`true`时，`else`才会执行。

### 嵌套的 if 语句

我们还可以为决策创建嵌套的 if。在此之前，请参考 href = '[https://guide . freecodecamp . org/python/code-blocks-and-indentation](https://guide.freecodecamp.org/python/code-blocks-and-indentation)' target = ' _ blank ' rel = ' no follow '>缩排指南。

让我们举一个例子，找出一个大于 10 的偶数

```
python 
x = 34
if x %  2 == 0:  # this is how you create a comment and now, checking for even.
  if x > 10:
    print("This number is even and is greater than 10")
  else:
    print("This number is even, but not greater 10")
else:
  print ("The number is not even. So point checking further.")
```

这只是嵌套 if 的一个简单例子。请随时在线探索更多信息。

虽然上面的例子很简单，但是您可以使用[布尔比较](https://guide.freecodecamp.org/python/comparisons)和[布尔运算符](https://guide.freecodecamp.org/python/boolean-operations)来创建复杂的条件。

### 内联 python if-else 语句

我们还可以在 python 函数中使用 if-else 语句。下面的例子应该检查数字是否大于或等于 50，如果是则返回 True:

```
python 
x = 89
is_greater = True if x >= 50 else False

print(is_greater)
```

输出

```
>
True
>
```

## 有关 if/elif/else 语句的更多信息:

*   [如何走出 if/else 地狱](https://www.freecodecamp.org/news/so-youre-in-if-else-hell-here-s-how-to-get-out-of-it-fc6407fec0e/)
*   [JavaScript 中的 If/else](https://www.freecodecamp.org/news/javascript-essentials-how-to-make-life-decisions-with-if-else-statements-1908ff7cf5da/)