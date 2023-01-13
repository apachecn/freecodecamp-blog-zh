# 如何在 Java 中处理 NullPointerException

> 原文：<https://www.freecodecamp.org/news/how-to-handle-nullpointerexception-in-java/>

如果您花了一些时间用 Java 开发程序，在某个时候您肯定会看到下面的异常:

```
java.lang.NullPointerException
```

一些主要的生产问题是由`NullPointerException`引起的。在本文中，我们将介绍一些在 Java 中处理`NullPointerException`的方法。

## 简单零检验

考虑下面这段代码:

```
public static void main(String args[]) {
    String input1 = null;
    simpleNullCheck(input1);
}

private static void simpleNullCheck(String str1) {
    System.out.println(str1.length());
}
```

如果您按原样运行此代码，将会得到以下异常:

```
Exception in thread "main" java.lang.NullPointerException
```

你得到这个错误的原因是因为我们正试图对`str1`也就是`null`执行`length()` 操作。

一个简单的解决方法是在`str1` 上添加一个空检查，如下所示:

```
private static void simpleNullCheck(String str1) {
    if (str1 != null) {
        System.out.println(str1.length());
    }
}
```

这样可以保证，当`str1` 是`null`时，你不会在上面运行`length()` 功能。

但是你可能会有下面这个问题。

### 如果 str1 是一个重要的变量呢？

在这种情况下，您可以尝试这样做:

```
 private static void simpleNullCheck(String str1) {
    if (str1 != null) {
        System.out.println(str1.length());
    } else {
        // Perform an alternate action when str1 is null
        // Print a message saying that this particular field is null and hence the program has to stop and cannot continue further.
    }
}
```

这个想法是，当你期望一个值是`null`时，最好对这个变量进行`null`检查。如果这个值确实是`null`，采取一个替代的行动。

这不仅适用于字符串，也适用于 Java 中的任何其他对象。

## Lombok Null Check

现在举以下例子:

```
public static void main(String args[]) {
    String input2 = "test";
    List<String> inputList = null;
    lombokNullCheck(input2, inputList, input2);
}

public static void lombokNullCheck(String str1, List<String> strList, String str2) {
    System.out.println(str1.length() + strList.size() + str2.length());
}
```

这里我们有一个接受三个参数的函数:`str1`、`strList`和`str2`。

如果这些值中的任何一个结果是`null`，我们根本不想执行这个函数中的逻辑。

### 你如何实现这一点？

这就是龙目岛派上用场的地方。为了在代码中添加 Lombok 库，请包含以下 Maven 依赖项:

```
 <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.12</version>
            <scope>provided</scope>
 </dependency>
```

要了解更多关于 Maven 的信息，请查看本文。

下面是带有 Lombok `null`检查的代码:

```
public static void main(String args[]) {
    String input2 = "test";
    List<String> inputList = null;
    try {
        lombokNullCheck(input2, inputList, input2);
    } catch (NullPointerException e) {
        System.out.println(e);
    }

}

public static void lombokNullCheck(@NonNull String str1, @NonNull List<String> strList, @NonNull String str2) {
    System.out.println(str1.length() + strList.size() + str2.length());
}
```

在函数的每个自变量前我们添加`@NonNull` 注释。

同样，当我们调用这个函数时，我们在函数调用周围放置一个`try-catch`块来捕捉`NullPointerException`。

如果函数中给出的任何参数结果是`null`，函数将抛出一个`NullPointerException`。这将被`try-catch`区块捕获。

这确保了，如果任何一个函数参数变成了`null`，那么函数中的逻辑不会被执行，我们知道代码不会有异常行为。

这也可以用一堆`null` check 语句来完成。但是使用 Lombok 有助于我们避免编写多个`null` check 语句，并使代码看起来更加整洁。

## 列表和空值

假设您有一个列表，并且想要打印列表中的所有元素:

```
List<String> stringList = new ArrayList<>();
stringList.add("ele1");
stringList.add("ele2");
if (stringList != null) {
    for (String element : stringList)
        System.out.println(element);
}
```

在遍历列表之前，我们需要对列表进行一个`null`检查。

如果`null`检查不存在，那么试图遍历一个`null`列表将会抛出一个`NullPointerException`。

## 地图和空值

让我们来看一个场景，您需要访问映射中特定键的值:

```
Map<String, String> testMap = new HashMap<>();
testMap.put("first_key", "first_val");
if (testMap != null && testMap.containsKey("first_key")) {
    System.out.println(testMap.get("first_key"));
}
```

首先，我们需要对地图对象本身进行空值检查。如果没有这样做，并且映射是`null`，则抛出一个`NullPointerException`。这是使用`testMap!=null`完成的

完成后，在访问某个特定的键之前检查它是否存在。您可以使用`testMap.containsKey("first_key")`检查钥匙是否存在。如果没有这样做，并且缺少特定的键，那么您将得到值`null`。

## 有必要总是添加空检查吗？

如果你确定某个变量永远不会是`null`，那么你可以避免添加`null`检查。这可能适用于私有函数，您可以控制进入函数的数据。

但是如果你不能确定一个对象的可空性，最好添加一个`null`检查。

## **代码**

本文讨论的所有代码都可以在这个 Github repo 中找到[。](https://github.com/aditya-sridhar/java-handling-nulls-example)

## **恭喜 ****？******

您现在知道如何在 Java 中处理`NullPointerException`!

## ******关于作者******

我热爱技术，关注该领域的进步。我也喜欢用我的技术知识帮助别人。

欢迎在我的博客上阅读更多我的文章，在 T2 的 LinkedIn 上联系我，或者在 T4 的 Twitter 上关注我。