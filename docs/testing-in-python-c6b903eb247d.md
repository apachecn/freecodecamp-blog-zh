# Python 中单元测试的介绍

> 原文：<https://www.freecodecamp.org/news/testing-in-python-c6b903eb247d/>

你刚刚写完一段代码，你想知道该做什么。你会提交一个拉取请求并让你的队友审查代码吗？还是会手动测试代码？

这两件事你都应该做，但是还有一个额外的步骤:你需要对你的代码进行单元测试，以确保代码按照预期的那样工作。

单元测试可以通过也可以失败，这使得它们成为检查代码的一项伟大技术。在本教程中，我将演示如何用 Python 编写单元测试，您将看到在您自己的项目中使用它们是多么容易。

# 入门指南

理解测试的最好方法是亲自动手。为此，在一个名为 name_function.py 的文件中，我将编写一个简单的函数，它接受名和姓，并返回全名:

```
#Generate a formatted full name
def formatted_name(first_name, last_name):
   full_name = first_name + ' ' + last_name
   return full_name.title()
```

formatted_name()函数取名字和姓氏，并用一个空格将它们组合起来，形成一个全名。然后将每个单词的首字母大写。要检查这段代码是否有效，您需要编写一些使用该函数的代码。在 names.py 中，我将编写一些简单的代码，让用户输入他们的名字和姓氏:

```
from name_function import formatted_name

print("Please enter the first and last names or enter x to E[x]it.")

while True:
   first_name = input("Please enter the first name: ")
   if first_name == "x":
       print("Good bye.")
       break

   last_name = input("Please enter the last name: ")
   if last_name == "x":
       print("Good bye.")
       break

   result = formatted_name(first_name, last_name)
   print("Formatted name is: " + result + ".")
```

这段代码从 name_function.py 导入 formatted_name()，并在运行时允许用户输入一系列名字和姓氏，并显示格式化的全名。

# 单元测试和测试用例

Python 的标准库中有一个名为 unittest 的模块，其中包含测试代码的工具。单元测试检查函数行为的所有特定部分是否正确，这将使它们与其他部分的集成更加容易。

测试用例是单元测试的集合，这些单元测试一起证明一个功能在该功能可能找到自己并期望它处理的所有情况下都能按预期工作。测试用例应该考虑一个功能可能从用户那里接收到的所有可能的输入，因此应该包括代表每一种情况的测试。

# 通过测试

下面是编写测试的典型场景:

首先，您需要创建一个测试文件。然后导入 unittest 模块，定义从 unittest 继承的测试类。TestCase，最后，编写一系列方法来测试函数行为的所有情况。

以下代码下面有一行一行的解释:

```
import unittest
from name_function import formatted_name

class NamesTestCase(unittest.TestCase):

   def test_first_last_name(self):
       result = formatted_name("pete", "seeger")
       self.assertEqual(result, "Pete Seeger")
```

首先，您需要导入一个 unittest 和您想要测试的函数，formatted_name()。然后创建一个类，例如 NamesTestCase，它将包含对 formatted_name()函数的测试。该类继承自 unittest.TestCase 类。

NamesTestCase 包含一个测试 formatted_name()一部分的方法。您可以将此方法称为 test_first_last_name()。

> 请记住，当您运行 test_name_function.py 时，每个以“test_”开头的方法都会自动运行。

在 test_first_last_name()测试方法中，调用要测试的函数并存储返回值。在本例中，我们将使用参数“pete”和“seeger”调用 formatted_name()，并将结果存储在结果变量中。

在最后一行，我们将使用 assert 方法。assert 方法验证您收到的结果是否与您预期收到的结果相匹配。在这种情况下，我们知道 formatted_name()函数将返回第一个字母大写的全名，所以我们期待结果“皮特·西格”。为了检查这一点，使用了 unittest 的 assertEqual()方法。

```
self.assertEqual(result, “Pete Seeger”)
```

这一行的基本意思是:将结果变量中的值与“皮特·西格”进行比较，如果它们相等就没问题，但是如果它们不相等就告诉我。

在运行 test_name_function.py 时，您应该得到一个 OK，这意味着测试已经通过。

```
Ran 1 test in 0.001s

OK
```

# 考试不及格

为了向您展示失败的测试是什么样的，我将通过包含一个新的中间名参数来修改 formatted_name()函数。

因此，我将重写该函数，如下所示:

```
#Generate a formatted full name including a middle name
def formatted_name(first_name, last_name, middle_name):
   full_name = first_name + ' ' + middle_name + ' ' + last_name
   return full_name.title()
```

这个版本的 formatted_name()适用于有中间名的人，但是当你测试它的时候，你会发现这个函数对于没有中间名的人是无效的。

因此，当您运行 test_name_function.py 时，您将得到类似如下的输出:

```
Error
Traceback (most recent call last):

File “test_name_function.py”, line 7, in test_first_last_name
    result = formatted_name(“pete”, “seeger”)

TypeError: formatted_name() missing 1 required positional argument: ‘middle_name’

Ran 1 test in 0.002s

FAILED (errors=1)
```

在输出中，您会看到一些信息，告诉您需要知道的测试失败的地方:

*   输出中的第一项是错误，告诉您测试用例中至少有一个测试导致了错误。
*   接下来，您将看到发生错误的文件和方法。
*   之后，您将看到发生错误的那一行。
*   以及这是什么样的错误，在这种情况下，我们缺少 1 个参数“middle_name”。
*   您还将看到运行测试的数量、完成测试所需的时间，以及一条文本消息，该消息表示测试的状态以及发生的错误数量。

# 测试失败时该做什么

> 通过测试意味着函数的行为符合预期。然而，一次失败的测试意味着你会有更多的乐趣。

我见过一些程序员更喜欢改变测试而不是改进代码——但他们不会这么做。多花一点时间来解决这个问题，因为这将帮助您更好地理解代码，并从长远来看节省时间。

在这个例子中，我们的函数 formatted_name()首先需要两个参数，现在重写后需要一个额外的参数:中间名。给我们的函数添加一个中间名破坏了它的预期行为。因为我们的想法是不对测试进行修改，所以最好的解决方案是让中间名可选。

这样做之后，我们的想法是在使用名和姓时通过测试，例如“皮特·西格”，以及在使用名、姓和中间名时通过测试，例如“雷蒙德·雷德·雷丁顿”。因此，让我们再次修改 formatted_name()的代码:

```
#Generate a formatted full name including a middle name
def formatted_name(first_name, last_name, middle_name=''):
   if len(middle_name) > 0:
       full_name = first_name + ' ' + middle_name + ' ' + last_name
   else:
       full_name = first_name + ' ' + last_name
   return full_name.title()
```

现在，这个函数应该对有中间名和没有中间名的名字都有效了。

为了确保它在“皮特·西格”上仍然有效，请再次运行测试:

```
Ran 1 test in 0.001s

OK
```

> 这就是我想要向你展示的:修改你的代码来适应你的测试总是比其他方式更好。现在是时候为有中间名的名字增加一个新的测试了。

# 添加新测试

向 NamesTestCase 类编写一个新方法，用于测试中间名:

```
import unittest
from name_function import formatted_name

class NamesTestCase(unittest.TestCase):

    def test_first_last_name(self):
        result = formatted_name("pete", "seeger")
        self.assertEqual(result, "Pete Seeger")

    def test_first_last_middle_name(self):
        result = formatted_name("raymond", "reddington", "red")
        self.assertEqual(result, "Raymond Red Reddington")
```

运行测试后，两个测试都应该通过:

```
Ran 2 tests in 0.001s

OK
```

> 干得好！
> 干得好！

您已经编写了测试来检查函数是否使用有中间名或没有中间名的名字。请继续关注第 2 部分，在那里我将更多地讨论 Python 中的测试。

* * *

感谢您的阅读！在我的 freeCodeCamp 个人资料上查看更多类似的文章:[https://www.freecodecamp.org/news/author/goran/](https://www.freecodecamp.org/news/author/goran/)和我在 GitHub 页面上创建的其他有趣的东西:[https://github.com/GoranAviani](https://github.com/GoranAviani)