# 用例子解释 Java 接口

> 原文：<https://www.freecodecamp.org/news/java-interfaces-explained-with-examples/>

# **接口**

Java 中的接口有点像类，但是有一个显著的不同:一个`interface`可以有*只有*有方法签名、字段和默认方法。从 Java 8 开始，你还可以创建[默认方法](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html)。在下一个模块中，您可以看到一个接口示例:

```
public interface Vehicle {
    public String licensePlate = "";
    public float maxVel
    public void start();
    public void stop();
    default void blowHorn(){
      System.out.println("Blowing horn");
   }
}
```

上面的接口包含两个字段、两个方法和一个默认方法。单独使用的话，用处不大，但是它们通常和类一起使用。怎么会？简单，你必须确定一些类`implements`它。

```
public class Car implements Vehicle {
    public void start() {
        System.out.println("starting engine...");
    }
    public void stop() {
        System.out.println("stopping engine...");
    }
}
```

现在，有一个 ****基本规则**** :这个类必须实现接口中所有 方法的 ****。这些方法必须具有与接口中描述的*完全相同的*签名(名称、参数和异常)。类*不需要声明字段，只需要声明方法。*****

## **一个接口的实例**

一旦你创建了一个 Java 类，它`implements`任何一个接口，这个对象实例都可以作为这个接口的一个实例被引用。这个概念类似于继承实例化的概念。

```
// following our previous example

Vehicle tesla = new Car();

tesla.start(); // starting engine ...
```

一个接口 ****不能**** 包含一个构造函数方法。因此，你 ****不能**** 创建一个接口本身的实例。您必须创建实现接口的某个类的实例来引用它。

把接口想象成一个空白的合同表单，或者一个模板。

你能用这个特性做什么？多态性！只能用接口来引用对象实例！

```
class Truck implements Vehicle {
    public void start() {
        System.out.println("starting truck engine...");
    }
    public void stop() {
        System.out.println("stopping truck engine...");
    }
}

class Starter {
    // static method, can be called without instantiating the class
    public static void startEngine(Vehicle vehicle) {
        vehicle.start();
    }
}

Vehicle tesla = new Car();
Vehicle tata = new Truck();

Starter.startEngine(tesla); // starting engine ...
Starter.startEngine(tata); // starting truck engine ...
```

## **但是多接口呢？**

是的，你可以在一个类中实现多个接口。在[继承](https://forum.freecodecamp.com/t/java-docs-inheritance)中，你被限制只能继承一个类，而在这里你可以扩展任意数量的接口。但是不要忘记实现所有接口的方法的 *all* ，否则编译会失败！

```
public interface GPS {
    public void getCoordinates();
}

public interface Radio {
    public void startRadio();
    public void stopRadio();
}

public class Smartphone implements GPS,Radio {
    public void getCoordinates() {
        // return some coordinates
    }
    public void startRadio() {
      // start Radio
    }
    public void stopRadio() {
        // stop Radio
    }
}
```

## **接口的一些特性**

*   你可以在一个接口中放置变量，尽管这不是一个明智的决定，因为类不一定有相同的变量。简而言之，避免放置变量！
*   一个接口中的所有变量和方法都是公共的，即使你省略了`public`关键字。
*   接口不能指定特定方法的实现。这要靠班级来做。尽管最近有一个例外(见下文)。
*   如果一个类实现了多个接口，那么方法签名重叠的可能性极小。由于 Java 不允许多个方法具有完全相同的签名，这可能会导致问题。更多信息见[这个问题](http://stackoverflow.com/questions/2598009/method-name-collision-in-interface-implementation-java)。

## **界面默认方法**

在 Java 8 之前，我们没有办法指导一个接口拥有一个特定的方法实现。如果接口定义突然改变，这会导致许多混乱和代码中断。

假设，你写了一个开源库，里面包含一个接口。比方说，你的客户，也就是世界上几乎所有的开发者，都在大量使用它，并且很开心。现在，您必须通过向接口添加新的方法定义来升级库，以支持新的特性。但是这会破坏所有的构建，因为所有实现该接口的类现在都必须改变。多么大的灾难！

幸运的是，Java 8 现在为我们提供了接口的`default`方法。一个`default`方法*可以*在接口内直接包含自己的实现*！因此，如果一个类没有实现一个默认的方法，编译器将采用接口中提到的实现。很好，不是吗？所以在你的库中，你可以在接口中添加任意数量的默认方法，而不用担心会破坏任何东西！*

```
public interface GPS {
    public void getCoordinates();
    default public void getRoughCoordinates() {
        // implementation to return coordinates from rough sources
        // such as wifi & mobile
        System.out.println("Fetching rough coordinates...");
    }
}

public interface Radio {
    public void startRadio();
    public void stopRadio();
}

public class Smartphone implements GPS,Radio {
    public void getCoordinates() {
        // return some coordinates
    }
    public void startRadio() {
      // start Radio
    }
    public void stopRadio() {
        // stop Radio
    }

    // no implementation of getRoughCoordinates()
}

Smartphone motoG = new Smartphone();
motog.getRoughCoordinates(); // Fetching rough coordinates...
```

但是，如果两个接口有相同的方法签名会怎么样呢？

很棒的问题。在这种情况下，如果您不在类中提供实现，糟糕的编译器将会感到困惑和失败！您还必须在类中提供一个默认的方法实现。还有一种使用`super`调用您喜欢的实现的好方法:

```
public interface Radio {
    // public void startRadio();
    // public void stopRadio();

    default public void next() {
        System.out.println("Next from Radio");
    }
}

public interface MusicPlayer {
    // public void start();
    // public void pause();
    // public void stop();

    default public void next() {
        System.out.println("Next from MusicPlayer");
    }
}

public class Smartphone implements Radio, MusicPlayer {
    public void next() {
        // Suppose you want to call MusicPlayer next
        MusicPlayer.super.next();
    }
}

Smartphone motoG = new Smartphone();
motoG.next(); // Next from MusicPlayer
```

## **接口中的静态方法**

Java 8 的另一个新特性是向接口添加静态方法的能力。接口中的静态方法与具体类中的静态方法几乎相同。唯一的区别是`static`方法没有在实现接口的类中被继承。这意味着调用静态方法时引用的是接口，而不是实现它的类。

```
interface MusicPlayer {
  public static void commercial(String sponsor) {
    System.out.println("Now for a message brought to you by " + sponsor);
  }

  public void play();
}

class Smartphone implements MusicPlayer {
	public void play() {
		System.out.println("Playing from smartphone");
	}
}

class Main {
  public static void main(String[] args) {
    Smartphone motoG = new Smartphone();
    MusicPlayer.commercial("Motorola"); // Called on interface not on implementing class
    // motoG.commercial("Motorola"); // This would cause a compilation error 
  }
}
```

## **继承一个接口**

在 Java 中，一个接口继承另一个接口也是可能的，通过使用`extends`关键字:

```
public interface Player {
    public void start();
    public void pause();
    public void stop();
}

public interface MusicPlayer extends Player {
    default public void next() {
        System.out.println("Next from MusicPlayer");
    }
}
```

也就是说，实现`MusicPlayer`接口的类必须实现`MusicPlayer`和`Player`的所有方法*:*

```
public class SmartPhone implements MusicPlayer {
    public void start() {
        System.out.println("start");
    }
    public void stop() {
        System.out.println("stop");
    }
    public void pause() {
        System.out.println("pause");
    }
}
```

所以现在你已经很好的掌握了 Java 接口！去学习抽象类，看看 Java 如何为您提供另一种定义契约的方式。