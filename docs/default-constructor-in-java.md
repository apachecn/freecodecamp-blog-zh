# Java 中的默认构造函数–类构造函数示例

> 原文：<https://www.freecodecamp.org/news/default-constructor-in-java/>

在本文中，我们将讨论构造函数，如何创建自己的构造函数，以及 Java 中的默认构造函数。

## 什么是构造函数？

作为基于类的面向对象编程术语，构造函数是一种用于初始化新创建的对象(类)的独特方法。创建构造函数时，有几条规则必须遵守。这些规则包括:

*   构造函数的名称必须与类名相同。
*   构造函数不能有返回类型。

在我们继续之前，让我们看看 Java 中的类是什么样子的:

```
public class Student {
  String firstName;
  String lastName;
  int age;
}
```

上面的代码显示了一个名为 Student 的类，它有三个属性—`firstName`、`lastName`和`age`。我们将假设该班级是注册学生的样本。回想一下，这三个属性没有任何值，因此没有任何信息是硬编码的。

现在我们将使用构造函数来创建我们的`Student`对象的一个新实例。那就是:

```
public class Student {
  String firstName;
  String lastName;
  int age;

  //Student constructor
  public Student(){
      firstName = "Ihechikara";
      lastName = "Abba";
      age = 100;
  }

  public static void main(String args[]) {
      Student myStudent = new Student();
      System.out.println(myStudent.age);
      // 100
  }
}
```

我们已经创建了一个构造函数，用来初始化在`Student`对象中定义的属性。上面的代码是一个**无参数**构造函数的例子。现在让我们看一个不同类型的例子:

```
public class Student {
  String firstName;
  String lastName;
  int age;

  //constructor
  public Student(String firstName, String lastName, int age){
      this.firstName = firstName;
      this.lastName = lastName;
      this.age = age;
  }

  public static void main(String args[]) {
    Student myStudent = new Student("Ihechikara", "Abba", 100);
    System.out.println(myStudent.age);
  }

}
```

现在我们已经创建了一个**参数化的构造函数**。参数化构造函数是用参数创建的构造函数。我们来分解一下。

```
public Student(String firstName, String lastName, int age){

  }
```

我们创建了一个新的构造函数，它接受三个参数——两个字符串和一个整数。

```
this.firstName = firstName;
this.lastName = lastName;
this.age = age;
```

然后，我们将这些参数与创建类时定义的属性联系起来。现在我们已经使用构造函数初始化了学生对象。

```
public static void main(String args[]) {
    Student myStudent = new Student("Ihechikara", "Abba", 100);
    System.out.println(myStudent.age);
  }
```

最后，我们创建了 Student 对象的一个新实例，并传入了我们的参数。我们能够传入这些参数，因为我们已经在构造函数中定义了它们。

我创建了一个带有三个参数的构造函数，但是您也可以创建单独的构造函数来初始化每个属性。

现在你知道了什么是 Java 中的构造函数以及如何使用它，让我们来看看默认的构造函数。

## 什么是默认构造函数？

如果我们没有为一个类定义任何构造函数，默认构造函数就是编译器创建的构造函数。这里有一个例子:

```
public class Student {
  String firstName;
  String lastName;
  int age;

  public static void main(String args[]) {
      Student myStudent = new Student();

      myStudent.firstName = "Ihechikara";
      myStudent.lastName = "Abba";
      myStudent.age = 100;

      System.out.println(myStudent.age);
      //100

      System.out.println(myStudent.firstName);
      //Ihechikara
  }
}
```

你能看出这个例子和前两个例子之间的区别吗？注意，在创建`myStudent`来初始化在类中创建的属性之前，我们没有定义任何构造函数。

这不会给我们带来错误。相反，编译器会创建一个空的构造函数，但是你不会在代码的任何地方看到这个构造函数——这是在幕后发生的。

当编译器开始工作时，上面的代码看起来是这样的:

```
public class Student {
  String firstName;
  String lastName;
  int age;

  /* empty constructor created by compiler. This constructor will not be seen in your code */
  Student() {

  }

  public static void main(String args[]) {
      Student myStudent = new Student();

      myStudent.firstName = "Ihechikara";
      myStudent.lastName = "Abba";
      myStudent.age = 100;

      System.out.println(myStudent.age);
      //100

      System.out.println(myStudent.firstName);
      //Ihechikara
  }
}
```

很多人把默认构造函数和无参数构造函数搞混了，但是在 Java 中它们是不一样的。程序员创建的任何构造函数都不被认为是 Java 中的默认构造函数。

## 结论

在本文中，我们学习了什么是构造函数，以及如何创建和使用它们来初始化我们的对象。

我们还讨论了默认构造函数以及它们与无参数构造函数的不同之处。

编码快乐！