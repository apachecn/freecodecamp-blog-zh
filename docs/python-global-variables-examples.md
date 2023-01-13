# Python 全局变量——如何定义全局变量示例

> 原文：<https://www.freecodecamp.org/news/python-global-variables-examples/>

在本文中，您将学习全局变量的基础知识。

首先，您将学习如何在 Python 中声明变量，以及术语“变量范围”的实际含义。

然后，您将学习局部变量和全局变量的区别，并了解如何定义全局变量以及如何使用`global`关键字。

以下是我们将要介绍的内容:

1.  [Python 中变量的介绍](#intro)
    1.  [变量范围解释](#scope)
2.  [如何创建具有局部作用域的变量](#local)
3.  [如何创建全局范围的变量](#global)
    1.  [`global`关键词](#keyword)

## Python 中的变量是什么，如何创建它们？初学者入门

你可以把变量想象成**存储容器**。

它们是存储容器，用于保存您希望保存在计算机内存中的数据、信息和值。然后，您可以在整个程序生命周期的某个时刻引用甚至操纵它们。

一个变量有一个符号化的**名**，你可以把这个名字想象成存储容器上的**标签**，作为它的标识符。

变量名将是存储在其中的数据的引用和指针。因此，没有必要记住数据和信息的细节——您只需要引用保存数据和信息的变量名。

当给一个变量命名时，确保它描述了它所保存的数据。变量名需要清晰易懂，无论是对你未来的自己，还是对你可能合作的其他开发人员。

现在，让我们看看如何在 Python 中实际创建一个变量。

在 Python 中声明变量时，不需要指定它们的数据类型。

例如，在 C 编程语言中，你必须明确地提到变量将保存的数据类型。

所以，如果你想存储你的年龄，这是一个整数，或者是`int`类型，这是你在 C 语言中必须做的:

```
#include <stdio.h>

int main(void)
{
  int age = 28;
  // 'int' is the data type
  // 'age' is the name 
  // 'age' is capable of holding integer values
  // positive/negative whole numbers or 0
  // '=' is the assignment operator
  // '28' is the value
} 
```

然而，这就是你如何用 Python 写上面的内容:

```
age = 28

#'age' is the variable name, or identifier
# '=' is the assignment operator
#'28' is the value assigned to the variable, so '28' is the value of 'age' 
```

变量名总是在左边，你要赋值的值在右边赋值操作符之后。

请记住，您可以在程序的整个生命周期中更改变量值:

```
my_age = 28

print(f"My age in 2022 is {my_age}.")

my_age = 29

print(f"My age in 2023 will be {my_age}.")

#output

#My age in 2022 is 28.
#My age in 2023 will be 29. 
```

您保留了相同的变量名`my_age`，但是只将值从`28`更改为`29`。

### Python 中的变量作用域是什么意思？

变量作用域是指 Python 程序中变量可用、可访问和可见的部分和边界。

Python 变量有四种类型的范围，也称为 **LEGB 规则**:

*   **L** ocal，
*   **E** 关闭，
*   **G** 叶状，
*   **B** 内置。

在本文的其余部分，您将重点学习如何创建具有全局作用域的变量，并且您将理解局部和全局变量作用域之间的区别。

## 如何在 Python 中创建具有局部作用域的变量

在函数体内定义的变量有*局部*作用域，这意味着它们只能在特定的函数内访问。换句话说，它们对于该功能是“局部的”。

您只能通过调用函数来访问局部变量。

```
def learn_to_code():
    #create local variable
    coding_website = "freeCodeCamp"
    print(f"The best place to learn to code is with {coding_website}!")

#call function
learn_to_code()

#output

#The best place to learn to code is with freeCodeCamp! 
```

看看当我试图从函数体外部用局部范围访问该变量时会发生什么:

```
def learn_to_code():
    #create local variable
    coding_website = "freeCodeCamp"
    print(f"The best place to learn to code is with {coding_website}!")

#try to print local variable 'coding_website' from outside the function
print(coding_website)

#output

#NameError: name 'coding_website' is not defined 
```

它引发了一个`NameError`,因为它在程序的其余部分是不可见的。它只在定义它的函数中“可见”。

## 如何在 Python 中创建全局范围的变量

当你在函数之外定义一个变量*时，比如在文件的顶部，它有一个全局范围，它被称为全局变量。*

从程序的任何地方都可以访问全局变量。

您可以在函数体内使用它，也可以从函数外部访问它:

```
#create a global variable
coding_website = "freeCodeCamp"

def learn_to_code():
    #access the variable 'coding_website' inside the function
    print(f"The best place to learn to code is with {coding_website}!")

#call the function
learn_to_code()

#access the variable 'coding_website' from outside the function
print(coding_website)

#output

#The best place to learn to code is with freeCodeCamp!
#freeCodeCamp 
```

如果有一个全局变量和一个局部变量，并且两者同名，会发生什么？

```
#global variable
city = "Athens"

def travel_plans():
    #local variable with the same name as the global variable
    city = "London"
    print(f"I want to visit {city} next year!")

#call function - this will output the value of local variable
travel_plans()

#reference global variable - this will output the value of global variable
print(f"I want to visit {city} next year!")

#output

#I want to visit London next year!
#I want to visit Athens next year! 
```

在上面的例子中，也许您并不期望得到那个特定的输出。

也许你认为当我在函数中赋予它不同的值时,`city`的值会改变。

也许您期望当我用行`print(f" I want to visit {city} next year!")`引用全局变量时，输出将是`#I want to visit London next year!`而不是`#I want to visit Athens next year!`。

然而，当函数被调用时，它打印局部变量的值。

然后，当我在函数外部引用全局变量时，分配给全局变量的值被打印出来。

他们互不干涉。

也就是说，对全局变量和局部变量使用相同的变量名并不是最佳实践。确保您的变量没有重名，因为当您运行程序时，可能会得到一些令人困惑的结果。

### 如何在 Python 中使用`global`关键字

如果你有一个全局变量，但想在函数中改变它的值，该怎么办？

看看当我尝试这样做时会发生什么:

```
#global variable
city = "Athens"

def travel_plans():
    #First, this is like when I tried to access the global variable defined outside the function. 
    # This works fine on its own, as you saw earlier on.
    print(f"I want to visit {city} next year!")

    #However, when I then try to re-assign a different value to the global variable 'city' from inside the function,
    #after trying to print it,
    #it will throw an error
    city = "London"
    print(f"I want to visit {city} next year!")

#call function
travel_plans()

#output

#UnboundLocalError: local variable 'city' referenced before assignment 
```

默认情况下，Python 认为你想在函数中使用局部变量。

所以，当我第一次尝试打印变量的值，然后*给我尝试访问的变量重新赋值时，Python 变得混乱了。*

改变函数中全局变量的值的方法是使用`global`关键字:

```
#global variable
city = "Athens"

#print value of global variable
print(f"I want to visit {city} next year!")

def travel_plans():
    global city
    #print initial value of global variable
    print(f"I want to visit {city} next year!")
    #assign a different value to global variable from within function
    city = "London"
    #print new value
    print(f"I want to visit {city} next year!")

#call function
travel_plans()

#print value of global variable
print(f"I want to visit {city} next year!") 
```

在函数中引用关键字之前使用关键字`global`，因为您将得到以下错误:`SyntaxError: name 'city' is used prior to global declaration`。

前面，您看到了您不能访问在函数内部创建的变量，因为它们有局部作用域。

关键字`global`改变函数中声明的变量的可见性。

```
def learn_to_code():
   global coding_website
   coding_website = "freeCodeCamp"
   print(f"The best place to learn to code is with {coding_website}!")

#call function
learn_to_code()

#access variable from within the function
print(coding_website)

#output

#The best place to learn to code is with freeCodeCamp!
#freeCodeCamp 
```

## 结论

现在你知道了！现在，您已经了解了 Python 中全局变量的基础知识，并且能够区分局部变量和全局变量。

我希望这篇文章对你有用。

要了解更多关于 Python 编程语言的知识，请查看 freeCodeCamp 的[带有 Python 认证的科学计算](https://www.freecodecamp.org/learn/scientific-computing-with-python/)。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢阅读和快乐编码！