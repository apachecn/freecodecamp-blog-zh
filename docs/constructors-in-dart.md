# Dart 中的构造函数——用例及示例

> 原文：<https://www.freecodecamp.org/news/constructors-in-dart/>

我们大多数人都熟悉构造函数的概念。它们允许我们创建类的不同实例。我们可以指定类在被实例化时应该依赖哪些参数，并隐藏内部初始化逻辑。

对于不同的用例，我们可以有许多构造函数，或者我们可以依赖默认的构造函数。

在 dart 中，构造函数扮演着类似的角色，但是有一些在大多数编程语言中不存在的变化。本文将介绍构造函数的不同用例及例子。

在本文的所有示例中，我们将使用下面的类:

```
class Car {
   String make;
   String model;
   String yearMade;
   bool hasABS;
}
```

## 如何开始使用 Dart 中的构造函数

如果您没有在 Dart 中指定任何构造函数，它将为您创建一个默认的构造函数。

这并不意味着您将看到类中生成的默认构造函数。相反，当创建类的新实例时，将调用此构造函数。它将没有参数，并将调用超类的构造函数，也没有参数。

若要在类中声明构造函数，可以执行以下操作:

```
class Car {
	String make;
   	String model;
   	String yearMade;
   	bool hasABS;

   	Car(String make, String model, int year, bool hasABS) {
    	this.make = make;
      	this.model = model;
      	this.yearMade = year;
      	this.hasABS = hasABS;
   	}
}
```

可以想象，一定有更好的方法来初始化我们的类字段——在 Dart 中，有:

```
class Car {
	String make;
   	String model;
   	String yearMade;
   	bool hasABS;

   	Car(this.make, this.model, this.yearMade, this.hasABS);
}
```

我们上面使用的方法只是 Dart 用来简化赋值的语法糖。

![image-321](img/592d553749d13eb4b6b9c29d80fe3d6d.png)

Photo by [Alessio Lin](https://unsplash.com/@lin_alessio?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

## 更复杂的构造函数特性

在其他语言中，重载构造函数是可能的。这意味着您可以有不同的构造函数，它们具有相同的名称，但是具有不同的签名(或者不同的参数集)。

### Dart 中的命名构造函数

在 Dart 中，这是不可能的，但是有一种方法可以绕过它。它被称为**命名构造函数**。给你的构造函数取不同的名字允许你的类有许多构造函数，也可以更好地在类外表示它们的用例。

```
class Car {
	String make;
   	String model;
   	String yearMade;
   	bool hasABS;

   	Car(this.make, this.model, this.yearMade, this.hasABS);

   	Car.withoutABS(this.make, this.model, this.yearMade): hasABS = false;
}
```

Named Constructor Example

不带 tABS 的构造函数**在构造函数体执行之前，将实例变量 hasABS 初始化为 false。这就是所谓的**初始化列表**，你可以初始化几个变量，用逗号隔开。**

初始化列表最常见的用例是初始化你的类声明的最终字段。

> ✋任何放在冒号(:)右边的东西都无法访问**这个**。

### Dart 中的工厂构造函数

工厂构造函数是一种构造函数，当您不需要构造函数来创建类的新实例时，可以使用它。

如果您将类的实例保存在内存中，并且不想每次都创建新的实例(或者创建实例的操作开销很大)，这可能会很有用。

另一个用例是，如果在构造函数中有某种逻辑来初始化一个 final 字段，而这在初始化列表中是做不到的。

```
class Car {
	String make;
   	String model;
   	String yearMade;
   	bool hasABS;

   	factory Car.ford(String model, String yearMade, bool hasABS) {
    	return FordCar(model, yearMade, hasABS);
    }
}

class FordCar extends Car {
	FordCar(String model, String yearMade, bool hasABS): super("Ford", model, yearMade, hasABS);

}
```

Factory Constructor Example

## Dart 中的高级构造函数

### Dart 中的常量构造函数和重定向构造函数

Dart 还允许您创建常量构造函数。这到底是什么意思？如果您的类表示一个在创建后永远不会改变的对象，您可以从常量构造函数的使用中受益。你必须确保你所有的类字段都是最终的。

```
class FordFocus {
   static const FordFocus fordFocus = FordFocus("Ford", "Focus", "2013", true);

   final String make;
   final String model;
   final String yearMade;
   final bool hasABS;

   const FordFocus(this.make, this.model, this.yearMade, this.hasABS);

}
```

Constant Constructor Example

当你希望一个构造函数调用另一个构造函数时，这被称为**重定向构造函数**。

```
class Car {
	String make;
   	String model;
   	String yearMade;
   	bool hasABS;

   	Car(this.make, this.model, this.yearMade, this.hasABS);

   	Car.withoutABS(this.make, this.model, this.yearMade): this(make, model, yearMade, false);
}
```

Redirecting Constructor Example

## 包扎

我们讨论的每个构造函数都有不同的用途和用例。由您来决定和理解何时使用每一个。希望这篇文章给了你这样做的必要知识。