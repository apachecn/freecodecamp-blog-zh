# Java Enum–Java 示例中的枚举

> 原文：<https://www.freecodecamp.org/news/java-enum-enumeration-in-java-example/>

Java 中的枚举(简称 enum)是一种特殊的数据类型，它包含一组预定义的常数。

当处理不需要改变的值时，比如一周中的日子、一年中的季节、颜色等等，通常会使用`enum`。

在本文中，我们将看到如何创建一个`enum`以及如何将它的值赋给其他变量。我们还将看到如何在`switch`语句中使用`enum`或者遍历它的值。

## 如何用 Java 创建枚举

为了创建一个`enum`，我们使用`enum`关键字，类似于使用`class`关键字创建一个类。

这里有一个例子:

```
enum Colors {
  RED,
  BLUE,
  YELLOW,
  GREEN
}
```

在上面的代码中，我们创建了一个名为`Colors`的`enum`。您可能会注意到这个`enum`的值都是大写的——这只是一个通用的约定。如果值是小写的，就不会出现错误。

`enum`中的每个值由逗号分隔。

接下来，我们将创建一个新变量，并将我们的`enum`中的一个值赋给它。

```
enum Colors {
  RED,
  BLUE,
  YELLOW,
  GREEN
}

public class Main { 
  public static void main(String[] args) { 

    Colors red = Colors.RED; 

    System.out.println(red); 
    // RED
  } 
} 
```

这类似于初始化任何其他变量。在上面的代码中，我们初始化了一个`Colors`变量，并使用点语法:`Colors red = Colors.RED;`将一个`enum`的值赋给它。

注意，我们可以在`Main`类中创建我们的`enum`,代码仍然可以工作。那就是:

```
public class Main { 
  enum Colors {
  RED,
  BLUE,
  YELLOW,
  GREEN
}
  public static void main(String[] args) { 

    Colors red = Colors.RED; 

    System.out.println(red); 
  } 
} 
```

如果我们想得到任何值的索引号，我们必须使用`ordinal()`方法。这里有一个例子:

```
enum Colors {
  RED,
  BLUE,
  YELLOW,
  GREEN
}

public class Main { 
  public static void main(String[] args) { 

    Colors red = Colors.RED; 

    System.out.println(red.ordinal()); 
    // 0
  } 
} 
```

`red.ordinal()`从上面的代码中返回 0。

## 如何在 Switch 语句中使用枚举

在这一节中，我们将看到如何在`switch`语句中使用`enum`。

这里有一个例子:

```
 public class Main { 
      enum Colors {
      RED,
      BLUE,
      YELLOW,
      GREEN
  }
  public static void main(String[] args) { 

    Colors myColor = Colors.YELLOW;

    switch(myColor) {
      case RED:
        System.out.println("The color is red");
        break;
      case BLUE:
         System.out.println("The color is blue");
        break;
      case YELLOW:
        System.out.println("The color is yellow");
        break;
      case GREEN:
        System.out.println("The color is green");
        break;
    }
  } 
} 
```

这是一个关于如何在`switch`语句中使用`enum`的非常基本的例子。我们会将“The color is yellow”打印到控制台上，因为这是唯一匹配`switch`语句条件的`case`。

## 如何循环遍历枚举的值

Java 中的`enum`有一个`values()`方法，返回一个`enum`的值数组。我们将使用 for-each 循环来迭代并打印我们的`enum`的值。

我们可以这样做:

```
enum Colors {
  RED,
  BLUE,
  YELLOW,
  GREEN
}

public class Main { 
  public static void main(String[] args) { 

      for (Colors allColors : Colors.values()) {
      System.out.println(allColors);

      /* 
      RED
      BLUE
      YELLOW
      GREEN
      */
    }

  } 
} 
```

## 结论

在本文中，我们了解了 Java 中的`enum`是什么，如何创建它，以及如何将它的值赋给其他变量。

我们还看到了如何使用带有`switch`语句的`enum`类型，以及如何循环遍历`enum`的值。

编码快乐！