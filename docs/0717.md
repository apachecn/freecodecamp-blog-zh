# C++中的 Double VS Float——Float 和 double 的区别

> 原文：<https://www.freecodecamp.org/news/double-vs-float-in-cpp-the-difference-between-floats-and-doubles/>

在 C++中，有各种数据类型，如`string`、`int`、`char`、`bool`、`float`和`double`。每种数据类型都有可以存储的特定值。

当处理整数时，我们通常将它们存储在一个`int`数据类型中。但这只对整数有用。

当我们想要存储带小数的数字时，我们可以使用`float`或`double`。虽然这两种数据类型用于类似的目的，但它们有一些不同。

在本文中，我们将讨论 C++中浮点数和双精度数的区别，并给出一些例子。

## 浮点数和双精度数的区别

这一节将被分成几个小节，每个小节集中在浮点型和双精度型之间的一个区别上。

### 字节大小的差异

`float`的字节大小是 4，而`double`的字节大小是 8。

这意味着`double`可以存储两倍于`float`的值。

我们可以通过使用`sizeof()`操作符看到这一点。这里有一个例子:

```
#include <iostream>
using namespace std;
int main() {

    cout << "float: " << sizeof(float) << endl; // float: 4
    cout << "double: " << sizeof(double) << endl;// double: 8

}
```

### 精度(准确度)差异

当处理有很多小数位数的数字时，我们通常希望得到的值是准确的。但是我们的结果的准确性依赖于我们处理的十进制数字的数量。

别急，我们说的还是 C++，不是数学。

`float`和`double`在十进制数字的数量方面都有不同的能力。`float`最多可以精确到 7 位小数，而`double`最多可以精确到 15 位。

让我们看一些例子来证明这一点。

```
#include <iomanip>
#include <iostream>
using namespace std;

int main() {
    double MY_DOUBLE_VALUE = 5.12345678987;

    float MY_FLOAT_VALUE = 5.12345678987;

    cout << setprecision(7);
    cout << MY_DOUBLE_VALUE << endl; // 5.123457
    cout << MY_FLOAT_VALUE << endl; // 5.123457
}
```

在上面的例子中，我们创建了`float`和`double`变量——它们都有相同的值:`5.12345678987`。

`setprecision()`函数用于告诉编译器我们想要打印的小数位数。在我们的例子中，值是 7。

我们可以从上面代码的结果中观察到，两个变量都打印了精确到小数点后第 7 位的值:`5.123457`。

让我们将`setprecision()`函数中的参数增加到 12，看看会发生什么。

```
#include <iomanip>
#include <iostream>
using namespace std;

int main() {
    double MY_DOUBLE_VALUE = 5.12345678987;

    float MY_FLOAT_VALUE = 5.12345678987;

    cout << setprecision(12);
    cout << MY_DOUBLE_VALUE << endl; // 5.12345678987
    cout << MY_FLOAT_VALUE << endl; // 5.12345695496
}
```

从上面的结果来看，`MY_DOUBLE_VALUE`变量打印出了精确的值。但是`MY_FLOAT_VALUE`变量，从它的第 7 个小数位开始，打印出的值与给它的原始值完全不同。

这向我们展示了两种数据类型的精度。就像`float`一样，如果我们试图返回一个超过`double`数据类型精度范围的值，我们将得到一个不准确的返回值。

### 用法的不同

`float`由于范围小，主要用于图形库中以获得高处理能力。

`double`主要用于编程中的计算，以消除十进制数值被舍入时的错误。虽然`float`仍然可以使用，但它应该只在我们处理小十进制值的情况下使用。为了安全起见，你应该总是使用`double`。

## 结论

在本文中，我们讨论了 C++中浮点数和双精度数的区别。

我们讨论了三个不同点:字节大小、精度和用法。

我们还了解到双精度浮点数的字节大小是浮点数的两倍。此外，在处理大的十进制值时，doubles 更准确。

最后，我们讨论了帮助我们理解何时使用每种数据类型的用例。

编码快乐！