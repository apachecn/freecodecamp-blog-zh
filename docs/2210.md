# 你应该知道的 C++关键词

> 原文：<https://www.freecodecamp.org/news/cpp-keywords-you-should-know/>

C++有各种各样的关键字，你应该知道它们是什么以及如何使用它们。所以在这篇文章中，我将谈论一些你会在这门语言中找到的最重要的关键词。

## C++中的关键字有哪些？

关键字是某些保留字，在 C++中有预定义的含义。因为关键字有自己的预定义含义，所以它们不能用作标识符(例如，函数名或变量名)。

下面的函数定义是错误的，因为 **friend** 是 c++中的一个关键字。

```
void friend(){
/*.......*/
}
```

让我们看看 C++中一些常见的关键字以及它们的用法。

## C++关键字

### C++中的关键字`typedef`

有时候，像这样声明某种类型变量可能会很乏味:

```
const unsigned char
```

程序员很难声明这样的变量，而且它们太长，不能经常使用。我们不能把它变短或者创造一些不同的东西吗？是的，我们可以。

使用 **`typedef`** 我们可以创建同义词:

```
typedef const unsigned char CON_UCHAR;
```

所以不要使用像这样的长基类型:

```
const unsigned char x;
```

我们可以用这个:

```
CON_UCHAR x;
```

类型定义也有助于指针声明:

```
typedef char *const CON_PTRCHAR; //const pointer to char

typedef const char* PTR_CONCHAR; //pointer to const char
```

### C++中的关键字`bool`

**bool** 是一个有两个值的类型名——它或者是**真**或者是**假。**

每一个非零值都是**真**、**而零是**假。****

```
if(1){
std::cout<<"Hello World"<<'\n';
}
else{
std::cout<<"Sorry World<<'\n';
}
```

由于所有非零值都为真，所以每次程序运行时，它都输出 **Hello World。**

记住两个布尔值，**真**和**假**，也是关键字。

### C++中的关键字`using`

```
using namespace std;
```

你可能在不知不觉中使用了关键字中的**。它可以这样使用:**

**:**

```
namespace My_space{

class My_class{
/*Your code here*/
};
}

namespace Her_space{

using My_space:: My_class;

}
```

这就是 using 声明的样子。using 声明将具有给定名称的每个声明都带入一个范围。

**:**

using 指令最常见的例子如下:

```
using namespace std;
```

上面的代码行使 std 名称空间中的每个名称都可用。

### C++中的`public`、`protected`和`private`关键字

关键字 **public** 、 **protected** 和 **private** 被用作类中的访问说明符。

```
class Home{
private:
int members;
protected:
double tot_expenditure;
public:
void display_detail();
};
```

**私有**标签后的成员只能通过成员函数访问。如果没有提供标签，那么默认情况下它是私有的。

**public** 标签后的成员随处可达。

**保护**标签后的成员类似于派生类的**公共**成员，类似于非派生类的**私有**成员。

### C++中的关键字`enum`

枚举是用户定义的类型。我们使用 **enum** 关键字声明枚举。

```
enum days{SUN, MON, TUE, WED, THU, FRI, SAT};
```

这里，太阳，星期一，TUE...称为枚举数，它们的值从 0 开始递增。

默认情况下，SUN==0，MON ==1，依此类推。

然而，我们也可以自己初始化枚举器。

```
enum{ a = 5, b = 6};
```

默认情况下，枚举被转换为整数以进行算术运算。由于枚举是*用户定义的类型*，我们也可以[为它们重载操作符](https://www.freecodecamp.org/news/how-to-overload-operators-in-cplusplus/)。

### C++中的关键字`new`

**new** 关键字(也是一个操作符)用于在自由存储(也称为堆)中创建对象。

```
int main(){

int *p = new int;

*p = 20;
//..

} 
```

对于上面的代码行，new 操作符分配一个内存来存储一个整数类型的对象，并返回一个指向该分配地址的指针。

### C++中的关键字`delete`

记忆对我们来说是一种重要的资源。所以我们应该明智地使用它。不需要的内存不应该被占用。所以 **delete** (也是一个操作符)用于释放之前由 **new** 操作符分配的内存。

```
int main(){

int *p = new int;
*p =20;

delete p;
//..

}
```

### C++中的关键字`this`

所有(非静态)成员函数都知道它们是为哪个对象调用的，它们可以使用指针 **this** 来引用它。

```
class A{
int b;
public:
//...
void display() const;
};
```

考虑一个有私有成员 b 和成员函数显示的类。

我们的显示功能如下:

```
void A::display() const{
cout<<"b = "<<b<<'\n';
}
```

上面同样的代码可以用 **this** 写成这样:

```
void A::display() const{
cout<<"b = "<<this->b<<'\n';
}
```

注意，**这个**是一个指针，所以我们应该使用- >操作符。此外**这里的**指的是调用函数的对象。

### C++中的关键字`class`

C++支持面向对象编程，所以我们有了类的概念。关键字 **class** 用于声明/定义一个类。

```
class Fb_user{
//..your code here
};
```

**struct** 也是一个关键字。默认情况下，这个类的所有成员都被标记为 public。

```
struct Fb_user{
//...Your code here
};
```

上述代码是该代码的简写:

```
class Fb_user{
public:
//..your code here
};
```

要了解更多关于类的信息，你可以参考我关于类的深度文章[这里](https://www.freecodecamp.org/news/how-classes-work-in-cplusplus/)。

### C++中的关键字`operator`

重载运算符时使用关键字**运算符**。重载运算符的语法如下:

```
return_type operator operator's_symbol(parameters){
//...Your code here...
}
```

要了解更多关于 c++中重载操作符的知识，你可以参考我的文章[这里](https://www.freecodecamp.org/news/how-to-overload-operators-in-cplusplus/)。

### C++中的关键字`inline`

关键字 **inline** 用于在每次调用中“内嵌”展开的函数。

在类定义中定义的成员函数被认为是内联函数。

但是我们可以使用关键字 **inline** 来内联一个成员函数，如下所示:

```
class Random(){
int a;
public:
int display const();
//...
};

inline int Random::display() const{
return a;
}
```

“内联”规范只是要求编译器内联一个函数。编译器可能会忽略该请求。

### C++中的关键字`goto`

C++也支持 **goto** 关键字。 **goto** 用作跳转语句，用于跳转进和跳出一个程序块。限制是我们不能跳入异常处理程序。

**goto** 语句对于从嵌套循环或任何 switch case 语句中跳出非常有用。

```
int main(){

for(int i = 0 ; i < 5 ; i++){
  for(int j = 0 ; j < 5 ; j++{
    if(){//check for some condition
    goto here;
    }
  }
}

here:
cout<<"I am here"<<;

}
```

所以基本上我们用 goto 从一个块跳到另一个块。

一般来说，避免使用 goto，是一个好主意，尽管它有时会很有用。

## 就是这样！

这些是 C++中最常见的一些关键字。我希望你能愉快地阅读这篇文章(不是这个指针:)

编码快乐！

你可以在这里阅读我的其他博客。