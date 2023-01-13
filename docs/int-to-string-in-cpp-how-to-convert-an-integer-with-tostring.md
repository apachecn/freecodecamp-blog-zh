# 如何用 to_string 转换一个整数

> 原文：<https://www.freecodecamp.org/news/int-to-string-in-cpp-how-to-convert-an-integer-with-tostring/>

在代码中处理字符串时，您可能希望执行某些操作，如连接(或链接)两个字符串。

但是有些情况下，您更愿意处理数值，就好像它们是字符串一样，因为连接一个字符串和一个整数会给你带来错误。

在本文中，我们将看到如何使用 C++中的`to_string()`方法将整数转换成字符串。

## 如何用`to_string()`转换整数

要使用`to_string()`方法，我们必须将整数作为参数传入。下面是帮助您理解的语法:

```
to_string(INTEGER)
```

让我们看一个例子。

```
#include <iostream>
using namespace std;

int main() {

    string first_name = "John";

    int age = 80;

    cout << first_name + " is " + age + " years old";

}
```

从上面的代码中，您会期望在控制台中看到“John 已经 80 岁了”。但这实际上会返回一个错误，因为我们试图用一个整数连接字符串。

让我们使用`to_string()`方法来解决这个问题。

```
#include <iostream>
using namespace std;

int main() {

    string first_name = "John";

    int age = 80;

    string AGE_TO_STRING = to_string(age);

    cout << first_name + " is " + AGE_TO_STRING + " years old";

}
```

我们创建了一个名为`AGE_TO_STRING`的新变量，它在`to_string()`方法的帮助下存储了`age`变量的字符串值。

正如您在示例中看到的，整数`age`作为参数传递给了`to_string()`方法，将其转换为字符串。

现在，当我们运行代码时，控制台上会显示“John 80 岁”。

当我们想要将`float`和`double`数据类型值——用于存储带小数的数字——转换为字符串时,`to_string()`方法也可以工作。

这里有一些例子来证明:

```
#include <iostream>
using namespace std;

int main() {

    string first_name = "John";

    float age = 10.5;

    string AGE_TO_STRING = to_string(age);

    cout << first_name + " is " + AGE_TO_STRING + " years old";
    // John is 10.500000 years old

}
```

上面的例子显示了一个被转换成字符串的`float`值。在输出中(在上面的代码中被注释掉)，您可以看到字符串中的十进制值。

对于`double`数据类型:

```
#include <iostream>
using namespace std;

int main() {

    string first_name = "John";

    double age = 10.5;

    string AGE_TO_STRING = to_string(age);

    cout << first_name + " is " + AGE_TO_STRING + " years old";
    // John is 10.500000 years old

}
```

这与上一个例子的结果相同。唯一的区别是我们使用了一个`double`值。

## 结论

在本文中，我们讨论了在 C++中使用`to_string()`方法将整数转换成字符串。

在我们的例子中，我们试图将字符串和一个整数连接成一个更大的字符串，但这给了我们一个错误。

使用`to_string()`方法，我们能够转换具有`int`、`float`和`double`数据类型的变量，并像使用字符串一样使用它们。

编码快乐！