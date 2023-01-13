# 用例子解释 Java 中的抽象类

> 原文：<https://www.freecodecamp.org/news/abstract-classes-in-java-explained-with-examples/>

抽象类是用`abstract`声明的类。它们可以被子类化或扩展，但不能被实例化。你可以把它们想象成一个 ****类版本的**** 接口，或者是一个带有附加到方法上的实际代码的接口。

例如，假设您有一个类`Vehicle`，它定义了车辆共有的基本功能(方法)和组件(对象变量)。

你不能创建一个`Vehicle`的物体，因为车辆本身是一个抽象的、一般的概念。车辆有轮子和马达，但是轮子的数量和马达的大小可以有很大的不同。

但是，您可以扩展 vehicle 类的功能来创建一个`Car`或`Motorcycle`:

```
abstract class Vehicle
{
  //variable that is used to declare the no. of wheels in a vehicle
  private int wheels;

  //Variable to define the type of motor used
  private Motor motor;

  //an abstract method that only declares, but does not define the start 
  //functionality because each vehicle uses a different starting mechanism
  abstract void start();
}

public class Car extends Vehicle
{
  ...
}

public class Motorcycle extends Vehicle
{
  ...
}
```

记住，你不能在程序的任何地方实例化一个`Vehicle`——相反，你可以使用你之前声明的`Car`和`Motorcycle`类并创建它们的实例:

```
Vehicle newVehicle = new Vehicle();    // Invalid
Vehicle car = new Car();  // valid
Vehicle mBike = new Motorcycle();  // valid

Car carObj = new Car();  // valid
Motorcycle mBikeObj = new Motorcycle();  // valid
```

## 更多信息:

*   [学习 Java 函数式编程-全教程](https://www.freecodecamp.org/news/functional-programming-in-java-course/)
*   [Java 中的 Getters 和 Setters 解释](https://www.freecodecamp.org/news/java-getters-and-setters/)
*   [Java 中的继承解释](https://www.freecodecamp.org/news/inheritance-in-java-explained/)