# Ruby 面向对象编程简介

> 原文：<https://www.freecodecamp.org/news/an-introduction-to-object-oriented-programming-with-ruby-73531e2b8ddc/>

尼尚·米什拉

# Ruby 面向对象编程简介

![4NdyCePiNGyAAsSOl3bwYWdN20pXjqCSqpz-](img/6ba0036fe9059636a1bf7b86e74e21d4.png)

作为一名计算机科学学生，我花了很多时间学习和玩新语言。每种新语言都有其独特之处。话虽如此，大多数初学者开始他们的编程之旅时，要么使用像 C 这样的过程语言，要么使用像 JavaScript 和 C++这样的面向对象语言。

因此，通读面向对象编程的基础知识是有意义的，这样您就可以理解这些概念，并轻松地将它们应用到您学习的语言中。我们将以 Ruby 编程语言为例。

你可能会问，为什么是 Ruby？因为它“旨在让程序员开心”,也因为 Ruby 中的几乎所有东西都是对象。

### 了解面向对象的范例(OOP)

在 OOP 中，我们识别程序处理的“事物”。作为人类，我们认为事物是具有属性和行为的对象，我们基于这些属性和行为与事物进行交互。一个东西可以是一辆汽车，一本书，等等。这样的东西变成了类(对象的蓝图)，我们从这些类中创建了对象。

每个实例(对象)包含实例变量，这些变量是对象的状态(属性)。对象行为由方法表示。

让我们以汽车为例。汽车是一种能使其成为**级**的东西。一款特定类型的车，比如说宝马是*级车的**对象**。*宝马的**属性/特性**比如颜色和型号可以存储在实例变量中。如果你想执行一个对象的操作，比如驱动，那么“驱动”描述了一个被定义为**方法**的行为。

### 快速语法课

*   要结束 Ruby 程序中的一行，请使用分号(；)是可选的(但通常不使用)
*   鼓励每个嵌套层次使用两个空格的缩进(不需要，就像 Python 中那样)
*   没有使用花括号`{}`，并且 **end** 关键字用于标记流控制块的结束
*   为了进行注释，我们使用了`#`符号

在 Ruby 中创建对象的方法是调用一个类上的 **new** 方法，如下例所示:

```
class Car  def initialize(name, color)    @name = name    @color = color  end
```

```
 def get_info    "Name: #{@name}, and Color: #{@color}"  endend
```

```
my_car = Car.new("Fiat", "Red")puts my_car.get_info
```

要理解上面代码中发生的事情:

*   我们有一个名为`Car`的类，它有两个方法，`initialize`和`get_info`。
*   Ruby 中的实例变量以`@`开头(例如`@name`)。有趣的是，变量最初并没有声明。它们在第一次被使用时就存在了，之后它们就可用于该类的所有实例方法。
*   调用`new`方法会导致`initialize`方法被调用。`initialize`是作为构造函数使用的一种特殊方法。

#### 访问数据

实例变量是私有的，不能从类外部访问。为了访问它们，我们需要创建方法。默认情况下，实例方法具有公共访问权限。我们可以限制对这些实例方法的访问，我们将在本文后面看到。

为了获取和修改数据，我们分别需要“getter”和“setter”方法。让我们以汽车为例来看看这些方法。

```
class Car  def initialize(name, color) # "Constructor"    @name = name    @color = color  end
```

```
 def color    @color  end
```

```
 def color= (new_color)    @color = new_color  endend
```

```
my_car = Car.new("Fiat", "Red")puts my_car.color # Red
```

```
my_car.color = "White"puts my_car.color # White
```

在 Ruby 中,“getter”和“setter”被定义为与我们正在处理的实例变量同名。

在上面的例子中，当我们说`my_car.color`时，它实际上调用了`color`方法，该方法反过来返回颜色的名称。

*注意:在使用 setter 时，注意 Ruby 如何允许在* `color` *和**之间有一个空格等于符号**，即使方法名是* `color=`

编写这些 getter/setter 方法允许我们有更多的控制。但是大多数时候，获取现有值并设置新值很简单。因此，应该有一种更简单的方法来代替实际定义 getter/setter 方法。

#### 更简单的方法

通过使用`attr_*`表单，我们可以获取现有值并设置一个新值。

*   `attr_accessor`:对于 getter 和 setter 两者
*   `attr_reader`:仅用于吸气剂
*   `attr_writer`:仅用于设定器

让我们以汽车为例来看看这个表格。

```
class Car  attr_accessor :name, :colorend
```

```
car1 = Car.newputs car1.name # => nil
```

```
car1.name = "Suzuki"car1.color = "Gray"puts car1.color # =&gt; Gray
```

```
car1.name = "Fiat"puts car1.name # =&gt; Fiat
```

这样我们可以完全跳过 getter/setter 定义。

#### 谈论最佳实践

在上面的例子中，我们没有初始化`@name`和`@color`实例变量的值，这不是一个好的做法。此外，由于实例变量被设置为零，对象`car1`没有任何意义。如下例所示，使用构造函数设置实例变量始终是一个好习惯。

```
class Car  attr_accessor :name, :color    def initialize(name, color)    @name = name    @color = color  endend
```

```
car1 = Car.new("Suzuki", "Gray")puts car1.color # =&gt; Gray
```

```
car1.name = "Fiat"puts car1.name # =&gt; Fiat
```

### 类方法和类变量

所以类方法是在类上调用的，而不是在类的实例上。这些类似于 Java 中的**静态**方法。

*注意:`self`方法定义之外的对象指的是类对象。类变量以`@@`开始*

现在，在 Ruby 中实际上有三种方法来定义类方法:

#### 在类定义内部

1.  在方法名称中使用关键字 self:

```
class MathFunctions  def self.two_times(num)    num * 2  endend
```

```
# No instance createdputs MathFunctions.two_times(10) # =&gt; 20
```

2.使用`<<`；自己

```
class MathFunctions  class << self    def two_times(num)      num * 2    end  endend
```

```
# No instance createdputs MathFunctions.two_times(10) # =&gt; 20
```

#### 在类定义之外

3.将类名与方法名一起使用

```
class MathFunctionsend
```

```
def MathFunctions.two_times(num)  num * 2end
```

```
# No instance createdputs MathFunctions.two_times(10) # =&gt; 20
```

### 类继承

在 Ruby 中，每个类都隐式地继承自 Object 类。让我们看一个例子。

```
class Car  def to_s    "Car"  end
```

```
 def speed    "Top speed 100"  endend
```

```
class SuperCar < Car  def speed # Override    "Top speed 200"  endend
```

```
car = Car.newfast_car = SuperCar.new
```

```
puts "#{car}1 #{car.speed}" # =&gt; Car1 Top speed 100puts "#{fast_car}2 #{fast_car.speed}" # => Car2 Top speed 200
```

在上面的例子中，`SuperCar`类覆盖了从`Car`类继承的`speed`方法。符号`&`lt；表示继承。

注意:Ruby 不支持多重继承，所以使用 mix-in 来代替。我们将在本文后面讨论它们。

### Ruby 中的模块

Ruby 模块是 Ruby 编程语言的重要组成部分。这是该语言主要的面向对象特性，并间接支持多重继承。

模块是类、方法、常量甚至其他模块的容器。像类一样，模块不能被实例化，但有两个主要用途:

*   命名空间
*   混合

#### 作为命名空间的模块

很多像 Java 这样的语言都有包结构的概念，只是为了避免两个类之间的冲突。让我们看一个例子来理解它是如何工作的。

```
module Patterns  class Match    attr_accessor :matched  endend
```

```
module Sports  class Match    attr_accessor :score  endend
```

```
match1 = Patterns::Match.newmatch1.matched = "true"
```

```
match2 = Sports::Match.newmatch2.score = 210
```

在上面的例子中，由于我们有两个名为`Match`的类，我们可以区分它们，并通过简单地将它们封装到不同的模块中来防止冲突。

#### 作为混合模块

在面向对象的范例中，我们有接口的概念。Mix-in 提供了一种在多个类之间共享代码的方法。不仅如此，我们还可以包含像`Enumerable`这样的内置模块，使我们的任务变得更加容易。让我们看一个例子。

```
module PrintName  attr_accessor :name  def print_it    puts "Name: #{@name}"  endend
```

```
class Person  include PrintNameend
```

```
class Organization  include PrintNameend
```

```
person = Person.newperson.name = "Nishant"puts person.print_it # =&gt; Name: Nishant
```

```
organization = Organization.neworganization.name = "freeCodeCamp"puts organization.print_it # =&gt; Name: freeCodeCamp 
```

mix-in 非常强大，因为我们只编写一次代码，然后可以根据需要在任何地方包含它们。

### Ruby 中的范围

我们将了解范围如何适用于:

*   变量
*   常数
*   阻碍

#### 变量的范围

方法和类为变量定义了一个新的范围，外部范围的变量不会被带入内部范围。让我们看看这意味着什么。

```
name = "Nishant"
```

```
class MyClass  def my_fun    name = "John"    puts name # =&gt; John  end
```

```
puts name # =&gt; Nishant
```

外部`name`变量和内部`name`变量不同。外部的`name`变量不会被带入内部范围。这意味着如果你试图在内部作用域中打印它，而没有再次定义它，将会抛出一个异常— **没有这样的变量存在**

#### 常数的范围

内部作用域可以看到外部作用域中定义的常数，也可以重写外部常数。但是重要的是要记住，即使在内部作用域中覆盖了常量值之后，外部作用域中的值仍然保持不变。让我们看看它的实际效果。

```
module MyModule  PI = 3.14  class MyClass    def value_of_pi      puts PI # =&gt; 3.14      PI = "3.144444"      puts PI # => 3.144444    end  end  puts PI # =&gt; 3.14end
```

#### 区块范围

块继承外部范围。让我们用我在网上找到的一个奇妙的例子来理解它。

```
class BankAccount  attr_accessor :id, :amount  def initialize(id, amount)    @id = id    @amount = amount  endend
```

```
acct1 = BankAccount.new(213, 300)acct2 = BankAccount.new(22, 100)acct3 = BankAccount.new(222, 500)
```

```
accts = [acct1, acct2, acct3]
```

```
total_sum = 0accts.each do |eachAcct|  total_sum = total_sum + eachAcct.amountend
```

```
puts total_sum # =&gt; 900
```

在上面的例子中，如果我们使用一个方法来计算`total_sum`，那么`total_sum`变量将会是方法中一个完全不同的变量。这就是为什么有时使用块可以节省我们很多时间。

话虽如此，在块内部创建的变量只对块可用。

### 访问控制

当设计一个类时，重要的是要考虑你将向外界展示多少。这被称为**封装，**，通常意味着隐藏对象的内部表示。

Ruby 中有三个级别的访问控制:

*   **Public** -不实施访问控制。任何人都可以调用这些方法。
*   **受保护的** -可以被定义类或其子类的对象调用。
*   **Private** -除非有明确的接收者，否则不能调用。

让我们看一个实际封装的例子:

```
class Car  def initialize(speed, fuel_eco)    @rating = speed * comfort  end
```

```
 def rating    @rating  endend
```

```
puts Car.new(100, 5).rating # =&gt; 500
```

现在，由于评级如何计算的细节保存在类中，我们可以在任何时间点更改它，而无需任何其他更改。此外，我们不能从外部设置评级。

谈到指定访问控制的方法，有两种:

1.  指定 public、protected 或 private 等，直到下一个访问控制关键字将具有该访问控制级别。
2.  定期定义方法，然后指定公共、私有和受保护的访问级别，并在这些级别下使用方法符号列出逗号(，)分隔的方法。

#### 第一种方式的例子:

```
class MyClass  private    def func1      "private"    end  protected    def func2      "protected"    end  public    def func3      "Public"    endend
```

#### 第二种方式的例子:

```
class MyClass  def func1    "private"  end  def func2    "protected"  end  def func3    "Public"  end  private :func1  protected :func2  public :func3end
```

*注意:公共和私有访问控制是最常用的。*

### 结论

这些是 Ruby 中面向对象编程的基础。现在，知道了这些概念，你可以更深入，通过构建酷的东西来学习它们。

如果你喜欢，不要忘了鼓掌和跟随！跟上我[这里](https://www.instagram.com/nishantmishra02/)。