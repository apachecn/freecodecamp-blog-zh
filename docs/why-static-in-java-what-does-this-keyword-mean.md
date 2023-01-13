# 为什么在 Java 中是静态的？这个关键词是什么意思？[已解决]

> 原文：<https://www.freecodecamp.org/news/why-static-in-java-what-does-this-keyword-mean/>

您可以在 Java 程序的不同部分使用`static`关键字，比如变量、方法和静态块。

在 Java 中使用`static`关键字的主要目的是节省内存。当我们在一个类中创建一个将被其他类访问的变量时，我们必须首先创建该类的一个实例，然后给每个变量实例分配一个新值——即使新变量的值在所有新类/对象中应该是相同的。

但是当我们创建一个静态变量时，它的值在所有其他类中保持不变，我们不必创建一个实例来使用该变量。这样，我们只创建了一次变量，所以内存只分配了一次。

通过下面几节中的例子，您会更好地理解这一点。

为了理解什么是`static`关键字以及它实际上做什么，我们将看到一些例子，展示它在 Java 中声明静态变量、方法和块的用法。

## 如何在 Java 中创建静态变量

为了理解`static`关键字在创建变量中的用法，让我们看看创建在类的每个实例中共享的变量的常用方法。

```
class Student {
    String studentName; 
    String course; 
    String school;

    public static void main(String[] args) {
        Student Student1 = new Student();
        Student Student2 = new Student();

        Student1.studentName = "Ihechikara";
        Student1.course = "Data Visualization";
        Student1.school = "freeCodeCamp";

        Student2.studentName = "John";
        Student2.course = "Data Analysis with Python";
        Student2.school = "freeCodeCamp";

        System.out.println(Student1.studentName + " " + Student1.course + " " + Student1.school + "\n");
        // Ihechikara Data Visualization freeCodeCamp
        System.out.println(Student2.studentName + " " + Student2.course + " " + Student2.school);
        // John Data Analysis with Python freeCodeCamp
    }
}
```

我会一步一步解释上面的代码中发生了什么。

我们创建了一个名为`Student`的类，有三个变量——`studentName`、`course`和`school`。

然后我们创建了两个`Student`类的实例:

```
Student Student1 = new Student();
Student Student2 = new Student();
```

第一个实例–`Student1`–可以访问在它的类中创建的变量，其值如下:

```
Student1.studentName = "Ihechikara";
Student1.course = "Data Visualization";
Student1.school = "freeCodeCamp";
```

第二个实例具有以下值:

```
Student2.studentName = "John";
Student2.course = "Data Analysis with Python";
Student2.school = "freeCodeCamp";
```

如果你仔细观察，你会发现这两个学生有着相同的校名——“freeCodeCamp”。如果我们必须为同一所学校创造 100 名学生，会怎么样？这意味着我们要用相同的值初始化一个变量 100 次——每次都分配新的内存。

虽然这看起来不是问题，但在更大的代码库中，它可能会成为一个缺陷，不必要地降低程序的速度。

为了解决这个问题，我们将使用`static`关键字来创建`school`变量。之后，该类的所有实例都可以使用该变量。

方法如下:

```
class Student {
    String studentName; 
    String course; 
    static String school;

    public static void main(String[] args) {
        Student Student1 = new Student();
        Student Student2 = new Student();

        Student1.studentName = "Ihechikara";
        Student1.course = "Data Visualization";
        Student1.school = "freeCodeCamp";

        Student2.studentName = "John";
        Student2.course = "Data Analysis with Python";

        System.out.println(Student1.studentName + " " + Student1.course + " " + Student1.school + "\n");
        // Ihechikara Data Visualization freeCodeCamp
        System.out.println(Student2.studentName + " " + Student2.course + " " + Student2.school);
        // John Data Analysis with Python freeCodeCamp
    }
}
```

在上面的代码中，我们创建了一个名为`school`的静态变量。您会注意到`static`关键字位于数据类型和变量名称之前:`static String school = "freeCodeCamp";`。

现在，当我们创建类的新实例时，我们不必为每个实例初始化`school`变量。在我们的代码中，我们只给第一个实例中的变量赋值，第二个实例也继承了它。

请注意，在代码中任何地方更改静态变量的值都会覆盖代码中先前声明该变量的其他部分的值。

所以你应该只对程序中应该保持不变的变量使用`static`关键字。

您还可以在创建变量时给它赋值，这样您就不必在创建类实例时再次声明它:`static String school = "freeCodeCamp";`。

如果你使用上面的方法，你会得到这个:

```
class Student {
    String studentName; 
    String course; 
    static String school = "freeCodeCamp";

    public static void main(String[] args) {
        Student Student1 = new Student();
        Student Student2 = new Student();

        Student1.studentName = "Ihechikara";
        Student1.course = "Data Visualization";

        Student2.studentName = "John";
        Student2.course = "Data Analysis with Python";

        System.out.println(Student1.studentName + " " + Student1.course + " " + Student1.school + "\n");
        // Ihechikara Data Visualization freeCodeCamp
        System.out.println(Student2.studentName + " " + Student2.course + " " + Student2.school);
        // John Data Analysis with Python freeCodeCamp
    }
}
```

在最后一节中，您将看到如何使用静态块初始化静态变量。

## 如何在 Java 中创建静态方法

在我们看例子之前，有一些关于 Java 中静态方法的事情你应该知道:

*   静态方法只能**访问和修改静态变量。**
*   无需创建类实例就可以调用/使用静态方法。

这里有一个例子可以帮助你理解:

```
class EvenNumber {

    static int evenNumber;

    static void incrementBy2(){
        evenNumber = evenNumber + 2;
        System.out.println(evenNumber);
    }

    public static void main(String[] args) {
        incrementBy2(); // 2
        incrementBy2(); // 4
        incrementBy2(); // 6
        incrementBy2(); // 8
    }
}
```

在上面的代码中，我们在名为`EvenNumber`的类中创建了一个整数(`evenNumber`)。

我们的静态方法被命名为`incrementBy2()`。这个方法增加了整数`evenNumber`的值并打印出它的值。

在没有创建类实例的情况下，我们能够在程序的`main`方法中调用`incrementBy2()`方法。每次我们调用这个方法，`evenNumber`的值就会增加 2 并打印出来。

还可以在调用方法时使用点符号将类名附加到方法上:`EvenNumber.incrementBy2();`。每个静态方法都属于类，而不是类的实例。

## 如何在 Java 中创建一个静态块

Java 中的静态块类似于构造函数。我们可以用它们来初始化静态变量，它们由编译器在`main`方法之前执行。

```
class Block {

    static int year;

    static {
        year = 2022;
        System.out.println("This code block got executed first");
    }

    public static void main(String[] args) {

        System.out.println("Hello World");
        System.out.println(year);
    }
} 
```

在上面的代码中，我们创建了一个静态整数变量`year`。然后我们在一个静态块中初始化它:

```
static {
        year = 2022;
        System.out.println("This code block got executed first");
    }
```

您可以创建一个静态块，正如您在上面看到的，使用`static`关键字后跟花括号。在代码的静态块中，我们用值 2022 初始化了`year`变量。我们还打印出一些文本——“这个代码块首先被执行”。

在`main`方法中，我们打印了“Hello World”和静态的`year`变量。

在控制台中，代码将按以下顺序执行:

```
This code block got executed first
Hello World
2022
```

这演示了静态块中的代码是如何在`main`方法之前首先执行的。

## 摘要

在本文中，我们讨论了 Java 中的关键字`static`。它是一个关键字，主要帮助我们优化 Java 程序中的内存。

我们通过例子看到了如何创建静态变量和方法。

最后，我们讨论了可以用来初始化静态变量的静态块。静态块在 main 方法之前执行。

编码快乐！