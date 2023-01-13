# C++中的 getline–CIN getline()函数示例

> 原文：<https://www.freecodecamp.org/news/getline-in-cpp-cin-getline-function-example/>

在本文中，我们将讨论 C++中的`getline()`函数。这是一个内置函数，接受单个和多个字符输入。

在 C++中处理用户输入时，`cin`对象允许我们从用户那里获得输入信息。但是当我们试图注销具有多个值的用户输入时，它只返回第一个字符。

发生这种情况是因为 C++编译器在获取输入时假设任何空格都会终止程序。也就是说，“我的名字是 Ihechikara”在注销时只会返回“我的”。

这里有一个更好的例子:

```
#include <iostream>
using namespace std;

int main() {

    string bio;

    // Information logged to the console
    cout << "Tell us about yourself: ";

    /* This prompts the user to input a string and I typed in this: 				"JavaScript is my favorite language"
    */
    cin >> bio;

    /* When logging out the bio inputed above, only "JavaScript" was logged 		out
    */
    cout << "Your bio says: " << bio;
    // Your bio says: JavaScript 

}
```

在上面的代码中，用户被要求输入他们的简历。他们接着输入“JavaScript 是我最喜欢的语言”。但是当 bio 被记录到控制台时，只有“JavaScript”被注销。

接下来，我们将看到如何使用`getline()`函数来获取字符串中的其余字符。

## C++ getline()函数示例

在本节中，我们将看到一个使用`getline()`函数的实际例子。

```
#include <iostream>
using namespace std;

int main() {

    string bio;

    cout << "Tell us about yourself: ";

    getline(cin, bio);

    cout << "Your bio says: " << bio;
}
```

在上面的例子中，我们在`getline()`函数中传递了两个参数:`getline(cin, bio);`。第一个参数是`cin`对象，第二个是`bio`字符串变量。

当您运行代码时，会提示您输入一些文本。完成后，按 enter 键并查看输出，其中包含输入的所有文本，而不仅仅是第一个字符。

在我的例子中，我输入了一个包含多个字符的字符串，并将其注销到控制台。去试试吧，看看效果如何。

这样，您就可以在程序中有效地处理用户输入。

## 结论

在本文中，我们讨论了`getline()`函数，它使我们能够从用户输入中获得多个字符。

我们首先看到当我们从用户那里得到一个包含多个字符的字符串时会发生什么——只返回第一个字符。

然后，我们看到了如何使用带有两个参数的`getline()`函数从字符串中获取所有字符——`cin`对象和字符串变量。

编码快乐！