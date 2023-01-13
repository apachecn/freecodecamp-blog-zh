# Java String to Int——如何将字符串转换成整数

> 原文：<https://www.freecodecamp.org/news/java-string-to-int-how-to-convert-a-string-to-an-integer/>

String 对象表示为字符串。

如果你用过 Java Swing，它有一些组件，比如 JTextField 和 JTextArea，我们用它们从 GUI 中获得输入。它将我们的输入作为一个字符串。

如果我们想用 Swing 制作一个简单的计算器，我们需要弄清楚如何将字符串转换成整数。这就引出了一个问题——我们如何将一个字符串转换成一个整数？

在 Java 中，我们可以使用`Integer.valueOf()`和`Integer.parseInt()`将字符串转换为整数。

## 1.使用 Integer.parseInt()将字符串转换为整数

该方法将字符串作为原始类型 int 返回。如果字符串不包含有效的整数，那么它将抛出一个 [NumberFormatException](https://docs.oracle.com/javase/7/docs/api/java/lang/NumberFormatException.html) 。

所以，每次我们把一个字符串转换成一个 int 时，我们需要通过把代码放在 try-catch 块中来处理这个异常。

让我们考虑一个使用`Integer.parseInt()`将字符串转换成 int 的例子:

```
 String str = "25";
        try{
            int number = Integer.parseInt(str);
            System.out.println(number); // output = 25
        }
        catch (NumberFormatException ex){
            ex.printStackTrace();
        }
```

让我们通过输入一个无效的整数来破解这段代码:

```
 String str = "25T";
        try{
            int number = Integer.parseInt(str);
            System.out.println(number);
        }
        catch (NumberFormatException ex){
            ex.printStackTrace();
        }
```

正如你在上面的代码中看到的，我们已经尝试将`25T`转换成一个整数。这不是有效的输入。因此，它必须抛出 NumberFormatException。

下面是上面代码的输出:

```
java.lang.NumberFormatException: For input string: "25T"
	at java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
	at java.lang.Integer.parseInt(Integer.java:580)
	at java.lang.Integer.parseInt(Integer.java:615)
	at OOP.StringTest.main(StringTest.java:51)
```

接下来，我们将考虑如何使用`Integer.valueOf()`方法将字符串转换为整数。

## 2.使用 Integer.valueOf()将字符串转换为整数

这个方法将字符串作为一个**整数对象**返回。如果你看一下[的 Java 文档](https://docs.oracle.com/javase/7/docs/api/java/lang/Integer.html#valueOf(java.lang.String))，`Integer.valueOf()`返回一个整数对象，相当于一个`new Integer(Integer.parseInt(s))`。

当使用这个方法时，我们将把代码放在 try-catch 块中。让我们考虑一个使用`Integer.valueOf()`方法的例子:

```
 String str = "25";
        try{
            Integer number = Integer.valueOf(str);
            System.out.println(number); // output = 25
        }
        catch (NumberFormatException ex){
            ex.printStackTrace();
        }
```

现在，让我们通过输入一个无效的整数来破解上面的代码:

```
 String str = "25TA";
        try{
            Integer number = Integer.valueOf(str);
            System.out.println(number); 
        }
        catch (NumberFormatException ex){
            ex.printStackTrace();
        }
```

与前面的例子类似，上面的代码会抛出一个异常。

下面是上面代码的输出:

```
java.lang.NumberFormatException: For input string: "25TA"
	at java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
	at java.lang.Integer.parseInt(Integer.java:580)
	at java.lang.Integer.valueOf(Integer.java:766)
	at OOP.StringTest.main(StringTest.java:42)
```

在使用上述方法之前，我们还可以创建一个方法来检查传入的字符串是否为数字。

我创建了一个简单的方法来检查传入的字符串是否是数字。

```
public class StringTest {
    public static void main(String[] args) {
        String str = "25";
        String str1 = "25.06";
        System.out.println(isNumeric(str));
        System.out.println(isNumeric(str1));
    }

    private static boolean isNumeric(String str){
        return str != null && str.matches("[0-9.]+");
    }
}
```

输出是:

```
true
true
```

`isNumeric()`方法接受一个字符串作为参数。首先它检查它是否是`null`。之后，我们使用`matches()`方法检查它是否包含数字 0 到 9 和一个句点字符。

这是检查数值的简单方法。根据您的使用情况，您可以编写或搜索更高级的正则表达式来捕获数字。

在尝试将传入的字符串转换为整数之前，最好先检查它是否为数字。

感谢您的阅读。

由[🇸🇮·扬科·菲利](https://unsplash.com/@itfeelslikefilm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/collections/139346/soul-care?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上发布图片

你可以通过[媒介](https://mvthanoshan.medium.com/)与我联系。

**编码快乐！**