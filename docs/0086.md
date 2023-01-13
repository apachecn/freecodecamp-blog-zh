# Python 字符串包含–Python 3 子字符串教程

> 原文：<https://www.freecodecamp.org/news/python-string-contains-python-3-substring-tutorial-2/>

在本文中，您将学习如何在 Python 中检查字符串是否包含子串。

当您接收到用户输入并希望您的程序以某种方式运行时，或者甚至当您想要用一个单词替换另一个单词时，检查一个字符串是否包含子字符串会很方便。

Python 提供了许多方法来确认一个子串是否存在于一个字符串中，在本快速指南中，您将看到其中一些方法的实际应用。

以下是我们将要介绍的内容:

1.  [如何使用`in`操作符](#in-operator)检查一个字符串是否包含子串
    1.  [使用`in`操作符](#case-insensitive)时如何执行不区分大小写的搜索
2.  [如何使用`.index()`方法](#index-method)检查一个字符串是否包含子串
3.  [如何使用`.find()`方法](#find-method)检查一个字符串是否包含子串

## 如何使用 Python 中的`in`运算符检查一个字符串是否包含子串

使用`in`操作符是确认一个子串是否存在于一个字符串中的最清晰、可读性最强、最 Pythonic 化的方法之一。

因此，`in`操作符是检查字符串是否包含子串的首选方法。

使用`in`运算符的一般语法如下:

```
substring in string 
```

`in`运算符返回一个布尔值。布尔值可以是`True`或`False`。

如果子串存在于字符串中，则`in`操作符返回`True`，如果子串不存在，则返回`False`。

```
>>> learn_coding = "You can learn to code for free!"
>>> "free" in learn_coding
True 
```

在上面的例子中，我进入了 Python 的交互式控制台，也称为 Python 解释器或 Python shell。

你可以在本地安装 Python，打开你的终端，根据你的系统输入`Python`或者`Python3`后输入。

我将短语`You can learn to code for free!`存储在一个名为`learn_coding`的变量中。

然后，我使用`in`操作符检查子串`free`是否出现在短语`You can learn to code for free!`中。

由于子串存在，`in`返回`True`。

需要注意的是，`in`操作符只检查子串是否存在，并且存在于字符串中。它不检查子串的位置，也不给出子串在字符串中出现了多少次的任何信息。

顺便提一下，您还可以选择使用`not in`操作符来检查一个子串是否是**而不是**出现在一个字符串中:

```
>>> learn_coding = "You can learn to code for free!"
>>> "free" not in learn_coding
False 
```

这一次，我使用`not in`操作符来检查子串`free`是否是出现在字符串`You can learn to code for free!`中的**而不是**。由于`free`存在，`not in`操作器返回`False`。

现在，让我们回到`in`操作符。

您可以使用`in`操作符通过设置条件来控制程序的行为。

让我们举下面的例子:

```
user_input = input("Do you need to pay to learn to code?: ")

if "yes" in user_input:
  print("Wrong! You can learn to code for free!!") 
```

在上面的例子中，我要求用户输入并将他们的答案存储在一个名为`user_input`的变量中。

然后，我使用与`in`操作符配对的条件语句来做出决定(如果你需要复习 Python 中的条件语句，请阅读[这篇文章](https://www.freecodecamp.org/news/python-else-if-statement-example/))。

如果他们的回答包含子串`yes`，那么`in`返回短语`Wrong! You can learn to code for free`，因为`if`语句中的代码执行:

```
Do you need to pay to learn to code?: yes, I think you do
Wrong! You can learn to code for free!! 
```

在上面的例子中，用户输入的字符串`yes, I think you do`，包含子字符串`yes`，并且运行`if`块中的代码。

当用户输入大写`Y`而不是小写`Yes, I think you do`时会发生什么？

```
Do you need to pay to learn to code?: Yes, I think you do 
```

如你所见，什么也没发生！程序没有输出，因为 Python 字符串是区分大小写的。

你可以写一个`else`语句来解决这个问题。但是，在检查字符串中是否存在子字符串时，可以考虑区分大小写。

让我们在下一节看看如何做到这一点。

### 在 Python 中使用`in`操作符时如何执行不区分大小写的搜索

在上一节中，您看到了在搜索字符串中是否存在子字符串时，搜索是区分大小写的。

那么，如何让搜索不区分大小写呢？

您可以使用`.lower()`方法将整个用户输入转换成小写:

```
user_input = input("Do you need to pay to learn to code?: ").lower()

if "yes" in user_input:
  print("Wrong! You can learn to code for free!!") 
```

现在，当用户输入大写`Y`的`Yes`时，`if`语句中的代码就会运行，即使您正在搜索小写`y`的子字符串`yes`。这是因为您将用户输入文本转换为全部小写字母。

## 如何使用 Python 中的`.index()`方法检查一个字符串是否包含子串

您可以使用`.index()` Python 方法来查找字符串中第一个子字符串出现的第一个字符的起始索引号:

```
learn_coding = "You can learn to code for free! Yes, for free!"
substring = "free"

print(learn_coding.index(substring))

# output

# 26 
```

在上面的例子中，我将字符串`You can learn to code for free! Yes, for free!`存储在一个名为`learn_coding`的变量中。

我还将子字符串`free`存储在变量`substring`中。

然后，我在字符串上调用了`.index()`方法，并将子字符串作为参数传递，以查找第一个`free`子字符串出现的位置。(存储在`learn_coding`中的字符串包含子字符串`free`的两个实例)。

最后，我打印了结果。

如果子字符串是**而不是**出现在字符串中，那么就会产生一个`ValueError: substring not found`错误:

```
learn_coding = "You can learn to code for free! Yes, for free!"
substring = "paid"

print(learn_coding.index(substring))

# output

# Traceback (most recent call last):
#  File "main.py", line 4, in <module>
#    print(learn_coding.index(substring))
# ValueError: substring not found 
```

当您想知道正在搜索的子串的位置以及子串在字符串中出现和开始的位置时，`.index()`方法就很方便了。

`in`操作符让您知道子串是否存在于字符串中，而`.index()`方法也告诉您*它存在于哪里*。

也就是说，当 Python 在字符串中找不到子串时，`.index()`并不理想，因为它会引发一个`ValueError`错误。

如果您想避免在搜索字符串时出现错误，并且不想使用`in`操作符，那么您可以使用 Python `find()`方法。

## 如何使用 Python 中的`.find()`方法检查一个字符串是否包含子串

`.find()`方法的工作方式类似于`.index()`方法——它检查一个字符串是否包含子串，如果子串存在，它返回它的起始索引。

```
learn_coding = "You can learn to code for free! Yes, for free!"
substring = "free"

print(learn_coding.find(substring))

# output

# 26 
```

与`.index()`相比，`.find()`方法和`.index()`方法的区别在于，有了`.find()`，你不必担心它何时处理异常。

正如您在上一节中看到的，当字符串不包含子字符串时，`index()`会引发一个错误。

另一方面，当您使用`.find()`方法并且字符串不包含您正在搜索的子字符串时，`.find()`返回`-1`而不引发异常:

```
learn_coding = "You can learn to code for free! Yes, for free!"
substring = "paid"

print(learn_coding.find(substring))

# output

# -1 
```

## 结论

希望本文能帮助您理解如何在 Python 中检查字符串是否包含子串。

要了解更多关于 Python 编程语言的知识，请查看 freeCodeCamp 的 Python 认证。

你将从基础开始，以互动和初学者友好的方式学习。最后，您还将构建五个项目来付诸实践，并帮助巩固您所学的内容。

感谢您的阅读，祝您编码愉快！