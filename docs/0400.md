# 编程命名惯例-骆驼，蛇，烤肉串，帕斯卡案例解释

> 原文：<https://www.freecodecamp.org/news/programming-naming-conventions-explained/>

如果你做过一段时间的编程，你可能听过“骆驼格”或者“帕斯卡格”这样的词。也许你想知道这些术语是什么意思。让我解释一下。

## 编程中的命名约定是什么？

除了我们从每种编程语言中得到的硬性规则之外，还有一些约定。这些是被大多数开发人员普遍接受的标准。

在各种约定中，命名约定是最常见的。因为作为程序员，我们给很多东西命名。比如变量、函数、类、方法、接口等等。

多年来，开发人员在他们的代码中使用不同的 case 类型来命名不同的实体。其中四个被证明是最受欢迎的。它们是:

*   [骆驼案](#what-is-camel-case)
*   [蛇案](#what-is-snake-case)
*   [烤肉串案](#what-is-kebab-case)
*   [帕斯卡格](#what-is-pascal-case)

让我们来看一些例子，这样你就能明白这些是如何工作的，好吗？

## 什么是骆驼案？

在 camel 的情况下，你用一个小写字母开始一个名字。如果名称有多个单词，后面的单词将以大写字母开头:

下面举几个骆驼案的例子:`firstName`和`lastName`。

## 什么是蛇案？

就像在 camel case 中一样，在 snake case 中，名称以一个小写字母开始。如果名称有多个单词，后面的单词将以小写字母开头，并且使用下划线(_)来分隔单词。

下面举几个蛇案的例子:`first_name`和`last_name`。

## 什么是烤肉串案？

Kebab 的大小写类似于 snake 的大小写，但是您使用连字符(-)而不是下划线(_)来分隔单词。

下面举几个烤肉串案例:`first-name`和`last-name`。

## 帕斯卡格是什么？

与前面的例子不同，pascal 大小写中的名字以大写字母开头。如果名称有多个单词，所有单词将以大写字母开头。

下面是一些 pascal case 的例子:`FirstName`和`LastName`。

## 何时使用每种命名约定

现在，根据您正在使用的语言和您正在命名的内容，首选的案例类型可以改变。

例如，根据 Python 代码的[PEP 8-Style Guide，变量和函数名应该使用 snake 大小写:](https://peps.python.org/pep-0008/)

```
user_name = 'Farhan'

def reverse_name(name):
	return name[::-1]
```

现在让我们来看看 JavaScript。根据 [Airbnb JavaScript 风格指南](https://github.com/airbnb/javascript)，变量和函数名应该使用骆驼大小写:

```
const userName = "Farhan";

function reverseName(name) {
 	return name.split("").reverse().join("");
}
```

尽管 Python 和 JavaScript 要求您在命名变量和函数时遵循不同的约定，但这两种语言都要求您在命名类时使用 pascal 大小写。

几乎所有流行的编程语言都有样式指南。以下是一些最常用的:

*   Python-[PEP 8-Python 代码风格指南](https://peps.python.org/pep-0008/)
*   JavaScript-[Airbnb JavaScript 风格指南](https://github.com/airbnb/javascript)
*   Java - [Java 风格指南](https://www.cs.cornell.edu/courses/JavaAndDS/JavaStyle.html)
*   C# - [C#编码约定](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
*   围棋- [优步围棋风格指南](https://github.com/uber-go/guide/blob/master/style.md)
*   C++ - [C++核心指南](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
*   PSR 12:扩展的编码风格

这些是我过去参考过的一些指南。还有其他指南。你可以随意做自己的研究，挑一个你喜欢的。只要确保你所遵循的指南确实被开发者社区所认可。

## 结论

这些是您应该知道的最流行的命名约定。如果您想了解更多关于不同命名约定的信息，您可以通读您正在使用的语言的样式指南。

了解你正在学习的语言的习惯是很重要的。虽然不遵循惯例不会破坏你的代码，但它会使你的代码更不一致，更难处理。

另一方面，遵循这些简单的约定会使你的代码可读性更强，更容易使用。所以，帮自己和他人一个忙，遵守惯例。