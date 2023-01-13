# C++中的类如何工作

> 原文：<https://www.freecodecamp.org/news/how-classes-work-in-cplusplus/>

C++支持面向对象的编程，而类和对象是这种编程范式的核心。

你可能想知道——什么是类，我们为什么需要它们？在这篇文章中，我将回顾一些基础知识来帮助你理解 C++中的类是如何工作的。

## C++中的类如何工作

C++有各种各样的内置类型(比如`bool`、`int`、`floats`等等)。每种类型都有不同的特性(例如，内存占用的大小)。运算符对于不同的类型有不同的含义。

例如:运算符“+”将整数、浮点数和双精度数相加:

```
int x = 5;
int y = 6;

int z= x+y;//z==11
```

然而，如果我们对字符串使用'+'操作符，它会连接这些字符串。

```
string s1 = "abhilekh";
string s2 = "gautam";

string s3=s1+s2;//s3 will be abhilekhgautam
```

这里，x、y 和 z 表示整数，而 s1、s2 和 s3 表示字符串。所以 x，y 和 z 是类型为`int`的对象。同时，s1、s2 和 s3 是类型`string`的对象。

**注意:不要把这个和‘物体’这个词**混淆。一个对象就是任何占用内存的东西。

但是如果我们想要一个类型来表示我们日常生活中的对象呢？你怎么能代表房子、汽车、书籍、动物等等呢？这就是我们需要课程的原因。

**类是用户定义的类型**。这意味着您可以定义自己的类型。您可以创建自己的类型，如 int、floats 和 chars。您可以为您的类型定义运算符，并为您自己的类型设置各种属性。

类确实是一个强大的特性。让我们看看它们是如何工作的:

```
class Book{
string author_name,title;
int no_of_pages,edition_no;
//...
};
```

这里我们创建了一个名为 Book 的类。关键字 class 足以让我们理解这一点。

一本书必须有作者、标题和页数——这些是我们类的成员。这里，`book`表示具有这些特征的实体。

但是创建我们自己的类型是没有用的，除非我们有一个该类型的对象。那么我们该怎么做呢？

```
int x;//x is an object of type x.

Book y;//y is an object of type Book
```

这里唯一不同的是，x 是内置类型的对象(也就是`int`)，y 是类型 Book 的对象(Book 是自定义类型，由你定义)。

所以现在让我们讨论定义一个类的基础知识，如何创建该类的对象，以及使用这些对象的方法。

## C++中的成员函数有哪些？

在类中声明的函数是成员函数。它们只能被定义函数的类的对象调用。

现在让我们稍微更新一下我们的`Book`类:

```
class Book{
//..
public:
void update_edition(int edition);
//..
};
```

就是它——`update_edition`是我们类 book 的一个成员函数。我们可以用两种方式定义函数:类内和类外。

让我们看看它们是如何工作的:

```
/*Inside class declaration*/
class Book{
//..
public:
void update_edition(int edition){
//Define your function 
edition_no = edition;
}
};
```

```
/* Outside Class declaration*/
void Book::update_edition(int edition){
//Define your function here
//..
}
```

那么调用函数呢？

```
Book y;//y is an object of type Book

y.update_edition(5);
```

请记住，只有 Book 类型的对象可以调用该函数。从任何其他类型的对象调用将导致语法错误。

(非静态)成员函数总是知道它被调用的对象，并且可以像这样引用它:

```
void Book::update_edition(int edition){
this->edition_no = edition;
}
```

## C++中成员函数的类型

C++中有许多类型的成员函数。让我们在这里更详细地看一下每一个。

### 常数成员函数

```
class Book{
//..
public:
//..
int display_edition() const{
return edition_no;
}
//..
};
```

`const`关键字表示这个函数不改变函数的状态。

从常量成员函数更改函数的状态将导致错误。

```
int Book::book_edition() const{
edition_no = edition_no + 1;//error:cannot change value in constant function
}
```

### 静态成员函数

如果您有一个成员函数需要访问该类的成员，但不需要被该类的每个对象调用，那么您应该将其声明为 static。

这样的函数将是类的一部分，但不是对象的一部分。

```
class Book{
//..
public:
static void my_static_func();
};
```

可以在不使用对象的情况下引用静态成员。

```
void Book::my_static_func(){
//define here.
}
```

**静态**关键字不应该在函数声明中重复。静态成员函数不能访问 **`this`** 指针。

### 朋友函数

好友功能不在一个类的范围内，对 **`this`** 没有任何访问权限。

```
class Book{
//..
public:
//..
void check();
void display();
//..
};

class E_book{
//..
public:
//..
friend void Book::check();
};
```

这里，类`Book`的成员函数`check`是类`E_book`的朋友。

如果我们希望一个类的所有成员函数都是另一个类的朋友，我们可以使用下面的语法:

```
class E_book{
//...
public:
//..
friend class Book;
};
```

现在类`Book`的所有函数都是类`E-book`的朋友。

### 访问说明符

如果你有 C 语言背景，你可能记得使用关键字 **struct** 创建你自己的类型。

```
struct Book{
char author_name[20];
char title[20];
int no_of_page,edition_no;
};

Book b1;//b1 is an object of type Book
```

结构是用户定义的类型。事实上， **structure 是一种默认情况下所有成员都是公共的类。**迷茫？

你还记得我们在上面的代码片段中使用的 **`public:`** 标签吗？这就是我们所说的访问说明符。

那么私有访问说明符和公共访问说明符有什么区别呢？

标签后的成员被称为私有成员。这意味着它们只能被该类的成员函数访问。如果没有提供标签，默认情况下它是私有的。

另一方面，`public`标签之后的成员随处可访问。

这些说明符用于**数据隐藏**、**数据抽象、**和**数据封装**的目的。

### 构造器

构造函数是用来初始化对象的成员函数。每当创建该类类型的对象时，都会运行构造函数。

构造函数的名字与类名相同，也没有任何返回类型。

```
class Book{
//..
public:
//Constructor

Book(){  //no return type and name same as the class name

//define constructor here

}
};
```

一个类可以有多个构造函数，因为可以有多个同名函数被称为函数重载。

### 析构函数

析构函数释放被对象占用的内存。它只是破坏了物体。符号表示一个析构函数。

```
class Book{
//..
public:
//constructor
//..
//destructor
~Book(){
//destroy object here
}
};
```

## 就是这样！

通过在 c++中使用这些不同的类概念，你可以很容易地创建新的类型(你自己的类型),你可以很方便地使用它们作为内置类型。

你可以在这里阅读我的其他文章[。](https://abhilekhblogs.blogspot.com/)