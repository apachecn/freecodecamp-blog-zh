# 如何在 C++中将 Int 转换成 String 整数转换教程

> 原文：<https://www.freecodecamp.org/news/how-to-convert-an-int-to-a-string-in-cpp/>

类型转换是将变量从一种数据类型转换为另一种数据类型的过程。

类型转换可以隐式或显式完成。

隐式类型转换通过编译器自动完成，而显式类型转换由开发人员完成。

在本文中，您将学习如何在 C++中使用`stringstream`类和`to_string()`方法将整数转换成字符串。

## 如何在 C++中使用`stringstream`类将 Int 转换成 String

使用`stringstream`类，我们可以将整数转换成字符串。

这里有一个例子可以帮助你理解为什么你需要把一个整数转换成一个字符串:

```
#include <iostream>
using namespace std;

int main() {

    int age = 20;

    cout << "The user is " + age + " years old";
    // error: invalid operands of types 'const char*' and 'const char [11]' to binary 'operator+'

    return 0; 

} 
```

在上面的例子中，我们创建了一个值为 20 的`int`变量。

当我们试图将那个值连接成一个字符串时，我们得到一个错误，说“无效的类型操作数…”。

引发该错误是因为我们试图使用两个不兼容的变量类型执行操作。解决方案是转换一个变量，使其与另一个变量兼容。

`stringstream`类有插入(`<<`)和提取(`>>`)操作符。

插入运算符用于将变量传递给流。在我们的例子中，向流传递一个整数。

提取运算符用于给出修改后的变量。

换句话说，`stringstream`对象将接受一种数据类型，将其转换为另一种数据类型，并将新的数据类型赋给一个变量。

这里有一个例子:

```
#include <iostream>
#include <sstream>  
using namespace std;

int main() {

    int age = 20;

    // stringstream object
    stringstream stream;

    // insertion of integer variable to stream
    stream << age;

    // variable to hold the new variable from the stream
    string age_as_string;

    // extraction of string type of the integer variable
    stream >> age_as_string;

    cout << "The user is " + age_as_string + " years old";
    // The user is 20 years old

    return 0; 

} 
```

在上面的代码中，我们创建了一个名为`stream`的`stringstream`对象。注意，在使用它之前，必须包含 stringstream 类:`include <sstream>`。

然后我们将整数插入到流中:`stream << age;`。

之后，我们创建了一个名为`age_as_string`的新变量。这个变量将存储从流中提取的字符串变量。

最后，我们提取整数的字符串类型，并将其存储在上面创建的变量中:`stream >> age_as_string;`。

现在我们可以连接字符串并获得期望的结果:

```
 cout << "The user is " + age_as_string + " years old";
    // The user is 20 years old
```

## 如何在 C++中使用`to_string()`方法将 Int 转换成 String

您可以使用 [to_string()方法](https://www.freecodecamp.org/news/int-to-string-in-cpp-how-to-convert-an-integer-with-tostring/)将 int、float 和 double 数据类型转换为字符串。

这里有一个例子:

```
#include <iostream>
using namespace std;

int main() {

    int age = 20;

    string age_as_string = to_string(age);

    cout << "The user is " + age_as_string + " years old";
    // The user is 20 years old

    return 0; 

} 
```

在上面的代码中，我们将`age`变量传递给了`to_string()` : `string age_as_string = to_string(age);`。

这将`age`变量转换成一个字符串。就像上一节中的例子一样，我们现在可以将变量作为字符串使用:

```
cout << "The user is " + age_as_string + " years old";
// The user is 20 years old
```

## 摘要

在本文中，我们讨论了在 C++中将整数转换成字符串的不同方法。

这些例子展示了如何使用`stringstream`类和`to_string()`方法将整数转换成字符串。

编码快乐！