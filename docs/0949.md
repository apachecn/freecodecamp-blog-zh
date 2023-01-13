# 如何在 Java 中找到一个字符串的索引

> 原文：<https://www.freecodecamp.org/news/indexof-in-java-how-to-find-the-index-of-a-string-in-java/>

字符串是嵌套在双引号中的字符的集合。`indexOf`方法返回指定字符或子串在字符串中的索引位置。

在本文中，我们将看到不同的`indexOf`方法的语法。我们还将看一些例子，以帮助您理解并有效地使用它们来查找 Java 代码中某个字符或子串的索引。

## `indexOf`方法的语法

`indexOf`方法有以下几种方法:

```
public int indexOf(int char)
public int indexOf(int char, int fromIndex)
public int indexOf(String str)
public int indexOf(String str, int fromIndex)
```

在看一些例子之前，让我们解释一下这些参数:

*   `char`表示字符串中的单个字符。
*   `fromIndex`表示开始搜索字符或子字符串索引的位置。当一个字符串中有两个值相同的字符/字符串时，这一点很重要。有了这个参数，您可以告诉`indexOf`方法从哪里开始它的操作。
*   `str`表示字符串中的子串。

如果您还不明白这些是如何工作的，请不要担心——示例会让您一目了然！

## 如何在 Java 中使用 indexOf 方法

在下面的第一个例子中，我们将找到字符串中单个字符的索引。这个例子将帮助我们理解`public int indexOf(int char)`方法。

### `indexOf(int Char)`方法示例

```
public class Main {
  public static void main(String[] args) {
    String greetings = "Hello World";

    System.out.println(greetings.indexOf("o"));

    // 4
  }
} 
```

在上面的代码中，我们得到了返回给我们的字符“0”的索引，即 4。我们有两个“o”字符，但是第一个字符的索引被返回。

在下一个例子中，我们将看到如何返回下一个例子中第二个“o”的索引。

如果您想知道索引号是如何得到的，那么您应该注意到字符串中的第一个字符的索引是 0，第二个字符的索引是 1，依此类推。

### `indexOf(int Char, Int fromIndex)`方法示例

这里有一个解释`int indexOf(int char, int fromIndex)`方法的例子:

```
public class Main {
  public static void main(String[] args) {
    String greetings = "Hello World";

    System.out.println(greetings.indexOf("o", 5));

    // 7
  }
} 
```

在上面的例子中，我们告诉`indexOf`方法从第五个索引开始操作。

H = >索引 0

e = >索引 1

l = >索引 2

l = >索引 3

0 = >索引 4

注意，索引 5 不是字符“W”。第五个指标是“你好”和“世界”之间的空间。

所以从上面的代码来看，第五个索引之前的所有其他字符都将被忽略。7 作为第二个“o”字符的索引返回。

### `Int indexOf(String Str)`方法示例

在下一个例子中，我们将理解返回子串索引的`public int indexOf(String str)`方法是如何工作的。

```
public class Main {
  public static void main(String[] args) {
    String motivation = "Coding can be difficult but don't give up";

    System.out.println(motivation.indexOf("be"));

    // 11
  }
} 
```

想知道我们如何得到 11 返回？您应该查看最后一节，以了解索引是如何计算的，以及子字符串之间的空格也是如何计算索引的。

请注意，当子字符串作为参数传入时，返回的索引是子字符串中第一个字符的索引——11 是“b”字符的索引。

### `indexOf(String Str, Int fromIndex)`方法示例

最后一种方法——`public int indexOf(String str, int fromIndex)`——与`public int indexOf(int char, int fromIndex)`方法相同。它从指定位置返回一个索引。

这里有一个例子:

```
public class Main {
  public static void main(String[] args) {
    String motivation = "The for loop is used for the following";

    System.out.println(motivation.indexOf("for", 5));

    // 21
  }
} 
```

在上面的例子中，我们已经指定方法应该从第五个索引开始操作，第五个索引是第一个“for”子串之后的索引。21 是第二个“for”子串的索引。

最后，当我们传入一个字符串中不存在的字符或子串时，`indexOf`方法将返回值-1。这里有一个例子:

```
public class Main {
  public static void main(String[] args) {
    String motivation = "The for loop is used for the following";

    System.out.println(motivation.indexOf("code"));

    // -1
  }
} 
```

## 结论

在本文中，我们学习了如何使用四种`indexOf`方法，并举例说明了每种不同的方法。

我们还看到了这些方法的语法，以及它们如何告诉索引返回。

我们最后展示了当一个不存在的字符或子串作为参数传入时会发生什么。

编码快乐！