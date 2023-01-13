# C++字符串–c++中的 std::string 示例

> 原文：<https://www.freecodecamp.org/news/c-string-std-string-example-in-cpp/>

字符串是任何编程语言中必不可少的组成部分，C++也不例外。

无论您想要存储文本、操作文本还是接受键盘输入和输出，了解什么是字符串以及如何有效地使用字符串都是极其重要的。

这篇文章将教你在 C++中处理和使用字符串所需要知道的一切。

## 什么是字符串？

本质上，字符串是字符的集合。一些例子包括“你好世界”，“我的名字是杰森”，等等。它们用双引号`"`括起来。

在 C++中，我们有两种类型的字符串:

1.  c 样式字符串
2.  `std::string` s(来自 C++标准字符串类)

你可以很容易地用自己的小函数创建自己的字符串类，但这不是我们在本文中要讨论的内容。

# c 样式字符串

这些是从 C 编程语言中派生出来的字符串，并且在 C++中继续得到支持。这些“字符集合”以类型为`char`的数组的形式存储，这些数组是以*空终止的*(`\0`空字符)。

#### 如何定义 C 风格字符串:

```
char str[] = "c string";
```

这里，`str`是一个长度为`9`的`char`数组(多余的字符来自编译器添加的`\0`空字符)。

以下是在 C++中定义 C 风格字符串的一些其他方法:

```
char str[9] = "c string";
char str[] = {'c', ' ', 's', 't', 'r', 'i', 'n', 'g', '\0'};
char str[9] = {'c', ' ', 's', 't', 'r', 'i', 'n', 'g', '\0'};
```

#### 如何将 C 风格的字符串传递给函数

```
#include <iostream>

int main() {
    char str[] = "This is a C-style string";
    display(str);
}

// C-style strings can be passed to functions as follows:
void display(char str[]) {
    std::cout << str << "\n";
}
```

### 如何在 C++中使用 C 风格的字符串函数

C 标准库附带了几个方便的函数，可以用来操作字符串。虽然不建议广泛使用它们(见下文)，但您仍然可以通过包含`<cstring>`头在 C++代码中使用它们:

```
#include <cstring> // required

1\. strcpy(s1,s2) --> Copies string s2 into string s1\.                 
2\. strcat(s1,s2) --> Concatenates string s2 onto the end of string s1
3\. strlen(s1)    --> Returns the length of string s1         
4\. strcmp(s1,s2) --> Returns 0 if s1==s2; less than 0 if s1<s2; greater than 0 if s1>s2
5\. strchr(s1,ch) --> Returns a pointer to the first occurrence of character ch in string s1
6\. strstr(s1,s2) --> Returns a pointer to the first string s2 in string s1 
```

# 标准::字符串

c 风格的字符串相对来说*不安全*——如果 string/char 数组不是空终止的，它会导致一大堆潜在的错误。

例如，[缓冲区溢出](https://en.wikipedia.org/wiki/Buffer_overflow)以及[一大堆其他缺点](https://stackoverflow.com/questions/312570/what-are-some-of-the-drawbacks-to-using-c-style-strings)是 C++开发人员社区不推荐使用 C 风格字符串的一些原因。

C++标准库提供的`std::string`类是一个更安全的选择。你可以这样使用它:

#### 如何定义一个`std::string`

```
#include <iostream> 
#include <string> // the C++ Standard String Class

int main() {
    std::string str = "C++ String";
    std::cout << str << "\n"; // prints `C++ String`"
}
```

C 型琴弦和 s 型琴弦最明显的区别是琴弦的 T2 长度 T3。如果你需要一个 C 风格的字符串的长度，你需要每次使用`strlen()`函数来计算它，如下所示:

```
#include <iostream>
#include <cstring> // required to use `strlen`

int main() {
    char str[] = "hello world";
    std::cout << strlen(str) << "\n";
}
```

如果您不将它存储在变量中，而是在程序的多个部分都需要它，您可以很快观察到这个选项有多昂贵。

另一方面，`std::string`字符串已经有了内置的长度属性。要访问它，可以使用如下的`.length()`属性:

```
#include <iostream>
#include <string> // required to use `std::string`

int main() {
    std::string str = "freeCodeCamp";
    std::cout << str.length() << "\n";
}
```

简单，整洁，简洁，但重要的计算减少。

但是访问*长度*属性并不是使用`std::string` s 的唯一好处。

```
#include <iostream>
#include <string>

int main() {
   std::string str = "freeCodeCamp";

   // Inserting a single character into `str`
   str.push_back('s');
   std::cout << str << "\n"; // `str` is now `freeCodeCamps`

   // Deleting the last character from `str`
   str.pop_back();
   std::cout << str << "\n"; // `str` is now `freeCodeCamp`

   // Resizing a string
   str.resize(13);
   std::cout << str << "\n"; 

   // Decreasing excess capacity of the string
   str.shrink_to_fit()
   std::cout << str << "\n";
}
```

#### 如何向函数传递一个`std::string`

```
#include <iostream>

int main() {
    std::string str = "This is a C-style string";
    display(str);
}

// Passing `std::string`s are as you would normally pass a regular object
void display(std::string str) {
    std::cout << str << "\n";
}
```

## 什么时候你会在`std::string`上使用 C 风格的字符串？

到目前为止，您应该已经确信`std::string` s 相对于 C 风格字符串的众多优势(最显著的是自动内存管理)。但是有时候你会想用 C 风格的字符串来代替:

1.  如果你有 C 语言的背景，你可能会习惯于使用 C 风格的字符串。
2.  尽管有它的好处，但是非常复杂。像语言的其他部分一样，如果你不知道自己在做什么，它会很快变得非常复杂。另外，它使用了大量的内存，这对于你的程序来说可能并不理想。
3.  如果你在运行时小心地管理程序的内存(当你使用完一个对象的内存时释放它)，考虑到 C 风格的字符串是如此的小和轻，使用 C 风格的字符串会有一个性能上的好处。

# 包扎

我希望这篇文章是对 C++中字符串的介绍。关于这个奇妙的抽象，还有很多东西需要学习，我希望能写更多的文章来深入研究字符串和 C++的更高级的概念。

请务必[在 Twitter](http://twitter.com/jasmcaus) 上关注我，了解我的 C++学习之旅的最新进展。快乐学习！