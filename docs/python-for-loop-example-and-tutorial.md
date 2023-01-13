# Python For 循环–示例和教程

> 原文：<https://www.freecodecamp.org/news/python-for-loop-example-and-tutorial/>

循环让你控制程序的逻辑和流程结构。

具体来说，`for`循环让您一遍又一遍地执行一组类似的代码操作，直到满足某个条件。

对您确定的一组值重复某些代码指令，并对每个值执行预定次数的操作。

## Python 中的 for 循环是什么？

一个`for`循环可以遍历列表中的每一项，或者遍历字符串中的每一个字符，直到遍历完每一个字符才会停止。

遵循 DRY(不要重复自己)原则，编写循环有助于减少代码中的重复性。您不会多次编写相同的代码块。

在本文中，我们将通过不同的例子来了解 Python 编程语言中`for`循环的基础知识。但是首先让我们学习一些`for`循环的基础知识。

## 在其他编程语言中，for 循环是如何工作的？

在大多数现代编程语言中，如 JavaScript、Java 或 C，循环类似于下面的例子。

JavaScript 中的循环:

```
for (let i = 0; i < 10; i++) {
  console.log('Counting numbers');
  // prints "Counting numbers" 10 times
  // values of i from 0 to 9 
  } 
```

for 循环通常跟踪三件事:

1.  被执行一次的初始化表达式语句，`let i = 0;`
2.  需要满足的条件，`i < 10;`。该条件被评估为`true`或`false`。如果是`false`，循环终止。
3.  如果条件是`true`，循环体将被执行，初始化的表达式将采取一些行动。在这种情况下，它将增加 1 ( `i++`)，直到满足设定的条件。

## Python 中的 for 循环语法

与其他编程语言相比，Python 中的 for 循环看起来非常不同。

Python 以可读性为傲，所以它的`for`循环更干净、更简单、更紧凑。

基本结构是这样的:

```
for item in sequence:
    execute expression 
```

其中:

*   `for`开始一个`for`循环。
*   `item`是每次迭代中的一个单独的项目。它被赋予一个临时的任意变量名。
*   将每个项目与其他项目分开。
*   `sequence`就是我们要迭代的。
*   冒号`:`给出了执行后面代码的指令。
*   新的一行。
*   缩进的级别。4 个空格，否则我们得到一个`IndentationError`。
*   包含需要采取和重复的操作的正文(例如，将某些内容打印到控制台)。它去了`execute experssion`线所在的地方。

### Python for 循环的工作原理

假设我们有一个序列，一个我们想要遍历的存储项目列表——在这个例子中是一个杂货列表:

```
groceries = ["bananas","butter","cheese","toothpaste"] 
```

关键字`in`检查一个项目是否包含在一个序列中。当与关键字`for`结合使用时，它表示遍历序列中的每一项。

它对列表上的每一项都做了一些事情。在这种情况下，它会将每个单独的项目分别打印到控制台，直到每个项目都被迭代过为止。

```
for grocery in groceries:
    # for each iteration print the value of grocery
    print(grocery) 
```

`grocery`是一个临时变量名，用来引用列表中的每一项。

这是一个`iterator variable`，随着每次连续的迭代，它的值被设置为列表中包含的每个值。本质上，它是一个带有临时值的临时变量。

我们可以给它起任何我们想要的名字，比如`g`或者`item`。但是这个名字应该是唯一的，不能和程序中的其他变量相同。

第一次运行时，第一个元素——`bananas`——存储在变量`item`中。

然后是表达式`print(grocery)`，它本质上就是如何执行`print("bananas")`的。

在第二次运行时，元素`butter`被存储在变量`item`中，如上所述，它被打印到控制台。

这个过程一直持续到迭代完所有项目。

下面是这段代码的输出:

```
bananas
butter
cheese
toothpaste 
```

### 如何对一系列数字使用 for 循环

我们可以使用给定范围的`range()`函数来指定我们希望`for`循环在一行中迭代多少次。这简化了`for`循环。

`range()`函数根据我们给它的参数创建一个整数序列。

这是如何工作的？

看看下面的例子:

```
for i in range(5):
    print(i) 
```

其输出是:

```
0
1
2
3
4 
```

它创建一个介于 0 和 4 之间的数字列表。

默认情况下，当我们给`range()`一个参数时，范围从`0`开始计数。

注意`5`没有打印到控制台上。

在`range(5)`中，我们指定`5`是我们想要的最高数字，但是*不包括*。它不包括它，它只是停止点。它定义了我们希望循环运行的次数。我们看到它运行了 5 次，创建了一个包含 5 个项目的列表:`0,1,2,3,4`。

如果您想查看`range()`产生了什么用于调试，您可以将它传递给`list()`函数。

在控制台中打开交互式 Python shell，通常使用命令`python3`，并键入:

```
show_numbers = list(range(5))

print(show_numbers) 
```

如果我们希望我们的范围从 1 开始，然后也看到 5 打印到控制台上呢？相反，这次我们给出了两个不同的论点:

```
for i in range(1,6):
    print(i) 
```

输出:

```
1
2
3
4
5 
```

第一个参数(`start`)是可选的，它是序列应该开始的地方(在这个例子中是 1)。本参数*含*，含本数。

第二个参数(`stop`)是必需的，是序列应该结束的地方，并且*不包括*，如前所述。在这种情况下是 6。

最后，您可以传入第三个可选参数:`step`。

这控制了范围内两个值之间的*增量*。`step`的默认值为 1。

假设我们想每两个数跳一次，从一个序列中得到奇数。我们可以做:

```
for i in range(1,10,2):
    print(i) 
```

输出:

```
1
3
5
7
9 
```

`1`是我们开始的地方，`10`比我们想要的高 1(也就是 9)，`2`是我们想要在数字之间跳转的量(在这种情况下我们每两个数字跳转一次)。

### 如何在 Python 中使用 enumerate()

到目前为止，我们在迭代时没有使用任何索引。有时，我们需要访问正在循环的项目的索引并显示它。

我们可以使用`enumerate()`循环遍历带有索引的项目。

我们之前的例子:

```
groceries = ["bananas","butter","cheese","toothpaste"]

for grocery in groceries:
    print(grocery) 
```

现在可以这样写:

```
groceries = ["bananas","butter","cheese","toothpaste"]

for index, grocery in enumerate(groceries):
    print(index,grocery) 
```

输出:

```
0 bananas
1 butter
2 cheese
3 toothpaste 
```

或者对于稍微复杂一点的输出:

```
groceries = ["bananas","butter","cheese","toothpaste"]
for index, grocery in enumerate(groceries):
    print(f"Grocery: {grocery} is at index: {index}.") 
```

输出:

```
Grocery: bananas is at index: 0.
Grocery: butter is at index: 1.
Grocery: cheese is at index: 2.
Grocery: toothpaste is at index: 3 
```

*   不再像以前那样只写一个变量`grocery`，现在我们写两个:`index,grocery`。在每次迭代中，`index`包含值的索引，`grocery`包含`groceries`的值。
*   `index`是被迭代的值的索引。
*   Python 中的索引从`0`开始计数。
*   `grocery`是当前迭代中项目的值
*   第`enumerate(groceries)`行让我们遍历序列并跟踪值的索引和值本身。

## 结论

我希望你喜欢这篇关于 Python 中`for`循环的基本介绍。

我们讨论了构成`for`循环的基本语法以及它是如何工作的。

然后，我们简要解释了两个内置的 python 方法`range()`和`enumerate()`在`for`循环中的作用。

感谢阅读和快乐编码！