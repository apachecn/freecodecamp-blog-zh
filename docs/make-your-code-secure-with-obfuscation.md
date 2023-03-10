# 什么是代码混淆？如何伪装你的代码使其更加安全

> 原文：<https://www.freecodecamp.org/news/make-your-code-secure-with-obfuscation/>

在计算的早期，开发人员不必担心网络。他们可以专注于确保他们的软件服务于其预期目的，而不是经常崩溃。

接触过该软件的普通人并不构成威胁。大多数用户甚至懒得阅读软件包装盒中的用户手册，更不用说搜索代码漏洞了。

然后，互联网出现了，改变了一切。

几乎在一夜之间，计算机网络变得互联互通。随着复杂性的增加，有人找到进入那些不属于那里的网络的机会也增加了。

通常情况下，这些人拥有利用有缺陷的代码所需的技能。

这就把我们带到了今天。这是一个网络安全威胁空前的时代。网络攻击的消息似乎每天都有。

作为回应，网络管理者部署越来越复杂的防御系统来加强他们的网络抵御入侵者。他们现在希望软件开发人员多做一点工作来保护他们的代码，防止未经授权的访问。

然而，计算机代码的硬化仍然没有在编码学校里教授多少。但是它正在成为现代应用程序开发中的必备工具。

为了帮助补救，在本文中我将解释什么是代码混淆。我还将概述目前使用的六种最重要的代码混淆技术，帮助您开始编写更安全的软件。

## 什么是代码混淆？

顾名思义，代码混淆是指一系列旨在掩饰程序代码元素的编程技术。这是程序员保护他们的工作免受黑客或知识产权窃贼未经授权的访问或修改的主要方式。

最重要的是，代码混淆技术可能会改变程序用来操作的结构和方法，但它们永远不会改变程序的输出。

问题是，许多代码混淆技术会增加程序的开销和执行时间。

因此，了解哪些技术相对来说没有损失，哪些技术会导致性能问题是非常重要的。一旦知道了成本，就有可能在实际应用中平衡保护和性能。

以下是目前最常用的六种代码混淆技术。

## 1.删除多余的数据

在任何情况下都应该应用的第一个代码强化技术是去掉代码中所有不必要的东西。

这样做将简化您的代码库，并减少您要防御的攻击面。

这意味着删除冗余函数、调试信息和尽可能多的元数据。简而言之——任何可能给攻击者提供路线图，引导他们找到漏洞的东西。

## 2.转换数据

接下来要做的是转换代码处理的数据，使其无法识别。

像用表达式替换值，改变你使用的数据存储格式，甚至使用代码数字的二进制版本这样的策略都会增加复杂性。这种复杂性将使任何人很难对你的代码进行逆向工程，从中获得任何有用的东西。

例如，您可以使用字符串加密来使代码中的纯文本字符串不可读。字符串加密可以使用简单的 base64 编码，这将使代码:

```
String s = "Hello World";
Into:
String s = new String(decryptString("SGVsbG8gV29ybGQ="));
```

尽管对于一个有经验的程序员来说，发现这里发生了什么并不困难，但是必须处理解密大量的字符串是非常耗时和令人沮丧的。

当与其他代码混淆技术结合使用时，数据转换是有效的第一道防线。

## 3.使用进程顺序混淆

对代码进行模糊处理的一个挑战性要求是，当您完成后，您仍然需要代码按预期工作。

但是没有说你必须以任何逻辑顺序执行你的代码。如果您混淆了代码的操作顺序，您仍然可以获得正确的结果——但是会让第三方更难理解您的代码在做什么。

唯一的警告是，您必须小心不要创建太多无意义的循环和死胡同，因为您可能会意外地降低代码的执行时间。

例如，看一下下面的代码片段，它计算 100 个数字的总和和平均值:

```
int i=1, sum=0, avg=0
while (i = 100)
{
sum+=i;
avg=sum/i;
i++;
}int i=1, sum=0, avg=0
while (i = 100)
{
sum+=i;
avg=sum/i;
i++;
}
```

通过添加一个条件变量，有可能掩盖代码正在做的事情。这是因为对函数的分析首先需要知道输入到函数中的是什么。

在下面的代码片段中，条件变量“random”创建了一个更复杂的代码结构，使其更难破译:

```
int random = 1;
while (random != 0)
{
switch (random)
{
Case 1:
{
i=0; sum=1; avg=1;
random = 2;
break;
}
case 2:
{
if (i = 100)
random = 3;
else random = 0;
break;
}
case 3:
{
sum+=i;avg=sum/i ; i++;
random = 2;
break;
}
}
}
```

## 4.尝试调试混淆

有时，一个有决心的攻击者可以通过检查代码的调试信息来了解关于代码的各种有用信息。

在某些情况下，他们可能会找到破解你正在使用的其他混淆技术的钥匙。

因此，只要有可能，删除对调试信息的访问是一个好主意。当这不是一个选项时，屏蔽调试报告中的任何标识信息是必不可少的。

## 5.使用地址随机化

近三十年来，与内存处理相关的错误一直是黑客利用的最常见的软件漏洞——尽管每个程序员都知道这个问题仍然存在。

而且不仅仅是在新手中。大约 70%的谷歌 Chrome 浏览器漏洞源于内存错误。

事实是，要防止所有的内存编程错误几乎是不可能的，尤其是当你使用像 C 和 c++这样的[语言时。但是你可以做的是在你的代码中包含一些内存随机化特性，这将会有所帮助。](https://neosmart.net/blog/2018/modern-c-isnt-memory-safe/)

在执行时，如果您的代码和数据的虚拟地址被赋予随机值，那么发现和利用任何未打补丁的漏洞将变得更加困难。

此外，它还增加了另一个好处。这使得即使成功破解您的代码也很难(如果不是不可能的话)复制。仅仅这一点就使得攻击者不太可能浪费时间去攻击你的软件。

## 6.旋转混淆的代码

尽管上述技术可以挫败攻击者，但它们远非完美的防御。只要有足够的时间和技能，任何人都有办法打败他们。但这让我们想到了最重要的混淆技术之一。

因为所有的混淆技术都旨在增加攻击者工作的复杂性，所以你可以做的任何事情都是一个很好的防御措施。因此，为了保护您的代码，请充分利用互联网。

您可以发布定期更新，轮换您正在使用的模糊技术的性质和细节。每当你这样做的时候，所有有人可能已经投入破解你的软件的工作都变成了浪费时间。

如果你经常变换你的混淆策略，任何人都不值得去尝试并持续分析足够长的时间以获得成功。

## 通过模糊获得安全感

这里的底线是没有“不可破解”的代码。无论程序员如何努力，总会有漏洞。不过，这并不是说你不应该继续努力。

但是在现实世界中，你的代码不一定要完美。只要它足够难破解，没有哪个正常人会去尝试。对于那些头脑不正常的人来说，这只是需要足够复杂和耗时才能让他们保持冷静。

而这正是上面六个战术可以帮你完成的。但是记住，没有不付出代价的防御。在部署这些选项时，一定要权衡它们可能带来的执行时间损失和带来的好处。

如果你正在做一件特别敏感的事情，尽一切努力都是值得的。但是如果你正在写一篇引用日发电机的文章，也许不用太担心。

但是，无论您选择如何继续，都不要忘记花时间以这样或那样的方式强化您的代码。在一个到处都充满网络安全威胁的世界里，这只是正确的做事方式。

来自 Pexels 的 ThisIsEngineering 专题照片。