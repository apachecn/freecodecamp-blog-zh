# Java 中的 charAt()-如何使用 Java charAt()方法

> 原文：<https://www.freecodecamp.org/news/charat-in-java-how-to-use-the-java-charat-method-2/>

Java 中的`charAt()`方法返回字符串中给定或指定索引处字符的`char`值。

在本文中，我们将看到如何使用`charAt()`方法，从它的语法开始，然后通过几个例子/用例。

## 如何使用 Java charAt()方法

下面是`charAt()`方法的语法:

```
public char charAt(int index)
```

注意，使用`charAt()`方法从字符串返回的字符有一个`char`数据类型。我们将在本文后面看到这如何影响返回值的连接。

现在让我们看一些例子。

```
public class Main {
  public static void main(String[] args) {

    String greetings = "Hello World";

    System.out.println(greetings.charAt(0));
    // H
  }
}
```

在上面的代码中，我们的字符串——存储在名为`greetings`的变量中——表示“Hello World”。我们使用了`charAt()`方法来获取索引为 0 的字符，也就是 h。

第一个字符的索引始终为 0，第二个字符的索引为 1，依此类推。子字符串之间的空格也算作一个索引。

在下一个例子中，我们将看到当我们尝试连接返回的不同字符时会发生什么。串联意味着将两个或多个值连接在一起(在大多数情况下，该术语用于连接字符串中的字符或子字符串)。

```
public class Main {
  public static void main(String[] args) {
    String greetings = "Hello World";

    char ch1 = greetings.charAt(0); // H
    char ch2 = greetings.charAt(4); // o
    char ch3 = greetings.charAt(9); // l
    char ch4 = greetings.charAt(10); // d

    System.out.println(ch1 + ch2 + ch3 + ch4);
    // 391
  }
}
```

使用`charAt()`方法，我们得到了索引为 0、4、9 和 10 的字符，它们分别是 H、o、l 和 d。

然后我们尝试打印并连接这些字符:`System.out.println(ch1 + ch2 + ch3 + ch4);`。

但是我们没有得到“保留”，而是得到了 391。这是因为返回值不再是字符串，而是数据类型为`char`。所以当我们连接它们时，解释器会添加它们的 [ASCII](https://www.cs.cmu.edu/~pattis/15-1XX/common/handouts/ascii.html) 值。

h 的 ASCII 值为 72，o 的值为 111，l 的值为 108，d 的值为 100。当我们将它们加在一起时，我们得到 391，这是上一个示例中返回的值。

## StringIndexOutOfBoundsException 错误

当我们传入的索引号超过字符串中的字符数时，我们会在控制台中得到 StringIndexOutOfBoundsException 错误。

这个错误也适用于使用 Java 不支持的负索引。在 Python 这样支持负索引的编程语言中，传入-1 将给出数据集中的最后一个字符或值，类似于 0 总是返回第一个字符。

这里有一个例子:

```
public class Main {
  public static void main(String[] args) {
    String greetings = "Hello World";

    char ch1 = greetings.charAt(20); 

    System.out.println(ch1);

    /* Exception in thread "main" java.lang.StringIndexOutOfBoundsException: String index out of range: 20 
    */
  }
} 
```

在上面的代码中，我们传入了一个 20: `char ch1 = greetings.charAt(20);`的索引，它超过了我们的`greetings`变量中的字符数——所以我们得到了一个错误。您可以在上面的代码块中看到注释掉的错误消息。

类似地，如果我们像这样传入一个负值:`char ch1 = greetings.charAt(-1);`，我们会得到一个类似的错误。

## 结论

在本文中，我们学习了如何在 Java 中使用`charAt()`方法。我们看到了如何根据索引号返回字符串中的字符，以及当我们连接这些字符时会发生什么。

最后，我们讨论了一些在 Java 中使用`charAt()`方法时会得到错误响应的例子。

编码快乐！