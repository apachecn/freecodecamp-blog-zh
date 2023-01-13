# 如何使用 Java 中的打印功能

> 原文：<https://www.freecodecamp.org/news/how-does-print-work-in-java/>

通常，当您用 Java 编码时，您需要将一些内容打印到控制台输出中。您首先想到的可能是打印功能或打印语句。

但是很少有人知道 Java 中三种不同的打印函数/语句。在这篇文章中，我将向你讲述它们，并通过例子向你展示它们是如何工作的。

## 如何在 Java 中使用`println()`函数

`println()`函数在打印其中的值/数据后添加一个新行。在这里，后缀`ln`充当换行符`\n`。如果你考虑下面的例子:

```
public class Main{
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

您可能不知道到底发生了什么，因为您只打印了一行，您会得到以下输出:

```
Hello World!
```

但是如果你试着用`println()`打印几个不同的表达式，你会清楚地看到它们的区别！

```
public class Main{
    public static void main(String[] args) {
        System.out.println("Hello World!");
        System.out.println("Welcome to freeCodeCamp");
    }
}
```

在这里，您可以看到在执行第一个 print 语句之后，它添加了一个新的行字符(`\n`)。因此，您将在下一行中获得第二个打印语句`Welcome to freeCodeCamp`。

整个输出如下所示:

```
Hello World!
Welcome to freeCodeCamp
```

你可以看看我的这个视频，我在里面详细地讲述了这个`println()`功能。

[https://www.youtube.com/embed/_jfnI7yyaPo?feature=oembed](https://www.youtube.com/embed/_jfnI7yyaPo?feature=oembed)

但是，在打印函数中有没有避免自动生成换行符的方法呢？

是啊！有。在这种情况下，您将需要使用`print()`语句。

## 如何在 Java 中使用`print()`函数

对于这个函数，我就用我刚才用过的例子。您应该能马上看出不同之处:

```
public class Main{
    public static void main(String[] args) {
        System.out.print("Hello World!");
        System.out.print("Welcome to freeCodeCamp");
    }
}
```

在这里，你可以看到我使用了`print`，而不是像前面那样使用`println`。`print`在执行完其中的任务后不添加额外的`\n`(新行符)。这意味着在执行了上面的任何`print`语句后，你不会得到任何新的一行。

输出将是这样的:

```
Hello World!Welcome to freeCodeCamp
```

如果你愿意，你也可以使用下面的`\n`来解决这个问题:

```
public class Main{
    public static void main(String[] args) {
        System.out.print("Hello World!\n");
        System.out.print("Welcome to freeCodeCamp");
    }
}
```

这一次，`\n`将作为新的行字符，您将获得新行中的第二个字符串。输出如下所示:

```
Hello World!
Welcome to freeCodeCamp
```

您也可以只使用一条如下所示的 print 语句来打印这两个字符串:

```
public class Main{
    public static void main(String[] args) {
        System.out.print("Hello World!\nWelcome to freeCodeCamp");
    }
}
```

这次输出将是相同的:

```
Hello World!
Welcome to freeCodeCamp
```

## 如何在 Java 中使用`printf()`函数

该`printf()`功能作为**格式化打印功能**工作。考虑下面给出的两种情况:

场景 1:您的朋友 Tommy 希望您通过电子邮件向他提供您笔记本的 PDF。你可以简单地写一封邮件，提供你喜欢的主题(比如，嘿，汤米，我是法希姆)。您也可以避免在电子邮件的正文部分写任何内容，而是在电子邮件中附上 PDF 后再发送。就这么简单——你不需要对你的朋友保持任何礼貌，对吗？

场景 2:你昨天不能来上课。你的教授要求你提供有效的理由和证据，并通过电子邮件提交文件。

在这里，你不能像给你的朋友汤米那样给你的教授写信。在这种情况下，你需要保持正式和适当的礼仪。你必须提供一个正式合法的主题，并在正文部分写下必要的信息。最后但并非最不重要的一点是，在用适当的命名惯例重命名病历后，您必须将病历附加到您的电子邮件中。在这里，你格式化你的电子邮件作为当局想要的！

在`printf()`函数中，我们遵循第二个场景。如果我们想要指定任何特定的打印格式/样式，我们使用`printf()`功能。

让我举一个简短的例子来说明这是如何工作的:

```
public class Main{
    public static void main(String[] args) {
        double value = 2.3897;
        System.out.println(value);
        System.out.printf("%.2f" , value);
    }
}
```

在这里，我声明了一个名为`value`的 double 类型变量，并把`2.3897`赋给它。现在，当我使用`println()`函数时，它打印出小数点后 4 位数的整数值。

这是输出:

```
2.3897
2.39
```

但是在那之后，当我使用`printf()`函数时，我可以修改我希望函数如何打印值的输出流。在这里，我告诉这个函数，我希望在小数点后打印 2 位数。因此，该函数打印小数点后 2 位数的舍入值。

在这种情况下，我们通常使用`printf()`函数。但是请记住，它在 Java 编程语言中有广泛的用途。我会试着在此基础上写一篇详细的文章。😄

## 结论

在本文中，我已经向您介绍了 Java 中三个打印函数的基本区别。如果这对你有帮助，请告诉我。😄

如果你对我有任何建议，或者如果你想和我聊聊天，那么我的 [twitter](https://twitter.com/Fahim_FBA) 和 [LinkedIn](https://www.linkedin.com/in/fahimfba/) 账户永远为你敞开。此外，如果你认为我在相关技能方面有专长，一定要在 LinkedIn 上支持我。😊

如果你对开源感兴趣，那么你也可以在 [GitHub](https://github.com/FahimFBA) 上关注我，你也可以随时查看我的[网站](http://fahimbinamin.com/)和[博客](https://blog.fahimbinamin.com/)。

我也有两个 YouTube 频道，我尝试在那里定期发布与编程相关的内容。

📹英文品牌频道:[法希姆·宾·阿明-英文](https://www.youtube.com/channel/UCG97GCUifMS2Vm28tgXQi0Q)
📹孟加拉语品牌频道:[法希姆·宾·阿明-孟加拉语](https://www.youtube.com/c/FahimBinAminBengali)😃有趣的事实:我也教授和指导学生一些流行的编程语言。

谢谢！