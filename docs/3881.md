# 使用抽象类和接口的多态性

> 原文：<https://www.freecodecamp.org/news/polymorphism-using-abstract-class-and-interface/>

在本文中，您将学习如何使用抽象类和接口共享和执行具有多态性的代码。

我们将更深入地研究面向对象编程，并尝试从设计模式的角度来思考，使用多态性来共享和执行我们的代码。

### **抽象类**

假设我们有一个名为 Man 的类，它具有一些属性(`name`、`age`、`height`、`fav_drinks`和`fav_sports`)和方法(`giveFirmHandshakes`、`beStubborn`和`notPutToiletPaper`)。

```
<?php

class Man
{
    public $name;
    public $age;
    public $height;
    public $fav_sports;
    public $fav_drinks;

    public function __construct($name, $age, $height)
    {
        $this->name = $name;
        $this->age = $age;
        $this->height = $height;
    }

    public function giveFirmHandshakes()
    {
        return "I give firm handshakes.";
    }

    public function beStubborn()
    {
        return "I am stubborn.";
    }

    public function notPutToiletPaper()
    {
        return "It's not humanly possible to remember to put toilet paper rolls when they are finished";
    }
}
```

我们需要按照构造函数的要求指定姓名、年龄和身高来实例化这个类。

```
<?php
$jack = new Man('Jack', '26', '5 Feet 6 Inches');

echo sprintf('%s - %s - %s', $jack->name, $jack->age, $jack->height);
// => Jack - 26 - 5 Feet 6 Inches
```

现在，假设我们想给这个类添加一个名为 isActive 的新方法。

此方法检查属性是否活动，并根据活动的值返回适当的消息，默认值为 false。我们可以为那些活跃的人设置为真。

```
<?php

class Man
{
    public $name;
    public $age;
    public $height;
    public $fav_sports;
    public $fav_drinks;
    public $active = false;

    .....
    .....

    public function isActive()
    {
        if ($this->active == true) {
            return "I am an active man.";
        } else {
            return "I am an idle man.";
        }
    }
}

$jack = new Man('Jack', '26', '5 Feet 6 Inches');
$jack->active = true;
echo $jack->isActive();
// => I am an active man.

$jake = new Man('Jake', '30', '6 Feet');
echo "\n" . $jake->isActive();
// => I am an idle man.
```

如果一个男人不仅仅是活跃或者无所事事呢？

如果有一个从 1 到 4 的尺度来衡量一个男人有多活跃(1-空闲，2-轻度活跃，3-中度活跃，4-非常活跃)会怎么样？

我们可以有一个如果..埃尔塞夫..else 语句如下:

```
<?php

public function isActive()
{
    if ($this->active == 1) {
        return "I am an idle man.";
    } elseif ($this->active == 2) {
        return "I am a lightly active man.";
    } elseif ($this->active == 3) {
        return "I am a moderately active man.";
    } else {
        return "I am a very active man.";
    }
}
```

现在，让我们更进一步。

如果人的主动属性不仅仅是一个整数(1，2，3，4 等)呢？如果 active 的价值是“运动型”或者“懒人型”呢？

难道我们不需要添加更多的 elseif 语句来寻找这些字符串的匹配吗？

抽象类可以用于这样的场景。

对于抽象类，基本上是将类定义为抽象的，将想要实现的方法定义为抽象的，而不需要将任何代码放入这些方法中。

然后创建一个子类，扩展父抽象类，并在该子类中实现抽象方法。

这样，您将强制所有的子类定义它们自己的抽象方法版本。让我们看看如何将我们的`isActive()`方法设置为抽象方法。

## 1:将类定义为抽象类。

```
<?php
abstract class Man
{
.....
.....
}
```

## 2:为您希望在抽象类中实施的方法创建一个抽象方法。

```
<?php
abstract class Man
{
.....
.....
abstract public function isActive();
}
```

## 3:创建一个扩展抽象类的子类。

```
<?php

class AthleticMan extends Man
{
.....
.....
}
```

## 4:在子类内部实现抽象方法。

```
<?php
class AthleticMan extends Man
{
    public function isActive()
    {
        return "I am a very active athlete.";
    }
}
```

## 5:实例化子类(不是抽象类)。

```
<?php
$jack = new AthleticMan('Jack', '26', '5 feet 6 inches');
echo $jack->isActive();
// => I am a very active athlete.
```

完整的抽象类定义和实现代码:

```
<?php

abstract class Man
{
    public $name;
    public $age;
    public $height;
    public $fav_sports;
    public $fav_drinks;

    public function __construct($name, $age, $height)
    {
        $this->name = $name;
        $this->age = $age;
        $this->height = $height;
    }

    public function giveFirmHandshakes()
    {
        return "I give firm handshakes.";
    }

    public function beStubborn()
    {
        return "I am stubborn.";
    }

    public function notPutToiletPaper()
    {
        return "It's not humanly possible to remember to put toilet paper rolls when they are finished";
    }

    abstract public function isActive();
}

class AthleticMan extends Man
{
    public function isActive()
    {
        return "I am a very active athlete.";
    }
}

$jack = new AthleticMan('Jack', '26', '5 feet 6 inches');
echo $jack->isActive();
// => I am a very active athlete.
```

在这段代码中，你会注意到`isActive()`抽象方法是在`Man`抽象类中定义的，并且是在子类`AthleticMan`中实现的。

现在`Man`类不能直接实例化来创建对象。

```
<?php
$ted = new Man('Ted', '30', '6 feet');
echo $ted->isActive();
// => Fatal error:  Uncaught Error: Cannot instantiate abstract class Man
```

同样，抽象类(`Man` class)的每个子类都需要实现所有的抽象方法。缺少这样的实现将导致致命的错误。

```
<?php
class LazyMan extends Man
{

}

$robert = new LazyMan('Robert', '40', '5 feet 10 inches');
echo $robert->isActive();
// => Fatal error:  Class LazyMan contains 1 abstract method 
// => and must therefore be declared abstract or implement 
// => the remaining methods (Man::isActive)
```

通过使用抽象类，您可以强制某些方法由子类单独实现。

## 连接

还有另一个与抽象类密切相关的面向对象编程概念，叫做接口。

抽象类和接口的唯一区别在于，在抽象类中，你可以混合使用定义的方法(`giveFirmHandshakes()`、`isStubborn()`等)。)和父类内部的抽象方法(`isActive()`)。但是在接口中，你只能在父类内部定义(而不是实现)方法。

让我们看看如何将上面的 Man 抽象类转换成一个接口。

## 1:用所有方法定义接口(用接口代替类)。

```
<?php
interface Man
{
    public function __construct($name, $age, $height);

    public function giveFirmHandshakes();

    public function beStubborn();

    public function notPutToiletPaper();

    public function isActive();
}
```

## 2:创建一个实现接口的类(使用实现而不是扩展)。

这个类必须实现接口中定义的所有方法，包括构造函数方法。

```
<?php
class AthleticMan implements Man
{
    public $name;
    public $age;
    public $height;

    public function __construct($name, $age, $height)
    {
        $this->name = $name;
        $this->age = $age;
        $this->height = $height;
    }

    public function giveFirmHandshakes()
    {
        return "I give firm handshakes.";
    }

    public function beStubborn()
    {
        return "I am stubborn.";
    }

    public function notPutToiletPaper()
    {
        return "It's not humanly possible to remember to put toilet paper rolls when they are finished";
    }

    public function isActive()
    {
        return "I am a very active athlete.";
    }
}
```

## 3:实例化实现类(AthleticMan)

```
<?php
$jack = new AthleticMan('Jack', '26', '5 feet 6 inches');
echo $jack->isActive();
// => I am a very active athlete.
```

对于接口，您需要记住:

*   这些方法不能在接口内部实现。
*   变量(属性)不能在接口内部定义。
*   接口中定义的所有方法都需要在子(实现)类中实现。
*   所有必要的变量都需要在子类中定义。
*   Man 接口强制其实现类来实现接口中的所有方法。

那么，接口有什么用呢？

难道不能只创建一个新类 AthleticMan，创建所有的方法，而不实现接口吗？

这就是*设计模式*发挥作用的地方。

当有一个基类(`Man`)想要强制你做一些事情(构造一个对象，给定 FirmHandshakes，beStubborn，notPutToiletPaper 并检查你是否活动)但又不想告诉你具体怎么做的时候，就要用到接口。

您可以继续创建您认为合适的实现类。

只要所有的方法都实现了，`Man` interface 不在乎如何实现。

我们已经讨论了如何以及何时在 PHP 中使用抽象类和接口。使用这些 OOP 概念让具有不同功能的类共享相同的基础“蓝图”(抽象类或接口)被称为多态性。