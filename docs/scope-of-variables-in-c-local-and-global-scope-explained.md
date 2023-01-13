# C #中的变量作用域——解释了局部和全局作用域

> 原文：<https://www.freecodecamp.org/news/scope-of-variables-in-c-local-and-global-scope-explained/>

在编程中，你经常需要处理变量的范围。变量的范围决定了您是否可以在特定的代码块中访问和修改它。

在本教程中，您将学习 C 编程语言中的变量作用域。您将看到一些代码示例来帮助您理解局部变量和全局变量之间的区别。

## 变量的范围是什么？

在继续学习局部和全局变量作用域之前，让我们先了解一下*作用域*是什么意思。

> 简单来说，变量的作用域就是它在程序中的*生存期*。

这意味着变量的作用域是整个程序中声明、使用和修改变量的代码块。

在下一节中，您将了解变量的局部范围

## C 嵌套块中变量的局部范围

在这一节中，您将学习 c 中局部变量的工作原理，首先编写几个例子，然后概括作用域原则。

这是第一个例子:

```
#include <stdio.h>

int main() 
{
    int my_num = 7;
    {
        //add 10 my_num
        my_num = my_num +10;
        //or my_num +=10 - more succinctly
        printf("my_num is %d",my_num);
    }

    return 0;
} 
```

我们来理解一下上面的程序是做什么的。

在 C 语言中，你用`{}`来分隔代码块。左花括号和右花括号分别表示一个块的开始和结束。

*   `main()`函数有一个整数变量`my_num`，它在*外部*块中被初始化为 7。
*   有一个*内部*块试图给变量`my_num`加 10。

现在，编译并运行上面的程序。以下是输出结果:

```
//Output

my_num is 17
```

您可以看到以下内容:

*   内部块能够访问外部块中声明的`my_num`的值，并通过给它加 7 来修改它。
*   `my_num`的值现在是 17，如输出所示。

## C 嵌套块中变量的局部范围示例 2

这里有另一个相关的例子:

```
#include <stdio.h>

int main() 
{
    int my_num = 7;
    {
        int new_num = 10;
    } 
    printf("new_num is %d",new_num); //this is line 9
    return 0;
}
```

*   在这个程序中，`main()`函数在*外*块中有一个整数变量`my_num`。
*   另一个变量`new_num`在*内部*块中初始化。内部块嵌套在外部块中。
*   我们试图访问并打印外部块*中内部块`new_num`的值。*

如果您尝试编译上面的代码，您会注意到它没有成功编译。您将得到以下错误消息:

```
Line   Message
9      error: 'new_num' undeclared (first use in this function)
```

> 这是因为变量`new_num`是在*内部*块中声明的，它的*范围*仅限于内部块。换句话说，它是内部块的本地块，不能从外部块访问。

基于以上观察，让我们写下以下变量局部作用域的一般原则:

```
{
	/*OUTER BLOCK*/

      {

        //contents of the outer block just before the start of this block
        //CAN be accessed here

        /*INNER BLOCK*/

      }

       //contents of the inner block are NOT accessible here
 }
```

## C 中变量的局部范围–不同的块

在前面的示例中，您了解了嵌套内部块中的变量如何不能从块外部访问。

在本节中，您将了解在不同块中声明的变量的局部范围。

```
#include <stdio.h>

int main()
{
    int my_num = 7;
    printf("%d",my_num);
    my_func();
    return 0;
}

void my_func()
{
    printf("%d",my_num);
}
```

在上面的例子中，

*   整数变量`my_num`在`main()`函数中声明。
*   在`main()`函数中，`my_num`的值被打印出来。
*   还有另一个函数`my_func()`试图访问并打印`my_num`的值。
*   当程序从`main()`函数开始执行时，在`main()`函数中有一个对`my_func()`的调用。

现在编译并运行上面的程序。您将得到以下错误消息:

```
Line   Message
13     error: 'my_num' undeclared (first use in this function)
```

如果你注意到，在`line 13`，函数`my_func()`试图访问在`main()`函数中声明并初始化的`my_num`变量。

> 因此，变量`my_num`的范围被限制在`main()`函数中，并被称为*对`main()`函数的局部*。

我们可以将局部作用域的概念概括地表示如下:

```
{

	/*BLOCK 1*/
    // contents of BLOCK 2 cannot be accessed here

}

{

	/*BLOCK 2*/
    // contents of BLOCK 1 cannot be accessed here

} 
```

## C #中变量的全局范围

到目前为止，您已经了解了 C 变量的局部范围。在这一节中，您将学习如何在 c 中声明全局变量。

先说个例子。

```
#include <stdio.h>
int my_num = 7;

int main()
{
    printf("my_num can be accessed from main() and its value is %d\n",my_num);
    //call my_func
    my_func();
    return 0;
}

void my_func()
{
  printf("my_num can be accessed from my_func() as well and its value is %d\n",my_num);
} 
```

在上面的例子中，

*   变量`my_num`在函数`main()`和`my_func()`之外声明。
*   我们尝试在`main()`函数中访问`my_num`，并打印它的值。
*   我们在`main()`函数内部调用函数`my_func()`。
*   函数`my_func()`也试图访问`my_num`的值，并将其打印出来。

该程序编译时没有任何错误，输出如下所示:

```
//Output
my_num can be accessed from main() and its value is 7
my_num can be accessed from my_func() as well and its value is 7
```

在本例中，有两个函数——`main()`和`my_func()`。

然而，变量`my_num`是*而不是程序中任何函数的局部*。对于任何函数来说，*不是局部*的变量被称为*全局*变量，称为*全局*变量。

变量的全局范围原则可以总结如下:

```
//all global variables are declared here
function1()
	{

    // all global variables can be accessed inside function1

    }
function2()
	{

    // all global variables can be accessed inside function2

    } 
```

## 包扎

在本教程中，您已经了解了局部作用域和全局作用域之间的区别。这是一个关于 c 语言中变量作用域的入门教程。

在 C #中，有一些访问修饰符来控制变量的访问级别。您可以在声明变量时使用相应的关键字来更改访问权限。

下节课再见。在那之前，编码快乐！