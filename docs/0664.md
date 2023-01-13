# Java 中的 hello World–示例程序

> 原文：<https://www.freecodecamp.org/news/hello-world-in-java-example-program/>

当你学习一门新的编程语言时，你经常会看到第一个程序叫做“Hello World”。它在大多数情况下被用作初学者的简单程序。

我假设您要么是作为 Java 编程语言的初学者阅读这篇文章，要么是还记得以前的 Hello World 程序。无论哪种方式，这将是简单和直截了当的。

本文不仅包括 Java 中的 hello world 程序，我们还将讨论一些作为初学使用 Java 的人应该知道的术语。

为了跟进，您需要一个集成开发环境(IDE)。这是您编写和编译代码的地方。你可以在你的电脑上安装一个，或者如果你不想完成安装过程，你可以使用任何在线 IDE。

## Java 的 Hello World 程序

在本节中，我们将创建一个简单的 Hello World 程序。然后我们将分解它，这样你就会明白它是如何工作的。

代码如下:

```
class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!"); 
        // Hello World!
    }
}
```

上面例子中的代码将打印“Hello World！”在控制台里。这在代码中被注释掉了。我们稍后将讨论注释。

让我们分解代码。

### Java 中的类

在 Java 中，类充当整个应用程序的构建块。不同的功能可以有不同的类。

类也可以有属性和方法来定义类的内容和作用。

一个例子是人类类。它可以有头发颜色、高度等属性。它可以有像跑、吃和睡这样的方法。

在我们的 Hello World 程序中，我们有一个名为`HelloWorld`的类。按照惯例，类的名称总是以大写字母开头。

要创建一个类，可以使用`class`关键字，后跟类名。下面是一个使用我们 Hello World 计划的示例:

```
class HelloWorld {

}
```

### Java 中的`main`方法

每个 Java 程序都必须有一个`main`方法。当 Java 编译器开始执行我们的代码时，它从`main`方法开始。

下面是`main`方法的样子:

```
public static void main(String[] args) {

    } 
```

为了使这篇文章简单，我们将不讨论上面找到的其他关键字，如`public`、`static`和`void`。

### `System.out.println()`声明

我们使用`System.out.println()`语句将信息打印到控制台。该语句需要一个参数。参数写在括号之间。

下面是语法:

```
System.out.println(Argument) 
```

在我们的例子中，我们传入了“Hello World！”作为论据。你会注意到文本被引号包围了。这告诉编译器参数是一个`string`。编程中的字符串只是字符的集合——就像我们写普通文本一样，但是它们必须用引号括起来。

下面是我们的代码:

```
class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!"); 
        // Hello World!
    }
} 
```

当我们运行这段代码时，“Hello World”将被打印出来。

不会打印在上面的代码块里面。我使用了`// Hello World!`来展示代码的输出。编译器不会执行这部分代码，因为它是一个注释。

在 Java 中，我们使用两个正斜杠(`//`)来开始单行注释。

## 结论

在本文中，我们讨论了 Java 中的 Hello World 程序。

我们从创建程序开始，然后分解它以理解用于创建程序的每一行代码。

我们讨论了 Java 中的类、`main`方法、`System.out.println()`语句、字符串和注释。

编码快乐！