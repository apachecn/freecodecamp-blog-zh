# 坚实的定义——解释了面向对象设计的坚实原则

> 原文：<https://www.freecodecamp.org/news/solid-principles-single-responsibility-principle-explained/>

**坚实的**设计原则帮助我们创建可维护的、可重用的、灵活的软件设计。首字母缩写 **SOLID** 中的每个字母都代表一个特定的原则。

以下是首字母缩略词中每个字母的含义:

*   **S** :单一责任原则。
*   **O** :开闭原理。
*   **L** :利斯科夫替代原理。
*   **I** :界面分离原理。
*   **D** :依存倒置原则。

在本文中，我们将从定义每个原则开始，然后我们将看到一些例子来帮助您理解您应该如何以及为什么在您的代码中使用这些原则。

我们将在这篇文章中使用的例子将是非常基本的。我们将使用 Java 作为例子。

我们将通过谈论面向对象设计的基础来结束这篇文章。

## 单一责任原则

SRP 背后的思想是程序中的每个类、模块或函数在程序中都应该有一个职责/目的。作为一个常用的定义，“每一个阶层都应该只有一个改变的理由”。

考虑下面的例子:

```
public class Student {

     public void registerStudent() {
         // some logic
     }

     public void calculate_Student_Results() {
         // some logic
     }

     public void sendEmail() {
         // some logic
     }

}
```

上面的类违反了单一责任原则。为什么？

这个`Student`类有三个职责——注册学生，计算他们的成绩，给学生发邮件。

上面的代码将完美地工作，但会导致一些挑战。我们不能让这些代码在其他类或对象中重用。这个类有很多相互关联的逻辑，我们很难修复错误。随着代码库的增长，逻辑也在增长，这使得理解正在发生的事情变得更加困难。

想象一下，一个新的开发人员加入了一个具有这种逻辑的团队，他们的代码库大约有 2000 行代码，全部集中在一个类中。

现在让我们来解决这个问题！

```
public class StudentRegister {
    public void registerStudent() {
        // some logic
    }
} 
```

```
public class StudentResult {
    public void calculate_Student_Result() {
        // some logic
    }
}
```

```
public class StudentEmails {
    public void sendEmail() {
        // some logic
    }
} 
```

现在我们已经将程序中的每个功能分开。我们可以在代码中任何需要的地方调用这些类。

我们使用的例子只是显示了每个类有一个方法——这主要是为了简单。你可以拥有任意多的方法，但是它们应该与类的责任相联系。

现在我们已经分离了逻辑，我们的代码更容易理解，因为每个核心功能都有自己的类。我们可以更有效地测试错误。

代码现在可以重用了。以前，我们只能在一个类中使用这些功能，但是现在它们可以在任何类中使用。

代码还易于维护和扩展，因为我们不必阅读相互关联的代码行，而是将关注点分开，这样我们就可以专注于我们想要处理的特性。

## 开闭原则(OCP)

开闭原则声明软件实体应该对扩展开放，但对修改关闭。

这意味着这样的实体——类、函数等等——应该以这样的方式创建，即它们的核心功能可以扩展到其他实体，而不需要改变初始实体的源代码。

在下面的例子中，我们将编写计算一个人的身体质量指数(身体质量指数)的代码:

```
public class Human  {

     public int height;
     public int weight;

}
```

我们已经创建了`Human`类，它提供了该类的`height`和`width`属性。现在，让我们计算第一个人的身体质量指数。

```
public class CalculateBMI {

     public int CALCULATE_JOHN_BMI(Human John) {   

         return John.height/John.weight;

     }
}
```

我们已经计算了一个名叫`John`的人的身体质量指数。我们继续计算一个叫`Jane`的人的身体质量指数。

```
public class CalculateBMI {

     public int CALCULATE_JOHN_BMI(Human John) {   

         return John.height/John.weight;

     }

     public int CALCULATE_JANE_BMI(Human Jane) {   

         return Jane.height/Jane.weight;

     }
}
```

这样做的问题是，每次我们需要计算另一个人的身体质量指数时，我们都要修改代码。

这也违反了 SRP，因为类现在有不止一个改变的理由。

尽管上面的代码可能工作得很完美，但它并不高效。我们不断修改代码，这可能会导致错误。法典只对人类有规定。如果我们必须为一个动物或物体计算呢？

让我们用开闭原理来解决这个问题。

```
public interface Entity {

     public int CalculateBMI();

}

// John entity
public class John implements Entity {

     int height;
     int weight;

     public double CalculateBMI() {

           return John.height/John.weight;
     }
}

// Jane entity
public class Jane implements Entity {

     int height;
     int weight;

     public double CalculateBMI() {

           return Jane.height/Jane.weight;
     }
}

// Dog entity
public class Dog implements Entity {

     int height;
     int weight;

     public double CalculateBMI() {

           return Dog.height/Dog.weight;
     }
}
```

在上面的代码中，我们用一个`CalculateBMI()`方法创建了一个名为`Entity`的接口。

每个实体——`John`、`Jane`和`Dog`——都扩展了`Entity`接口的功能。

现在，当我们创建一个新的实体时，我们不再需要修改现有的代码——我们只需要扩展我们需要的功能，并将其应用到新的实体中。

接下来，我们将讨论利斯科夫替代原理。

## 利斯科夫替代原理

根据 Barbara Liskov 和 Jeannette Wing 的观点，Liskov 替代原则指出:

> *设*φ(x)*是关于类型 *T* 的对象 *x* 的一个可证性质。那么*φ(y)*对于类型 *S* 的对象 *y* 应该为真，其中 *S* 是 *T* 的子类型。(来源:* [维基百科](https://en.wikipedia.org/wiki/Liskov_substitution_principle#:~:text=Subtype%20Requirement%3A%20Let,a%20subtype%20of%20T) *)*

如果你觉得困惑，不要担心，很快就会明白的。下面我们来简化一下这个原理:

Liskov 替换原则仅仅意味着当一个类的实例被传递/扩展到另一个类时，继承类应该有一个被继承类的所有属性和行为的用例。

比方说，我们有一个名为`Amphibian`的类，是关于既能生活在陆地上也能生活在水中的动物的。这个类有两种方法来展示两栖动物的特征——`swim()`和`walk()`。

```
public class Amphibian {

    public void swim();
    public void walk();

} 
```

因为青蛙是两栖动物，所以`Amphibian`类可以扩展到`Frog`类，所以它们可以继承`Amphibian`类的属性，而不改变类的逻辑和目的。

```
public class Frog extends Amphibian {
    public void swim() {
        System.out.println("The frog is swimming");
    }

    public void walk() {
        System.out.println("The frog is walking on land");
    }
}
```

但是我们不能将`Amphibian`类扩展到`Dolphin`类，因为海豚只生活在水中，这意味着`walk()`方法与`Dolphin`类无关。

所以，当你扩展一个类的时候，如果初始类的一些属性对新类没有用，那就违背了利斯科夫替换原理。

解决这个问题的方法很简单:创建符合继承类需求的接口。

总的来说，如果一个类继承了另一个类，它应该以这样的方式继承，即被继承类的所有属性都保持与其功能相关。

## 接口隔离原则(ISP)

接口分离原则规定程序的接口应该以这样一种方式分离，即用户/客户只能访问与他们的需求相关的必要方法。

为了更好地理解这一点，我们先来看一个违反 ISP 的例子:

```
public interface Teacher {

    void English();

    void Biology();

    void Chemistry();

    void Mathematics();

}
```

我们创建了一个名为`Teacher`的接口，它有各种各样的主题作为它的方法。让我们把这个接口扩展到我们的第一位老师。

```
public class Jane implements Teacher {

    @Override
    public void English() {
        System.out.println("Jane is teaching the students English language.");
    }

    @Override
    public void Biology() {
    }

    @Override
    public void Chemistry() {
    }

    @Override
    public void Mathematics() {
    }
}
```

从上面的代码中，你可以看出`Jane`是一名英语老师，与其他科目没有任何关系。但是默认情况下，这些其他方法是用`Teacher`接口扩展的。

不要混淆利斯科夫替代原理和界面分离原理。它们可能看起来相似，但并不完全相同。

Liskov 替换原则给了我们一个想法，当一个新类需要继承一个现有类时，它应该这样做，因为这个新类需要现有类的方法。

另一方面，接口分离原则让我们明白，用许多方法创建一个接口是不必要的，也是不合理的，因为当扩展时，这些方法中的一些可能与特定用户的需求无关。

现在让我们修复上一个例子中的代码。

```
public interface Teacher {

    void Teach();

}
```

`Teacher`接口现在只有一个方法。让我们继续扩展这个接口来支持不同的主题。

```
// English teacher interface

public interface EnglishTeacher extends Teacher {

    void English();

}
```

```
// Biology teacher interface

public interface BiologyTeacher extends Teacher {

    void Bilogy();

}
```

```
// Chemistry teacher interface

public interface ChemistryTeacher extends Teacher {

    void Chemistry();

}
```

```
// Mathematics teacher interface

public interface MathematicsTeacher extends Teacher {

    void Mathematics();

}
```

我们为每个主题创建了不同的界面。现在`Jane`可以不用其他方法来教英语了。这里有一个例子:

```
public class Jane implements EnglishTeacher {

    @Override
    public void Teach() {
        System.out.println("Jane has started teaching.");
    }

    @Override
    public void English() {
        System.out.println("Jane is teaching the students English language.");
    }

}
```

## 从属倒置原则

依赖性反转原则声明:

> 高级模块不应该从低级模块导入任何东西。两者都应该依赖于抽象(例如接口)。*(来源:* [维基](https://en.wikipedia.org/wiki/Liskov_substitution_principle#:~:text=Subtype%20Requirement%3A%20Let,a%20subtype%20of%20T) *)。*

而且，

> 抽象不应该依赖于细节。细节(具体的实现)应该依赖于抽象。*(来源:* [维基](https://en.wikipedia.org/wiki/Liskov_substitution_principle#:~:text=Subtype%20Requirement%3A%20Let,a%20subtype%20of%20T) *)。*

在编写一些代码之前，让我们使用一个真实的例子。

想象一下，每次你不得不在柜台取钱的时候，步行一分钟到银行。然后你需要额外的 30 秒才能拿到钱。这非常有效，因为浪费的时间很少。我们假设你是高级模块，银行是低级模块。

但是，当银行因假日或紧急情况而关闭时，会发生什么情况呢？你完全无法动用你的资金。如果你搬得离银行更远，问题就更大了，因为你要花更多的时间去那里。

为了解决这个问题，引入了一个接口——自动柜员机(ATM)或移动银行应用程序。即使您与银行有关系，您也不再需要与他们进行身体接触来获得服务。

这个例子类似于依赖性反转原理。我们应该让我们的类依赖于接口中的属性，而不是相互依赖。

违反这一原则的后果将导致一个僵化的系统，其中独立测试代码块将非常困难，代码的可重用性将几乎不可能，而代码的添加或删除将导致系统的进一步复杂化并引入错误。

下面是一个违反这一原则的代码示例:

```
public class Bank {

    public void GIVE_CUSTOMER_MONEY_OTC() {
        // some logic
    }
}
```

```
public class Customer {
    private Bank myBank = new Bank();

    public void withdraw() {
        myBank.GIVE_CUSTOMER_MONEY_OTC();
    }
}
```

从上面的代码示例中，我们可以看到`Customer`类导入并依赖于`Bank`类中的一个方法。这种对底层阶级的依赖是与衰退背道而驰的。

就像我们现实生活中的例子一样，我们将通过引入一个两个类都可以交互的接口来解决这个问题。

下面是我们的`Bank`和`Customer`类将与之交互的 ATM 接口:

```
public interface ATM {
    void ATM_OPERATION();
}
```

这里有一个`Bank`类，它使用了`ATM`接口中的一个方法将钱添加到 ATM 中:

```
public class Bank implements ATM {
    @Override
    ATM_OPERATION(){
        // code to add money to ATM and increase the ATM balance
    }
}
```

最后，使用相同接口取款的`Customer`类:

```
public class Customer implements ATM {

    @Override
    ATM_OPERATION(){
        // code to withdraw money from ATM and decrease the ATM balance
    }
}
```

## 什么是面向对象设计？

面向对象设计是一种用于构建基于对象的系统和应用程序的设计方法。这使我们能够用一组对象构建系统，其中每个对象都有自己的属性和方法。

以计算机系统为例。它的硬件由组成整个系统的不同部分组成。

以下是一些与面向对象设计相关的通用术语:

*   **对象**:组成系统的每个独立单元都是一个对象。对象可以有属性和方法。
*   **类**:类作为对象的一般描述。所以一个对象是一个类的实例。
*   **封装**:这有助于将一个对象的所有相关数据捆绑在一个单元中。这也有助于限制对只能在一个对象中找到的特定数据和方法的访问。
*   **继承**:继承让我们更容易将一个类的功能扩展到其他类。这样，我们就不会一遍又一遍地重复创建这些功能的过程。
*   抽象:这意味着只显示重要的属性，隐藏不相关的属性。
*   **多态**:这是一个接口以各种形式存在。扩展对象/接口的能力，但具有不同的或附加的属性。

## 结论

解决一个问题有很多方法。但是也有很多方法可以从解决方案中产生问题。

代码中的类和方法越严格、越耦合，维护和重用代码就越困难。

忽视或违反这些原则不仅会对代码库和开发人员造成严重威胁，也会对拥有产品的组织造成严重威胁。

严格和紧密耦合的代码库使得在产品中添加或删除功能、测试和重用代码块变得困难，并且在每次代码修改时引入了可能的破坏性变化。

坚实的原则是帮助我们创造一个灵活和动态的产品的指南，我们在这篇文章中回顾了每个原则，以帮助我们理解我们创造的对象应该如何相互作用。

我希望这篇文章对你继续进行面向对象设计有所帮助。

编码快乐！