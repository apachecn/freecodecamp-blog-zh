# 解释了 Java 中的继承

> 原文：<https://www.freecodecamp.org/news/inheritance-in-java-explained/>

# **继承**

Java 继承指的是一个 Java 类从其他类继承属性的能力。把它想象成一个孩子从父母那里继承财产，这个概念与此非常相似。在 Java 行话中，它也被称为*扩展* -ing 类。

需要记住一些简单的事情:

*   扩展或继承的类称为 ****子类****
*   被扩展或继承的类被称为 ****超类****

因此，继承给了 Java 很酷的能力，即*重用*代码，或者在类之间共享代码！

让我们用一个经典的`Vehicle`类和一个`Car`类的例子来描述它:

```
public class Vehicle {
    public void start() {
        // starting the engine
    }

    public void stop() {
        // stopping the engine
    }
}

public class Car extends Vehicle {
    int numberOfSeats = 4;

    public int getNumberOfSeats() {
        return numberOfSeats;
    }
}
```

在这里，我们可以看到`Car`类继承了`Vehicle`类的属性。因此，我们不必为方法`start()`和`Car`编写相同的代码，因为这些属性可以从其父类或超类中获得。因此，从`Car`类创建的对象将*也*具有那些属性！

```
Car tesla = new Car();

tesla.start();

tesla.stop();
```

[运行代码](https://repl.it/CJXz/0)

但是，父类有子类的方法吗？不，不是的。

因此，每当您需要在多个类之间共享一些公共代码时，拥有一个父类总是好的，然后在需要时扩展该类！减少代码行数，使代码模块化，并简化测试。

## **什么可以遗传？**

*   来自父级的所有`protected`和`public`字段和方法

## 什么是不能遗传的？

*   `private`字段和方法
*   构造函数。尽管子类构造函数*有*来调用超类构造函数(如果它被定义的话)(后面会有更多内容！)
*   多个类。Java 只支持 ****单继承**** ，即一次只能继承一个类。
*   字段。子类不能覆盖类的单个字段。

## **型铸造&参考**

在 Java 中，可以将子类作为其超类的*实例*来引用。在面向对象编程(OOP)中，这被称为*多态性*，一个对象具有多种形式的能力。例如，`Car`类对象可以被引用为`Vehicle`类实例，如下所示:

```
Vehicle car = new Car();
```

虽然，相反是不可能的:

```
Car car = new Vehicle(); // ERROR
```

[运行代码](https://repl.it/CJYB/0)

因为可以将 Java 子类作为超类实例引用，所以可以很容易地将子类对象的实例转换为超类实例。将超类对象转换成子类类型是可能的，但是只有当该对象确实是子类的实例时*。所以请记住这一点:*

```
Car car = new Car();
Vehicle vehicle = car; // upcasting
Car car2 = (Car)vechile; //downcasting

Bike bike = new Bike(); // say Bike is also a subclass of Vehicle
Vehicle v = bike; // upcasting, no problem here.
Car car3 = (Car)bike; // Compilation Error : as bike is NOT a instance of Car
```

[运行代码](https://repl.it/CJYM/0)

现在您知道了如何通过父子关系共享代码。但是，如果您不喜欢子类中特定方法的实现，并想为它编写一个新的方法，该怎么办呢？那你会怎么做？

## 覆盖它！

Java 让你*覆盖*或者重新定义超类中定义的方法。例如，你的`Car`类与父类`Vehicle`有不同的`start()`实现，所以你这样做:

```
public class Vehicle {
    public void start() {
      System.out.println("Vehicle start code");
    }
}

public class Car extends Vehicle {
    public void start() {
      System.out.println("Car start code");
  }
}

Car car = new Car();
car.start(); // "Car start code"
```

[运行代码](https://repl.it/CJYZ/1)

因此，在子类中覆盖方法非常简单。虽然，有*赶*。只有与子类方法具有*完全相同方法签名*的超类方法将被覆盖。这意味着子类方法定义必须具有完全相同的名称、相同的参数数量和类型，以及完全相同的序列。因此，`public void start(String key)`不会覆盖`public void start()`。

****备注**** :

*   您不能重写超类的私有方法。(相当明显，不是吗？)
*   如果你在子类中覆盖的超类的方法突然被删除或者方法被改变了怎么办？它会在运行时失败！所以 Java 为您提供了一个漂亮的注释`@Override`，您可以将它放在子类方法上，这将警告编译器这些事件！

Java 中的注释是一种很好的编码实践，但它们不是必需的。不过，编译器足够聪明，能够自己解决覆盖问题。与其他 OOP 语言不同，Java it 中的注释不一定要修改方法或添加额外的功能。

## **如何调用超类方法？**

你问得真有趣！只需使用关键字`super`:

```
public class Vehicle() {
    public void start() {
      System.out.println("Vehicle start code");
    }
}

public class Car extends Vehicle {
    public void run() {
      super.start();
  }
}

Car car = new Car();
car.run(); // "Vehicle start code"
```

[运行代码](https://repl.it/CJY4/0)

****注意:**** :虽然您可以通过使用`super`调用来调用父方法，但是您不能通过链接的`super`调用来提升继承层次。

## **如何知道一个类的类型？**

使用`instanceof`关键字。有许多类和子类，在运行时要知道哪个类是哪个类的子类会有点混乱。所以，我们可以使用`instanceof`来确定一个对象是一个类的实例，一个子类的实例，还是一个接口的实例。

```
Car car = new Car();

boolean flag = car instanceof Vehicle; // true in this case!
```

## **构造函数&继承**

如前所述，构造函数不能被子类直接继承。虽然，子类是*要求*调用其父类的构造函数作为[在自己的构造函数中的第一个操作](http://stackoverflow.com/questions/1168345/why-does-this-and-super-have-to-be-the-first-statement-in-a-constructor)。怎么会？你猜对了，用`super`:

```
public class Vehicle {
    public Vehicle() {
        // constructor
    }
    public void start() {
      System.out.println("Vehicle start code");
    }
}

public class Car extends Vehicle {
    public Car() {
      super();
    }
    public void run() {
      super.start();
  }
}
```

[运行代码](https://repl.it/CJY8/0)

请记住，如果超类没有定义任何构造函数，您不必在子类中显式调用它。Java 在内部为您处理这个问题！调用`super`构造函数是在用除了*默认构造函数*之外的任何其他构造函数调用超类的情况下完成的。

如果没有定义其他构造函数，那么 Java 调用默认的超类构造函数(*，即使没有显式定义*)。

恭喜，现在你知道所有关于继承的事情了！阅读更多关于在抽象类和[接口](https://forum.freecodecamp.com/t/java-docs-interfaces)中继承事物的高级方法！