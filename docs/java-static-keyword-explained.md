# Java 静态关键字举例说明

> 原文：<https://www.freecodecamp.org/news/java-static-keyword-explained/>

# **静态是什么意思？**

当您将变量或方法声明为静态时，它属于类，而不是特定的实例。这意味着静态成员只有一个实例，即使您创建了该类的多个对象，或者您没有创建任何对象。它将由所有对象共享。

static 关键字可用于变量、方法、代码块和嵌套类。

## **静态变量**

*****举例:*****

```
public class Counter {
  public static int COUNT = 0;
  Counter() {
    COUNT++;
  }
}
```

该类的所有对象将共享`COUNT`变量。当我们在 main 中创建 Counter 类的对象，并访问静态变量时。

```
public class MyClass {
  public static void main(String[] args) {
    Counter c1 = new Counter();
    Counter c2 = new Counter();
    System.out.println(Counter.COUNT);
  }
}
// Outputs "2"
```

outout 是 2，因为`COUNT`变量是静态的，每次创建 Counter 类的一个新对象时，它就加 1。您还可以使用该类的任何对象来访问静态变量，比如`c1.COUNT`。

## **静态方法**

静态方法属于类而不是实例。因此，可以在不创建类实例的情况下调用它。它用于改变类的静态内容。静态方法有一些限制:

1.  静态方法不能使用类的非静态成员(变量或函数)。
2.  静态方法不能使用`this`或`super`关键字。

*****举例:*****

```
public class Counter {
  public static int COUNT = 0;
  Counter() {
    COUNT++;
  }

  public static void increment(){
    COUNT++;
  }
}
```

静态方法也可以从类的实例中调用。

```
public class MyClass {
  public static void main(String[] args) {
    Counter.increment();
    Counter.increment();
    System.out.println(Counter.COUNT);
  }
}
// Outputs "2"
```

输出是 2，因为它由静态方法`increament()`递增。与静态变量类似，静态方法也可以使用实例变量来访问。

## **静态块**

静态代码块用于初始化静态变量。这些块在静态变量声明后立即执行。

*****举例:*****

```
public class Saturn {
  public static final int MOON_COUNT;

  static {
    MOON_COUNT = 62;
  }
}
```

```
public class Main {
  public static void main(String[] args) {
    System.out.println(Saturn.MOON_COUNT);
  }
}
// Outputs "62"
```

输出是 62，因为变量`MOON_COUNT`在静态块中被赋予该值。

## **静态嵌套类**

一个类可以有静态嵌套类，可以通过使用外部类名来访问。

*****举例:*****

```
public class Outer {

  public Outer() {
  }

  public static class Inner {
    public Inner() {
    }
  }
}
```

在上面的例子中，类`Inner`可以作为类`Outer`的静态成员被直接访问。

```
public class Main {
  public static void main(String[] args) {
    Outer.Inner inner = new Outer.Inner();
  }
}
```

java 中普遍使用的[构建器模式](https://en.wikipedia.org/wiki/Builder_pattern#Java)中静态嵌套类的用例之一。