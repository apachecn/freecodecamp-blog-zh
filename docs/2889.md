# 解释了 C 语言中的结构化数据类型

> 原文：<https://www.freecodecamp.org/news/structured-data-types-in-c-explained/>

C 中有不同数据类型的变量，比如`int` s、`char` s 和`float` s。它们让你存储数据。

我们用数组将相同数据类型的数据集合组合在一起。

但是在现实中，我们并不总是拥有仅仅一种类型的数据。这就是一个**结构**出现的原因。在本文中，我们将学习更多关于 c 语言中的结构化数据类型。

## 目录

**A .基础知识**

1.  [定义和声明](#1-definition-and-declaration)
2.  [初始化和访问结构成员](#2-initialization-and-accessing-the-members-of-a-structure)
3.  [用结构变量操作](#3-operating-with-structure-variable)
4.  [结构数组](#4-array-of-structure)
5.  [嵌套结构](#5-nested-structure)

**B .内存分配**

1.  [数据对齐](#1-data-alignment)
2.  [结构填充](#2-structure-padding)
3.  [结构构件对齐](#3-structure-member-alignment)
4.  [结构填料](#4-structure-packing)

**C .指针**

1.  [指针为成员](#1-pointer-as-a-member)
2.  [结构指针](#2-pointer-to-structure)
3.  [指针和数组结构](#3-pointer-and-array-of-structure)

**D .功能**

1.  [作为成员的功能](#1-function-as-a-member)
2.  [作为函数自变量的结构](#2-structure-as-a-function-argument)
3.  [结构作为函数返回](#3-structure-as-a-function-return)

e .自指结构

**F .结论**

我们开始吧，好吗？

## 基本原则

### 1.定义和声明

一个结构是一个或多个变量的**集合**，这些变量可能是不同类型的，分组在一个名字下。它是一种**用户自定义的**数据类型。

它们有助于在大型程序中组织复杂的数据，因为它们允许将一组逻辑上相关的变量视为一个整体。

例如，学生可以有姓名、年龄、性别和分数等属性。我们可以为`name`创建一个`char` acter 数组，为`roll`创建一个`int` eger 变量，为性别创建一个`char` acter 变量，为`marks`创建一个`int` eger 数组。

但如果有 20 或 100 名学生，就很难处理这些变量。

我们可以按照如下语法使用`struct`关键字声明一个结构:

```
 /* Syntax */
 struct structureName
        {
            dataType memberVariable1;
            datatype memberVariable2;
            ...
        };

 /* Example */
struct student
    {
        char name[20];
        int roll;
        char gender;
        int marks[5];
    }; 
```

上面的语句定义了一个新的数据类型`struct student`。该数据类型的每个变量将由`name[20]`、`roll`、`gender`和`marks[5]`组成。这些被称为结构的**成员**。

一旦一个结构被声明为一个新的数据类型，那么就可以创建该数据类型的变量。

```
 /* Variable declaration */
struct structureName structureVariable;

 /* Example /*
struct student st1;
struct student st2,st3,st4; 
```

`struct student`的每个变量都有其成员的副本。

**几个重要的金块:**

1.  在创建结构变量之前，结构的成员不会占用内存。
2.  你可能已经注意到我们也在使用`struct`作为变量声明。那不是很乏味吗？

在结构声明中使用`typedef`关键字，我们可以避免再次编写`struct`。

```
typedef struct students
    {
        char name[20];
        int roll;
        char gender;
        int marks[5];
    } STUDENT; 

/* or */
typedef struct
    {
        char name[20];
        int roll;
        char gender;
        int marks[5];
    } STUDENT; 

STUDENT st1,st2,st3,st4; 
```

按照惯例，**大写**字母用于*类型定义*(如`STUDENT`)。

3.结构定义和变量声明可以按如下方式组合。

```
struct student
    {
        char name[20];
        int roll;
        chat gender;
        int marks[5];
    }st1, st2, st3, st4; 
```

4.`structureName`的使用是可选的。下面的代码完全有效。

```
struct
    {
        char name[20];
        int roll;
        char gender;
        int marks[5];
    }st1, st2, st3, st4; 
```

5.结构通常是在源代码文件的顶部声明的，甚至是在定义函数之前(你会看到为什么)。

6.c 不允许在结构声明中初始化变量。

### 2.初始化和访问结构的成员

像任何其他变量一样，结构变量也可以在声明的地方初始化。成员和它们的初始值之间有一对一的关系。

```
 /* Variable Initialization */
struct structureName = { value1, value2,...};

 /* Example */
typedef struct
    {
        char name[20];
        int roll;
        char gender;
        int marks[5];
    }STUDENT;

void main(){
STUDENT st1 = { "Alex", 43, 'M', {76, 78, 56, 98, 92}};
STUDENT st2 = { "Max", 33, 'M', {87, 84, 82, 96, 78}};
} 
```

要访问成员，我们必须使用`.`(**点操作符**)。

```
 /* Accessing Memebers of a Structure */
structureVariable.memberVariable

/* Example */
 printf("Name: %s\n", st1.name);
 printf("Roll: %d\n", st1.roll);
 printf("Gender: %c\n", st1.gender);
 for( int i = 0; i < 5; i++)
   printf("Marks in %dth subject: %d\n", i, st1.marks[i]);

/* Output */
Name: Alex
Roll: 43
Gender: M
Marks in 0th subject: 76
Marks in 1th subject: 78
Marks in 2th subject: 56
Marks in 3th subject: 98
Marks in 4th subject: 92 
```

可以使用`.`以任何顺序在变量声明中初始化成员。

```
STUDENT st3 = { .gender = 'M', .roll = 23, .name = "Gasly", .marks = { 99, 45, 67, 78, 94}}; 
```

我们也可以初始化前几个成员，剩下的保持空白。然而，未初始化的成员应该是列表末尾的 **only** 。

未初始化的`int`整数和浮点数的默认值为`0`。对于`char`字符和字符串是`\0`(空)。

```
STUDENT st4 = { "Kviyat", 65};
 /* same as { "Kviyat", 65, '\0', { 0, 0, 0, 0, 0} } */ 
```

### 3.使用结构变量操作

和原始数据类型的变量一样，我们不能进行`+`、`-`、`*`、`/`等算术运算。同样，关系运算符和等式运算符不能与结构变量一起使用。

但是，我们可以将一个结构变量复制到另一个结构变量，只要它们属于同一个结构。

```
 /* Invalid Operations */
st1 + st2
st1 - st2
st1 == st2
st1 != st2 etc.

 /* Valid Operation */
st1 = st2 
```

我们将不得不单独比较结构成员来比较结构变量。

```
#include <stdio.h>
#include <string.h>

 struct student
    {
        char name[20];
        double roll;
        char gender;
        int marks[5];
    }st1,st2;

void main()
{
    struct student st1= { "Alex", 43, 'M', {76, 78, 56, 98, 92}};
    struct student st2 = { "Max", 33, 'M', {87, 84, 82, 96, 78}};

    if( strcmp(st1.name,st2.name) == 0 && st1.roll == st2.roll)
        printf("Both are the records of the same student.\n");
    else printf("Different records, different students.\n");

     /* Copiying the structure variable */
    st2 = st1;

    if( strcmp(st1.name,st2.name) == 0 && st1.roll == st2.roll)
        printf("\nBoth are the records of the same student.\n");
    else printf("\nDifferent records, different students.\n");
}

 /* Output */
Different records, different students.

Both are the records of the same student. 
```

### 4.结构的数组

您已经看到了我们如何创建 4 个不同的`struct student`类型变量来存储 4 个学生的记录。

更好的方法是创建一个`struct student`的数组(就像一个`int`的数组)。

```
struct student
    {
        char name[20];
        double roll;
        char gender;
        int marks[5];
    };

struct student stu[4]; 
```

为了访问数组`stu`的元素和每个元素的成员，我们可以使用循环。

```
 /* Taking values for the user */

for(int i = 0; i < 4; i++)
    {
        printf("Enter name:\n");
        scanf("%s",&stu[i].name);
        printf("Enter roll:\n");
        scanf("%d",&stu[i].roll);
        printf("Enter gender:\n");
        scanf(" %c",&stu[i].gender);

        for( int j = 0; j < 5; j++)
        {
            printf("Enter marks of %dth subject:\n",j);
            scanf("%d",&stu[i].marks[j]);
        }

        printf("\n-------------------\n\n");
    }

 /* Finding the average marks and printing it */

for(int i = 0; i < 4; i++)
    {
        float sum = 0;
        for( int j = 0; j < 5; j++)
        {
            sum += stu[i].marks[j];
        }

        printf("Name: %s\nAverage Marks = %.2f\n\n", stu[i].name,sum/5);
    } 
```

### 5.嵌套结构

嵌套结构意味着在另一个结构中有一个或多个结构变量。就像我们声明一个`int`成员或者`char`成员一样，我们也可以声明一个结构变量作为成员。

```
struct date
    {
       int date;
       int month;
       int year;
    };

struct student
    {
        char name[20];
        int roll;
        char gender;
        int marks[5];
        struct birth birthday;
    };

void main(){
 struct student stu1; 
```

类型为`struct birth`的结构变量`birthday`嵌套在`struct student`中。应该清楚的是**不能**在`struct student`中嵌套`struct student`类型的结构变量。

请注意，要嵌套的结构必须首先声明。使用`.`，我们可以访问内部结构中包含的成员以及其他成员。

```
 /* Example */
stu1.birthday.date
stu1.birthday.month
stu1.birthday.year
stu1.name 
```

不同类型的结构变量也可以嵌套。

```
struct birth
    {
       int date;
       int month;
       int year;
    };

struct relation
    {
        char fathersName[20];
        char mothersName[20];
    };

struct student
    {
        char name[20];
        int roll;
        char gender;
        int marks[5];
        struct birth birthday;
        struct relation parents; 
    }; 
```

## 存储器分配

当声明某种类型的结构变量时，结构成员被分配连续的内存位置。

```
struct student
    {
        char name[20];
        int roll;
        char gender;
        int marks[5];
    } stu1; 
```

在这里，内存将被分配给`name[20]`，然后是`roll`、`gender`和`marks[5]`。这意味着`st1`或`struct student`的大小将是其成员大小的总和，对吗？让我们检查一下。

```
void main()
{
    printf("Sum of the size of members = %I64d bytes\n", sizeof(stu1.name) + sizeof(stu1.roll) + sizeof(stu1.gender) + sizeof(stu1.marks));
    printf("Using sizeof() operator = %I64d bytes\n",sizeof(stu1));
}

 /* Output */
Sum of the size of members = 45 bytes
Using sizeof() operator = 48 bytes 
```

> 因为`sizeof()`操作符返回`long long unsigned int`，所以使用`%I64d`作为格式说明符。根据你的编译器，你可能需要使用`%llu`或者`%lld`。
> 
> 使用`%d`将给出警告-格式“%d”需要类型为“int”的参数，但参数 2 的类型为“long long unsigned int”。

使用`sizeof()`操作符给`3`的字节数比成员大小的总和还要多。为什么？这 3 个字节在内存的什么地方？

先回答第二个问题。我们可以打印成员的地址来找到那些`3`字节的地址。

```
void main()
{
    printf("Address of member name = %d\n", &stu1.name);
    printf("Address of member roll = %d\n", &stu1.roll);
    printf("Address of member gender = %d\n", &stu1.gender);
    printf("Address of member marks = %d\n", &stu1.marks);
}

 /* Output */
Address of member name = 4225408
Address of member roll = 4225428
Address of member gender = 4225432
Address of member marks = 4225436 
```

![fae6d1a3f7a5d79152e78239fa9257b4](img/774ca7674a91767f34ff181c2689083a.png)

我们可以看到数组`marks[5]`不是从`4225433`分配的，而是从`4224536`分配的。但是为什么呢？

### 1.数据调整

在研究数据对齐之前，了解处理器如何从存储器中读取数据非常重要。

处理器在一个周期内读取一个字。这个字对于一个 **32 位**处理器来说是 **4 字节**，对于一个 **64 位**处理器来说是 **8 字节**。周期数越低，CPU 的性能越好。

实现这一点的一个方法是通过**调整**数据。对齐意味着*大小为`t`的任何原始数据类型的变量将总是(默认情况下)拥有一个是`t`* 倍数的地址。这本质上是数据对齐。每次都是这样。

**某些数据类型的对齐地址**

| 数据类型 | 大小(字节) | 地址 |
| 茶 | one | 1 的倍数 |
| 短的 | Two | 2 的倍数 |
| int，float | four | 4 的倍数 |
| 双精度长整型，*(指针) | eight | 8 的倍数 |
| 长双份 | Sixteen | 16 的倍数 |

### 2.结构填充

您可能需要在结构的成员之间插入一些额外的字节来对齐数据。这些额外的字节被称为**填充**。

在上面的例子中，`3`字节充当了填充符。如果没有它们，类型为`int`(地址为 4 的倍数)的标记[0]的基址将为`4225433`(不是 4 的倍数)。

你现在可能明白为什么结构不能直接比较了。

![2992912eb4c1e6779b34d0a2ef4a9f63](img/b8527559c7ed9fc397da5e285c56f835.png)

### 3.结构成员对齐

为了解释这一点，我们将举另一个例子(你会明白为什么)。

```
struct example
    {
        int i1;
        double d1;
        char c1;

    } example1;

void main()
{
    printf("size = %I64d bytes\n",sizeof(example1));
} 
```

输出会是什么？让我们应用我们所知道的。

`i1`为 4 字节。因为`d1`的地址应该能被 8 整除，所以后面会有一个 4 字节的填充。

随后将分别为`d1`和`c1`发送 8 个和 1 个字节。因此，输出应该是 4 + 4 + 8 + 1 = 17 个字节。

```
 /* Output */
size = 24 bytes 
```

什么？又错了！怎么会？通过一个`struct example`的数组，我们可以更好的理解。我们还将打印出`example2[0]`成员的地址。

```
void main()
{
    struct example example2[2];
    printf("Address of example2[0].i1 = %d\n", &example2[0].i1);
    printf("Address of example2[0].d1 = %d\n", &example2[0].d1);
    printf("Address of example2[0].c1 = %d\n", &example2[0].c1);

}

 /* Output */
Address of example2[0].i1 = 4225408
Address of example2[0].d1 = 4225416
Address of example2[0].c1 = 4225424 
```

![4954ec331e15428bac5f3bf8eba31986](img/0f1151762b63003027db00eda2484205.png)

我们假设`example2[0]`的大小是 17 字节。这意味着`example2[1].i1`的地址将是`4225425`。这个**不可能是**，因为`int`的地址应该是 4 的倍数。

从逻辑上讲，`example2[1].i1`的可能地址似乎是`4225428`，是 4 的倍数。

这也是错误的。你知道为什么吗？现在`example2[1].d1`的地址将是(28 + 4 ( `i1` ) + 3(填充))`4225436`，它不是 8 的倍数。

为了避免这种不对齐，编译器对每个结构都引入了对齐。这是通过在最后一个成员后添加额外的字节来实现的，称为**结构成员对齐**。

在本节开始讨论的例子中，这不是必需的(这就是为什么我们需要这个例子)。

一个简单的记忆方法是通过这个规则:结构的地址和结构长度必须是`t_max`的倍数。这里，`t_max`是结构中一个构件所取的最大尺寸。

![5e3a7ca0bb894743995e2c7d8fec8bfe](img/8664f60fdc668cb4457c241b89dc3e44.png)

对于`struct example`，8 字节是`d1`的最大大小。因此，在结构的末尾有一个 7 字节的填充，使其大小为 24 字节。

**遵循这两条规则，你可以很容易地找到任何结构的大小:**

1.  任何数据类型都将其值存储在一个是其大小倍数的地址中。
2.  任何结构的大小都是成员最大字节数的倍数。

尽管我们能够降低 CPU 周期，但仍有大量内存被浪费。

将填充量减少到可能的最小值的一种方法是在**中声明成员变量，并按其大小**的降序排列。

如果我们在`struct example`中这样做，结构的大小减少到 16 字节。填充从 7 字节减少到 3 字节。

![b4e23f98514884e13ff86272c8b181cd](img/98839d780581b801d28981ecb19d2242.png)

```
struct example
    {
        double d1; 
        int i1;
        char c1;

    } example3;

void main()
{
    printf("size = %I64d bytes\n",sizeof(example3));
}

 /* Output */
size = 16 bytes 
```

### 4.结构包装

包装是填充的反义词。它防止编译器填充并移除未分配的内存。

在 Windows 的情况下，我们使用`#pragma pack`指令，它指定了结构成员的打包对齐。

```
#pragma pack(1)

struct example
    {
        double d1; 
        int i1;
        char c1;

    } example4;

void main()
{
    printf("size = %I64d bytes\n",sizeof(example4));
}

 /* Output */
size = 13 bytes 
```

![0d76bb850b7fc75f4091cd1dbe02cb87](img/73b56f056c8890a72ad749ba8165a584.png)

这确保了成员在 1 字节边界上*对齐*。换句话说，任何数据类型的地址必须是 1 字节或其大小的倍数(以较小者为准)。

## 两颗北极指极星

> 如果你想在继续之前温习一下指针，这里有一个[链接](https://www.freecodecamp.org/news/pointers-in-c-are-not-as-difficult-as-you-think/)指向一篇深入讨论指针的文章。

### 1.作为成员的指针

一个结构也可以有指针作为成员。

```
struct student
    {
        char *name;
        int *roll;
        char gender;
        int marks[5];
    };

void main()
{   int alexRoll = 44;
   struct student stu1 = { "Alex", &alexRoll, 'M', { 76, 78, 56, 98, 92 }};
} 
```

使用`.`(点操作符)，我们可以再次访问成员。由于`roll`现在有了`alexRoll`的地址，我们将不得不取消引用`stu1.roll`来获得值(而不是`stu1.(*roll)`)。

```
 printf("Name: %s\n", stu1.name);
   printf("Roll: %d\n", *(stu1.roll));
   printf("Gender: %c\n", stu1.gender);

   for( int i = 0; i < 5; i++)
    printf("Marks in %dth subject: %d\n", i, stu1.marks[i]);

 /* Output */
Name: Alex
Roll: 43
Gender: M
Marks in 0th subject: 76
Marks in 1th subject: 78
Marks in 2th subject: 56
Marks in 3th subject: 98
Marks in 4th subject: 92 
```

### 2.指向结构的指针

像整数指针、数组指针和函数指针一样，我们也有指向结构的指针或**结构指针**。

```
struct student {
    char name[20];
    int roll;
    char gender;
    int marks[5];
};

struct student stu1 = {"Alex", 43, 'M', {76, 98, 68, 87, 93}};

struct student *ptrStu1 = &stu1; 
```

这里，我们声明了一个类型为`struct student`的指针`ptrStu1`。我们已经把`stu1`的地址分配给了`ptrStu1`。

`ptrStu1`存储`stu1`的基址，是结构第一个成员的基址。增加 1 将使地址增加`sizeof(stu1)`字节。

```
printf("Address of structure = %d\n", ptrStu1);
printf("Adress of member `name` = %d\n", &stu1.name);
printf("Increment by 1 results in %d\n", ptrStu1 + 1);

/* Output */
Address of structure = 6421968
Adress of member 'name' = 6421968
Increment by 1 results in 6422016 
```

我们可以通过两种方式使用`ptrStu1`访问`stu1`的成员。使用`*`(间接运算符)或使用`->` ( **中缀或箭头运算符**)。

对于`*`，我们将继续使用`.`(点操作符)，而对于`->`，我们将不需要点操作符。

```
printf("Name w.o using ptrStu1 : %s\n", stu1.name);
printf("Name using ptrStu1 and * : %s\n", (*ptrStu1).name);
printf("Name using ptrStu1 and -> : %s\n", ptrStu1->name);

/* Output */
Name without using ptrStu1: Alex
Name using ptrStu1 and *: Alex
Name using ptrStu1 and ->: Alex 
```

同样，我们也可以访问和修改其他成员。注意，使用`*`时括号是必要的，因为点运算符(`.`)比`*`优先级高。

### 3.指针和结构数组

我们可以创建一个类型为`struct student`的数组，并使用指针来访问元素及其成员。

```
struct student stu[10];

 /* Pointer to the first element (structure) of the array */
struct student *ptrStu_type1 = stu;

 /* Pointer to an array of 10 struct student */
struct student (*ptrStu_type2)[10] = &stu; 
```

注意，`ptrStu_type1`是指向`stu[0]`的指针，而`ptrStu_type2`是指向 10 个`struct student`的整个数组的指针。给`ptrStu_type1`加 1 将指向`stu[1]`。

我们可以使用带有循环的`ptrStu_type1`来遍历元素及其成员。

```
for( int i = 0; i <  10; i++)
printf("%s, %d\n", ( ptrStu_type1 + i)->name, ( ptrStu_type1 + i)->roll); 
```

## 功能

### 1.作为成员发挥作用

函数**不能**成为一个结构的成员。然而，使用*函数指针*，我们可以使用`.`调用函数。请记住，不建议这样做。

```
 struct example
    {
        int i;
        void (*ptrMessage)(int i);

    };

void message(int);

void message(int i)
{
    printf("Hello, I'm a member of a structure. This structure also has an integer with value %d", i);
}

void main()
{
    struct example eg1 = {6, message};
    eg1.ptrMessage(eg1.i);
} 
```

我们已经声明了两个成员，一个`int` eger `i`和一个`struct example`内部的函数指针`ptrMessage`。函数指针指向一个采用`int` eger 并返回`void`的函数。

`message`就是这样一个函数。我们用`6`和`message`初始化了`eg1`。然后我们使用`.`调用使用`ptrMessage`的函数并传递`eg1.i`。

### 2.作为函数参数的结构

像变量一样，我们可以传递单个的**结构成员**作为参数。

```
#include <stdio.h>

struct student {
    char name[20];
    int roll;
    char gender;
    int marks[5];
};

void display(char a[], int b, char c, int marks[])
{
    printf("Name: %s\n", a);
    printf("Roll: %d\n", b);
    printf("Gender: %c\n", c);

    for(int i = 0; i < 5; i++)
        printf("Marks in %dth subject: %d\n",i,marks[i]);
}
void main()
{
    struct student stu1 = {"Alex", 43, 'M', {76, 98, 68, 87, 93}};
    display(stu1.name, stu1.roll, stu1.gender, stu1.marks);
}

 /* Output */
Name: Alex
Roll: 43
Gender: M
Marks in 0th subject: 76
Marks in 1th subject: 98
Marks in 2th subject: 68
Marks in 3th subject: 87
Marks in 4th subject: 93 
```

请注意，结构`struct student`是在`main()`的最顶端声明的。这是为了确保它是全球可用的，并且`display()`可以使用它。

如果该结构定义在`main()`内部，则其范围将被限制在`main()`内。

当有大量的结构成员时，传递结构成员是没有效率的。然后**结构变量**可以传递给一个函数。

```
void display(struct student a)
{
    printf("Name: %s\n", a.name);
    printf("Roll: %d\n", a.roll);
    printf("Gender: %c\n", a.gender);

    for(int i = 0; i < 5; i++)
        printf("Marks in %dth subject: %d\n",i,a.marks[i]);
}
void main()
{
    struct student stu1 = {"Alex", 43, 'M', {76, 98, 68, 87, 93}};
    display(stu1);
} 
```

如果结构的大小很大，那么传递它的副本就不会很有效率。我们可以将一个**结构指针**传递给一个函数。在这种情况下，该结构的地址作为一个*实际参数*被传递。

```
void display(struct student *p)
{
    printf("Name: %s\n", p->name);
    printf("Roll: %d\n", p->roll);
    printf("Gender: %c\n", p->gender);

    for(int i = 0; i < 5; i++)
        printf("Marks in %dth subject: %d\n",i,p->marks[i]);
}
void main()
{
    struct student stu1 = {"Alex", 43, 'M', {76, 98, 68, 87, 93}};
    struct student *ptrStu1 = &stu1;
    display(ptrStu1);
} 
```

将结构的**数组传递给函数类似于将任何类型的数组传递给函数。数组的名称是结构数组的基址，它被传递给函数。**

```
void display(struct student *p)
{   
    for( int j = 0; j < 10; j++)
   {
       printf("Name: %s\n", (p+j)->name);
        printf("Roll: %d\n", (p+j)->roll);
        printf("Gender: %c\n", (p+j)->gender);

        for(int i = 0; i < 5; i++)
        printf("Marks in %dth subject: %d\n",i,(p+j)->marks[i]);
   }
}

void main()
{
    struct student stu1[10];
    display(stu1);
} 
```

### 3.结构作为函数返回

我们可以返回一个**结构变量**，就像任何其他变量一样。

```
#include <stdio.h>

struct student {
    char name[20];
    int roll;
    char gender;
    int marks[5];
};

struct student increaseBy5(struct student p)
{
    for( int i =0; i < 5; i++)
        if(p.marks[i] + 5 <= 100)
           {
               p.marks[i]+=5;
           }
    return p;
}

void main()
{
    struct student stu1 = {"Alex", 43, 'M', {76, 98, 68, 87, 93}};
    stu1 = increaseBy5(stu1);

    printf("Name: %s\n", stu1.name);
    printf("Roll: %d\n", stu1.roll);
    printf("Gender: %c\n", stu1.gender);

    for(int i = 0; i < 5; i++)
        printf("Marks in %dth subject: %d\n",i,stu1.marks[i]);
}

 /* Output */
Name: Alex
Roll: 43
Gender: M
Marks in 0th subject: 81
Marks in 1th subject: 98
Marks in 2th subject: 73
Marks in 3th subject: 92
Marks in 4th subject: 98 
```

功能`increaseBy5()`为分数增加后小于或等于 100 分的受试者增加 5 分。注意，返回类型是一个类型为`struct student`的结构变量。

当返回一个**结构成员**时，返回类型必须是该成员的类型。

一个**结构指针**也可以由一个函数返回。

```
#include <stdio.h>
#include <stdlib.h>

struct rectangle {
    int length;
    int breadth;
};

struct rectangle* function(int length, int breadth)
{
    struct rectangle *p  = (struct rectangle *)malloc(sizeof(struct rectangle));
     p->length = length;
     p->breadth = breadth;
    return p;
}

void main()
{
    struct rectangle *rectangle1 = function(5,4);
    printf("Length of rectangle = %d units\n", rectangle1->length);
    printf("Breadth of rectangle = %d units\n", rectangle1->breadth);
    printf("Area of rectangle = %d square units\n", rectangle1->length * rectangle1->breadth);
}

 /* Output */
Length of rectangle = 5 units
Breadth of rectangle = 4 units
Area of rectangle = 20 square units 
```

注意，我们已经使用`malloc()`动态分配了大小为`struct rectangle`的内存。因为它返回一个 *void* 指针，我们必须*将*转换成一个`struct rectangle`指针。

## 自我参照结构

我们讨论过指针也可以是一个结构的成员。如果指针是结构指针呢？结构指针可以是与结构相同类型的**，也可以是与结构**不同类型的**。**

**自引用**结构是指那些拥有与其成员相同类型的结构指针的结构。

```
struct student {
    char name[20];
    int roll;
    char gender;
    int marks[5];
    struct student *next;
}; 
```

这是一个自引用结构，其中`next`是一个`struct student`类型的结构指针。

我们现在将创建两个结构变量`stu1`和`stu2`，并用值初始化它们。然后我们将把`stu2`的地址存储在`stu1`的`next`成员中。

```
void main()
{
    struct student stu1 = {"Alex", 43, 'M', {76, 98, 68, 87, 93}, NULL};
    struct student stu2 = { "Max", 33, 'M', {87, 84, 82, 96, 78}, NULL};
    stu1.next = &stu2;
} 
```

![d0e96dd633d93a28082bddbe932f70a8](img/9eafb40e6e60efdaa51e76f49fcfcb3b.png)

我们现在可以使用`stu1`和`next`来访问`stu2`的成员。

```
void main()
{
    printf("Name: %s\n", stu1.next->name);
    printf("Roll: %d\n", stu1.next->roll);
    printf("Gender: %c\n", stu1.next->gender);

    for(int i = 0; i < 5; i++)
        printf("Marks in %dth subject: %d\n",i,stu1.next->marks[i]);
}

 /* Output */
Name: Max
Roll: 33
Gender: M
Marks in 0th subject: 87
Marks in 1th subject: 84
Marks in 2th subject: 82
Marks in 3th subject: 96
Marks in 4th subject: 78 
```

假设我们希望在`stu1`之后有一个不同的结构变量，即*在`stu1`和`stu2`* 之间插入另一个结构变量。这很容易做到。

```
void main()
{
    struct student stuBetween = { "Gasly", 23, 'M', {83, 64, 88, 79, 91}, NULL};
    st1.next = &stuBetween;
    stuBetween.next = &stu2;
} 
```

现在`stu1.next`存储了`stuBetween`的地址。而`stuBetween.next`有`stu2`的地址。我们现在可以使用`stu1`访问所有三个结构。

```
 printf("Roll Of %s: %d\n", stu1.next->name, stu1.next->roll);
    printf("Gender Of %s: %c\n", stu1.next->next->name, stu1.next->next->gender);

 /* Output */
Roll Of Gasly: 23
Gender Of Max: M 
```

![270110c92242e31902924c84c008d489](img/0e2ba109f7418219dae6ec1202cab02a.png)

注意我们是如何在`stu1`、`stuBetween`和`stu3`之间建立联系的。我们这里讨论的是一个*链表*的起点。

自引用结构在创建数据结构时非常有用，比如*链表*、*栈*、*队列*、*图*等等。

## 结论

搞定了。我们已经涵盖了从结构的定义到自指结构的使用的所有内容。

试着复述你读过的所有子话题。如果你能回忆起它们，干得好！把记不住的再看一遍。

下一个合乎逻辑的步骤是学习更多关于链表和这里使用的各种其他数据结构的知识。

不断学习。呆在家里，注意安全。