# Java 面向对象编程——初学者指南

> 原文：<https://www.freecodecamp.org/news/object-oriented-programming-concepts-java/>

嗨，伙计们！今天我们要谈谈 Java 中的面向对象编程。

本文将帮助您彻底理解面向对象编程的基本原则及其概念。

一旦您理解了这些概念，您就应该有信心和能力使用 Java 中的面向对象编程原则开发基本的解决问题的应用程序。

## 什么是面向对象编程？

面向对象编程(OOP)是一种基于*、*、对象*、*概念的基本编程范式。这些对象可以包含字段形式的数据(通常称为属性或特性)和过程形式的代码(通常称为方法)。

面向对象方法的核心概念是将复杂的问题分解成更小的对象。

**在本文中，我们将探讨以下 OOP 概念:**

1.  Java 是什么？
2.  什么是课？
3.  什么是对象？
4.  什么是 Java 虚拟机(JVM)？
5.  访问修饰符在 Java 中的工作方式。
6.  Java 中构造函数的工作方式。
7.  Java 中方法的工作原理。
8.  面向对象的关键原则。
9.  Java 中的接口。

## Java 是什么？

Java 是一种通用的、基于类的、面向对象的编程语言，可以在 Windows、Mac 和 Linux 等不同的操作系统上工作。

您可以使用 Java 来开发:

*   桌面应用程序
*   网络应用
*   移动应用程序(尤其是 Android 应用程序)
*   Web 和应用服务器
*   大数据处理
*   嵌入式系统

还有更多。

在 Java 中，每个应用程序都以一个类名开始，这个类必须与文件名匹配。保存文件时，使用类名保存并添加 *"* 。java 的 *"* 以文件名结尾。

让我们编写一个 Java 程序，打印消息*"*Hello freeCodeCamp community。我叫...*。*

*我们将从创建第一个名为 Main.java 的 Java 文件开始，这可以在任何文本编辑器中完成。创建并保存文件后，我们将使用下面几行代码来获得预期的输出。*

```
*`public class Main 
{
  public static void main(String[] args) 
  {
    System.out.println("Hello freeCodeCamp community. My name is Patrick Cyubahiro.");
  }
}`* 
```

*如果你暂时不理解上面的代码，也不要担心。我们将一步步地讨论下面的每一行代码。*

*现在，我希望您从注意到 Java 中运行的每一行代码都必须在一个类中开始。*

*您可能还注意到 Java 是区分大小写的。这意味着 Java 有能力区分大小写字母。比如变量 *"* myClass *"* 和变量 *"* myclass *"* 是两个完全不同的东西。*

***好了，让我们看看这段代码在做什么:***

*我们先来看看`main()`的方法:`public static void main(String[] args)`。*

*每个 Java 程序都需要这个方法，而且它是最重要的一个，因为它是任何 Java 程序的入口点。*

*它的语法总是`public static void main(String[] args)`。唯一可以更改的是字符串数组参数的名称。比如你可以把`args`改成`myStringArgs`。*

## *Java 中的类是什么？*

*类被定义为对象的集合。你也可以把一个类想象成一个蓝图，从这个蓝图中你可以创建一个单独的对象。*

*为了创建一个类，我们使用关键字`class`。*

### *Java 中类的语法:*

```
*`class ClassName {
  // fields
  // methods
}`*
```

*在上面的语法中，我们有字段(也称为变量)和方法，它们分别代表对象的状态和行为。*

***注意**在 Java 中，我们使用字段存储数据，而使用方法执行操作。*

### *让我们举个例子:*

*我们将创建一个名为 *"* Main *"* 的类，带有一个变量 *"* y *"* 。变量 *"* y *"* 将存储值 2。*

```
*`public class Main { 

int y = 2; 

}`*
```

*注意，类应该总是以大写的第一个字母开头，Java 文件应该与类名匹配。*

## *Java 中的对象是什么？*

*对象是现实世界中可以被清楚识别的实体。对象有状态和行为。换句话说，它们由使特定类型的数据有用的方法和属性组成。*

*一个对象包括:*

*   ***唯一标识:**每个对象都有唯一标识，即使状态与另一个对象的状态相同。*
*   ***状态/属性/属性:**状态告诉我们物体的样子或者它有什么属性。*
*   ***行为:**行为告诉我们对象做了什么。*

### *Java 中对象状态和行为的示例:*

*让我们看一些现实生活中的例子，看看对象可能具有的状态和行为。*

#### *示例 1:*

*   *对象:汽车。*
*   *状态:颜色，品牌，重量，型号。*
*   *行为:刹车、加速、转弯、换挡。*

#### *示例 2:*

*   *对象:房子。*
*   *状态:地址、颜色、位置。*
*   *行为:开门，关门，打开百叶窗。*

### *Java 中对象的语法:*

```
*`public class Number {

int y = 10;

public static void main(String[] args) {

Number myObj = new Number();

System.out.println(myObj.y);

}

}`* 
```

## *什么是 Java 虚拟机(JVM)？*

*Java 虚拟机(JVM)是使计算机能够运行 Java 程序的虚拟机。*

*JVM 有两个主要功能，它们是:*

*   *允许 Java 程序在任何设备或操作系统上运行(这也称为“一次编写，随处运行”原则)。*
*   *以及管理和优化程序内存。*

## *访问修饰符在 Java 中如何工作*

*在 Java 中，访问修饰符是设置类、方法和其他成员的可访问性的关键字。*

*这些关键字决定了一个类中的字段或方法是否可以被另一个类或子类中的另一个方法使用或调用。*

*访问修饰符也可以用来限制访问。*

*在 Java 中，我们有四种类型的访问修饰符，它们是:*

*   *默认*
*   *公共*
*   *私人的*
*   *保护*

*现在让我们更详细地看一看每一个。*

### *默认访问修饰符*

*默认访问修饰符也被称为包私有。你用它来使同一个包内的所有成员可见，但是他们只能在同一个包内被访问。*

*请注意，当没有为类、方法或数据成员指定或声明访问修饰符时，它会自动采用默认的访问修饰符。*

*下面是一个如何使用默认访问修饰符的示例:*

```
*`class SampleClass 
{
    void output() 
       { 
           System.out.println("Hello World! This is an Introduction to OOP - 			Beginner's guide."); 
       } 
} 
class Main
{ 
    public static void main(String args[]) 
       {   
          SampleClass obj = new SampleClass(); 
          obj.output();  
       } 
}`*
```

***现在让我们看看这段代码在做什么:***

*`void output()`:没有访问修饰符时，程序自动取默认修饰符。*

*这一行代码允许程序使用默认的访问修饰符来访问类。*

*这一行代码允许程序使用默认的访问修饰符来访问类方法。*

*输出为:`Hello World! This is an Introduction to OOP - Beginner's guide.`。*

### *公共访问修饰符*

***公共访问修饰符**允许从 Java 程序中的任何类或包访问类、方法或数据字段。公共访问修饰符既可以在包内访问，也可以在包外访问。*

*一般来说，公共访问修饰符根本不限制实体。*

*下面是如何使用公共访问修饰符的示例:*

```
*`// Car.java file
// public class
public class Car {
    // public variable
    public int tireCount;

    // public method
    public void display() {
        System.out.println("I am a Car.");
        System.out.println("I have " + tireCount + " tires.");
    }
}

// Main.java
public class Main {
    public static void main( String[] args ) {
        // accessing the public class
        Car car = new Car();

        // accessing the public variable
        car.tireCount = 4;
        // accessing the public method
        car.display();
    }
}`*
```

***输出:***

```
*`I am a Car.

I have 4 tires.`*
```

***现在让我们看看代码中发生了什么:***

*在上面的例子中，*

*   *公共类`Car`是从主类访问的。*
*   *从主类中访问公共变量`tireCount`。*
*   *从主类访问公共方法`display()`。*

### *私有访问修饰符*

***私有访问修饰符**是具有最低可访问性级别的访问修饰符。这意味着声明为私有的方法和字段在类外部是不可访问的。它们只能在以这些私有实体为成员的类中访问。*

*您可能还会注意到，私有实体甚至对该类的子类都是不可见的。*

*下面是一个例子，说明如果您试图在类外访问声明为私有的变量和方法会发生什么:*

```
*`class SampleClass 
{

    private String activity;
}

public class Main 
{

    public static void main(String[] main)
    {

        SampleClass task = new SampleClass();

        task.activity = "We are learning the core concepts of OOP.";
    }
}`*
```

*好吧，这是怎么回事？*

1.  *私有访问修饰符使变量“activity”成为私有变量。*
2.  *我们已经创建了一个 SampleClass 对象。*
3.  *在这一行代码中，我们试图从另一个类中访问私有变量和字段(由于私有访问修饰符的原因，这个类永远不能被访问)。*

*当我们运行上面的程序时，我们会得到下面的错误:*

```
*`Main.java:49: error: activity has private access in SampleClass
        task.activity = "We are learning the core concepts of OOP.";
            ^
1 error`*
```

*这是因为我们试图从另一个类中访问私有变量和字段。*

*因此，访问这些私有变量的最佳方式是使用 getter 和 setter 方法。*

*Getters 和 setters 用于保护数据，尤其是在创建类的时候。当我们为每个实例变量创建一个 getter 方法时，该方法返回它的值，而 setter 方法设置它的值。*

*让我们看看如何使用 getters 和 setters 方法来访问私有变量。*

```
*`class SampleClass 
{

    private String task;

    // This is the getter method.
    public String getTask() 
    {

        return this.task;
    }

    // This is the setter method.
    public void setTask(String task) 
    {

        this.task= task;
    }
}

public class Main 
{

    public static void main(String[] main)
    {

        SampleClass task = new SampleClass();

        // We want to access the private variable using the getter and 				   setter.

        task.setTask("We are learning the core concepts of OOP.");

        System.out.println(task.getTask());
    }
}`*
```

*当我们运行上面的程序时，输出如下:*

```
*`We are learning the core concepts of OOP.`*
```

*在上面的例子中，我们有一个名为`task`的私有变量，为了从外部类访问这个变量，我们使用了方法`getTask()`和`setTask()`。这些方法在 Java 中被称为 getter 和 setter。*

*我们使用 setter 方法(`setTask()`)为变量赋值，使用 getter 方法(`getTask()`)访问变量。*

*要了解更多关于关键词`this`的信息，你可以在这里阅读这篇文章[。](https://www.programiz.com/java-programming/this-keyword)*

### *受保护的访问修饰符*

*当方法和数据成员被声明为`protected`时，我们可以在同一个包中以及从子类中访问它们。*

*我们也可以说,`protected`访问修饰符在某种程度上类似于默认访问修饰符。只是它有一个例外，就是它在子类中的可见性。*

*请注意，类不能声明为 protected。这个访问修饰符通常用在父子关系中。*

*让我们看看如何使用受保护的访问修饰符:*

```
*`// Multiplication.java

package learners;

public class Multiplication 
{

   protected int multiplyTwoNumbers(int a, int b)
   {

	return a*b;

   }

}

// Test.java

package javalearners;

import learners.*;

class Test extends Multiplication
{

   public static void main(String args[])
   {

	Test obj = new Test();

	System.out.println(obj.multiplyTwoNumbers(2, 4));

   }

} //output: 8`*
```

*这段代码在做什么？*

*在这个例子中，存在于另一个包中的类`Test`能够调用被声明为 protected 的`multiplyTwoNumbers()`方法。*

*该方法之所以能够这样做，是因为`Test`类扩展了类添加，而`protected`修饰符允许访问子类中的`protected`成员(在任何包中)。*

## *Java 中的构造函数是什么？*

*Java 中的构造函数是一种用来初始化新创建的对象的方法。*

### *Java 中构造函数的语法:*

```
*`public class Main { 

int a;  

public Main() { 

a = 3 * 3; 

} 

public static void main(String[] args) { 

Main myObj = new Main();

System.out.println(myObj.a); 

} 

}`* 
```

*那么这段代码是怎么回事呢？*

1.  *我们从创建`Main`类开始。*
2.  *之后，我们创建了一个类属性，就是变量`a`。*
3.  *第三，我们为主类创建了一个类构造函数。*
4.  *之后，我们为已经声明的变量`a`设置了初始值。变量`a`的值将为 9。我们的程序只需要 3 乘以 3，等于 9。你可以随意给变量`a`赋值。(在编程中，符号“*”表示乘法)。*

*每个 Java 程序都在`main()`方法中开始执行。所以，我们使用了`public static void main(String[] args)`，这是程序开始执行的地方。换句话说，`main()`方法是每个 Java 程序的入口点。*

*现在我将解释`main()`方法中每个关键字的作用。*

#### *public 关键字。*

***public** 关键字是一个**访问修饰符**。它的作用是指定从哪里可以访问该方法，以及谁可以访问它。因此，当我们将`main()`方法公开时，它就可以在全球范围内使用。换句话说，程序的所有部分都可以访问它。*

#### *静态关键字。*

*当一个方法用一个**静态**关键字声明时，它被称为静态方法。所以，Java `main()`方法总是**静态的**，这样编译器可以在没有创建类的对象之前调用它。*

*如果允许`main()`方法是非静态的，那么 Java 虚拟机将不得不在调用`main()`方法时实例化它的类。*

*static 关键字也很重要，因为它节省了不必要的内存浪费，这些内存浪费会被 Java 虚拟机声明只用于调用`main()`方法的对象所使用。*

#### *Void 关键字。*

***void** 关键字是用来指定一个方法不返回任何东西的关键字。每当`main()`方法不需要返回任何东西时，它的返回类型就是 void。因此，这意味着一旦`main()`方法终止，Java 程序也会终止。*

#### *主要的。*

***Main** 是 Java main 方法的名称。它是 java 虚拟机寻找的作为 Java 程序起点的标识符。*

#### *`String[] args`。*

*这是一个存储 Java 命令行参数的字符串数组。*

*下一步是创建 Main 类的对象。我们已经创建了一个调用类构造函数的函数调用。*

*最后一步是打印`a`的值，就是 9。*

## *方法如何在 Java 中工作*

*方法是执行特定任务的代码块。在 Java 中，我们使用术语“方法”,但在其他一些编程语言(如 C++)中，同样的方法通常被称为函数。*

*在 Java 中，有两种类型的方法:*

*   ***用户定义的方法**:这些是我们可以根据自己的需求创建的方法。*
*   ***标准库方法**:这些是 Java 中的内置方法，可以使用。*

*让我给你一个在 Java 中如何使用方法的例子。*

### *Java 方法示例 1:*

```
*`class Main {

  // create a method
  public int divideNumbers(int x, int y) {
    int division = x / y;
    // return value
    return division;
  }

  public static void main(String[] args) {

    int firstNumber = 4;
    int secondNumber = 2;

    // create an object of Main
    Main obj = new Main();
    // calling method
    int result = obj.divideNumbers(firstNumber, secondNumber);
    System.out.println("Dividing " + firstNumber + " by " + secondNumber + " is: " + result);
  }
}`*
```

***输出:***

```
*`Dividing 4 by 2 is: 2`*
```

*在上面的例子中，我们创建了一个名为`divideNumbers()`的方法。该方法有两个参数 x 和 y，我们通过传递两个参数`firstNumber`和`secondNumber`来调用该方法。*

*现在您已经了解了一些 Java 基础知识，让我们更深入地研究一下面向对象编程原则。*

## *面向对象编程的关键原则。*

*面向对象编程范例有四个主要原则。这些原则也被称为面向对象编程的支柱。*

*面向对象编程的四个主要原则是:*

1.  *封装(我还将简要地谈到信息隐藏)*
2.  *遗产*
3.  *抽象*
4.  *多态性*

### *Java 中的封装和信息隐藏*

*封装是将数据包装在一个单元下。简单来说，它或多或少就像一个保护罩，防止数据被这个保护罩之外的代码访问。*

*封装的一个简单例子是书包。书包可以把你所有的东西安全地放在一个地方，比如你的书、钢笔、铅笔、尺子等等。*

*编程中的信息隐藏或数据隐藏是关于保护数据或信息在整个程序中不被意外更改。这是一个强大的面向对象编程特性，它与封装密切相关。*

*封装背后的想法是确保对用户隐藏“**敏感的**”数据。为此，您必须:*

1.  *将类变量/属性声明为`private`。*
2.  *提供公共的`get`和`set`方法来访问和更新`private`变量的值。*

*正如您所记得的，`private`变量只能在同一个类中访问，外部类不能访问它们。然而，如果我们提供公共的`get`和`set`方法，就可以访问它们。*

*让我再给你一个例子，演示一下`get`和`set`方法是如何工作的:*

```
*`public class Student {
  private String name; // private = restricted access

  // Getter
  public String getName() {
    return name;
  }

  // Setter
  public void setName(String newName) {
    this.name = newName;
  }
}`*
```

### *Java 中的继承*

*继承允许类继承其他类的属性和方法。这意味着父类将属性和行为扩展到了子类。继承支持可重用性。*

*解释术语“继承”的一个简单例子是，人类(一般来说)从类“人”继承某些属性，例如说话、呼吸、吃、喝等等的能力。*

*我们将“继承概念”分为两类:*

*   ***子类**(child)——从另一个类继承的类。*
*   ***超类**(父类)——被继承的类。*

*为了继承一个类，我们使用了`extends`关键字。*

*在下面的例子中，`JerryTheMouse`类是通过继承`Animal`类的方法和字段创建的。*

*`JerryTheMouse`是子类，`Animal`是超类。*

```
*`class Animal {

  // field and method of the parent class
  String name;
  public void eat() {
    System.out.println("I can eat");
  }
}

// inherit from Animal
class JerryTheMouse extends Animal {

  // new method in subclass
  public void display() {
    System.out.println("My name is " + name);
  }
}

class Main {
  public static void main(String[] args) {

    // create an object of the subclass
    JerryTheMouse labrador = new JerryTheMouse();

    // access field of superclass
    mouse.name = "Jerry, the mouse";
    mouse.display();

    // call method of superclass
    // using object of subclass
    mouse.eat();

  }
}`*
```

***输出:***

```
*`My name is Jerry

I can eat`*
```

### *Java 中的抽象*

*抽象是面向对象编程中的一个概念，它让您只显示基本属性，隐藏代码中不必要的信息。抽象的主要目的是向用户隐藏不必要的细节。*

*解释抽象的一个简单例子是考虑当你发送一封电子邮件时发挥作用的过程。当你发送一封电子邮件时，复杂的细节，如邮件发送后会发生什么以及服务器使用的协议，对你是隐藏的。*

*当你发送一封电子邮件时，你只需要输入收件人的电子邮件地址，邮件主题，键入内容，然后点击发送。*

*你可以通过使用**抽象类**或者**接口**来抽象东西。*

*`abstract`关键字是一个非访问修饰符，用于类和方法:*

*   ***抽象类:**是一个受限类，不能用来创建对象。要访问它，它必须从另一个类继承。*
*   ***抽象方法:**没有实体的方法被称为抽象方法。我们使用相同的`abstract`关键字来创建抽象方法。*

*抽象方法的主体由子类提供(继承自)。*

***举例:***

```
*`// Abstract class
abstract class Animal {
  // Abstract method (does not have a body)
  public abstract void animalSound();
  // Regular method
  public void sleep() {
    System.out.println("Zzzz");
  }
}

// Subclass (inherit from Animal)
class Cow extends Animal {
  public void animalSound() {
    // The body of animalSound() is provided here
    System.out.println("The cow says: Moo");
  }
}

class Main {
  public static void main(String[] args) {
    Cow myCow = new Cow(); // Create a Cow object
    myCow.animalSound();
    myCow.sleep();
  }
}`*
```

### *Java 中的多态性*

*多态性指的是一个对象具有多种形式的能力。当我们有许多通过继承相互关联的类时，通常会出现多态性。*

*多态性类似于一个人可以同时具有不同的特征。*

*例如，一个男人可以同时是父亲、祖父、丈夫、雇员等等。所以，同一个人在不同的情况下拥有不同的特征或行为。*

***举例:***

*我们将创建对象 Cow 和 Cat，并对它们分别调用`animalSound()`方法。*

```
*`class Animal {
  public void animalSound() {
    System.out.println("An animal can make a sound.");
  }
}

class Cow extends Animal {
  public void animalSound() {
    System.out.println("A cow says: Moooo");
  }
}

class Cat extends Animal {
  public void animalSound() {
    System.out.println("A cat says: Meeooww");
  }
}

class Main {
  public static void main(String[] args) {
    Animal myAnimal = new Animal();
    Animal myCow = new Cow();
    Animal myCat = new Cat();

    myAnimal.animalSound();
    myCow.animalSound();
    myCat.animalSound();
  }
}`*
```

*继承和多态对于代码的可重用性非常有用。创建新类时，可以重用现有类的属性和方法。*

## *Java 中的接口*

*An `interface`是抽象方法的集合。换句话说，一个`interface`是一个完全的**抽象类**，用于将相关方法与空体组合在一起。*

*一个接口指定了一个类能做什么，但没有指定它如何做。*

***举例:***

```
*`// create an interface
interface Language {
  void getName(String name);
}

// class implements interface
class ProgrammingLanguage implements Language {

  // implementation of abstract method
  public void getName(String name) {
    System.out.println("One of my favorite programming languages is: " + name);
  }
}

class Main {
  public static void main(String[] args) {
    ProgrammingLanguage language = new ProgrammingLanguage();
    language.getName("Java");
  }
}`*
```

***输出:***

```
*`One of my favorite programming languages is: Java`*
```

## *结论*

*在本文中，我们已经研究了一些主要的面向对象编程概念。如果您想很好地使用这些概念并编写出好的代码，那么很好地理解这些概念是必不可少的。*

*我希望这篇文章是有帮助的。*

*我的名字是 Patrick Cyubahiro，我是一名软件和 web 开发人员，UI/UX 设计师，技术作家和社区建设者。*

*请随时在 Twitter 上与我联系:@ [Pat_Cyubahiro](https://twitter.com/Pat_Cyubahiro) ，或写信至:ampatrickcyubahiro[at]Gmail . com*

*感谢阅读，快乐学习！*