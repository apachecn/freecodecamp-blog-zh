# Python 中的函数–用代码示例解释

> 原文：<https://www.freecodecamp.org/news/functions-in-python-a-beginners-guide/>

在任何编程语言中，函数都有助于*代码的重用*。简单来说，当你想重复做某件事的时候，你可以把那件事定义为一个函数，需要的时候就调用那个函数。

在本教程中，我们将学习 Python 中的用户自定义函数。

当你开始用 Python 编码时，你会在你的`Hello World!`程序中使用内置的`print()`函数😀以及用于读取用户输入的`input()`功能。

只要你知道如何使用这些函数，你就不必担心它们是如何实现的。

在编程中，这被称为*抽象。*它允许您通过调用带有必需参数的函数来使用函数，而不必担心它们实际上是如何工作的。

Python 中有大量的内置函数。在这篇文章中，我们将看到如何定义和使用我们自己的函数。我们开始吧！

## Python 函数语法

以下代码片段显示了在 Python 中定义函数的一般语法:

```
def function_name(parameters):
    # What the function does goes here
    return result 
```

*   您需要使用`def`关键字，为您的函数命名，后跟一对括号，并以冒号(:)结束该行。
*   如果您的函数有参数，那么在左括号和右括号中会提到参数的名称。
*   请注意，在*函数定义中，*函数消耗的参数被称为*参数*。
*   当您使用这些参数的特定值调用函数时，它们被称为*参数*或实际参数。这是因为*函数调用*中的*参数*是用于函数参数的值。
*   然后，你开始一个缩进块。这是描述你的函数做什么的函数体。
*   有一个`return`语句返回参数的运算结果。`return`语句将控制返回到最初调用函数的地方。

注意，`arguments`和`return`语句是可选的。这意味着你可以有一个函数不接受*的参数*，而*不返回任何东西*。😀

现在让我们用简单的例子来理解上面的陈述。

## 如何用 Python 创建一个简单的函数

现在让我们用 Python 创建一个简单的欢迎用户的函数，如下所示:

```
def my_func():
  print("Hello! Hope you're doing well") 
```

如你所见，函数`my_func()`:

*   不需要争论，
*   不返回任何内容，并且
*   每当*被称为*时，就打印出`"Hello! Hope you're doing well"`。

注意，上面的函数定义是惰性的，直到函数被触发或调用。让我们继续调用函数`my_func()`并检查输出。

```
my_func()

# Output
Hello! Hope you're doing well
```

## 如何在 Python 中创建带参数的函数

现在，我们将修改函数`my_func()`以包含用户的`name`和`place`。

```
def my_func(name,place):
  print(f"Hello {name}! Are you from {place}?")
```

我们现在可以通过为用户的`name`和`place`传入两个字符串来调用`my_func()`，如下所示。

```
my_func("Jane","Paris")

# Output
Hello Jane! Are you from Paris?
```

如果先指定`place`，再指定`name`，会发生什么？让我们找出答案。

```
my_func("Hawaii","Robert")

# Output
Hello Hawaii! Are you from Robert?
```

我们得到`Hello Hawaii! Are you from Robert?`——这没有意义。🙂是什么导致了这个问题？

函数调用中的参数是*位置参数*。这意味着函数调用中的第一个自变量被用作第一个参数(`name`)的值，函数调用中的第二个自变量被用作第二个参数(`place`)的值

请参见下面的代码片段。我们没有只指定参数，而是提到了参数和它们所取的值。

```
my_func(place="Hawaii",name="Robert")

# Output
Hello Robert! Are you from Hawaii?
```

这些被称为*关键字参数。*只要参数名称正确，函数调用中参数的顺序并不重要。

## 如何在 Python 中创建带有默认参数的函数

如果在函数调用过程中，我们有一些参数大部分时间都取特定的值，那会怎么样呢？

除了为一个特定的参数调用具有相同值的函数，我们能做得更好吗？

是的，我们可以做得更好，这就是`default arguments`的目的！😀

让我们创建一个函数`total_calc()`来帮助我们计算并打印出在一家餐馆要支付的总金额。给定一个`bill_amount`和您选择支付的`bill_amount`的百分比(`tip_perc`，该函数计算您应该支付的总金额。

注意函数定义是如何包含当用户没有指定小费百分比时要使用的参数`tip_perc`的默认值的。

运行下面的代码片段。👇🏽您现在已经准备好了您的函数！

```
def total_calc(bill_amount,tip_perc=10):
  total = bill_amount*(1 + 0.01*tip_perc)
  total = round(total,2)
  print(f"Please pay ${total}")
```

现在让我们以几种不同的方式调用这个函数。下面的代码片段显示了以下内容:

*   当您只使用`bill_amount`调用函数`total_calc`时，默认使用 10%的提示百分比。
*   当您显式指定百分比提示时，将使用函数调用中提到的提示百分比。

```
# specify only bill_amount
# default value of tip percentage is used

 total_calc(150)
 # Output
 Please pay $165.0

 # specify both bill_amount and a custom tip percentage
 # the tip percentage specified in the function call is used

 total_calc(200,15)
 # Output
 Please pay $230.0

 total_calc(167,7.5)
 # Output
 Please pay $179.53 
```

## 如何在 Python 中创建返回值的函数

到目前为止，我们只创建了可能带参数也可能不带参数并且不返回任何东西的函数。现在，让我们创建一个简单的函数，返回给定`length`、`width`和`height`的长方体的体积。

```
def volume_of_cuboid(length,breadth,height):
  return length*breadth*height 
```

回想一下，`return`关键字将控制返回到调用函数的地方。函数调用被替换为函数中的`return value`。

让我们用必要的维度参数调用函数`volume_of_cuboid()`，如下面的代码片段所示。请注意我们如何使用变量`volume`来捕获从函数返回的值。

```
volume = volume_of_cuboid(5.5,20,6)
print(f"Volume of the cuboid is {volume} cubic units")

# Output
Volume of the cuboid is 660.0 cubic units
```

## 如何在 Python 中创建返回多个值的函数

在我们之前的例子中，函数`volume_of_cuboid()`只返回一个值，即给定尺寸的长方体的体积。让我们看看如何从一个函数中返回多个值。

*   要从一个函数返回多个值，只需指定要返回的值，用逗号分隔。
*   默认情况下，该函数以元组的形式返回值。如果有`N`个返回值，我们得到一个`N-`元组。

让我们创建一个简单的函数`cube()`,在给定立方体边长的情况下，返回立方体的体积和总表面积。

```
def cube(side):
  volume = side **3
  surface_area = 6 * (side**2)
  return volume, surface_area
```

为了验证是否返回了一个元组，让我们将它收集到一个变量`returned_values`中，如下所示:

```
returned_values = cube(8)
print(returned_values)

# Output
(512, 384)
```

现在，我们将*解包元组*并将值存储在两个不同的变量中。

```
volume, area = cube(6.5)
print(f"Volume of the cube is {volume} cubic units and the total surface area is {area} sq. units")

# Outputs
Volume of the cube is 274.625 cubic units and the total surface area is 253.5 sq. units
```

## 如何在 Python 中创建接受可变数量参数的函数

让我们先问几个问题:

*   如果我们事先不知道参数的确切数目，那该怎么办？
*   我们能创建参数数量可变的函数吗？

答案是肯定的！我们会马上创建这样一个函数。

让我们创建一个简单的函数`my_var_sum()`，它返回作为参数传入的所有数字的总和。但是，每次调用函数时，参数的数量可能会有所不同。

注意函数定义现在有了`*args`而不仅仅是参数名。在函数体中，我们循环通过`args`,直到我们使用了所有的参数。函数`my_var_sum`返回作为参数传入的所有数字的总和。

```
def my_var_sum(*args):
  sum = 0
  for arg in args:
    sum += arg
  return sum
```

现在让我们每次用不同数量的参数调用函数`my_var_sum()`，并快速检查返回的答案是否正确！🙂

```
# Example 1 with 4 numbers
sum = my_var_sum(99,10,54,23)
print(f"The numbers that you have add up to {sum}")
# Output
The numbers that you have add up to 186

# Example 2 with 3 numbers
sum = my_var_sum(9,87,45)
print(f"The numbers that you have add up to {sum}")
# Output
The numbers that you have add up to 141

# Example 3 with 6 numbers
sum = my_var_sum(5,21,36,79,45,65)
print(f"The numbers that you have add up to {sum}")
# Output
The numbers that you have add up to 251
```

## ⌛简单回顾一下

让我们快速总结一下我们已经讲过的内容。在本教程中，我们学习了:

*   如何定义函数，
*   如何向函数传递参数，
*   如何创建带有默认和可变数量参数的函数，以及
*   如何创建具有返回值的函数？

希望你喜欢阅读这篇文章。感谢您的阅读。一如既往，直到下次！😀