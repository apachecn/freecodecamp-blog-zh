# Java 排序数组——如何用 Arrays.sort()按升序或降序反转一个数组

> 原文：<https://www.freecodecamp.org/news/java-sort-array-how-to-reverse-an-array-in-ascending-or-descending-order-with-arrays-sort-2/>

在 Java 中，使用数组在单个变量中存储一组变量(具有相同的数据类型)。

在许多情况下，存储在数组中的值以随机顺序出现。使用 Java 中的`Arrays`类，您可以使用各种方法来操作数组。

我们将从`Arrays`类中使用的方法之一是`sort()`方法，它以升序对数组进行排序。

我们还将看到如何使用 Java 中的`Collections`类中的`reverseOrder()`方法对数组进行降序排序。

## 如何在 Java 中使用`Arrays.sort()`对数组进行升序排序

在这一节中，我们将看到一个如何使用`sort()`方法对数组进行升序排序的例子。

```
import java.util.Arrays;

class ArraySort {
    public static void main(String[] args) {
        int[] arr = { 5, 2, 1, 8, 10 };
        Arrays.sort(arr);

        for (int values : arr) {
            System.out.print(values + ", ");
            // 1, 2, 5, 8, 10,
        }
    }
}
```

在上面的例子中，我们做的第一件事是导入`Arrays`类:`import java.util.Arrays;`。这让我们可以访问`Arrays`类的所有方法。

然后我们创建了一个随机排列的数组:`int[] arr = { 5, 2, 1, 8, 10 };`。

为了对这个数组进行升序排序，我们将这个数组作为参数传递给了`sort()`方法:`Arrays.sort(arr);`。

注意，`Arrays`类是在使用点符号访问`sort()`方法之前首先编写的。

最后，我们在控制台中循环并打印了数组。结果是一个排序后的数组:`1, 2, 5, 8, 10`。

在下一节中，我们将讨论如何对数组进行降序排序。

## 如何在 Java 中使用`Collections.reverseOrder()`对数组进行降序排序

为了对数组进行降序排序，我们使用了可以从`Collections`类中访问的`reverseOrder()`。

我们仍将使用`Arrays.sort();`，但是在这个例子中，它将接受两个参数——要排序的数组和`Collections.reverseOrder()`。

这里有一个例子:

```
import java.util.Arrays;
import java.util.Collections;

class ArraySort {
    public static void main(String[] args) {
        Integer[] arr = { 5, 2, 1, 8, 10 };
        Arrays.sort(arr, Collections.reverseOrder());

        for (int values : arr) {
            System.out.print(values + ", ");
            // 10, 8, 5, 2, 1,
        }
    }
}
```

首先，我们导入了 Arrays 和 Collections 类，因为我们将使用这些类提供的方法。

然后，我们按照随机顺序创建了一个数字数组:`Integer[] arr = { 5, 2, 1, 8, 10 };`。你会注意到我们使用了`Integer[]`而不是`int[]`,就像我们在上一个例子中做的那样——后者会抛出一个错误。

为了对数组进行降序排序，我们这样做了:`Arrays.sort(arr, Collections.reverseOrder());`。

第一个参数是数组`arr`，它将按升序排序。第二个参数–`Collections.reverseOrder()`–将反转排序后的数组的顺序，使其按降序排列。

当循环并打印时，数组看起来像这样:`10, 8, 5, 2, 1`。

## 摘要

在本文中，我们讨论了 Java 中的数组排序。数组可以按升序或降序排序。

我们可以使用从`Arrays`类访问的`sort()`方法对数组进行升序排序。`sort()`方法接受要排序的数组作为参数。

为了对数组进行降序排序，我们使用了由`Collections`类提供的`reverseOrder()`方法。这作为第二个参数传入到`sort()`方法中，这样排序后的数组可以按降序重新排列。

编码快乐！