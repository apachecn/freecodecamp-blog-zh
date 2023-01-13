# C String——如何在 C 编程语言中声明字符串

> 原文：<https://www.freecodecamp.org/news/c-string-how-to-declare-strings-in-the-c-programming-language/>

计算机储存和处理各种数据。

字符串只是信息被呈现并被计算机处理的许多形式中的一种。

C 编程语言中的字符串工作方式与其他现代编程语言不同。

在本文中，您将学习如何在 c 中声明字符串。

在此之前，您将大致了解 c 中的数据类型、变量和数组，这样，您将理解在 c 中使用字符串时这些数据类型、变量和数组是如何相互联系的。

了解这些概念的基础将有助于您更好地理解如何在 c 中声明和使用字符串。

我们开始吧！

## C #中的数据类型

c 有一些内置的数据类型。

分别是`int`、`short`、`long`、`float`、`double`、`long double`和`char`。

如您所见，没有内置的 string 或 str(string 的缩写)数据类型。

### C 中的`char`数据类型

从您刚才看到的那些类型来看，在 C 中使用和表示字符的唯一方法是使用`char`数据类型。

使用`char`，你能够从你的计算机识别的 256 个字符中代表一个*单个*字符。它最常用于表示 ASCII 图表中的字符。

单个字符由*单引号*包围。

下面的例子都是`char`s——即使是被单引号和一个空格包围的数字在 C 中也是`char`:

```
'D', '!', '5', 'l', ' ' 
```

单引号包围的每个字母、符号、数字和空格都是 c 语言中的一段字符数据。

如果你想表现一个以上的角色呢？

下面的**不是**有效的`char`——尽管被单引号括起来。这是因为单引号中不仅仅包含一个字符:

`'freeCodeCamp is awesome'`

当许多单个字符串成一组时，就像你在上面看到的句子一样，就会创建一个*字符串*。在这种情况下，当你使用字符串，而不是单引号，你应该只使用*双*引号。

`"freeCodeCamp is awesome"`

## 如何在 C 中声明变量

到目前为止，您已经看到了文本在 c 中是如何表示的。

但是，如果你想在某个地方存储文本，会发生什么呢？毕竟，计算机确实擅长将信息保存到内存中，以供以后检索和使用。

在 C 和大多数编程语言中，存储数据的方式是在变量中。

本质上，你可以把变量想象成一个盒子，里面装着一个在程序生命周期中可以改变的值。变量在计算机的内存中分配空间，并让 C 知道你想保留一些空间。

c 是一种静态类型的语言，这意味着当你创建一个变量时，你必须指定这个变量的数据类型。

C 中有许多不同的变量类型，因为有许多不同种类的数据。

每个变量都有关联的数据类型。

当您创建一个变量时，您首先要提到变量的类型(它是保存整数、浮点、字符还是任何其他数据值)、它的名称，然后可选地为它赋值:

```
#include <stdio.h>
int main(void){

char letter = 'D';

//creates a variable named letter 
//it holds only values of type char
// the single character 'D' is assigned to letter
} 
```

在 C 语言中处理变量时，注意不要混合数据类型，因为这会导致错误。

举个例子，如果你试图将上面的例子改成使用双引号(记住只有 chars *使用单引号)，你会在编译代码时得到一个错误:*

```
#include <stdio.h>
int main(void){

char letter = "D";

//output:

test.c:4:6: warning: incompatible pointer to integer conversion initializing 'char' with an expression of type
      'char [2]' [-Wint-conversion]
char letter = "D";
     ^        ~~~
1 warning generated.

} 
```

如前所述，C 没有内置的字符串数据类型。这也意味着 C 没有字符串变量！

### 如何在 C 中创建数组

数组本质上是一个存储多个值的变量。它是许多相同类型项目的集合。

与常规变量一样，数组也有许多不同的类型，因为数组只能保存相同数据类型的项。有些数组只保存`int`和`float`，依此类推。

这就是你如何定义一个`ints`的数组，例如:

```
int numbers[3]; 
```

首先，指定数组将保存的项目的数据类型。然后你给它一个名字，在名字后面你还包括一对带有整数的方括号。整数表示数组的长度。

在上面的例子中，数组可以保存`3`值。

定义数组后，您可以使用索引，用方括号符号单独赋值。C 语言(和大多数编程语言)中的索引从`0`开始。

```
//Define the array; it can hold 3 values
int numbers[3];

//assign the 1st item of the numbers array the value of 1
int numbers[0] = 1;

//assign the 2nd item of the numbers array the value of 2
int numbers[1] = 2;

//assing the 3rd item of the numbers array the value of 3
int numbers[2] = 3; 
```

通过使用方括号中的数组名称和项目索引，可以从数组中引用和获取项目，如下所示:

```
numbers[2]; // returns the value 3 
```

## C 中的字符数组是什么？

那么，到目前为止提到的所有东西是如何组合在一起的，它与在 C 中初始化字符串并将其保存到内存中有什么关系？

嗯，C 语言中的字符串实际上是一种数组——具体来说，它们是一个`character array`。字符串是`char`值的集合。

### 字符串在 C 中是如何工作的

在 C 语言中，所有字符串都以`0`结尾。那个`0`让 C 知道一个字符串在哪里结束。

那个字符串终止零被称为**字符串终止符**。你也可能看到术语**零零**用于此，它有相同的意思。

不要将这个最后的零与数字整数`0`甚至字符`'0'`混淆——它们不是一回事。

在 c 语言中，字符串结束符会自动添加到每个字符串的末尾，但是我们看不到它——它总是在那里。

字符串终止符是这样表示的:`'\0'`。它与字符`'0'`的区别在于它有一个反斜杠。

在 C 语言中处理字符串时，想象它们总是以 *null zero* 结尾并在末尾有一个额外的字节是很有帮助的。

![Screenshot-2021-10-04-at-8.46.08-PM](img/1a03d55ebf808986a1c3a5c2b357e64b.png)

每个字符占用内存中的一个字节。

上图中的字符串`"hello"`，占用了`6 bytes`。

“Hello”有五个字母，每个字母占用 1 个字节的空间，然后空零也占用 1 个字节。

### C 语言中字符串的长度

C 语言中字符串的长度就是一个单词中的字符数，**不包括**包括字符串终止符(尽管它总是用来终止字符串)。

当您想要查找字符串的长度时，不考虑字符串终止符。

例如，字符串`freeCodeCamp`的长度为`12`个字符。

但是当计算一个字符串的长度时，你也必须计算所有的空格。

例如，字符串`I code`的长度为`6`个字符。`I`是 1 个字符，`code`有 4 个字符，然后还有 1 个空格。

所以一个字符串的长度，和它所拥有的字节数，以及它所占用的内存空间是不一样的。

### 如何在 C 中创建字符数组和初始化字符串

第一步是使用`char`数据类型。这让 C 知道你想创建一个数组来保存字符。

然后你给数组起一个名字，紧接着你要加上一对左右方括号。

在方括号内，您将包括一个整数。这个整数将是您希望字符串包含的最大字符数***，包括字符串结束符**。*

```
char city[7]; 
```

你可以像这样一次一个字符地初始化一个字符串:

```
#include <stdio.h>

int main(void) {
    char city[7];

    city[0] = 'A';
    city[1] = 't';
    city[2] = 'h';
    city[3] = 'e';
    city[4] = 'n';
    city[5] = 's';
    city[6] = '\0'; //don't forget this!

    printf("I live in %s",city);

} 
```

但这是相当耗时的。相反，当您第一次定义字符数组时，您可以选择使用双引号中的字符串直接为它赋值:

```
#include <stdio.h>
int main(void){

char city[7] = "Athens";

//defines a character array named city
//it can hold a string up to 7 characters INCLUDING the string terminator
//the value "Athens" is assigned when the character array is being defined

//this is how you print the character array value
printf("I live in %s",city);

} 
```

如果你愿意，你可以只给字符数组赋值，而不包括方括号中的数字。

它的工作方式与上面的例子完全一样。它将计算您提供的值中的字符数，并自动在末尾添加空零字符:

```
char city[] = "Athens";

//"Athens" has a length of 6 characters
//"Athens" takes up 7 bytes in memory,with the null zero included

/*
char city[7]  = "Athens";

is equal to 

char city[] = "Athens";

*/ 
```

记住，你总是需要为你想包含的最长的字符串保留足够的空间*加上字符串结束符*。

如果您想要更多的空间，需要更多的内存，并计划以后更改该值，请在方括号中包含一个更大的数字:

```
char city[15] = "Athens";

/* 
The city character array will now be able to hold 15 characters 
(including the null zero)

In this case, the remaining 8 (15 - 7) places will be empty

You'll be able to reassign a value up to 15 characters (including null zero as always)
*/ 
```

### 如何改变字符数组的内容

那么，你知道如何在 c 中初始化字符串，如果你想改变那个字符串呢？

您不能简单地使用赋值运算符(`=`)并为其赋值。您只能在第一次定义字符数组时这样做。

如前所述，从数组中访问一个项的方法是引用数组的名称和该项的索引号。

所以要改变一个字符串，你可以一个接一个地单独改变每个字符:

```
#include <stdio.h>

int main(void) {
    char city[7] = "Athens";

    printf("I live in %s",city);

    //changing each character individually means you have to use single quotation marks

    //the new value has to take up 7 bytes of memory

   //indexing starts at 0, the first character has an index of 0

    city[0] = 'L';
    city[1] = 'o';
    city[2] = 'n';
    city[3] = 'd';
    city[4] = 'o';
    city[5] = 'n';
    city[6] = '\0'; //DON'T FORGET THIS!

    printf("\nBut now I live in %s",city);

}
//output:
//I live in Athens
//But now I live in London 
```

不过，这种方法相当麻烦、耗时，而且容易出错。这绝对不是首选方式。

你可以使用代表`string copy`的`strcpy()`函数。

要使用这个函数，您必须在文件顶部的`#include <stdio.h>`行之后包含`#include <string.h>`行。

`<string.h>`文件提供了`strcpy()`功能。

当使用`strcpy()`时，首先包括字符数组的名称，然后是您想要分配的新值。`strcpy()`函数自动在创建的新字符串上添加字符串终止符:

```
#include <stdio.h>
#include <string.h>

int main(void) {
    char city[15] = "Athens";
    strcpy(city,"Barcelona");

    printf("I am going on holiday to %s",city);

    //output:
    //I am going on holiday to Barcelona
} 
```

## 结论

现在你知道了。现在你知道如何在 c 语言中声明字符串了。

总结一下:

*   c 没有内置的字符串函数。
*   要处理字符串，你必须使用字符数组。
*   当创建字符数组时，要为你想存储的最长的字符串留出足够的空间，还要考虑 c 语言中每个字符串末尾的字符串结束符。
*   要在字符数组中放置或更改字符串，您可以:
    *   定义数组，然后一次分配一个单独的字符元素。
    *   或者定义数组并同时初始化一个值。
*   当改变字符串的值时，可以在包含了`<string.h>`头文件后使用`strcpy()`函数。

如果你想学习更多关于 C 语言的知识，我为初学这门语言的人写了一个指南。

它基于 CS50 的计算机科学入门课程的前几周，我解释了一些基本概念，并回顾了这种语言在高层次上是如何工作的。

还可以在 freeCodeCamp 的 YouTube 频道上观看 [C 编程初学者教程](https://www.youtube.com/watch?v=KJgsSFOSQv0&t=14s)。

感谢阅读，快乐学习:)*