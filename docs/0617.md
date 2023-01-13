# Java Switch 语句–如何在 Java 中使用 Switch Case

> 原文：<https://www.freecodecamp.org/news/java-switch-statement-how-to-use-a-switch-case-in-java/>

在 Java 中，当满足特定条件时，使用`switch`语句来执行特定的代码块。

下面是语法的样子:

```
switch(expression) {
  case 1:
    // code block
    break;
  case 2:
    // code block
    break;
    case 3:
    // code block
    break;
  default:
    // code block
}
```

上面，`switch`括号中的`expression`与每个`case`进行了比较。当`expression`与`case`相同时，执行`case`中相应的代码块。

如果所有案例都不匹配`expression`，那么在`default`关键字下定义的代码块将被执行。

每当满足某个条件时(当`expression`与`case`匹配时)，我们使用`break`关键字来终止代码。

让我们看一些代码示例。

## 如何在 Java 中使用 Switch Case

看一下下面的代码:

```
class CurrentMonth {
    public static void main(String[] args) {

        int month = 6;

        switch (month) {
          case 1:
            System.out.println("January");
            break;
          case 2:
            System.out.println("February");
            break;
          case 3:
            System.out.println("March");
            break;
          case 4:
            System.out.println("April");
            break;
          case 5:
            System.out.println("May");
            break;
          case 6:
            System.out.println("June");
            break;
          case 7:
            System.out.println("July");
            break;
          case 8:
            System.out.println("August");
            break;
          case 9:
            System.out.println("September");
            break;
          case 10:
            System.out.println("October");
            break;
          case 11:
            System.out.println("November");
            break;
          case 12:
            System.out.println("December");
            break;

            // June
        }
    }
}
```

在上面的代码中，输出了六月。不要担心庞大的代码。这里有一个分类帮助你理解:

我们创建了一个名为`month`的整数，并给它赋值 6:`int month = 6;`。

接下来，我们创建了一个`switch`语句，并将`month`变量作为一个参数传入:`switch (month){...}`。

作为`switch`语句表达式的`month`值将与代码中的每个`case`值进行比较。我们有案例 1 到 12。

`month`的值是 6，所以它与`case` 6 匹配。这就是为什么执行了`case` 6 中的代码。其他代码块都被忽略了。

下面是另一个简化问题的例子:

```
class Username {
    public static void main(String[] args) {

        String username = "John";

        switch (username) {
          case "Doe":
            System.out.println("Username is Doe");
            break;
          case "John":
            System.out.println("Username is John");
            break;
          case "Jane":
            System.out.println("Username is Jane");
            break;
            // Username is John
        }
    }
}
```

在上面的例子中，我们创建了一个名为`username`的字符串，它的值为“John”。

在`switch`语句中，`username`作为表达式传入。然后，我们创建了三个案例——“Doe”、“John”和“Jane”。

在这三个类中，只有一个与`username`的值相匹配——“John”。结果，`case "John"`中的代码块被执行。

## 如何在 Switch 语句中使用 Default 关键字

在前一节的例子中，我们的代码被执行是因为一个`case`匹配一个`expression`。

在这一节中，您将看到如何使用`default`关键字。在没有一个案例与`expression`匹配的情况下，您可以使用它作为后备。

这里有一个例子:

```
class Username {
    public static void main(String[] args) {

        String username = "Ihechikara";

        switch (username) {
          case "Doe":
            System.out.println("Username is Doe");
            break;
          case "John":
            System.out.println("Username is John");
            break;
          case "Jane":
            System.out.println("Username is Jane");
            break;
          default:
            System.out.println("Username not found!");
            // Username not found!
        }
    }
}
```

上例中的`username`变量的值为“Ihechikara”。

将执行关键字`default`的代码块，因为创建的案例都不匹配`username`的值。

## 摘要

在本文中，我们看到了如何在 Java 中使用`switch`语句。

我们还讨论了 Java 中的`switch`语句的表达式、cases 和 default 关键字，以及它们的用例和代码示例。

编码快乐！