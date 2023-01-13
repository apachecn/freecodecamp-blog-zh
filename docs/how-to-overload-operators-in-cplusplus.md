# 如何在 c++中霸王操作符

> 原文：<https://www.freecodecamp.org/news/how-to-overload-operators-in-cplusplus/>

[类](https://www.freecodecamp.org/news/how-classes-work-in-cplusplus/)是用户定义的类型。它们允许我们表现各种实体的意义。为一个类定义一个操作符给了我们一个更好的处理对象的方法。

那么我们如何为我们的类定义操作符，我们应该如何使用这样的操作符呢？我将在本文中向您展示如何做到这一点。

我们开始吧！

## C++中的运算符有哪些？

运算符是用来对各种操作数执行运算的符号。例如:

```
int x = 5;
int y = 10;

int z = x + y;
```

对于上面的例子，`+`是一个对两个操作数`x`和`y`执行加法运算的运算符。

## C++中什么是运算符重载？

让我们先来看一个例子。

```
int x=5;
int y= 10;

int z = x+y;//z==15

string s1="Abhi";
string s2="gautam";

string s3= s1+s3;//s3==Abhigautam 
```

你有没有想过这种类型的代码**–**为什么是`z== 15`和`s3== Abhigautam`？这是因为运算符对于不同类型的操作数有不同的含义。

对于整数类型，`+`操作符给出两个数的和，对于字符串类型，它将它们连接起来。

因此，操作符重载就是赋予操作符新的含义。但是:

*   不能为内置类型的运算符设置新的含义。
*   您不能创建新的操作员。

所以，基本上我的意思是你不能重定义一个操作符，也不能创建一个新的操作符。

如果您希望创建像`**`这样的新操作符用于指数目的，您不能这样做。

### 重载是如何工作的？

因此，运算符重载让我们可以为用户定义的类型(例如，一个类就是一个用户定义的类型)的操作数定义现有运算符的含义(注意，您不能重载某些运算符)。

重载操作符只是带有特殊关键字`operator`的函数(但属于特殊类型),后跟要重载的操作符符号。

```
/*overloading + for class type object*/

return_type operator+(params..){}
```

正如我已经提到的，重载操作符只是一种特殊类型的函数。它们必须有一个返回类型，并且参数总是可选的(根据它们的需求)。

现在让我们为我们的类重载一些操作符，看看它是如何工作的:

```
class Complex{
int real,imag;
public:
Complex(int re,int im):real(re),imag(im){}
Complex(){
real = 0;
imag = 0;
}
void display() const;
//overloading operators
Complex operator+(const Complex);
Complex operator-(const Complex);
};
```

这里我们有两个函数作为成员函数，语法如上所述。所以首先让我们理解语法。

```
Complex operator+(const Complex);
Complex operator-(const Complex);
```

这里的两个函数都返回一个`Complex`类型的对象。后跟操作符符号的 operator 关键字告诉我们哪个操作符被重载了。

我们还有一个显示功能，允许我们看到对象成员值的显示。我们将在后面的文章中用重载操作符(`<<`)来代替它。

```
void Complex::display(){
if(imag<0)
cout<<real<<imag<<"i"<<'\n';
else
cout<<real<<'+'<<imag<<"i"<<'\n';
}
```

## 如何在 c++中霸王二进制加 _+)运算符

现在就让我们来霸王这个`+`操作员吧。

```
Complex Complex::operator+(const Complex c1){
Complex temp;
temp.real = real + c1.real;
temp.imag = imag + c1.imag;
return temp;
}
```

在这个定义之后，如果我们做以下事情:

```
Complex c1(2,2);
Complex c2(2,2);
Complex c3 = c1+c2;
c3.display();
```

应该清楚的是，c1+c2 等价于此:

```
c1.operator+(c2); 
```

和

```
operator(c1,c2);
```

调用成员函数显示后，输出如下:

```
4+4i
```

所以基本上我们为类型为`Complex`的对象定义了`+`操作符的含义。

## 如何在 c++中霸王二进制减 _-)运算符

现在让我们来支配减号运算符。

```
Complex Complex::operator-(const Complex c1){
Complex temp;
temp.real = real - c1.real;
temp.imag = imag - c1.imag;
return temp;
}
```

这就是我们在 c++中重载运算符的方式。现在让我们讨论函数中应该传递的参数数量。

传递给函数的参数数量等于运算符所采用的操作数数量。

但是在(非静态)成员函数的情况下，参数的数量减少了一个。这是因为(非静态)成员函数以某种方式知道它被调用的对象。

那不是很有趣吗？咱们现在为我们班霸王多操作员。

```
bool operator!=(const Complex);
bool operator==(const Complex);
```

## 怎么霸王了不等于 _！=)运算符

所以我们对`!=`运算符的函数定义是这样的:

```
bool Complex::operator!=(const Complex c1){
if(real!=c1.real || real!=c1.imag){
    return true;
}
else
return false;
}
```

返回类型是 bool，所以它返回 true 或 false。

## 如何在 c++中霸王等号 _==)运算符

操作员`==`也是如此:

```
bool Complex::operator==(const Complex c1){
  if(real == c1.real && imag == c1.imag){
    return true;
  }
  else
  return false;
}
```

### c++中如何霸王 get from _ <

所以现在让我们重载`<<`操作符。会很好玩的！

我们先来看看函数声明:

```
friend ostream& operator<<(ostream&,Complex); 
```

与之前的功能相比变化不大。让我们更清楚地理解它。

该函数是友元函数。这意味着它不在任何类的范围内，并且不能被对象调用。此外，该函数返回对 ostream 对象的引用，并采用两个参数作为参数:

*   对 ostream 对象的引用。
*   对类类型的对象的引用。

你可能想知道为什么朋友功能？先说说为什么现在需要好友功能。

如果一个运算符函数是一个(非静态)成员函数，那么左边的操作数将被绑定到指向调用该函数的对象的 **`this`** 指针。

但是我们不希望它出现在`<<`操作符的例子中，因为`<<`操作符的左侧操作数应该是`cout`。而`cout`是 ostream 的一个对象。因此，为了避免与对象绑定，我们在这里使用了一个友元函数。

我们可以像这样为我们的类定义`<<`操作符。

```
ostream& operator<<(ostream& os,Complex c1){
if(c1.imag<0)
os<<c1.real<<c1.imag<<"i";
else
os<<c1.real<<"+"<<c1.imag<<"i";

return os;
}
```

这样我们可以重载我们类的大多数操作符。

## 有些运算符在 C++中不能重载

在 c++中，我们不能支配以下运算符:

*   ::(范围解析运算符)
*   。(点运算符)
*   。*(通过指针选择成员)

> 它们将名称而不是值作为第二个操作数，并提供引用成员的主要方法。允许它们过载会导致微妙的问题。[斯特劳斯集团，1994 年]

此外，三元运算符(？:)和命名运算符 **sizeof** 和 **typeid** 也不能重载。

### 要记住的错误

还要记住，下面的声明是错误的:

```
int operator+(int,int);
/*error : cannot redefine operators for built in type.*/
```

如前所述，为内置类型重新定义运算符是错误的。

### 最后一点

你不应该重载像`&&`和`||`这样的操作符。这是因为这些运算符具有特定的操作数求值顺序。由于重载操作符只是函数调用，我们不能保证操作数的求值顺序。

## 就是这样！

编码快乐！

你可以在这里阅读我的其他博客。