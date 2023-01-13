# 如何在 C 语言中用 Size of 运算符计算数组的大小

> 原文：<https://www.freecodecamp.org/news/how-to-find-the-size-of-an-array-in-c-with-the-sizeof-operator/>

用 C 编程语言编码时，有时可能需要知道数组的大小。

例如，当您想要遍历存储在数组中的所有元素以确定某个特定值是否存在时。

在本文中，您将学习如何使用`sizeof()`操作符找到数组的大小。

让我们开始吧！

## 什么是 C 编程语言中的数组？

数组允许您在同一个变量名下存储多个值。

C 编程语言中的数组是同一数据类型的项的集合。这意味着你可以创建一个只有整数值的数组或者一个由`char`组成的数组等等。

要在 C 中创建数组，首先需要指定数组将存储的值的数据类型。

然后，给数组起一个名字，后跟一对方括号`[]`。

在方括号内，您可以指定数组的大小。

所以，下面是你如何创建一个名为`faveNumbers`的`int`类型的数组来保存`5`整数:

```
int faveNumbers[5]; 
```

要在数组声明期间在数组内部插入值，请使用赋值运算符`=`和一对花括号`{}`。

在花括号内，输入项目并用逗号分隔每个项目:

```
int faveNumbers[5] = {7, 33, 13, 9, 29}; 
```

上面的代码创建了一个名为`faveNumbers`的数组，其中保存了`5`个整数、`7, 33, 13, 9, 29`。

您也可以编写如下代码:

```
int faveNumbers[] = {7, 33, 13, 9, 29}; 
```

在上面的例子中，我没有指定数组的大小。

然而，由于我用`5`元素对其进行了初始化，编译器可以判断出大小为`5`。

这里需要注意的是，一旦声明了数组，就不能改变它的大小和类型，因为它们的长度是固定的。

## 如何在 C 编程语言中找到数组的大小

c #没有提供一种内置的方法来获取数组的大小。

也就是说，它有内置的`sizeof`操作符，可以用来确定大小。

使用`sizeof`运算符的一般语法如下:

```
datatype size = sizeof(array_name) / sizeof(array_name[index]); 
```

让我们来分解一下:

*   `size`是存储数组大小的变量名，`datatype`指定存储在`size`中的数据值的类型。
*   `sizeof(array_name)`以字节为单位计算数组的大小。
*   `sizeof(array_name[index])`计算数组中一个元素的大小。

现在，让我们看看这个操作的实际情况，并把它分解成单独的步骤，看看它是如何工作的。

首先，`sizeof`操作符返回分配给数组的内存总量，以字节为单位。

```
#include <stdio.h>
int main() {
    // my array
    int faveNumbers[] = {7, 33, 13, 9, 29};

    // using sizeof to get the size of the array in bytes
    size_t size = sizeof(faveNumbers);

    printf("The size is %d bytes \n", size);
}

// output

// The size is 20 bytes 
```

然而，上面的代码并不直接计算数组的大小。

您将需要一些额外的编程逻辑，如下所示:

```
array_length = (total size of the array) / (size of the first element in the array) 
```

要计算数组的长度，需要用内存总量除以一个元素的大小——这种方法是可行的，因为数组存储相同类型的项。

因此，您可以用总字节数除以数组中第一个元素的大小。

要访问数组中的第一个元素，请指定名称，并在方括号中包含`0`。

在一般的编程和计算机科学中，索引总是从`0`开始，所以数组中的第一个元素的索引总是为`0`。

```
#include <stdio.h>
int main() {
    int faveNumbers[] = {7, 33, 13, 9, 29};

    size_t size = sizeof(faveNumbers) / sizeof(faveNumbers[0]);

    printf("The length of the array is %d \n", size);
}

// output

// The length of the array is 5 
```

这里需要注意的是，数据类型的大小因平台而异。

## 结论

希望本文能帮助您理解如何使用内置的`sizeof`操作符在 C 编程语言中找到数组的大小。

感谢您的阅读，祝您编码愉快！