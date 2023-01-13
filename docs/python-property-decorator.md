# Python 中的@property 装饰器:它的用例、优点和语法

> 原文：<https://www.freecodecamp.org/news/python-property-decorator/>

## 🔹会议属性

欢迎光临！在本文中，您将学习如何使用 Python 中的`@property`装饰器。

**您将了解到:**

*   在 Python 中使用属性的优势。
*   装饰函数的基础:它们是什么以及它们与@property 的关系。
*   如何使用@property 定义 getters、setters 和 deleters。

### Python 中属性的 1️⃣优势

让我们从一点点背景开始。**为什么**你会在 Python 中使用属性？

属性可以被认为是处理属性的“Pythonic 式”方法，因为:

*   用于定义属性的语法非常简洁易读。
*   您可以像访问公共属性一样访问实例属性，同时使用中介(getters 和 setters)的“魔力”来验证新值并避免直接访问或修改数据。
*   通过使用@property，您可以“重用”属性的名称，以避免为 getters、setters 和 deleters 创建新名称。

这些优点使属性成为一个非常棒的工具，可以帮助你编写更简洁易读的代码。？

### 2️⃣介绍**装修工**

一个**装饰函数**基本上是一个向作为参数传递的函数添加新功能的函数。使用装饰函数就像在冰淇淋中加入巧克力粉一样？。它允许我们在不修改现有功能的情况下向其添加新功能。

在下面的示例中，您可以看到 Python 中典型的装饰函数是什么样子的:

```
def decorator(f):
    def new_function():
        print("Extra Functionality")
        f()
    return new_function

@decorator
def initial_function():
    print("Initial Functionality")

initial_function()
```

**我们来详细分析一下这些元素:**

*   我们首先找到装饰函数`def decorator(f)`(洒落的✨),它将函数`f`作为参数。

```
def decorator(f):
    def new_function():
        print("Extra Functionality")
        f()
    return new_function
```

*   这个装饰函数有一个嵌套函数`new_function`。注意`f`是如何在`new_function`中被调用的，以便在函数调用之前添加新功能的同时实现相同的功能(我们也可以在函数调用之后添加新功能)。
*   装饰函数本身返回嵌套函数`new_function`。
*   然后(下图)，我们找到将被*装饰的*(冰淇淋？)`initial_function`。注意函数头上方非常特殊的语法(`@decorator`)。

```
@decorator
def initial_function():
    print("Initial Functionality")

initial_function()
```

如果我们运行代码，我们会看到以下输出:

```
Extra Functionality
Initial Functionality
```

注意装饰函数是如何运行的，即使我们只调用了`initial_function()`。这就是加@decorator 的魔力？。

**💡注意:**一般来说，我们会写`@<decorator_function_name>`，替换@符号后的装饰函数名。

我知道你可能会问:这与@property 有什么关系？@ property 是 Python 中 [property()](https://docs.python.org/3/library/functions.html#property) 函数的内置装饰器。当我们在一个类中定义属性时，它用来给某些方法提供“特殊”的功能，使它们充当 getters、setters 或 deleters。

现在您已经熟悉了 decorators，让我们来看一个使用@property 的真实场景！

## 🔸真实场景:@property

假设这门课是你项目的一部分。您正在用一个`House`类建模一所房子(目前，该类只定义了一个*价格*实例属性):

```
class House:

	def __init__(self, price):
		self.price = price
```

此实例属性是公共的，因为它的名称没有前导下划线。由于该属性目前是公共的，很有可能您和您团队中的其他开发人员在程序的其他部分使用点符号直接访问和修改了属性**和**，如下所示:

```
# Access value
obj.price

# Modify value
obj.price = 40000
```

💡**提示:** *obj* 表示引用`House`实例的变量。

到目前为止一切都很好，对吗？**但是** **假设您被要求将这个属性设为受保护的(非公共的),并在给它赋值之前验证新值**。具体来说，您需要检查该值是否为正浮点数。你会怎么做？让我们看看。

### 更改您的代码

此时，如果您决定添加 getters 和 setters，您和您的团队可能会惊慌失措？。这是因为访问或修改属性值的每一行代码都必须被修改以分别调用 getter 或 setter。否则，代码将打破⚠️.

```
# Changed from obj.price
obj.get_price()

# Changed from obj.price = 40000
obj.set_price(40000)
```

**但是...物业来救援！**使用`@property`，您和您的团队将不需要修改其中的任何一行，因为您将能够在“幕后”添加 getters 和 setters，而不会影响您用来访问或修改公共属性的语法。

太棒了，对吧？

## 🔹@property:语法和逻辑

如果您决定使用`@property`，您的类将看起来像下面的例子:

```
class House:

	def __init__(self, price):
		self._price = price

	@property
	def price(self):
		return self._price

	@price.setter
	def price(self, new_price):
		if new_price > 0 and isinstance(new_price, float):
			self._price = new_price
		else:
			print("Please enter a valid price")

	@price.deleter
	def price(self):
		del self._price
```

具体来说，你可以为一个属性定义三个方法:

*   一个**getter**——用来访问属性的值。
*   一个**设置器** -用来设置属性的值。
*   一个**删除器** -删除实例属性。

**价格现在是“受保护的”**
请注意，*价格*属性现在被认为是“受保护的”，因为我们在`self._price`中为其名称添加了前导下划线:

```
self._price = price
```

在 Python 中，[按照惯例](https://www.python.org/dev/peps/pep-0008/#method-names-and-instance-variables)，当你给名字添加一个前导下划线时，你是在告诉其他开发人员不应该在类之外直接访问或修改它。如果中介体(getters 和 setters)可用的话，应该只通过它们来访问它。

### 🔸吸气剂

这里我们有 getter 方法:

```
@property
def price(self):
	return self._price
```

注意语法:

*   `@property` -用来表示我们将要定义一个属性。请注意这是如何立即提高可读性的，因为我们可以清楚地看到这个方法的目的。
*   `def price(self)` -表头。请注意，getter 的命名与我们定义的属性完全一样: *price* 。这是我们将用来在类外部访问和修改属性的名称。该方法只接受一个形式参数 self，它是对实例的引用。
*   这一行正是你在常规 getter 中所期望的。返回受保护属性的值。

下面是使用 getter 方法的一个示例:

```
>>> house = House(50000.0) # Create instance
>>> house.price            # Access value
50000.0
```

注意我们是如何访问 *price* 属性的，就好像它是一个公共属性一样。我们根本没有改变语法，但是我们实际上使用 getter 作为中介来避免直接访问数据。

### 🔹作曲者

现在我们有了 setter 方法:

```
@price.setter
def price(self, new_price):
	if new_price > 0 and isinstance(new_price, float):
		self._price = new_price
	else:
		print("Please enter a valid price")
```

注意语法:

*   `@price.setter` -用于表示这是*价格*属性的*设置者*方法。注意，我们使用的是 *@ **属性**，而不是**。setter*** ，我们用的是 *@ **价格**。设定器*。属性名称包含在*之前。设定器*。
*   `def price(self, new_price):` -表头和参数列表。请注意属性的名称是如何用作 setter 的名称的。我们还有第二个形参( *new_price* )，这是分配给 *price* 属性的新值(如果有效)。
*   最后，我们有 setter 的主体，在这里我们**验证**参数以检查它是否是正浮点数，然后，如果参数有效，我们更新属性的值。如果该值无效，将打印一条描述性消息。您可以根据程序的需要选择如何处理无效值。

这是一个使用 setter 方法和@property 的示例:

```
>>> house = House(50000.0)  # Create instance
>>> house.price = 45000.0   # Update value
>>> house.price             # Access value
45000.0
```

请注意，我们并没有改变语法，但是现在我们使用了一个中介(setter)在赋值之前验证参数。新值(45000.0)作为参数传递给 setter:

```
house.price = 45000.0
```

如果我们试图分配一个无效值，我们会看到描述性消息。我们还可以检查该值是否没有更新:

```
>>> house = House(50000.0)
>>> house.price = -50
Please enter a valid price
>>> house.price
50000.0
```

💡**提示:**这证明 setter 方法是作为中介工作的。当我们尝试更新该值时，它在“幕后”被调用，因此当该值无效时，会显示描述性消息。

### 🔸删除器

最后，我们有 deleter 方法:

```
@price.deleter
def price(self):
	del self._price
```

注意语法:

*   `@price.deleter` -用于表示这是*价格*属性的*删除者*方法。注意，这一行非常类似于@price.setter，但是现在我们正在定义 deleter 方法，所以我们编写@price。**删除器**。
*   `def price(self):` -表头。这个方法只定义了一个形参 self。
*   `del self._price` -主体，在这里我们删除实例属性。

💡**提示:**注意，属性的名称对于所有三种方法都是“重用”的。

这是一个使用带有@property 的 deleter 方法的示例:

```
# Create instance
>>> house = House(50000.0)

# The instance attribute exists
>>> house.price
50000.0

# Delete the instance attribute
>>> del house.price

# The instance attribute doesn't exist
>>> house.price
Traceback (most recent call last):
  File "<pyshell#35>", line 1, in <module>
    house.price
  File "<pyshell#20>", line 8, in price
    return self._price
AttributeError: 'House' object has no attribute '_price'
```

实例属性删除成功？。当我们再次尝试访问它时，会抛出一个错误，因为该属性不再存在。

### 🔹一些最后的提示

您不必为每个属性定义所有三种方法。您可以通过仅包含一个 getter 方法来定义只读属性。您也可以选择定义不带删除器的 getter 和 setter。

如果您认为属性只应该在创建实例时设置，或者只应该在类内部修改，那么您可以省略 setter。

您可以根据所使用的上下文来选择要包含的方法。

## 🔸概括起来

*   您可以使用@property 语法来定义属性，这种语法更加简洁，可读性更好。
*   @property 可以被认为是定义 getters、setters 和 deleters 的“pythonic 式”方法。
*   通过定义属性，可以在不影响程序的情况下更改类的内部实现，因此可以添加 getters、setters 和 deleters 作为“幕后”中介，以避免直接访问或修改数据。

我真的希望你喜欢我的文章，并觉得它很有帮助。要了解 Python 中的属性和面向对象编程的更多信息，[请查看我的在线课程](https://www.udemy.com/course/python-object-oriented-programming-oop/?referralCode=69EAFFB4805866B8CC31)，其中包括 6 个多小时的视频讲座、编码练习和迷你项目。