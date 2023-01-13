# C#中的继承是如何工作的——代码示例

> 原文：<https://www.freecodecamp.org/news/inheritance-in-c-sharp/>

继承是面向对象编程的一个分支，它帮助您编写可重用的代码。它允许您将一个类的内容扩展到另一个类。

面向对象编程的其他支柱包括封装、多态和抽象。

在这篇文章中，我们将学习 C#中的继承以及 OOP 中的各种类型的继承。

## 什么是继承？

继承是面向对象编程(OOP)的关键特征之一。它只是一个类(子类或派生类)获取另一个类(基类、父类或超类)的属性、方法和字段的过程。

面向对象编程中的继承意味着您创建的类可以将其属性传递给其他类，而不必在新类中显式定义属性。

继承不仅确保了代码库的可重用性，还降低了代码的复杂性。

## C#中的继承类型

继承允许你建立相关类的家族。基类/父类定义了子类继承它的公共数据。您使用冒号运算符(:)来显示两个类之间的继承。

```
using System;

namespace LearningInheritance
{
    class ParentClass
    {
        //...
    }
    class ChildClass:ParentClass
    {
        //..
    }
} 
```

C#中有不同类型的继承。我们现在将讨论它们。

### C#中的单一继承

单一继承通常发生在两个类之间——基类和派生类。当一个类是从单亲类继承而来时，就会发生这种情况。

下面是一个代码示例，展示了 C#中的单一继承:

```
using System;

namespace LearningInheritance
{
    class ParentClass
    {
        public string name;
        public int Id = 9;

        public void displayParentClassDetails()
        {
            Console.WriteLine($"I am {name}");
            Console.WriteLine($"ID : {Id}");
        }
    }

    class ChildClass : ParentClass
    {
        public void getIdFromParentClass()
        {
            Console.WriteLine($"This is my ID : {Id}");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            //accessing the inherited members from the child class
            ChildClass child = new ChildClass();
            child.getIdFromParentClass();
        }
    }
} 
```

在上面的代码中，我们从一个超类(ParentClass)派生了一个子类(ChildClass)。ChildClass 现在可以通过继承访问 ParentClass 的字段和属性。如上所示，我们可以很容易地从 ChildClass 中访问继承的成员。

### C#中的分层继承

当从单个父类创建多个派生类时，就会发生分层继承。

```
namespace LearningInheritance
{
    class ParentClass
    {
        public string name;
        public int Id = 9;

        public void displayParentClassDetails()
        {
            Console.WriteLine($"I am {name}");
            Console.WriteLine($"ID : {Id}");
        }
    }

    class ChildClass : ParentClass
    {
        public void getIdFromParentClass()
        {
            Console.WriteLine("Displaying from my Child Class");
            Console.WriteLine($"This is my ID : {Id}.");
        }
    }

    class AnotherChildClass : ParentClass
    {
        public void getIdFromParentClass()
        {
            Console.WriteLine("Displaying from my other Child Class");
            Console.WriteLine($"This is my ID : {Id}");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            //accessing the inherited members in the parent class (from the child class)
            ChildClass child = new ChildClass();
            child.getIdFromParentClass();

            //accessing the inherited members in the parent class (from the other child class)
            AnotherChildClass anotherChild = new AnotherChildClass();
            anotherChild.getIdFromParentClass();
        }
    }
}
```

在上面的代码中，我们展示了我们可以从一个父类中派生出多个子类。

ChildClass 和 AnotherChildClass 都继承了基类 ParentClass 的字段和方法。这叫层次继承。因此，这两个子类可以访问父类的字段和方法。

### C#中的多级继承

当一个类从另一个派生类派生时，就会发生多级继承。它只是一种情况，一个派生类被创建并用作另一个类的基类。

```
namespace LearningInheritance
{
    class ParentClass
    {
        public string name;
        public int Id = 9;

        //...
    }

    class ChildClass : ParentClass
    {
        //...
        //The Child class is the derived class in this case
    }

    class ThirdClass : ChildClass
    {
        //...
        //The ChildClass is the base class for the ThirdClass
    }

    class Program
    {
        static void Main(string[] args)
        {
            //...
        }
    }
}
```

在上面的代码中，我们还从一个超类(ParentClass)派生了一个子类(ChildClass)。然后，ChildClass 充当名为 ThirdClass 的子子类的基类。

### 多重继承——c#中的接口

C#不支持多重继承。但是你可以通过使用*接口*来实现。

多重继承允许派生类从多个父类继承。您可以看到一个子类如何从多个接口继承的示例，这些接口的行为类似于下面的父类:

```
namespace LearningInheritance
{    
    class Program
    {
        interface InterfaceA
        {
            //...
        }

        interface InterfaceB
        {
            //...
        }     

        class NewClass: InterfaceA, InterfaceB
        {
            //...            
        }

        static void Main(string[] args)
        {
            //...
        }
    }
}
```

## 结论

继承很重要，因为它有助于保持代码的整洁。这使得构建相关类的族变得更加容易。子类可以继承父类中包含的所有字段、属性和方法，但声明为私有类的类除外。

通过这篇文章，我希望你对 C#中的继承有所了解。

快乐编码。