# Java 中的字符串转换为整数——如何将字符串转换为整数

> 原文：<https://www.freecodecamp.org/news/how-to-convert-a-string-to-an-integer-in-java/>

使用编程语言时，您可能希望将字符串转换为整数。例如，使用字符串变量的值执行数学运算。

在本文中，您将学习如何使用`Integer`类的两个方法— `parseInt()`和`valueOf()`在 Java 中将字符串转换成整数。

## 如何在 Java 中使用`Integer.parseInt`将字符串转换成整数

`parseInt()`方法把要转换成整数的字符串作为参数。那就是:

```
Integer.parseInt(string_varaible)
```

在查看其用法的示例之前，让我们看看在不进行任何转换的情况下添加一个字符串值和一个整数会发生什么:

```
class StrToInt {
    public static void main(String[] args) {
        String age = "10";

        System.out.println(age + 20);
        // 1020
    }
}
```

在上面的代码中，我们创建了一个字符串值为“10”的`age`变量。

当加上整数值 20 时，我们得到 1020 而不是 30。

这里有一个使用`parseInt()`方法的快速修复方法:

```
class StrToInt {
    public static void main(String[] args) {
        String age = "10";

        int age_to_int = Integer.parseInt(age);

        System.out.println(age_to_int + 20);
        // 30
    }
}
```

为了将`age`变量转换成整数，我们将其作为参数传递给`parseInt()`方法— `Integer.parseInt(age)` —并将其存储在名为`age_to_int`的变量中。

当与另一个整数相加时，我们得到了一个合适的加法:`age_to_int + 20`。

## 如何在 Java 中使用`Integer.valueOf`将字符串转换成整数

`valueOf()`方法的工作方式与`parseInt()`方法一样。它把要转换成整数的字符串作为它的参数。

这里有一个例子:

```
class StrToInt {
    public static void main(String[] args) {
        String age = "10";

        int age_to_int = Integer.valueOf(age);

        System.out.println(age_to_int + 20);
        // 30
    }
}
```

对上述代码的解释与上一节相同:

*   我们将字符串作为参数传递给`valueOf()` : `Integer.valueOf(age)`。它存储在一个名为`age_to_int`的变量中。
*   然后我们给创建的变量:`age_to_int + 20`加 20。结果值是 30，而不是 1020。

## 摘要

在本文中，我们讨论了在 Java 中将字符串转换成整数。

我们看到了如何在 Java 中使用`Integer`类的两个方法——`parseInt()`和`valueOf()`将字符串转换成整数。

编码快乐！