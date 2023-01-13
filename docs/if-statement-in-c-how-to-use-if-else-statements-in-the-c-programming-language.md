# C 语言中的 If 语句–如何在 C 编程语言中使用 If-Else 语句

> 原文：<https://www.freecodecamp.org/news/if-statement-in-c-how-to-use-if-else-statements-in-the-c-programming-language/>

在 C 编程语言中，你有能力控制程序的流程。

特别是，程序能够决定下一步该做什么。这些决定是基于您设置的某些预定义条件的状态。

程序将根据条件是否满足来决定下一步应该做什么。

满足特定条件时做一件事，不满足特定条件时做另一件事的行为称为*控制流*。

例如，您可能希望仅在特定条件下执行某个操作。您可能想在完全不同的条件下执行另一个操作。或者，当您设置特定条件是*而不是*满足时，您可能想要执行另一个完全不同的动作。

为了能够做到以上所有，并控制程序的流程，你需要使用一个`if`语句。

在本文中，您将了解到关于`if`语句的所有内容——它的语法以及如何使用它的例子，这样您就可以理解它是如何工作的。

您还将了解到`if else`语句——也就是添加到`if`语句中的`else`语句，以增加程序的灵活性。

此外，您将了解到当您想要向条件中添加更多选择时的`else if`语句。

以下是我们将要介绍的内容:

1.  [C 中的`if`语句是什么？](#intro)
    1.  [如何在 C 语言中创建`if`语句](#syntax)
    2.  [什么是`if`陈述的例子？](#example)
2.  [C 中的`if else`语句是什么？](#else)
    1.  [什么是`if else`陈述的例子？](#else-example)
3.  [什么是`else if`语句？](#else-if)
    1.  [什么是`else if`陈述的例子？](#example-else-if)

## C 语言中的`if`语句是什么？

`if`语句也称为条件语句，用于决策。它就像一个岔路口或者一个分支。

条件语句根据检查或比较的结果采取特定的操作。

所以，总而言之，`if`语句是基于一个条件做出决定的。

该条件是一个布尔表达式。布尔表达式只能是两个值之一-真或假。

如果给定条件评估为`true`，则执行`if`块中的代码。

如果给定条件评估为`false`，则`if`块内的代码被忽略并跳过。

### 如何用 C 语言创建一个`if`语句——初学者的语法分析

C 语言中`if`语句的一般语法如下:

```
if (condition) {
  // run this code if condition is true
} 
```

让我们来分解一下:

*   使用关键字`if`开始一个`if`语句。
*   在圆括号中，您包含了一个需要检查和评估的条件，它总是一个布尔表达式。该条件将仅评估为`true`或`false`。
*   `if`块由一组花括号`{}`表示。
*   在`if`块中，有几行代码——确保代码是缩进的，这样更容易阅读。

### 什么是`if`陈述的例子？

接下来，我们来看一个`if`语句的实际例子。

我将创建一个名为`age`的变量来保存一个整数值。

然后，我会提示用户输入他们的年龄，并将答案存储在变量`age`中。

然后，我将创建一个条件，检查变量`age`中包含的值是否小于 18。

如果是这样，我希望在控制台上打印一条消息，让用户知道要继续，用户应该至少年满 18 岁。

```
#include <stdio.h>

int main(void) {
    // variable age
   int age;

   // prompt user to enter their age
   printf("Please enter your age: ");

   // store user's answer in the variable
   scanf("%i", &age);

    // check if age is less than 18
    // if it is, then and only then, print a message to the console

   if (age < 18) {
       printf("You need to be over 18 years old to continue\n");
   }
} 
```

我使用`gcc conditionals.c`编译代码，其中`gcc`是 C 编译器的名称，`conditionals.c`是包含 C 源代码的文件的名称。

然后，为了运行代码，我键入`./a.out`。

当被问及我的年龄时，我输入`16`，得到如下输出:

```
#output

Please enter your age: 16
You need to be over 18 years old to continue 
```

条件(`age < 18`)评估为`true`，因此`if`块中的代码执行。

然后，我重新编译并重新运行程序。

这一次，当询问我的年龄时，我输入`28`,得到如下输出:

```
#output

Please enter your age: 28 
```

良好的...没有输出。

这是因为条件评估为`false`，因此跳过了`if`块的主体。

我也没有具体说明在用户年龄大于 18 岁的情况下会发生什么。

我可以编写另一个`if`语句，如果用户的年龄大于 18 岁，该语句将向控制台打印一条消息，这样代码会更清楚一些:

```
#include <stdio.h>

int main(void) {
    // variable age
   int age;

   // prompt user to enter their age
   printf("Please enter your age: ");

   // store user's answer in the variable
   scanf("%i", &age);

    // check if age is less than 18
    // if it is, print a message to the console

   if (age < 18) {
       printf("You need to be over 18 years old to continue\n");
   }

   // check if age is greater than 18
   // if it is, print a message to the console

  if (age > 18) {
      printf("You are over 18 so you can continue \n");
  }

   } 
```

我编译并运行代码，当系统提示我的年龄时，我再次输入 28:

```
#output

Please enter your age: 28
You are over 18 so you can continue 
```

这个代码是有效的。也就是说，有一种更好的方式来编写它，您将在下一节中看到如何去做。

## C 语言中的`if else`语句是什么？

多个语句本身是没有帮助的——尤其是当程序变得越来越大的时候。

因此，出于这个原因，`if`语句伴随着`else`语句。

`if else`语句实质上意味着“`if`这个条件成立做下面的事情，`else`做这个事情代替”。

如果括号内的条件评估为`true`，那么`if`块内的代码将执行。然而，如果条件评估为`false`，那么`else`块中的代码将会执行。

`else`关键字是当`if`条件为假并且`if`块内的代码不运行时的解决方案。它提供了另一种选择。

一般语法如下所示:

```
if (condition) {
  // run this code if condition is true
} else {
  // if the condition above is false run this code
} 
```

### 什么是`if else`陈述的例子？

现在，让我们用前面的两个单独的`if`语句来回顾这个例子:

```
#include <stdio.h>

int main(void) {
   int age;

   printf("Please enter your age: ");

   scanf("%i", &age);

   if (age < 18) {
       printf("You need to be over 18 years old to continue\n");
   }
  if (age > 18) {
      printf("You are over 18 so you can continue \n");
  }

   } 
```

让我们用一个`if else`语句来重写它:

```
#include <stdio.h>

int main(void) {
   int age;

   printf("Please enter your age: ");

   scanf("%i", &age);

    // if the condition in the parentheses is true the code inside the curly braces will execute
    // otherwise it is skipped
    // and the code in the else block will execute

   if (age < 18) {
       printf("You need to be over 18 years old to continue\n");
   } else {
      printf("You are over 18 so you can continue \n");
  }

   } 
```

如果条件为`true`，则`if`块中的代码运行:

```
#output

Please enter your age: 14
You need to be over 18 years old to continue 
```

如果条件为`false`，则跳过`if`块中的代码，转而运行`else`块中的代码:

```
#output

Please enter your age: 45
You are over 18 so you can continue 
```

## 什么是`else if`语句？

但是，当您希望有多个条件可供选择时，会发生什么情况呢？

如果你希望在不止一个选项中做出选择，并希望有更多的动作变化，那么你可以引入一个`else if`语句。

一个`else if`语句本质上意味着“如果这个条件为真，执行以下操作。如果不是，请改为这样做。但是，如果以上都不是真的，其他都失败了，最后做这个。”

一般语法如下所示:

```
if (condition) {
   // if condition is true run this code
} else if(another_condition) {
   // if the above condition was false and this condition is true,
   // run the code in this block
} else {
   // if the two above conditions are false run this code
} 
```

### 什么是`else if`陈述的例子？

让我们看看`else if`语句是如何工作的。

假设您有以下示例:

```
#include <stdio.h>

int main(void) {
   int age;

   printf("Please enter your age: ");

   scanf("%i", &age);

   if (age < 18) {
       printf("You need to be over 18 years old to continue\n");
   }  else if (age < 21) {
       printf("You need to be over 21\n");
   } else {
      printf("You are over 18 and older than 21 so you can continue \n");
  }

   } 
```

如果第一个`if`语句为真，剩余的块将不会运行:

```
#output

Please enter your age: 17
You need to be over 18 years old to continue 
```

如果第一个`if`语句为假，则程序继续下一个条件。

如果这是真的，那么`else if`块中的代码执行，而块的其余部分不运行:

```
#output

Please enter your age: 20
You are need to be over 21 
```

如果前面的两个条件都为假，那么最后一步是执行`else`块:

```
#output

Please enter your age: 22
You are over 18 and older than 21 so you can continue 
```

## 结论

好了，现在你已经知道了 C 语言中`if`、`if else`和`else if`语句的基础！

我希望这篇文章对你有所帮助。

要了解关于 C 编程语言的更多信息，请查看以下免费资源:

*   [C 编程初学者教程](https://www.youtube.com/watch?v=KJgsSFOSQv0)
*   [什么是 C 编程语言？初学者教程](https://www.freecodecamp.org/news/what-is-the-c-programming-language-beginner-tutorial/)
*   [C 初学者手册:在几个小时内学会 C 编程语言基础知识](https://www.freecodecamp.org/news/the-c-beginners-handbook/)

非常感谢您的阅读和快乐编码:)