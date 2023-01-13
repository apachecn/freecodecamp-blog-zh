# r 编程语言讲解

> 原文：<https://www.freecodecamp.org/news/r-programming-language-explained/>

r 是一种用于统计计算和图形的开源编程语言和软件环境。它是数据科学家和统计学家使用的主要语言之一。它得到了 R 统计计算基金会和一个大型开源开发者社区的支持。由于 R 使用了命令行界面，对于一些习惯于使用以 GUI 为中心的程序(如 SPSS 和 SAS)的人来说，学习曲线可能会很陡，因此对 R 的扩展(如 RStudio)可能非常有益。由于 R 是一个开源程序，可以免费获得，因此对那些通过与各种学院或大学的联系来管理对统计程序的访问的学者有很大的吸引力。

## **安装**

你需要开始使用 R 的第一件事是根据你的操作系统从它的官方网站下载它。

## **流行的 R 工具和包**

*   RStudio 是 r 的集成开发环境(IDE)。它包括一个控制台、支持直接代码执行的语法高亮编辑器，以及用于绘图、历史、调试和工作空间管理的工具。
*   [全面的 R 档案网络(CRAN)](https://cran.r-project.org/) 是 R 工具和资源的主要来源。
*   是一个自以为是的 R 包集合，为数据科学设计，如 ggplot2、dplyr、readr、tidyr、purr、tibble。
*   [data.table](https://github.com/Rdatatable/data.table/wiki) 是 base `data.frame`的一个实现，致力于提高性能和简洁、灵活的语法。
*   用于在 r 中构建仪表板风格的 web 应用程序的闪亮框架。

## R 中的数据类型

### 矢量

它是相同基本类型的数据元素序列。例如:

```
> o <- c(1,2,5.3,6,-2,4)                             	 # Numeric vector
> p <- c("one","two","three","four","five","six")    	 # Character vector
> q <- c(TRUE,TRUE,FALSE,TRUE,FALSE,TRUE)                # Logical vector
> o;p;q
[1]  1.0  2.0  5.3  6.0 -2.0  4.0
[1] "one"   "two"   "three" "four"  "five"  "six"
[1]  TRUE  TRUE FALSE  TRUE FALSE
```

### [数]矩阵

它是一个二维矩形数据集。矩阵中的组件也必须是相同的基本类型，如向量。例如:

```
> m = matrix( c('a','a','b','c','b','a'), nrow = 2, ncol = 3, byrow = TRUE)
> m
>[,1] [,2] [,3]
[1,] "a"  "a"  "b" 
[2,] "c"  "b"  "a"
```

### 数据帧

它比矩阵更通用，因为不同的列可以有不同的基本数据类型。例如:

```
> d <- c(1,2,3,4)
> e <- c("red", "white", "red", NA)
> f <- c(TRUE,TRUE,TRUE,FALSE)
> mydata <- data.frame(d,e,f)
> names(mydata) <- c("ID","Color","Passed")
> mydata
```

### 列表

它是一个 R 对象，里面可以包含许多不同类型的元素，比如向量、函数，甚至是另一个列表。例如:

```
> list1 <- list(c(2,5,3),21.3,sin)
> list1
[[1]]
[1] 2 5 3
[[2]]
[1] 21.3
[[3]]
function (x)  .Primitive("sin")
```

## R 中的函数

函数允许你定义一个可重用的代码块，它可以在你的程序中多次执行。

函数可以重复命名和调用，也可以就地匿名运行(类似于 python 中的 lambda 函数)。

充分理解 R 函数需要理解环境。环境只是管理对象的一种方式。实际环境的一个例子是，您可以在函数中使用冗余的变量名，如果更大的运行时已经有了相同的变量，这不会受到影响。此外，如果一个函数调用一个函数中没有定义的变量，它将检查该变量的更高级环境。

### **语法**

在 R 中，函数定义具有以下特性:

1.  关键词`function`
2.  函数名
3.  输入参数(可选)
4.  一些要执行的代码块
5.  返回语句(可选)

```
# a function with no parameters or returned values
sayHello() = function(){
  "Hello!"
}

sayHello()  # calls the function, 'Hello!' is printed to the console

# a function with a parameter
helloWithName = function(name){
  paste0("Hello, ", name, "!")
}

helloWithName("Ada")  # calls the function, 'Hello, Ada!' is printed to the console

# a function with multiple parameters with a return statement
multiply = function(val1, val2){
  val1 * val2
}

multiply(3, 5)  # prints 15 to the console
```

函数是代码块，只需调用函数就可以重用。这使得简单、优雅的代码重用成为可能，而无需显式地重写代码部分。这使得代码更具可读性，更易于调试，并减少了键入错误。

R 中的函数是使用关键字`function`创建的，圆括号内是函数名和函数参数。

函数可以使用`return()`函数返回值，通常用于强制提前终止返回值的函数。或者，该函数将返回最终的打印值。

```
# return a value explicitly or simply by printing
sum = function(a, b){
  c = a + b
  return(c)
}

sum = function(a, b){
  a + b
}

result = sum(1, 2)
# result = 3
```

您还可以为参数定义默认值，当函数调用期间没有指定变量时，R 将使用这些值。

```
sum = function(a, b = 3){
  a + b
}

result = sum(a = 1)
# result = 4
```

您也可以使用参数的名称，按照您想要的顺序传递参数。

```
result = sum(b=2, a=2)
# result = 4
```

r 还可以用“…”接受额外的可选参数

```
sum = function(a, b, ...){
  a + b + ...
}

sum(1, 2, 3) #returns 6
```

函数也可以匿名运行。这些功能与“应用”系列功能结合使用非常有用。

```
# loop through 1, 2, 3 - add 1 to each
sapply(1:3,
       function(i){
         i + 1
         })
```

### **注释**

如果函数定义包含没有指定默认值的参数，则必须包含这些值的值。

```
sum = function(a, b = 3){
a + b
}

sum(b = 2) # Error in sum(b = 2) : argument "a" is missing, with no default
```

函数中定义的变量只存在于该函数的范围内，但是如果没有指定变量，它将检查更大的环境

```
double = function(a){
a * 2
}

double(x)  # Error in double(x) : object 'x' not found

double = function(){
a * 2
}

a = 3
double() # 6
```

### R 中的内置函数

*   r 附带了许多函数，可以用来完成复杂的任务，比如随机采样。
*   例如，您可以用`round()`舍入一个数字，或者用`factorial()`计算它的阶乘。

```
> round(4.147)
[1] 4
> factorial(3)
[1] 6
> round(mean(1:6))
[1] 4
```

*   传递给函数的数据称为函数的参数。
*   您可以使用 R 的`sample()`功能模拟骰子的滚动。`sample()`函数有两个参数:一个名为 x 的向量和一个名为 size 的数字。例如:

```
> sample(x = 1:4, size = 2)
[] 4 2
> sample(x = die, size = 1)
[] 3
>dice <- sample(die, size = 2, replace = TRUE)
>dice
[1] 2 4
>sum(dice)
[1] 6
```

*   如果您不确定函数要使用哪些名称，可以使用 args 查找函数的参数。

```
> args(round)
[1] function(x, digits=0)
```

## **R 中的对象**

R 允许通过将数据存储在 R 对象中来保存数据。

### 什么是对象？

它只是一个名称，您可以使用它来调用存储的数据。例如，您可以将数据保存到像 a 或 b 这样的对象中。

```
> a <- 5
> a
[1] 5
```

### 如何在 R 中创建一个对象？

1.  要创建一个 R 对象，选择一个名称，然后使用小于号`<`，后跟一个减号`-`，将数据保存到其中。这个组合看起来像一个箭头，`<-`。r 将创建一个对象，给它起你的名字，并在里面存储箭头所指的内容。
2.  当你问 R a 里有什么，它会在下一行告诉你。例如:

```
> die <- 1:6
> die
[1] 1 2 3 4 5 6
```

1.  你可以给 R 中的一个对象起任何你想要的名字，但是有一些规则。首先，名字不能以数字开头。第二，名字不能使用一些特殊符号，比如`^, !, $, @, +, -, /, or *`:
2.  r 也懂大写(或者是区分大小写)，所以 name 和 Name 会引用不同的对象。
3.  您可以通过函数`ls()`查看已经使用的对象名称。

## 更多信息:

*   [通过这个关于统计编程的免费课程，在短短 2 小时内学习 R 编程语言基础知识](https://www.freecodecamp.org/news/r-programming-course/)
*   [使用 R 的网页抓取简介](https://www.freecodecamp.org/news/an-introduction-to-web-scraping-using-r-40284110c848/)
*   [R 中的聚合简介:处理数据的强大工具](https://www.freecodecamp.org/news/aggregates-in-r-one-of-the-most-powerful-tool-you-can-ask-for-4dd14eafff1f/)