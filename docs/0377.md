# Java scanner.nextLine()方法调用被跳过错误[已解决]

> 原文：<https://www.freecodecamp.org/news/java-scanner-nextline-call-gets-skipped-solved/>

有一个常见的错误往往会难倒新的 Java 程序员。当您将一堆输入提示组合在一起，并且跳过了一个`scanner.nextLine()`方法调用时，就会发生这种情况——没有任何失败或错误的迹象。

例如，看一下下面的代码片段:

```
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("What's your name? ");
        String name = scanner.nextLine();

        System.out.printf("So %s. How old are you? ", name);
        int age = scanner.nextInt();

        System.out.printf("Cool! %d is a good age to start programming. \nWhat language would you prefer? ", age);
        String language = scanner.nextLine();

        System.out.printf("Ah! %s is a solid programming language.", language);

        scanner.close();

    }

} 
```

第一个`scanner.nextLine()`调用提示用户输入他们的名字。然后`scanner.nextInt()`呼叫提示用户他们的年龄。最后一个`scanner.nextLine()`调用提示用户选择他们喜欢的编程语言。最后，您关闭 scanner 对象并结束工作。

这是非常基本的 Java 代码，涉及一个`scanner`对象来接受用户的输入，对吗？让我们试着运行这个程序，看看会发生什么。

如果您确实运行了该程序，您可能已经注意到该程序要求输入姓名，然后输入年龄，然后跳过最后一个关于首选编程语言的提示并突然结束。这就是我们今天要解决的问题。

## 为什么在`scanner.nextInt()`呼叫之后会跳过`scanner.nextLine()`呼叫？

这种行为并不仅限于`scanner.nextInt()`方法。如果你在任何其他的`scanner.nextWhatever()`方法之后调用`scanner.nextLine()`方法，程序将会跳过这个调用。

这与这两种方法的工作原理有关。第一个`scanner.nextLine()`提示用户输入他们的名字。

当用户输入姓名并按回车键时，`scanner.nextLine()`使用姓名和回车键或末尾的换行符。

这意味着输入缓冲区现在是空的。然后`scanner.nextInt()`提示用户输入他们的年龄。用户输入年龄并按 enter 键。

与`scanner.nextLine()`方法不同，`scanner.nextInt()`方法只消耗整数部分，并将回车或换行符留在输入缓冲区中。

当第三个`scanner.nextLine()`被调用时，它发现输入缓冲区中仍然存在回车或换行符，将其误认为是来自用户的输入，并立即返回。

如您所见，像许多现实生活中的问题一样，这是由用户和程序员之间的误解引起的。

有两种方法可以解决这个问题。您可以在调用`scanner.nextInt()`之后使用换行符，也可以将所有输入作为字符串，稍后解析为正确的数据类型。

## 如何在调用`scanner.nextInt()`后清除输入缓冲区

这比你想象的要容易。你所要做的就是在`scanner.nextInt()`呼叫发生后再打一个`scanner.nextLine()`呼叫。

```
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("What's your name? ");
        String name = scanner.nextLine();

        System.out.printf("So %s. How old are you? ", name);
        int age = scanner.nextInt();

        // consumes the dangling newline character
        scanner.nextLine();

        System.out.printf("Cool! %d is a good age to start programming. \nWhat language would you prefer? ", age);
        String language = scanner.nextLine();

        System.out.printf("Ah! %s is a solid programming language.", language);

        scanner.close();

    }

} 
```

尽管这个解决方案可行，但是无论何时调用任何其他方法，您都必须添加额外的`scanner.nextLine()`调用。这对于小程序来说没问题，但是在大程序中，这可能会很快变得很难看。

## 如何解析使用`scanner.nextLine()`方法获得的输入

Java 中的所有包装类都包含解析字符串值的方法。例如，`Integer.parseInt()`方法可以从给定的字符串中解析一个整数值。

```
import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("What's your name? ");
        String name = scanner.nextLine();

        System.out.printf("So %s. How old are you? ", name);
        // parse the integer from the string
        int age = Integer.parseInt(scanner.nextLine());

        System.out.printf("Cool! %d is a good age to start programming. \nWhat language would you prefer? ", age);
        String language = scanner.nextLine();

        System.out.printf("Ah! %s is a solid programming language.", language);

        scanner.close();

    }

} 
```

这是在 Java 中混合多种类型输入提示的一种更干净的方式。只要你小心用户输入的内容，解析应该没问题。

## **结论**

我衷心感谢你对我的写作感兴趣。我希望它对你有所帮助。

如果是的话，请随意与你的关系人分享。如果你想联系我，我可以在 [Twitter](https://twitter.com/frhnhsin) 和 [LinkedIn](https://www.linkedin.com/in/farhanhasin/) 上找到你。