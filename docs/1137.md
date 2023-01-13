# Python 中的面向对象编程

> 原文：<https://www.freecodecamp.org/news/object-oriented-programming-in-python/>

Python 是一种奇妙的编程语言，它允许您使用函数式和面向对象的编程范式。

Python 程序员应该能够使用基本的面向对象编程概念，无论他们是软件开发人员、机器学习工程师还是其他人。

Python 的面向对象编程系统支持通用 OOP 框架的所有四个核心方面:封装、抽象、继承和多态。

在本教程中，我们将快速浏览一下这些特性，并进行一些实践。

# Python 中面向对象的编程概念

## 什么是类和对象？

Python 和其他面向对象的语言一样，允许定义类来创建对象。内置 Python 类是 Python 中最常见的数据类型，比如字符串、列表、字典等等。

类是定义特定对象类型的实例变量和相关方法的集合。你可以把一个类想象成一个对象的蓝图或模板。属性是给组成类的变量起的名字。

具有一组已定义属性的类实例称为对象。因此，同一个类可以根据需要构造任意多的对象。

让我们为一个书商的销售软件定义一个名为 *Book* 的类。

```
class Book:
    def __init__(self, title, quantity, author, price):
        self.title = title
        self.quantity = quantity
        self.author = author
        self.price = price 
```

`__init__`特殊方法，也称为**构造函数**，用于初始化 Book 类的属性，如标题、数量、作者和价格。

在 Python 中，内置类以小写命名，但用户定义的类以 Camel 或 Snake 大小写命名，首字母大写。

这个类可以实例化成任意数量的对象。以下示例代码中实例化了三本书:

```
book1 = Book('Book 1', 12, 'Author 1', 120)
book2 = Book('Book 2', 18, 'Author 2', 220)
book3 = Book('Book 3', 28, 'Author 3', 320)
```

book1、book2 和 book3 是类 *Book* 的不同对象。属性中的术语 **self** 指的是相应的实例(对象)。

```
print(book1)
print(book2)
print(book3)
```

输出:

```
<__main__.Book object at 0x00000156EE59A9D0>
<__main__.Book object at 0x00000156EE59A8B0>
<__main__.Book object at 0x00000156EE59ADF0>
```

打印时会打印对象的类别和内存位置。我们不能指望他们提供质量方面的具体信息，如标题、作者姓名等等。但是我们可以使用一种叫做`__repr__`的特定方法来做到这一点。

在 Python 中，一个特殊的方法是一个定义的函数，以两个下划线开始和结束，当满足某些条件时会自动调用。

```
class Book:
    def __init__(self, title, quantity, author, price):
        self.title = title
        self.quantity = quantity
        self.author = author
        self.price = price

    def __repr__(self):
        return f"Book: {self.title}, Quantity: {self.quantity}, Author: {self.author}, Price: {self.price}"

book1 = Book('Book 1', 12, 'Author 1', 120)
book2 = Book('Book 2', 18, 'Author 2', 220)
book3 = Book('Book 3', 28, 'Author 3', 320)

print(book1)
print(book2)
print(book3) 
```

输出:

```
Book: Book 1, Quantity: 12, Author: Author 1, Price: 120
Book: Book 2, Quantity: 18, Author: Author 2, Price: 220
Book: Book 3, Quantity: 28, Author: Author 3, Price: 320
```

## 什么是封装？

封装是防止客户端访问某些属性的过程，这些属性只能通过特定的方法来访问。

私有属性是不可访问的属性，信息隐藏是使特定属性私有的过程。您使用两个下划线来声明私有特征。

下面介绍一下*书*类中一个叫做 **`__discount`** 的私有属性。

```
class Book:
    def __init__(self, title, quantity, author, price):
        self.title = title
        self.quantity = quantity
        self.author = author
        self.price = price
        self.__discount = 0.10

    def __repr__(self):
        return f"Book: {self.title}, Quantity: {self.quantity}, Author: {self.author}, Price: {self.price}"

book1 = Book('Book 1', 12, 'Author 1', 120)

print(book1.title)
print(book1.quantity)
print(book1.author)
print(book1.price)
print(book1.__discount)
```

输出:

```
Book 1
12
Author 1
120
Traceback (most recent call last):
  File "C:\Users\ashut\Desktop\Test\hello\test.py", line 19, in <module>
    print(book1.__discount)
AttributeError: 'Book' object has no attribute '__discount'
```

我们可以看到，除了私有属性 **`__discount`** 之外，所有的属性都被打印出来了。您使用 getter 和 setter 方法来访问私有属性。

在下面的代码示例中，我们将 price 属性设为私有，并使用 setter 方法来分配 discount 属性，使用 getter 函数来获取 price 属性。

```
class Book:
    def __init__(self, title, quantity, author, price):
        self.title = title
        self.quantity = quantity
        self.author = author
        self.__price = price
        self.__discount = None

    def set_discount(self, discount):
        self.__discount = discount

    def get_price(self):
        if self.__discount:
            return self.__price * (1-self.__discount)
        return self.__price

    def __repr__(self):
        return f"Book: {self.title}, Quantity: {self.quantity}, Author: {self.author}, Price: {self.get_price()}" 
```

这次我们将创建两个对象，一个用于购买单本书，另一个用于购买大量书籍。在批量购买图书时，我们得到 20%的折扣，所以我们将使用 **`set_discount()`** 方法来设置折扣为 20%。

```
single_book = Book('Two States', 1, 'Chetan Bhagat', 200)

bulk_books = Book('Two States', 25, 'Chetan Bhagat', 200)
bulk_books.set_discount(0.20)

print(single_book.get_price())
print(bulk_books.get_price())
print(single_book)
print(bulk_books) 
```

输出:

```
200
160.0
Book: Two States, Quantity: 1, Author: Chetan Bhagat, Price: 200
Book: Two States, Quantity: 25, Author: Chetan Bhagat, Price: 160.0
```

## 什么是继承？

继承被认为是面向对象程序设计的最重要的特征。一个类从另一个类继承方法和/或特征的能力称为继承。

子类或子类是继承的类。超类或父类是从其继承方法和/或属性的类。

我们书商的销售软件增加了两个新的类:小说类和 T2 学术类。

我们可以看到，无论一本书被归类为小说还是学术，它都可能有一些类似标题和作者的属性，以及类似**`get_price()`****`set_discount()`**的常用方法。为每个新类重写所有代码是浪费时间、精力和内存。

```
class Book:
    def __init__(self, title, quantity, author, price):
        self.title = title
        self.quantity = quantity
        self.author = author
        self.__price = price
        self.__discount = None

    def set_discount(self, discount):
        self.__discount = discount

    def get_price(self):
        if self.__discount:
            return self.__price * (1-self.__discount)
        return self.__price

    def __repr__(self):
        return f"Book: {self.title}, Quantity: {self.quantity}, Author: {self.author}, Price: {self.get_price()}"

class Novel(Book):
    def __init__(self, title, quantity, author, price, pages):
        super().__init__(title, quantity, author, price)
        self.pages = pages

class Academic(Book):
    def __init__(self, title, quantity, author, price, branch):
        super().__init__(title, quantity, author, price)
        self.branch = branch 
```

让我们为这些类创建对象来可视化它们。

```
novel1 = Novel('Two States', 20, 'Chetan Bhagat', 200, 187)
novel1.set_discount(0.20)

academic1 = Academic('Python Foundations', 12, 'PSF', 655, 'IT')

print(novel1)
print(academic1)
```

输出:

```
Book: Two States, Quantity: 20, Author: Chetan Bhagat, Price: 160.0
Book: Python Foundations, Quantity: 12, Author: PSF, Price: 655
```

## 什么是多态性？

术语'**多态性**'来自希腊语，意思是'*,有多种形式。*'

多态性指的是子类改编已经存在于其超类中的方法以满足其需求的能力。换句话说，子类可以直接使用超类中的方法，也可以根据需要修改它。

```
class Academic(Book):
    def __init__(self, title, quantity, author, price, branch):
        super().__init__(title, quantity, author, price)
        self.branch = branch

    def __repr__(self):
        return f"Book: {self.title}, Branch: {self.branch}, Quantity: {self.quantity}, Author: {self.author}, Price: {self.get_price()}"
```

*Book* 超类有一个特定的方法叫做`__repr__`。子类 Novel 可以使用这个方法，这样每当打印一个对象时就可以调用它。

另一方面，在上面的示例代码中，*学术类*子类是用它自己的`__repr__`特殊函数定义的。得益于多态性， *Academic* 子类将通过抑制其超类中存在的相同方法来调用自己的方法。

```
novel1 = Novel('Two States', 20, 'Chetan Bhagat', 200, 187)
novel1.set_discount(0.20)

academic1 = Academic('Python Foundations', 12, 'PSF', 655, 'IT')

print(novel1)
print(academic1)
```

输出:

```
Book: Two States, Quantity: 20, Author: Chetan Bhagat, Price: 160.0
Book: Python Foundations, Branch: IT, Quantity: 12, Author: PSF, Price: 655
```

## 什么是抽象？

Python 中不直接支持抽象。另一方面，调用一个神奇的方法允许抽象。

如果抽象方法是在超类中声明的，那么从超类继承的子类必须有自己的方法实现。

超类的抽象方法永远不会被它的子类调用。但是抽象有助于在所有子类中维护相似的结构。

在我们的父类*书*中，我们已经定义了`__repr__`方法。让我们抽象这个方法，强制每个子类都有自己的`__repr__`方法。

```
from abc import ABC, abstractmethod

class Book(ABC):
    def __init__(self, title, quantity, author, price):
        self.title = title
        self.quantity = quantity
        self.author = author
        self.__price = price
        self.__discount = None

    def set_discount(self, discount):
        self.__discount = discount

    def get_price(self):
        if self.__discount:
            return self.__price * (1-self.__discount)
        return self.__price

    @abstractmethod
    def __repr__(self):
        return f"Book: {self.title}, Quantity: {self.quantity}, Author: {self.author}, Price: {self.get_price()}"

class Novel(Book):
    def __init__(self, title, quantity, author, price, pages):
        super().__init__(title, quantity, author, price)
        self.pages = pages

class Academic(Book):
    def __init__(self, title, quantity, author, price, branch):
        super().__init__(title, quantity, author, price)
        self.branch = branch

    def __repr__(self):
        return f"Book: {self.title}, Branch: {self.branch}, Quantity: {self.quantity}, Author: {self.author}, Price: {self.get_price()}"

novel1 = Novel('Two States', 20, 'Chetan Bhagat', 200, 187)
novel1.set_discount(0.20)

academic1 = Academic('Python Foundations', 12, 'PSF', 655, 'IT')

print(novel1)
print(academic1)
```

在上面的代码中，我们继承了 *ABC* 类来创建 *Book* 类。我们通过添加 **`@abstractmethod`** 装饰器使`__repr__`方法变得抽象。

在创建新类时，我们故意忽略了`__repr__`方法的实现，看看会发生什么。

输出:

```
Traceback (most recent call last):
  File "C:\Users\ashut\Desktop\Test\hello\test.py", line 40, in <module>
    novel1 = Novel('Two States', 20, 'Chetan Bhagat', 200, 187)
TypeError: Can't instantiate abstract class Novel with abstract method __repr__ 
```

我们得到一个**类型错误**,说我们不能实例化小说类的对象。让我们添加`__repr__`方法的实现，看看现在会发生什么。

```
class Novel(Book):
    def __init__(self, title, quantity, author, price, pages):
        super().__init__(title, quantity, author, price)
        self.pages = pages

    def __repr__(self):
        return f"Book: {self.title}, Quantity: {self.quantity}, Author: {self.author}, Price: {self.get_price()}"
```

输出:

```
Book: Two States, Quantity: 20, Author: Chetan Bhagat, Price: 160.0
Book: Python Foundations, Branch: IT, Quantity: 12, Author: PSF, Price: 655
```

现在它工作正常。

## 方法重载

几乎所有遵循面向对象编程概念的著名编程语言中都有方法重载的概念。它只是指使用许多同名的方法，这些方法在一个类中接受不同数量的参数。

现在让我们借助下面的代码来理解方法重载:

```
class OverloadingDemo:
    def add(self, x, y):
        print(x+y)

    def add(self, x, y, z):
        print(x+y+z)

obj = OverloadingDemo()
obj.add(2, 3) 
```

输出:

```
Traceback (most recent call last):
  File "C:\Users\ashut\Desktop\Test\hello\setup.py", line 10, in <module>
    obj.add(2, 3)
TypeError: add() missing 1 required positional argument: 'z'
```

你可能想知道为什么会这样。结果，显示错误是因为 Python 只记住了 add(self，x，y，z)的最新定义，它除了 self 之外还采用了三个参数。因此，在调用`add()`方法时，必须向其传递三个参数。换句话说，它忽略了前面对 add()的定义。

因此，Python **默认不支持**方法重载。

## 方法覆盖

当在派生类和基类或超类中都使用了具有相同名称和参数的方法时，我们说派生类方法“覆盖”了基类中提供的方法。

当被重写的方法被调用时，派生类的方法总是被调用。基类中使用的方法现在隐藏了。

```
class ParentClass:
    def call_me(self):
        print("I am parent class")

class ChildClass(ParentClass):
    def call_me(self):
        print("I am child class")

pobj = ParentClass()
pobj.call_me()

cobj = ChildClass()
cobj.call_me()
```

输出:

```
I am parent class
I am child class
```

在上面的代码中，当在子对象上调用 **`call_me()`** 方法时，来自子类的`call_me()`被调用。我们可以使用 **`super()`** 从子类调用父类的`call_me()`方法，就像这样:

```
class ParentClass:
    def call_me(self):
        print("I am parent class")

class ChildClass(ParentClass):
    def call_me(self):
        print("I am child class")
        super().call_me()

pobj = ParentClass()
pobj.call_me()

cobj = ChildClass()
cobj.call_me()
```

输出:

```
I am parent class
I am child class
I am parent class
```

## 包扎

在本文中，我们讨论了类和对象的含义。我们还讨论了面向对象编程的四个支柱。

除此之外，我们还讨论了两个重要的主题——方法重载和方法覆盖。

感谢阅读！

[Subscribe to my newsletter](https://www.getrevue.co/profile/ashutoshkrris target=)