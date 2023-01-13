# Python 中的 Lambda 函数–示例语法

> 原文：<https://www.freecodecamp.org/news/lambda-function-in-python-example-syntax/>

Lambda 函数是匿名函数，只能包含一个表达式。

您可能认为 lambda 函数是一个中级或高级特性，但是在这里您将了解如何在代码中轻松使用它们。

在 Python 中，函数通常是这样创建的:

```
def my_func(a):
  # function body
```

您用关键字`def`声明它们，给它们一个名称，然后添加用圆括号括起来的参数列表。可以有很多行代码，里面有你需要的任意多的语句和表达式。

但有时您可能需要一个内部只有一个表达式的函数，例如一个双参数函数:

```
def double(x):
  return x*2
```

这是一个可以使用的函数，例如，与`map`方法一起使用。

```
def double(x):
  return x*2

my_list = [1, 2, 3, 4, 5, 6]
new_list = list(map(double, my_list))
print(new_list) # [2, 4, 6, 8, 10, 12]
```

这是使用 lambda 函数的好地方，因为它可以在你需要使用它的地方被创建。这意味着使用更少的代码行，并且可以避免创建只使用一次的命名函数(然后必须存储在内存中)。

## 如何在 Python 中使用 lambda 函数

当你在短时间内需要一个小函数时，你可以使用 lambda 函数——例如作为像`[map](/news/python-map-function-how-to-map-a-list-in-python-3-0-with-example-code/)`或`filter`这样的高阶函数的参数。

lambda 函数的语法是`lambda args: expression`。您首先编写单词`lambda`，然后是一个空格，接着是一个逗号分隔的所有参数的列表，接着是一个冒号，然后是作为函数体的表达式。

注意，你不能给 lambda 函数命名，因为根据定义，它们是匿名的(没有名字)。

lambda 函数可以有任意多的参数，但是函数体必须是一个表达式。

### 示例 1

例如，您可以编写一个 lambda 函数，将它的参数`lambda x: x*2`加倍，并与`map`函数一起使用，将列表中的所有元素加倍:

```
my_list = [1, 2, 3, 4, 5, 6]
new_list = list(map(lambda x: x*2, my_list))
print(new_list) # [2, 4, 6, 8, 10, 12]
```

注意这个函数和我们上面用`double`函数写的函数的区别。这个更紧凑，没有一个额外的函数占用内存空间。

### 示例 2

或者你可以写一个 lambda 函数来检查一个数字是否是正数，比如`lambda x: x > 0`，并用它和`filter`来创建一个只有正数的列表。

```
my_list = [18, -3, 5, 0, -1, 12]
new_list = list(filter(lambda x: x > 0, my_list))
print(new_list) # [18, 5, 12]
```

lambda 函数是在使用它的地方定义的，这样在内存中就没有一个已命名的函数。如果一个函数只在一个地方使用，使用 lambda 函数来避免混乱是有意义的。

### 示例 3

你也可以从一个函数返回一个 lambda 函数。

如果你需要创建多个数字相乘的函数，比如双倍或三倍等等，lambda 可以帮上忙。

您可以不创建多个函数，而是创建一个函数`multiplyBy`，如下所示，然后用不同的参数多次调用这个函数来创建 double、triple 等函数。

```
def muliplyBy (n):
  return lambda x: x*n

double = multiplyBy(2)
triple = muliplyBy(3)
times10 = multiplyBy(10)
```

lambda 函数从父函数获取值`n`，因此在`double`中，`n`的值为`2`，在`triple`中为`3`，在`times10`中为`10`。现在用一个参数调用这些函数将会成倍增加这个数字。

```
double(6)
> 12
triple(5)
> 15
times10(12)
> 120
```

如果你在这里没有使用 lambda 函数，你需要在`multiplyBy`中定义一个不同的函数，如下所示:

```
def muliplyBy (x):
  def temp (n):
    return x*n
  return temp
```

使用 lambda 函数可以使用一半的代码行，并且可读性更好。

## 结论

如果你的函数只包含一个小的表达式，Lambda 函数是一种编写函数的简洁方法。初学者通常不会使用它们，但是在这里您已经看到了如何在任何级别轻松使用它们。