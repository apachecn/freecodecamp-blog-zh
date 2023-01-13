# 解释了 C - Integer、Floating Point 和 Void 中的数据类型

> 原文：<https://www.freecodecamp.org/news/data-types-in-c-integer-floating-point-and-void-explained/>

## C #中的数据类型

在 C 中有几种不同的方法来存储数据，并且它们都是互不相同的。可以存储信息的数据类型称为数据类型。与其他语言相比，c 语言对数据类型的宽容要少得多。因此，确保理解现有的数据类型、它们的能力和它们的局限性是很重要的。

C 的数据类型的一个怪癖是它们完全依赖于运行代码的硬件。笔记本电脑上的`int`会比超级计算机上的`int`小，所以了解你正在使用的硬件的局限性是很重要的。这也是数据类型被定义为最小值的原因——正如您将了解到的，一个`int`值至少是-32767 到 32767:在某些机器上，它将能够存储比这个值更多的值。

我们可以把它分为两类:整数和浮点数。整数是整数。它们可以是正数、负数或零。像-321、497、19345 和-976812 这样的数字都是完全有效的整数，但 4.5 不是，因为 4.5 不是一个整数。

浮点数是带小数的数字。像整数，-321，497，19345 和-976812 都是有效的，但现在 4.5，0.0004，-324.984 和其他非整数也是有效的。

c 允许我们在数据类型的几个不同选项中进行选择，因为它们在计算机上以不同的方式存储。因此，了解每种数据类型的能力和局限性以选择最合适的类型是很重要的。

## **整数数据类型**

### 字符:`char`

保存字符，如字母、标点和空格。在计算机中，字符存储为数字，因此`char`保存代表字符的整数值。实际的翻译由 ASCII 标准描述。[这里有一张](http://www.asciitable.com/)方便查找的表格。

与 C 中的其他数据类型一样，实际大小取决于您所使用的硬件。最低限度，它至少是 8 位，所以你至少会有 0 到 127。或者，您可以使用`signed char`获得至少-128 到 127。

### 标准整数:`int`

单个`int`占用的内存量取决于硬件。然而，你可以期望一个`int`至少有 16 位大小。这意味着它可以存储从-32，768 到 32，767 或更多的值，具体取决于硬件。

像所有这些其他数据类型一样，有一个可以使用的`unsigned`变量。`unsigned int`可以是正数和零，但不能是负数，因此它可以存储从 0 到 65，535 或更多的值，具体取决于硬件。

### 短整数:`short`

这并不经常使用，但知道它的存在是件好事。和 int 一样，可以存储-32768 到 32767。然而与 int 不同的是，这是它的能力范围。任何可以使用`short`的地方，都可以使用`int`。

### 更长的整数:`long`

`long`数据类型存储类似于`int`的整数，但是以占用更多内存为代价给出了更大范围的值。Long 至少存储 32 位，其范围为-2，147，483，648 到 2，147，483，647。或者，使用`unsigned long`，范围为 0 到 4，294，967，295。

### 更长的整数:`long long`

对于几乎所有的应用程序来说,`long long`数据类型都是多余的，但是 C 还是会让你使用它。它至少能够存储 9，223，372，036，854，775，807 到 9，223，372，036，854，775，807。或者，用`unsigned long long`获得更大的杀伤力，至少可以得到 0 到 18，446，744，073，709，551，615。

## **浮点数数据类型**

### 基本浮点数:`float`

`float`需要至少 32 位来存储，但是给我们 6 个小数位，从 1.2E-38 到 3.4E+38。

### 双打:`double`

`double`占用两倍的 float 内存(所以至少 64 位)。反过来，double 可以提供从 2.3E-308 到 1.7E+308 的 15 个小数位。

### 获得更大范围的替身:`long double`

`long double`至少需要 80 位。这样一来，我们就可以得到从 3.4E-4932 到 1.1E+4932 的 19 位小数。

## **选择正确的数据类型**

c 让我们选择数据类型，并让我们对我们做这件事的方式非常具体和有意识。这为您的代码提供了很大的权力，但是选择正确的代码是很重要的。

一般来说，你应该为你的任务选择最小值。如果你知道你要从整数 1 数到 10，你不需要长整型也不需要双精度型。如果你知道你永远不会有负值，考虑使用数据类型的`unsigned`变体。通过提供这种功能而不是自动完成，C 能够生成非常轻量和高效的代码。然而，作为程序员，您应该理解这些能力和局限性，并做出相应的选择。

我们可以使用 sizeof()操作符来检查变量的大小。有关各种数据类型的用法，请参见下面的 C 程序:

```
#include <stdio.h>

int main()
{
    int a = 1;

    char b ='G';

    double c = 3.14;

    printf("Hello World!\n");

    //printing the variables defined above along with their sizes
    printf("Hello! I am a character. My value is %c and "
           "my size is %lu byte.\n", b,sizeof(char));
    //can use sizeof(b) above as well

    printf("Hello! I am an integer. My value is %d and "
           "my size is %lu  bytes.\n", a,sizeof(int));
    //can use sizeof(a) above as well

    printf("Hello! I am a double floating point variable."
           " My value is %lf and my size is %lu bytes.\n",c,sizeof(double));
    //can use sizeof(c) above as well

    printf("Bye! See you soon. :)\n");
    return 0;
}
```

### 输出:

```
Hello World!Hello! I am a character. 
My value is G and my size is 1 byte.
Hello! I am an integer. 
My value is 1 and my size is 4 bytes.
Hello! I am a double floating point variable. 
My value is 3.140000 and my size is 8 bytes.
Bye! See you soon. :)
```

## **虚空型**

void 类型指定没有可用的值。它用于三种情况:

### 1.函数作为 void 返回

C 中有很多函数不返回任何值，或者你可以说它们返回 void。没有返回值的函数的返回类型为 void。例如，`void exit (int status);`

### 2.作为 void 的函数参数

C 语言中有许多不接受任何参数的函数。没有参数的函数可以接受空值。例如，`int rand(void);`

### 3.指向 void 的指针

void *类型的指针表示对象的地址，但不表示其类型。例如，内存分配函数`void *malloc( size_t size);`返回一个指向 void 的指针，该指针可以被转换为任何数据类型。