# c++ Vector–CPP 中的 STD 模式向量，带有示例代码

> 原文：<https://www.freecodecamp.org/news/c-vector-std-pattern-vector-in-cpp-with-example-code/>

C++中的向量是存储动态数据的一种有用方式。它们还可以帮助您避免处理继承自 C 编程语言的不太灵活的数组。

这篇文章是对初学者友好的矢量介绍。它将向您展示它们的一些基本和必要的功能，以帮助您开始学习。

## C++中的向量是什么？

程序需要成组的数据来做任何事情。

这些数据可以是你一年中读过的书的列表，或者是餐馆就餐的支付选项，或者仅仅是一个名字列表——这些数据可以是任何类型的。

但是如何存储这些数据组呢？

C++中的向量是一种简单有效的存储数据和组织数据的方式。

Vectors，或`std::vector`，是 STL(标准模板库)中的一个模板类。但这意味着什么呢？

它们是 C 编程语言(C++语言的基础)中使用的数组的更灵活、更精炼、更有效的替代物。

与 C #中具有静态、固定大小的数组不同，向量是存储数据元素动态集合的有序容器。容器的大小不是固定的，而是动态地增长和收缩。

## 为什么以及何时在 C++中使用向量

当您处理不断变化的数据时，可以考虑使用向量。

如果您经常添加或删除数据，那么向量是处理这些动态元素的首选方式，因为它们能够自动调整大小。

如前所述，向量不是固定大小的，所以当您事先不知道数据的大小时，以及当您的数据没有事先建立时，使用向量是理想的。

容器的大小可以改变，所以不需要一开始就指定它们的最大大小。

## 如何在 C++中创建向量

要在 C++中创建 vector，首先必须包含 vector 库。

您可以通过在文件顶部添加行`#include <vector>`来实现这一点。这一行跟在第`#include <iostream>`行和程序中包含的任何其他头文件之后。

`std::vector`包含在`#include <vector>`库中。

创建向量的一般语法如下所示:

```
std::vector<data_type> name (items); 
```

让我们来分解一下:

*   你从关键字`std::vector`开始。
*   `<data_type>`指定矢量将存储的数据类型，用左右尖括号`<>`括起来。可以存储的数据类型的一些例子有:`<string>` s、`<int>` s、`double` s 和`char` s。注意，向量的类型一旦声明就不能改变。
*   接下来是你要给向量起的名字。定义数据类型和给向量命名是必需的步骤。
*   或者，您可以指定向量将容纳的元素数量。这将定义向量的大小。当你不知道向量从一开始就包含的项目的具体值，但是你知道大小的时候，它就派上用场了。
*   最后，不要忘记用分号`;`结束语句。

例如:

```
#include <iostream>
#include <vector>

int main() {
//without specifying the number of elements

//defines a vector named prices that stores floating point numbers
std::vector<double> prices;
} 
```

```
#include <iostream>
#include <vector>

int main() {
//specifying the number of elements

//creates a vector names prices
// it will hold floating point numbers
// the initial size of the vector is set to 10
std::vector<double> prices (10);
} 
```

当你在文件的顶部(头文件之后)使用`using namespace std;`时，你不需要在`std::`前面加上前缀，就像这样:

```
#include <iostream>
#include <vector>

using namespace std;

int main() {
vector<double> prices;
} 
```

## 如何在 C++中求向量的大小

`.size()`函数将返回一个向量中包含的元素数量。

你之前看到了如何创建一个空的向量。

要仔细检查，您应该:

```
#include <iostream>
#include <vector>

int main() {
std::vector<double> prices;

//returns the size
std::cout << prices.size() << std::endl;

//prints 0
} 
```

## 如何在 C++中给向量添加项

向量是元素的动态容器，它们的大小可以在程序的整个生命周期中增长，这取决于它的需要。

要向向量的**端**一次添加一个项目，可以使用`.push_back()`方法。

要添加的元素放在括号内。

```
#include <iostream>
#include <vector>

int main() {
std::vector <std::string> names;

//add elements to the vector by using .push_back()

names.push_back("Dionysia");
names.push_back("Dimitra");
names.push_back("George");

} 
```

## 如何在 C++中从 vector 中删除一个项目

除了向向量添加元素之外，您还可以删除它们。

`.pop_back()`函数将删除向量中最后一个项**。**

与用于添加元素的`.push_back()`方法相比，`.pop_back()`函数没有任何参数。

```
#include <iostream>
#include <vector>

int main() {
std::vector <std::string> names;

//add elements to the vector by using .push_back()

names.push_back("Dionysia");
names.push_back("Dimitra");
names.push_back("George");

//check the size
std::cout << names.size() << std::endl; //outputs 3

//remove the last element
names.pop_back();

//ckeck the size again
std::cout << names.size() << std::endl; // outputs 2
} 
```

## 向量索引

如前所述，向量是顺序项的容器，因此这意味着每个单独的项都可以通过其索引来访问。

向量中的索引从`0`开始–向量中的第一项的索引为`0`，第二项的索引为`1`，依此类推。

您可以通过指定要添加的位置来单独添加项目。

因此，您可以像这样重写前面的示例:

```
#include <iostream>
#include <vector>

int main() {
std::vector <std::string> names;

names[0] = "Dionysia"; // adds the string to the 1st place
names[1] = "Dimitra";  // adds the string to the 2nd place
names[2] = "George";   // adds the string to the third place
} 
```

要访问一个元素并输出每个单独的项，您需要在方括号中输入向量的名称和所需元素的位置。

```
#include <iostream>
#include <vector>

int main() {
std::vector<std::string> names;

names[0] = "Dionysia"; 
names[1] = "Dimitra";  
names[2] = "George";   

//print each element to the console:
std::cout << names[0] << std::endl; //prints 'Dionysia'
std::cout << names[1] << std::endl; // prints 'Dimitra'
std::cout << names[2] << std::endl; // prints 'George'
} 
```

## 结论

现在你知道了——你现在知道了 C++中向量的基础。

如果你有兴趣学习更多关于 C++的知识，可以在 freeCodeCamp 的 YouTube 频道上观看面向初学者的 C++教程。

感谢阅读和快乐编码:)