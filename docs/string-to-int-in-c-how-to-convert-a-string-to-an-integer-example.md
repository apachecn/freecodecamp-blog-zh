# C++中的字符串转换为整数——如何将字符串转换为整数的例子

> 原文：<https://www.freecodecamp.org/news/string-to-int-in-c-how-to-convert-a-string-to-an-integer-example/>

当你用 C++编写代码时，经常会有需要将一种数据类型转换成另一种数据类型的时候。

在本文中，您将通过了解两种最流行的方法来学习如何在 C++中将字符串转换为整数。

我们开始吧！

## C++中的数据类型

C++编程语言有一些内置的数据类型:

*   `int`，用于整数(整)数(例如 10，150)
*   `double`，用于浮点数(例如 5.0，4.5)
*   `char`，用于单个字符(例如' D '，'！')
*   `string`，表示一个字符序列(例如“Hello”)
*   `bool`，为布尔值(真或假)

C++是一种**强类型**编程语言，这意味着当你创建一个变量时，你必须显式声明什么类型的值将被存储在其中。

## 如何在 C++中声明和初始化`int` s

要在 C++中用*声明*一个`int`变量，你需要首先写出变量的数据类型——在本例中为`int`。这将让编译器知道变量可以存储什么样的值，从而知道它可以采取什么行动。

接下来，您需要给变量一个名称。

最后，不要忘记结束语句的分号！

```
#include <iostream>

int main() {
    int age;
} 
```

然后你可以给你创建的变量赋值，就像这样:

```
#include <iostream>

int main() {
    int age;
    age = 28;
} 
```

您可以通过*初始化*变量并最终打印结果来组合这些动作，而不是作为单独的步骤来完成:

```
// a header file that enables the use of functions for outputing information
//e.g. cout or inputing information e.g. cin
#include <iostream> 

// a namespace statement; you won't have to use the std:: prefix
using namespace std;

int main() { // start of main function of the program
    int age = 28; 
    // initialize a variable. 
    //Initializing  is providing the type,name and value of the varibale in one go.

    // output to the console: "My age is 28",using chaining, <<
    cout << "My age is: " << age << endl;
}// end the main function 
```

## 如何在 C++中声明和初始化`string` s

字符串是单个字符的集合。

在 C++中声明字符串的工作方式与声明和初始化`int` s 非常相似，您在上一节中已经看到了。

C++标准库提供了一个`string`类。为了使用 string 数据类型，您必须在文件的顶部，在`#include <iostream>`之后包含`<string>`头文件库。

在包含了那个头文件之后，还可以添加前面看到的`using namespace std;`。

除此之外，添加这一行之后，在创建一个字符串变量时，您不必使用`std::string`——只需使用`string`。

```
#include <iostream>
#include <string>
using namespace std;

int main() {
    //declare a string variable

    string greeting;
    greeting = "Hello";
    //the `=` is the assignment operator,assigning the value to the variable

} 
```

或者，您可以初始化一个字符串变量，并将其打印到控制台:

```
#include <iostream>
#include <string>
using namespace std;

int main() {
    //initialize a string variable

    string greeting = "Hello";

   //output "Hello" to the console
   cout << greeting << endl;
} 
```

## 如何将字符串转换成整数

如前所述，C++是一种*强类型*语言。

如果你试图给出一个与数据类型不一致的值，你会得到一个错误。

此外，将字符串转换为整数不像使用类型转换那么简单，在将`double` s 转换为`int` s 时可以使用类型转换。

例如，你**不能**这样做:

```
#include <iostream>
#include <string>
using namespace std;

int main() {
   string str = "7";
   int num;

   num = (int) str;
} 
```

编译后的错误将是:

```
hellp.cpp:9:10: error: no matching conversion for C-style cast from 'std::__1::string' (aka
      'basic_string<char, char_traits<char>, allocator<char> >') to 'int'
   num = (int) str;
         ^~~~~~~~~
/Library/Developer/CommandLineTools/usr/bin/../include/c++/v1/string:875:5: note: candidate function
    operator __self_view() const _NOEXCEPT { return __self_view(data(), size()); }
    ^
1 error generated. 
```

有几种方法可以将一个字符串转换成一个 int，在接下来的章节中你会看到其中的两种。

### 如何使用`stoi()`函数将字符串转换成整数

将 string 对象转换成数字 int 的一个有效方法是使用`stoi()`函数。

这种方法通常用于较新版本的 C++，C++11 中引入了这种方法。

它将一个字符串值作为输入，并将它的整数版本作为输出返回。

```
#include <iostream>
#include <string>
using namespace std;

int main() {
   // a string variable named str
   string str = "7";
   //print to the console
   cout << "I am a string " << str << endl;

   //convert the string str variable to have an int value
   //place the new value in a new variable that holds int values, named num
   int num = stoi(str);

   //print to the console
   cout << "I am an int " << num << endl;
} 
```

输出:

```
I am a string 7
I am an int 7 
```

### 如何使用`stringstream`类将字符串转换成整数

`stringstream`类主要用于 C++的早期版本。它通过在字符串上执行输入和输出来工作。

要使用它，首先必须通过添加行`#include <sstream>`将`sstream`库包含在程序的顶部。

然后添加`stringstream`并创建一个`stringstream`对象，它将保存要转换为 int 的字符串的值，并将在将其转换为 int 的过程中使用。

使用`<<`操作符*从字符串变量中提取*字符串。

最后，使用`>>`操作符*将新转换的 int 值输入*到 int 变量。

```
#include <iostream>
#include <string>
#include <sstream> // this will allow you to use stringstream in your program

using namespace std;

int main() {
    //create a stringstream object, to input/output strings
   stringstream ss; 

   // a variable named str, that is of string data type
   string str = "7";

   // a variable named num, that is of int data type
   int num;

   //extract the string from the str variable (input the string in the stream)
   ss << str;

   // place the converted value to the int variable
   ss >> num;

   //print to the consloe
   cout << num << endl; // prints the intiger value 7
} 
```

## 结论

现在你知道了！您已经看到了在 C++中将字符串转换为整数的两种简单方法。

如果你想学习更多关于 C++编程语言的知识，请在 freeCodeCamp 的 YouTube 频道上查看这个 4 小时的课程。

感谢阅读和快乐学习😊