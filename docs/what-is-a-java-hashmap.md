# 什么是 Java 散列表？

> 原文：<https://www.freecodecamp.org/news/what-is-a-java-hashmap/>

在 Java 中，使用 HashMap 以键/值对的形式存储项目。您可以使用项目的关键字访问存储在`HashMap`中的项目，该关键字对每个项目都是唯一的。

在本文中，我们将讨论一个`HashMap`的特性，如何创建一个`HashMap`，以及我们可以用来与存储在其中的数据进行交互的不同方法。

## Java 中 HashMap 的特点是什么？

在使用 HashMaps 之前，理解它们是如何工作的很重要。

以下是`HashMap`的一些特征:

*   项目存储在键/值对中。
*   添加时，项目不保持任何顺序。数据是无序的。
*   在存在重复键的情况下，最后一个键将覆盖其他键。
*   使用包装类而不是原始数据类型来指定数据类型。

## 如何在 Java 中创建散列表

为了创建和使用 HashMap，您必须首先导入`java.util.HashMap`包。那就是:

```
import java.util.HashMap;
```

下面是创建新的`HashMap`的语法:

```
HashMap<KeyDataType, ValueDataType> HashMapName = new HashMap<>();
```

让我们解释一下上面语法中的一些关键术语。

*   `KeyDataType`表示将存储在`HashMap`中的所有键的数据类型。
*   `ValueDataType`表示将存储在`HashMap`中的所有值的数据类型。
*   `HashMapName`表示`HashMap`的名称。

这里有一个简化术语的例子:

```
HashMap<Integer, String> StudentInfo = new HashMap<>();
```

在上面的代码中，我们创建了一个名为`StudentInfo`的`HashMap`。存储在`HashMap`中的键都是整数，而值是字符串。

您会注意到，在为键和值指定数据类型时，我们使用的是包装类，而不是基本类型。这就是 HashMaps 的工作方式。

在我们深入例子之前，这里有一个包装类及其相应的 Java 原始数据类型的列表:

### Java 中的包装类和基本类型

| 包装类 | 原始数据类型 |
| --- | --- |
| 整数 | （同 Internationalorganizations）国际组织 |
| 性格；角色；字母 | 茶 |
| 浮动 | 漂浮物 |
| 字节 | 字节 |
| 短的 | 短的 |
| 长的 | 长的 |
| 两倍 | 两倍 |
| 布尔代数学体系的 | 布尔型 |

当使用 HashMaps 时，我们使用包装类。

## Java 中的 HashMap 方法

在这一节中，我们将讨论一些在使用 HashMaps 时可以使用的有用方法。

您将学习如何在`HashMap`中添加、访问、删除和更新项目。

### 如何在 Java 中给一个`HashMap`添加项目

为了向`HashMap`添加项目，我们使用了`put()`方法。它接受两个参数——被添加的项的键和值。

它是这样工作的:

```
import java.util.HashMap;
class HashMapExample {
    public static void main(String[] args) {

        HashMap<Integer, String> StudentInfo = new HashMap<>();

        StudentInfo.put(1, "Ihechikara");
        StudentInfo.put(2, "Jane");
        StudentInfo.put(3, "John");

        System.out.println(StudentInfo);
        // {1=Ihechikara, 2=Jane, 3=John}
    }
}
```

在上面的代码中，`HashMap`被称为`StudentInfo`。我们将键指定为整数，而值是字符串:`HashMap<Integer, String>`。

为了向`HashMap`添加项目，我们使用了`put()`方法:

```
StudentInfo.put(1, "Ihechikara");
StudentInfo.put(2, "Jane");
StudentInfo.put(3, "John");
```

我们添加了三个条目，每个条目都有一个整数作为键，一个字符串作为它们的值。

### 如何在 Java 中访问`HashMap`中的项目

您可以使用`get()`方法来访问存储在`HashMap`中的项目。它接受一个参数——被访问项的键。

这里有一个例子:

```
import java.util.HashMap;
class HashMapExample {
    public static void main(String[] args) {
        HashMap<Integer, String> StudentInfo = new HashMap<>();

        StudentInfo.put(1, "Ihechikara");
        StudentInfo.put(2, "Jane");
        StudentInfo.put(3, "John");

        System.out.println(StudentInfo.get(2));
        // Jane
    }
}
```

在上面的例子中，`StudentInfo.get(2)`返回带有键`2`的值。“简”被打印到控制台上。

### 如何在 Java 中改变 HashMap 中项目的值

为了改变`HashMap`中条目的值，我们使用了`replace()`方法。它有两个参数——要更改的项目的键和要分配给它的新值。

```
import java.util.HashMap;
class HashMapExample {
    public static void main(String[] args) {
        HashMap<Integer, String> StudentInfo = new HashMap<>();

        StudentInfo.put(1, "Ihechikara");
        StudentInfo.put(2, "Jane");
        StudentInfo.put(3, "John");

        // Update key 1
        StudentInfo.replace(1, "Doe");

        System.out.println(StudentInfo);
        // {1=Doe, 2=Jane, 3=John}
    }
}
```

当上面的`HashMap`获得分配给它的项目时，关键字为`1`的项目的值为“Ihechikara”。

我们使用`replace()`方法将它的值改为“Doe”:`StudentInfo.replace(1, "Doe");`

### 如何在 Java 中删除`HashMap`中的项目

您可以使用`remove()`方法从`HashMap`中删除一个项目。它接受一个参数—要删除的项目的键。

```
import java.util.HashMap;
class HashMapExample {
    public static void main(String[] args) {
        HashMap<Integer, String> StudentInfo = new HashMap<>();

        StudentInfo.put(1, "Ihechikara");
        StudentInfo.put(2, "Jane");
        StudentInfo.put(3, "John");

        // Remove key 1
        StudentInfo.remove(1);

        System.out.println(StudentInfo);
        // {2=Jane, 3=John}
    }
}
```

使用`remove()`方法，我们删除了键为`1`的项目。

如果你想一次删除一个`HashMap`中的所有条目，你可以使用`clear()`方法。那就是:

```
import java.util.HashMap;
class HashMapExample {
    public static void main(String[] args) {
        HashMap<Integer, String> StudentInfo = new HashMap<>();

        StudentInfo.put(1, "Ihechikara");
        StudentInfo.put(2, "Jane");
        StudentInfo.put(3, "John");

        // Remove all items
        StudentInfo.clear();

        System.out.println(StudentInfo);
        // {}
    }
}
```

还有其他有用的方法，如:

*   如果一个指定的键存在于一个`HashMap`中，它返回`true`。
*   `containsValue`如果一个指定的值存在于一个`HashMap`中，它将返回`true`。
*   `size()`返回一个`HashMap`中的项数。
*   `isEmpty()`如果一个`HashMap`中没有任何条目，它将返回`true`，依此类推。

## 摘要

在本文中，我们讨论了 Java 中的`HashMap`。首先，我们讲了一个`HashMap`的特点。

然后，我们看到了如何创建一个`HashMap`，以及一些可以用来与存储在其中的数据进行交互的方法，并给出了代码示例。

编码快乐！