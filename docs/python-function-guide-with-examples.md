# 带示例的 Python 函数指南

> 原文：<https://www.freecodecamp.org/news/python-function-guide-with-examples/>

## Python 中的函数简介

函数允许你定义一个可重用的代码块，它可以在你的程序中多次执行。

函数允许你为复杂的问题创建更多的模块和[干](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)的解决方案。

虽然 Python 已经提供了许多内置函数，比如`print()`和`len()`，但是您也可以定义自己的函数在项目中使用。

在代码中使用函数的一个很大的好处是，它减少了项目中代码的总行数。

### **语法**

在 Python 中，函数定义具有以下特性:

1.  关键词`def`
2.  函数名
3.  paranthesis '()'，并在 paranthesis 内输入参数，尽管这些输入参数是可选的。
4.  冒号':'
5.  一些要执行的代码块
6.  返回语句(可选)

```
# a function with no parameters or returned values
def sayHello():
  print("Hello!")

sayHello()  # calls the function, 'Hello!' is printed to the console

# a function with a parameter
def helloWithName(name):
  print("Hello " + name + "!")

helloWithName("Ada")  # calls the function, 'Hello Ada!' is printed to the console

# a function with multiple parameters with a return statement
def multiply(val1, val2):
  return val1 * val2

multiply(3, 5)  # prints 15 to the console
```

函数是代码块，只需调用函数就可以重用。这使得简单、优雅的代码重用成为可能，而无需显式地重写代码部分。这使得代码更具可读性，更易于调试，并减少了键入错误。

Python 中的函数是使用`def`关键字创建的，后跟括号内的函数名和函数参数。

一个函数总是返回一个值，函数使用关键字`return`来返回值，如果你不想返回任何值，默认值`None`将被返回。

函数名用于调用函数，在括号内传递所需的参数。

```
# this is a basic sum function
def sum(a, b):
  return a + b

result = sum(1, 2)
# result = 3
```

您可以为参数定义默认值，这样，如果没有给定，Python 会将该参数的值解释为默认值。

```
def sum(a, b=3):
  return a + b

result = sum(1)
# result = 4
```

您可以使用参数的名称，按照您想要的顺序传递参数。

```
result = sum(b=2, a=2)
# result = 4
```

但是，不可能在非关键字参数之前传递关键字参数

```
result = sum(3, b=2)
#result = 5
result2 = sum(b=2, 3)
#Will raise SyntaxError
```

函数也是对象，所以你可以把它们赋给一个变量，然后像函数一样使用这个变量。

```
s = sum
result = s(1, 2)
# result = 3
```

### **注释**

如果函数定义包含参数，则在调用函数时必须提供相同数量的参数。

```
print(multiply(3))  # TypeError: multiply() takes exactly 2 arguments (0 given)

print(multiply('a', 5))  # 'aaaaa' printed to the console

print(multiply('a', 'b'))  # TypeError: Python can't multiply two strings
```

函数将运行的代码块包括函数内缩进的所有语句。

```
def myFunc():
print('this will print')
print('so will this')

x = 7
# the assignment of x is not a part of the function since it is not indented
```

函数中定义的变量只存在于该函数的范围内。

```
def double(num):
x = num * 2
return x

print(x)  # error - x is not defined
print(double(4))  # prints 8
```

Python 只在调用函数时解释函数块，而不是在定义函数时解释。因此，即使函数定义块包含某种错误，python 解释器也只会在调用函数时指出来。

现在我们来看一些具体函数的例子。

## max()函数

`max()`是 Python 3 中的内置函数。它返回 iterable 中最大的项或两个或多个参数中最大的项。

### 争论

这个函数接受两个或更多的数字或任何类型的 iterable 作为参数。当给定一个 iterable 作为参数时，我们必须确保 iterable 中的所有元素都是同一类型的。这意味着我们不能传递一个既有字符串又有整数值的列表。语法:max(iterable，*iterables[，key，default]) max(arg1，arg2，*args[，key])

有效参数:

```
max(2, 3)
max([1, 2, 3])
max('a', 'b', 'c')
```

无效参数:

```
max(2, 'a')
max([1, 2, 3, 'a'])
max([])
```

### 返回值

返回 iterable 中最大的项。如果提供了两个或更多位置参数，则返回最大的位置参数。如果 iterable 为空并且没有提供 default，则引发一个`ValueError`。

### 代码示例

```
print(max(2, 3)) # Returns 3 as 3 is the largest of the two values
print(max(2, 3, 23)) # Returns 23 as 23 is the largest of all the values

list1 = [1, 2, 4, 5, 54]
print(max(list1)) # Returns 54 as 54 is the largest value in the list

list2 = ['a', 'b', 'c' ]
print(max(list2)) # Returns 'c' as 'c' is the largest in the list because c has ascii value larger then 'a' ,'b'.

list3 = [1, 2, 'abc', 'xyz']
print(max(list3)) # Gives TypeError as values in the list are of different type

#Fix the TypeError mentioned above first before moving on to next step

list4 = []
print(max(list4)) # Gives ValueError as the argument is empty
```

[运行代码](https://repl.it/CVok)

[正式文件](https://docs.python.org/3/library/functions.html#max)

## min()函数

`min()`是 Python 3 中的内置函数。它返回 iterable 中最小的项或两个或多个参数中最小的一个。

### 争论

这个函数接受两个或更多的数字或任何类型的 iterable 作为参数。当给定一个 iterable 作为参数时，我们必须确保 iterable 中的所有元素都是同一类型的。这意味着我们不能传递一个既有字符串又有整数值的列表。

有效参数:

```
min(2, 3)
min([1, 2, 3])
min('a', 'b', 'c')
```

无效参数:

```
min(2, 'a')
min([1, 2, 3, 'a'])
min([])
```

### 返回值

返回 iterable 中最小的项。如果提供了两个或更多位置参数，则返回最小的位置参数
。如果 iterable 为空且未提供默认值，则会引发 ValueError。

### 代码示例

```
print(min(2, 3)) # Returns 2 as 2 is the smallest of the two values
print(min(2, 3, -1)) # Returns -1 as -1 is the smallest of the two values

list1 = [1, 2, 4, 5, -54]
print(min(list1)) # Returns -54 as -54 is the smallest value in the list

list2 = ['a', 'b', 'c' ]
print(min(list2)) # Returns 'a' as 'a' is the smallest in the list in alphabetical order

list3 = [1, 2, 'abc', 'xyz']
print(min(list3)) # Gives TypeError as values in the list are of different type

#Fix the TypeError mentioned above first before moving on to next step

list4 = []
print(min(list4)) # Gives ValueError as the argument is empty
```

[运行代码](https://repl.it/CVir/4)

[正式文件](https://docs.python.org/3/library/functions.html#min)

# divmod()函数

`divmod()`是 Python 3 中的内置函数，返回数字`a`除以数字`b`时的商和余数。它以两个数为自变量`a` & `b`。参数不能是复数。

### 争吵

它需要两个参数`a`&`b`——一个整数，或者一个小数。不能是复数。

### 返回值

返回值将是由商和余数组成的正数对，商和余数通过将`a`除以`b`获得。在混合操作数类型的情况下，将应用二元算术运算符的规则。
对于 ****整数参数**** ，返回值与`(a // b, a % b)`相同。
对于 ****十进制数参数**** ，返回值将与`(q, a % b)`相同，其中`q`通常为****math . floor(a/b)****但也可能比其小 1。

### 代码示例

```
print(divmod(5,2)) # prints (2,1)
print(divmod(13.5,2.5)) # prints (5.0, 1.0)
q,r = divmod(13.5,2.5)  # Assigns q=quotient & r= remainder
print(q) # prints 5.0 because math.floor(13.5/2.5) = 5.0
print(r) # prints 1.0 because (13.5 % 2.5) = 1.0
```

[REPL 它！](https://repl.it/FGLK/0)

[正式文件](https://docs.python.org/3/library/functions.html#divmod)

## 十六进制函数

`hex(x)`是 Python 3 中的内置函数，将整数转换成小写的[十六进制的](https://www.mathsisfun.com/hexadecimals.html)字符串，前缀为“0x”。

### 争吵

此函数采用一个参数`x`，该参数应为整数类型。

### 返回

该函数返回一个以“0x”为前缀的小写十六进制字符串。

### 例子

```
print(hex(16))    # prints  0x10
print(hex(-298))  # prints -0x12a
print(hex(543))   # prints  0x21f
```

[运行代码](https://repl.it/CV0S)

[正式文件](https://docs.python.org/3/library/functions.html#hex)

## len()函数

`len()`是 Python 3 中的内置函数。这个方法返回一个对象的长度(项目的数量)。它需要一个参数`x`。

### 争论

它需要一个参数。该参数可以是序列(如字符串、字节、元组、列表或范围)或集合(如字典、集合或冻结集)。

### 返回值

该函数返回传递给`len()`函数的参数中元素的数量。

### 代码示例

```
list1 = [123, 'xyz', 'zara'] # list
print(len(list1)) # prints 3 as there are 3 elements in the list1

str1 = 'basketball' # string
print(len(str1)) # prints 10 as the str1 is made of 10 characters

tuple1 = (2, 3, 4, 5) # tuple 
print(len(tuple1)) # prints 4 as there are 4 elements in the tuple1

dict1 = {'name': 'John', 'age': 4, 'score': 45} # dictionary
print(len(dict1)) # prints 3 as there are 3 key and value pairs in the dict1
```

[运行代码](https://repl.it/CUmt/15)

[正式文件](https://docs.python.org/3/library/functions.html#len)

## **Ord 函数**

`ord()`是 Python 3 中的内置函数，将代表一个 Unicode 字符的字符串转换为代表该字符 Unicode 代码的整数。

#### **例子:**

```
>>> ord('d')
100
>>> ord('1')
49
```

## chr 功能

`chr()`是 Python 3 中的内置函数，将表示 Unicode 代码的整数转换成表示相应字符的字符串。

#### **例子:**

```
>>> chr(49)
'1'
```

需要注意的一点是，如果传递给`chr()`的整数值超出范围，则会引发 ValueError。

```
>>> chr(-10)
'Traceback (most recent call last):
  File "<pyshell#24>", line 1, in <module>
    chr(-1)
ValueError: chr() arg not in range(0x110000)'
```

## 输入()函数

很多时候，在程序中我们需要用户的输入。接受用户的输入使程序感觉是交互式的。在 Python 3 中，为了接受用户的输入，我们有一个函数`input()`。如果输入函数被调用，程序流程将被停止，直到用户给出一个输入并用返回键结束输入。让我们看一些例子:

当我们只想接受输入时:

# ****这只会给出一个提示，没有任何消息****

inp =输入()

[运行代码](https://repl.it/CUqX/0)

要给出带有消息的提示:

用消息提示*=输入(')*

# ****_****

# ****输出中的' _ '是提示****

[运行代码](https://repl.it/CUqX/1)

3.当我们需要一个整数输入时:

```
number = int(input('Please enter a number: '))
```

[运行代码](https://repl.it/CUqX/2)

如果你输入一个非整数值，Python 将抛出一个错误`ValueError`。 ****所以无论何时你使用这个，请确保你也抓住它。**** 否则，你的程序会在提示后意外停止。

```
number = int(input('Please enter a number: '))
# Please enter a number: as
# Enter a string and it will throw this error
# ValueError: invalid literal for int() with base 10 'as'
```

4.当我们需要一个字符串输入时:

```
string = str(input('Please enter a string: '))
```

[运行代码](https://repl.it/CUqX/3)

但是，默认情况下，输入存储为字符串。使用`str()`函数让代码阅读器清楚地知道输入将是一个“字符串”。一个好的做法是事先说明将采取哪种类型的输入。

[正式文件](https://docs.python.org/3/library/functions.html#input)

## 如何在 Python 中调用函数

函数定义语句不执行函数。执行(调用)一个函数是通过使用该函数的名称，后跟括起所需参数(如果有的话)的括号来完成的。

```
>>> def say_hello():
...     print('Hello')
...
>>> say_hello()
Hello
```

函数的执行引入了一个新的符号表，用于函数的局部变量。更准确地说，函数中的所有变量赋值都将值存储在局部符号表中；而变量引用首先在局部符号表中查找，然后在封闭函数的局部符号表中查找，然后在全局符号表中查找，最后在内置名称表中查找。因此，全局变量不能在函数中直接赋值(除非在全局语句中命名)，尽管它们可能被引用。

```
>>> a = 1
>>> b = 10
>>> def fn():
...     print(a)    # local a is not assigned, no enclosing function, global a referenced.
...     b = 20      # local b is assigned in the local symbol table for the function.
...     print(b)    # local b is referenced.
...
>>> fn()
1
20
>>> b               # global b is not changed by the function call.
10
```

函数调用的实际参数(自变量)在被调用函数被调用时被引入到被调用函数的局部符号表中；因此，使用 call by value 传递参数(其中值总是对象引用，而不是对象的值)。当一个函数调用另一个函数时，会为该调用创建一个新的局部符号表。

```
>>> def greet(s):
...     s = "Hello " + s    # s in local symbol table is reassigned.
...     print(s)
...
>>> person = "Bob"
>>> greet(person)
Hello Bob
>>> person                  # person used to call remains bound to original object, 'Bob'.
'Bob'
```

用于调用函数的参数不能被函数重新分配，但是引用可变对象的参数的值可以改变:

```
>>> def fn(arg):
...     arg.append(1)
...
>>> a = [1, 2, 3]
>>> fn(a)
>>> a
[1, 2, 3, 1]
```