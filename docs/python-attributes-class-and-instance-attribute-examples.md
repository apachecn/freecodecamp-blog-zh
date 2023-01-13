# Python 属性–类和实例属性示例

> 原文：<https://www.freecodecamp.org/news/python-attributes-class-and-instance-attribute-examples/>

当在 Python 中创建一个`class`时，你通常会创建一个类的每个对象共享的属性或者这个类的每个对象独有的属性。

在本文中，我们将通过例子来看看 Python 中类属性和实例属性的区别。

在此之前，让我们看看如何用 Python 创建一个类。

## 如何用 Python 创建一个类

为了在 Python 中创建一个类，我们使用`class`关键字，后跟类名。这里有一个例子:

```
class Student:
    name = "Jane"
    course = "JavaScript"
```

在上面的代码中，我们创建了一个名为`Student`的类，具有`name`和`course`属性。现在让我们从这个类创建新的对象。

```
class Student:
    name = "Jane"
    course = "JavaScript"

Student1 = Student()

print(Student1.name)
# Jane
```

我们从`Student`类中创建了一个名为`Student1`的新对象。

当我们打印`Student1.name`时，我们将“Jane”打印到控制台。回想一下，Jane 的值存储在最初创建的类的变量中。

这个`name`和`course`变量实际上是类属性。我们将在下一节看到更多的例子来帮助你更好地理解。

## Python 中的类和实例属性

为了给出这两个术语的基本定义，**类属性**是由类的每个对象继承的类变量。对于每个新对象，类属性的值保持不变。

正如您将在本节的示例中看到的，类属性是在`__init__()`函数之外定义的。

另一方面，在`__init__()`函数中定义的**实例属性**是类变量，允许我们为类的每个对象定义不同的值。

这里有一个例子:

```
class Student:
    school = "freeCodeCamp.org"

    def __init__(self, name, course):
        self.name = name
        self.course = course

Student1 = Student("Jane", "JavaScript")
Student2 = Student("John", "Python")

print(Student1.name) # Jane
print(Student2.name) # John 
```

在上面的代码中，我们在`Student`类中创建了一个名为`school`的变量。

我们又创建了两个变量，但是是在`__init__()`函数中——`name`和`course`——我们使用`self`参数对它们进行了初始化。

`__init__()`函数中的第一个参数用于在函数中创建变量时初始化其他参数。你想怎么叫都行——按照惯例，`self`用得最多。

`school`变量充当类属性，而`name`和`course`是实例属性。让我们分解上面的例子来解释实例属性。

```
Student1 = Student("Jane", "JavaScript")
Student2 = Student("John", "Python")

print(Student1.name) # Jane
print(Student2.name) # John
```

我们从`Student`类中创建了两个对象——T1 和 T2。默认情况下，这些对象中的每一个都有在类中创建的所有变量。但是每个对象都能够拥有自己的`name`和`course`变量，因为它们是在`__init__()`函数中创建的。

现在让我们打印每个对象的`school`变量，看看会发生什么。

```
print(Student1.school) # freeCodeCamp.org
print(Student2.school) # freeCodeCamp.org
```

两者给了我们相同的值，因为`school`变量是一个类属性。

## 结论

在本文中，我们看到了如何用 Python 创建一个类，以及类和实例属性之间的区别。

总之，**类属性**对每个对象保持不变，并且在`__init__()`函数之外定义。**实例属性**有些动态，因为它们在每个对象中可以有不同的值。

**实例属性**在`__init__()`函数中定义。

编码快乐！