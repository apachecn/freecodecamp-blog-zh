# C 中的 malloc:解释 C 中的动态内存分配

> 原文：<https://www.freecodecamp.org/news/malloc-in-c-dynamic-memory-allocation-in-c-explained/>

# **C 中的 malloc()是什么？**

malloc()是一个库函数，允许 C 从堆中动态分配内存。堆是存储某些东西的内存区域。

malloc()是 stdlib.h 的一部分，为了能够使用它，您需要使用`#include <stdlib.h>`。

## **如何使用 Malloc**

malloc()分配所请求大小的内存，并返回一个指向所分配块开头的指针。为了保存这个返回的指针，我们必须创建一个变量。指针应该与 malloc 语句中使用的类型相同。这里我们将创建一个指向即将到来的整型数组的指针

```
int* arrayPtr;
```

与其他语言不同，C 不知道它分配内存的数据类型；它需要被告知。幸运的是，C 有一个我们可以使用的函数`sizeof()`。

```
arrayPtr = (int *)malloc(10 * sizeof(int));
```

该语句使用 malloc 为 10 个整数的数组留出内存。由于不同计算机之间的大小会有所不同，因此使用 sizeof()函数来计算当前计算机上的大小非常重要。

程序执行期间分配的任何内存都需要在程序关闭前释放。为了`free`记忆，我们可以使用 free()函数

```
free( arrayPtr );
```

该语句将释放以前分配的内存。c 不像其他一些语言，比如 Java，有一个`garbage collector`。因此，在程序关闭后，没有正确释放的内存将继续被分配。

# **在你继续之前……**

## **一次回顾**

*   Malloc 用于动态内存分配，当您不知道编译时所需的内存量时非常有用。
*   分配内存允许对象存在于当前块的范围之外。
*   c #通过值而不是引用传递。使用 malloc 分配内存，然后将指针传递给另一个函数，这比让该函数重新创建结构更有效。

## 关于 C 编程的更多信息:

*   [C 编程初学者手册](https://www.freecodecamp.org/news/the-c-beginners-handbook/)
*   [如果...C 中的 else 语句讲解](https://www.freecodecamp.org/news/if-statements-in-c/)
*   [C 中的三元运算符讲解](https://www.freecodecamp.org/news/c-ternary-operator/)